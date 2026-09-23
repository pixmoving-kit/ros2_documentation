<span id="writing-a-simple-publisher-and-subscriber-c"></span> <span id="cpppubsub"></span>
# 编写简单的发布者和订阅者（C++）

**目标：** 使用 C++ 创建并运行发布者和订阅者节点。

**教程级别：** 初学者

**预计用时：** 20 分钟

<span id="background"></span>
## 背景

[节点](../Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md)通过 ROS 计算图通信。本教程的节点通过[话题](../Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)相互传递字符串消息。示例是简单的 talker（发布者）和 listener（订阅者）系统：一个节点发布数据，另一个订阅话题以接收数据。

示例代码见 [ros2/examples](https://github.com/ros2/examples/tree/rolling/rclcpp/topics)。

<span id="prerequisites"></span>
## 前提条件

此前教程介绍了[创建工作空间](Creating-A-Workspace/Creating-A-Workspace.md)和[创建软件包](Creating-Your-First-ROS2-Package.md)。

<span id="tasks"></span>
## 操作步骤

<span id="create-a-package"></span>
### 1 创建软件包

打开新终端，[加载 ROS 2 安装环境](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)，使 `ros2` 命令可用。

进入[此前创建](Creating-A-Workspace/Creating-A-Workspace.md#new-directory)的 `ros2_ws`。软件包应创建在 `src` 中，而非根目录，因此进入 `ros2_ws/src` 并运行：

```console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 cpp_pubsub
```

终端会确认已创建 `cpp_pubsub` 及其必需文件和目录。

进入 `ros2_ws/src/cpp_pubsub/src`。对于 CMake 软件包，可执行程序的源文件应放在这里。

<span id="write-the-publisher-node"></span>
### 2 编写发布者节点

运行对应命令下载 talker 示例代码。

**Linux**

```console
$ wget -O publisher_member_function.cpp https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_publisher/member_function.cpp
```

**macOS**

```console
$ wget -O publisher_member_function.cpp https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_publisher/member_function.cpp
```

**Windows 命令提示符**

```console
$ curl -sk https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_publisher/member_function.cpp -o publisher_member_function.cpp
```

**Windows PowerShell**

```console
$ curl https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_publisher/member_function.cpp -o publisher_member_function.cpp
```

目录中会出现 `publisher_member_function.cpp`。用文本编辑器打开：

```C++
#include <chrono>
#include <functional>
#include <memory>
#include <string>

#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/string.hpp"

using namespace std::chrono_literals;

/* This example creates a subclass of Node and uses std::bind() to register a
* member function as a callback from the timer. */

class MinimalPublisher : public rclcpp::Node
{
  public:
    MinimalPublisher()
    : Node("minimal_publisher"), count_(0)
    {
      publisher_ = this->create_publisher<std_msgs::msg::String>("topic", 10);
      timer_ = this->create_wall_timer(
      500ms, std::bind(&MinimalPublisher::timer_callback, this));
    }

  private:
    void timer_callback()
    {
      auto message = std_msgs::msg::String();
      message.data = "Hello, world! " + std::to_string(count_++);
      RCLCPP_INFO(this->get_logger(), "Publishing: '%s'", message.data.c_str());
      publisher_->publish(message);
    }
    rclcpp::TimerBase::SharedPtr timer_;
    rclcpp::Publisher<std_msgs::msg::String>::SharedPtr publisher_;
    size_t count_;
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<MinimalPublisher>());
  rclcpp::shutdown();
  return 0;
}
```

<span id="examine-the-code"></span>
#### 2.1 分析代码

代码开头包含所需的 C++ 标准头文件。随后是 `rclcpp/rclcpp.hpp`，提供 ROS 2 系统最常用的功能。最后的 `std_msgs/msg/string.hpp` 包含发布数据所用的内置消息类型。

另见 [rclcpp 便捷头文件说明](../../_internal/Rclcpp-Convenience-Header-Note.md)。

```C++
#include <chrono>
#include <functional>
#include <memory>
#include <string>

#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/string.hpp"

using namespace std::chrono_literals;
```

这些语句体现了节点依赖，下一节需要在 `package.xml` 和 `CMakeLists.txt` 中声明。

接下来通过继承 `rclcpp::Node` 创建 `MinimalPublisher` 节点类。代码中的 `this` 都指向该节点对象。

```C++
class MinimalPublisher : public rclcpp::Node
```

公开的构造函数将节点命名为 `minimal_publisher`，并把 `count_` 初始化为 0。构造函数中创建发布者，指定 `String` 消息类型、话题名称 `topic`，以及限制消息积压数量的队列大小。随后初始化 `timer_`，使 `timer_callback` 每秒执行两次。

```C++
public:
  MinimalPublisher()
  : Node("minimal_publisher"), count_(0)
  {
    publisher_ = this->create_publisher<std_msgs::msg::String>("topic", 10);
    timer_ = this->create_wall_timer(
    500ms, std::bind(&MinimalPublisher::timer_callback, this));
  }
```

`timer_callback` 设置消息数据并实际发布消息。`RCLCPP_INFO` 宏将每条发布的消息打印到控制台：

```C++
private:
  void timer_callback()
  {
    auto message = std_msgs::msg::String();
    message.data = "Hello, world! " + std::to_string(count_++);
    RCLCPP_INFO(this->get_logger(), "Publishing: '%s'", message.data.c_str());
    publisher_->publish(message);
  }
```

最后声明定时器、发布者和计数器成员：

```C++
rclcpp::TimerBase::SharedPtr timer_;
rclcpp::Publisher<std_msgs::msg::String>::SharedPtr publisher_;
size_t count_;
```

`MinimalPublisher` 类之后是实际运行节点的 `main` 函数。`rclcpp::init` 初始化 ROS 2；`rclcpp::spin` 开始处理节点的数据，包括定时器回调。

```C++
int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<MinimalPublisher>());
  rclcpp::shutdown();
  return 0;
}
```

<span id="add-dependencies"></span>
#### 2.2 添加依赖

返回上一级 `ros2_ws/src/cpp_pubsub`，其中已生成 `CMakeLists.txt` 和 `package.xml`。

打开 `package.xml`，按照[上一篇教程](Creating-Your-First-ROS2-Package.md)填写 `<description>`、`<maintainer>` 和 `<license>`：

```xml
<description>Examples of minimal publisher/subscriber using rclcpp</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

在 `ament_cmake` 构建工具依赖之后，添加与节点 include 语句对应的依赖：

```xml
<depend>rclcpp</depend>
<depend>std_msgs</depend>
```

这表示软件包在构建和运行时需要 `rclcpp` 和 `std_msgs`。保存文件。

<span id="cmakelists-txt"></span>
#### 2.3 CMakeLists.txt

打开 `CMakeLists.txt`，在已有的 `find_package(ament_cmake REQUIRED)` 下方添加：

```cmake
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
```

随后添加名为 `talker` 的可执行程序，以便通过 `ros2 run` 运行节点：

```cmake
add_executable(talker src/publisher_member_function.cpp)
ament_target_dependencies(talker rclcpp std_msgs)
```

最后添加 `install(TARGETS...)`，让 `ros2 run` 能找到可执行程序：

```cmake
install(TARGETS
  talker
  DESTINATION lib/${PROJECT_NAME})
```

可以删除不必要的部分和注释，整理后的 `CMakeLists.txt` 如下：

```cmake
cmake_minimum_required(VERSION 3.5)
project(cpp_pubsub)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)

add_executable(talker src/publisher_member_function.cpp)
ament_target_dependencies(talker rclcpp std_msgs)

install(TARGETS
  talker
  DESTINATION lib/${PROJECT_NAME})

ament_package()
```

现在已经可以构建、加载本地环境并运行；不过先创建订阅者节点，就能观察完整系统的工作情况。

<span id="write-the-subscriber-node"></span>
### 3 编写订阅者节点

返回 `ros2_ws/src/cpp_pubsub/src`，运行以下命令获取下一个节点。

**Linux**

```console
$ wget -O subscriber_member_function.cpp https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_subscriber/member_function.cpp
```

**macOS**

```console
$ wget -O subscriber_member_function.cpp https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_subscriber/member_function.cpp
```

**Windows 命令提示符**

```console
$ curl -sk https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_subscriber/member_function.cpp -o subscriber_member_function.cpp
```

**Windows PowerShell**

```console
$ curl https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_subscriber/member_function.cpp -o subscriber_member_function.cpp
```

确认以下文件存在：

```console
publisher_member_function.cpp  subscriber_member_function.cpp
```

用文本编辑器打开 `subscriber_member_function.cpp`：

```C++
#include <memory>

#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/string.hpp"
using std::placeholders::_1;

class MinimalSubscriber : public rclcpp::Node
{
  public:
    MinimalSubscriber()
    : Node("minimal_subscriber")
    {
      subscription_ = this->create_subscription<std_msgs::msg::String>(
      "topic", 10, std::bind(&MinimalSubscriber::topic_callback, this, _1));
    }

  private:
    void topic_callback(const std_msgs::msg::String & msg) const
    {
      RCLCPP_INFO(this->get_logger(), "I heard: '%s'", msg.data.c_str());
    }
    rclcpp::Subscription<std_msgs::msg::String>::SharedPtr subscription_;
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<MinimalSubscriber>());
  rclcpp::shutdown();
  return 0;
}
```

<span id="id1"></span>
#### 3.1 分析代码

订阅者代码与发布者非常相似。节点名称改为 `minimal_subscriber`，构造函数使用 `create_subscription` 创建订阅并注册回调。

这里不需要定时器，订阅者只需在 `topic` 话题上收到数据时作出响应：

```C++
public:
  MinimalSubscriber()
  : Node("minimal_subscriber")
  {
    subscription_ = this->create_subscription<std_msgs::msg::String>(
    "topic", 10, std::bind(&MinimalSubscriber::topic_callback, this, _1));
  }
```

根据[话题教程](../Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)，发布者与订阅者的话题名称和消息类型必须一致才能通信。

`topic_callback` 接收话题发布的字符串消息，通过 `RCLCPP_INFO` 宏将数据写入控制台。该类唯一的成员变量是订阅对象：

```C++
private:
  void topic_callback(const std_msgs::msg::String & msg) const
  {
    RCLCPP_INFO(this->get_logger(), "I heard: '%s'", msg.data.c_str());
  }
  rclcpp::Subscription<std_msgs::msg::String>::SharedPtr subscription_;
```

`main` 函数的结构相同，只是改为对 `MinimalSubscriber` 调用 spin。发布者的 spin 处理定时器回调，订阅者的 spin 则等待并处理随时到来的消息。

两个节点依赖相同，因此无需向 `package.xml` 添加新内容。

<span id="id2"></span>
#### 3.2 CMakeLists.txt

重新打开 `CMakeLists.txt`，在发布者配置下方添加订阅者的可执行程序和目标配置：

```cmake
add_executable(listener src/subscriber_member_function.cpp)
ament_target_dependencies(listener rclcpp std_msgs)

install(TARGETS
  talker
  listener
  DESTINATION lib/${PROJECT_NAME})
```

保存文件后，发布/订阅系统就准备好了。

<span id="build-and-run"></span> <span id="cpppubsub-build-and-run"></span>
### 4 构建并运行

ROS 2 安装中通常已包含 `rclcpp` 和 `std_msgs`。不过，推荐构建前在工作空间根目录 `ros2_ws` 运行 `rosdep` 检查缺失依赖。

**Linux**

```console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

**macOS 和 Windows**

本教程的 rosdep 步骤仅适用于 Linux，可跳到下一步。

仍在 `ros2_ws` 根目录，构建新软件包。

**Linux**

```console
$ colcon build --packages-select cpp_pubsub
```

**macOS**

```console
$ colcon build --packages-select cpp_pubsub
```

**Windows**

```console
$ colcon build --merge-install --packages-select cpp_pubsub
```

打开新终端，进入 `ros2_ws` 并加载环境设置文件。

**Linux**

```console
$ . install/setup.bash
```

**macOS**

```console
$ . install/setup.bash
```

**Windows**

```console
$ call install/setup.bat
```

运行 talker 节点，终端应每 0.5 秒输出一条 Info 消息：

```console
$ ros2 run cpp_pubsub talker
[INFO] [minimal_publisher]: Publishing: "Hello World: 0"
[INFO] [minimal_publisher]: Publishing: "Hello World: 1"
[INFO] [minimal_publisher]: Publishing: "Hello World: 2"
[INFO] [minimal_publisher]: Publishing: "Hello World: 3"
[INFO] [minimal_publisher]: Publishing: "Hello World: 4"
```

打开另一个终端，在 `ros2_ws` 中加载环境，然后启动 listener。它会从发布者当时的计数开始打印收到的消息：

```console
$ ros2 run cpp_pubsub listener
[INFO] [minimal_subscriber]: I heard: "Hello World: 10"
[INFO] [minimal_subscriber]: I heard: "Hello World: 11"
[INFO] [minimal_subscriber]: I heard: "Hello World: 12"
[INFO] [minimal_subscriber]: I heard: "Hello World: 13"
[INFO] [minimal_subscriber]: I heard: "Hello World: 14"
```

在每个终端中按 `Ctrl+C` 停止节点。

<span id="summary"></span>
## 小结

你创建了两个通过话题发布和订阅数据的节点，在编译和运行前，将依赖和可执行程序配置加入软件包配置文件。

<span id="next-steps"></span>
## 后续步骤

接下来创建另一个使用服务/客户端模型的简单 ROS 2 软件包，可以选择 [C++](Writing-A-Simple-Cpp-Service-And-Client.md) 或 [Python](Writing-A-Simple-Py-Service-And-Client.md)。

<span id="related-content"></span>
## 相关内容

C++ 发布者和订阅者有多种写法，可参阅 [ros2/examples](https://github.com/ros2/examples/tree/rolling/rclcpp/topics) 中的 `minimal_publisher` 和 `minimal_subscriber` 软件包。
