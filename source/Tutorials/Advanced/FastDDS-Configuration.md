---
translation_status: machine_translated
source: Tutorials/Advanced/FastDDS-Configuration.rst
---

<span id="unlocking-the-potential-of-fast-dds-middleware-community-contributed"></span>

# 发挥 Fast DDS 中间件的能力（社区贡献）

**目标：** 此教程将显示如何在ROS 2 中使用 Fast DDS 的扩展配置能力.

**教程级别：** 高级

**用时：** 20分钟

<span id="background"></span>

## 背景

ROS 2 堆栈和 *Fast DDS* 由ROS 2 中间软件执行提供 [rmw_fastrtps](https://github.com/ros2/rmw_fastrtps)这套执行办法可以从二进制和来源获得所有ROS 2的分发。

ROS 2 RMW 仅允许配置某些中间软件 QoS(参见 [ROS 2 QoS政策](../../Concepts/Intermediate/About-Quality-of-Service-Settings.md)然而, `rmw_fastrtps` 提供扩展配置能力,以充分利用 *Fast DDS*。此教程将引导您通过一系列实例来解释如何使用 XML 文件解锁此扩展配置 。

为了获得更多关于使用的信息 *Fast DDS* 二号机,请检查一下 [随函附上文件](https://fast-dds.docs.eprosima.com/en/latest/fastdds/ros2/ros2.html).

<span id="prerequisites"></span>

## 前提条件

这个教程假设你知道如何 [创建软件包](../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md)。它也假设你知道如何写一个 [简单的出版商和订阅商](../Beginner-Client-Libraries/Writing-A-Simple-Cpp-Publisher-And-Subscriber.md) 备注a [简单服务和客户端](../Beginner-Client-Libraries/Writing-A-Simple-Cpp-Service-And-Client.md)。虽然实例在 C++ 中执行,但同样的概念适用于 Python 软件包。

<span id="mixing-synchronous-and-asynchronous-publications-in-the-same-node"></span>

## 在同一节点混合同步和同步出版物

在这个第一个例子中,将创建一个由两个出版商组成的节点,其中一个是同步出版模式,另一个是同步出版模式.

`rmw_fastrtps` 默认情况下使用同步发布模式。

在同步发布模式下,数据直接在用户线程的上下文中发送。这意味着在写入操作中发生的任何阻断调用都会阻断用户线程,从而阻止应用程序继续运行。然而,这种模式通常在较低延迟时产生更高的吞吐率,因为线程之间没有通知或上下文切换。

另一方面,采用同步发布模式,发布者每次引用写入操作,数据都会被复制成队列,一个背景线程(同步线程)被通知加入队列,在数据实际发送之前,对线程的控制会返回给用户,背景线程负责消耗队列,并将数据发送给每个匹配的读者.

<span id="create-the-node-with-the-publishers"></span>

### 与出版商创建节点

首先创建新软件包 `sync_async_node_example_cpp` 新建工作空间:

##### Linux

``` console
$ mkdir -p ~/ros2_ws/src
$ cd ~/ros2_ws/src
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 --dependencies rclcpp std_msgs -- sync_async_node_example_cpp
```

##### macOS

``` console
$ mkdir -p ~/ros2_ws/src
$ cd ~/ros2_ws/src
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 --dependencies rclcpp std_msgs -- sync_async_node_example_cpp
```

##### Windows

``` console
$ md \ros2_ws\src
$ cd \ros2_ws\src
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 --dependencies rclcpp std_msgs -- sync_async_node_example_cpp
```

然后,添加一个名为 `src/sync_async_writer.cpp` 到软件包中,包含以下内容。请注意,同步出版商将针对专题出版 `sync_topic`,而同步的将发布关于主题 `async_topic`.

``` C++
#include <chrono>
#include <functional>
#include <memory>
#include <string>

#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/string.hpp"

using namespace std::chrono_literals;

class SyncAsyncPublisher : public rclcpp::Node
{
public:
    SyncAsyncPublisher()
        : Node("sync_async_publisher"), count_(0)
    {
        // Create the synchronous publisher on topic 'sync_topic'
        sync_publisher_ = this->create_publisher<std_msgs::msg::String>("sync_topic", 10);

        // Create the asynchronous publisher on topic 'async_topic'
        async_publisher_ = this->create_publisher<std_msgs::msg::String>("async_topic", 10);

        // This timer will trigger the publication of new data every half a second
        timer_ = this->create_wall_timer(
                500ms, std::bind(&SyncAsyncPublisher::timer_callback, this));
    }

private:
    /**
     * Actions to run every time the timer expires
     */
    void timer_callback()
    {
        // Create a new message to be sent
        auto sync_message = std_msgs::msg::String();
        sync_message.data = "SYNC: Hello, world! " + std::to_string(count_);

        // Log the message to the console to show progress
        RCLCPP_INFO(this->get_logger(), "Synchronously publishing: '%s'", sync_message.data.c_str());

        // Publish the message using the synchronous publisher
        sync_publisher_->publish(sync_message);

        // Create a new message to be sent
        auto async_message = std_msgs::msg::String();
        async_message.data = "ASYNC: Hello, world! " + std::to_string(count_);

        // Log the message to the console to show progress
        RCLCPP_INFO(this->get_logger(), "Asynchronously publishing: '%s'", async_message.data.c_str());

        // Publish the message using the asynchronous publisher
        async_publisher_->publish(async_message);

        // Prepare the count for the next message
        count_++;
    }

    // This timer will trigger the publication of new data every half a second
    rclcpp::TimerBase::SharedPtr timer_;

    // A publisher that publishes asynchronously
    rclcpp::Publisher<std_msgs::msg::String>::SharedPtr async_publisher_;

    // A publisher that publishes synchronously
    rclcpp::Publisher<std_msgs::msg::String>::SharedPtr sync_publisher_;

    // Number of messages sent so far
    size_t count_;
};

int main(int argc, char * argv[])
{
    rclcpp::init(argc, argv);
    rclcpp::spin(std::make_shared<SyncAsyncPublisher>());
    rclcpp::shutdown();
    return 0;
}
```

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

现在打开 `CMakeLists.txt` 文件并添加一个新的可执行文件并命名它 `SyncAsyncWriter` 这样你就可以使用 `ros2 run`:

``` cmake
add_executable(SyncAsyncWriter src/sync_async_writer.cpp)
ament_target_dependencies(SyncAsyncWriter rclcpp std_msgs)
```

最后,添加: `install(TARGETS…)` 第 15 条 `ros2 run` 能找到您的可执行文件 :

``` cmake
install(TARGETS
    SyncAsyncWriter
    DESTINATION lib/${PROJECT_NAME})
```

你可以帮你清理干净 `CMakeLists.txt` 删掉一些不必要的章节和评论,所以看起来是这样:

``` cmake
cmake_minimum_required(VERSION 3.8)
project(sync_async_node_example_cpp)

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

add_executable(SyncAsyncWriter src/sync_async_writer.cpp)
ament_target_dependencies(SyncAsyncWriter rclcpp std_msgs)

install(TARGETS
    SyncAsyncWriter
    DESTINATION lib/${PROJECT_NAME})

ament_package()
```

如果现在这个节点已经建立和运行,两个出版商都会表现相同,在两个话题中同步发布,因为这是默认的出版模式. 默认的出版模式配置可以在节点启动期间运行时更改,使用XML文件.

<span id="create-the-xml-file-with-the-profile-configuration"></span>

### 创建配置配置的 XML 文件

创建一个名称的文件 `SyncAsync.xml` 和以下内容:

``` XML
<?xml version="1.0" encoding="UTF-8" ?>
<profiles xmlns="http://www.eprosima.com/XMLSchemas/fastRTPS_Profiles">

    <!-- default publisher profile -->
    <publisher profile_name="default_publisher" is_default_profile="true">
        <historyMemoryPolicy>DYNAMIC</historyMemoryPolicy>
    </publisher>

    <!-- default subscriber profile -->
    <subscriber profile_name="default_subscriber" is_default_profile="true">
        <historyMemoryPolicy>DYNAMIC</historyMemoryPolicy>
    </subscriber>

    <!-- publisher profile for topic sync_topic -->
    <publisher profile_name="/sync_topic">
        <historyMemoryPolicy>DYNAMIC</historyMemoryPolicy>
        <qos>
            <publishMode>
                <kind>SYNCHRONOUS</kind>
            </publishMode>
        </qos>
    </publisher>

    <!-- publisher profile for topic async_topic -->
    <publisher profile_name="/async_topic">
        <historyMemoryPolicy>DYNAMIC</historyMemoryPolicy>
        <qos>
            <publishMode>
                <kind>ASYNCHRONOUS</kind>
            </publishMode>
        </qos>
    </publisher>

 </profiles>
```

请注意, 已定义了多个发布者和订阅者配置文件。 定义了两个默认配置文件 。 `is_default_profile` 改为: `true`,以及两个名称与先前定义的专题名称相吻合的简介: `sync_topic` 和另一个给 `async_topic`。后两个简介将出版模式设定为 `SYNCHRONOUS` 或 时 间 `ASYNCHRONOUS` 并注意,所有简介都具体规定: `historyMemoryPolicy` 值,是范例发挥作用所需的值,其原因将在此教程的后面解释。

<span id="execute-the-publisher-node"></span>

### 执行出版商节点

要装入 XML , 您需要导出以下环境变量 :

##### Linux

``` console
$ export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
$ export RMW_FASTRTPS_USE_QOS_FROM_XML=1
$ export FASTRTPS_DEFAULT_PROFILES_FILE=path/to/SyncAsync.xml
```

##### macOS

``` console
$ export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
$ export RMW_FASTRTPS_USE_QOS_FROM_XML=1
$ export FASTRTPS_DEFAULT_PROFILES_FILE=path/to/SyncAsync.xml
```

##### Windows

``` console
$ SET RMW_IMPLEMENTATION=rmw_fastrtps_cpp
$ SET RMW_FASTRTPS_USE_QOS_FROM_XML=1
$ SET FASTRTPS_DEFAULT_PROFILES_FILE=path/to/SyncAsync.xml
```

最后,确保您已获取您的设置文件并运行节点 :

``` console
$ source install/setup.bash
$ ros2 run sync_async_node_example_cpp SyncAsyncWriter
[INFO] [1612972049.994630332] [sync_async_publisher]: Synchronously publishing: 'SYNC: Hello, world! 0'
[INFO] [1612972049.995097767] [sync_async_publisher]: Asynchronously publishing: 'ASYNC: Hello, world! 0'
[INFO] [1612972050.494478706] [sync_async_publisher]: Synchronously publishing: 'SYNC: Hello, world! 1'
[INFO] [1612972050.494664334] [sync_async_publisher]: Asynchronously publishing: 'ASYNC: Hello, world! 1'
[INFO] [1612972050.994368474] [sync_async_publisher]: Synchronously publishing: 'SYNC: Hello, world! 2'
[INFO] [1612972050.994549851] [sync_async_publisher]: Asynchronously publishing: 'ASYNC: Hello, world! 2'
```

现在有一个同步出版商和一个同步出版商在同一节点内运行.

<span id="create-a-node-with-the-subscribers"></span>

### 与订阅者创建节点

接下来,与用户新建一个节点,该节点将收听 `sync_topic` 财务报告和财务报告 `async_topic` 正在创建出版物。 在一个新的源文件中, 名为 `src/sync_async_reader.cpp` 写下以下内容:

``` C++
#include <functional>
#include <memory>

#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/string.hpp"

using std::placeholders::_1;

class SyncAsyncSubscriber : public rclcpp::Node
{
public:

    SyncAsyncSubscriber()
        : Node("sync_async_subscriber")
    {
        // Create the synchronous subscriber on topic 'sync_topic'
        // and tie it to the topic_callback
        sync_subscription_ = this->create_subscription<std_msgs::msg::String>(
            "sync_topic", 10, std::bind(&SyncAsyncSubscriber::topic_callback, this, _1));

        // Create the asynchronous subscriber on topic 'async_topic'
        // and tie it to the topic_callback
        async_subscription_ = this->create_subscription<std_msgs::msg::String>(
            "async_topic", 10, std::bind(&SyncAsyncSubscriber::topic_callback, this, _1));
    }

private:

    /**
     * Actions to run every time a new message is received
     */
    void topic_callback(const std_msgs::msg::String & msg) const
    {
        RCLCPP_INFO(this->get_logger(), "I heard: '%s'", msg.data.c_str());
    }

    // A subscriber that listens to topic 'sync_topic'
    rclcpp::Subscription<std_msgs::msg::String>::SharedPtr sync_subscription_;

    // A subscriber that listens to topic 'async_topic'
    rclcpp::Subscription<std_msgs::msg::String>::SharedPtr async_subscription_;
};

int main(int argc, char * argv[])
{
    rclcpp::init(argc, argv);
    rclcpp::spin(std::make_shared<SyncAsyncSubscriber>());
    rclcpp::shutdown();
    return 0;
}
```

打开 `CMakeLists.txt` 文件并添加一个新的可执行文件并命名它 `SyncAsyncReader` 上一个 `SyncAsyncWriter`:

``` cmake
add_executable(SyncAsyncReader src/sync_async_reader.cpp)
ament_target_dependencies(SyncAsyncReader rclcpp std_msgs)

install(TARGETS
    SyncAsyncReader
    DESTINATION lib/${PROJECT_NAME})
```

<span id="execute-the-subscriber-node"></span>

### 执行订阅者节点

随着出版商节点在一个终端运行,打开另一个终端,并导出 XML 装入所需的环境变量 :

##### Linux

``` console
$ export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
$ export RMW_FASTRTPS_USE_QOS_FROM_XML=1
$ export FASTRTPS_DEFAULT_PROFILES_FILE=path/to/SyncAsync.xml
```

##### macOS

``` console
$ export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
$ export RMW_FASTRTPS_USE_QOS_FROM_XML=1
$ export FASTRTPS_DEFAULT_PROFILES_FILE=path/to/SyncAsync.xml
```

##### Windows

``` console
$ SET RMW_IMPLEMENTATION=rmw_fastrtps_cpp
$ SET RMW_FASTRTPS_USE_QOS_FROM_XML=1
$ SET FASTRTPS_DEFAULT_PROFILES_FILE=path/to/SyncAsync.xml
```

最后,确保您已获取您的设置文件并运行节点 :

``` console
$ source install/setup.bash
$ ros2 run sync_async_node_example_cpp SyncAsyncReader
[INFO] [1612972054.495429090] [sync_async_subscriber]: I heard: 'SYNC: Hello, world! 10'
[INFO] [1612972054.995410057] [sync_async_subscriber]: I heard: 'ASYNC: Hello, world! 10'
[INFO] [1612972055.495453494] [sync_async_subscriber]: I heard: 'SYNC: Hello, world! 11'
[INFO] [1612972055.995396561] [sync_async_subscriber]: I heard: 'ASYNC: Hello, world! 11'
[INFO] [1612972056.495534818] [sync_async_subscriber]: I heard: 'SYNC: Hello, world! 12'
[INFO] [1612972056.995473953] [sync_async_subscriber]: I heard: 'ASYNC: Hello, world! 12'
```

<span id="analysis-of-the-example"></span>

### 实例分析

<span id="configuration-profiles-xml"></span>

#### 配置配置配置文件 XML

XML 文件为出版商和订阅者定义了多个配置。 您可以有一个默认的出版商配置配置配置和几个特定主题的出版商配置。 唯一的要求是所有出版商配置都有不同的名称, 只有一个默认配置。 订阅者也是如此 。

为了定义特定主题的配置,只需在ROS 2 主题名称后命名配置( 如 `/sync_topic` 财务报告和财务报告 `/async_topic` )),和(见A/CN.9/WG.III/WP.18号文件。 `rmw_fastrtps` 将将此配置应用于该主题的所有出版商和订阅者。默认配置配置配置由属性识别 `is_default_profile` 设置为 `true`,并在没有其他名称与主题名称相匹配时作为倒置配置。

环境变量 `FASTRTPS_DEFAULT_PROFILES_FILE` 用于通知 *Fast DDS* XML 文件的路径与要加载的配置配置配置文件。

<span id="rmw-fastrtps-use-qos-from-xml"></span>

#### RMW_FASTRTPS_USE_QOS_FROM_XML

在所有的可塑属性中, `rmw_fastrtps` 处理 `publishMode` 财务报告和财务报告 `historyMemoryPolicy` 不同。默认情况下,这些值将被设定为 `ASYNCHRONOUS` 财务报告和财务报告 `PREALLOCATED_WITH_REALLOC` 内部 `rmw_fastrtps` 执行,以及 XML 文件上设定的值会被忽略。为了使用 XML 文件中的值,环境变量 `RMW_FASTRTPS_USE_QOS_FROM_XML` 必须设定为 `1`.

然而,这需要 **另外一个警告**: 如果 `RMW_FASTRTPS_USE_QOS_FROM_XML` 设定,但 XML 文件没有定义 `publishMode` 或 时 间 `historyMemoryPolicy`,这些属性将 *Fast DDS* 默认值而不是 `rmw_fastrtps` 默认值。这很重要,特别是对于 `historyMemoryPolicy`,因为 *Fast DDS* 默认值为 `PREALLOCATED` 因此,在此例子中,已明确设定了此政策的有效值( ROS2 ) 。`DYNAMIC`).

<span id="prioritization-of-rmw-qos-profile-t"></span>

#### rmw_qos\_ profile_t 的优先级

ROS 2 QoS,载于 [rmw_qos_profile_t](http://docs.ros2.org/latest/api/rmw/structrmw__qos__profile__t.html) 总是被尊重,除非被赋予 `*_SYSTEM_DEFAULT`在这种情况下, XML 值(或 *Fast DDS* 在没有 XML 值的情况下, 应用默认值。 这意味着如果在 QoS 中出现任何 QoS 值 `rmw_qos_profile_t` 被设定为非 `*_SYSTEM_DEFAULT`,则忽略 XML 中的相应值。

<span id="using-other-fastdds-capabilities-with-xml"></span>

## 使用其他具有 XML 的 FastDDS 能力

尽管我们创建了一个由两个不同配置的出版商组成的节点,但是要检查它们的行为是否不同并不容易。既然XML配置的基本内容已经覆盖,让我们用它们来配置对节点有视觉效果的东西。具体来说,将设定一个出版商的最大对等用户数量和另一个出版商的分区定义。请注意,这些只是所有配置属性中非常简单的例子,可以调用。 `rmw_fastrtps` 通过 XML 文件。 请参考 [\* 快速DDS\* 文档](https://fast-dds.docs.eprosima.com/en/latest/fastdds/xml_configuration/xml_configuration.html#xml-profiles) 以查看可通过 XML 文件配置的全部属性列表。

<span id="limiting-the-number-of-matching-subscribers"></span>

### 限制匹配的用户数量

将最多匹配的订阅者数添加到 `/async_topic` 出版商简介。 它应该是这样的 :

``` XML
<!-- publisher profile for topic async_topic -->
<publisher profile_name="/async_topic">
    <historyMemoryPolicy>DYNAMIC</historyMemoryPolicy>
    <qos>
        <publishMode>
            <kind>ASYNCHRONOUS</kind>
        </publishMode>
    </qos>
    <matchedSubscribersAllocation>
        <initial>0</initial>
        <maximum>1</maximum>
        <increment>1</increment>
    </matchedSubscribersAllocation>
</publisher>
```

匹配的订户数目限于一个。

现在打开三个终端, 不要忘记源代码设置文件并设置所需的环境变量。 在第一个终端上运行发布器节点, 在另外两个终端上运行订阅器节点。 您应该看到只有第一个订阅器节点收到来自这两个主题的信息。 第二个终端无法完成匹配进程 。 `/async_topic` 因为出版商阻止了它,因为它已经达到了与它相匹配的出版商的极限。 `/sync_topic` 将在第三个终点站收到:

``` console
[INFO] [1613127657.088860890] [sync_async_subscriber]: I heard: 'SYNC: Hello, world! 18'
[INFO] [1613127657.588896594] [sync_async_subscriber]: I heard: 'SYNC: Hello, world! 19'
[INFO] [1613127658.088849401] [sync_async_subscriber]: I heard: 'SYNC: Hello, world! 20'
```

<span id="using-partitions-within-the-topic"></span>

### 使用专题内的分区

分区功能可用于控制哪些出版商和订户在同一主题范围内交换信息.

分区在域名ID引发的物理隔离中引入了逻辑实体隔离级概念。要让出版商与订阅者进行通信,它们必须至少属于一个共同分区。分区代表了域名和专题以外的独立出版商和订阅者的另一个级别。与域名和专题不同的是,一个端点可以同时属于多个分区。要在不同域或专题上共享某些数据,就必须有一个不同的出版商,共享自己的变化历史。然而,一个单一的出版商可以使用单个主题数据变化在不同分区上共享相同的数据样本,从而减少网络超载。

让我们改变 `/sync_topic` 发布器到分区 `part1` 并创建新的 `/sync_topic` 使用分区的订阅者 `part2`。现在,他们的简介应该是这样的:

``` XML
<!-- publisher profile for topic sync_topic -->
<publisher profile_name="/sync_topic">
    <historyMemoryPolicy>DYNAMIC</historyMemoryPolicy>
    <qos>
        <publishMode>
            <kind>SYNCHRONOUS</kind>
        </publishMode>
        <partition>
            <names>
                <name>part1</name>
            </names>
        </partition>
    </qos>
</publisher>

<!-- subscriber profile for topic sync_topic -->
<subscriber profile_name="/sync_topic">
    <historyMemoryPolicy>DYNAMIC</historyMemoryPolicy>
    <qos>
        <partition>
            <names>
                <name>part2</name>
            </names>
        </partition>
    </qos>
</subscriber>
```

打开两个终端。 请不要忘记源代码和设置所需的环境变量。 在第一个终端上运行发布器节点, 在另一个终端上运行订阅器节点。 您应该只看到 `/async_topic` 信件正在到达订阅者。 `/sync_topic` 订阅者没有接收数据,因为它与相应的出版者处于不同的分区.

``` console
[INFO] [1612972054.995410057] [sync_async_subscriber]: I heard: 'ASYNC: Hello, world! 10'
[INFO] [1612972055.995396561] [sync_async_subscriber]: I heard: 'ASYNC: Hello, world! 11'
[INFO] [1612972056.995473953] [sync_async_subscriber]: I heard: 'ASYNC: Hello, world! 12'
```

<span id="configuring-a-service-and-a-client"></span>

## 配置服务和客户端

服务和客户端各有一个出版商和一个订阅商,通过两个不同的题目进行交流。 `ping` 有以下内容:

- 一个服务订阅者正在收听请求 `/rq/ping`.

- 服务出版社发送回复 `/rr/ping`.

- 客户端出版商发送请求 `/rq/ping`.

- 一个客户端用户在收听回复 `/rr/ping`.

尽管您可以使用这些主题名称设置 XML 上的配置配置配置, 但有时您可能希望在节点上对所有服务或客户端应用相同的配置配置。 您不复制所有服务生成的所有主题名称的相同配置, 只需创建一个名为“ 出版商” 和“ 订阅商” 的配置配置配对 `service`。客户端创建一对名为 `client`.

<span id="create-the-nodes-with-the-service-and-client"></span>

### 用服务和客户端创建节点

以服务开始创建节点。 添加一个名为新源文件 `src/ping_service.cpp` 在您的软件包中, 其内容如下:

``` C++
#include <memory>

#include "rclcpp/rclcpp.hpp"
#include "example_interfaces/srv/trigger.hpp"

/**
 * Service action: responds with success=true and prints the request on the console
 */
void ping(const std::shared_ptr<example_interfaces::srv::Trigger::Request> request,
        std::shared_ptr<example_interfaces::srv::Trigger::Response> response)
{
    // The request data is unused
    (void) request;

    // Build the response
    response->success = true;

    // Log to the console
    RCLCPP_INFO(rclcpp::get_logger("ping_server"), "Incoming request");
    RCLCPP_INFO(rclcpp::get_logger("ping_server"), "Sending back response");
}

int main(int argc, char **argv)
{
    rclcpp::init(argc, argv);

    // Create the node and the service
    std::shared_ptr<rclcpp::Node> node = rclcpp::Node::make_shared("ping_server");
    rclcpp::Service<example_interfaces::srv::Trigger>::SharedPtr service =
        node->create_service<example_interfaces::srv::Trigger>("ping", &ping);

    // Log that the service is ready
    RCLCPP_INFO(rclcpp::get_logger("ping_server"), "Ready to serve.");

    // run the node
    rclcpp::spin(node);
    rclcpp::shutdown();
}
```

在名为“ 创建” 的文件中创建客户端 `src/ping_client.cpp` 内容如下:

``` C++
#include <chrono>
#include <memory>

#include "rclcpp/rclcpp.hpp"
#include "example_interfaces/srv/trigger.hpp"

using namespace std::chrono_literals;

int main(int argc, char **argv)
{
    rclcpp::init(argc, argv);

    // Create the node and the client
    std::shared_ptr<rclcpp::Node> node = rclcpp::Node::make_shared("ping_client");
    rclcpp::Client<example_interfaces::srv::Trigger>::SharedPtr client =
        node->create_client<example_interfaces::srv::Trigger>("ping");

    // Create a request
    auto request = std::make_shared<example_interfaces::srv::Trigger::Request>();

    // Wait for the service to be available
    while (!client->wait_for_service(1s)) {
        if (!rclcpp::ok()) {
            RCLCPP_ERROR(rclcpp::get_logger("ping_client"), "Interrupted while waiting for the service. Exiting.");
            return 0;
        }
        RCLCPP_INFO(rclcpp::get_logger("ping_client"), "Service not available, waiting again...");
    }

    // Now that the service is available, send the request
    RCLCPP_INFO(rclcpp::get_logger("ping_client"), "Sending request");
    auto result = client->async_send_request(request);

    // Wait for the result and log it to the console
    if (rclcpp::spin_until_future_complete(node, result) ==
        rclcpp::FutureReturnCode::SUCCESS)
    {
        RCLCPP_INFO(rclcpp::get_logger("ping_client"), "Response received");
    } else {
        RCLCPP_ERROR(rclcpp::get_logger("ping_client"), "Failed to call service ping");
    }

    rclcpp::shutdown();
    return 0;
}
```

打开 `CMakeLists.txt` 文件并添加两个新的可执行文件 `ping_service` 财务报告和财务报告 `ping_client`:

``` cmake
find_package(example_interfaces REQUIRED)

add_executable(ping_service src/ping_service.cpp)
ament_target_dependencies(ping_service example_interfaces rclcpp)

add_executable(ping_client src/ping_client.cpp)
ament_target_dependencies(ping_client example_interfaces rclcpp)

install(TARGETS
    ping_service
    DESTINATION lib/${PROJECT_NAME})

install(TARGETS
    ping_client
    DESTINATION lib/${PROJECT_NAME})
```

最后,构建这个包.

<span id="create-the-xml-profiles-for-the-service-and-client"></span>

### 为服务和客户端创建 XML 配置文件

创建一个名称的文件 `ping.xml` 内容如下:

``` XML
<?xml version="1.0" encoding="UTF-8" ?>
<profiles xmlns="http://www.eprosima.com/XMLSchemas/fastRTPS_Profiles">

    <!-- default publisher profile -->
    <publisher profile_name="default_publisher" is_default_profile="true">
        <historyMemoryPolicy>DYNAMIC</historyMemoryPolicy>
    </publisher>

    <!-- default subscriber profile -->
    <subscriber profile_name="default_subscriber" is_default_profile="true">
        <historyMemoryPolicy>DYNAMIC</historyMemoryPolicy>
    </subscriber>

    <!-- service publisher is SYNC -->
    <publisher profile_name="service">
        <historyMemoryPolicy>DYNAMIC</historyMemoryPolicy>
        <qos>
            <publishMode>
                <kind>SYNCHRONOUS</kind>
            </publishMode>
        </qos>
    </publisher>

    <!-- client publisher is ASYNC -->
    <publisher profile_name="client">
        <historyMemoryPolicy>DYNAMIC</historyMemoryPolicy>
        <qos>
            <publishMode>
                <kind>ASYNCHRONOUS</kind>
            </publishMode>
        </qos>
    </publisher>

</profiles>
```

此配置文件将发布模式设置为 `SYNCHRONOUS` 服务及服务 `ASYNCHRONOUS` 。请注意,我们只是定义服务及客户端的出版商配置,但也可以提供订阅者配置。

<span id="execute-the-nodes"></span>

### 执行节点

打开两个终端并源代码文件。 然后设置要加载的 XML 所需的环境变量 :

##### Linux

``` console
$ export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
$ export RMW_FASTRTPS_USE_QOS_FROM_XML=1
$ export FASTRTPS_DEFAULT_PROFILES_FILE=path/to/ping.xml
```

##### macOS

``` console
$ export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
$ export RMW_FASTRTPS_USE_QOS_FROM_XML=1
$ export FASTRTPS_DEFAULT_PROFILES_FILE=path/to/ping.xml
```

##### Windows

``` console
$ SET RMW_IMPLEMENTATION=rmw_fastrtps_cpp
$ SET RMW_FASTRTPS_USE_QOS_FROM_XML=1
$ SET FASTRTPS_DEFAULT_PROFILES_FILE=path/to/ping.xml
```

在第一个终端运行服务节点。 您应该看到服务等待请求 :

``` console
$ ros2 run sync_async_node_example_cpp ping_service
[INFO] [1612977403.805799037] [ping_server]: Ready to serve.
```

在第二个终端上, 运行客户端节点。 您应该看到发送请求和接收回复的客户端 :

``` console
$ ros2 run sync_async_node_example_cpp ping_client
[INFO] [1612977404.805799037] [ping_client]: Sending request
[INFO] [1612977404.825473835] [ping_client]: Response received
```

同时,服务器控制台的输出已经更新:

``` console
[INFO] [1612977403.805799037] [ping_server]: Ready to serve
[INFO] [1612977404.807314904] [ping_server]: Incoming request
[INFO] [1612977404.836405125] [ping_server]: Sending back response
```
