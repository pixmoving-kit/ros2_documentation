---
translation_status: machine_translated
source: Tutorials/Advanced/Topic-Statistics-Tutorial/Topic-Statistics-Tutorial.rst
---

<span id="enabling-topic-statistics-c"></span>

# 启用话题统计（C++）

**目标：** 启用 ROS 2 主题统计并查看输出统计数据 。

**教程级别：** 高级

**用时：** 10分钟

<span id="background"></span>

## 背景

这是关于如何启用ROS 2中的专题统计并使用命令行工具查看已公布的统计输出的简短教程([ros2 主题](../../Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)).

ROS 2为任何订阅(称为“专题统计”)收到的信息提供了统计的综合计量。如果您可以订阅“专题统计”,您可以描述您的系统的性能,或者利用数据帮助诊断当前任何问题。

详情请参见 [专题统计概念 页次](../../../Concepts/Intermediate/About-Topic-Statistics.md).

<span id="prerequisites"></span>

## 前提条件

一个来自二进制或来源的安装 。

在之前的教程中,你学会了如何 [创建工作空间](../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md), [创建软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md),并创建一个 [C++](../../Beginner-Client-Libraries/Writing-A-Simple-Cpp-Publisher-And-Subscriber.md) 出版社和订户。

这个教程假设你还有你的 `cpp_pubsub` 软件包 [C++](../../Beginner-Client-Libraries/Writing-A-Simple-Cpp-Publisher-And-Subscriber.md) 教学。

<span id="tasks"></span>

## 操作步骤

<span id="write-the-subscriber-node-with-statistics-enabled"></span>

### 1 用启用的统计数据写入用户节点

导航到 `ros2_ws/src/cpp_pubsub/src` 文件夹,创建于 [上一个教程](../../Beginner-Client-Libraries/Writing-A-Simple-Cpp-Publisher-And-Subscriber.md),然后通过输入以下命令来下载示例谈话者代码:

##### Linux

``` console
$ wget -O member_function_with_topic_statistics.cpp https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_subscriber/member_function_with_topic_statistics.cpp
```

##### macOS

``` console
$ wget -O member_function_with_topic_statistics.cpp https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_subscriber/member_function_with_topic_statistics.cpp
```

##### Windows

右键单击此链接并选择另存为 `publisher_member_function.cpp`:

<https://raw.githubusercontent.com/ros2/examples/rolling/rclcpp/topics/minimal_subscriber/member_function_with_topic_statistics.cpp>

现在有一个新的文件命名 `member_function_with_topic_statistics.cpp`。使用您首选的文本编辑器打开文件。

``` C++
#include <chrono>
#include <memory>

#include "rclcpp/rclcpp.hpp"
#include "rclcpp/subscription_options.hpp"

#include "std_msgs/msg/string.hpp"

class MinimalSubscriberWithTopicStatistics : public rclcpp::Node
{
public:
  MinimalSubscriberWithTopicStatistics()
  : Node("minimal_subscriber_with_topic_statistics")
  {
    // manually enable topic statistics via options
    auto options = rclcpp::SubscriptionOptions();
    options.topic_stats_options.state = rclcpp::TopicStatisticsState::Enable;

    // configure the collection window and publish period (default 1s)
    options.topic_stats_options.publish_period = std::chrono::seconds(10);

    // configure the topic name (default '/statistics')
    // options.topic_stats_options.publish_topic = "/topic_statistics"

    auto callback = [this](const std_msgs::msg::String & msg) {
        this->topic_callback(msg);
      };

    subscription_ = this->create_subscription<std_msgs::msg::String>(
      "topic", 10, callback, options);
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
  rclcpp::spin(std::make_shared<MinimalSubscriberWithTopicStatistics>());
  rclcpp::shutdown();
  return 0;
}
```

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

<span id="examine-the-code"></span>

#### 1.1 审查守则

碞钩妓 [C++](../../Beginner-Client-Libraries/Writing-A-Simple-Cpp-Publisher-And-Subscriber.md) 教程,我们有一个用户节点,接收来自 `topic` 专题 `topic_callback` 不过,我们现在增加了一些选项,用于配置订阅,以便让专题统计与 `rclcpp::SubscriptionOptions()` 选项结构。

``` C++
// manually enable topic statistics via options
auto options = rclcpp::SubscriptionOptions();
options.topic_stats_options.state = rclcpp::TopicStatisticsState::Enable;
```

可以选择,统计收集/出版期等领域以及用于发布统计的专题也可以配置.

``` C++
// configure the collection window and publish period (default 1s)
options.topic_stats_options.publish_period = std::chrono::seconds(10);

// configure the topic name (default '/statistics')
// options.topic_stats_options.publish_topic = "/my_topic"
```

下表介绍可配置的田地:

| 订阅配置字段 | 目的 |
|----|----|
| topic_stats_options.state | 启用或禁用主题统计( 默认) `rclcpp::TopicStatisticsState::Disable`) |
| topic_stats_options.publish_period | 收集统计数据和发布统计信息的期限(默认) `1s`) |
| topic_stats_options.publish_topic | 发布统计数据时使用的主题( 默认) `/statistics`) |

<span id="cmakelists-txt"></span>

#### 1.2 CMakeLists.txt

现在打开 `CMakeLists.txt` 文档。

添加可执行文件并命名 `listener_with_topic_statistics` 这样你就可以使用 `ros2 run`:

``` cmake
add_executable(listener_with_topic_statistics src/member_function_with_topic_statistics.cpp)
ament_target_dependencies(listener_with_topic_statistics rclcpp std_msgs)

install(TARGETS
  talker
  listener
  listener_with_topic_statistics
  DESTINATION lib/${PROJECT_NAME})
```

确保保存文件,然后您的 pub/ sub 系统, 并启用主题统计, 应该可以使用 。

<span id="build-and-run"></span>

### 2 构建和运行

建造,看 [构建和运行](../../Beginner-Client-Libraries/Writing-A-Simple-Cpp-Publisher-And-Subscriber.md#cpppubsub-build-and-run) 在酒吧/子辅导栏中。

以启用的统计数据运行用户节点 :

``` console
$ ros2 run cpp_pubsub listener_with_topic_statistics
```

现在运行谈话者节点:

``` console
$ ros2 run cpp_pubsub talker
[INFO] [minimal_publisher]: Publishing: "Hello World: 0"
[INFO] [minimal_publisher]: Publishing: "Hello World: 1"
[INFO] [minimal_publisher]: Publishing: "Hello World: 2"
[INFO] [minimal_publisher]: Publishing: "Hello World: 3"
[INFO] [minimal_publisher]: Publishing: "Hello World: 4"
```

听者会开始向主控台打印消息,从任何信息数起,出版商在那个时候就像这样:

``` console
[INFO] [minimal_subscriber_with_topic_statistics]: I heard: "Hello World: 10"
[INFO] [minimal_subscriber_with_topic_statistics]: I heard: "Hello World: 11"
[INFO] [minimal_subscriber_with_topic_statistics]: I heard: "Hello World: 12"
[INFO] [minimal_subscriber_with_topic_statistics]: I heard: "Hello World: 13"
[INFO] [minimal_subscriber_with_topic_statistics]: I heard: "Hello World: 14"
```

现在订阅者节点正在接收消息,它将定期发布统计信息。我们将在下一节中观察这些信息。

<span id="observe-published-statistic-data"></span>

### 3 观察公布的统计数据

当节点运行时, 打开一个新的终端窗口。 执行以下命令, 这将列出所有当前活动的主题 。

``` console
$ ros2 topic list
/parameter_events
/rosout
/statistics
/topic
```

如果你可以随意更改 `topic_stats_options.publish_topic` 字段,然后您可以看到该名称,而不是 `/statistics`.

您创建的订阅者节点是发布统计数据, 用于此主题 `topic`,转到输出主题 `/statistics`.

我们能用这个来想象 [RQt 语录](../../../Concepts/Intermediate/About-RQt.md)

![](images/topic_stats_rqt.png)

现在,我们可以用以下命令查看针对这一主题公布的统计数据。终端应开始每10秒发布一次统计信息,因为 `topic_stats_options.publish_period` 订阅配置在教程中稍早时可选择更改 :

``` console
$ ros2 topic echo /statistics
---
measurement_source_name: minimal_subscriber_with_topic_statistics
metrics_source: message_age
unit: ms
window_start:
  sec: 1594856666
  nanosec: 931527366
window_stop:
  sec: 1594856676
  nanosec: 930797670
statistics:
- data_type: 1
  data: 0.5522003000000001
- data_type: 3
  data: 0.756992
- data_type: 2
  data: 0.269039
- data_type: 5
  data: 20.0
- data_type: 4
  data: 0.16441001797065166
---
measurement_source_name: minimal_subscriber_with_topic_statistics
metrics_source: message_period
unit: ms
window_start:
  sec: 1594856666
  nanosec: 931527366
window_stop:
  sec: 1594856676
  nanosec: 930797670
statistics:
- data_type: 1
  data: 499.2746365105009
- data_type: 3
  data: 500.0
- data_type: 2
  data: 499.0
- data_type: 5
  data: 619.0
- data_type: 4
  data: 0.4463309283488427
---
```

从 [信件定义](https://github.com/ros2/rcl_interfaces/tree/rolling/statistics_msgs) 编号 `data_types` 现予公布,具体如下:

| 数据_类型值 | 统计     |
|-------------|----------|
| 1           | 平均数   |
| 2           | 最小值   |
| 3           | 最高额   |
| 4           | 标准偏差 |
| 5           | 样本数   |

这里我们可以看到目前可能计算出的两种统计数据。 `std_msgs::msg::String` 已发布消息到 `/topic` 编辑: `minimal_publisher`.

<span id="summary"></span>

## 小结

您创建了一个用户节点, 并启用了主题统计, 该节点公布了来自 [C++](../../Beginner-Client-Libraries/Writing-A-Simple-Cpp-Service-And-Client.md)发布器节点。 您能够编译和运行此节点。 在运行期间, 您能够观察统计数据 。

<span id="related-content"></span>

## 相关内容

如何观察 `message_age` 期限计算请参见 [ROS 2 专题统计演示](https://github.com/ros2/demos/tree/rolling/topic_statistics_demo).
