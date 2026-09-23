---
translation_status: machine_translated
source: Installation/RMW-Implementations.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="rmw-implementations"></span>

# RMW 实现

默认情况下,ROS 2 使用 DDS 作为它的 [中间软件](https://design.ros2.org/articles/ros_on_dds.html)。它与多个DS或RTPS(DDS电线协议)供应商兼容。目前支持eProsima的快速DDS、RTI的Connext DDS、Eclipse气旋DDS和GurumNetworks GurumDDS。

它还支持非DDS RMW的执行,如Zenoh.

见 [REP-2000号报告](https://reps.openrobotics.org/rep-2000/) 以销售方式向支持的RMW销售商发放。

默认的 RMW 供应商是 eProsima 的快速DDS 。

审查所有可能的备选方案:

- [DDS 实现](RMW-Implementations/DDS-Implementations.md) 解释如何使用DDS.

- [非DDS执行](RMW-Implementations/Non-DDS-Implementations.md) 解释如何使用非DDS执行.
