---
translation_status: machine_translated
source: Tutorials/Intermediate/Writing-an-Action-Server-Client/Cpp.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="writing-an-action-server-and-client-c"></span> <span id="actionscpp"></span>

# 编写动作服务端与客户端（C++）

**目标：** 在 C++ 中执行动作服务器和客户端.

**教程级别：** 中级

**用时：** 15分钟

<span id="background"></span>

## 背景

动作是在ROS中的一种同步通信形式. *行动客户* 发送目标请求到 *动作服务器*. *动作服务器* 将目标反馈和结果发送给 *动作客户端*.

<span id="prerequisites"></span>

## 前提条件

你需要那个... `action_tutorials_interfaces` 软件包和 `Fibonacci.action` 在上一个教程中定义的界面, [创建动作](../Creating-an-Action.md).

<span id="tasks"></span>

## 操作步骤

<span id="creating-the-action-tutorials-cpp-package"></span>

### 1 创建动作_tutorys_cpp 套件

正如我们所看到的那样 [创建软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md) 教程,我们需要创建一个新的软件包来保存我们的 C++ 和辅助代码 。

<span id="id1"></span>

#### 1.1 创建动作_tutoris_cpp 套件

进入您在其中创建的动作工作空间 [上一个教程](../Creating-an-Action.md) (记住要源代码工作空间),并为 C++ 动作服务器创建新软件包 :

##### Linux

``` console
$ cd ~/ros2_ws/src
$ ros2 pkg create --dependencies action_tutorials_interfaces rclcpp rclcpp_action rclcpp_components -- action_tutorials_cpp
```

##### macOS

``` console
$ cd ~/ros2_ws/src
$ ros2 pkg create --dependencies action_tutorials_interfaces rclcpp rclcpp_action rclcpp_components -- action_tutorials_cpp
```

##### Windows

``` console
$ cd \dev\ros2_ws\src
$ ros2 pkg create --dependencies action_tutorials_interfaces rclcpp rclcpp_action rclcpp_components -- action_tutorials_cpp
```

<span id="adding-in-visibility-control"></span>

#### 1.2 在可见度控制中添加

为了使软件包在Windows上编译和工作,我们需要在一些“可见控制”中添加。 [Windows 文档中的 Windows 符号可见度](../../../The-ROS2-Project/Contributing/Windows-Tips-and-Tricks.md#windows-symbol-visibility).

开门 `action_tutorials_cpp/include/action_tutorials_cpp/visibility_control.h`,并插入以下代码:

``` c++
#ifndef ACTION_TUTORIALS_CPP__VISIBILITY_CONTROL_H_
#define ACTION_TUTORIALS_CPP__VISIBILITY_CONTROL_H_

#ifdef __cplusplus
extern "C"
{
#endif

// This logic was borrowed (then namespaced) from the examples on the gcc wiki:
//     https://gcc.gnu.org/wiki/Visibility

#if defined _WIN32 || defined __CYGWIN__
  #ifdef __GNUC__
    #define ACTION_TUTORIALS_CPP_EXPORT __attribute__ ((dllexport))
    #define ACTION_TUTORIALS_CPP_IMPORT __attribute__ ((dllimport))
  #else
    #define ACTION_TUTORIALS_CPP_EXPORT __declspec(dllexport)
    #define ACTION_TUTORIALS_CPP_IMPORT __declspec(dllimport)
  #endif
  #ifdef ACTION_TUTORIALS_CPP_BUILDING_DLL
    #define ACTION_TUTORIALS_CPP_PUBLIC ACTION_TUTORIALS_CPP_EXPORT
  #else
    #define ACTION_TUTORIALS_CPP_PUBLIC ACTION_TUTORIALS_CPP_IMPORT
  #endif
  #define ACTION_TUTORIALS_CPP_PUBLIC_TYPE ACTION_TUTORIALS_CPP_PUBLIC
  #define ACTION_TUTORIALS_CPP_LOCAL
#else
  #define ACTION_TUTORIALS_CPP_EXPORT __attribute__ ((visibility("default")))
  #define ACTION_TUTORIALS_CPP_IMPORT
  #if __GNUC__ >= 4
    #define ACTION_TUTORIALS_CPP_PUBLIC __attribute__ ((visibility("default")))
    #define ACTION_TUTORIALS_CPP_LOCAL  __attribute__ ((visibility("hidden")))
  #else
    #define ACTION_TUTORIALS_CPP_PUBLIC
    #define ACTION_TUTORIALS_CPP_LOCAL
  #endif
  #define ACTION_TUTORIALS_CPP_PUBLIC_TYPE
#endif

#ifdef __cplusplus
}
#endif

#endif  // ACTION_TUTORIALS_CPP__VISIBILITY_CONTROL_H_
```

<span id="writing-an-action-server"></span>

### 2 写入动作服务器

让我们集中力量写一个动作服务器,利用我们创建的动作来计算Fibonacci序列 [创建动作](../Creating-an-Action.md) 教学。

<span id="writing-the-action-server-code"></span>

#### 2.1 写入动作服务器代码

开门 `action_tutorials_cpp/src/fibonacci_action_server.cpp`,并插入以下代码:

``` c++
#include <functional>
#include <memory>
#include <thread>

#include "action_tutorials_interfaces/action/fibonacci.hpp"
#include "rclcpp/rclcpp.hpp"
#include "rclcpp_action/rclcpp_action.hpp"
#include "rclcpp_components/register_node_macro.hpp"

#include "action_tutorials_cpp/visibility_control.h"

namespace action_tutorials_cpp
{
class FibonacciActionServer : public rclcpp::Node
{
public:
  using Fibonacci = action_tutorials_interfaces::action::Fibonacci;
  using GoalHandleFibonacci = rclcpp_action::ServerGoalHandle<Fibonacci>;

  ACTION_TUTORIALS_CPP_PUBLIC
  explicit FibonacciActionServer(const rclcpp::NodeOptions & options = rclcpp::NodeOptions())
  : Node("fibonacci_action_server", options)
  {
    using namespace std::placeholders;

    this->action_server_ = rclcpp_action::create_server<Fibonacci>(
      this,
      "fibonacci",
      std::bind(&FibonacciActionServer::handle_goal, this, _1, _2),
      std::bind(&FibonacciActionServer::handle_cancel, this, _1),
      std::bind(&FibonacciActionServer::handle_accepted, this, _1));
  }

private:
  rclcpp_action::Server<Fibonacci>::SharedPtr action_server_;

  rclcpp_action::GoalResponse handle_goal(
    const rclcpp_action::GoalUUID & uuid,
    std::shared_ptr<const Fibonacci::Goal> goal)
  {
    RCLCPP_INFO(this->get_logger(), "Received goal request with order %d", goal->order);
    (void)uuid;
    return rclcpp_action::GoalResponse::ACCEPT_AND_EXECUTE;
  }

  rclcpp_action::CancelResponse handle_cancel(
    const std::shared_ptr<GoalHandleFibonacci> goal_handle)
  {
    RCLCPP_INFO(this->get_logger(), "Received request to cancel goal");
    (void)goal_handle;
    return rclcpp_action::CancelResponse::ACCEPT;
  }

  void handle_accepted(const std::shared_ptr<GoalHandleFibonacci> goal_handle)
  {
    using namespace std::placeholders;
    // this needs to return quickly to avoid blocking the executor, so spin up a new thread
    std::thread{std::bind(&FibonacciActionServer::execute, this, _1), goal_handle}.detach();
  }

  void execute(const std::shared_ptr<GoalHandleFibonacci> goal_handle)
  {
    RCLCPP_INFO(this->get_logger(), "Executing goal");
    rclcpp::Rate loop_rate(1);
    const auto goal = goal_handle->get_goal();
    auto feedback = std::make_shared<Fibonacci::Feedback>();
    auto & sequence = feedback->partial_sequence;
    sequence.push_back(0);
    sequence.push_back(1);
    auto result = std::make_shared<Fibonacci::Result>();

    for (int i = 1; (i < goal->order) && rclcpp::ok(); ++i) {
      // Check if there is a cancel request
      if (goal_handle->is_canceling()) {
        result->sequence = sequence;
        goal_handle->canceled(result);
        RCLCPP_INFO(this->get_logger(), "Goal canceled");
        return;
      }
      // Update sequence
      sequence.push_back(sequence[i] + sequence[i - 1]);
      // Publish feedback
      goal_handle->publish_feedback(feedback);
      RCLCPP_INFO(this->get_logger(), "Publish feedback");

      loop_rate.sleep();
    }

    // Check if goal is done
    if (rclcpp::ok()) {
      result->sequence = sequence;
      goal_handle->succeed(result);
      RCLCPP_INFO(this->get_logger(), "Goal succeeded");
    }
  }
};  // class FibonacciActionServer

}  // namespace action_tutorials_cpp

RCLCPP_COMPONENTS_REGISTER_NODE(action_tutorials_cpp::FibonacciActionServer)
```

前几行包括我们需要编译的所有信头.

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

接下来我们创建一个类 一个衍生的类: `rclcpp::Node`:

``` c++
class FibonacciActionServer : public rclcpp::Node
```

设计器 `FibonacciActionServer` 类初始化节点名称为 `fibonacci_action_server`:

``` c++
  explicit FibonacciActionServer(const rclcpp::NodeOptions & options = rclcpp::NodeOptions())
  : Node("fibonacci_action_server", options)
```

构造器还即时化新动作服务器 :

``` c++
    this->action_server_ = rclcpp_action::create_server<Fibonacci>(
      this,
      "fibonacci",
      std::bind(&FibonacciActionServer::handle_goal, this, _1, _2),
      std::bind(&FibonacciActionServer::handle_cancel, this, _1),
      std::bind(&FibonacciActionServer::handle_accepted, this, _1));
```

动作服务器需要六件东西 :

1.  模板动作类型名称 : `Fibonacci`.

2.  一个ROS 2节点将动作添加到: `this`.

3.  动作名称 : `'fibonacci'`.

4.  一个处理目标的回调函数 : `handle_goal`

5.  处理取消的召回功能 : `handle_cancel`.

6.  处理目标时的回调功能接受 : `handle_accept`.

各种回调的执行是文件的下一个。 请注意, 所有的回调需要迅速返回, 否则我们有可能饿死执行者 。

我们从处理新目标的回话开始:

``` c++
  rclcpp_action::GoalResponse handle_goal(
    const rclcpp_action::GoalUUID & uuid,
    std::shared_ptr<const Fibonacci::Goal> goal)
  {
    RCLCPP_INFO(this->get_logger(), "Received goal request with order %d", goal->order);
    (void)uuid;
    return rclcpp_action::GoalResponse::ACCEPT_AND_EXECUTE;
  }
```

这种执行只是接受所有目标。

接下来是处理取消的回调 :

``` c++
  rclcpp_action::CancelResponse handle_cancel(
    const std::shared_ptr<GoalHandleFibonacci> goal_handle)
  {
    RCLCPP_INFO(this->get_logger(), "Received request to cancel goal");
    (void)goal_handle;
    return rclcpp_action::CancelResponse::ACCEPT;
  }
```

这一执行只是告诉客户,它接受了取消。

最后一个回调接受一个新的目标并开始处理:

``` c++
  void handle_accepted(const std::shared_ptr<GoalHandleFibonacci> goal_handle)
  {
    using namespace std::placeholders;
    // this needs to return quickly to avoid blocking the executor, so spin up a new thread
    std::thread{std::bind(&FibonacciActionServer::execute, this, _1), goal_handle}.detach();
  }
```

由于行刑是长期的行动,我们从一个线上孵化出来来做实际的工作,然后从 `handle_accepted` 快点

所有进一步处理和更新均在 `execute` 新线程中的方法 :

``` c++
  void execute(const std::shared_ptr<GoalHandleFibonacci> goal_handle)
  {
    RCLCPP_INFO(this->get_logger(), "Executing goal");
    rclcpp::Rate loop_rate(1);
    const auto goal = goal_handle->get_goal();
    auto feedback = std::make_shared<Fibonacci::Feedback>();
    auto & sequence = feedback->partial_sequence;
    sequence.push_back(0);
    sequence.push_back(1);
    auto result = std::make_shared<Fibonacci::Result>();

    for (int i = 1; (i < goal->order) && rclcpp::ok(); ++i) {
      // Check if there is a cancel request
      if (goal_handle->is_canceling()) {
        result->sequence = sequence;
        goal_handle->canceled(result);
        RCLCPP_INFO(this->get_logger(), "Goal canceled");
        return;
      }
      // Update sequence
      sequence.push_back(sequence[i] + sequence[i - 1]);
      // Publish feedback
      goal_handle->publish_feedback(feedback);
      RCLCPP_INFO(this->get_logger(), "Publish feedback");

      loop_rate.sleep();
    }

    // Check if goal is done
    if (rclcpp::ok()) {
      result->sequence = sequence;
      goal_handle->succeed(result);
      RCLCPP_INFO(this->get_logger(), "Goal succeeded");
    }
  }
```

此工作线索每秒处理一个Fibonacci序列序列的序列号, 发布每个步骤的反馈更新。 当它完成处理后, 它会标记 `goal_handle` 已成功,则退出。

我们现在已经有了一个功能完备的动作服务器。让我们把它建成并运行起来。

<span id="compiling-the-action-server"></span>

#### 2.2 编译动作服务器

在上一节中,我们设置了动作服务器代码。要编译和运行它,我们需要做一些额外的工作。

首先,我们需要设置 CMakeLists.txt , 以便编译动作服务器。 打开 `action_tutorials_cpp/CMakeLists.txt`,然后在 `find_package` 电话:

``` cmake
add_library(action_server SHARED
  src/fibonacci_action_server.cpp)
target_include_directories(action_server PRIVATE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_compile_definitions(action_server
  PRIVATE "ACTION_TUTORIALS_CPP_BUILDING_DLL")
ament_target_dependencies(action_server
  "action_tutorials_interfaces"
  "rclcpp"
  "rclcpp_action"
  "rclcpp_components")
rclcpp_components_register_node(action_server PLUGIN "action_tutorials_cpp::FibonacciActionServer" EXECUTABLE fibonacci_action_server)
install(TARGETS
  action_server
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin)
```

现在我们可以编译软件包了,请到顶层 `ros2_ws`,然后运行 :

``` console
$ colcon build
```

这应当汇编整个工作空间,包括 `fibonacci_action_server` 输入 `action_tutorials_cpp` 软件包。

<span id="running-the-action-server"></span>

#### 2.3 运行动作服务器

现在我们已经建造了动作服务器,我们可以运行它。源码我们刚刚建造的工作空间(`ros2_ws`),并尝试运行动作服务器:

``` console
$ ros2 run action_tutorials_cpp fibonacci_action_server
```

<span id="writing-an-action-client"></span>

### 3 写入动作客户端

<span id="writing-the-action-client-code"></span>

#### 3.1 写入动作客户端代码

开门 `action_tutorials_cpp/src/fibonacci_action_client.cpp`,并插入以下代码:

``` c++
#include <functional>
#include <future>
#include <memory>
#include <string>
#include <sstream>

#include "action_tutorials_interfaces/action/fibonacci.hpp"

#include "rclcpp/rclcpp.hpp"
#include "rclcpp_action/rclcpp_action.hpp"
#include "rclcpp_components/register_node_macro.hpp"

namespace action_tutorials_cpp
{
class FibonacciActionClient : public rclcpp::Node
{
public:
  using Fibonacci = action_tutorials_interfaces::action::Fibonacci;
  using GoalHandleFibonacci = rclcpp_action::ClientGoalHandle<Fibonacci>;

  explicit FibonacciActionClient(const rclcpp::NodeOptions & options)
  : Node("fibonacci_action_client", options)
  {
    this->client_ptr_ = rclcpp_action::create_client<Fibonacci>(
      this,
      "fibonacci");

    this->timer_ = this->create_wall_timer(
      std::chrono::milliseconds(500),
      std::bind(&FibonacciActionClient::send_goal, this));
  }

  void send_goal()
  {
    using namespace std::placeholders;

    this->timer_->cancel();

    if (!this->client_ptr_->wait_for_action_server()) {
      RCLCPP_ERROR(this->get_logger(), "Action server not available after waiting");
      rclcpp::shutdown();
    }

    auto goal_msg = Fibonacci::Goal();
    goal_msg.order = 10;

    RCLCPP_INFO(this->get_logger(), "Sending goal");

    auto send_goal_options = rclcpp_action::Client<Fibonacci>::SendGoalOptions();
    send_goal_options.goal_response_callback =
      std::bind(&FibonacciActionClient::goal_response_callback, this, _1);
    send_goal_options.feedback_callback =
      std::bind(&FibonacciActionClient::feedback_callback, this, _1, _2);
    send_goal_options.result_callback =
      std::bind(&FibonacciActionClient::result_callback, this, _1);
    this->client_ptr_->async_send_goal(goal_msg, send_goal_options);
  }

private:
  rclcpp_action::Client<Fibonacci>::SharedPtr client_ptr_;
  rclcpp::TimerBase::SharedPtr timer_;

  void goal_response_callback(const GoalHandleFibonacci::SharedPtr & goal_handle)
  {
    if (!goal_handle) {
      RCLCPP_ERROR(this->get_logger(), "Goal was rejected by server");
    } else {
      RCLCPP_INFO(this->get_logger(), "Goal accepted by server, waiting for result");
    }
  }

  void feedback_callback(
    GoalHandleFibonacci::SharedPtr,
    const std::shared_ptr<const Fibonacci::Feedback> feedback)
  {
    std::stringstream ss;
    ss << "Next number in sequence received: ";
    for (auto number : feedback->partial_sequence) {
      ss << number << " ";
    }
    RCLCPP_INFO(this->get_logger(), ss.str().c_str());
  }

  void result_callback(const GoalHandleFibonacci::WrappedResult & result)
  {
    switch (result.code) {
      case rclcpp_action::ResultCode::SUCCEEDED:
        break;
      case rclcpp_action::ResultCode::ABORTED:
        RCLCPP_ERROR(this->get_logger(), "Goal was aborted");
        return;
      case rclcpp_action::ResultCode::CANCELED:
        RCLCPP_ERROR(this->get_logger(), "Goal was canceled");
        return;
      default:
        RCLCPP_ERROR(this->get_logger(), "Unknown result code");
        return;
    }
    std::stringstream ss;
    ss << "Result received: ";
    for (auto number : result.result->sequence) {
      ss << number << " ";
    }
    RCLCPP_INFO(this->get_logger(), ss.str().c_str());
    rclcpp::shutdown();
  }
};  // class FibonacciActionClient

}  // namespace action_tutorials_cpp

RCLCPP_COMPONENTS_REGISTER_NODE(action_tutorials_cpp::FibonacciActionClient)
```

前几行包括我们需要编译的所有信头.

接下来我们创建一个类 一个衍生的类: `rclcpp::Node`:

``` c++
class FibonacciActionClient : public rclcpp::Node
```

设计器 `FibonacciActionClient` 类初始化节点名称为 `fibonacci_action_client`:

``` c++
  explicit FibonacciActionClient(const rclcpp::NodeOptions & options)
  : Node("fibonacci_action_client", options)
```

构造器还即时切换了一个新的动作客户端:

``` c++
    this->client_ptr_ = rclcpp_action::create_client<Fibonacci>(
      this,
      "fibonacci");
```

动作客户端需要三件事:

1.  模板动作类型名称 : `Fibonacci`.

2.  一个ROS 2节点将动作客户端添加到: `this`.

3.  动作名称 : `'fibonacci'`.

我们还在现场播放一个ROS计时器 它将启动一个,唯一的呼唤 `send_goal`:

``` c++
    this->timer_ = this->create_wall_timer(
      std::chrono::milliseconds(500),
      std::bind(&FibonacciActionClient::send_goal, this));
```

当计时器到期时,它会呼叫 `send_goal`:

``` c++
  void send_goal()
  {
    using namespace std::placeholders;

    this->timer_->cancel();

    if (!this->client_ptr_->wait_for_action_server()) {
      RCLCPP_ERROR(this->get_logger(), "Action server not available after waiting");
      rclcpp::shutdown();
    }

    auto goal_msg = Fibonacci::Goal();
    goal_msg.order = 10;

    RCLCPP_INFO(this->get_logger(), "Sending goal");

    auto send_goal_options = rclcpp_action::Client<Fibonacci>::SendGoalOptions();
    send_goal_options.goal_response_callback =
      std::bind(&FibonacciActionClient::goal_response_callback, this, _1);
    send_goal_options.feedback_callback =
      std::bind(&FibonacciActionClient::feedback_callback, this, _1, _2);
    send_goal_options.result_callback =
      std::bind(&FibonacciActionClient::result_callback, this, _1);
    this->client_ptr_->async_send_goal(goal_msg, send_goal_options);
  }
```

此函数具有以下功能:

1.  取消计时器(所以只调用一次).

2.  等待动作服务器出现.

3.  证明一个新的 `Fibonacci::Goal`.

4.  设置响应、反馈和结果回调。

5.  将目标发送给服务器。

当服务器接收和接受目标时,它会向客户端发送响应。该响应由 `goal_response_callback`:

``` c++
  void goal_response_callback(const GoalHandleFibonacci::SharedPtr & goal_handle)
  {
    if (!goal_handle) {
      RCLCPP_ERROR(this->get_logger(), "Goal was rejected by server");
    } else {
      RCLCPP_INFO(this->get_logger(), "Goal accepted by server, waiting for result");
    }
  }
```

假设目标被服务器接受,它会开始处理。对客户端的任何反馈都会由服务器处理。 `feedback_callback`:

``` c++
  void feedback_callback(
    GoalHandleFibonacci::SharedPtr,
    const std::shared_ptr<const Fibonacci::Feedback> feedback)
  {
    std::stringstream ss;
    ss << "Next number in sequence received: ";
    for (auto number : feedback->partial_sequence) {
      ss << number << " ";
    }
    RCLCPP_INFO(this->get_logger(), ss.str().c_str());
  }
```

当服务器完成处理后,它会返回结果给客户端。结果由处理器处理 `result_callback`:

``` c++
  void result_callback(const GoalHandleFibonacci::WrappedResult & result)
  {
    switch (result.code) {
      case rclcpp_action::ResultCode::SUCCEEDED:
        break;
      case rclcpp_action::ResultCode::ABORTED:
        RCLCPP_ERROR(this->get_logger(), "Goal was aborted");
        return;
      case rclcpp_action::ResultCode::CANCELED:
        RCLCPP_ERROR(this->get_logger(), "Goal was canceled");
        return;
      default:
        RCLCPP_ERROR(this->get_logger(), "Unknown result code");
        return;
    }
    std::stringstream ss;
    ss << "Result received: ";
    for (auto number : result.result->sequence) {
      ss << number << " ";
    }
    RCLCPP_INFO(this->get_logger(), ss.str().c_str());
    rclcpp::shutdown();
  }
};  // class FibonacciActionClient
```

我们现在已经有了一个功能完备的动作客户端。 让我们把它建成并运行起来。

<span id="compiling-the-action-client"></span>

#### 3.2 编译动作客户端

在上一节中,我们设置了动作客户端代码。要编译和运行它,我们需要做一些额外的工作。

首先我们需要设置 CMakeLists.txt , 以便编译动作客户端。 打开 `action_tutorials_cpp/CMakeLists.txt`,然后在 `find_package` 电话:

``` cmake
add_library(action_client SHARED
  src/fibonacci_action_client.cpp)
target_include_directories(action_client PRIVATE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_compile_definitions(action_client
  PRIVATE "ACTION_TUTORIALS_CPP_BUILDING_DLL")
ament_target_dependencies(action_client
  "action_tutorials_interfaces"
  "rclcpp"
  "rclcpp_action"
  "rclcpp_components")
rclcpp_components_register_node(action_client PLUGIN "action_tutorials_cpp::FibonacciActionClient" EXECUTABLE fibonacci_action_client)
install(TARGETS
  action_client
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin)
```

现在我们可以编译软件包了,请到顶层 `ros2_ws`,然后运行 :

``` console
$ colcon build
```

这应当汇编整个工作空间,包括 `fibonacci_action_client` 输入 `action_tutorials_cpp` 软件包。

<span id="running-the-action-client"></span>

#### 3.3 运行动作客户端

现在我们已经构建了动作客户端, 我们可以运行它。 首先要确保一个动作服务器在单独的终端运行。 现在从我们刚刚构建的工作空间中找到源( ) 。`ros2_ws`),并尝试运行动作客户端:

``` console
$ ros2 run action_tutorials_cpp fibonacci_action_client
```

您应该看到已登录的用于目标被接受的信息, 反馈被打印, 以及最终结果 。

<span id="summary"></span>

## 小结

在此教程中,您按行设置了 C++ 动作服务器和动作客户端行,并配置它们以交换目标,反馈和结果.

<span id="related-content"></span>

## 相关内容

- C++ 有几种方法可以写一个动作服务器和客户端; 请检查 `minimal_action_server` 财务报告和财务报告 `minimal_action_client` 软件包中 [ros2/examples](https://github.com/ros2/examples/tree/rolling/rclcpp) 复传.

- 欲了解关于ROS行动的更详细资料,请参见: [设计文章](http://design.ros2.org/articles/actions.html).
