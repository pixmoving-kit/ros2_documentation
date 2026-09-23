<span id="writing-a-static-broadcaster-c"></span>

# 编写静态广播器（C++）

**目标：** 学习向 tf2 广播静态坐标系。

**教程级别：** 中级

**预计耗时：** 15 分钟

<span id="background"></span>

## 背景

静态变换用于描述机器人基座与传感器或不动部件之间的关系。例如，在以激光扫描器中心为原点的坐标系中理解扫描测量值最为方便。

这是介绍静态变换基础的独立教程，分为两部分：先编写代码发布静态变换，再介绍 `tf2_ros` 中的命令行工具 `static_transform_publisher`。

后面两篇教程会编写代码，复现 [tf2 入门](Introduction-To-Tf2.md)中的示例，再进一步扩展更高级的 tf2 功能。

<span id="prerequisites"></span>

## 前提条件

应已学习[创建工作空间](../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md)和[创建软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md)。

<span id="tasks"></span>

## 任务

<span id="create-a-package"></span>

### 1 创建软件包

创建本篇及后续教程使用的 `learning_tf2_cpp` 包，它依赖 `geometry_msgs`、`rclcpp`、`tf2`、`tf2_ros` 和 `turtlesim`。完整代码见[源文件](https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/static_turtle_tf2_broadcaster.cpp)。

打开新终端，[加载 ROS 2 环境](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)，进入工作空间的 `src` 目录并创建软件包：

```console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 --dependencies geometry_msgs rclcpp tf2 tf2_ros turtlesim -- learning_tf2_cpp
```

终端会确认 `learning_tf2_cpp` 及所需文件和目录已创建。

<span id="write-the-static-broadcaster-node"></span>

### 2 编写静态广播器节点

在 `src/learning_tf2_cpp/src` 目录中下载示例源码。

Linux：

```console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/static_turtle_tf2_broadcaster.cpp
```

macOS：

```console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/static_turtle_tf2_broadcaster.cpp
```

Windows 命令提示符：

```console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/static_turtle_tf2_broadcaster.cpp -o static_turtle_tf2_broadcaster.cpp
```

或 PowerShell：

```console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/static_turtle_tf2_broadcaster.cpp -o static_turtle_tf2_broadcaster.cpp
```

用编辑器打开 `static_turtle_tf2_broadcaster.cpp`：

```C++
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

参阅 [rclcpp 便捷头文件说明](../../../_internal/Rclcpp-Convenience-Header-Note.md)。

<span id="examine-the-code"></span>

#### 2.1 分析代码

下面重点介绍向 tf2 发布海龟静态位姿的部分。首先引入 `TransformStamped` 消息类型，用于向变换树发布消息：

```C++
#include "geometry_msgs/msg/transform_stamped.hpp"
```

再引入 `rclcpp`，以使用其节点类：

```C++
#include "rclcpp/rclcpp.hpp"
```

`tf2::Quaternion` 提供欧拉角和四元数互相转换的便捷方法。同时引入 `tf2_ros/static_transform_broadcaster.h`，以使用 `StaticTransformBroadcaster`。

```C++
#include "tf2/LinearMath/Quaternion.h"
#include "tf2_ros/static_transform_broadcaster.h"
```

`StaticFramePublisher` 构造函数将节点名设为 `static_turtle_tf2_broadcaster`，然后创建 `StaticTransformBroadcaster`，在启动时发送一次静态变换。

```C++
tf_static_broadcaster_ = std::make_shared<tf2_ros::StaticTransformBroadcaster>(this);

this->make_transforms(transformation);
```

创建待发送的 `TransformStamped` 对象。在填写实际变换值之前，先设置元数据：

1. 用 `this->get_clock()->now()` 设置当前时间戳。
2. 将父坐标系设为 `world`。
3. 设置子坐标系名称。

```C++
geometry_msgs::msg::TransformStamped t;

t.header.stamp = this->get_clock()->now();
t.header.frame_id = "world";
t.child_frame_id = transformation[1];
```

填入海龟的六维位姿，即平移和旋转：

```C++
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

最后通过 `sendTransform()` 广播静态变换：

```C++
tf_static_broadcaster_->sendTransform(t);
```

<span id="update-package-xml"></span>

#### 2.2 更新 package.xml

返回 `src/learning_tf2_cpp`，其中已有 `CMakeLists.txt` 和 `package.xml`。用编辑器打开 `package.xml`，按照[创建软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md)教程，填写 `<description>`、`<maintainer>` 和 `<license>`：

```xml
<description>Learning tf2 with rclcpp</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

保存文件。

<span id="cmakelists-txt"></span>

#### 2.3 CMakeLists.txt

添加名为 `static_turtle_tf2_broadcaster` 的可执行目标，之后通过 `ros2 run` 调用：

```console
add_executable(static_turtle_tf2_broadcaster src/static_turtle_tf2_broadcaster.cpp)
ament_target_dependencies(
   static_turtle_tf2_broadcaster
   geometry_msgs
   rclcpp
   tf2
   tf2_ros
)
```

最后添加安装规则，使 `ros2 run` 能找到程序：

```console
install(TARGETS
   static_turtle_tf2_broadcaster
   DESTINATION lib/${PROJECT_NAME})
```

<span id="build"></span>

### 3 构建

构建前，建议在工作空间根目录运行 `rosdep` 检查缺失依赖。

Linux：

```console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

本教程的 macOS 和 Windows 流程需自行安装 `geometry_msgs`、`turtlesim`，因为此处的 rosdep 步骤仅用于 Linux。

仍在工作空间根目录构建。

Linux：

```console
$ colcon build --packages-select learning_tf2_cpp
```

macOS：

```console
$ colcon build --packages-select learning_tf2_cpp
```

Windows：

```console
$ colcon build --merge-install --packages-select learning_tf2_cpp
```

打开新终端，进入工作空间根目录并加载环境。

Linux：

```console
$ . install/setup.bash
```

macOS：

```console
$ . install/setup.bash
```

Windows 命令提示符：

```console
$ call install\setup.bat
```

或 PowerShell：

```console
$ .\install\setup.ps1
```

<span id="run"></span>

### 4 运行

运行静态广播器节点：

```console
$ ros2 run learning_tf2_cpp static_turtle_tf2_broadcaster mystaticturtle 0 0 1 0 0 0
```

这将发布 `mystaticturtle` 的位姿，使其位于地面上方 1 米。

查看 `tf_static` 话题以验证发布成功，正常情况下应看到一个静态变换：

```console
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

## 推荐的静态变换发布方式

本教程通过编写代码展示 `StaticTransformBroadcaster` 的用法。实际开发中，通常无须自己编写这些代码，可直接使用 `tf2_ros` 提供的 `static_transform_publisher`，既能从命令行运行，也可作为节点加入启动文件。

以下命令发布 `world` 和 `mystaticturtle` 之间的静态变换：z 方向偏移 1 米，无旋转。ROS 2 中 roll、pitch、yaw 分别表示绕 x、y、z 轴的旋转。

```console
$ ros2 run tf2_ros static_transform_publisher --x 0 --y 0 --z 1 --yaw 0 --pitch 0 --roll 0 --frame-id world --child-frame-id mystaticturtle
```

下面使用四元数表示旋转，发布相同的变换：

```console
$ ros2 run tf2_ros static_transform_publisher --x 0 --y 0 --z 1 --qx 0 --qy 0 --qz 0 --qw 1 --frame-id world --child-frame-id mystaticturtle
```

启动文件中的使用示例：

- [XML](launch/static_transform_publisher_launch.xml)
- [YAML](launch/static_transform_publisher_launch.yaml)
- [Python](launch/static_transform_publisher_launch.py)

除 `--frame-id` 和 `--child-frame-id` 外，其余参数均可省略。未指定的选项按单位变换的相应分量处理。

<span id="summary"></span>

## 小结

本教程介绍了如何用静态变换定义坐标系间固定的关系，例如 `mystaticturtle` 相对于 `world` 的关系；也介绍了将激光扫描器等传感器数据关联到公共坐标系的用途。你编写了静态变换发布节点，并学习了通过 `static_transform_publisher` 和启动文件发布所需变换。
