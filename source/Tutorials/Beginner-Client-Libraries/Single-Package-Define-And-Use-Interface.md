<span id="implementing-custom-interfaces"></span> <span id="singlepkginterface"></span>
# 实现自定义接口

**目标：** 学习在 ROS 2 中实现自定义接口的更多方法。

**教程级别：** 初学者

**预计用时：** 15 分钟

<span id="background"></span>
## 背景

[上一篇教程](Custom-ROS2-Interfaces.md)介绍了如何创建自定义 msg 和 srv 接口。

通常推荐在专用接口软件包中声明接口，但有时在同一个包中声明、生成并使用接口会更方便。

目前接口只能在 CMake 软件包中定义。不过，通过 [ament_cmake_python](https://github.com/ament/ament_cmake/tree/rolling/ament_cmake_python)，CMake 软件包也可以包含 Python 库和节点，因此接口与 Python 节点也可以放在同一个包中。为简化示例，这里使用 CMake 软件包和 C++ 节点。

本教程主要介绍 msg 接口类型，但步骤同样适用于其他接口类型。

<span id="prerequisites"></span>
## 前提条件

开始前，应已掌握[创建自定义接口](Custom-ROS2-Interfaces.md)教程中的基础知识。

需要[安装 ROS 2](../../Installation.md)、准备[工作空间](Creating-A-Workspace/Creating-A-Workspace.md)，并了解[如何创建软件包](Creating-Your-First-ROS2-Package.md)。

与往常一样，不要忘记在每个新终端中[加载 ROS 2 环境](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)。

<span id="tasks"></span>
## 操作步骤

<span id="create-a-package"></span>
### 1 创建软件包

在工作空间的 `src` 目录创建 `more_interfaces`，并在包内创建用于存放 msg 文件的目录：

```console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 more_interfaces
$ mkdir more_interfaces/msg
```

<span id="create-a-msg-file"></span>
### 2 创建 msg 文件

在 `more_interfaces/msg` 中创建 `AddressBook.msg`，粘贴以下内容，定义用于保存个人信息的消息：

```text
uint8 PHONE_TYPE_HOME=0
uint8 PHONE_TYPE_WORK=1
uint8 PHONE_TYPE_MOBILE=2

string first_name
string last_name
string phone_number
uint8 phone_type
```

该消息包含以下字段：

- `first_name`：字符串类型。
- `last_name`：字符串类型。
- `phone_number`：字符串类型。
- `phone_type`：`uint8` 类型，并定义了几个具名常量值。

消息定义中的字段也可以设置默认值。更多自定义方法见[接口概念文档](../../Concepts/Basic/About-Interfaces.md)。

接下来需要确保 msg 文件能被转换为 C++、Python 及其他语言的源代码。

<span id="build-a-msg-file"></span>
#### 2.1 构建 msg 文件

打开 `package.xml`，添加：

```xml
<buildtool_depend>rosidl_default_generators</buildtool_depend>

<exec_depend>rosidl_default_runtime</exec_depend>

<member_of_group>rosidl_interface_packages</member_of_group>
```

构建时需要 `rosidl_default_generators`，运行时只需要 `rosidl_default_runtime`。

打开 `CMakeLists.txt`，依次添加以下配置。

查找根据 msg/srv 文件生成消息代码的软件包：

```cmake
find_package(rosidl_default_generators REQUIRED)
```

声明要生成的消息列表：

```cmake
set(msg_files
  "msg/AddressBook.msg"
)
```

手动列出 `.msg` 文件，可以让 CMake 在添加新消息文件后知道需要重新配置项目。

生成消息：

```cmake
rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
)
```

同时导出消息的运行时依赖：

```cmake
ament_export_dependencies(rosidl_default_runtime)
```

现在已经可以根据 msg 定义生成源文件。暂时跳过编译，稍后在步骤 4 中一起进行。

<span id="use-an-interface-from-the-same-package"></span>
### 3 使用同一软件包中的接口

现在开始编写使用该消息的代码。在 `more_interfaces/src` 中创建 `publish_address_book.cpp`，粘贴：

```c++
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

另见 [rclcpp 便捷头文件说明](../../_internal/Rclcpp-Convenience-Header-Note.md)。

<span id="the-code-explained"></span>
#### 3.1 代码说明

包含由新建的 `AddressBook.msg` 生成的头文件：

```c++
#include "more_interfaces/msg/address_book.hpp"
```

创建节点和 `AddressBook` 发布者：

```c++
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

创建定期发布消息的回调：

```c++
auto publish_msg = [this]() -> void {
```

创建稍后要发布的 `AddressBook` 消息实例：

```c++
auto message = more_interfaces::msg::AddressBook();
```

填写 `AddressBook` 的字段：

```c++
message.first_name = "John";
message.last_name = "Doe";
message.phone_number = "1234567890";
message.phone_type = message.PHONE_TYPE_MOBILE;
```

最后发布消息：

```c++
std::cout << "Publishing Contact\nFirst:" << message.first_name <<
  "  Last:" << message.last_name << std::endl;

this->address_book_publisher_->publish(message);
```

创建周期为 1 秒的定时器，每秒调用一次 `publish_msg`：

```c++
timer_ = this->create_wall_timer(1s, publish_msg);
```

<span id="build-the-publisher"></span>
#### 3.2 构建发布者

在 `CMakeLists.txt` 中为该节点添加构建目标：

```cmake
find_package(rclcpp REQUIRED)

add_executable(publish_address_book src/publish_address_book.cpp)
ament_target_dependencies(publish_address_book rclcpp)

install(TARGETS
    publish_address_book
  DESTINATION lib/${PROJECT_NAME})
```

<span id="link-against-the-interface"></span>
#### 3.3 链接接口

要使用同一软件包中生成的消息，需要以下 CMake 代码：

```cmake
rosidl_get_typesupport_target(cpp_typesupport_target
  ${PROJECT_NAME} rosidl_typesupport_cpp)

target_link_libraries(publish_address_book "${cpp_typesupport_target}")
```

这段配置找到从 `AddressBook.msg` 生成的相关 C++ 代码，并让目标链接它。

使用其他独立构建的软件包中的接口时，不需要这一步。只有在定义接口的软件包内部使用该接口，才需要此配置。

<span id="try-it-out"></span>
### 4 运行测试

返回工作空间根目录，构建软件包。

**Linux**

```console
$ cd ~/ros2_ws
$ colcon build --packages-up-to more_interfaces
```

**macOS**

```console
$ cd ~/ros2_ws
$ colcon build --packages-up-to more_interfaces
```

**Windows**

```console
$ cd /ros2_ws
$ colcon build --merge-install --packages-up-to more_interfaces
```

加载工作空间环境并运行发布者。

**Linux**

```console
$ source install/local_setup.bash
$ ros2 run more_interfaces publish_address_book
```

**macOS**

```console
$ . install/local_setup.bash
$ ros2 run more_interfaces publish_address_book
```

**Windows 命令提示符**

```console
$ call install/local_setup.bat
$ ros2 run more_interfaces publish_address_book
```

**Windows PowerShell**

```console
$ install/local_setup.ps1
$ ros2 run more_interfaces publish_address_book
```

你应当能看到发布者发布自定义消息，其中包含在 `publish_address_book.cpp` 中设置的值。

要确认消息确实发布到 `address_book` 话题，打开另一个终端，加载工作空间并执行 `topic echo`。

**Linux**

```console
$ source install/setup.bash
$ ros2 topic echo /address_book
```

**macOS**

```console
$ . install/setup.bash
$ ros2 topic echo /address_book
```

**Windows 命令提示符**

```console
$ call install/setup.bat
$ ros2 topic echo /address_book
```

**Windows PowerShell**

```console
$ install/setup.ps1
$ ros2 topic echo /address_book
```

本教程不创建订阅者，但可以参考[C++ 发布者和订阅者教程](Writing-A-Simple-Cpp-Publisher-And-Subscriber.md)自行编写，作为练习。

<span id="extra-use-an-existing-interface-definition"></span>
### 5 扩展：使用现有接口定义

新接口定义中可以使用现有接口。例如，假设已有 ROS 2 软件包 `rosidl_tutorials_msgs`，其中包含 `Contact.msg`，其定义与前面创建的 `AddressBook.msg` 完全相同。

这种情况下，可以在与节点同包的 `AddressBook.msg` 中使用另一个包中的 `Contact` 类型，甚至定义为 `Contact` 数组：

```text
rosidl_tutorials_msgs/Contact[] address_book
```

生成这个消息时，需要在 `package.xml` 中声明对 `Contact.msg` 所属软件包 `rosidl_tutorials_msgs` 的依赖：

```xml
<build_depend>rosidl_tutorials_msgs</build_depend>

<exec_depend>rosidl_tutorials_msgs</exec_depend>
```

在 `CMakeLists.txt` 中添加：

```cmake
find_package(rosidl_tutorials_msgs REQUIRED)

rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
  DEPENDENCIES rosidl_tutorials_msgs
)
```

还需要在发布者节点中包含 `Contact.msg` 的头文件，才能向 `address_book` 添加联系人：

```c++
#include "rosidl_tutorials_msgs/msg/contact.hpp"
```

可以将回调改为：

```c++
auto publish_msg = [this]() -> void {
   auto msg = std::make_shared<more_interfaces::msg::AddressBook>();
   {
     rosidl_tutorials_msgs::msg::Contact contact;
     contact.first_name = "John";
     contact.last_name = "Doe";
     contact.phone_number = "1234567890";
     contact.phone_type = contact.PHONE_TYPE_MOBILE;
     msg->address_book.push_back(contact);
   }
   {
     rosidl_tutorials_msgs::msg::Contact contact;
     contact.first_name = "Jane";
     contact.last_name = "Doe";
     contact.phone_number = "4254242424";
     contact.phone_type = contact.PHONE_TYPE_HOME;
     msg->address_book.push_back(contact);
   }

   std::cout << "Publishing address book:" << std::endl;
   for (auto contact : msg->address_book) {
     std::cout << "First:" << contact.first_name << "  Last:" << contact.last_name <<
       std::endl;
   }

   address_book_publisher_->publish(*msg);
 };
```

构建并运行后，就能看到消息按预期发布，并包含上述消息数组。

<span id="summary"></span>
## 小结

本教程尝试了定义接口的不同字段类型，并在使用接口的软件包中直接构建该接口。

你还学习了如何将其他接口用作字段类型，以及为此需要配置的 `package.xml`、`CMakeLists.txt` 和 `#include` 语句。

<span id="next-steps"></span>
## 后续步骤

接下来创建带自定义参数的简单 ROS 2 软件包，学习通过 launch 文件设置参数。可以选择 [C++](Using-Parameters-In-A-Class-CPP.md) 或 [Python](Using-Parameters-In-A-Class-Python.md)。

<span id="related-content"></span>
## 相关内容

ROS 2 接口和 IDL（接口定义语言）的更多设计说明，见[这些设计文章](https://design.ros2.org/#interfaces)。
