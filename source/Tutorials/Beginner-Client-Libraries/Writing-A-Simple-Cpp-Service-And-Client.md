---
translation_status: machine_translated
source: Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Cpp-Service-And-Client.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="writing-a-simple-service-and-client-c"></span> <span id="cppsrvcli"></span>

# 编写简单的服务端与客户端（C++）

**目标：** 使用 C++ 创建并运行服务和客户端节点.

**教程级别：** 入门

**用时：** 20分钟

<span id="background"></span>

## 背景

何时 [节点](../Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md) 使用 [服务](../Beginner-CLI-Tools/Understanding-ROS2-Services/Understanding-ROS2-Services.md),发送数据请求的节点称为客户端节点,响应请求的节点为服务节点。请求和响应的结构由一个 `.srv` 文档。

这里使用的例子是一个简单的整数加法系统;一个节点请求两个整数的总和,另一个响应结果.

<span id="prerequisites"></span>

## 前提条件

在之前的教程中,你学会了如何 [创建工作空间](Creating-A-Workspace/Creating-A-Workspace.md) 财务报告和财务报告 [创建软件包](Creating-Your-First-ROS2-Package.md).

<span id="tasks"></span>

## 操作步骤

<span id="create-a-package"></span>

### 1 创建软件包

打开一个新的终端 [源代码 ROS 2 安装](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md) 这样一来 `ros2` 命令会起作用的。

导航到 `ros2_ws` 在 a 中创建目录 [上一个教程](Creating-A-Workspace/Creating-A-Workspace.md#new-directory).

回顾 应在 `src` 目录,不是工作空间的根。导航到 `ros2_ws/src` 并创建新软件包 :

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 cpp_srvcli --dependencies rclcpp example_interfaces
```

您的终端将返回一个消息, 以验证您的软件包的创建 `cpp_srvcli` 以及所有必要的文件和文件夹。

那个... `--dependencies` 参数将自动添加必要的依赖线到 `package.xml` 财务报告和财务报告 `CMakeLists.txt`. `example_interfaces` 是包含以下内容的软件包 [.srv 文件](https://github.com/ros2/example_interfaces/blob/rolling/srv/AddTwoInts.srv) 您需要组织您的请求和答复:

``` bash
int64 a
int64 b
---
int64 sum
```

前两行是请求的参数,短线以下是响应.

<span id="update-package-xml"></span>

#### 1.1 最新情况 `package.xml`

因为你用了 `--dependencies` 在创建软件包时,您不需要手动添加依赖性到 `package.xml` 或 时 间 `CMakeLists.txt`.

但是,与往常一样,确保添加描述、维护者电子邮件和姓名,并给信息发放许可证。 `package.xml`.

``` xml
<description>C++ client server tutorial</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

<span id="write-the-service-node"></span>

### 2 写入服务节点

内侧 `ros2_ws/src/cpp_srvcli/src` 目录,创建名为新文件 `add_two_ints_server.cpp` 并粘贴下列编码:

``` C++
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

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

<span id="examine-the-code"></span>

#### 2.1 审查守则

头两个 `#include` 语句是您的软件包依赖关系。

那个... `add` 函数从请求中添加两个整数,并给出响应的总和,同时使用日志通知控制台其状态。

``` C++
void add(const std::shared_ptr<example_interfaces::srv::AddTwoInts::Request> request,
         std::shared_ptr<example_interfaces::srv::AddTwoInts::Response>      response)
{
    response->sum = request->a + request->b;
    RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "Incoming request\na: %ld" " b: %ld",
        request->a, request->b);
    RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "sending back response: [%ld]", (long int)response->sum);
}
```

那个... `main` 函数实现下列,逐行:

- 初始化 ROS 2 C++ 客户端库 :

  ``` C++
  rclcpp::init(argc, argv);
  ```

- 创建命名的节点 `add_two_ints_server`:

  ``` C++
  std::shared_ptr<rclcpp::Node> node = rclcpp::Node::make_shared("add_two_ints_server");
  ```

- 创建名为服务 `add_two_ints` 并且自动在网络上发布广告 `&add` 方法 :

  ``` C++
  rclcpp::Service<example_interfaces::srv::AddTwoInts>::SharedPtr service =
  node->create_service<example_interfaces::srv::AddTwoInts>("add_two_ints", &add);
  ```

- 当日志信息准备好时打印它 :

  ``` C++
  RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "Ready to add two ints.");
  ```

- 旋转节点,使服务可用.

  ``` C++
  rclcpp::spin(node);
  ```

<span id="add-executable"></span>

#### 2.2 添加可执行文件

那个... `add_executable` 宏生成可执行文件,您可以使用 `ros2 run`中添加以下代码块: `CMakeLists.txt` 仅低于创建可执行文件的依赖关系 `server`:

``` cmake
add_executable(server src/add_two_ints_server.cpp)
ament_target_dependencies(server rclcpp example_interfaces)
```

这么说吧 `ros2 run` 可以在文件结尾处找到可执行文件, 在文件结尾处添加以下行, 就在 `ament_package()`:

``` cmake
install(TARGETS
    server
  DESTINATION lib/${PROJECT_NAME})
```

您现在可以构建您的软件包, 源代码本地设置文件, 并运行它, 但让我们先创建客户端节点, 这样您就可以看到整个系统在工作之中 。

<span id="write-the-client-node"></span>

### 3 写入客户端节点

内侧 `ros2_ws/src/cpp_srvcli/src` 目录,创建名为新文件 `add_two_ints_client.cpp` 并粘贴下列编码:

``` C++
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

#### 3.1 审查守则

与服务节点类似,以下的代码行创建节点,然后为该节点创建客户端:

``` C++
std::shared_ptr<rclcpp::Node> node = rclcpp::Node::make_shared("add_two_ints_client");
rclcpp::Client<example_interfaces::srv::AddTwoInts>::SharedPtr client =
  node->create_client<example_interfaces::srv::AddTwoInts>("add_two_ints");
```

下一个是创建请求。其结构由 `.srv` 刚才提到的档案。

``` C++
auto request = std::make_shared<example_interfaces::srv::AddTwoInts::Request>();
request->a = atoll(argv[1]);
request->b = atoll(argv[2]);
```

那个... `while` 循环让客户端在网络中搜索服务节点1秒。 如果找不到, 则会继续等待 。

``` C++
RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "service not available, waiting again...");
```

如果客户端被取消( 例如您输入) `Ctrl+C` 输入终端时,它会返回一个错误日志消息,说明它被中断了。

``` C++
RCLCPP_ERROR(rclcpp::get_logger("rclcpp"), "Interrupted while waiting for the service. Exiting.");
```

然后客户端发送其请求,节点旋转直到收到其回复,或者失败.

<span id="id2"></span>

#### 3.2 添加可执行文件

返回到 `CMakeLists.txt` 为新节点添加可执行文件和目标。从自动生成的文件中删除一些不必要的锅炉板后,请 `CMakeLists.txt` 应该是这样的:

``` cmake
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

### 4 构建和运行

运行是好的做法 `rosdep` 在工作空间的根部(`ros2_ws`在建构前检查缺失的依赖性 :

##### Linux

``` console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

##### macOS

rosdep只运行在Linux上,所以可以提前跳到下一步.

##### Windows

rosdep只运行在Linux上,所以可以提前跳到下一步.

导航回你工作空间的根, `ros2_ws`,并构建您的新软件包:

##### Linux

``` console
$ colcon build --packages-select cpp_srvcli
```

##### macOS

``` console
$ colcon build --packages-select cpp_srvcli
```

##### Windows

``` console
$ colcon build --merge-install --packages-select cpp_srvcli
```

打开新终端, 导航到 `ros2_ws`,并源代码设置文件 :

##### Linux

``` console
$ source install/setup.bash
```

##### macOS

``` console
$ . install/setup.bash
```

##### Windows

``` console
$ call install/setup.bat
```

现在运行服务节点:

``` console
$ ros2 run cpp_srvcli server
```

终端应返回以下信息,然后等待:

``` console
[INFO] [rclcpp]: Ready to add two ints.
```

打开另一个终端, 从内部源出设置文件 `ros2_ws` 。启动客户端节点,然后用空格分隔任意两个整数。如果您选择 `2` 财务报告和财务报告 `3`例如,客户会收到这样的回复:

``` console
$ ros2 run cpp_srvcli client 2 3
[INFO] [rclcpp]: Sum: 5
```

返回您的服务节点运行所在的终端。 您会看到它收到请求和数据时发布了日志消息, 以及它发送回的回复 :

``` console
[INFO] [rclcpp]: Incoming request
a: 2 b: 3
[INFO] [rclcpp]: sending back response: [5]
```

输入 `Ctrl+C` 在服务器终端中阻止节点旋转。

<span id="summary"></span>

## 小结

您创建了两个节点来通过一个服务请求和响应数据。 您在软件包配置文件中添加了它们的依赖性和可执行性, 这样您就可以构建和运行它们, 并在工作时看到服务/ 客户端系统 。

<span id="next-steps"></span>

## 后续步骤

在最近几次的辅导中,您一直在使用接口来传递数据,以跨越主题和服务。接下来,您将学习如何 [创建自定义接口](Custom-ROS2-Interfaces.md).

<span id="related-content"></span>

## 相关内容

- C++ 中您可以写一个服务和客户端的几种方法; 请检查 `minimal_service` 财务报告和财务报告 `minimal_client` 软件包中 [ros2/examples](https://github.com/ros2/examples/tree/rolling/rclcpp/services) 复传.
