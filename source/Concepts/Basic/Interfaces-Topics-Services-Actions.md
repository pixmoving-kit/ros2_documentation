---
translation_status: machine_translated
source: Concepts/Basic/Interfaces-Topics-Services-Actions.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="interfaces-topics-services-actions"></span> <span id="id1"></span>

# 接口：话题、服务和动作

ROS中的接口定义了节点如何交换数据。本文解释了ROS接口的不同类型和它们之间的区别。有了这些信息,您将能够为您的目的选择正确的接口。

**领域:ROS-框架 QQ 内容类型:概念 QQ 经验:初学者**

<span id="summary"></span>

## 小结

ROS节点一般通过以下三类接口进行通信:

- 题目:用于连续的数据流。

- 服务:用于同步请求/响应互动(即时发生的短任务).

- 动作:用于有反馈的长期任务(任务可能需要一些时间才能完成).

为进行一致的交流,每个接口都使用下列定义: `.msg`, `.srv`,或 `.action` 文档。

[学习更多节点](About-Nodes.md)

<span id="topics"></span>

## 话题

主题界面用于连续的数据流,例如流传的传感器数据或您的机器人状态。主题定义被存储在 `.msg` 文件。主题执行一个发布/订阅模式。一个节点将数据发布到一个主题,而其他节点则订阅以接收该数据。该界面类型具有以下主要特点:

- 同步、单向通信

- 多个出版商和订户可以分享同一话题

``` mermaid

        flowchart LR
 P[Publisher node] -->|Publishes messages| T[Topic]
 T -->|Delivers messages| S1[Subscriber node]
 T -->|Delivers messages| S2[Subscriber node]
    
```

主题键在某一主题上识别单个出版商,因此节点和工具可以区分消息来自何处. 每个主题键在多个出版商共享同一主题时更容易跟踪数据源.

<span id="topic-statistics"></span>

## 话题统计

主题统计是内置的测量数据, 帮助您了解订阅时消息的表现。 当启用时, 它们会自动跟踪两件事:

信件年龄 :  
消息到来时,根据时间戳算出多少年了.

信件周期 :  
发送信件之间的时间 。

对于信件的时代和期间,ROS使用每次新信件到达时都会更新的移动窗口计算平均、最小、最大、标准偏差和样本数量。这些计算经常在时间和内存中运行,使用专用工具。当您启用订阅的话题统计时,ROS会定期公布所收集的数据。 `MetricsMessage` 这样可以清晰地了解时间规律、延误和不合规定之处,从而更容易评估系统性能或诊断与信息流有关的问题。

> **提示**
>
> 默认间隔为 1 秒。 默认统计主题为 `/statistics`.

[学习如何启用专题统计](../../Tutorials/Advanced/Topic-Statistics-Tutorial/Topic-Statistics-Tutorial.md)

<span id="services"></span>

## 服务

服务接口用于同步请求/响应交互,例如,当您想要发送一个查询请求特定机器人的配置时。服务定义被存储在 `.srv` 文件。服务执行一个请求/响应模式。客户端发送一个请求,服务器回复一个响应。该接口类型具有以下主要特点:

- 同步通讯

- 需要确认或应请求提供结果的短期行动的理想

``` mermaid

        sequenceDiagram
 participant Service client
 participant Service server
 Service client->>Service server: Request
 Service server-->>Service client: Response
    
```

<span id="actions"></span>

## 动作

动作接口是指有反馈的长期任务,例如将机器人移动到特定位置,或要求机器人执行复杂的运动。动作定义被存储在 `.action` 文件。动作允许客户端发送目标,在执行过程中接收反馈,必要时取消,如有结果,则返回。该接口类型有以下主要特点:

- 与反馈和结果同步

- 适用于需要时间的行动

``` mermaid

        sequenceDiagram
 participant c as Action client
 participant s as Action server
 c->>s: Sends a goal
 s-->>c: Provides feedback (periodic)
 s-->>c: Sends a result
    
```

<span id="key-differences-between-ros-interfaces"></span>

## ROS 接口之间的关键差异

所有三个接口都允许节点之间的通信,但每个节点都服务于不同的目的. 下表概括了ROS接口类型之间的差异:

|          | 图案                 | 方向     | 提供的结果 | 典型使用情况 | 取消     |
|----------|----------------------|----------|------------|--------------|----------|
| **话题** | Publish/Subscribe    | 单程     | 否         | 连续数据     | 不予支持 |
| **服务** | Request/Response     | 双向     | 是         | 快速查询     | 不予支持 |
| **动作** | Goal/Feedback/Result | 双向反馈 | 是         | 长期任务     | 获得支持 |
