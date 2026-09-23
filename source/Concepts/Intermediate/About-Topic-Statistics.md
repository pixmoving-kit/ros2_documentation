---
translation_status: machine_translated
source: Concepts/Intermediate/About-Topic-Statistics.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="topic-statistics"></span>

# 话题统计

<span id="overview"></span>

## 概述

ROS 2为任何订阅者收到的信息提供综合统计,允许用户收集订阅统计数据,使他们能够描述其系统的业绩,或协助诊断任何当前的问题。

提供的测量是收到信件的时代和收到信件的时期。每次测量所提供的统计是平均值、最大值、最低值、标准差和样本数。这些统计是在移动窗口中计算的。

<span id="how-statistics-are-calculated"></span>

## A. 如何计算统计数据

每一统计组均使用在2005年1月1日至12月31日期间执行的公用设施,以恒定时间和恒定内存计算。 [libstatistics_collector](https://github.com/ros-tooling/libstatistics_collector) 软件包。当订阅收到新信件时,这是当前测量窗口中计算的新样本。平均计算为 [移动平均值](https://en.wikipedia.org/wiki/Moving_average)。最大、最小和样本数在收到每个新样本后更新,而标准差则使用 [韦尔福德的在线算法](https://en.wikipedia.org/wiki/Algorithms_for_calculating_variance#Welford's_online_algorithm).

<span id="types-of-statistics-calculated"></span>

## 计算的统计数字类型

- 收到信件时段

  - 单位:毫秒

  - 使用系统时钟来测量收到信件之间的时间段

- 收到信件的年龄

  - 单位:毫秒

  - 要求信件在信头字段中包含一个时间戳,以便计算发布者发送的信件的年代

<span id="behavior"></span>

## 行为

默认情况下, 主题统计测量无法启用。 在通过订阅配置选项为特定节点启用此功能后, 接收消息的年龄和接收消息的时间段测量都启用了该特定订阅 。

数据公布于 [statistics_msg/msg/MetricsMessage](https://github.com/ros2/rcl_interfaces/blob/rolling/statistics_msgs/msg/MetricsMessage.msg) 可配置期间( 默认 1 秒) 至 可配置专题( 默认 ) `/statistics`),注意出版期也作为样本收集窗口期.

由于收到消息时段需要在信头字段内有一个消息时间戳,空数据会被发布,也就是说,如果找不到时间戳,所有统计值都是NaN. 发布NaN值而不是完全不发布,可以避免信号不存在问题,并旨在明确显示无法进行测量.

接收到的信件周期统计的每个窗口的首个样本不产生测量结果。 这是因为计算该统计需要知道上一封信件到达的时间, 所以在窗口中后续的样本生成测量结果 。

<span id="comparison-to-ros-1"></span>

## 与ROS 1的比较

与ROS 1类似 [专题统计](https://wiki.ros.org/Topics#Topic_statistics),既计算信件的时代,也计算信件的期间,尽管是从订阅方算起。其他 ROS 1 度量,例如,投放信件的数量或流量,目前没有提供。

<span id="support"></span>

## 支助

此功能目前仅用于C++(rclcpp)的ROS 2 Foxy中支持. 未来的工作和改进,如Python支持,可以找到 [这儿](https://github.com/ros2/ros2/issues/917).
