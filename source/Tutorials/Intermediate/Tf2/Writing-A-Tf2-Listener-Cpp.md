<span id="writing-a-listener-c"></span>

# 编写监听器（C++）

**目标：** 学习通过 tf2 获取坐标系变换。

**教程级别：** 中级

**预计耗时：** 10 分钟

<span id="background"></span>

## 背景

此前已经创建 tf2 广播器来发布海龟位姿。本教程创建监听器，开始使用这些变换。

<span id="prerequisites"></span>

## 前提条件

应已完成[静态广播器](Writing-A-Tf2-Static-Broadcaster-Cpp.md)和[广播器](Writing-A-Tf2-Broadcaster-Cpp.md)教程。本篇继续在此前的 `learning_tf2_cpp` 包中开发。

<span id="tasks"></span>

## 任务

<span id="write-the-listener-node"></span>

### 1 编写监听器节点

进入 `src/learning_tf2_cpp/src`，下载监听器源码。

Linux：

```console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_listener.cpp
```

macOS：

```console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_listener.cpp
```

Windows 命令提示符：

```console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_listener.cpp -o turtle_tf2_listener.cpp
```

或 PowerShell：

```console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_listener.cpp -o turtle_tf2_listener.cpp
```

用编辑器打开 `turtle_tf2_listener.cpp`：

```C++
#include <chrono>
#include <functional>
#include <memory>
#include <string>

#include "geometry_msgs/msg/transform_stamped.hpp"
#include "geometry_msgs/msg/twist.hpp"
#include "rclcpp/rclcpp.hpp"
#include "tf2/exceptions.h"
#include "tf2_ros/transform_listener.h"
#include "tf2_ros/buffer.h"
#include "turtlesim/srv/spawn.hpp"

using namespace std::chrono_literals;

class FrameListener : public rclcpp::Node
{
public:
  FrameListener()
  : Node("turtle_tf2_frame_listener"),
    turtle_spawning_service_ready_(false),
    turtle_spawned_(false)
  {
    // Declare and acquire `target_frame` parameter
    target_frame_ = this->declare_parameter<std::string>("target_frame", "turtle1");

    tf_buffer_ =
      std::make_unique<tf2_ros::Buffer>(this->get_clock());
    tf_listener_ =
      std::make_shared<tf2_ros::TransformListener>(*tf_buffer_);

    // Create a client to spawn a turtle
    spawner_ =
      this->create_client<turtlesim::srv::Spawn>("spawn");

    // Create turtle2 velocity publisher
    publisher_ =
      this->create_publisher<geometry_msgs::msg::Twist>("turtle2/cmd_vel", 1);

    // Call on_timer function every second
    timer_ = this->create_wall_timer(
      1s, std::bind(&FrameListener::on_timer, this));
  }

private:
  void on_timer()
  {
    // Store frame names in variables that will be used to
    // compute transformations
    std::string fromFrameRel = target_frame_.c_str();
    std::string toFrameRel = "turtle2";

    if (turtle_spawning_service_ready_) {
      if (turtle_spawned_) {
        geometry_msgs::msg::TransformStamped t;

        // Look up for the transformation between target_frame and turtle2 frames
        // and send velocity commands for turtle2 to reach target_frame
        try {
          t = tf_buffer_->lookupTransform(
            toFrameRel, fromFrameRel,
            tf2::TimePointZero);
        } catch (const tf2::TransformException & ex) {
          RCLCPP_INFO(
            this->get_logger(), "Could not transform %s to %s: %s",
            toFrameRel.c_str(), fromFrameRel.c_str(), ex.what());
          return;
        }

        geometry_msgs::msg::Twist msg;

        static const double scaleRotationRate = 1.0;
        msg.angular.z = scaleRotationRate * atan2(
          t.transform.translation.y,
          t.transform.translation.x);

        static const double scaleForwardSpeed = 0.5;
        msg.linear.x = scaleForwardSpeed * sqrt(
          pow(t.transform.translation.x, 2) +
          pow(t.transform.translation.y, 2));

        publisher_->publish(msg);
      } else {
        RCLCPP_INFO(this->get_logger(), "Successfully spawned");
        turtle_spawned_ = true;
      }
    } else {
      // Check if the service is ready
      if (spawner_->service_is_ready()) {
        // Initialize request with turtle name and coordinates
        // Note that x, y and theta are defined as floats in turtlesim/srv/Spawn
        auto request = std::make_shared<turtlesim::srv::Spawn::Request>();
        request->x = 4.0;
        request->y = 2.0;
        request->theta = 0.0;
        request->name = "turtle2";

        // Call request
        using ServiceResponseFuture =
          rclcpp::Client<turtlesim::srv::Spawn>::SharedFuture;
        auto response_received_callback = [this](ServiceResponseFuture future) {
            auto result = future.get();
            if (strcmp(result->name.c_str(), "turtle2") == 0) {
              turtle_spawning_service_ready_ = true;
            } else {
              RCLCPP_ERROR(this->get_logger(), "Service callback result mismatch");
            }
          };
        auto result = spawner_->async_send_request(request, response_received_callback);
      } else {
        RCLCPP_INFO(this->get_logger(), "Service is not ready");
      }
    }
  }

  // Boolean values to store the information
  // if the service for spawning turtle is available
  bool turtle_spawning_service_ready_;
  // if the turtle was successfully spawned
  bool turtle_spawned_;
  rclcpp::Client<turtlesim::srv::Spawn>::SharedPtr spawner_{nullptr};
  rclcpp::TimerBase::SharedPtr timer_{nullptr};
  rclcpp::Publisher<geometry_msgs::msg::Twist>::SharedPtr publisher_{nullptr};
  std::shared_ptr<tf2_ros::TransformListener> tf_listener_{nullptr};
  std::unique_ptr<tf2_ros::Buffer> tf_buffer_;
  std::string target_frame_;
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<FrameListener>());
  rclcpp::shutdown();
  return 0;
}
```

参阅 [rclcpp 便捷头文件说明](../../../_internal/Rclcpp-Convenience-Header-Note.md)。

<span id="examine-the-code"></span>

#### 1.1 分析代码

生成海龟所用服务的工作原理见[编写简单服务端和客户端](../../Beginner-Client-Libraries/Writing-A-Simple-Cpp-Service-And-Client.md)。

下面重点介绍获取坐标系变换的代码。`tf2_ros` 提供 `TransformListener`，简化接收变换的操作：

```C++
#include "tf2_ros/transform_listener.h"
```

创建监听器后，它就会开始接收网络中的 tf2 变换，缓存最长 10 秒的数据：

```C++
tf_listener_ =
  std::make_shared<tf2_ros::TransformListener>(*tf_buffer_);
```

> 上面的 `TransformListener(*tf_buffer_)` 是简化构造函数，会在内部新建独立节点管理订阅。编写可组合节点，或希望监听器遵循当前节点的选项与话题重映射（例如 `/tf` 的命名空间或重映射）时，应传入 `this` 或节点的 `NodeInterfaces`：

```C++
tf_listener_ =
  std::make_shared<tf2_ros::TransformListener>(*tf_buffer_, this);
```

这样订阅就会创建在现有节点上，并继承其参数和话题配置。

查询特定变换时，向 `lookupTransform` 传入目标坐标系、源坐标系和所需时间。传入 `tf2::TimePointZero` 即获取最新可用变换。用异常处理块包裹查询，以处理可能发生的异常：

```C++
t = tf_buffer_->lookupTransform(
  toFrameRel, fromFrameRel,
  tf2::TimePointZero);
```

所得变换表示目标海龟相对于 `turtle2` 的位置和朝向。再利用两者之间的角度计算速度命令，跟随目标。更多背景见 [tf2 概念](../../../Concepts/Intermediate/About-Tf2.md)。

<span id="cmakelists-txt"></span>

#### 1.2 CMakeLists.txt

返回包含 `CMakeLists.txt` 和 `package.xml` 的 `learning_tf2_cpp` 目录，添加名为 `turtle_tf2_listener` 的可执行目标：

```console
add_executable(turtle_tf2_listener src/turtle_tf2_listener.cpp)
ament_target_dependencies(
    turtle_tf2_listener
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
    turtle_tf2_listener
    DESTINATION lib/${PROJECT_NAME})
```

<span id="update-the-launch-file"></span>

### 2 更新启动文件

打开 `src/learning_tf2_cpp/launch` 中的 `turtle_tf2_demo_launch.xml`、`.yaml` 或 `.py`，加入两个新节点、一个启动参数，以及所需导入。更新后的完整示例：

- [XML](launch/listener_cpp_launch.xml)

- [YAML](launch/listener_cpp_launch.yaml)

- [Python](launch/listener_cpp_launch.py)

这会声明 `target_frame` 启动参数，为即将生成的第二只海龟启动广播器，并启动订阅这些变换的监听器。

<span id="build"></span>

### 3 构建

在工作空间根目录运行 `rosdep` 检查依赖。

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

启动完整海龟示例。

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

仿真中应出现两只海龟。在第二个终端运行：

```console
$ ros2 run turtlesim turtle_teleop_key
```

让终端而非仿真窗口处于焦点，用方向键控制第一只海龟，应看到第二只跟随它。

<span id="summary"></span>

## 小结

本教程介绍了通过 tf2 获取坐标系变换的方法，并完成了你在 [tf2 入门](Introduction-To-Tf2.md)中体验过的海龟仿真示例。
