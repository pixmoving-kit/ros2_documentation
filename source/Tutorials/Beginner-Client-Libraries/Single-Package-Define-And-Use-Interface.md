---
translation_status: machine_translated
source: Tutorials/Beginner-Client-Libraries/Single-Package-Define-And-Use-Interface.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="implementing-custom-interfaces"></span> <span id="singlepkginterface"></span>

# 实现自定义接口

**目标：** 在ROS 2中学习更多执行自定义接口的方法.

**教程级别：** 入门

**用时：** 15分钟

<span id="background"></span>

## 背景

在一个 [上一个教程](Custom-ROS2-Interfaces.md),您学会了如何创建自定义的 msg 和 srv 接口.

虽然最佳做法是在专用接口包中声明接口,但有时可以方便地在一个包中全部声明,创建和使用接口.

记得接口目前只能在 CMake 软件包中定义。 但是, 可以在 CMake 软件包中设置 Python 库和节点( 使用) [ament_cmake_python](https://github.com/ament/ament_cmake/tree/rolling/ament_cmake_python)),这样您就可以在一个软件包中一起定义接口和 Python 节点。我们将在这里使用 CMake 软件包和 C++ 节点,以便简单化。

此教程将侧重于 msg 接口类型, 但这里的步骤适用于所有接口类型 。

<span id="prerequisites"></span>

## 前提条件

我们假设你已经检讨了其中的基本内容。 [创建自定义 msg 和 srv 文件](Custom-ROS2-Interfaces.md) 操作此操作前的教程 。

你应该有 [安装了 ROS 2](../../Installation.md), a [工作空间](Creating-A-Workspace/Creating-A-Workspace.md),并理解 [创建软件包](Creating-Your-First-ROS2-Package.md).

与往常一样, [来源 ROS 2](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md) 在您打开的每一个新终端。

<span id="tasks"></span>

## 操作步骤

<span id="create-a-package"></span>

### 1 创建软件包

在你的工作空间 `src` 目录,创建软件包 `more_interfaces` ,并在其中为 msg 文件创建目录 :

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 more_interfaces
$ mkdir more_interfaces/msg
```

<span id="create-a-msg-file"></span>

### 2 创建 msg 文件

内部 `more_interfaces/msg`,创建新文件 `AddressBook.msg`,并粘贴以下代码,以创建一条意在传递个人信息的信息:

``` default
uint8 PHONE_TYPE_HOME=0
uint8 PHONE_TYPE_WORK=1
uint8 PHONE_TYPE_MOBILE=2

string first_name
string last_name
string phone_number
uint8 phone_type
```

这一信息由以下领域组成:

- 第一个名称:类型字符串

- 上一个名称: 类型字符串

- 电话_数字:类型字符串

- 电话_类型: 类型为 uint8, 并定义了多个命名的常数

请注意, 在信件定义中设置字段的默认值是可能的 。 见 [接口](../../Concepts/Basic/About-Interfaces.md) 对于更多您可以自定义界面的方法。

接下来,我们需要确保msg文件被转换成C++,Python等语言的源代码.

<span id="build-a-msg-file"></span>

#### 2.1 构建 msg 文件

打开 `package.xml` 并增加以下几行:

``` xml
<buildtool_depend>rosidl_default_generators</buildtool_depend>

<exec_depend>rosidl_default_runtime</exec_depend>

<member_of_group>rosidl_interface_packages</member_of_group>
```

请注意,在建设时,我们需要 `rosidl_default_generators`,在运行时间,我们只需要 `rosidl_default_runtime`.

打开 `CMakeLists.txt` 并增加以下几行:

查找从 msg/srv 文件生成消息代码的软件包 :

``` cmake
find_package(rosidl_default_generators REQUIRED)
```

声明您要生成的信件列表 :

``` cmake
set(msg_files
  "msg/AddressBook.msg"
)
```

通过手动添加 . msg 文件,我们确保 CMake 在您添加其他 . msg 文件后知道何时需要重新配置项目 。

生成信件 :

``` cmake
rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
)
```

同时确保您导出消息运行时间依赖 :

``` cmake
ament_export_dependencies(rosidl_default_runtime)
```

现在您已经准备好从您的 msg 定义中生成源文件。 我们现在将跳过编译步骤, 因为我们将在第四步中一起完成。

<span id="use-an-interface-from-the-same-package"></span>

### 3 使用同一软件包的接口

现在,我们可以开始写 代码使用这个消息。

内 `more_interfaces/src` 创建名为的文件 `publish_address_book.cpp` 并粘贴以下代码:

``` c++
#include <chrono>
#include <memory>

#include "rclcpp/rclcpp.hpp"
#include "more_interfaces/msg/address_book.hpp"

using namespace std::chrono_literals;

class AddressBookPublisher : public rclcpp::Node
{
public:
  AddressBookPublisher()
  : Node("address_book_publisher")
  {
    address_book_publisher_ =
      this->create_publisher<more_interfaces::msg::AddressBook>("address_book", 10);

    auto publish_msg = [this]() -> void {
        auto message = more_interfaces::msg::AddressBook();

        message.first_name = "John";
        message.last_name = "Doe";
        message.phone_number = "1234567890";
        message.phone_type = message.PHONE_TYPE_MOBILE;

        std::cout << "Publishing Contact\nFirst:" << message.first_name <<
          "  Last:" << message.last_name << std::endl;

        this->address_book_publisher_->publish(message);
      };
    timer_ = this->create_wall_timer(1s, publish_msg);
  }

private:
  rclcpp::Publisher<more_interfaces::msg::AddressBook>::SharedPtr address_book_publisher_;
  rclcpp::TimerBase::SharedPtr timer_;
};


int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<AddressBookPublisher>());
  rclcpp::shutdown();

  return 0;
}
```

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

<span id="the-code-explained"></span>

#### 3.1 说明的代码

包含我们新创建的标题 `AddressBook.msg`.

``` c++
#include "more_interfaces/msg/address_book.hpp"
```

创建一个节点和一个 `AddressBook` 出版社。

``` c++
using namespace std::chrono_literals;

class AddressBookPublisher : public rclcpp::Node
{
public:
  AddressBookPublisher()
  : Node("address_book_publisher")
  {
    address_book_publisher_ =
      this->create_publisher<more_interfaces::msg::AddressBook>("address_book");
```

创建回调以定期发布信件 。

``` c++
auto publish_msg = [this]() -> void {
```

创建一个 `AddressBook` 消息实例,我们稍后将公布。

``` c++
auto message = more_interfaces::msg::AddressBook();
```

分布 `AddressBook` 字段。

``` c++
message.first_name = "John";
message.last_name = "Doe";
message.phone_number = "1234567890";
message.phone_type = message.PHONE_TYPE_MOBILE;
```

最后定期发送信息。

``` c++
std::cout << "Publishing Contact\nFirst:" << message.first_name <<
  "  Last:" << message.last_name << std::endl;

this->address_book_publisher_->publish(message);
```

创建一个第二个计时器来调用我们 `publish_msg` 函数每秒。

``` c++
timer_ = this->create_wall_timer(1s, publish_msg);
```

<span id="build-the-publisher"></span>

#### 3.2 建立出版商

我们需要为这个节点建立一个新的目标 `CMakeLists.txt`:

``` cmake
find_package(rclcpp REQUIRED)

add_executable(publish_address_book src/publish_address_book.cpp)
ament_target_dependencies(publish_address_book rclcpp)

install(TARGETS
    publish_address_book
  DESTINATION lib/${PROJECT_NAME})
```

<span id="link-against-the-interface"></span>

#### 3.3 连接到接口

为了使用在同一软件包中生成的信息,我们需要使用以下的 CMake 代码:

``` cmake
rosidl_get_typesupport_target(cpp_typesupport_target
  ${PROJECT_NAME} rosidl_typesupport_cpp)

target_link_libraries(publish_address_book "${cpp_typesupport_target}")
```

从中找到生成的相关 C++ 代码 `AddressBook.msg` 并允许你的目标 链接到它。

您可能已经注意到, 当使用的界面来自独立构建的另类软件包时, 这个步骤是不必要的 。 只有在您想要使用与定义的接口相同的软件包中的接口时, 才需要这个 CMake 代码 。

<span id="try-it-out"></span>

### 四,试试看吧

返回工作空间的根来构建软件包 :

##### Linux

``` console
$ cd ~/ros2_ws
$ colcon build --packages-up-to more_interfaces
```

##### macOS

``` console
$ cd ~/ros2_ws
$ colcon build --packages-up-to more_interfaces
```

##### Windows

``` console
$ cd /ros2_ws
$ colcon build --merge-install --packages-up-to more_interfaces
```

然后源代码和运行 出版商:

##### Linux

``` console
$ source install/local_setup.bash
$ ros2 run more_interfaces publish_address_book
```

##### macOS

``` console
$ . install/local_setup.bash
$ ros2 run more_interfaces publish_address_book
```

##### Windows

``` console
$ call install/local_setup.bat
$ ros2 run more_interfaces publish_address_book
```

或使用Powershell:

``` console
$ install/local_setup.ps1
$ ros2 run more_interfaces publish_address_book
```

您应该看到该出版社转发您定义的 msg, 包括您在其中设置的值 `publish_address_book.cpp`.

为了确认该消息正在发表于 `address_book` 主题, 打开另一个终端, 源码工作空间, 并调用 `topic echo`:

##### Linux

``` console
$ source install/setup.bash
$ ros2 topic echo /address_book
```

##### macOS

``` console
$ . install/setup.bash
$ ros2 topic echo /address_book
```

##### Windows

``` console
$ call install/setup.bat
$ ros2 topic echo /address_book
```

或使用Powershell:

``` console
$ install/setup.ps1
$ ros2 topic echo /address_book
```

我们不会在这个教程中创建订阅者, 但你可以自己写一个来练习( 使用 ) [编写简单的发布者与订阅者（C++）](Writing-A-Simple-Cpp-Publisher-And-Subscriber.md) 帮助).

<span id="extra-use-an-existing-interface-definition"></span>

### 5 (Extra) 使用现有的接口定义

> **说明**
>
> 您可以在新的界面定义中使用已有的界面定义。 例如, 让我们假设有一个消息命名为 `Contact.msg` 属于一个已命名的 ROS 2 软件包 `rosidl_tutorials_msgs`假设其定义与我们定制的相同 `AddressBook.msg` 界面。
>
> 那样的话,你就可以定义 `AddressBook.msg` (软件包中的接口) *与* 作为类型 `Contact` (a) 接口 *单独* 软件包。您甚至可以定义 `AddressBook.msg` 作为 *数组* 类型 `Contact`,这样的话:
>
> ``` default
> rosidl_tutorials_msgs/Contact[] address_book
> ```
>
> 要生成此信件, 您需要声明依赖 `Contact.msg's` 软件包, `rosidl_tutorials_msgs`时,在 `package.xml`:
>
> ``` xml
> <build_depend>rosidl_tutorials_msgs</build_depend>
>
> <exec_depend>rosidl_tutorials_msgs</exec_depend>
> ```
>
> 和在 `CMakeLists.txt`:
>
> ``` cmake
> find_package(rosidl_tutorials_msgs REQUIRED)
>
> rosidl_generate_interfaces(${PROJECT_NAME}
>   ${msg_files}
>   DEPENDENCIES rosidl_tutorials_msgs
> )
> ```
>
> 您还需要包含以下标题: `Contact.msg` 在您的出版商节点中添加 `contacts` 给您的 `address_book`.
>
> ``` c++
> #include "rosidl_tutorials_msgs/msg/contact.hpp"
> ```
>
> 你可以把回话改成这样的事情:
>
> ``` c++
> auto publish_msg = [this]() -> void {
>    auto msg = std::make_shared<more_interfaces::msg::AddressBook>();
>    {
>      rosidl_tutorials_msgs::msg::Contact contact;
>      contact.first_name = "John";
>      contact.last_name = "Doe";
>      contact.phone_number = "1234567890";
>      contact.phone_type = contact.PHONE_TYPE_MOBILE;
>      msg->address_book.push_back(contact);
>    }
>    {
>      rosidl_tutorials_msgs::msg::Contact contact;
>      contact.first_name = "Jane";
>      contact.last_name = "Doe";
>      contact.phone_number = "4254242424";
>      contact.phone_type = contact.PHONE_TYPE_HOME;
>      msg->address_book.push_back(contact);
>    }
>
>    std::cout << "Publishing address book:" << std::endl;
>    for (auto contact : msg->address_book) {
>      std::cout << "First:" << contact.first_name << "  Last:" << contact.last_name <<
>        std::endl;
>    }
>
>    address_book_publisher_->publish(*msg);
>  };
> ```
>
> 构建和运行这些变化将显示定义为预期的msg,以及上面定义的msg数组.

<span id="summary"></span>

## 小结

在此教程中, 您尝试了不同的字段类型来定义接口, 然后在使用的同一软件包中构建一个接口 。

您还学会了如何使用另一个接口作为字段类型,以及 `package.xml`, `CMakeLists.txt`,以及 `#include` 使用该特性所必需的声明。

<span id="next-steps"></span>

## 后续步骤

接下来您将创建一个简单的 ROS 2 软件包, 并附带一个自定义参数, 您将学习从启动文件中设置。 您也可以选择将其写入其中之一 。 [C++](Using-Parameters-In-A-Class-CPP.md) 或 时 间 [Python](Using-Parameters-In-A-Class-Python.md).

<span id="related-content"></span>

## 相关内容

Τㄇ [若干设计文章](https://design.ros2.org/#interfaces) 在ROS 2接口和IDL(界面定义语言)上.
