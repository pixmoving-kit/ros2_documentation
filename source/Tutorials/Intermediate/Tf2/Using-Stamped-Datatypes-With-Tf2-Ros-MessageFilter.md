---
translation_status: machine_translated
source: Tutorials/Intermediate/Tf2/Using-Stamped-Datatypes-With-Tf2-Ros-MessageFilter.rst
---

<span id="using-stamped-datatypes-with-tf2-ros-messagefilter"></span> <span id="usingstampeddatatypeswithtf2rosmessagefilter"></span>

# 使用印有标记的数据类型 `tf2_ros::MessageFilter`

**目标：** 学会如何使用 `tf2_ros::MessageFilter` 用于处理盖章的数据类型。

**教程级别：** 中级

**用时：** 10分钟

<span id="background"></span>

## 背景

此教程解释如何使用带有 tf2. 一些真实世界的传感器数据例子有:

> - 单摄像头和立体声
>
> - 激光扫描

假设一只新乌龟的名字 `turtle3` 但有一个高空摄像头追踪它的位置并发布它。 `PointStamped` B. 与《京都议定书》有关的信息 `world` 边框。

`turtle1` 想知道在哪里吗? `turtle3` 被比作自己。

做这个 `turtle1` 必须听主题在哪里 `turtle3`正在公布其姿势,等到转变成理想的框架之后,再开始行动。 `tf2_ros::MessageFilter` 很有用处。 `tf2_ros::MessageFilter` 将使用一个带有标题的 ROS 2 信件订阅并缓存它,直到有可能将其转换成目标框架。

<span id="prerequisites"></span>

## 前提条件

此教程期望您拥有 `turtle_tf2_py` 已安装软件包。

##### Ubuntu

``` console
$ sudo apt install ros-rolling-turtle-tf2-py
```

##### RHEL

``` console
$ sudo dnf install ros-rolling-turtle-tf2-py
```

##### 从源

``` console
# Clone the required package repository inside src directory of the ros2_ws
$ git clone https://github.com/ros/geometry_tutorials.git -b ros2
# Build the required package
$ colcon build --packages-select turtle_tf2_py
```

<span id="tasks"></span>

## 操作步骤

<span id="write-the-broadcaster-node-of-pointstamped-messages"></span>

### 1 写入 Point 标定消息的播音员节点

对于这个教程,我们将设置一个演示应用程序,它有一个节点(在 Python 中)来播放 `PointStamped` 位置信息 : `turtle3`.

首先,让我们创建源文件。

转到 `learning_tf2_py` [软件包](Writing-A-Tf2-Static-Broadcaster-Py.md) 我们在前一个教程中创建了。 `src/learning_tf2_py/learning_tf2_py` 目录通过输入以下命令来下载示例传感器消息广播器代码:

##### Linux

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_message_broadcaster.py
```

##### macOS

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_message_broadcaster.py
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_message_broadcaster.py -o turtle_tf2_message_broadcaster.py
```

或于权壳中:

``` console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_message_broadcaster.py -o turtle_tf2_message_broadcaster.py
```

使用您首选的文本编辑器打开文件 。

``` python
from geometry_msgs.msg import PointStamped
from geometry_msgs.msg import Twist

import rclpy
from rclpy.node import Node

from turtlesim.msg import Pose
from turtlesim.srv import Spawn


class PointPublisher(Node):

    def __init__(self):
        super().__init__('turtle_tf2_message_broadcaster')

        # Create a client to spawn a turtle
        self.spawner = self.create_client(Spawn, 'spawn')
        # Boolean values to store the information
        # if the service for spawning turtle is available
        self.turtle_spawning_service_ready = False
        # if the turtle was successfully spawned
        self.turtle_spawned = False
        # if the topics of turtle3 can be subscribed
        self.turtle_pose_cansubscribe = False

        self.timer = self.create_timer(1.0, self.on_timer)

    def on_timer(self):
        if self.turtle_spawning_service_ready:
            if self.turtle_spawned:
                self.turtle_pose_cansubscribe = True
            else:
                if self.result.done():
                    self.get_logger().info(
                        f'Successfully spawned {self.result.result().name}')
                    self.turtle_spawned = True
                else:
                    self.get_logger().info('Spawn is not finished')
        else:
            if self.spawner.service_is_ready():
                # Initialize request with turtle name and coordinates
                # Note that x, y and theta are defined as floats in turtlesim/srv/Spawn
                request = Spawn.Request()
                request.name = 'turtle3'
                request.x = 4.0
                request.y = 2.0
                request.theta = 0.0
                # Call request
                self.result = self.spawner.call_async(request)
                self.turtle_spawning_service_ready = True
            else:
                # Check if the service is ready
                self.get_logger().info('Service is not ready')

        if self.turtle_pose_cansubscribe:
            self.vel_pub = self.create_publisher(Twist, 'turtle3/cmd_vel', 10)
            self.sub = self.create_subscription(Pose, 'turtle3/pose', self.handle_turtle_pose, 10)
            self.pub = self.create_publisher(PointStamped, 'turtle3/turtle_point_stamped', 10)

    def handle_turtle_pose(self, msg):
        vel_msg = Twist()
        vel_msg.linear.x = 1.0
        vel_msg.angular.z = 1.0
        self.vel_pub.publish(vel_msg)

        ps = PointStamped()
        ps.header.stamp = self.get_clock().now().to_msg()
        ps.header.frame_id = 'world'
        ps.point.x = msg.x
        ps.point.y = msg.y
        ps.point.z = 0.0
        self.pub.publish(ps)


def main():
    rclpy.init()
    node = PointPublisher()
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        pass

    rclpy.shutdown()
```

<span id="examine-the-code"></span>

#### 1.1 审查守则

现在让我们看看密码。 `on_timer` 调用功能,我们产下 `turtle3` {\fn华文楷体\fs16\1cHE0E0E0}同时呼唤 `Spawn` 服务处 `turtlesim`,并初始化其位置在(4,2,0),当龟产卵服务准备好了.

``` python
# Initialize request with turtle name and coordinates
# Note that x, y and theta are defined as floats in turtlesim/srv/Spawn
request = Spawn.Request()
request.name = 'turtle3'
request.x = 4.0
request.y = 2.0
request.theta = 0.0
# Call request
self.result = self.spawner.call_async(request)
```

之后,节点会发布话题 `turtle3/cmd_vel`,主题 `turtle3/turtle_point_stamped`,并订阅主题 `turtle3/pose` 并运行回调函数 `handle_turtle_pose` 每一封来信上都写着

``` python
self.vel_pub = self.create_publisher(Twist, '/turtle3/cmd_vel', 10)
self.sub = self.create_subscription(Pose, '/turtle3/pose', self.handle_turtle_pose, 10)
self.pub = self.create_publisher(PointStamped, '/turtle3/turtle_point_stamped', 10)
```

最后,在召回函数中 `handle_turtle_pose`,我们初始化 `Twist` 发送电子邮件 `turtle3` 然后出版它们,以便 `turtle3` 沿着一个圆圈走 然后我们填满 `PointStamped` 发送电子邮件 `turtle3` 正在接收 `Pose` 并发布信息。

``` python
vel_msg = Twist()
vel_msg.linear.x = 1.0
vel_msg.angular.z = 1.0
self.vel_pub.publish(vel_msg)

ps = PointStamped()
ps.header.stamp = self.get_clock().now().to_msg()
ps.header.frame_id = 'world'
ps.point.x = msg.x
ps.point.y = msg.y
ps.point.z = 0.0
self.pub.publish(ps)
```

<span id="write-the-launch-file"></span>

#### 1.2 写入发射文件

为了运行这个演示,我们需要创建一个启动文件 `turtle_tf2_sensor_message_launch` 带有扩展的 `.py`, `.xml`,或 `.yaml` 输入 `launch` 软件包子目录 `learning_tf2_py`:

##### Python

``` python
from launch import LaunchDescription
from launch.actions import DeclareLaunchArgument
from launch_ros.actions import Node


def generate_launch_description():
    return LaunchDescription([
        DeclareLaunchArgument(
            'target_frame', default_value='turtle1',
            description='Target frame name.'
        ),
        Node(
            package='turtlesim',
            executable='turtlesim_node',
            name='sim',
            output='screen'
        ),
        Node(
            package='turtle_tf2_py',
            executable='turtle_tf2_broadcaster',
            name='broadcaster1',
            parameters=[
                {'turtlename': 'turtle1'}
            ]
        ),
        Node(
            package='turtle_tf2_py',
            executable='turtle_tf2_broadcaster',
            name='broadcaster2',
            parameters=[
                {'turtlename': 'turtle3'}
            ]
        ),
        Node(
            package='turtle_tf2_py',
            executable='turtle_tf2_message_broadcaster',
            name='message_broadcaster',
        ),
    ])
```

##### XML 数据

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <arg name="target_frame" default="turtle1" description="Target frame name." />
  <node pkg="turtlesim" exec="turtlesim_node" name="sim" output="screen" />
  <node pkg="turtle_tf2_py" exec="turtle_tf2_broadcaster" name="broadcaster1">
    <param name="turtlename" value="turtle1" />
  </node>
  <node pkg="turtle_tf2_py" exec="turtle_tf2_broadcaster" name="broadcaster2">
    <param name="turtlename" value="turtle3" />
  </node>
  <node pkg="turtle_tf2_py" exec="turtle_tf2_message_broadcaster" name="message_broadcaster" />
</launch>
```

##### 也门

``` yaml
%YAML 1.2
---
launch:
  - arg:
      name: "target_frame"
      default: "turtle1"
      description: "Target frame name."
  - node:
      pkg: "turtlesim"
      exec: "turtlesim_node"
      name: "sim"
      output: "screen"
  - node:
      pkg: "turtle_tf2_py"
      exec: "turtle_tf2_broadcaster"
      name: "broadcaster1"
      param:
      - name: "turtlename"
        value: "turtle1"
  - node:
      pkg: "turtle_tf2_py"
      exec: "turtle_tf2_broadcaster"
      name: "broadcaster2"
      param:
      - name: "turtlename"
        value: "turtle3"
  - node:
      pkg: "turtle_tf2_py"
      exec: "turtle_tf2_message_broadcaster"
      name: "message_broadcaster"
```

<span id="add-an-entry-point"></span>

#### 1.3 增加一个切入点

允许 `ros2 run` 命令来运行您的节点,您必须添加切入点到 `setup.py` 页:1 `src/learning_tf2_py` 目录).

将下行添加到 `'console_scripts':` 括号 :

``` python
'turtle_tf2_message_broadcaster = learning_tf2_py.turtle_tf2_message_broadcaster:main',
```

<span id="add-an-data-file"></span>

#### 1.4 添加数据文件

允许 `ros2 launch` 命令以启动您的发射文件,您必须添加数据文件到 `setup.py` 页:1 `src/learning_tf2_py` 目录).

导入以下顶端的库, 以 `setup.py`:

``` python
...
import os
from glob import glob
```

将下行添加到 `'data_files':` 括号 :

``` python
data_files=[
    ...
    (os.path.join('share', package_name, 'launch'), glob('launch/*')),
],
```

<span id="build"></span>

#### 1.5 建设

运行 `rosdep` 在工作空间的根中检查缺失的依赖性。

##### Linux

``` console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

##### macOS

rosdep 只运行在 Linux 上, 因此您需要安装 `geometry_msgs` 财务报告和财务报告 `turtlesim` 依附关系

##### Windows

rosdep 只运行在 Linux 上, 因此您需要安装 `geometry_msgs` 财务报告和财务报告 `turtlesim` 依附关系

然后,我们可以构建这个软件包:

##### Linux

``` console
$ colcon build --packages-select learning_tf2_py
```

##### macOS

``` console
$ colcon build --packages-select learning_tf2_py
```

##### Windows

``` console
$ colcon build --merge-install --packages-select learning_tf2_py
```

<span id="writing-the-message-filter-listener-node"></span>

### 2 写入信件过滤器/收听器节点

现在,让流水 `PointStamped` 数据 `turtle3` 一、导 言1 - 2 2 `turtle1` 可靠地,我们将创建消息过滤器/收听器节点的源文件.

转到 `learning_tf2_cpp` [软件包](Writing-A-Tf2-Static-Broadcaster-Cpp.md) 我们在前一个教程中创建了。 `src/learning_tf2_cpp/src` 目录下载文件 `turtle_tf2_message_filter.cpp` 输入以下命令:

##### Linux

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_message_filter.cpp
```

##### macOS

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_message_filter.cpp
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_message_filter.cpp -o turtle_tf2_message_filter.cpp
```

或于权壳中:

``` console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_message_filter.cpp -o turtle_tf2_message_filter.cpp
```

使用您首选的文本编辑器打开文件 。

``` C++
#include <chrono>
#include <memory>
#include <string>

#include "geometry_msgs/msg/point_stamped.hpp"
#include "message_filters/subscriber.h"
#include "rclcpp/rclcpp.hpp"
#include "tf2_ros/buffer.h"
#include "tf2_ros/create_timer_ros.h"
#include "tf2_ros/message_filter.h"
#include "tf2_ros/transform_listener.h"
#ifdef TF2_CPP_HEADERS
  #include "tf2_geometry_msgs/tf2_geometry_msgs.hpp"
#else
  #include "tf2_geometry_msgs/tf2_geometry_msgs.h"
#endif

using namespace std::chrono_literals;

class PoseDrawer : public rclcpp::Node
{
public:
  PoseDrawer()
  : Node("turtle_tf2_pose_drawer")
  {
    // Declare and acquire `target_frame` parameter
    target_frame_ = this->declare_parameter<std::string>("target_frame", "turtle1");

    std::chrono::duration<int> buffer_timeout(1);

    tf2_buffer_ = std::make_shared<tf2_ros::Buffer>(this->get_clock());
    // Create the timer interface before call to waitForTransform,
    // to avoid a tf2_ros::CreateTimerInterfaceException exception
    auto timer_interface = std::make_shared<tf2_ros::CreateTimerROS>(
      this->get_node_base_interface(),
      this->get_node_timers_interface());
    tf2_buffer_->setCreateTimerInterface(timer_interface);
    tf2_listener_ =
      std::make_shared<tf2_ros::TransformListener>(*tf2_buffer_);

    point_sub_.subscribe(this, "/turtle3/turtle_point_stamped");
    tf2_filter_ = std::make_shared<tf2_ros::MessageFilter<geometry_msgs::msg::PointStamped>>(
      point_sub_, *tf2_buffer_, target_frame_, 100, this->get_node_logging_interface(),
      this->get_node_clock_interface(), buffer_timeout);
    // Register a callback with tf2_ros::MessageFilter to be called when transforms are available
    tf2_filter_->registerCallback(&PoseDrawer::msgCallback, this);
  }

private:
  void msgCallback(const geometry_msgs::msg::PointStamped::SharedPtr point_ptr)
  {
    geometry_msgs::msg::PointStamped point_out;
    try {
      tf2_buffer_->transform(*point_ptr, point_out, target_frame_);
      RCLCPP_INFO(
        this->get_logger(), "Point of turtle3 in frame of turtle1: x:%f y:%f z:%f\n",
        point_out.point.x,
        point_out.point.y,
        point_out.point.z);
    } catch (const tf2::TransformException & ex) {
      RCLCPP_WARN(
        // Print exception which was caught
        this->get_logger(), "Failure %s\n", ex.what());
    }
  }

  std::string target_frame_;
  std::shared_ptr<tf2_ros::Buffer> tf2_buffer_;
  std::shared_ptr<tf2_ros::TransformListener> tf2_listener_;
  message_filters::Subscriber<geometry_msgs::msg::PointStamped> point_sub_;
  std::shared_ptr<tf2_ros::MessageFilter<geometry_msgs::msg::PointStamped>> tf2_filter_;
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<PoseDrawer>());
  rclcpp::shutdown();
  return 0;
}
```

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

<span id="id1"></span>

#### 2.1 审查守则

首先,你必须包括 `tf2_ros::MessageFilter` 标题来自 `tf2_ros` 软件包,以及以前使用的软件包 `tf2` 财务报告和财务报告 `ros2` 相关标题。

``` C++
#include "geometry_msgs/msg/point_stamped.hpp"
#include "message_filters/subscriber.h"
#include "rclcpp/rclcpp.hpp"
#include "tf2_ros/buffer.h"
#include "tf2_ros/create_timer_ros.h"
#include "tf2_ros/message_filter.h"
#include "tf2_ros/transform_listener.h"
#ifdef TF2_CPP_HEADERS
  #include "tf2_geometry_msgs/tf2_geometry_msgs.hpp"
#else
  #include "tf2_geometry_msgs/tf2_geometry_msgs.h"
#endif
```

第二,需要不断发生一些情况,例如: `tf2_ros::Buffer`, `tf2_ros::TransformListener` 财务报告和财务报告 `tf2_ros::MessageFilter`.

``` C++
std::string target_frame_;
std::shared_ptr<tf2_ros::Buffer> tf2_buffer_;
std::shared_ptr<tf2_ros::TransformListener> tf2_listener_;
message_filters::Subscriber<geometry_msgs::msg::PointStamped> point_sub_;
std::shared_ptr<tf2_ros::MessageFilter<geometry_msgs::msg::PointStamped>> tf2_filter_;
```

第三,《规则》2 `message_filters::Subscriber` 必须先初始化该主题。 `tf2_ros::MessageFilter` 必须初始化 。 `Subscriber` 对象。注释中的其他参数 `MessageFilter` 构造器是 `target_frame` 和调回函数。目标框架是它能够确保的框架 `canTransform` 。调用函数是数据准备好后调用的函数。

``` C++
PoseDrawer()
: Node("turtle_tf2_pose_drawer")
{
  // Declare and acquire `target_frame` parameter
  target_frame_ = this->declare_parameter<std::string>("target_frame", "turtle1");

  std::chrono::duration<int> buffer_timeout(1);

  tf2_buffer_ = std::make_shared<tf2_ros::Buffer>(this->get_clock());
  // Create the timer interface before call to waitForTransform,
  // to avoid a tf2_ros::CreateTimerInterfaceException exception
  auto timer_interface = std::make_shared<tf2_ros::CreateTimerROS>(
    this->get_node_base_interface(),
    this->get_node_timers_interface());
  tf2_buffer_->setCreateTimerInterface(timer_interface);
  tf2_listener_ =
    std::make_shared<tf2_ros::TransformListener>(*tf2_buffer_);

  point_sub_.subscribe(this, "/turtle3/turtle_point_stamped");
  tf2_filter_ = std::make_shared<tf2_ros::MessageFilter<geometry_msgs::msg::PointStamped>>(
    point_sub_, *tf2_buffer_, target_frame_, 100, this->get_node_logging_interface(),
    this->get_node_clock_interface(), buffer_timeout);
  // Register a callback with tf2_ros::MessageFilter to be called when transforms are available
  tf2_filter_->registerCallback(&PoseDrawer::msgCallback, this);
}
```

最后,回调方法将调用 `tf2_buffer_->transform` 当数据准备好并打印输出到控制台时。

``` C++
private:
  void msgCallback(const geometry_msgs::msg::PointStamped::SharedPtr point_ptr)
  {
    geometry_msgs::msg::PointStamped point_out;
    try {
      tf2_buffer_->transform(*point_ptr, point_out, target_frame_);
      RCLCPP_INFO(
        this->get_logger(), "Point of turtle3 in frame of turtle1: x:%f y:%f z:%f\n",
        point_out.point.x,
        point_out.point.y,
        point_out.point.z);
    } catch (const tf2::TransformException & ex) {
      RCLCPP_WARN(
        // Print exception which was caught
        this->get_logger(), "Failure %s\n", ex.what());
    }
  }
```

<span id="add-dependencies"></span>

#### 2.2 增加依附关系

在制作软件包之前 `learning_tf2_cpp`中,请在 `package.xml` 此软件包的文件 :

``` xml
<depend>message_filters</depend>
<depend>tf2_geometry_msgs</depend>
```

<span id="cmakelists-txt"></span>

#### 2.3 CMakeLists.txt (中文(简体) ).

和在 `CMakeLists.txt` 文件,在现有依赖关系下增加两行:

``` console
find_package(message_filters REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)
```

以下各行将处理ROS分布之间的差异:

``` console
if(TARGET tf2_geometry_msgs::tf2_geometry_msgs)
  get_target_property(_include_dirs tf2_geometry_msgs::tf2_geometry_msgs INTERFACE_INCLUDE_DIRECTORIES)
else()
  set(_include_dirs ${tf2_geometry_msgs_INCLUDE_DIRS})
endif()

find_file(TF2_CPP_HEADERS
  NAMES tf2_geometry_msgs.hpp
  PATHS ${_include_dirs}
  NO_CACHE
  PATH_SUFFIXES tf2_geometry_msgs
)
```

之后添加可执行文件并命名 `turtle_tf2_message_filter`,您稍后将使用 `ros2 run`.

``` console
add_executable(turtle_tf2_message_filter src/turtle_tf2_message_filter.cpp)
ament_target_dependencies(
  turtle_tf2_message_filter
  geometry_msgs
  message_filters
  rclcpp
  tf2
  tf2_geometry_msgs
  tf2_ros
)

if(EXISTS ${TF2_CPP_HEADERS})
  target_compile_definitions(turtle_tf2_message_filter PUBLIC -DTF2_CPP_HEADERS)
endif()
```

最后,添加: `install(TARGETS…)` 区域(低于其他现有节点),所以 `ros2 run` 能找到您的可执行文件 :

``` console
install(TARGETS
  turtle_tf2_message_filter
  DESTINATION lib/${PROJECT_NAME})
```

<span id="id2"></span>

#### 2.4 构建

运行 `rosdep` 在工作空间的根中检查缺失的依赖性。

##### Linux

``` console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

##### macOS

rosdep 只运行在 Linux 上, 因此您需要安装 `geometry_msgs` 财务报告和财务报告 `turtlesim` 依附关系

##### Windows

rosdep 只运行在 Linux 上, 因此您需要安装 `geometry_msgs` 财务报告和财务报告 `turtlesim` 依附关系

现在打开一个新的终端,导航到您工作空间的根部,并用命令重建软件包:

##### Linux

``` console
$ colcon build --packages-select learning_tf2_cpp
```

##### macOS

``` console
$ colcon build --packages-select learning_tf2_cpp
```

##### Windows

``` console
$ colcon build --merge-install --packages-select learning_tf2_cpp
```

打开新终端, 导航到您工作空间的根, 并源代码设置文件 :

##### Linux

``` console
$ . install/setup.bash
```

##### macOS

``` console
$ . install/setup.bash
```

##### Windows

在窗口命令行提示中 :

``` console
$ call install\setup.bat
```

或于权壳中:

``` console
$ .\install\setup.ps1
```

<span id="run"></span>

### 3 运行

首先,我们需要通过发射发射文件来运行几个节点(包括PointStamped messages的广播机节点) `turtle_tf2_sensor_message_launch`:

##### XML 数据

``` console
$ ros2 launch learning_tf2_py turtle_tf2_sensor_message_launch.xml
```

##### 也门

``` console
$ ros2 launch learning_tf2_py turtle_tf2_sensor_message_launch.yaml
```

##### Python

``` console
$ ros2 launch learning_tf2_py turtle_tf2_sensor_message_launch.py
```

这会引起 `turtlesim` 窗口和两只乌龟,其中 `turtle3` 正在沿着一个圆环移动,而 `turtle1` 。但您可以运行 `turtle_teleop_key` 在另一个终端驱动的节点 `turtle1` 移动 :

``` console
$ ros2 run turtlesim turtle_teleop_key
```

![](images/turtlesim_messagefilter.png)

现在如果你回过头来 `turtle3/turtle_point_stamped`:

``` console
$ ros2 topic echo /turtle3/turtle_point_stamped
header:
  stamp:
    sec: 1629877510
    nanosec: 902607040
  frame_id: world
point:
  x: 4.989276885986328
  y: 3.073937177658081
  z: 0.0
---
header:
  stamp:
    sec: 1629877510
    nanosec: 918389395
  frame_id: world
point:
  x: 4.987966060638428
  y: 3.089883327484131
  z: 0.0
---
header:
  stamp:
    sec: 1629877510
    nanosec: 934186680
  frame_id: world
point:
  x: 4.986400127410889
  y: 3.105806589126587
  z: 0.0
---
```

当演示运行时, 打开另一个终端并运行信件过滤器/ 收听器节点 :

``` console
$ ros2 run learning_tf2_cpp turtle_tf2_message_filter
[INFO] [1630016162.006173900] [turtle_tf2_pose_drawer]: Point of turtle3 in frame of turtle1: x:-6.493231 y:-2.961614 z:0.000000

[INFO] [1630016162.006291983] [turtle_tf2_pose_drawer]: Point of turtle3 in frame of turtle1: x:-6.472169 y:-3.004742 z:0.000000

[INFO] [1630016162.006326234] [turtle_tf2_pose_drawer]: Point of turtle3 in frame of turtle1: x:-6.479420 y:-2.990479 z:0.000000

[INFO] [1630016162.006355644] [turtle_tf2_pose_drawer]: Point of turtle3 in frame of turtle1: x:-6.486441 y:-2.976102 z:0.000000
```

<span id="summary"></span>

## 小结

在这个教程中,你学会了如何使用 tf2 中的传感器数据/消息。 具体地说,你学会了如何发布 `PointStamped` 如何倾听专题并改变专题框架 `PointStamped` 信件为 `tf2_ros::MessageFilter`.
