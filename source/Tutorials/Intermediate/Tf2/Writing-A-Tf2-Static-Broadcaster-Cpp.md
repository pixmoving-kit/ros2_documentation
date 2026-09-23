---
translation_status: machine_translated
source: Tutorials/Intermediate/Tf2/Writing-A-Tf2-Static-Broadcaster-Cpp.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="writing-a-static-broadcaster-c"></span>

# 编写静态广播器（C++）

**目标：** 学习如何向 tf2 播放静态坐标帧.

**教程级别：** 中级

**用时：** 15分钟

<span id="background"></span>

## 背景

公布静态变换对于定义机器人基础与其传感器或非移动部件之间的关系很有用,例如,在激光扫描仪中心一个框架里进行激光扫描测量是最容易解释的.

这是一个独立的教程,涵盖静态变换的基本内容,它由两部分组成。在第一部分,我们将写出代码,发布静态变换到 tf2. 在第二部分,我们将解释如何使用命令行。 `static_transform_publisher` 可执行工具在 `tf2_ros`.

在接下来的两个教程中,我们会写出代码来复制演示文稿 [tf2 介绍](Introduction-To-Tf2.md) 教程。在此之后,以下的教程侧重于扩展具有更高级的 tf2 特性的演示。

<span id="prerequisites"></span>

## 前提条件

在之前的教程中,你学会了如何 [创建工作空间](../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md) 财务报告和财务报告 [创建软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md).

<span id="tasks"></span>

## 操作步骤

<span id="create-a-package"></span>

### 1 创建软件包

首先我们将创建一个用于此教程和以下教程的软件包。 软件包叫做 `learning_tf2_cpp` 将依赖于 `geometry_msgs`, `rclcpp`, `tf2`, `tf2_ros`,以及 `turtlesim`。此教程的代码被存储 [这儿](https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/static_turtle_tf2_broadcaster.cpp).

打开一个新的终端 [源代码 ROS 2 安装](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md) 这样一来 `ros2` 命令将会起作用。导航到工作空间 `src` 文件夹并创建新软件包 :

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 --dependencies geometry_msgs rclcpp tf2 tf2_ros turtlesim -- learning_tf2_cpp
```

您的终端将返回一个消息, 以验证您的软件包的创建 `learning_tf2_cpp` 以及所有必要的文件和文件夹。

<span id="write-the-static-broadcaster-node"></span>

### 2 写入静态播音器节点

让我们首先创建源文件。 `src/learning_tf2_cpp/src` 目录通过输入以下命令来下载示例静态播音器代码:

##### Linux

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/static_turtle_tf2_broadcaster.cpp
```

##### macOS

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/static_turtle_tf2_broadcaster.cpp
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/static_turtle_tf2_broadcaster.cpp -o static_turtle_tf2_broadcaster.cpp
```

或于权壳中:

``` console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/static_turtle_tf2_broadcaster.cpp -o static_turtle_tf2_broadcaster.cpp
```

使用您首选的文本编辑器打开文件 。

``` C++
#include <memory>

#include "geometry_msgs/msg/transform_stamped.hpp"
#include "rclcpp/rclcpp.hpp"
#include "tf2/LinearMath/Quaternion.h"
#include "tf2_ros/static_transform_broadcaster.h"

class StaticFramePublisher : public rclcpp::Node
{
public:
  explicit StaticFramePublisher(char * transformation[])
  : Node("static_turtle_tf2_broadcaster")
  {
    tf_static_broadcaster_ = std::make_shared<tf2_ros::StaticTransformBroadcaster>(this);

    // Publish static transforms once at startup
    this->make_transforms(transformation);
  }

private:
  void make_transforms(char * transformation[])
  {
    geometry_msgs::msg::TransformStamped t;

    t.header.stamp = this->get_clock()->now();
    t.header.frame_id = "world";
    t.child_frame_id = transformation[1];

    t.transform.translation.x = atof(transformation[2]);
    t.transform.translation.y = atof(transformation[3]);
    t.transform.translation.z = atof(transformation[4]);
    tf2::Quaternion q;
    q.setRPY(
      atof(transformation[5]),
      atof(transformation[6]),
      atof(transformation[7]));
    t.transform.rotation.x = q.x();
    t.transform.rotation.y = q.y();
    t.transform.rotation.z = q.z();
    t.transform.rotation.w = q.w();

    tf_static_broadcaster_->sendTransform(t);
  }

  std::shared_ptr<tf2_ros::StaticTransformBroadcaster> tf_static_broadcaster_;
};

int main(int argc, char * argv[])
{
  auto logger = rclcpp::get_logger("logger");

  // Obtain parameters from command line arguments
  if (argc != 8) {
    RCLCPP_INFO(
      logger, "Invalid number of parameters\nusage: "
      "$ ros2 run learning_tf2_cpp static_turtle_tf2_broadcaster "
      "child_frame_name x y z roll pitch yaw");
    return 1;
  }

  // As the parent frame of the transform is `world`, it is
  // necessary to check that the frame name passed is different
  if (strcmp(argv[1], "world") == 0) {
    RCLCPP_INFO(logger, "Your static turtle name cannot be 'world'");
    return 1;
  }

  // Pass parameters and initialize node
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<StaticFramePublisher>(argv));
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

#### 2.1 审查守则

现在让我们看看与公布静态龟姿势 tf2 相关的代码。 头几行包括所需的页眉文件。 首先,我们包括 `geometry_msgs/msg/transform_stamped.hpp` 访问 `TransformStamped` 消息类型, 我们将发布到变换树上 。

``` C++
#include "geometry_msgs/msg/transform_stamped.hpp"
```

事后, `rclcpp` 因此,列入《公约》 `rclcpp::Node` 可使用类。

``` C++
#include "rclcpp/rclcpp.hpp"
```

`tf2::Quaternion` 是四角星的类,为将欧勒角转换成四角星提供了方便的函数,反之亦然。我们还包括 `tf2_ros/static_transform_broadcaster.h` 用于使用 `StaticTransformBroadcaster` 使静态变换的出版变得容易。

``` C++
#include "tf2/LinearMath/Quaternion.h"
#include "tf2_ros/static_transform_broadcaster.h"
```

那个... `StaticFramePublisher` 类构造器以名称初始化节点 `static_turtle_tf2_broadcaster`. 然后 . . . . . `StaticTransformBroadcaster` 被创建,在启动时会发出一个静态转换。

``` C++
tf_static_broadcaster_ = std::make_shared<tf2_ros::StaticTransformBroadcaster>(this);

this->make_transforms(transformation);
```

在这里,我们创建 `TransformStamped` 对象,它将成为我们一旦有人居住后发送的信息。在传递实际变换值之前,我们需要给它适当的元数据。

1.  我们需要给正在出版的变形图贴上时间戳, `this->get_clock()->now()`

2.  那么我们需要设定我们所创建的链接的父框架的名称,在这种情况下 `world`

3.  最后,我们需要设定我们创建的链接的儿童框架的名称。

``` C++
geometry_msgs::msg::TransformStamped t;

t.header.stamp = this->get_clock()->now();
t.header.frame_id = "world";
t.child_frame_id = transformation[1];
```

在这里,我们填充龟的6D姿势(翻译和旋转).

``` C++
t.transform.translation.x = atof(transformation[2]);
t.transform.translation.y = atof(transformation[3]);
t.transform.translation.z = atof(transformation[4]);
tf2::Quaternion q;
q.setRPY(
  atof(transformation[5]),
  atof(transformation[6]),
  atof(transformation[7]));
t.transform.rotation.x = q.x();
t.transform.rotation.y = q.y();
t.transform.rotation.z = q.z();
t.transform.rotation.w = q.w();
```

最后,我们广播静态变换使用 `sendTransform()` 函数。

``` C++
tf_static_broadcaster_->sendTransform(t);
```

<span id="update-package-xml"></span>

#### 2.2 更新软件包.xml

导航一个关卡返回 `src/learning_tf2_cpp` 目录,其中 `CMakeLists.txt` 财务报告和财务报告 `package.xml` 已经为您创建文件 。

打开 `package.xml` 与您的文本编辑器。

如本报告所述, [创建软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md) 教程,确保填入 `<description>`, `<maintainer>` 财务报告和财务报告 `<license>` 标签 :

``` xml
<description>Learning tf2 with rclcpp</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

确保保存文件 。

<span id="cmakelists-txt"></span>

#### 2.3 CMakeLists.txt (中文(简体) ).

将可执行文件添加到 CMakeLists.txt 并命名 `static_turtle_tf2_broadcaster`,您稍后将使用 `ros2 run`.

``` console
add_executable(static_turtle_tf2_broadcaster src/static_turtle_tf2_broadcaster.cpp)
ament_target_dependencies(
   static_turtle_tf2_broadcaster
   geometry_msgs
   rclcpp
   tf2
   tf2_ros
)
```

最后,添加: `install(TARGETS…)` 第 15 条 `ros2 run` 能找到您的可执行文件 :

``` console
install(TARGETS
   static_turtle_tf2_broadcaster
   DESTINATION lib/${PROJECT_NAME})
```

<span id="build"></span>

### 3 构建

运行是好的做法 `rosdep` 在工作空间的根中, 在构建前检查缺失的依赖性 :

##### Linux

``` console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

##### macOS

rosdep 只运行在 Linux 上, 因此您需要安装 `geometry_msgs` 财务报告和财务报告 `turtlesim` 依附关系

##### Windows

rosdep 只运行在 Linux 上, 因此您需要安装 `geometry_msgs` 财务报告和财务报告 `turtlesim` 依附关系

仍然在工作空间的根部,构建您的新软件包:

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

现在运行 `static_turtle_tf2_broadcaster` 节点 :

``` console
$ ros2 run learning_tf2_cpp static_turtle_tf2_broadcaster mystaticturtle 0 0 1 0 0 0
```

这个是海龟的姿势 播放给 `mystaticturtle` 将1米的高度浮在地上

我们现在可以检查一下静态变换是否已经通过回荡 `tf_static` 主题。如果一切都好,你应该看到一个单一的静态转变:

``` console
$ ros2 topic echo /tf_static
transforms:
- header:
   stamp:
      sec: 1622908754
      nanosec: 208515730
   frame_id: world
child_frame_id: mystaticturtle
transform:
   translation:
      x: 0.0
      y: 0.0
      z: 1.0
   rotation:
      x: 0.0
      y: 0.0
      z: 0.0
      w: 1.0
```

<span id="the-proper-way-to-publish-static-transforms"></span>

## 公布静态变换的正确方式

此教程旨在显示 `StaticTransformBroadcaster` 用于发布静态变换 。 在您真正的开发过程中, 您不需要自己写这个代码, 并且应该使用专用代码 。 `tf2_ros` 用于实现该目标的工具。 `tf2_ros` 提供名为可执行文件 `static_transform_publisher` ,可以用作命令行工具或节点,您可以添加到您的发射文件中。

以下命令发布静态坐标转换为 tf2 , 从而在 z 中抵消 1 公尺, 且框架之间没有旋转 `world` 财务报告和财务报告 `mystaticturtle`在ROS 2中,卷/pitch/yaw分别指x/y/z轴的旋转.

``` console
$ ros2 run tf2_ros static_transform_publisher --x 0 --y 0 --z 1 --yaw 0 --pitch 0 --roll 0 --frame-id world --child-frame-id mystaticturtle
```

以下命令发布相同的静态坐标转换为tf2,但使用四角表示进行旋转.

``` console
$ ros2 run tf2_ros static_transform_publisher --x 0 --y 0 --z 1 --qx 0 --qy 0 --qz 0 --qw 1 --frame-id world --child-frame-id mystaticturtle
```

`static_transform_publisher` 既作为命令行工具设计,供手工使用,也供内部使用。 `launch` 用于设置静态变换的文件。例如:

##### XML 数据

``` xml
<launch>
  <node
    pkg="tf2_ros" exec="static_transform_publisher"
    args="--x 0 --y 0 --z 1 --yaw 0 --pitch 0 --roll 0 --frame-id world --child-frame-id mystaticturtle"
  />
</launch>
```

##### 也门

``` yaml
launch:
  - node:
      pkg: "tf2_ros"
      exec: "static_transform_publisher"
      args: "--x 0 --y 0 --z 1 --yaw 0 --pitch 0 --roll 0 --frame-id world --child-frame-id mystaticturtle"
```

##### Python

``` python
from launch import LaunchDescription
from launch_ros.actions import Node


def generate_launch_description():
    return LaunchDescription([
        Node(
            package='tf2_ros',
            executable='static_transform_publisher',
            arguments=[
                '--x', '0', '--y', '0', '--z', '1',
                '--yaw', '0', '--pitch', '0', '--roll',
                '0', '--frame-id', 'world', '--child-frame-id', 'mystaticturtle']
        ),
    ])
```

请注意,除下列情况外,其他所有论据均不在此列: `--frame-id` 财务报告和财务报告 `--child-frame-id` 是可选的; 如果未指定特定选项, 则将假定身份 。

<span id="summary"></span>

## 小结

在这个教程中,你学会了静态变换如何对定义帧之间的静态关系有用,比如: `mystaticturtle` 与《公约》第2条有关的 `world` 此外,您还学习了静态变换如何有助于理解传感器数据,例如激光扫描仪,将数据与一个共同坐标帧联系起来。最后,您自己写了节点,以发布静态变换到 tf2,并学会了如何使用静态变换来发布所需的静态变换 `static_transform_publisher` 可执行文件并启动文件 。
