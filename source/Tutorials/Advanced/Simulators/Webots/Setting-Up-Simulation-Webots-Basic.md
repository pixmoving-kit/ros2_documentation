---
translation_status: machine_translated
source: Tutorials/Advanced/Simulators/Webots/Setting-Up-Simulation-Webots-Basic.rst
---

<span id="setting-up-a-robot-simulation-basic"></span>

# 配置机器人仿真（基础）

**目标：** 设置机器人模拟,从ROS 2控制.

**教程级别：** 高级

**用时：** 30分钟

<span id="background"></span>

## 背景

在这个教程中,你会使用 Webots 机器人模拟器来设置和运行一个非常简单的ROS 2模拟方案.

那个... `webots_ros2` 软件包提供了 ROS 2 和 Webots 之间的接口。它包括多个子软件包,但在此教程中,您将只使用 `webots_ros2_driver` 子包用于执行 Python 或 C++ 插件, 控制模拟机器人。 其他一些子包中包含有不同机器人的演示, 如TurtleBot3 。 这些演示文件在文档中记录 。 [Webots ROS 2 实例](https://github.com/cyberbotics/webots_ros2/wiki/Examples) 页面。

<span id="prerequisites"></span>

## 前提条件

建议理解初学者所包括的基本ROS原则。 [教程](../../../../Tutorials.md)特别是, [使用乌龟、罗素、罗素和rqt](../../../Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md), [理解话题](../../../Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md), [创建工作空间](../../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md), [创建软件包](../../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md) 财务报告和财务报告 [创建启动文件](../../../Intermediate/Launch/Creating-Launch-Files.md) 是有用的先决条件。

##### Linux

此教程的 Linux 和 ROS 命令可以在标准的 Linux 终端中运行。 以下页面 [安装（Ubuntu）](Installation-Ubuntu.md) 解释如何安装 `webots_ros2` Linux 上的软件包。

##### Windows

此教程的 Linux 和 ROS 命令必须在 WSL (Linux 的 Windows 子系统) 环境中运行。 下面的页面 [安装（Windows）](Installation-Windows.md) 解释如何安装 `webots_ros2` 在Windows上的软件包。

##### macOS

此教程的 Linux 和 ROS 命令必须在预配置的 Linux 虚拟机( VM) 中运行。 以下页面 [安装（macOS）](Installation-MacOS.md) 解释如何安装 `webots_ros2` (原始内容存档于2019-09-31). package on macOS.

此教程与版本 2023.1.0 兼容 `webots_ros2` 和Webots R2023b,以及即将发行的版本.

<span id="tasks"></span>

## 操作步骤

<span id="create-the-package-structure"></span>

### 1 创建软件包结构

让我们在自定义的 ROS 2 软件包中组织代码。 创建新软件包命名 `my_package` 从 `src` 您的 ROS 2 工作空间的文件夹。将您的终端当前目录更改为 `ros2_ws/src` 运行 :

##### Python

``` console
$ ros2 pkg create --build-type ament_python --license Apache-2.0 --node-name my_robot_driver my_package --dependencies rclpy geometry_msgs webots_ros2_driver
```

那个... `--node-name my_robot_driver` 选项将创建 `my_robot_driver.py` 模板 Python 插件 `my_package` 子文件夹,您稍后将修改。 `--dependencies rclpy geometry_msgs webots_ros2_driver` 选项指定所需的软件包 `my_robot_driver.py` 插件在“% 1”中 `package.xml` 文档。

我们再加一句 `launch` 备注a `worlds` 文件夹在 `my_package` 文件夹。

``` console
$ cd my_package
$ mkdir launch
$ mkdir worlds
```

您最后应该使用以下文件夹结构 :

``` console
src/
└── my_package/
    ├── launch/
    ├── my_package/
    │   ├── __init__.py
    │   └── my_robot_driver.py
    ├── resource/
    │   └── my_package
    ├── test/
    │   ├── test_copyright.py
    │   ├── test_flake8.py
    │   └── test_pep257.py
    ├── worlds/
    ├── package.xml
    ├── setup.cfg
    └── setup.py
```

##### C++

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 --node-name MyRobotDriver my_package --dependencies rclcpp geometry_msgs webots_ros2_driver pluginlib
```

那个... `--node-name MyRobotDriver` 选项将创建 `MyRobotDriver.cpp` 模板 C++ 插件 `my_package/src` 子文件夹,您稍后将修改。 `--dependencies rclcpp geometry_msgs webots_ros2_driver pluginlib` 选项指定所需的软件包 `MyRobotDriver` 插件在“% 1”中 `package.xml` 文档。

我们再加一句 `launch`, a `worlds` 备注a `resource` 文件夹在 `my_package` 文件夹。

``` console
$ cd my_package
$ mkdir launch
$ mkdir worlds
$ mkdir resource
```

必须创建两个额外的文件: 页眉文件 `MyRobotDriver` 页:1 `my_robot_driver.xml` 插件lib描述文件.

``` console
$ touch my_robot_driver.xml
$ touch include/my_package/MyRobotDriver.hpp
```

您最后应该使用以下文件夹结构 :

``` console
src/
└── my_package/
    ├── include/
    │   └── my_package/
    │       └── MyRobotDriver.hpp
    ├── launch/
    ├── resource/
    ├── src/
    │   └── MyRobotDriver.cpp
    ├── worlds/
    ├── CMakeList.txt
    ├── my_robot_driver.xml
    └── package.xml
```

<span id="setup-the-simulation-world"></span>

### 2 建立模拟世界

你需要一个包含机器人的世界文件来启动模拟. [`Download this world file`](Code/my_world.wbt) 并把它移动到里面 `my_package/worlds/`.

这是一个相当简单的文本文件, 您可以在文本编辑器中直观。 简单的机器人已经包含在此 。 `my_world.wbt` 世界档案.

> **说明**
>
> 如果你想学习如何在 Webots 创建自己的机器人模型, 你可以检查这个 [教程](https://cyberbotics.com/doc/guide/tutorial-6-4-wheels-robot).

<span id="edit-the-my-robot-driver-plugin"></span>

### 3 编辑 `my_robot_driver` 插件

那个... `webots_ros2_driver` 子包自动为大多数传感器创建 ROS 2 接口。 更多关于现有设备接口和如何配置这些接口的细节,请见教程的第二部分: [配置机器人仿真（高级）](Setting-Up-Simulation-Webots-Advanced.md)。在此任务中,您将通过创建自定义插件来扩展此接口。此自定义插件是一个相当于机器人控制器的ROS节点。您可以用它来访问 [Webots 机器人 API](https://cyberbotics.com/doc/reference/robot?tab-language=python) 并创建自己的话题和服务来控制你的机器人。

> **说明**
>
> 此教程的用意是显示一个基本实例, 并列出最低限度的依赖性。 然而, 您可以通过使用另一个插件来避免此插件的使用 。 `webots_ros2` 命名的子包装 `webots_ros2_control`,引入新的依赖关系。这个其他子软件包将创建一个接口。 `ros2_control` 软件包,方便对有差别的轮式机器人进行控制。

##### Python

打开 `my_package/my_package/my_robot_driver.py` 在您最喜欢的编辑器中将其内容替换为:

``` python
import rclpy
from geometry_msgs.msg import Twist

HALF_DISTANCE_BETWEEN_WHEELS = 0.045
WHEEL_RADIUS = 0.025

class MyRobotDriver:
    def init(self, webots_node, properties):
        self.__robot = webots_node.robot

        self.__left_motor = self.__robot.getDevice('left wheel motor')
        self.__right_motor = self.__robot.getDevice('right wheel motor')

        self.__left_motor.setPosition(float('inf'))
        self.__left_motor.setVelocity(0)

        self.__right_motor.setPosition(float('inf'))
        self.__right_motor.setVelocity(0)

        self.__target_twist = Twist()

        rclpy.init(args=None)
        self.__node = rclpy.create_node('my_robot_driver')
        self.__node.create_subscription(Twist, 'cmd_vel', self.__cmd_vel_callback, 1)

    def __cmd_vel_callback(self, twist):
        self.__target_twist = twist

    def step(self):
        rclpy.spin_once(self.__node, timeout_sec=0)

        forward_speed = self.__target_twist.linear.x
        angular_speed = self.__target_twist.angular.z

        command_motor_left = (forward_speed - angular_speed * HALF_DISTANCE_BETWEEN_WHEELS) / WHEEL_RADIUS
        command_motor_right = (forward_speed + angular_speed * HALF_DISTANCE_BETWEEN_WHEELS) / WHEEL_RADIUS

        self.__left_motor.setVelocity(command_motor_left)
        self.__right_motor.setVelocity(command_motor_right)
```

如你所见, `MyRobotDriver` 班级执行三种方法。

第一个方法,命名 `init(self, ...)`,实际上是 Python 的 ROS 节点对应 `__init__(self, ...)` 建筑师。 `init` 方法总是需要两个参数:

- 那个... `webots_node` 参数在 Webots 实例中包含一个引用。

- 那个... `properties` 参数是从URDF文件中给出的 XML 标记创建的字典([4 创建 my_robot.urdf 文件](#create-the-my-robot-urdf-file))并允许您向控制器传递参数。

模拟中的机器人实例 `self.__robot` 可用于访问 [Webots 机器人 API](https://cyberbotics.com/doc/reference/robot?tab-language=python)。然后,它得到两个电动机实例,然后以目标位置和目标速度初始化它们。最后创建一个ROS节点,并为命名为ROS的ROS主题注册了回调方法。 `/cmd_vel` 将处理 `Twist` 留言。

``` python
def init(self, webots_node, properties):
    self.__robot = webots_node.robot

    self.__left_motor = self.__robot.getDevice('left wheel motor')
    self.__right_motor = self.__robot.getDevice('right wheel motor')

    self.__left_motor.setPosition(float('inf'))
    self.__left_motor.setVelocity(0)

    self.__right_motor.setPosition(float('inf'))
    self.__right_motor.setVelocity(0)

    self.__target_twist = Twist()

    rclpy.init(args=None)
    self.__node = rclpy.create_node('my_robot_driver')
    self.__node.create_subscription(Twist, 'cmd_vel', self.__cmd_vel_callback, 1)
```

接下来是执行 `__cmd_vel_callback(self, twist)` 调用私有方法, 将要求每种方法 `Twist` 发件人 `/cmd_vel` 并保存在 `self.__target_twist` 成员变量。

``` python
def __cmd_vel_callback(self, twist):
    self.__target_twist = twist
```

最后, `step(self)` 方法在模拟的每个步骤中都使用。 `rclpy.spin_once()` 要保持ROS节点的顺利运行,需要使用该方法。在每一步骤中,该方法将获取所期望的 `forward_speed` 财务报告和财务报告 `angular_speed` 从 `self.__target_twist`由于马达被角速度控制,该方法随后将转换 `forward_speed` 财务报告和财务报告 `angular_speed` 转换取决于机器人的结构,更具体地说,取决于轮子的半径和它们之间的距离。

``` python
def step(self):
    rclpy.spin_once(self.__node, timeout_sec=0)

    forward_speed = self.__target_twist.linear.x
    angular_speed = self.__target_twist.angular.z

    command_motor_left = (forward_speed - angular_speed * HALF_DISTANCE_BETWEEN_WHEELS) / WHEEL_RADIUS
    command_motor_right = (forward_speed + angular_speed * HALF_DISTANCE_BETWEEN_WHEELS) / WHEEL_RADIUS

    self.__left_motor.setVelocity(command_motor_left)
    self.__right_motor.setVelocity(command_motor_right)
```

##### C++

打开 `my_package/include/my_package/MyRobotDriver.hpp` 在您最喜欢的编辑器中将其内容替换为:

``` cpp
#ifndef WEBOTS_ROS2_PLUGIN_EXAMPLE_HPP
#define WEBOTS_ROS2_PLUGIN_EXAMPLE_HPP

#include "rclcpp/macros.hpp"
#include "webots_ros2_driver/PluginInterface.hpp"
#include "webots_ros2_driver/WebotsNode.hpp"

#include "geometry_msgs/msg/twist.hpp"
#include "rclcpp/subscription.hpp"

namespace my_robot_driver {
class MyRobotDriver : public webots_ros2_driver::PluginInterface {
public:
  void step() override;
  void init(webots_ros2_driver::WebotsNode *node,
            std::unordered_map<std::string, std::string> &parameters) override;

private:
  void cmdVelCallback(const geometry_msgs::msg::Twist::SharedPtr msg);

  rclcpp::Subscription<geometry_msgs::msg::Twist>::SharedPtr
      cmd_vel_subscription_;
  geometry_msgs::msg::Twist cmd_vel_msg;

  WbDeviceTag right_motor;
  WbDeviceTag left_motor;
};
} // namespace my_robot_driver
#endif
```

班级 `MyRobotDriver` 定义,该定义继承自 `webots_ros2_driver::PluginInterface` 类。插件必须覆盖 `step(...)` 财务报告和财务报告 `init(...)` 函数。在表格中提供了更多细节。 `MyRobotDriver.cpp` 文件。插件内部将使用的若干帮助方法、回调和成员变量被私下宣布。

然后,打开 `my_package/src/MyRobotDriver.cpp` 在您最喜欢的编辑器中将其内容替换为:

``` cpp
#include "my_package/MyRobotDriver.hpp"

#include "rclcpp/qos.hpp"
#include <cstdio>
#include <functional>
#include <webots/motor.h>
#include <webots/robot.h>

#define HALF_DISTANCE_BETWEEN_WHEELS 0.045
#define WHEEL_RADIUS 0.025

namespace my_robot_driver {
void MyRobotDriver::init(
    webots_ros2_driver::WebotsNode *node,
    std::unordered_map<std::string, std::string> &parameters) {

  right_motor = wb_robot_get_device("right wheel motor");
  left_motor = wb_robot_get_device("left wheel motor");

  wb_motor_set_position(left_motor, INFINITY);
  wb_motor_set_velocity(left_motor, 0.0);

  wb_motor_set_position(right_motor, INFINITY);
  wb_motor_set_velocity(right_motor, 0.0);

  cmd_vel_subscription_ = node->create_subscription<geometry_msgs::msg::Twist>(
      "/cmd_vel", rclcpp::SensorDataQoS().reliable(),
      std::bind(&MyRobotDriver::cmdVelCallback, this, std::placeholders::_1));
}

void MyRobotDriver::cmdVelCallback(
    const geometry_msgs::msg::Twist::ConstSharedPtr msg) {
  cmd_vel_msg.linear = msg->linear;
  cmd_vel_msg.angular = msg->angular;
}

void MyRobotDriver::step() {
  auto forward_speed = cmd_vel_msg.linear.x;
  auto angular_speed = cmd_vel_msg.angular.z;

  auto command_motor_left =
      (forward_speed - angular_speed * HALF_DISTANCE_BETWEEN_WHEELS) /
      WHEEL_RADIUS;
  auto command_motor_right =
      (forward_speed + angular_speed * HALF_DISTANCE_BETWEEN_WHEELS) /
      WHEEL_RADIUS;

  wb_motor_set_velocity(left_motor, command_motor_left);
  wb_motor_set_velocity(right_motor, command_motor_right);
}
} // namespace my_robot_driver

#include "pluginlib/class_list_macros.hpp"
PLUGINLIB_EXPORT_CLASS(my_robot_driver::MyRobotDriver,
                       webots_ros2_driver::PluginInterface)
```

那个... `MyRobotDriver::init` 一旦插件被装入, 将执行方法 `webots_ros2_driver` 软件包。需要两个参数:

- 一个指针 `WebotsNode` 定义 `webots_ros2_driver`,它允许访问ROS 2节点函数。

- 那个... `parameters` 参数是无序字符串地图,由URDF文件中给出的 XML 标记创建([4 创建 my_robot.urdf 文件](#create-the-my-robot-urdf-file))并允许将参数传递给控制器。在此示例中不使用。

它通过设置机器人马达,设置其位置和速度,并订阅该插件来初始化插件. `/cmd_vel` 主题。

``` cpp
void MyRobotDriver::init(
    webots_ros2_driver::WebotsNode *node,
    std::unordered_map<std::string, std::string> &parameters) {

  right_motor = wb_robot_get_device("right wheel motor");
  left_motor = wb_robot_get_device("left wheel motor");

  wb_motor_set_position(left_motor, INFINITY);
  wb_motor_set_velocity(left_motor, 0.0);

  wb_motor_set_position(right_motor, INFINITY);
  wb_motor_set_velocity(right_motor, 0.0);

  cmd_vel_subscription_ = node->create_subscription<geometry_msgs::msg::Twist>(
      "/cmd_vel", rclcpp::SensorDataQoS().reliable(),
      std::bind(&MyRobotDriver::cmdVelCallback, this, std::placeholders::_1));
}
```

接下来是执行 `cmdVelCallback()` 调用函数,该函数将调用在您接收的每个 Twist 信件 `/cmd_vel` 并保存在 `cmd_vel_msg` 成员变量。

``` cpp
void MyRobotDriver::cmdVelCallback(
    const geometry_msgs::msg::Twist::ConstSharedPtr msg) {
  cmd_vel_msg.linear = msg->linear;
  cmd_vel_msg.angular = msg->angular;
}
```

那个... `step()` 方法在模拟的每个步骤中都会被调用。在每一个步骤中,该方法将检索想要的 `forward_speed` 财务报告和财务报告 `angular_speed` 从 `cmd_vel_msg`由于马达被角速度控制,该方法随后将转换 `forward_speed` 财务报告和财务报告 `angular_speed` 转换取决于机器人的结构,更具体地说,取决于轮子的半径和它们之间的距离。

``` cpp
void MyRobotDriver::step() {
  auto forward_speed = cmd_vel_msg.linear.x;
  auto angular_speed = cmd_vel_msg.angular.z;

  auto command_motor_left =
      (forward_speed - angular_speed * HALF_DISTANCE_BETWEEN_WHEELS) /
      WHEEL_RADIUS;
  auto command_motor_right =
      (forward_speed + angular_speed * HALF_DISTANCE_BETWEEN_WHEELS) /
      WHEEL_RADIUS;

  wb_motor_set_velocity(left_motor, command_motor_left);
  wb_motor_set_velocity(right_motor, command_motor_right);
}
```

文件的最后一行定义了结束 `my_robot_driver` 名称空间,并包含用于导出该名称的宏 `MyRobotDriver` 类作为插件使用 `PLUGINLIB_EXPORT_CLASS` 宏。 这样可以让插件在运行时被 Webots ROS2 驱动程序加载 。

``` cpp
#include "pluginlib/class_list_macros.hpp"
PLUGINLIB_EXPORT_CLASS(my_robot_driver::MyRobotDriver,
                       webots_ros2_driver::PluginInterface)
```

> **说明**
>
> 在C++中执行插件的同时,必须使用CAPI与Webots控制器库进行交互.

<span id="create-the-my-robot-urdf-file"></span> <span id="id2"></span>

### 4 创建 `my_robot.urdf` 文件

您现在必须创建 URDF 文件来宣布 `MyRobotDriver` 插件。 这将允许 `webots_ros2_driver` ROS节点可以发射插件并将其与目标机器人连接.

在那个 `my_package/resource` 文件夹创建名为文本文件 `my_robot.urdf` 含此内容 :

##### Python

``` xml
<?xml version="1.0" ?>
<robot name="My robot">
    <webots>
        <plugin type="my_package.my_robot_driver.MyRobotDriver" />
    </webots>
</robot>
```

那个... `type` 属性指定由文件的等级结构给出的类的路径。 `webots_ros2_driver` 负责根据指定的软件包和模块加载该类。

##### C++

``` xml
<?xml version="1.0" ?>
<robot name="My robot">
    <webots>
        <plugin type="my_robot_driver::MyRobotDriver" />
    </webots>
</robot>
```

那个... `type` 属性指定要加载的命名空间和类名。 `pluginlib` 负责根据指定信息加载该类。

> **说明**
>
> 此简单的 URDF 文件并不包含任何关于机器人的链接或联合信息, 因为此教程中不需要它。 然而, URDF 文件通常包含更多的信息, 如您所解释的那样 。 [URDF](../../../Intermediate/URDF/URDF-Main.md) 教学。

> **说明**
>
> 在此插件不使用任何输入参数, 但可以通过包含参数名称的标签来实现 。
>
> ##### Python
>
> ``` xml
> <plugin type="my_package.my_robot_driver.MyRobotDriver">
>     <parameterName>someValue</parameterName>
> </plugin>
> ```
>
> ##### C++
>
> ``` xml
> <plugin type="my_robot_driver::MyRobotDriver">
>     <parameterName>someValue</parameterName>
> </plugin>
> ```
>
> 这是用来将参数传递给现有的 Webots 设备插件(参见 [配置机器人仿真（高级）](Setting-Up-Simulation-Webots-Advanced.md)).

<span id="create-the-launch-file"></span>

### 5 创建发射文件

让我们创建发射文件,以方便地发射模拟和ROS控制器,使用单一命令。 `my_package/launch` 文件夹创建新文本文件 `robot_launch.py` 使用此代码 :

``` python
import os
import launch
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

    return LaunchDescription([
        webots,
        my_robot_driver,
        launch.actions.RegisterEventHandler(
            event_handler=launch.event_handlers.OnProcessExit(
                target_action=webots,
                on_exit=[launch.actions.EmitEvent(event=launch.events.Shutdown())],
            )
        )
    ])
```

那个... `WebotsLauncher` 对象是一个自定义动作,允许您启动 Webots 模拟实例。您必须在构建器中指定模拟器将打开的世界文件。

``` python
webots = WebotsLauncher(
    world=os.path.join(package_dir, 'worlds', 'my_world.wbt')
)
```

然后,与模拟机器人互动的ROS节点被创建。这个节点被命名为 `WebotsController`,则位于 `webots_ros2_driver` 软件包。

##### Linux

节点可以通过使用基于IPC和共享内存的自定义协议来与模拟机器人进行通信.

##### Windows

节点(在WSL)将能够通过TCP连接与模拟机器人(在本地Windows上的Webots)进行通信.

##### macOS

节点(在嵌入器容器中)将能够通过TCP连接与模拟机器人(在原生macOS上的Webots)进行通信.

对于您来说, 您需要运行一个节点的单个实例, 因为您在模拟中有一个单一的机器人。 但如果您在模拟中拥有更多的机器人, 您必须运行一个单个的节点。 。 Name `robot_name` 参数用于定义驱动程序应连接的机器人名称。 `robot_description` 参数持有 URDF 文件的路径,其中提及 `MyRobotDriver` 插件。 您可以看到 `WebotsController` 作为连接控制器插件与目标机器人的接口的节点.

``` python
my_robot_driver = WebotsController(
    robot_name='my_robot',
    parameters=[
        {'robot_description': robot_description_path},
    ]
)
```

之后,两个节点被设定在 `LaunchDescription` 构造器 :

``` python
return LaunchDescription([
    webots,
    my_robot_driver,
```

最后,增加了一个可选部分,以便在Webots终止时关闭所有节点(例如当它从图形用户界面关闭时).

``` python
launch.actions.RegisterEventHandler(
    event_handler=launch.event_handlers.OnProcessExit(
        target_action=webots,
        on_exit=[launch.actions.EmitEvent(event=launch.events.Shutdown())],
    )
)
```

> **说明**
>
> 详细情况 `WebotsController` 财务报告和财务报告 `WebotsLauncher` 参数可以找到 [在节点参考页面上](https://github.com/cyberbotics/webots_ros2/wiki/References-Nodes).

<span id="edit-additional-files"></span>

### 6 编辑附加文件

##### Python

在启动发射文件之前,必须修改 `setup.py` 要包含您添加的额外文件的文件。 打开 `my_package/setup.py` 并将其内容替换为:

``` python
from setuptools import setup

package_name = 'my_package'
data_files = []
data_files.append(('share/ament_index/resource_index/packages', ['resource/' + package_name]))
data_files.append(('share/' + package_name + '/launch', ['launch/robot_launch.py']))
data_files.append(('share/' + package_name + '/worlds', ['worlds/my_world.wbt']))
data_files.append(('share/' + package_name + '/resource', ['resource/my_robot.urdf']))
data_files.append(('share/' + package_name, ['package.xml']))

setup(
    name=package_name,
    version='0.0.0',
    packages=[package_name],
    data_files=data_files,
    install_requires=['setuptools'],
    zip_safe=True,
    maintainer='user',
    maintainer_email='user.name@mail.com',
    description='TODO: Package description',
    license='TODO: License declaration',
    tests_require=['pytest'],
    entry_points={
        'console_scripts': [
            'my_robot_driver = my_package.my_robot_driver:main',
        ],
    },
)
```

此设置软件包并添加到 `data_files` 变量新添加的文件 : `my_world.wbt`, `my_robot.urdf` 财务报告和财务报告 `robot_launch.py`.

##### C++

在启动发射文件之前,必须修改 `CMakeLists.txt` 财务报告和财务报告 `my_robot_driver.xml` 文件 :

- `CMakeLists.txt` 定义您的插件的编译规则。

- `my_robot_driver.xml` 用于插件lib 找到您的 Webots ROS 2 插件 。

打开 `my_package/my_robot_driver.xml` 并将其内容替换为:

``` xml
<library path="my_package">
  <!-- The `type` attribute is a reference to the plugin class. -->
  <!-- The `base_class_type` attribute is always `webots_ros2_driver::PluginInterface`. -->
  <class type="my_robot_driver::MyRobotDriver" base_class_type="webots_ros2_driver::PluginInterface">
    <description>
      This is a Webots ROS 2 plugin example
    </description>
  </class>
</library>
```

打开 `my_package/CMakeLists.txt` 并将其内容替换为:

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

CMakeLists.txt 导出插件配置文件 `pluginlib_export_plugin_description_file()`,定义 C++ 插件的共享库 `src/MyRobotDriver.cpp`,并使用 `ament_target_dependencies()`.

文件然后安装库、目录 `launch`, `resource`,以及 `worlds` 页:1 `share/my_package` 目录。最后,它导出包含目录和使用 `ament_export_include_directories()` 财务报告和财务报告 `ament_export_libraries()`,并分别使用 `ament_package()`.

<span id="test-the-code"></span>

### 7 测试代码

##### Linux

从ROS 2工作空间运行的终端:

``` console
$ colcon build
$ source install/local_setup.bash
$ ros2 launch my_package robot_launch.py
```

这将启动模拟。 Webots 将在第一次运行时自动安装, 以防它尚未安装 。

##### Windows

从您 WSL ROS 2 工作空间运行的终端:

``` console
$ colcon build
$ export WEBOTS_HOME=/mnt/c/Program\ Files/Webots
$ source install/local_setup.bash
$ ros2 launch my_package robot_launch.py
```

一定要用那个 `/mnt` 前缀在Webots安装文件夹路径前,以便从WSL访问Windows文件系统。

这将启动模拟。 Webots 将在第一次运行时自动安装, 以防它尚未安装 。

##### macOS

在 macOS 上,必须在主机上启动本地服务器从 VM 启动 Webots 。 本地服务器可以下载 [在 Webots- server 仓库中](https://github.com/cyberbotics/webots-server/blob/main/local_simulation_server.py).

在主机的终端(不在VM)中,指定Webots安装文件夹(例如. `/Applications/Webots.app`)并使用下列命令启动服务器:

``` console
$ export WEBOTS_HOME=/Applications/Webots.app
$ python3 local_simulation_server.py
```

使用 ROS 2 工作空间中的 Linux VM 终端,构建并启动您的自定义包:

``` console
$ colcon build
$ source install/local_setup.bash
$ ros2 launch my_package robot_launch.py
```

> **说明**
>
> 如果您想要手动安装 Webots, 可以下载它 [这儿](https://github.com/cyberbotics/webots/releases/latest).

然后,打开第二个终端并发送一个命令:

``` console
$ ros2 topic pub /cmd_vel geometry_msgs/Twist  "linear: { x: 0.1 }"
```

机器人正在前进。

![](Image/Robot_moving_forward.png)

此时,机器人能够盲目地遵循你的运动指令。但是它最终会随着你命令它向前移动而撞入墙壁。

![](Image/Robot_colliding_wall.png)

关闭 Webots 窗口, 这也应该关闭您从发射台启动的ROS 节点。 同时关闭主题命令 。 `Ctrl+C` 在第二航站楼。

<span id="summary"></span>

## 小结

在这个教程中,你与Webots一起设置了一个现实的机器人模拟,并实施了自定义插件来控制机器人的马达.

<span id="next-steps"></span>

## 后续步骤

为了改进模拟,机器人的传感器可以用来探测和避免障碍。

- [配置机器人仿真（高级）](Setting-Up-Simulation-Webots-Advanced.md).
