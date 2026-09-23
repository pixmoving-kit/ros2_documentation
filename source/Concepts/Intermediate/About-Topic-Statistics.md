<span id="topic-statistics"></span>
# 话题统计

<span id="overview"></span>
## 概述

ROS 2 提供了内置的统计测量功能，可对任意订阅收到的消息进行统计。收集订阅统计数据，可以帮助用户评估系统性能，或诊断当前存在的问题。

提供的测量项包括接收消息的年龄（message age，即消息从发布到接收所经过的时间）和接收消息的周期。每个测量项都提供平均值、最大值、最小值、标准差和样本数。这些统计量在移动窗口中计算。

<span id="how-statistics-are-calculated"></span>
## 统计量的计算方式

借助 [`libstatistics_collector`](https://github.com/ros-tooling/libstatistics_collector) 包中的工具，每组统计量都可以使用恒定时间和恒定内存进行计算。订阅每收到一条新消息，就会在当前测量窗口中增加一个用于计算的样本。平均值就是[移动平均值](https://en.wikipedia.org/wiki/Moving_average)。每收到一个新样本，都会更新最大值、最小值和样本数；标准差则使用 [Welford 在线算法](https://en.wikipedia.org/wiki/Algorithms_for_calculating_variance#Welford's_online_algorithm)计算。

<span id="types-of-statistics-calculated"></span>
## 统计测量的类型

- 接收消息的周期：单位为毫秒；使用系统时钟测量相邻两条消息的接收时间间隔。
- 接收消息的年龄：单位为毫秒；消息的 `header` 字段必须包含时间戳，才能计算消息从发布者发出后经过的时间。

<span id="behavior"></span>
## 行为

默认情况下，话题统计测量未启用。通过订阅配置选项为特定节点启用此功能后，该订阅的接收消息年龄和接收消息周期测量都会启用。

数据以 [`statistics_msg/msg/MetricsMessage`](https://github.com/ros2/rcl_interfaces/blob/rolling/statistics_msgs/msg/MetricsMessage.msg) 的形式，按照可配置的周期（默认 1 秒）发布到可配置的话题（默认 `/statistics`）。注意，发布周期也是样本采集窗口的周期。

对于需要 `header` 字段中时间戳的测量，如果找不到时间戳，就会发布空数据，即所有统计值均为 NaN。发布 NaN 而非完全不发布，可以避免信号缺失的问题，并明确表示无法完成测量。

每个窗口中，接收消息周期统计的第一个样本不会产生测量值。这是因为计算该统计量需要知道上一条消息的到达时间，因此只有窗口中后续的样本才能产生测量值。

<span id="comparison-to-ros-1"></span>
## 与 ROS 1 的比较

与 ROS 1 的[话题统计](https://wiki.ros.org/Topics#Topic_statistics)类似，这里也计算消息年龄和消息周期，但计算发生在订阅端。目前尚未提供 ROS 1 中的其他指标，例如丢失的消息数量或流量。

<span id="support"></span>
## 支持情况

目前，此功能在 ROS 2 Foxy 中仅支持 C++（rclcpp）。Python 支持等后续工作和改进计划见[此问题](https://github.com/ros2/ros2/issues/917)。
