---
translation_status: machine_translated
source: Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Cpp-Publisher-And-Subscriber.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="writing-a-simple-publisher-and-subscriber-c"></span> <span id="cpppubsub"></span>

# 编写简单的发布者与订阅者（C++）

**目标：** 使用 C++ 创建并运行一个出版商和订阅者节点.

**教程级别：** 入门

**用时：** 20分钟

<span id="background"></span>

## 背景

[节点](../Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md) 。在这个教程中,节点将以字符串消息的形式相互传递信息。 [话题](../Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md). 这里使用的例子是一个简单的“谈话者”和“听众”系统;一个节点公布数据,另一个节点订阅这个专题,以便接收数据。

这些示例中使用的代码可以找到 [这儿](https://github.com/ros2/examples/tree/rolling/rclcpp/topics).

<span id="prerequisites"></span>

## 前提条件

在之前的教程中,你学会了如何 [创建工作空间](Creating-A-Workspace/Creating-A-Workspace.md) 财务报告和财务报告 [创建软件包](Creating-Your-First-ROS2-Package.md).

<span id="tasks"></span>

## 操作步骤

<span id="create-a-package"></span>

### 1 创建软件包

打开一个新的终端 [源代码 ROS 2 安装](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md) 这样一来 `ros2` 命令会起作用的。

导航到 `ros2_ws` 在 a 中创建目录 [上一个教程](Creating-A-Workspace/Creating-A-Workspace.md#new-directory).

回顾 应在 `src` 目录,不是工作空间的根。所以,导航到 `ros2_ws/src`,并运行软件包创建命令:

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 cpp_pubsub
```

您的终端将返回一个消息, 以验证您的软件包的创建 `cpp_pubsub` 以及所有必要的文件和文件夹。

导航进入 `ros2_ws/src/cpp_pubsub/src`。回顾,这是包含可执行文件的源文件所属的 CMake 软件包中的目录。

<span id="write-the-publisher-node"></span>

### 2 写入出版商节点

输入以下命令, 下载“ 举例谈话者” 代码 :

##### Linux

``` console
$ wget -O publisher_member_function.cpp https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_publisher/member_function.cpp
```

##### macOS

``` console
$ wget -O publisher_member_function.cpp https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_publisher/member_function.cpp
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ curl -sk https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_publisher/member_function.cpp -o publisher_member_function.cpp
```

或于权壳中:

``` console
$ curl https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_publisher/member_function.cpp -o publisher_member_function.cpp
```

现在有一个新的文件命名 `publisher_member_function.cpp`。使用您首选的文本编辑器打开文件。

``` C++
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

#### 2.1 审查守则

代码的顶部包括您将使用的标准 C++ 头。 在标准 C++ 头后面是 `rclcpp/rclcpp.hpp` 包含允许您使用 ROS 2 系统中最常见的部件。 `std_msgs/msg/string.hpp`,包括您将用来发布数据的内置消息类型。

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

``` C++
#include <chrono>
#include <functional>
#include <memory>
#include <string>

#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/string.hpp"

using namespace std::chrono_literals;
```

这些线条代表了节点的附属关系。 提醒注意, 附属关系必须添加到 `package.xml` 财务报告和财务报告 `CMakeLists.txt`,您将在下一节中这样做。

下一行创建节点类 `MinimalPublisher` 继承 `rclcpp::Node`。每个 `this` 代码中是指节点。

``` C++
class MinimalPublisher : public rclcpp::Node
```

公共构造器命名节点 `minimal_publisher` 并初始化 `count_` 至 0. 在构造器内, 出版商与 `String` 消息类型, 主题名称 `topic`,并需要队列大小,以在备份时限制消息。下一步, `timer_` 初始化,从而导致 `timer_callback` 函数将每秒执行两次。

``` C++
public:
  MinimalPublisher()
  : Node("minimal_publisher"), count_(0)
  {
    publisher_ = this->create_publisher<std_msgs::msg::String>("topic", 10);
    timer_ = this->create_wall_timer(
    500ms, std::bind(&MinimalPublisher::timer_callback, this));
  }
```

那个... `timer_callback` 函数是信件数据设置和信件实际发布的地方。 `RCLCPP_INFO` 宏确保每个已发布的消息都打印到控制台上。

``` C++
private:
  void timer_callback()
  {
    auto message = std_msgs::msg::String();
    message.data = "Hello, world! " + std::to_string(count_++);
    RCLCPP_INFO(this->get_logger(), "Publishing: '%s'", message.data.c_str());
    publisher_->publish(message);
  }
```

最后是定时器,发布器,以及计数字段的宣告.

``` C++
rclcpp::TimerBase::SharedPtr timer_;
rclcpp::Publisher<std_msgs::msg::String>::SharedPtr publisher_;
size_t count_;
```

紧接着 `MinimalPublisher` 类是 `main`,节点实际执行的地方。 `rclcpp::init` 初始化ROS 2,并 `rclcpp::spin` 开始处理来自节点的数据,包括来自定时器的回调。

``` C++
int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<MinimalPublisher>());
  rclcpp::shutdown();
  return 0;
}
```

<span id="add-dependencies"></span>

#### 2.2 增加依附关系

导航一个关卡返回 `ros2_ws/src/cpp_pubsub` 目录,其中 `CMakeLists.txt` 财务报告和财务报告 `package.xml` 已经为您创建文件 。

打开 `package.xml` 与您的文本编辑器。

如本报告所述, [上一个教程](Creating-Your-First-ROS2-Package.md)中,确保填写 `<description>`, `<maintainer>` 财务报告和财务报告 `<license>` 标签 :

``` xml
<description>Examples of minimal publisher/subscriber using rclcpp</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

在后添加新行 `ament_cmake` 构建工具依赖性并粘贴与您节点对应的下列依赖性包括语句:

``` xml
<depend>rclcpp</depend>
<depend>std_msgs</depend>
```

此声明软件包需要 `rclcpp` 财务报告和财务报告 `std_msgs` 当它的代码被构建和执行时。

确保保存文件 。

<span id="cmakelists-txt"></span>

#### 2.3 CMakeLists.txt (中文(简体) ).

现在打开 `CMakeLists.txt` 文件。在现有依赖性下方 `find_package(ament_cmake REQUIRED)`,添加行号:

``` cmake
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
```

之后添加可执行文件并命名 `talker` 这样你就可以使用 `ros2 run`:

``` cmake
add_executable(talker src/publisher_member_function.cpp)
ament_target_dependencies(talker rclcpp std_msgs)
```

最后,添加: `install(TARGETS...)` 第 15 条 `ros2 run` 能找到您的可执行文件 :

``` cmake
install(TARGETS
  talker
  DESTINATION lib/${PROJECT_NAME})
```

你可以帮你清理干净 `CMakeLists.txt` 删掉一些不必要的章节和评论,所以看起来是这样:

``` cmake
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

您现在可以构建您的软件包, 源代码本地设置文件, 并运行它, 但让我们先创建用户节点, 这样您就可以在工作时看到完整的系统 。

<span id="write-the-subscriber-node"></span>

### 3 写入订阅者节点

返回到 `ros2_ws/src/cpp_pubsub/src` 创建下一个节点。在终端中输入以下代码:

##### Linux

``` console
$ wget -O subscriber_member_function.cpp https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_subscriber/member_function.cpp
```

##### macOS

``` console
$ wget -O subscriber_member_function.cpp https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_subscriber/member_function.cpp
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ curl -sk https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_subscriber/member_function.cpp -o subscriber_member_function.cpp
```

或于权壳中:

``` console
$ curl https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_subscriber/member_function.cpp -o subscriber_member_function.cpp
```

检查以确保这些文件存在 :

``` console
publisher_member_function.cpp  subscriber_member_function.cpp
```

打开 `subscriber_member_function.cpp` 与您的文本编辑器。

``` C++
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

#### 3.1 审查守则

订阅者节点的代码与出版商的几乎相同。现在节点被命名为 `minimal_subscriber`,而构造器使用节点 `create_subscription` 类以执行回调。

没有定时器,因为用户只需在数据发布时回复 `topic` 主题。

``` C++
public:
  MinimalSubscriber()
  : Node("minimal_subscriber")
  {
    subscription_ = this->create_subscription<std_msgs::msg::String>(
    "topic", 10, std::bind(&MinimalSubscriber::topic_callback, this, _1));
  }
```

召回从 [主题教程](../Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md) ,出版商和订阅商使用的主题名称和消息类型必须匹配,以允许它们进行通信。

那个... `topic_callback` 函数接收在主题上发布的字符串消息数据,并使用 `RCLCPP_INFO` 宏 。

此类中唯一的字段声明是订阅.

``` C++
private:
  void topic_callback(const std_msgs::msg::String & msg) const
  {
    RCLCPP_INFO(this->get_logger(), "I heard: '%s'", msg.data.c_str());
  }
  rclcpp::Subscription<std_msgs::msg::String>::SharedPtr subscription_;
```

那个... `main` 函数是完全相同的,但现在它旋转 `MinimalSubscriber` 节点。对于出版商节点来说,旋转意味着启动计时器,但是对于订阅者来说,它只是意味着随时准备接收消息。

这个节点与出版商节点的依赖性相同, `package.xml`.

<span id="id2"></span>

#### 3.2 CMakeLists.txt (中文(简体) ).

重新打开 `CMakeLists.txt` ,并在出版商的条目下添加订阅者节点的可执行性和目标。

``` cmake
add_executable(listener src/subscriber_member_function.cpp)
ament_target_dependencies(listener rclcpp std_msgs)

install(TARGETS
  talker
  listener
  DESTINATION lib/${PROJECT_NAME})
```

确保保存文件,然后你的酒吧/子系统应该准备好.

<span id="build-and-run"></span> <span id="cpppubsub-build-and-run"></span>

### 4 构建和运行

你可能已经拥有了 `rclcpp` 财务报告和财务报告 `std_msgs` 作为 ROS 2 系统的一部分安装的软件包。运行是好的做法 `rosdep` 在工作空间的根部(`ros2_ws`在建构前检查缺失的依赖性 :

##### Linux

``` console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

##### macOS

rosdep只运行在Linux上,所以可以提前跳到下一步.

##### Windows

rosdep只运行在Linux上,所以可以提前跳到下一步.

还在你工作空间的根部 `ros2_ws`,构建您的新软件包 :

##### Linux

``` console
$ colcon build --packages-select cpp_pubsub
```

##### macOS

``` console
$ colcon build --packages-select cpp_pubsub
```

##### Windows

``` console
$ colcon build --merge-install --packages-select cpp_pubsub
```

打开新终端, 导航到 `ros2_ws`,并源代码设置文件 :

##### Linux

``` console
$ . install/setup.bash
```

##### macOS

``` console
$ . install/setup.bash
```

##### Windows

``` console
$ call install/setup.bat
```

现在运行谈话者节点。终端应该开始每0.5秒发布一次信息信息,比如:

``` console
$ ros2 run cpp_pubsub talker
[INFO] [minimal_publisher]: Publishing: "Hello World: 0"
[INFO] [minimal_publisher]: Publishing: "Hello World: 1"
[INFO] [minimal_publisher]: Publishing: "Hello World: 2"
[INFO] [minimal_publisher]: Publishing: "Hello World: 3"
[INFO] [minimal_publisher]: Publishing: "Hello World: 4"
```

打开另一个终端, 从内部源出设置文件 `ros2_ws` ,然后启动收听器节点。收听器会开始向控制台打印消息,从发布器当时的任意消息计数开始:

``` console
$ ros2 run cpp_pubsub listener
[INFO] [minimal_subscriber]: I heard: "Hello World: 10"
[INFO] [minimal_subscriber]: I heard: "Hello World: 11"
[INFO] [minimal_subscriber]: I heard: "Hello World: 12"
[INFO] [minimal_subscriber]: I heard: "Hello World: 13"
[INFO] [minimal_subscriber]: I heard: "Hello World: 14"
```

输入 `Ctrl+C` 在每个终端中阻止节点旋转。

<span id="summary"></span>

## 小结

您创建了两个节点来在一个主题上发布和订阅数据。 在编译和运行之前, 您会在软件包配置文件中添加它们的依赖性和可执行文件 。

<span id="next-steps"></span>

## 后续步骤

您接下来会使用服务/客户端模式创建另一个简单的ROS 2 软件包。 您也可以选择将其写入其中之一 [C++](Writing-A-Simple-Cpp-Service-And-Client.md) 或 时 间 [Python](Writing-A-Simple-Py-Service-And-Client.md).

<span id="related-content"></span>

## 相关内容

您可以用 C++ 写出一个出版商和订阅者, 有几种方法; 请检查 `minimal_publisher` 财务报告和财务报告 `minimal_subscriber` 软件包中 [ros2/examples](https://github.com/ros2/examples/tree/rolling/rclcpp/topics) 复传.
