<span id="overview-and-usage-of-rqt"></span>
# RQt 概述与使用

<span id="overview"></span>
## 概述

RQt 是一个图形用户界面框架，以插件形式实现各种工具和界面。现有的 GUI 工具都可以作为可停靠窗口在 RQt 中运行。这些工具仍然可以按传统方式独立运行，但 RQt 能让你更方便地在同一个屏幕布局中管理各种窗口。

运行以下命令，即可方便地使用 RQt 工具和插件：

```console
$ rqt
```

在这个图形界面中，可以选择系统上可用的任意插件。插件也可以在独立窗口中运行，例如 RQt Python 控制台：

```console
$ ros2 run rqt_py_console rqt_py_console
```

用户可以使用 `Python` 或 `C++` 编写自己的 RQt 插件。要查看系统上有哪些 RQt 插件，请运行：

```console
$ ros2 pkg list
```

然后查找名称以 `rqt_` 开头的软件包。

<span id="system-setup"></span>
## 系统配置

<span id="installing-from-debs"></span>
### 从 deb 包安装

```console
$ sudo apt install ros-rolling-rqt*
```

<span id="rqt-components-structure"></span>
## RQt 组件结构

RQt 由两个元包组成：

- *rqt*：核心基础设施模块。
- *rqt_common_plugins*：常用的调试工具。

<span id="advantage-of-rqt-framework"></span>
## RQt 框架的优势

与从头构建自己的 GUI 相比，RQt 具有以下优势：

- 标准化的 GUI 通用流程，例如启动和关闭钩子、恢复先前状态。
- 多个控件可以停靠在同一个窗口中。
- 可以方便地将现有 Qt 控件转换为 RQt 插件。
- 可以在 ROS 社区问答网站 [Robotics Stack Exchange](https://robotics.stackexchange.com/) 上寻求帮助。

从系统架构的角度看：

- 支持多个平台，基本上可运行于任何支持 [Qt](http://qt-project.org/) 和 ROS 的平台；支持多种语言，包括 `Python` 和 `C++`。
- 生命周期易于管理：RQt 插件使用统一 API，便于维护和复用。

<span id="further-reading"></span>
## 延伸阅读

- ROS 2 Discourse 上[关于移植到 ROS 2 的公告](https://discourse.openrobotics.org/t/rqt-in-ros2/6428)。
- [ROS 1 的 RQt 文档](https://wiki.ros.org/rqt)。
- RQt 简介，来自 [Willow Garage 实习生的博客文章](http://web.archive.org/web/20130518142837/http://www.willowgarage.com/blog/2012/10/21/ros-gui)。
