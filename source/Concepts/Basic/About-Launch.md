---
translation_status: machine_translated
source: Concepts/Basic/About-Launch.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="launch"></span>

# 启动

一个ROS 2系统通常由许多跨不同过程(甚至不同的机器)运行的节点组成。 虽然每个节点都可以手动启动,但是它变得非常麻烦。

ROS 2的发射系统旨在将许多节点的运行自动化,使用单一命令。它帮助用户描述其系统的配置,然后按描述执行。系统的配置包括运行哪些程序,运行在何处,传递哪些参数,以及ROS特定的常规,通过给每个组件一个不同的配置,使得整个系统易于再利用组件。它还负责监测所启动的流程的状况,报告并/或对这些流程的状态变化作出反应。

以上所有内容均在“发射文件”中指定,文件可以写成XML、YAML或Python。然后,该发射文件可以使用 `ros2 launch` 命令,所有指定的节点都将运行。

要开始写和使用发射文件,请看 [发射教程](../../Tutorials/Intermediate/Launch/Launch-Main.md).

详细情况见 [发射文件](https://docs.ros.org/en/rolling/p/launch).
