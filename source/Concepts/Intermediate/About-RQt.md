---
translation_status: machine_translated
source: Concepts/Intermediate/About-RQt.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="overview-and-usage-of-rqt"></span>

# RQt 概述与用法

<span id="overview"></span>

## 概述

RQt是一个图形化的用户界面框架,以插件的形式执行各种工具和界面. 一个可以运行所有现有的GUI工具作为RQt内部的可对接窗口. 这些工具仍然可以使用传统的独立方法运行,但RQt使得在单一屏幕布局下管理所有各种窗口更加容易.

您可以轻松地运行任意 RQt 工具/插件 :

``` console
$ rqt
```

此图形用户界面允许您在系统中选择任何可用的插件。 您也可以在独立的窗口中运行插件。 例如, RQt Python Console :

``` console
$ ros2 run rqt_py_console rqt_py_console
```

用户可以为 RQt 创建自己的插件 `Python` 或 时 间 `C++`。要查看您系统可用的 RQt 插件,请运行:

``` console
$ ros2 pkg list
```

然后寻找从开始的软件包 `rqt_`.

<span id="system-setup"></span>

## 系统设置

<span id="installing-from-debs"></span>

### 从 debs 安装

``` console
$ sudo apt install ros-rolling-rqt*
```

<span id="rqt-components-structure"></span>

## RQt 组件结构

RQt由两个元件组成:

- *rqt* - 核心基础设施模块。

- *rqt_common_plugins* - 常用调试工具。

<span id="advantage-of-rqt-framework"></span>

## RQt 框架的优点

与从零开始构建自己的图形用户界面相比 :

- GUI的标准化通用程序(启动- shutdown hook, 恢复以前的状态).

- 多个部件可以停靠在一个单一窗口中.

- 将您现有的 Qt 部件轻松转换为 RQt 插件 。

- 预计支持时间 [机器人堆栈交换](https://robotics.stackexchange.com/) (ROS社区问讯网站).

从系统架构的角度来看:

- 支持多平台(基本上在任何地方) [QT 语句](http://qt-project.org/) 和 ROS 运行)和多种语言(`Python`, `C++`).

- 可管理寿命周期:使用常用API的RQt插件使得维护和再利用更加容易.

<span id="further-reading"></span>

## 进一步阅读

- ROS 2 专题演讲 [宣布移植到 ROS 2](https://discourse.openrobotics.org/t/rqt-in-ros2/6428))

- [RQt 用于 ROS 1 文件](https://wiki.ros.org/rqt)

- RQt的简要概述(来自 [一个柳园实习生博客文章:](http://web.archive.org/web/20130518142837/http://www.willowgarage.com/blog/2012/10/21/ros-gui))
