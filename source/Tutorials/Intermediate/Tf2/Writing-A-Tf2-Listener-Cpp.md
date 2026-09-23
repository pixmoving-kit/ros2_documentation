---
translation_status: machine_translated
source: Tutorials/Intermediate/Tf2/Writing-A-Tf2-Listener-Cpp.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="writing-a-listener-c"></span>

# 编写监听器（C++）

**目标：** 学习如何使用 tf2 来获取帧变换的存取.

**教程级别：** 中级

**用时：** 10分钟

<span id="background"></span>

## 背景

在之前的教程中,我们创建了tf2广播机,以发布龟到tf2的姿势.

在此教程中, 我们将创建一个 tf2 的听众开始使用 tf2 。

<span id="prerequisites"></span>

## 前提条件

此教程假设您已完成 [tf2 静态播音器教程( C++)](Writing-A-Tf2-Static-Broadcaster-Cpp.md) 页:1 [tf2 播音员辅导( C++)](Writing-A-Tf2-Broadcaster-Cpp.md)。在之前的教程中,我们创建了一个 `learning_tf2_cpp` 我们将继续从这个角度开展工作。

<span id="tasks"></span>

## 操作步骤

<span id="write-the-listener-node"></span>

### 1 写入收听器节点

让我们首先创建源文件。请到 `learning_tf2_cpp` 我们在上一个教程中创建的软件包。 `src` 目录通过输入以下命令下载示例听器代码 :

##### Linux

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_listener.cpp
```

##### macOS

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_listener.cpp
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_listener.cpp -o turtle_tf2_listener.cpp
```

或于权壳中:

``` console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/turtle_tf2_listener.cpp -o turtle_tf2_listener.cpp
```

使用您首选的文本编辑器打开文件 。

``` C++
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

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

<span id="examine-the-code"></span>

#### 1.1 审查守则

为了了解产卵海龟背后的服务如何运作,请参见: [写入一个简单的服务和客户端( C++)](../../Beginner-Client-Libraries/Writing-A-Simple-Cpp-Service-And-Client.md) 教学。

现在,让我们看看与获取帧转换相关的代码。 `tf2_ros` 包含一个 `TransformListener` 类,使接受变换的任务变得容易.

``` C++
#include "tf2_ros/transform_listener.h"
```

在这里,我们创建一个 `TransformListener` 对象。一旦创建了监听器,它就会开始接收Tf2在电线上的变换,并缓冲它们长达10秒。

``` C++
tf_listener_ =
  std::make_shared<tf2_ros::TransformListener>(*tf_buffer_);
```

> **说明**
>
> 上面的构造器( A)`TransformListener(*tf_buffer_)`)是一个简化的构造器,在罩子下创建一个单独的内部节点来管理订阅.
>
> 如果你正在写一个 **可编译节点** (组件)或需要变换的收听器来尊重节点特定选项和主题重映射(例如命名空间或主题重映射) `/tf`),通过 `this` (或你的节点) `NodeInterfaces`改为构造器:
>
> ``` C++
> tf_listener_ =
>   std::make_shared<tf2_ros::TransformListener>(*tf_buffer_, this);
> ```
>
> 这保证了订阅是在现有的节点上创建的,并继承所有参数和主题配置.

最后,我们向听众询问一个具体的转变。 `lookup_transform` 使用下列参数的方法:

1.  目标框架

2.  来源框架

3.  我们想要改变的时刻

提供 `tf2::TimePointZero` 所有这一切都被包在一个捕捉区块里 以便处理可能的例外。

``` C++
t = tf_buffer_->lookupTransform(
  toFrameRel, fromFrameRel,
  tf2::TimePointZero);
```

由此产生的转变代表了目标龟相对于: `turtle2`。然后使用海龟之间的角度来计算跟随目标海龟的速度命令。关于 tf2 的更一般信息,另见 [概念部分 tf2 页面](../../../Concepts/Intermediate/About-Tf2.md).

<span id="cmakelists-txt"></span>

#### 1.2 CMakeLists.txt

导航一个关卡返回 `learning_tf2_cpp` 目录,其中 `CMakeLists.txt` 财务报告和财务报告 `package.xml` 文件已经找到 。

现在打开 `CMakeLists.txt` 添加可执行文件并命名它 `turtle_tf2_listener`,您稍后将使用 `ros2 run`.

``` console
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

最后,添加: `install(TARGETS…)` 第 15 条 `ros2 run` 能找到您的可执行文件 :

``` console
install(TARGETS
    turtle_tf2_listener
    DESTINATION lib/${PROJECT_NAME})
```

<span id="update-the-launch-file"></span>

### 2 更新发射文件

打开所谓的发射文件 `turtle_tf2_demo_launch` 带有扩展的 `.py`, `.xml`,或 `.yaml` 输入 `src/learning_tf2_cpp/launch` 带有文本编辑器的目录, 在发射描述中添加两个新的节点, 添加发射参数, 并添加导入。 由此生成的文件应该看起来像 :

##### Python

``` python
from launch import LaunchDescription
from launch.actions import DeclareLaunchArgument
from launch.substitutions import LaunchConfiguration

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
        DeclareLaunchArgument(
            'target_frame', default_value='turtle1',
            description='Target frame name.'
        ),
        Node(
            package='learning_tf2_cpp',
            executable='turtle_tf2_broadcaster',
            name='broadcaster2',
            parameters=[
                {'turtlename': 'turtle2'}
            ]
        ),
        Node(
            package='learning_tf2_cpp',
            executable='turtle_tf2_listener',
            name='listener',
            parameters=[
                {'target_frame': LaunchConfiguration('target_frame')}
            ]
        ),
    ])
```

##### XML 数据

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <node pkg="turtlesim" exec="turtlesim_node" name="sim" />
  <node pkg="learning_tf2_cpp" exec="turtle_tf2_broadcaster" name="broadcaster1">
    <param name="turtlename" value="turtle1" />
  </node>
  <arg name="target_frame" default="turtle1" description="Target frame name." />
  <node pkg="learning_tf2_cpp" exec="turtle_tf2_broadcaster" name="broadcaster2">
    <param name="turtlename" value="turtle2" />
  </node>
  <node pkg="learning_tf2_cpp" exec="turtle_tf2_listener" name="listener">
    <param name="target_frame" value="$(var target_frame)" />
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
  - arg:
      name: "target_frame"
      default: "turtle1"
      description: "Target frame name."
  - node:
      pkg: "learning_tf2_cpp"
      exec: "turtle_tf2_broadcaster"
      name: "broadcaster2"
      param:
      - name: "turtlename"
        value: "turtle2"
  - node:
      pkg: "learning_tf2_cpp"
      exec: "turtle_tf2_listener"
      name: "listener"
      param:
      - name: "target_frame"
        value: "$(var target_frame)"
```

这个将宣布 `target_frame` 启动辩论,开始一个播音员 为第二只乌龟,我们将产卵 和一个听众,将赞同这些转变。

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

现在,你准备开始你的全龟演示:

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

您应该看到两只龟的图案。 在第二个终端窗口中, 命令如下 :

``` console
$ ros2 run turtlesim turtle_teleop_key
```

看事情是否可行, 请用箭头键在第一只龟周围开车( 确定您的终端窗口是活动的, 而不是模拟窗口) , 而您会看到第二只龟在第一只龟之后!

<span id="summary"></span>

## 小结

在此教程中, 您学会了如何使用 tf2 来访问框架转换 。 您也已完成了您自己首次尝试的龟兹演示文件的编写工作 。 [tf2 介绍](Introduction-To-Tf2.md) 教学。
