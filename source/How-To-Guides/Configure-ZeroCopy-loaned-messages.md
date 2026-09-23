---
translation_status: machine_translated
source: How-To-Guides/Configure-ZeroCopy-loaned-messages.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="configure-zero-copy-loaned-messages"></span>

# 配置零拷贝借用消息

<span id="overview"></span>

## 概述

ROS 2 借出消息和零复制数据共享是设计通过最小化数据复制来提高性能的机制,在使用借出消息时,RMW 中间软件可以分配和管理消息内存,允许出版商和订阅商直接共享数据缓冲器,这减少了内存分配和数据复制相关的间接费用,导致延迟和吞吐量降低. 0复制数据共享在需要高效传输大量数据的高性能应用中特别有益.

详情见 [借出的信件](https://design.ros2.org/articles/zero_copy.html) 关于出借电文如何运作的详情的条文。

<span id="rmw-support"></span>

## RAMW 支持

借出的信息需要RMW的实施支持.

<span id="id1"></span>

|  |  |  |
|----|----|----|
| RMW 执行 | 支助状况 | 文档 |
| rmw_fastrtps | 已支持 | [启用零复制数据共享](https://github.com/ros2/rmw_fastrtps?tab=readme-ov-file#enable-zero-copy-data-sharing) |
| rmw_connextdds | 不支持( N) | N.A |
| rmw_cyclonedds | 不支持( N) | N.A |

借出的信件支持状态 {.docutils .align-default}

<span id="installing-the-demo"></span>

## 安装演示

见 [安装指令](../Installation.md) 关于安装ROS 2的详情。

如果您已经从软件包中安装了 ROS 2, 请确保您已经安装过 `ros-rolling-demo-nodes-cpp` 已安装。如果从源头下载归档或建立ROS 2,它就已经是安装的一部分。

<span id="using-loaned-messages"></span>

## 使用借出的信件

当基于 RMW 执行支持 时, 默认使用 发布器上的借出信件。 如果 RMW 执行不支持借出信件, 则信件会与发布器提供的分配实例分配 。 [朗读器\_ 借出\_ 消息类示例](https://github.com/ros2/demos/blob/rolling/demo_nodes_cpp/src/topics/talker_loaned_message.cpp) 演示如何创建ROS 2发布器,使用借出的信息高效发布数据而不复制信息数据.

``` c++
#include <chrono>
#include <cstdio>
#include <memory>
#include <utility>

#include "rclcpp/rclcpp.hpp"
#include "rclcpp_components/register_node_macro.hpp"

#include "std_msgs/msg/float64.hpp"
#include "std_msgs/msg/string.hpp"

#include "demo_nodes_cpp/visibility_control.h"

using namespace std::chrono_literals;

namespace demo_nodes_cpp
{
// Create a Talker class that subclasses the generic rclcpp::Node base class.
// The main function below will instantiate the class as a ROS node.
class LoanedMessageTalker : public rclcpp::Node
{
public:
  DEMO_NODES_CPP_PUBLIC
  explicit LoanedMessageTalker(const rclcpp::NodeOptions & options)
  : Node("loaned_message_talker", options)
  {
    // Create a function for when messages are to be sent.
    setvbuf(stdout, NULL, _IONBF, BUFSIZ);

    // We differentiate in this demo between two fundamental message types - POD and non-POD
    // PODs are plain old data types, meaning all the data of its type is encapsulated within
    // the structure and does not require any heap allocation or dynamic resizing.
    // non-PODs are essentially the opposite where the data size changes during runtime.
    // All containers (including Strings) are such non-PODs.
    // Most middlewares won't be able to loan non-POD datatypes.
    // We thus feature two publishers in this demo where both, a POD and non-POD message
    // will be used to publish data.
    // The take-away for this is that the rclcpp API for message loaning can cope with
    // either POD and non-POD transparently.
    auto publish_message =
      [this]() -> void
      {
        // We loan a message here and don't allocate the memory on the stack.
        // For middlewares which support message loaning, this means the middleware
        // completely owns the memory for this message.
        // This enables a zero-copy message transport for middlewares with shared memory
        // capabilities.
        // If the middleware doesn't support this, the loaned message will be allocated
        // with the allocator instance provided by the publisher.
        auto pod_loaned_msg = pod_pub_->borrow_loaned_message();
        auto pod_msg_data = static_cast<double>(count_);
        pod_loaned_msg.get().data = pod_msg_data;
        RCLCPP_INFO(this->get_logger(), "Publishing: '%f'", pod_msg_data);
        // As the middleware might own the memory allocated for this message,
        // a call to publish explicitly transfers ownership back to the middleware.
        // The loaned message instance is thus no longer valid after a call to publish.
        pod_pub_->publish(std::move(pod_loaned_msg));

        // Similar as in the above case, we ask the middleware to loan a message.
        // As most likely the middleware won't be able to loan a message for a non-POD
        // data type, the memory for the message will be allocated on the heap within
        // the scope of the `LoanedMessage` instance.
        // After the call to `publish()`, the message will be correctly allocated.
        auto non_pod_loaned_msg = non_pod_pub_->borrow_loaned_message();
        auto non_pod_msg_data = "Hello World: " + std::to_string(count_);
        non_pod_loaned_msg.get().data = non_pod_msg_data;
        RCLCPP_INFO(this->get_logger(), "Publishing: '%s'", non_pod_msg_data.c_str());
        non_pod_pub_->publish(std::move(non_pod_loaned_msg));
        count_++;
      };

    // Create a publisher with a custom Quality of Service profile.
    rclcpp::QoS qos(rclcpp::KeepLast(7));
    pod_pub_ = this->create_publisher<std_msgs::msg::Float64>("chatter_pod", qos);
    non_pod_pub_ = this->create_publisher<std_msgs::msg::String>("chatter", qos);

    // Use a timer to schedule periodic message publishing.
    timer_ = this->create_wall_timer(1s, publish_message);
  }

private:
  size_t count_ = 1;
  rclcpp::Publisher<std_msgs::msg::Float64>::SharedPtr pod_pub_;
  rclcpp::Publisher<std_msgs::msg::String>::SharedPtr non_pod_pub_;
  rclcpp::TimerBase::SharedPtr timer_;
};

}  // namespace demo_nodes_cpp
```

这个例子试图从RMW执行中借出两种信息,即: `borrow_loaned_message()`一个是普通的旧数据(POD)消息类型, `std_msgs::msg::Float64`,另一个是非Plain Old Data (POD) 消息类型, `std_msgs::msg::String`。对出借信件的要求是,信件类型是普通的旧数据类型。 [rmw_fastrtps](https://github.com/ros2/rmw_fastrtps) 如下所示。

我们可以运行演示 通过运行 `ros2 run demo_nodes_cpp talker_loaned_message` 可执行文件( 请不要忘记先从设置文件源出) :

``` console
$ ros2 run demo_nodes_cpp talker_loaned_message
[INFO] [1741063656.446278828] [loaned_message_talker]: Publishing: '1.000000'
[INFO] [1741063656.446705580] [rclcpp]: Currently used middleware cannot loan messages. Local allocator will be used.
[INFO] [1741063656.446754794] [loaned_message_talker]: Publishing: 'Hello World: 1'
[INFO] [1741063657.446232119] [loaned_message_talker]: Publishing: '2.000000'
[INFO] [1741063657.446401820] [loaned_message_talker]: Publishing: 'Hello World: 2'
[INFO] [1741063658.446217220] [loaned_message_talker]: Publishing: '3.000000'
[INFO] [1741063658.446383011] [loaned_message_talker]: Publishing: 'Hello World: 3'
[...]
```

如果 RMW 执行不支持借出的消息, 所有的消息都会与出版商提供的分配器实例分配。 我们可以通过执行来尝试 。 `RMW_IMPLEMENTATION=rmw_cyclonedds_cpp ros2 run demo_nodes_cpp talker_loaned_message`.

``` console
$ RMW_IMPLEMENTATION=rmw_cyclonedds_cpp ros2 run demo_nodes_cpp talker_loaned_message
[INFO] [1741064109.676860153] [rclcpp]: Currently used middleware cannot loan messages. Local allocator will be used.
[INFO] [1741064109.677043250] [loaned_message_talker]: Publishing: '1.000000'
[INFO] [1741064109.677185724] [rclcpp]: Currently used middleware cannot loan messages. Local allocator will be used.
[INFO] [1741064109.677224058] [loaned_message_talker]: Publishing: 'Hello World: 1'
[INFO] [1741064110.676842111] [loaned_message_talker]: Publishing: '2.000000'
[INFO] [1741064110.677008774] [loaned_message_talker]: Publishing: 'Hello World: 2'
[INFO] [1741064111.676779850] [loaned_message_talker]: Publishing: '3.000000'
[INFO] [1741064111.676937613] [loaned_message_talker]: Publishing: 'Hello World: 3'
[...]
```

正如我们所见,这两条消息都是成功发布的,但是由于RMW的执行不支持借出的消息,这些消息被分配到由出版商提供的本地分配器实例中.

<span id="how-to-disable-loaned-messages"></span>

## 如何禁用借出的信件

<span id="publishers"></span>

### 出版商

默认, *借出的信件* 如果内存支持, 将尝试从内置中间软件中借用内存 *借出的信件*。该词 `ROS_DISABLE_LOANED_MESSAGES` 环境变量可用于禁用 *借出的信件*,并返回到正常的出版者行为,而无需修改代码或中间软件配置。您可以用以下命令设置环境变量:

##### Linux

``` console
$ export ROS_DISABLE_LOANED_MESSAGES=1
```

要在 shell 会话之间保持此设置, 您可以在您的 shell 启动脚本中添加命令 :

``` console
$ echo "export ROS_DISABLE_LOANED_MESSAGES=1" >> ~/.bashrc
```

##### macOS

``` console
$ export ROS_DISABLE_LOANED_MESSAGES=1
```

要在 shell 会话之间保持此设置, 您可以在您的 shell 启动脚本中添加命令 :

``` console
$ echo "export ROS_DISABLE_LOANED_MESSAGES=1" >> ~/.bash_profile
```

##### Windows

``` console
$ set ROS_DISABLE_LOANED_MESSAGES=1
```

如果您想在 shell 会话间使这个永久化, 也运行 :

``` console
$ setx ROS_DISABLE_LOANED_MESSAGES 1
```

<span id="subscriptions"></span>

### 订阅

当前使用 *借出的信件* 订阅时不安全,请查看更多信息。 [rmw 问题](https://github.com/ros2/rmw_cyclonedds/issues/469) 财务报告和财务报告 [rclpp 问题](https://github.com/ros2/rclcpp/issues/2401)。因此,默认情况下 *借出的信件* 是,这是 `disabled` 在订阅时 [默认设置禁用贷款](https://github.com/ros2/rcl/pull/1110) 即使内置中间软件支持这一点。要启用 *借出的信件* 在订阅时,需要设置环境变量 `ROS_DISABLE_LOANED_MESSAGES` 改为: `0` 明确无误。

##### Linux

``` console
$ export ROS_DISABLE_LOANED_MESSAGES=0
```

要在 shell 会话之间保持此设置, 您可以在您的 shell 启动脚本中添加命令 :

``` console
$ echo "export ROS_DISABLE_LOANED_MESSAGES=0" >> ~/.bashrc
```

##### macOS

``` console
$ export ROS_DISABLE_LOANED_MESSAGES=0
```

要在 shell 会话之间保持此设置, 您可以在您的 shell 启动脚本中添加命令 :

``` console
$ echo "export ROS_DISABLE_LOANED_MESSAGES=0" >> ~/.bash_profile
```

##### Windows

``` console
$ set ROS_DISABLE_LOANED_MESSAGES=0
```

如果您想在 shell 会话间使这个永久化, 也运行 :

``` console
$ setx ROS_DISABLE_LOANED_MESSAGES 0
```
