---
translation_status: machine_translated
source: Tutorials/Demos/Content-Filtering-Subscription.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="creating-a-content-filtering-subscription"></span>

# 创建内容过滤订阅

**目标：** 创建内容过滤订阅 。

**教程级别：** 高级

**用时：** 15分钟

<span id="overview"></span>

## 概述

ROS 2 应用程序通常包括从出版商将数据传送到订阅的话题。 基本上, 订阅商从出版商那里接收所有关于该主题的已发布数据。 但有时, 订阅商可能只对出版商发送的数据的子集感兴趣。 内容过滤订阅只允许接收应用程序感兴趣的数据 。

我们将会强调如何创建内容过滤订阅,

<span id="rmw-support"></span>

## RAMW 支持

内容过滤订阅需要 RMW 执行支持.

<span id="id1"></span>

|                |            |
|----------------|------------|
| rmw_fastrtps   | 已支持     |
| rmw_connextdds | 已支持     |
| rmw_cyclonedds | 不支持( N) |
| rmw_zenoh_cpp  | 不支持( N) |

内容- 编辑- 订阅支持状态 {.docutils .align-default}

目前所有支持内容过滤订阅的 RMW 实施 [DDS](https://www.omg.org/omg-dds-portal/) 。这意味着所支持的过滤表达式和参数也取决于 [DDS](https://www.omg.org/omg-dds-portal/),您可以参考 [DDS 规格](https://www.omg.org/spec/DDS/1.4/PDF) `Annex B - Syntax for Queries and Filters` 详细情况。

<span id="installing-the-demo"></span>

## 安装演示

见 [安装指令](../../Installation.md) 关于安装ROS 2的详情。

如果您已经从软件包中安装了 ROS 2, 请确保您已经安装过 `ros-rolling-demo-nodes-cpp` 已安装。如果从源头下载归档或建立ROS 2,它就已经是安装的一部分。

<span id="temperature-filtering-demo"></span>

## 温度过滤演示

此演示显示内容过滤订阅如何仅用于接收超出可接受温度范围的温度值, 检测紧急情况。 内容过滤订阅过滤出无趣的温度数据, 从而不发布订阅回调 。

内容过滤工具 :

<https://github.com/ros2/demos/blob/rolling/demo_nodes_cpp/src/topics/content_filtering_publisher.cpp>

``` c++
#include <chrono>
#include <cstdio>
#include <memory>
#include <utility>

#include "rclcpp/rclcpp.hpp"
#include "rclcpp_components/register_node_macro.hpp"

#include "std_msgs/msg/float32.hpp"

#include "demo_nodes_cpp/visibility_control.h"

using namespace std::chrono_literals;

namespace demo_nodes_cpp
{
// The simulated temperature data starts from -100.0 and ends at 150.0 with a step size of 10.0
constexpr std::array<float, 3> TEMPERATURE_SETTING {-100.0f, 150.0f, 10.0f};

// Create a ContentFilteringPublisher class that subclasses the generic rclcpp::Node base class.
// The main function below will instantiate the class as a ROS node.
class ContentFilteringPublisher : public rclcpp::Node
{
public:
  DEMO_NODES_CPP_PUBLIC
  explicit ContentFilteringPublisher(const rclcpp::NodeOptions & options)
  : Node("content_filtering_publisher", options)
  {
    // Create a function for when messages are to be sent.
    setvbuf(stdout, NULL, _IONBF, BUFSIZ);
    auto publish_message =
      [this]() -> void
      {
        msg_ = std::make_unique<std_msgs::msg::Float32>();
        msg_->data = temperature_;
        temperature_ += TEMPERATURE_SETTING[2];
        if (temperature_ > TEMPERATURE_SETTING[1]) {
          temperature_ = TEMPERATURE_SETTING[0];
        }
        RCLCPP_INFO(this->get_logger(), "Publishing: '%f'", msg_->data);
        // Put the message into a queue to be processed by the middleware.
        // This call is non-blocking.
        pub_->publish(std::move(msg_));
      };
    // Create a publisher with a custom Quality of Service profile.
    // Uniform initialization is suggested so it can be trivially changed to
    // rclcpp::KeepAll{} if the user wishes.
    // (rclcpp::KeepLast(7) -> rclcpp::KeepAll() fails to compile)
    rclcpp::QoS qos(rclcpp::KeepLast{7});
    pub_ = this->create_publisher<std_msgs::msg::Float32>("temperature", qos);

    // Use a timer to schedule periodic message publishing.
    timer_ = this->create_wall_timer(1s, publish_message);
  }

private:
  float temperature_ = TEMPERATURE_SETTING[0];
  std::unique_ptr<std_msgs::msg::Float32> msg_;
  rclcpp::Publisher<std_msgs::msg::Float32>::SharedPtr pub_;
  rclcpp::TimerBase::SharedPtr timer_;
};

}  // namespace demo_nodes_cpp
```

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

内容过滤器在订阅方被定义, 出版商不需要以任何特殊的方式配置内容过滤器 。 `ContentFilteringPublisher` 节点发布模拟温度数据,从-100.0开始,到15.0结束,步数每秒10.0.

我们可以运行演示 通过运行 `ros2 run demo_nodes_cpp content_filtering_publisher` 可执行文件( 请不要忘记先从设置文件源出) :

``` console
$ ros2 run demo_nodes_cpp content_filtering_publisher
[INFO] [1651094594.822753479] [content_filtering_publisher]: Publishing: '-100.000000'
[INFO] [1651094595.822723857] [content_filtering_publisher]: Publishing: '-90.000000'
[INFO] [1651094596.822752996] [content_filtering_publisher]: Publishing: '-80.000000'
[INFO] [1651094597.822752475] [content_filtering_publisher]: Publishing: '-70.000000'
[INFO] [1651094598.822721485] [content_filtering_publisher]: Publishing: '-60.000000'
[INFO] [1651094599.822696188] [content_filtering_publisher]: Publishing: '-50.000000'
[INFO] [1651094600.822699217] [content_filtering_publisher]: Publishing: '-40.000000'
[INFO] [1651094601.822744113] [content_filtering_publisher]: Publishing: '-30.000000'
[INFO] [1651094602.822694805] [content_filtering_publisher]: Publishing: '-20.000000'
[INFO] [1651094603.822735805] [content_filtering_publisher]: Publishing: '-10.000000'
[INFO] [1651094604.822722094] [content_filtering_publisher]: Publishing: '0.000000'
[INFO] [1651094605.822699960] [content_filtering_publisher]: Publishing: '10.000000'
[INFO] [1651094606.822748946] [content_filtering_publisher]: Publishing: '20.000000'
[INFO] [1651094607.822694017] [content_filtering_publisher]: Publishing: '30.000000'
[INFO] [1651094608.822708798] [content_filtering_publisher]: Publishing: '40.000000'
[INFO] [1651094609.822692417] [content_filtering_publisher]: Publishing: '50.000000'
[INFO] [1651094610.822696426] [content_filtering_publisher]: Publishing: '60.000000'
[INFO] [1651094611.822751913] [content_filtering_publisher]: Publishing: '70.000000'
[INFO] [1651094612.822692231] [content_filtering_publisher]: Publishing: '80.000000'
[INFO] [1651094613.822745549] [content_filtering_publisher]: Publishing: '90.000000'
[INFO] [1651094614.822701982] [content_filtering_publisher]: Publishing: '100.000000'
[INFO] [1651094615.822691465] [content_filtering_publisher]: Publishing: '110.000000'
[INFO] [1651094616.822649070] [content_filtering_publisher]: Publishing: '120.000000'
[INFO] [1651094617.822693616] [content_filtering_publisher]: Publishing: '130.000000'
[INFO] [1651094618.822691832] [content_filtering_publisher]: Publishing: '140.000000'
[INFO] [1651094619.822688452] [content_filtering_publisher]: Publishing: '150.000000'
[INFO] [1651094620.822645327] [content_filtering_publisher]: Publishing: '-100.000000'
[INFO] [1651094621.822689219] [content_filtering_publisher]: Publishing: '-90.000000'
[INFO] [1651094622.822694292] [content_filtering_publisher]: Publishing: '-80.000000'
[...]
```

内容过滤子程序 :

<https://github.com/ros2/demos/blob/rolling/demo_nodes_cpp/src/topics/content_filtering_subscriber.cpp>

``` c++
#include "rclcpp/rclcpp.hpp"
#include "rclcpp_components/register_node_macro.hpp"
#include "rcpputils/join.hpp"

#include "std_msgs/msg/float32.hpp"

#include "demo_nodes_cpp/visibility_control.h"

namespace demo_nodes_cpp
{
// Emergency temperature data less than -30 or greater than 100
constexpr std::array<float, 2> EMERGENCY_TEMPERATURE {-30.0f, 100.0f};

// Create a ContentFilteringSubscriber class that subclasses the generic rclcpp::Node base class.
// The main function below will instantiate the class as a ROS node.
class ContentFilteringSubscriber : public rclcpp::Node
{
public:
  DEMO_NODES_CPP_PUBLIC
  explicit ContentFilteringSubscriber(const rclcpp::NodeOptions & options)
  : Node("content_filtering_subscriber", options)
  {
    setvbuf(stdout, NULL, _IONBF, BUFSIZ);
    // Create a callback function for when messages are received.
    auto callback =
      [this](const std_msgs::msg::Float32 & msg) -> void
      {
        if (msg.data < EMERGENCY_TEMPERATURE[0] || msg.data > EMERGENCY_TEMPERATURE[1]) {
          RCLCPP_INFO(
            this->get_logger(),
            "I receive an emergency temperature data: [%f]", msg.data);
        } else {
          RCLCPP_INFO(this->get_logger(), "I receive a temperature data: [%f]", msg.data);
        }
      };

    // Initialize a subscription with a content filter to receive emergency temperature data that
    // are less than -30 or greater than 100.
    rclcpp::SubscriptionOptions sub_options;
    sub_options.content_filter_options.filter_expression = "data < %0 OR data > %1";
    sub_options.content_filter_options.expression_parameters = {
      std::to_string(EMERGENCY_TEMPERATURE[0]),
      std::to_string(EMERGENCY_TEMPERATURE[1])
    };

    sub_ = create_subscription<std_msgs::msg::Float32>("temperature", 10, callback, sub_options);

    if (!sub_->is_cft_enabled()) {
      RCLCPP_WARN(
        this->get_logger(), "Content filter is not enabled since it's not supported");
    } else {
      RCLCPP_INFO(
        this->get_logger(),
        "subscribed to topic \"%s\" with content filter options \"%s, {%s}\"",
        sub_->get_topic_name(),
        sub_options.content_filter_options.filter_expression.c_str(),
        rcpputils::join(sub_options.content_filter_options.expression_parameters, ", ").c_str());
    }
  }

private:
  rclcpp::Subscription<std_msgs::msg::Float32>::SharedPtr sub_;
};

}  // namespace demo_nodes_cpp
```

为了实现内容过滤,应用程序可以设置过滤表达式和表达式参数。 `SubscriptionOptions`。应用程序还可以检查订阅时是否启用了内容过滤。

在这个演示中, `ContentFilteringSubscriber` 节点创建内容过滤订阅,只有在温度值小于 -30.0 或大于 1000.

如前所述,内容过滤订阅支持取决于 RMW 执行。应用程序可以使用 `is_cft_enabled` 用于检查订阅时是否实际启用内容过滤的方法。

为了测试内容过滤订阅, 让我们运行它:

``` console
$ ros2 run demo_nodes_cpp content_filtering_subscriber
[INFO] [1651094590.682660703] [content_filtering_subscriber]: subscribed to topic "/temperature" with content filter options "data < %0 OR data > %1, {-30.000000, 100.000000}"
[INFO] [1651094594.823805294] [content_filtering_subscriber]: I receive an emergency temperature data: [-100.000000]
[INFO] [1651094595.823419993] [content_filtering_subscriber]: I receive an emergency temperature data: [-90.000000]
[INFO] [1651094596.823410859] [content_filtering_subscriber]: I receive an emergency temperature data: [-80.000000]
[INFO] [1651094597.823350377] [content_filtering_subscriber]: I receive an emergency temperature data: [-70.000000]
[INFO] [1651094598.823282657] [content_filtering_subscriber]: I receive an emergency temperature data: [-60.000000]
[INFO] [1651094599.823297857] [content_filtering_subscriber]: I receive an emergency temperature data: [-50.000000]
[INFO] [1651094600.823355597] [content_filtering_subscriber]: I receive an emergency temperature data: [-40.000000]
[INFO] [1651094615.823315377] [content_filtering_subscriber]: I receive an emergency temperature data: [110.000000]
[INFO] [1651094616.823258458] [content_filtering_subscriber]: I receive an emergency temperature data: [120.000000]
[INFO] [1651094617.823323525] [content_filtering_subscriber]: I receive an emergency temperature data: [130.000000]
[INFO] [1651094618.823315527] [content_filtering_subscriber]: I receive an emergency temperature data: [140.000000]
[INFO] [1651094619.823331424] [content_filtering_subscriber]: I receive an emergency temperature data: [150.000000]
[INFO] [1651094620.823271748] [content_filtering_subscriber]: I receive an emergency temperature data: [-100.000000]
[INFO] [1651094621.823343550] [content_filtering_subscriber]: I receive an emergency temperature data: [-90.000000]
[INFO] [1651094622.823286326] [content_filtering_subscriber]: I receive an emergency temperature data: [-80.000000]
[INFO] [1651094623.823371031] [content_filtering_subscriber]: I receive an emergency temperature data: [-70.000000]
[INFO] [1651094624.823333112] [content_filtering_subscriber]: I receive an emergency temperature data: [-60.000000]
[INFO] [1651094625.823266469] [content_filtering_subscriber]: I receive an emergency temperature data: [-50.000000]
[INFO] [1651094626.823284093] [content_filtering_subscriber]: I receive an emergency temperature data: [-40.000000]
```

您应当看到一个显示所使用内容过滤选项的信件, 并仅在温度值小于 - 30.0 或大于 1000.0 时记录收到的每个信件。

如果内容过滤不为 RMW 执行所支持, 订阅仍然会被创建而不启用内容过滤。 我们可以通过执行来尝试 `RMW_IMPLEMENTATION=rmw_cyclonedds_cpp ros2 run demo_nodes_cpp content_filtering_publisher`.

``` console
$ RMW_IMPLEMENTATION=rmw_cyclonedds_cpp ros2 run demo_nodes_cpp content_filtering_subscriber
[WARN] [1651096637.893842072] [content_filtering_subscriber]: Content filter is not enabled since it is not supported
[INFO] [1651096641.246043703] [content_filtering_subscriber]: I receive an emergency temperature data: [-100.000000]
[INFO] [1651096642.245833527] [content_filtering_subscriber]: I receive an emergency temperature data: [-90.000000]
[INFO] [1651096643.245743471] [content_filtering_subscriber]: I receive an emergency temperature data: [-80.000000]
[INFO] [1651096644.245833932] [content_filtering_subscriber]: I receive an emergency temperature data: [-70.000000]
[INFO] [1651096645.245916679] [content_filtering_subscriber]: I receive an emergency temperature data: [-60.000000]
[INFO] [1651096646.245861895] [content_filtering_subscriber]: I receive an emergency temperature data: [-50.000000]
[INFO] [1651096647.245946352] [content_filtering_subscriber]: I receive an emergency temperature data: [-40.000000]
[INFO] [1651096648.245934569] [content_filtering_subscriber]: I receive a temperature data: [-30.000000]
[INFO] [1651096649.245877906] [content_filtering_subscriber]: I receive a temperature data: [-20.000000]
[INFO] [1651096650.245939068] [content_filtering_subscriber]: I receive a temperature data: [-10.000000]
[INFO] [1651096651.245911450] [content_filtering_subscriber]: I receive a temperature data: [0.000000]
[INFO] [1651096652.245879830] [content_filtering_subscriber]: I receive a temperature data: [10.000000]
[INFO] [1651096653.245858329] [content_filtering_subscriber]: I receive a temperature data: [20.000000]
[INFO] [1651096654.245916370] [content_filtering_subscriber]: I receive a temperature data: [30.000000]
[INFO] [1651096655.245933741] [content_filtering_subscriber]: I receive a temperature data: [40.000000]
[INFO] [1651096656.245833975] [content_filtering_subscriber]: I receive a temperature data: [50.000000]
[INFO] [1651096657.245971483] [content_filtering_subscriber]: I receive a temperature data: [60.000000]
```

你可以看到这个消息 `Content filter is not enabled` 因为基本的 RMW 执行并不支持该特性,但是演示仍然成功地创建了接收所有温度数据的正常订阅.

<span id="related-content"></span>

## 相关内容

- [内容过滤示例](https://github.com/ros2/examples/blob/rolling/rclcpp/topics/minimal_subscriber/content_filtering.cpp) 覆盖所有用于内容过滤订阅的接口。

- [内容过滤设计 PR](https://github.com/ros2/design/pull/282)
