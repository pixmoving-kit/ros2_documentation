---
translation_status: machine_translated
source: Tutorials/Intermediate/Tf2/Writing-A-Tf2-Broadcaster-Cpp.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="writing-a-broadcaster-c"></span>

# 编写广播器（C++）

**目标：** 学习如何广播机器人状态到 tf2.

**教程级别：** 中级

**用时：** 15分钟

<span id="background"></span>

## 背景

在接下来的两个教程中,我们会写出代码来复制演示文稿 [tf2 介绍](Introduction-To-Tf2.md) 教程。在此之后,以下教程将侧重于扩展演示,使其具有更先进的 tf2 特性,包括在变换浏览和时间旅行中使用超时功能。

<span id="prerequisites"></span>

## 前提条件

这个教程假设你对ROS 2有工作知识,你已经完成了 [tf2 教程介绍](Introduction-To-Tf2.md) 财务报告和财务报告 [tf2 静态播音器教程( C++)](Writing-A-Tf2-Static-Broadcaster-Cpp.md)。我们将重新使用 `learning_tf2_cpp` 软件包,从最后一个教程。

在之前的教程中,你学会了如何 [创建工作空间](../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md) 财务报告和财务报告 [创建软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md).

<span id="tasks"></span>

## 操作步骤

<span id="write-the-broadcaster-node"></span>

### 1 写入播音员节点

让我们首先创建源文件。请到 `learning_tf2_cpp` 我们在上一个教程中创建的软件包。 `src` 目录通过输入以下命令来下载实例播放器代码:

##### Linux

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_broadcaster.cpp
```

##### macOS

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_broadcaster.cpp
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_broadcaster.cpp -o turtle_tf2_broadcaster.cpp
```

或于权壳中:

``` console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_broadcaster.cpp -o turtle_tf2_broadcaster.cpp
```

使用您首选的文本编辑器打开文件 。

``` C++
#include <functional>
#include <memory>
#include <sstream>
#include <string>

#include "geometry_msgs/msg/transform_stamped.hpp"
#include "rclcpp/rclcpp.hpp"
#include "tf2/LinearMath/Quaternion.h"
#include "tf2_ros/transform_broadcaster.h"
#include "turtlesim/msg/pose.hpp"

class FramePublisher : public rclcpp::Node
{
public:
  FramePublisher()
  : Node("turtle_tf2_frame_publisher")
  {
    // Declare and acquire `turtlename` parameter
    turtlename_ = this->declare_parameter<std::string>("turtlename", "turtle");

    // Initialize the transform broadcaster
    tf_broadcaster_ =
      std::make_unique<tf2_ros::TransformBroadcaster>(*this);

    // Subscribe to a turtle{1}{2}/pose topic and call handle_turtle_pose
    // callback function on each message
    std::ostringstream stream;
    stream << "/" << turtlename_.c_str() << "/pose";
    std::string topic_name = stream.str();

    subscription_ = this->create_subscription<turtlesim::msg::Pose>(
      topic_name, 10,
      std::bind(&FramePublisher::handle_turtle_pose, this, std::placeholders::_1));
  }

private:
  void handle_turtle_pose(const std::shared_ptr<const turtlesim::msg::Pose> msg)
  {
    geometry_msgs::msg::TransformStamped t;

    // Read message content and assign it to
    // corresponding tf variables
    t.header.stamp = this->get_clock()->now();
    t.header.frame_id = "world";
    t.child_frame_id = turtlename_.c_str();

    // Turtle only exists in 2D, thus we get x and y translation
    // coordinates from the message and set the z coordinate to 0
    t.transform.translation.x = msg->x;
    t.transform.translation.y = msg->y;
    t.transform.translation.z = 0.0;

    // For the same reason, turtle can only rotate around one axis
    // and this why we set rotation in x and y to 0 and obtain
    // rotation in z axis from the message
    tf2::Quaternion q;
    q.setRPY(0, 0, msg->theta);
    t.transform.rotation.x = q.x();
    t.transform.rotation.y = q.y();
    t.transform.rotation.z = q.z();
    t.transform.rotation.w = q.w();

    // Send the transformation
    tf_broadcaster_->sendTransform(t);
  }

  rclcpp::Subscription<turtlesim::msg::Pose>::SharedPtr subscription_;
  std::unique_ptr<tf2_ros::TransformBroadcaster> tf_broadcaster_;
  std::string turtlename_;
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<FramePublisher>());
  rclcpp::shutdown();
  return 0;
}
```

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

<span id="examine-the-code"></span>

#### 1.1 审查守则

现在,让我们看看与公布海龟的姿势相关的代码。 首先,我们定义并获得一个单一参数。 `turtlename`,它指定了龟名,例如: `turtle1` 或 时 间 `turtle2`.

``` C++
turtlename_ = this->declare_parameter<std::string>("turtlename", "turtle");
```

之后, 节点订阅主题 `turtleX/pose` 运行函数 `handle_turtle_pose` 每一封来信上都写着

``` C++
subscription_ = this->create_subscription<turtlesim::msg::Pose>(
  topic_name, 10,
  std::bind(&FramePublisher::handle_turtle_pose, this, _1));
```

现在,我们创建一个 `TransformStamped` 对象并给出适当的元数据。

1.  我们需要给正在出版的变换图案一个时间戳, `this->get_clock()->now()`中返回当前使用的时间。 `Node`.

2.  那么我们需要设定我们所创建的链接的父框架的名称,在这种情况下 `world`.

3.  最后,我们需要设定我们所创建的链接的儿童节点的名称,在这种情况下,这就是龟本身的名称。

龟的处理器功能将信息播放给龟的翻译和旋转,并将其出版为从框架的变换 `world` 创建框架 `turtleX`.

``` C++
geometry_msgs::msg::TransformStamped t;

// Read message content and assign it to
// corresponding tf variables
t.header.stamp = this->get_clock()->now();
t.header.frame_id = "world";
t.child_frame_id = turtlename_.c_str();
```

在这里,我们复制了来自3D龟姿势的信息进入3D变换.

``` C++
// Turtle only exists in 2D, thus we get x and y translation
// coordinates from the message and set the z coordinate to 0
t.transform.translation.x = msg->x;
t.transform.translation.y = msg->y;
t.transform.translation.z = 0.0;

// For the same reason, turtle can only rotate around one axis
// and this why we set rotation in x and y to 0 and obtain
// rotation in z axis from the message
tf2::Quaternion q;
q.setRPY(0, 0, msg->theta);
t.transform.rotation.x = q.x();
t.transform.rotation.y = q.y();
t.transform.rotation.z = q.z();
t.transform.rotation.w = q.w();
```

最后,我们做了我们构建的转变, 并把它传递给 `sendTransform` 方法 `TransformBroadcaster` 那会照顾广播。

``` C++
// Send the transformation
tf_broadcaster_->sendTransform(t);
```

<span id="cmakelists-txt"></span>

#### 1.2 CMakeLists.txt

导航一个关卡返回 `learning_tf2_cpp` 目录,其中 `CMakeLists.txt` 财务报告和财务报告 `package.xml` 文件已经找到 。

现在打开 `CMakeLists.txt` 添加可执行文件并命名它 `turtle_tf2_broadcaster`,您稍后将使用 `ros2 run`.

``` console
add_executable(turtle_tf2_broadcaster src/turtle_tf2_broadcaster.cpp)
ament_target_dependencies(
    turtle_tf2_broadcaster
    geometry_msgs
    rclcpp
    tf2
    tf2_ros
    turtlesim
)
```

最后,添加: `install(TARGETS…)` 第 15 条 `ros2 run` 能找到您的可执行文件 :

``` console
install(TARGETS
    turtle_tf2_broadcaster
    DESTINATION lib/${PROJECT_NAME})
```

<span id="write-the-launch-file"></span>

### 2 写入发射文件

现在为此演示创建一个启动文件。 创建 `launch` 文件夹中 `src/learning_tf2_cpp` 目录。用您的文本编辑器创建新文件 `turtle_tf2_demo_launch` 带有扩展的 `.py`, `.xml`,或 `.yaml` 输入 `launch` 文件夹,并添加以下行:

##### XML 数据

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <node pkg="turtlesim" exec="turtlesim_node" name="sim" />
  <node pkg="learning_tf2_cpp" exec="turtle_tf2_broadcaster" name="broadcaster1">
    <param name="turtlename" value="turtle1" />
  </node>
</launch>
```

##### 也门

``` yaml
%YAML 1.2
---
launch:
  - node:
      pkg: "turtlesim"
      exec: "turtlesim_node"
      name: "sim"
  - node:
      pkg: "learning_tf2_cpp"
      exec: "turtle_tf2_broadcaster"
      name: "broadcaster1"
      param:
      - name: "turtlename"
        value: "turtle1"
```

##### Python

``` python
from launch import LaunchDescription
from launch_ros.actions import Node


def generate_launch_description():
    return LaunchDescription([
        Node(
            package='turtlesim',
            executable='turtlesim_node',
            name='sim'
        ),
        Node(
            package='learning_tf2_cpp',
            executable='turtle_tf2_broadcaster',
            name='broadcaster1',
            parameters=[
                {'turtlename': 'turtle1'}
            ]
        ),
    ])
```

<span id="id1"></span>

#### 2.1 审查守则

让我们来检查一下发射文件的结构。每种格式都有自己设置发射文件的方法:

##### XML 数据

XML 启动文件从 XML 声明和根开始 `<launch>` 键。

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
```

##### 也门

YAML 启动文件从 YAML 版本声明开始, 以及一个 `launch:` 键。

``` yaml
%YAML 1.2
---
launch:
```

##### Python

在 Python 启动文件中,我们首先从 `launch` 财务报告和财务报告 `launch_ros` 软件包。应当指出, `launch` 是一个通用发射框架(而非ROS 2 具体内容),以及 `launch_ros` 有ROS 2 特殊的东西, 像节点,我们在这里导入。

``` python
from launch import LaunchDescription
from launch_ros.actions import Node
```

现在我们运行我们的节点 开始龟象模拟和广播 `turtle1` 状态到 tf2,使用我们 `turtle_tf2_broadcaster` 节点。

##### XML 数据

``` xml
  <node pkg="turtlesim" exec="turtlesim_node" name="sim" />
  <node pkg="learning_tf2_cpp" exec="turtle_tf2_broadcaster" name="broadcaster1">
    <param name="turtlename" value="turtle1" />
  </node>
```

##### 也门

``` yaml
  - node:
      pkg: "turtlesim"
      exec: "turtlesim_node"
      name: "sim"
  - node:
      pkg: "learning_tf2_cpp"
```

##### Python

``` python
def generate_launch_description():
    return LaunchDescription([
        Node(
            package='turtlesim',
            executable='turtlesim_node',
            name='sim'
        ),
        Node(
            package='learning_tf2_cpp',
            executable='turtle_tf2_broadcaster',
            name='broadcaster1',
            parameters=[
                {'turtlename': 'turtle1'}
            ]
        ),
    ])
```

<span id="add-dependencies"></span>

#### 2.2 增加依附关系

导航一个关卡返回 `learning_tf2_cpp` 目录,其中 `CMakeLists.txt` 财务报告和财务报告 `package.xml` 文件已经找到 。

打开 `package.xml` 使用文本编辑器。添加以下与您的发射文件导入语句相对应的依赖性 :

``` xml
<exec_depend>launch</exec_depend>
<exec_depend>launch_ros</exec_depend>
```

这说明需要额外 `launch` 财务报告和财务报告 `launch_ros` 执行代码时的依赖性 。

确保保存文件 。

<span id="id2"></span>

#### 2.3 CMakeLists.txt (中文(简体) ).

重新打开 `CMakeLists.txt` 并添加该行,使发射文件从 `launch/` 文件夹将被安装 。

``` console
install(DIRECTORY launch
  DESTINATION share/${PROJECT_NAME})
```

您可以在下列情况下学习更多关于创建启动文件的知识: [此教程](../Launch/Creating-Launch-Files.md).

<span id="build"></span>

### 3 构建

运行 `rosdep` 在工作空间的根中检查缺失的依赖性。

##### Linux

``` console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

##### macOS

rosdep 只运行在 Linux 上, 因此您需要安装 `geometry_msgs` 财务报告和财务报告 `turtlesim` 依附关系

##### Windows

rosdep 只运行在 Linux 上, 因此您需要安装 `geometry_msgs` 财务报告和财务报告 `turtlesim` 依附关系

仍然在工作区根部,构建您的软件包:

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

在 Windows 命令行提示中 :

``` console
$ call install\setup.bat
```

或于权壳中:

``` console
$ .\install\setup.ps1
```

<span id="run"></span>

### 4 运行

现在运行启动龟兹模拟节点的发射文件 `turtle_tf2_broadcaster` 节点 :

##### XML 数据

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_demo_launch.xml
```

##### 也门

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_demo_launch.yaml
```

##### Python

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_demo_launch.py
```

在第二个终端窗口中,以下命令:

``` console
$ ros2 run turtlesim turtle_teleop_key
```

你们现在可以看到龟类模拟的开始 是一个你能够控制的龟类。

![](images/turtlesim_broadcast.png)

现在,用 `tf2_echo` 用于检查龟姿是否真的被播放到 tf2: 的工具 :

``` console
$ ros2 run tf2_ros tf2_echo world turtle1
```

这应该能让你看到第一只龟的姿势。用箭键绕龟行驶(确保您能够 `turtle_teleop_key` 终端窗口是活动窗口, 而不是模拟窗口。 在控制台输出中, 您可以看到类似此窗口的东西 :

``` console
At time 1625137663.912474878
- Translation: [5.276, 7.930, 0.000]
- Rotation: in Quaternion [0.000, 0.000, 0.934, -0.357]
At time 1625137664.950813527
- Translation: [3.750, 6.563, 0.000]
- Rotation: in Quaternion [0.000, 0.000, 0.934, -0.357]
At time 1625137665.906280726
- Translation: [2.320, 5.282, 0.000]
- Rotation: in Quaternion [0.000, 0.000, 0.934, -0.357]
At time 1625137666.850775673
- Translation: [2.153, 5.133, 0.000]
- Rotation: in Quaternion [0.000, 0.000, -0.365, 0.931]
```

如果你跑的话 `tf2_echo` 中间的变换 `world` 财务报告和财务报告 `turtle2`,你不应该看到变形,因为第二只龟还没有出现。但是,一旦我们把第二只龟加到下一个教程中,姿势就是: `turtle2` 将广播到 tf2。

<span id="summary"></span>

## 小结

在这个教程中,你学会了如何播放机器人的姿势(龟的姿势和方向)到 tf2,以及如何使用 `tf2_echo` 工具。要实际使用播放到 tf2 的变换,您应该转到下一个关于创建 [tf2 收听器](Writing-A-Tf2-Listener-Cpp.md).
