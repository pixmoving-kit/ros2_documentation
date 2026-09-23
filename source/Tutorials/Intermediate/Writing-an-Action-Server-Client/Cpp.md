<span id="writing-an-action-server-and-client-c"></span> <span id="actionscpp"></span>

# 编写动作服务端和客户端（C++）

**目标：** 使用 C++ 实现动作服务端和客户端。

**教程级别：** 中级

**预计耗时：** 15 分钟

<span id="background"></span>

## 背景

动作是 ROS 中的一种异步通信形式。动作客户端向服务端发送目标请求，服务端向客户端返回目标反馈和结果。

<span id="prerequisites"></span>

## 前提条件

需要使用[创建动作](../Creating-an-Action.md)教程中的 `action_tutorials_interfaces` 包及 `Fibonacci.action` 接口。

<span id="tasks"></span>

## 任务

<span id="creating-the-action-tutorials-cpp-package"></span>

### 1 创建 action_tutorials_cpp 软件包

按照[创建 ROS 2 软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md)教程的方法，新建存放 C++ 及配套代码的软件包。

<span id="id1"></span>

#### 1.1 创建 action_tutorials_cpp

进入[上一篇教程](../Creating-an-Action.md)创建的工作空间，并先加载其环境，然后为 C++ 动作服务端创建软件包。

Linux：

```console
$ cd ~/ros2_ws/src
$ ros2 pkg create --dependencies action_tutorials_interfaces rclcpp rclcpp_action rclcpp_components -- action_tutorials_cpp
```

macOS：

```console
$ cd ~/ros2_ws/src
$ ros2 pkg create --dependencies action_tutorials_interfaces rclcpp rclcpp_action rclcpp_components -- action_tutorials_cpp
```

Windows：

```console
$ cd \dev\ros2_ws\src
$ ros2 pkg create --dependencies action_tutorials_interfaces rclcpp rclcpp_action rclcpp_components -- action_tutorials_cpp
```

<span id="adding-in-visibility-control"></span>

#### 1.2 添加可见性控制

为了让软件包能在 Windows 上编译运行，需要添加符号可见性控制。详见 [Windows 技巧中的符号可见性说明](../../../The-ROS2-Project/Contributing/Windows-Tips-and-Tricks.md#windows-symbol-visibility)。

打开 `action_tutorials_cpp/include/action_tutorials_cpp/visibility_control.h`，填入：

```c++
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

### 2 编写动作服务端

使用[创建动作](../Creating-an-Action.md)中定义的接口，编写计算斐波那契数列的服务端。

<span id="writing-the-action-server-code"></span>

#### 2.1 编写服务端代码

将[完整服务端代码](scripts/server.cpp)放入 `action_tutorials_cpp/src/fibonacci_action_server.cpp`。

文件开头引入编译所需的头文件；另见 [rclcpp 便捷头文件说明](../../../_internal/Rclcpp-Convenience-Header-Note.md)。

[代码第 14 行](scripts/server.cpp)定义继承 `rclcpp::Node` 的类；第 21–22 行的 `FibonacciActionServer` 构造函数将节点名设为 `fibonacci_action_server`；第 26–31 行创建动作服务端。

动作服务端需要六项信息：

1. 模板动作类型 `Fibonacci`。
2. 用于承载动作的 ROS 2 节点 `this`。
3. 动作名称 `'fibonacci'`。
4. 处理目标请求的回调 `handle_goal`。
5. 处理取消请求的回调 `handle_cancel`。
6. 处理已接受目标的回调 `handle_accepted`。

随后实现各回调。所有回调都应迅速返回，否则可能使执行器无法处理其他任务。

第 37–44 行的 `handle_goal` 接受所有新目标；第 46–52 行的 `handle_cancel` 通知客户端取消请求已被接受；第 54–59 行的 `handle_accepted` 开始处理新目标。由于执行过程耗时较长，它会启动新线程完成实际工作，以便回调迅速返回。

新线程中所有后续处理与更新都在 `execute` 方法完成，见第 61–95 行。工作线程每秒计算斐波那契数列的一个数，并在每一步发布反馈；完成后将 `goal_handle` 标记为成功，然后退出。

至此，服务端功能已完整，接下来构建并运行。

<span id="compiling-the-action-server"></span>

#### 2.2 编译动作服务端

还需配置 CMake 才能编译运行。打开 `action_tutorials_cpp/CMakeLists.txt`，在 `find_package` 调用之后加入：

```cmake
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

进入 `ros2_ws` 顶层并构建：

```console
$ colcon build
```

这会编译整个工作空间，包括 `action_tutorials_cpp` 中的 `fibonacci_action_server`。

<span id="running-the-action-server"></span>

#### 2.3 运行动作服务端

加载刚构建的 `ros2_ws` 工作空间环境，然后运行：

```console
$ ros2 run action_tutorials_cpp fibonacci_action_server
```

<span id="writing-an-action-client"></span>

### 3 编写动作客户端

<span id="writing-the-action-client-code"></span>

#### 3.1 编写客户端代码

将[完整客户端代码](scripts/client.cpp)放入 `action_tutorials_cpp/src/fibonacci_action_client.cpp`。

开头引入所需头文件，第 15 行定义继承 `rclcpp::Node` 的类。第 20–22 行的构造函数将节点名设为 `fibonacci_action_client`，第 24–26 行创建动作客户端。

动作客户端需要三项信息：

1. 模板动作类型 `Fibonacci`。
2. 用于承载客户端的 ROS 2 节点 `this`。
3. 动作名称 `'fibonacci'`。

第 27–30 行还创建了 ROS 定时器，触发唯一一次 `send_goal` 调用。计时结束后，执行第 32–57 行的 `send_goal`，它会：

1. 取消定时器，保证只调用一次。
2. 等待动作服务端就绪。
3. 创建新的 `Fibonacci::Goal`。
4. 设置响应、反馈和结果回调。
5. 向服务端发送目标。

服务端收到并接受目标后，会向客户端发送响应，由第 62–71 行的 `goal_response_callback` 处理。

如果目标已被接受，服务端开始执行；期间反馈由第 72–83 行的 `feedback_callback` 处理。执行完成后的结果则由第 84–107 行的 `result_callback` 处理。

客户端功能已经完整，接下来构建并运行。

<span id="compiling-the-action-client"></span>

#### 3.2 编译动作客户端

打开 `action_tutorials_cpp/CMakeLists.txt`，在 `find_package` 调用之后加入：

```cmake
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

进入 `ros2_ws` 顶层并构建：

```console
$ colcon build
```

这会编译整个工作空间，包括 `action_tutorials_cpp` 中的 `fibonacci_action_client`。

<span id="running-the-action-client"></span>

#### 3.3 运行动作客户端

先确认动作服务端正在另一终端运行。加载刚构建的 `ros2_ws` 环境后，启动客户端：

```console
$ ros2 run action_tutorials_cpp fibonacci_action_client
```

应看到目标已接受的日志、反馈和最终结果。

<span id="summary"></span>

## 小结

本教程逐步编写了 C++ 动作服务端和客户端，并配置它们交换目标、反馈和结果。

<span id="related-content"></span>

## 相关内容

- C++ 动作服务端和客户端有多种写法，可参阅 [ros2/examples](https://github.com/ros2/examples/tree/rolling/rclcpp) 中的 `minimal_action_server` 和 `minimal_action_client`。
- 动作的更多细节见[设计文章](http://design.ros2.org/articles/actions.html)。
