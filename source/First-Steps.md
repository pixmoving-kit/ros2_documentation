<span id="first-steps-with-ros-learning-path"></span>
<span id="id1"></span>

# ROS 入门学习路线

ROS（Robot Operating System，机器人操作系统）是一个开源生态系统，为机器人应用的构建、部署、运行和维护提供框架、工具及库。本页通过一组文章和动手练习，介绍 ROS 框架的主要概念。完成这些内容后，你将掌握开始使用 ROS 开发应用所需的基础知识。

**领域：ROS 框架 | 内容类型：学习路线 | 经验水平：初学者**

<span id="summary"></span>

## 概述

ROS 框架是机器人不同部分之间进行通信的基础设施。它提供消息传递、标准接口，以及对多种编程语言和平台的支持。

在使用 ROS 开发或维护应用之前，需要理解框架的基本概念。turtlesim 工具和本站教程将帮助你快速入门。

<span id="prerequisites"></span>

## 前提条件

无。本文中的步骤将引导你下载并安装学习 ROS 基础知识所需的一切。

<span id="steps"></span>

## 学习步骤

<span id="learn-about-fundamental-concepts-behind-ros"></span>

### 1 了解 ROS 的基本概念

- [关于 ROS](About-ROS.md)
- [节点](Concepts/Basic/About-Nodes.md)
- [接口：话题、服务和动作](Concepts/Basic/Interfaces-Topics-Services-Actions.md)
- [参数](Concepts/Basic/About-Parameters.md)

<span id="install-ros-and-turtlesim"></span>

### 2 安装 ROS 和 turtlesim

ROS 安装包含使用 ROS 所需的基本软件包。如果熟悉 Linux，建议选择 Ubuntu（deb 软件包）；否则，Windows（二进制安装）也是不错的选择。参阅[安装选项](Installation.md)。

turtlesim 是为初学者设计的轻量级二维仿真工具，可以让你在简单、直观的环境中学习 ROS 核心概念。参阅[安装和设置 turtlesim](Tutorials/Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md)。

<span id="try-out-working-with-the-main-communication-components-of-the-ros-framework"></span>

### 3 尝试使用 ROS 框架的主要通信组件

使用 turtlesim 熟悉主要通信组件，并体验 ROS 框架中的消息传递。

1. 完成[节点教程](Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md)。
2. 完成[话题教程](Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)。
3. 完成[服务教程](Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Services/Understanding-ROS2-Services.md)。
4. 完成[参数教程](Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md)。
5. 完成[动作教程](Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Actions/Understanding-ROS2-Actions.md)。

<span id="learn-about-introspection-with-logs"></span>

### 4 了解如何通过日志查看系统内部状态

内省功能让你能够查看系统运行情况。节点通过日志，以多种方式输出事件和状态信息。

完成 [rqt_console 教程](Tutorials/Beginner-CLI-Tools/Using-Rqt-Console/Using-Rqt-Console.md)，体验如何利用日志查看系统状态。

<span id="learn-about-using-launch-files"></span>

### 5 学习使用启动文件

启动文件可以同时启动并配置多个包含 ROS 节点的进程，无需打开多个终端并为每个节点重复输入配置信息。

完成[启动文件教程](Tutorials/Beginner-CLI-Tools/Launching-Multiple-Nodes/Launching-Multiple-Nodes.md)。

<span id="learn-about-data-recording-and-playback"></span>

### 6 学习数据记录与回放

有时可以通过回放数据来复现测试和实验结果、调试机器人行为，或与他人分享工作成果。

完成[数据记录与回放教程](Tutorials/Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.md)。

<span id="next-steps"></span>

## 后续学习

为了进一步完善对 ROS 框架的认识，建议学习 [ROS 客户端库](Tutorials/Beginner-Client-Libraries.md)。
