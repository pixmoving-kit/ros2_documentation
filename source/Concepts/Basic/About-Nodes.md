---
translation_status: machine_translated
source: Concepts/Basic/About-Nodes.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="nodes"></span>

# 节点

节点是 ROS 2 图中的参与者，通过 [客户端库](About-Client-Libraries.md) 与其他节点通信。节点可以与同一进程、其他进程或其他计算机上的节点通信。节点通常是 ROS 图中的计算单元；每个节点应负责一项逻辑任务。

节点可以 [发布](About-Topics.md) 数据到指定话题，将数据传递给其他节点，或者 [订阅](About-Topics.md) 指定话题，接收其他节点的数据。节点也可以作为 [服务客户端](About-Services.md) ，让其他节点代为计算，或者作为 [服务端](About-Services.md) 为其他节点提供功能。对于耗时较长的计算，节点可以作为 [动作客户端](About-Actions.md) ，让其他节点代为执行，或者作为 [动作服务端](About-Actions.md) 为其他节点提供功能。节点还可以提供可配置的 [参数](About-Parameters.md) ，在运行时改变行为。

一个节点通常同时包含发布者、订阅者、服务端、服务客户端、动作服务端和动作客户端等多种实体。

节点之间的连接通过分布式 [发现](About-Discovery.md) 机制建立。
