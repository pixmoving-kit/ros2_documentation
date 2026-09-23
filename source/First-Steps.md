---
translation_status: machine_translated
source: First-Steps.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="first-steps-with-ros-learning-path"></span> <span id="id1"></span>

# ROS 入门学习路径

ROS(Robot 操作系统)是一个开源生态系统,为机器人应用的建设、部署、运行和维护提供了框架、工具和库。本页面介绍一系列文章和操作活动,介绍ROS框架背后的主要概念。通过这些操作系统,您将获得开始使用ROS开发应用所需的基本知识。

**领域:ROS-框架 QQ 内容类型:学习-路径 QQ 经验:初学者**

<span id="summary"></span>

## 小结

ROS框架是“发光”框架,它使机器人不同部分之间的通信成为可能。 它包括信息传递、标准接口以及支持多种编程语言和平台。

您需要先了解框架的基本概念, 才能与 ROS 合作开发或维护应用程序 。 此站点中的龟兹工具和教程将帮助您提升速度 。

<span id="prerequisites"></span>

## 前提条件

无。 此文章中概述的步骤将引导您下载和安装所有您需要学习 ROS 基本内容的软件 。

<span id="steps"></span>

## 步骤

<span id="learn-about-fundamental-concepts-behind-ros"></span>

### 1 了解ROS背后的基本概念

- [关于 ROS](About-ROS.md)

- [节点](Concepts/Basic/About-Nodes.md)

- [接口：话题、服务和动作](Concepts/Basic/Interfaces-Topics-Services-Actions.md)

- [参数](Concepts/Basic/About-Parameters.md)

<span id="install-ros-and-turtlesim"></span>

### 2 安装ROS和龟兹

ROS的安装包括了与ROS合作的基本软件包。 如果您熟悉Linux, 我们推荐的平台是 Ubuntu (deb 软件包 ) 。 否则, 一个很好的替代安装平台是 Windows (二进制): [安装选项](Installation.md)

有了龟兹姆,一个为初学者设计的轻量级2D模拟工具,可以在简单的视觉环境中学习核心ROS概念: [安装和设置龟兹](Tutorials/Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md)

<span id="try-out-working-with-the-main-communication-components-of-the-ros-framework"></span>

### 3 尝试与ROS框架的主要通信部分合作

使用龟兹姆来熟悉主要通信组件,并尝试在ROS框架中发布消息.

1.  完成节点教程 : [理解节点](Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md)

2.  完成主题教程 : [理解话题](Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)

3.  完成服务辅导: [理解服务](Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Services/Understanding-ROS2-Services.md)

4.  完成参数教程 : [理解参数](Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md)

5.  完成动作教程 : [理解动作](Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Actions/Understanding-ROS2-Actions.md)

<span id="learn-about-introspection-with-logs"></span>

### 4 学习用日志进行反省

透视可以让您看到一个系统如何运行的信息。节点会使用日志以各种方式输出关于事件和状态的信息 。

要通过运行中的日志看到内在回顾, 请完成 rqt\_ console 教程 : [使用 rqt\_ console 查看日志](Tutorials/Beginner-CLI-Tools/Using-Rqt-Console/Using-Rqt-Console.md)

<span id="learn-about-using-launch-files"></span>

### 5 学习如何使用发射文件

启动文件允许您同时启动和配置包含ROS节点的多个进程,而不是打开多个终端并重新进入每个节点的配置细节.

完成发射文件教程 : [启动节点](Tutorials/Beginner-CLI-Tools/Launching-Multiple-Nodes/Launching-Multiple-Nodes.md)

<span id="learn-about-data-recording-and-playback"></span>

### 6 学习数据记录和回放

有时重放数据以复制测试和实验的结果,调试机器人的行为,或与其他人分享你的工作是有用的。

完成录制和播放教程 : [录制与回放数据](Tutorials/Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.md)

<span id="next-steps"></span>

## 后续步骤

为了完成你对ROS框架的了解,我们建议熟悉ROS客户端库: [入门：客户端库](Tutorials/Beginner-Client-Libraries.md)
