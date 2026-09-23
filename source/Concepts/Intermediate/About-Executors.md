---
translation_status: machine_translated
source: Concepts/Intermediate/About-Executors.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="executors"></span>

# 执行器

<span id="overview"></span>

## 概述

ROS 2 中的执行管理由执行者处理。 一个执行者使用一个或多个基础操作系统的线程来引用用户、定时器、服务服务器、动作服务器等在来信和事件上的召回。 明确的执行者类( in) [executor.hpp](https://github.com/ros2/rclcpp/blob/rolling/rclcpp/include/rclcpp/executor.hpp) 在 rclpp 中,在 [executors.py](https://github.com/ros2/rclpy/blob/rolling/rclpy/rclpy/executors.py) 以粗略表示,或以粗略表示 [executor.h](https://github.com/ros2/rclc/blob/master/rclc/include/rclc/executor.h) 在rclc)中,比ROS 1中的自旋机制更能提供对执行管理的控制,尽管基本的API非常相似.

下面,我们专注于 C++ 客户端库 *rclcpp*.

<span id="basic-use"></span>

## 基本用途

在最简单的情况下,主线程用于通过调用处理一个节点的来函和事件 `rclcpp::spin(..)` 现将有关事项通知如下:

``` cpp
int main(int argc, char* argv[])
{
   // Some initialization.
   rclcpp::init(argc, argv);
   ...

   // Instantiate a node.
   rclcpp::Node::SharedPtr node = ...

   // Run the executor.
   rclcpp::spin(node);

   // Shutdown and exit.
   ...
   return 0;
}
```

呼唤 `spin(node)` 基本扩展为即时引用单曲执行器,这是最简单的执行器:

``` cpp
rclcpp::executors::SingleThreadedExecutor executor;
executor.add_node(node);
executor.spin();
```

通过援引 `spin()` 中执行器实例中,当前线索开始查询 rcl 和 中间软件层,以获取来信和其他事件,并调用相应的调回功能,直到节点关闭。为了不抵制中间软件的 QoS 设置,来信不会存储在客户端库层的队列中,而是保存在中间软件中,直到它被调回功能处理。 (这对 ROS 1. 是一个关键区别 。) A *等待设定* 用于向执行者通报中间软件层上可用的消息,每个队列有一个二进制标记。 *等待设定* 用于检测计时器过期时。

![](../images/executors_basic_principle.png)

容器过程也使用单图执行器 [组件](About-Composition.md),即所有在没有明确主要功能的情况下创建和执行节点。

<span id="types-of-executors"></span> <span id="typesofexecutors"></span>

## 执行者的类型

目前,rclcpp提供三种执行器类型,来源于一个共享的父类:

``` dot

digraph Flatland {

   Executor -> SingleThreadedExecutor [dir = back, arrowtail = empty];
   Executor -> MultiThreadedExecutor [dir = back, arrowtail = empty];
   Executor -> StaticSingleThreadedExecutor [dir = back, arrowtail = empty];
   Executor  [shape=polygon,sides=4];
   SingleThreadedExecutor  [shape=polygon,sides=4];
   MultiThreadedExecutor  [shape=polygon,sides=4];
   StaticSingleThreadedExecutor  [shape=polygon,sides=4];

   }
```

![执行器继承关系](../images/executor-types.svg)

那个... *多轨执行器* 创建可配置的线程数,以便并行处理多个消息或事件。 *静态单向执行器* 在订阅,定时器,服务服务器,动作服务器等方面优化扫描节点结构的运行时间成本,它只在添加节点时进行一次扫描,而其他两个执行器则定期扫描这些更改。因此,在初始化时,只应该使用创建所有订阅,定时器等节点的静态单向执行器.

所有三个执行器都可以通过调用多个节点来使用 `add_node(..)` 用于每个节点。

``` cpp
rclcpp::Node::SharedPtr node1 = ...
rclcpp::Node::SharedPtr node2 = ...
rclcpp::Node::SharedPtr node3 = ...

rclcpp::executors::StaticSingleThreadedExecutor executor;
executor.add_node(node1);
executor.add_node(node2);
executor.add_node(node3);
executor.spin();
```

在上述例子中,静态单轨执行器的一个线程用于一起服务三个节点。如果是多轨执行器,则实际的并行性取决于召回组。

<span id="callback-groups"></span>

## 召回组

ROS 2 允许将节点的调用重新组织成组。在 rclcpp 中,这样的调用 *回调组* 可以通过 `create_callback_group` 在 rclpy 中,同样通过调用特定调用组类型的构建器来实现。调用组必须在节点的整个执行过程中存储(例如作为类成员),否则执行器将无法触发调用。然后,在创建订阅、计时器等时可以指定这个调用组。例如,通过订阅选项:

##### C++

``` cpp
my_callback_group = create_callback_group(rclcpp::CallbackGroupType::MutuallyExclusive);

rclcpp::SubscriptionOptions options;
options.callback_group = my_callback_group;

my_subscription = create_subscription<Int32>("/topic", rclcpp::SensorDataQoS(),
                                             callback, options);
```

##### Python

``` python
my_callback_group = MutuallyExclusiveCallbackGroup()
my_subscription = self.create_subscription(Int32, "/topic", self.callback, qos_profile=1,
                                           callback_group=my_callback_group)
```

所有未注明回调组而创建的订阅器、定时器等都指定给 *默认回调组*。默认的召回组可以通过 `NodeBaseInterface::get_default_callback_group()` 以 rclpp 和 by 键 `Node.default_callback_group` 在rclpy。 (原始内容存档于2018-09-21).

召回组有两种类型,类型必须在即时指定:

- *相互排斥:* 此组的召回不能平行执行 。

- *归依者:* 此组的召回可能平行执行 。

不同召回组的召回总是平行执行的。 多线程执行器使用它的线程作为集合, 根据这些条件来并行处理尽可能多的召回。 关于如何高效使用召回组的提示, 请参见 。 [使用回调组](../../How-To-Guides/Using-callback-groups.md).

rclcpp 中的执行器基础类也具有功能 `add_callback_group(..)`,它允许向不同的执行器分配调回组。通过使用操作系统调度器配置基本线程,特定的调回可以优先于其他调回。例如,控制循环的订阅和定时器可以优先于节点的所有其他订阅和标准服务。 [示例_rclcpp_cbg_执行器软件包](https://github.com/ros2/examples/tree/rolling/rclcpp/executors/cbg_executor) 提供了此机制的演示。

<span id="scheduling-semantics"></span>

## 排程语义

如果调用回调的处理时间比消息和事件发生的时间短, 执行器基本上按照 FIFO 顺序处理它们。 但是, 如果一些调用回调的处理时间更长, 消息和事件会排在堆栈的下层。 等待机制只向执行器报告极少有关这些队列的信息 。 详细来说, 它只报告是否有针对特定主题的任何消息 。 执行器使用这种信息来处理消息( 包括服务和动作) , 而不是在 FIFO 顺序中。 以下流程图可视化这种调度语义 。

![](../images/executors_scheduling_semantics.png)

这个语义最早是在 [Casini等人在ECRTS 2019上发表的论文](https://drops.dagstuhl.de/opus/volltexte/2019/10743/pdf/LIPIcs-ECRTS-2019-6.pdf).(注:本文还解释说,计时器事件优先于所有其他信息。) [这一优先次序在讨论会上被删除。](https://github.com/ros2/rclcpp/pull/841))

<span id="outlook"></span>

## 展望

虽然rclcpp的三名执行者在大多数应用中效果良好,但有些问题使其不适合实时应用,这需要明确的执行时间、确定性以及自定义对执行命令的控制。

1.  复杂和混合的排程语义。 理想的情况是, 要进行正式的排程语义分析 。

2.  回调可能会受到优先级反转的影响. 更高优先级回调可能会受到较低优先级回调的阻塞.

3.  对召回执行命令没有明确的控制.

4.  对特定主题的触发没有内置控制 。

此外,在CPU和内存使用方面,执行器的间接费用相当大。 Static Single-Treaded执行器大大降低了这一间接费用,但对于一些应用程序来说可能还不够。

这些问题已经通过下列事态发展得到部分解决:

- [rclcpp 等待设置](https://github.com/ros2/rclcpp/blob/rolling/rclcpp/include/rclcpp/wait_set.hpp)编号: `WaitSet` rclcpp 类允许直接等待订阅、定时器、服务服务器、动作服务器等,而不是使用执行器。它可用于执行决定性的、用户定义的处理序列,可能同时处理来自不同订阅的多个消息。 [示例_rclcpp_wait_set 软件包](https://github.com/ros2/examples/tree/rolling/rclcpp/wait_set) 提供了使用此用户级等待设置机制的几个示例。

- [rcc 执行器](https://github.com/ros2/rclc/blob/master/rclc/include/rclc/executor.h): C 客户端库中的此执行器 *rc( 红色)*为微ROS开发,赋予用户对回调执行顺序的精细控制,并允许自定义触发条件激活回调。此外,它执行逻辑执行时间(LET)语义学的想法。

<span id="further-information"></span>

## 更多信息

- 迈克尔·波赫纳尔等人: [“ROS 2 执行者:如何使它具有效率、实时性和决定性?”](https://www.apex.ai/roscon-21)2021年世界ROS讲习班,虚拟活动,2021年10月19日。

- 拉尔夫·兰格: [“使用《规则》第2条的高级执行管理”](https://www.youtube.com/watch?v=Sz-nllmtcc8&t=109s). ROS工业会议 虚拟活动. 2020年12月16日.

- 丹尼尔·卡西尼,托比亚斯·布拉斯,英戈·吕特克博赫勒,比约恩·勃兰登堡: [“根据基于保留的时间安排对ROS 2处理链的响应-时间分析”](https://drops.dagstuhl.de/opus/volltexte/2019/10743/pdf/LIPIcs-ECRTS-2019-6.pdf),第31届ECRTS 2019年7月,德国斯图加特.
