---
translation_status: machine_translated
source: Tutorials/Advanced/Simulators/Webots/Setting-Up-Simulation-Webots-Advanced.rst
---

<span id="setting-up-a-robot-simulation-advanced"></span>

# 配置机器人仿真（高级）

**目标：** 以障碍避免节点扩展机器人模拟.

**教程级别：** 高级

**用时：** 20分钟

<span id="background"></span>

## 背景

在此教程中, 您将扩展教程前半部分创建的软件包 : [配置机器人仿真（基础）](Setting-Up-Simulation-Webots-Basic.md). 目的是实施一个ROS 2节点,避免使用机器人的距离传感器设置障碍。 `webots_ros2_driver` 接口。

<span id="prerequisites"></span>

## 前提条件

这是教程第一部分的继续: [配置机器人仿真（基础）](Setting-Up-Simulation-Webots-Basic.md)。必须从第一部分开始设置自定义软件包和必要的文件。

此教程与版本 2023.1.0 兼容 `webots_ros2` 和Webots R2023b,以及即将发行的版本.

<span id="tasks"></span>

## 操作步骤

<span id="updating-my-robot-urdf"></span>

### 1 更新 `my_robot.urdf`

如前所述, [配置机器人仿真（基础）](Setting-Up-Simulation-Webots-Basic.md), `webots_ros2_driver` 包含插件, 可以直接用 ROS 2 接口大多数 Webots 设备。 这些插件可以使用 `<device>` 标记在机器人的URDF文件中。 `reference` 属性应匹配 Webots 设备 `name` 参数。可以找到所有现有接口和相应的参数列表 [设备参考页面](https://github.com/cyberbotics/webots_ros2/wiki/References-Devices)。对于URDF文件中未配置的可用设备,接口将自动创建,默认值将用于ROS参数(例如. `update rate`, `topic name`,以及 `frame name`).

内 `my_robot.urdf` 将全部内容改为:

##### Python

``` xml
<?xml version="1.0" ?>
<robot name="My robot">
    <webots>
        <device reference="ds0" type="DistanceSensor">
            <ros>
                <topicName>/left_sensor</topicName>
                <alwaysOn>true</alwaysOn>
            </ros>
        </device>
        <device reference="ds1" type="DistanceSensor">
            <ros>
                <topicName>/right_sensor</topicName>
                <alwaysOn>true</alwaysOn>
            </ros>
        </device>
        <plugin type="my_package.my_robot_driver.MyRobotDriver" />
    </webots>
</robot>
```

##### C++

``` xml
<?xml version="1.0" ?>
<robot name="My robot">
    <webots>
        <device reference="ds0" type="DistanceSensor">
            <ros>
                <topicName>/left_sensor</topicName>
                <alwaysOn>true</alwaysOn>
            </ros>
        </device>
        <device reference="ds1" type="DistanceSensor">
            <ros>
                <topicName>/right_sensor</topicName>
                <alwaysOn>true</alwaysOn>
            </ros>
        </device>
        <plugin type="my_robot_driver::MyRobotDriver" />
    </webots>
</robot>
```

除了您的自定义插件之外, `webots_ros2_driver` 将解析 `<device>` 提及此标签的标签 **远程传感器** 节点并使用 `<ros>` 标记以启用传感器并命名其主题。

<span id="creating-a-ros-node-to-avoid-obstacles"></span>

### 2 建立一个ROS节点以避免障碍

##### Python

机器人将使用一个标准的ROS节点来探测墙壁,并发送运动命令来避开它。 `my_package/my_package/` 文件夹,创建名为 `obstacle_avoider.py` 使用此代码 :

``` python
import rclpy
from rclpy.node import Node
from sensor_msgs.msg import Range
from geometry_msgs.msg import Twist


MAX_RANGE = 0.15


class ObstacleAvoider(Node):
    def __init__(self):
        super().__init__('obstacle_avoider')

        self.__publisher = self.create_publisher(Twist, 'cmd_vel', 1)

        self.create_subscription(Range, 'left_sensor', self.__left_sensor_callback, 1)
        self.create_subscription(Range, 'right_sensor', self.__right_sensor_callback, 1)

    def __left_sensor_callback(self, message):
        self.__left_sensor_value = message.range

    def __right_sensor_callback(self, message):
        self.__right_sensor_value = message.range

        command_message = Twist()

        command_message.linear.x = 0.1

        if self.__left_sensor_value < 0.9 * MAX_RANGE or self.__right_sensor_value < 0.9 * MAX_RANGE:
            command_message.angular.z = -2.0

        self.__publisher.publish(command_message)


def main(args=None):
    rclpy.init(args=args)
    avoider = ObstacleAvoider()
    rclpy.spin(avoider)
    # Destroy the node explicitly
    # (optional - otherwise it will be done automatically
    # when the garbage collector destroys the node object)
    avoider.destroy_node()
    rclpy.shutdown()


if __name__ == '__main__':
    main()
```

这个节点会为命令创建一个发布器,并订阅这里的传感器主题:

``` python
self.__publisher = self.create_publisher(Twist, 'cmd_vel', 1)

self.create_subscription(Range, 'left_sensor', self.__left_sensor_callback, 1)
self.create_subscription(Range, 'right_sensor', self.__right_sensor_callback, 1)
```

当从左侧传感器收到测量数据时,它将被复制到一个成员字段:

``` python
def __left_sensor_callback(self, message):
    self.__left_sensor_value = message.range
```

最后,将发文给 `/cmd_vel` 当收到来自右传感器的测量值时的主题。 `command_message` 将登记至少一个前进速度 `linear.x` 如果两个传感器中的任何一个探测到一个障碍, `command_message` 也会在 `angular.z` 为了让机器人向右转

``` python
def __right_sensor_callback(self, message):
    self.__right_sensor_value = message.range

    command_message = Twist()

    command_message.linear.x = 0.1

    if self.__left_sensor_value < 0.9 * MAX_RANGE or self.__right_sensor_value < 0.9 * MAX_RANGE:
        command_message.angular.z = -2.0

    self.__publisher.publish(command_message)
```

##### C++

机器人将使用一个标准的ROS节点来探测墙壁,并发送运动命令来避开它。 `my_package/include/my_package` 文件夹,创建标题文件 `ObstacleAvoider.hpp` 使用此代码 :

``` cpp
#include <memory>

#include "geometry_msgs/msg/twist.hpp"
#include "rclcpp/rclcpp.hpp"
#include "sensor_msgs/msg/range.hpp"

class ObstacleAvoider : public rclcpp::Node {
public:
  explicit ObstacleAvoider();

private:
  void leftSensorCallback(const sensor_msgs::msg::Range::ConstSharedPtr msg);
  void rightSensorCallback(const sensor_msgs::msg::Range::ConstSharedPtr msg);

  rclcpp::Publisher<geometry_msgs::msg::Twist>::SharedPtr publisher_;
  rclcpp::Subscription<sensor_msgs::msg::Range>::SharedPtr left_sensor_sub_;
  rclcpp::Subscription<sensor_msgs::msg::Range>::SharedPtr right_sensor_sub_;

  double left_sensor_value{0.0};
  double right_sensor_value{0.0};
};
```

在那个 `my_package/src` 文件夹,创建名为源文件 `ObstacleAvoider.cpp` 使用此代码 :

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

``` cpp
#include "my_package/ObstacleAvoider.hpp"

#define MAX_RANGE 0.15

ObstacleAvoider::ObstacleAvoider() : Node("obstacle_avoider") {
  publisher_ = create_publisher<geometry_msgs::msg::Twist>("/cmd_vel", 1);

  left_sensor_sub_ = create_subscription<sensor_msgs::msg::Range>(
      "/left_sensor", 1,
      std::bind(&ObstacleAvoider::leftSensorCallback, this,
                std::placeholders::_1));

  right_sensor_sub_ = create_subscription<sensor_msgs::msg::Range>(
      "/right_sensor", 1,
      std::bind(&ObstacleAvoider::rightSensorCallback, this,
                std::placeholders::_1));
}

void ObstacleAvoider::leftSensorCallback(
    const sensor_msgs::msg::Range::ConstSharedPtr msg) {
  left_sensor_value = msg->range;
}

void ObstacleAvoider::rightSensorCallback(
    const sensor_msgs::msg::Range::ConstSharedPtr msg) {
  right_sensor_value = msg->range;

  auto command_message = std::make_unique<geometry_msgs::msg::Twist>();

  command_message->linear.x = 0.1;

  if (left_sensor_value < 0.9 * MAX_RANGE ||
      right_sensor_value < 0.9 * MAX_RANGE) {
    command_message->angular.z = -2.0;
  }

  publisher_->publish(std::move(command_message));
}

int main(int argc, char *argv[]) {
  rclcpp::init(argc, argv);
  auto avoider = std::make_shared<ObstacleAvoider>();
  rclcpp::spin(avoider);
  rclcpp::shutdown();
  return 0;
}
```

这个节点会为命令创建一个发布器,并订阅这里的传感器主题:

``` cpp
  publisher_ = create_publisher<geometry_msgs::msg::Twist>("/cmd_vel", 1);

  left_sensor_sub_ = create_subscription<sensor_msgs::msg::Range>(
      "/left_sensor", 1,
      std::bind(&ObstacleAvoider::leftSensorCallback, this,
                std::placeholders::_1));

  right_sensor_sub_ = create_subscription<sensor_msgs::msg::Range>(
      "/right_sensor", 1,
      std::bind(&ObstacleAvoider::rightSensorCallback, this,
                std::placeholders::_1));
```

当从左侧传感器收到测量数据时,它将被复制到一个成员字段:

``` cpp
void ObstacleAvoider::leftSensorCallback(
    const sensor_msgs::msg::Range::ConstSharedPtr msg) {
  left_sensor_value = msg->range;
}
```

最后,将发文给 `/cmd_vel` 当收到来自右传感器的测量值时的主题。 `command_message` 将登记至少一个前进速度 `linear.x` 如果两个传感器中的任何一个探测到一个障碍, `command_message` 也会在 `angular.z` 为了让机器人向右转

``` cpp
void ObstacleAvoider::rightSensorCallback(
    const sensor_msgs::msg::Range::ConstSharedPtr msg) {
  right_sensor_value = msg->range;

  auto command_message = std::make_unique<geometry_msgs::msg::Twist>();

  command_message->linear.x = 0.1;

  if (left_sensor_value < 0.9 * MAX_RANGE ||
      right_sensor_value < 0.9 * MAX_RANGE) {
    command_message->angular.z = -2.0;
  }

  publisher_->publish(std::move(command_message));
}
```

<span id="updating-additional-files"></span>

### 3 更新额外文件

您必须修改另外两个文件以启动您的新节点 。

##### Python

编辑 `setup.py` 替换 `'console_scripts'` 改为:

``` python
'console_scripts': [
    'my_robot_driver = my_package.my_robot_driver:main',
    'obstacle_avoider = my_package.obstacle_avoider:main'
],
```

这将增加一个切入点。 `obstacle_avoider` 节点。

##### C++

编辑 `CMakeLists.txt` 并添加汇编和安装 `obstacle_avoider`:

``` cmake
cmake_minimum_required(VERSION 3.5)
project(my_package)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

# Besides the package specific dependencies we also need the `pluginlib` and `webots_ros2_driver`
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(pluginlib REQUIRED)
find_package(webots_ros2_driver REQUIRED)

# Export the plugin configuration file
pluginlib_export_plugin_description_file(webots_ros2_driver my_robot_driver.xml)

# Obstacle avoider
include_directories(
  include
)
add_executable(obstacle_avoider
  src/ObstacleAvoider.cpp
)
ament_target_dependencies(obstacle_avoider
  rclcpp
  geometry_msgs
  sensor_msgs
)
install(TARGETS
  obstacle_avoider
  DESTINATION lib/${PROJECT_NAME}
)
install(
  DIRECTORY include/
  DESTINATION include
)

# MyRobotDriver library
add_library(
  ${PROJECT_NAME}
  SHARED
  src/MyRobotDriver.cpp
)
target_include_directories(
  ${PROJECT_NAME}
  PRIVATE
  include
)
ament_target_dependencies(
  ${PROJECT_NAME}
  pluginlib
  rclcpp
  webots_ros2_driver
)
install(TARGETS
  ${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)
# Install additional directories.
install(DIRECTORY
  launch
  resource
  worlds
  DESTINATION share/${PROJECT_NAME}/
)

ament_export_include_directories(
  include
)
ament_export_libraries(
  ${PROJECT_NAME}
)
ament_package()
```

转到文件 `robot_launch.py` 改为:

``` python
import os
import launch
from launch_ros.actions import Node
from launch import LaunchDescription
from ament_index_python.packages import get_package_share_directory
from webots_ros2_driver.webots_launcher import WebotsLauncher
from webots_ros2_driver.webots_controller import WebotsController


def generate_launch_description():
    package_dir = get_package_share_directory('my_package')
    robot_description_path = os.path.join(package_dir, 'resource', 'my_robot.urdf')

    webots = WebotsLauncher(
        world=os.path.join(package_dir, 'worlds', 'my_world.wbt')
    )

    my_robot_driver = WebotsController(
        robot_name='my_robot',
        parameters=[
            {'robot_description': robot_description_path},
        ]
    )

    obstacle_avoider = Node(
        package='my_package',
        executable='obstacle_avoider',
    )

    return LaunchDescription([
        webots,
        my_robot_driver,
        obstacle_avoider,
        launch.actions.RegisterEventHandler(
            event_handler=launch.event_handlers.OnProcessExit(
                target_action=webots,
                on_exit=[launch.actions.EmitEvent(event=launch.events.Shutdown())],
            )
        )
    ])
```

这将创建一个 `obstacle_avoider` 将包含在 `LaunchDescription`.

<span id="test-the-obstacle-avoidance-code"></span>

### 4 测试障碍避免代码

从ROS 2工作空间的终端启动模拟:

##### Linux

从ROS 2工作空间运行的终端:

``` console
$ colcon build
$ source install/local_setup.bash
$ ros2 launch my_package robot_launch.py
```

##### Windows

从您 WSL ROS 2 工作空间运行的终端:

``` console
$ colcon build
$ export WEBOTS_HOME=/mnt/c/Program\ Files/Webots
$ source install/local_setup.bash
$ ros2 launch my_package robot_launch.py
```

一定要用那个 `/mnt` 前缀在Webots安装文件夹路径前,以便从WSL访问Windows文件系统。

##### macOS

在主机的终端(不在VM)中,如果还没有完成,则指定Webots安装文件夹(例如. `/Applications/Webots.app`)并使用下列命令启动服务器:

``` console
$ export WEBOTS_HOME=/Applications/Webots.app
$ python3 local_simulation_server.py
```

请注意, 一旦ROS 2 节点结束, 服务器会继续运行。 您不需要每次想要启动新的模拟时都会重新启动它。 从 ROS 2 工作空间的 Linux VM 终端中, 用 :

``` console
$ cd ~/ros2_ws
$ colcon build
$ source install/local_setup.bash
$ ros2 launch my_package robot_launch.py
```

你的机器人应该向前走,在撞墙之前,它应该顺时针转动。你可以按 `Ctrl+F10` 在 Webots 中或进入 `View` 菜单, `Optional Rendering` 财务报告和财务报告 `Show DistanceSensor Rays` 以显示机器人的距离传感器范围。

![](Image/Robot_turning_clockwise.png) <span id="summary"></span>

## 小结

在这个教程中,你用一个障碍避让ROS 2节点来扩展基本模拟,这个节点根据机器人的距离传感器值发布速度命令.

<span id="next-steps"></span>

## 后续步骤

您可能想要改进插件或创建新的节点来改变机器人的行为。 您也可以执行一个重置处理器, 在模拟从 Webots 界面重置时自动重新启动您的 ROS 节点 :

- [配置重置处理器](Simulation-Reset-Handler.md).
