<span id="writing-a-broadcaster-c"></span>

# 编写广播器（C++）

**目标：** 学习向 tf2 广播机器人状态。

**教程级别：** 中级

**预计耗时：** 15 分钟

<span id="background"></span>

## 背景

本篇及下一篇将编写代码，复现 [tf2 入门](Introduction-To-Tf2.md)中的示例。之后的教程会扩展更高级的功能，包括变换查询的超时和跨时间查询。

<span id="prerequisites"></span>

## 前提条件

应已具备 ROS 2 基础知识，并完成 [tf2 入门](Introduction-To-Tf2.md)和[静态广播器教程](Writing-A-Tf2-Static-Broadcaster-Cpp.md)。本教程继续使用此前创建的 `learning_tf2_cpp` 包。

此前也应学习过[创建工作空间](../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md)和[创建软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md)。

<span id="tasks"></span>

## 任务

<span id="write-the-broadcaster-node"></span>

### 1 编写广播器节点

进入 `src/learning_tf2_cpp/src`，下载广播器示例源码。

Linux：

```console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_broadcaster.cpp
```

macOS：

```console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_broadcaster.cpp
```

Windows 命令提示符：

```console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_broadcaster.cpp -o turtle_tf2_broadcaster.cpp
```

或 PowerShell：

```console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_broadcaster.cpp -o turtle_tf2_broadcaster.cpp
```

用编辑器打开 `turtle_tf2_broadcaster.cpp`：

```C++
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

参阅 [rclcpp 便捷头文件说明](../../../_internal/Rclcpp-Convenience-Header-Note.md)。

<span id="examine-the-code"></span>

#### 1.1 分析代码

先定义并读取 `turtlename` 参数，指定海龟名称，例如 `turtle1` 或 `turtle2`：

```C++
turtlename_ = this->declare_parameter<std::string>("turtlename", "turtle");
```

然后订阅 `turtleX/pose` 话题，每收到一条消息就调用 `handle_turtle_pose`：

```C++
subscription_ = this->create_subscription<turtlesim::msg::Pose>(
  topic_name, 10,
  std::bind(&FramePublisher::handle_turtle_pose, this, _1));
```

创建 `TransformStamped` 并设置元数据：

1. 调用 `this->get_clock()->now()` 获取节点当前使用的时间，作为变换时间戳。
2. 将父坐标系设为 `world`。
3. 将子坐标系设为海龟自身的名称。

位姿消息处理函数将海龟的平移和旋转发布为 `world` 到 `turtleX` 的变换：

```C++
geometry_msgs::msg::TransformStamped t;

// Read message content and assign it to
// corresponding tf variables
t.header.stamp = this->get_clock()->now();
t.header.frame_id = "world";
t.child_frame_id = turtlename_.c_str();
```

将海龟位姿信息填入三维变换：

```C++
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

最后将构造好的变换交给 `TransformBroadcaster` 的 `sendTransform` 方法广播：

```C++
// Send the transformation
tf_broadcaster_->sendTransform(t);
```

<span id="cmakelists-txt"></span>

#### 1.2 CMakeLists.txt

返回 `learning_tf2_cpp`，其中有 `CMakeLists.txt` 和 `package.xml`。添加名为 `turtle_tf2_broadcaster` 的可执行目标，以供 `ros2 run` 使用：

```console
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

再加入安装规则，使 `ros2 run` 能找到它：

```console
install(TARGETS
    turtle_tf2_broadcaster
    DESTINATION lib/${PROJECT_NAME})
```

<span id="write-the-launch-file"></span>

### 2 编写启动文件

在 `src/learning_tf2_cpp` 中创建 `launch` 目录，按所选格式新建 `turtle_tf2_demo_launch.xml`、`.yaml` 或 `.py`，复制相应示例：

- [XML](launch/turtle_tf2_demo_launch.xml)

- [YAML](launch/turtle_tf2_demo_launch.yaml)

- [Python](launch/turtle_tf2_demo_launch.py)

<span id="id1"></span>

#### 2.1 分析代码

XML 文件以 XML 声明和根元素 `<launch>` 开头，见 [XML 示例第 1–2 行](launch/turtle_tf2_demo_launch.xml)。YAML 文件以版本声明和 `launch:` 键开头，见 [YAML 示例第 1–3 行](launch/turtle_tf2_demo_launch.yaml)。

Python 文件先从 `launch` 和 `launch_ros` 导入所需模块，见 [Python 示例第 1–2 行](launch/turtle_tf2_demo_launch.py)。`launch` 是通用启动框架，并非 ROS 2 专用；`launch_ros` 提供节点等 ROS 2 专用功能。

接着启动 turtlesim 仿真，以及向 tf2 广播 `turtle1` 状态的 `turtle_tf2_broadcaster`。对应代码见 XML 第 3–6 行、YAML 第 4–9 行、Python 第 5–20 行。

<span id="add-dependencies"></span>

#### 2.2 添加依赖

返回 `learning_tf2_cpp` 目录，打开 `package.xml`，根据启动文件用到的模块加入运行依赖：

```xml
<exec_depend>launch</exec_depend>
<exec_depend>launch_ros</exec_depend>
```

这声明了执行时额外需要的 `launch` 和 `launch_ros`。保存文件。

<span id="id2"></span>

#### 2.3 CMakeLists.txt

重新打开 `CMakeLists.txt`，加入安装 `launch/` 文件的规则：

```console
install(DIRECTORY launch
  DESTINATION share/${PROJECT_NAME})
```

更多启动文件知识见[创建启动文件](../Launch/Creating-Launch-Files.md)。

<span id="build"></span>

### 3 构建

在工作空间根目录运行 `rosdep` 检查缺失依赖。

Linux：

```console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

本教程的 macOS 和 Windows 流程需自行安装 `geometry_msgs`、`turtlesim`，因为此处的 rosdep 步骤仅用于 Linux。

在工作空间根目录构建。

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

运行启动文件，启动 turtlesim 仿真和广播器。

XML：

```console
$ ros2 launch learning_tf2_cpp turtle_tf2_demo_launch.xml
```

YAML：

```console
$ ros2 launch learning_tf2_cpp turtle_tf2_demo_launch.yaml
```

Python：

```console
$ ros2 launch learning_tf2_cpp turtle_tf2_demo_launch.py
```

在第二个终端执行：

```console
$ ros2 run turtlesim turtle_teleop_key
```

现在仿真中应有一只可以控制的海龟。

![](images/turtlesim_broadcast.png)

通过 `tf2_echo` 检查位姿是否确实广播到了 tf2：

```console
$ ros2 run tf2_ros tf2_echo world turtle1
```

命令应显示第一只海龟的位姿。将焦点放到 `turtle_teleop_key` 终端而非仿真窗口，使用方向键控制海龟。输出类似：

```console
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

此时查询 `world` 和 `turtle2` 之间的变换不会有结果，因为第二只海龟尚不存在。下一篇添加它后，就会广播 `turtle2` 的位姿。

<span id="summary"></span>

## 小结

本教程介绍了如何向 tf2 广播机器人位姿，即海龟的位置和朝向，以及如何使用 `tf2_echo`。要实际使用这些变换，请继续学习[编写 tf2 监听器](Writing-A-Tf2-Listener-Cpp.md)。
