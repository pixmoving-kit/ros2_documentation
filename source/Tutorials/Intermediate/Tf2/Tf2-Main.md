---
translation_status: machine_translated
source: Tutorials/Intermediate/Tf2/Tf2-Main.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="tf2"></span> <span id="tf2main"></span>

# `tf2`

许多 tf2 的教程同时为 C++ 和 Python 提供。 教程被精简以完成 C++ 音轨或 Python 音轨。 如果您想要同时学习 C++ 和 Python, 您应该通过 C++ 和 Python 的教程一次 。

<span id="workspace-setup"></span>

## 工作空间设置

如果您尚未创建完成教程的工作空间, [遵循此教程](../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md).

<span id="learning-tf2"></span>

## 学习 tf2

1.  [tf2 介绍](Introduction-To-Tf2.md).

    此教程会给您一个 Tf2 可以为您做什么的好概念 。 它会用 urtersim 来显示多机器人实例中的一些 tf2 功率 。 这也引入了使用 `tf2_echo`, `view_frames`,以及 `rviz`.

2.  写入静态播放器 [(彼贤).](Writing-A-Tf2-Static-Broadcaster-Py.md) [(C++)](Writing-A-Tf2-Static-Broadcaster-Cpp.md).

    此教程教你如何向 tf2 播放静态坐标框架 。

3.  写一个广播机 [(彼贤).](Writing-A-Tf2-Broadcaster-Py.md) [(C++)](Writing-A-Tf2-Broadcaster-Cpp.md).

    这个教程教你如何将机器人状态广播到 tf2.

4.  写一个收听器 [(彼贤).](Writing-A-Tf2-Listener-Py.md) [(C++)](Writing-A-Tf2-Listener-Cpp.md).

    此教程教你如何使用 tf2 来获取帧变换的访问权限 。

5.  添加一个框架 [(彼贤).](Adding-A-Frame-Py.md) [(C++)](Adding-A-Frame-Cpp.md).

    此教程教你如何在 tf2 中添加额外的固定框架 。

6.  利用时间 [(C++)](Learning-About-Tf2-And-Time-Cpp.md).

    此教程教你使用超时功能 `lookup_transform` 函数以等待 tf2 树上的变换。

7.  及时旅行 [(C++)](Time-Travel-With-Tf2-Cpp.md).

    这个教程教你 tf2 的高级时间旅行特性.

<span id="debugging-tf2"></span>

## 调试 tf2

1.  [四元数基础](Quaternion-Fundamentals.md).

    这个教程教你在ROS 2中使用的四角形的基本原理.

2.  [调试 tf2 问题](Debugging-Tf2-Problems.md).

    此教程教给您系统调试 tf2 相关问题的方法 。

<span id="using-sensor-messages-with-tf2"></span>

## 使用带有 tf2 的传感器信息

1.  [使用印有 tf2_ros 的数据类型: MessageFilter](Using-Stamped-Datatypes-With-Tf2-Ros-MessageFilter.md).

    此教程教你如何使用 `tf2_ros::MessageFilter` 用于处理盖章的数据类型。
