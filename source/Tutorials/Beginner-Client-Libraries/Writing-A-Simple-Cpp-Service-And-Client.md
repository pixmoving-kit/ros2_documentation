<span id="writing-a-simple-service-and-client-c"></span> <span id="cppsrvcli"></span>
# 编写简单的服务端和客户端（C++）

**目标：** 使用 C++ 创建并运行服务端和客户端节点。

**教程级别：** 初学者

**预计用时：** 20 分钟

<span id="background"></span>
## 背景

[节点](../Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md)通过[服务](../Beginner-CLI-Tools/Understanding-ROS2-Services/Understanding-ROS2-Services.md)通信时，发送数据请求的节点称为客户端节点，响应请求的节点称为服务端节点。请求和响应的结构由 `.srv` 文件决定。

本例实现简单的整数加法系统：一个节点请求计算两个整数的和，另一个返回结果。

<span id="prerequisites"></span>
## 前提条件

此前教程介绍了[创建工作空间](Creating-A-Workspace/Creating-A-Workspace.md)和[创建软件包](Creating-Your-First-ROS2-Package.md)。

<span id="tasks"></span>
## 操作步骤

<span id="create-a-package"></span>
### 1 创建软件包

打开新终端，[加载 ROS 2 安装环境](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)，使 `ros2` 命令可用。

进入[此前创建](Creating-A-Workspace/Creating-A-Workspace.md#new-directory)的 `ros2_ws`。软件包应放在 `src` 而非根目录，因此进入 `ros2_ws/src` 并创建新软件包：

```console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 cpp_srvcli --dependencies rclcpp example_interfaces
```

终端会确认已创建 `cpp_srvcli` 及其必需文件和目录。

`--dependencies` 自动在 `package.xml` 和 `CMakeLists.txt` 中添加依赖。`example_interfaces` 包含用于定义请求和响应结构的 [.srv 文件](https://github.com/ros2/example_interfaces/blob/rolling/srv/AddTwoInts.srv)：

```bash
int64 a
int64 b
---
int64 sum
```

前两行是请求参数，分隔线下方是响应。

<span id="update-package-xml"></span>
#### 1.1 更新 package.xml

创建时使用了 `--dependencies`，因此无需手动向 `package.xml` 或 `CMakeLists.txt` 添加依赖。但仍需在 `package.xml` 中填写说明、维护者邮箱和姓名，以及许可证：

```xml
<description>C++ client server tutorial</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

<span id="write-the-service-node"></span>
### 2 编写服务端节点

在 `ros2_ws/src/cpp_srvcli/src` 中创建 `add_two_ints_server.cpp`，粘贴以下代码：

```C++
#include "rclcpp/rclcpp.hpp"
#include "example_interfaces/srv/add_two_ints.hpp"

#include <memory>

void add(const std::shared_ptr<example_interfaces::srv::AddTwoInts::Request> request,
          std::shared_ptr<example_interfaces::srv::AddTwoInts::Response>      response)
{
  response->sum = request->a + request->b;
  RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "Incoming request\na: %ld" " b: %ld",
                request->a, request->b);
  RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "sending back response: [%ld]", (long int)response->sum);
}

int main(int argc, char **argv)
{
  rclcpp::init(argc, argv);

  std::shared_ptr<rclcpp::Node> node = rclcpp::Node::make_shared("add_two_ints_server");

  rclcpp::Service<example_interfaces::srv::AddTwoInts>::SharedPtr service =
    node->create_service<example_interfaces::srv::AddTwoInts>("add_two_ints", &add);

  RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "Ready to add two ints.");

  rclcpp::spin(node);
  rclcpp::shutdown();
}
```

另见 [rclcpp 便捷头文件说明](../../_internal/Rclcpp-Convenience-Header-Note.md)。

<span id="examine-the-code"></span>
#### 2.1 分析代码

前两条 `#include` 对应软件包依赖。

`add` 函数将请求中的两个整数相加，把和写入响应，同时用日志向控制台报告状态：

```C++
void add(const std::shared_ptr<example_interfaces::srv::AddTwoInts::Request> request,
         std::shared_ptr<example_interfaces::srv::AddTwoInts::Response>      response)
{
    response->sum = request->a + request->b;
    RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "Incoming request\na: %ld" " b: %ld",
        request->a, request->b);
    RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "sending back response: [%ld]", (long int)response->sum);
}
```

`main` 依次完成以下操作。

初始化 ROS 2 C++ 客户端库：

```C++
rclcpp::init(argc, argv);
```

创建名为 `add_two_ints_server` 的节点：

```C++
std::shared_ptr<rclcpp::Node> node = rclcpp::Node::make_shared("add_two_ints_server");
```

为该节点创建名为 `add_two_ints` 的服务，使用 `&add` 作为回调，并自动在网络中公布服务：

```C++
rclcpp::Service<example_interfaces::srv::AddTwoInts>::SharedPtr service =
node->create_service<example_interfaces::srv::AddTwoInts>("add_two_ints", &add);
```

准备就绪后打印日志：

```C++
RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "Ready to add two ints.");
```

对节点调用 spin，使服务能够处理请求：

```C++
rclcpp::spin(node);
```

<span id="add-executable"></span>
#### 2.2 添加可执行程序

`add_executable` 生成可供 `ros2 run` 运行的可执行程序。在 `CMakeLists.txt` 的依赖配置下方添加以下内容，创建名为 `server` 的可执行程序：

```cmake
add_executable(server src/add_two_ints_server.cpp)
ament_target_dependencies(server rclcpp example_interfaces)
```

为让 `ros2 run` 找到它，在文件末尾的 `ament_package()` 之前添加：

```cmake
install(TARGETS
    server
  DESTINATION lib/${PROJECT_NAME})
```

此时已经可以构建、加载本地环境并运行，不过先创建客户端节点，就能观察完整系统的运行情况。

<span id="write-the-client-node"></span>
### 3 编写客户端节点

在 `ros2_ws/src/cpp_srvcli/src` 中创建 `add_two_ints_client.cpp`，粘贴以下代码：

```C++
#include "rclcpp/rclcpp.hpp"
#include "example_interfaces/srv/add_two_ints.hpp"

#include <chrono>
#include <cstdlib>
#include <memory>

using namespace std::chrono_literals;

int main(int argc, char **argv)
{
  rclcpp::init(argc, argv);

  if (argc != 3) {
      RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "usage: add_two_ints_client X Y");
      return 1;
  }

  std::shared_ptr<rclcpp::Node> node = rclcpp::Node::make_shared("add_two_ints_client");
  rclcpp::Client<example_interfaces::srv::AddTwoInts>::SharedPtr client =
    node->create_client<example_interfaces::srv::AddTwoInts>("add_two_ints");

  auto request = std::make_shared<example_interfaces::srv::AddTwoInts::Request>();
  request->a = atoll(argv[1]);
  request->b = atoll(argv[2]);

  while (!client->wait_for_service(1s)) {
    if (!rclcpp::ok()) {
      RCLCPP_ERROR(rclcpp::get_logger("rclcpp"), "Interrupted while waiting for the service. Exiting.");
      return 0;
    }
    RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "service not available, waiting again...");
  }

  auto result = client->async_send_request(request);
  // Wait for the result.
  if (rclcpp::spin_until_future_complete(node, result) ==
    rclcpp::FutureReturnCode::SUCCESS)
  {
    RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "Sum: %ld", result.get()->sum);
  } else {
    RCLCPP_ERROR(rclcpp::get_logger("rclcpp"), "Failed to call service add_two_ints");
  }

  rclcpp::shutdown();
  return 0;
}
```

<span id="id1"></span>
#### 3.1 分析代码

与服务端相似，以下代码先创建节点，再为该节点创建客户端：

```C++
std::shared_ptr<rclcpp::Node> node = rclcpp::Node::make_shared("add_two_ints_client");
rclcpp::Client<example_interfaces::srv::AddTwoInts>::SharedPtr client =
  node->create_client<example_interfaces::srv::AddTwoInts>("add_two_ints");
```

随后创建请求，其结构由前面介绍的 `.srv` 文件定义：

```C++
auto request = std::make_shared<example_interfaces::srv::AddTwoInts::Request>();
request->a = atoll(argv[1]);
request->b = atoll(argv[2]);
```

`while` 循环每次给客户端 1 秒时间，在网络中查找服务节点。若没有找到，就继续等待：

```C++
RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "service not available, waiting again...");
```

如果客户端被中断，例如在终端按下 `Ctrl+C`，会输出说明中断原因的错误日志：

```C++
RCLCPP_ERROR(rclcpp::get_logger("rclcpp"), "Interrupted while waiting for the service. Exiting.");
```

随后客户端发送请求，节点通过 spin 等待响应，直到收到响应或调用失败。

<span id="id2"></span>
#### 3.2 添加可执行程序

返回 `CMakeLists.txt`，为新节点添加可执行程序和目标配置。删除自动生成文件中不必要的模板内容后，文件应如下所示：

```cmake
cmake_minimum_required(VERSION 3.5)
project(cpp_srvcli)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(example_interfaces REQUIRED)

add_executable(server src/add_two_ints_server.cpp)
ament_target_dependencies(server rclcpp example_interfaces)

add_executable(client src/add_two_ints_client.cpp)
ament_target_dependencies(client rclcpp example_interfaces)

install(TARGETS
  server
  client
  DESTINATION lib/${PROJECT_NAME})

ament_package()
```

<span id="build-and-run"></span>
### 4 构建并运行

推荐构建前在工作空间根目录 `ros2_ws` 运行 `rosdep`，检查缺失依赖。

**Linux**

```console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

**macOS 和 Windows**

本教程的 rosdep 步骤仅适用于 Linux，可跳到下一步。

返回工作空间根目录 `ros2_ws`，构建新软件包。

**Linux**

```console
$ colcon build --packages-select cpp_srvcli
```

**macOS**

```console
$ colcon build --packages-select cpp_srvcli
```

**Windows**

```console
$ colcon build --merge-install --packages-select cpp_srvcli
```

打开新终端，进入 `ros2_ws` 并加载环境设置文件。

**Linux**

```console
$ source install/setup.bash
```

**macOS**

```console
$ . install/setup.bash
```

**Windows**

```console
$ call install/setup.bat
```

运行服务端节点：

```console
$ ros2 run cpp_srvcli server
```

终端应显示以下消息，随后等待请求：

```console
[INFO] [rclcpp]: Ready to add two ints.
```

再打开一个终端，在 `ros2_ws` 中加载环境。启动客户端节点，在命令后跟两个以空格分隔的整数。例如输入 `2` 和 `3`：

```console
$ ros2 run cpp_srvcli client 2 3
[INFO] [rclcpp]: Sum: 5
```

回到服务端终端，可以看到它收到请求时记录的请求数据和返回的响应：

```console
[INFO] [rclcpp]: Incoming request
a: 2 b: 3
[INFO] [rclcpp]: sending back response: [5]
```

在服务端终端按 `Ctrl+C` 停止节点。

<span id="summary"></span>
## 小结

你创建了两个通过服务发送请求和响应数据的节点，将依赖和可执行程序配置加入软件包配置文件，完成构建与运行，并观察了服务端/客户端系统的工作方式。

<span id="next-steps"></span>
## 后续步骤

最近几篇教程使用接口通过话题和服务传递数据。接下来学习[创建自定义接口](Custom-ROS2-Interfaces.md)。

<span id="related-content"></span>
## 相关内容

C++ 服务端和客户端有多种写法，可参阅 [ros2/examples](https://github.com/ros2/examples/tree/rolling/rclcpp/services) 中的 `minimal_service` 和 `minimal_client` 软件包。
