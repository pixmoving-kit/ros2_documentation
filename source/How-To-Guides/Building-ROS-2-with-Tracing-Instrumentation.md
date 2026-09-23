---
translation_status: machine_translated
source: How-To-Guides/Building-ROS-2-with-Tracing-Instrumentation.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="building-ros-2-with-tracing-instrumentation"></span>

# 构建启用追踪插桩的 ROS 2

本指南介绍如何用ROS 2 提供的追查仪器来构建 ROS 2 。 `ros2_tracing`。关于更多信息,参见 [存储器](https://github.com/ros2/ros2_tracing).

仪器包含在 ROS 2 源代码中。 然而, 如果使用二进制或从源建置, 仪器不会默认触发痕量点。 要获得痕量点, 需要安装 LTTng 痕量器, 然后需要从源头重建 ROS 2 的一部分 。

> **说明**
>
> 本指南仅适用于Linux系统,并假定Ubuntu被使用.

<span id="prerequisites"></span>

## 前提条件

设置您的系统从源代码创建 ROS 2 。 [源安装页面](../Installation/Alternatives/Ubuntu-Development-Setup.md) 以获取更多信息。

<span id="installing-the-tracer"></span>

## 安装跟踪器

安装 [LTTng 跟踪器](https://lttng.org/docs) 以及相关的工具和依赖性。

``` bash
sudo apt-get update
sudo apt-get install -y lttng-tools liblttng-ust-dev python3-lttng python3-babeltrace babeltrace
```

这只安装了LTTng用户空间跟踪器,而不是LTTng内核跟踪器,因为不需要跟踪ROS 2应用.

<span id="building"></span>

## 大楼

此步骤取决于您是否从源头构建 ROS 2 , 还是使用 ROS 2 二进制 。

<span id="with-source-installation"></span>

### 安装源码

如果你已经知道 [从源头构建 ROS 2](../Installation/Alternatives/Ubuntu-Development-Setup.md) 在安装 LTTng 之前,您至少需要重建到 `tracetools` 软件包 :

``` bash
cd ~/ws
colcon build --packages-up-to tracetools --cmake-force-configure
```

<span id="with-binary-installation"></span>

### 用二进制安装

若依赖ROS 2 二进制([deb 软件包](../Installation/Ubuntu-Install-Debs.md) 或 时 间 [“脂肪”档案](../Installation/Alternatives/Ubuntu-Install-Binary.md)),您需要复制 `ros2_tracing` 存储到您的工作空间中, 并至少构建到 `tracetools` 软件包 :

``` bash
cd ~/ws/src
git clone https://github.com/ros2/ros2_tracing.git
cd ../
colcon build --packages-up-to tracetools
```

<span id="validating"></span>

## 校验

来源并证实已启用追踪:

``` bash
cd ~/ws
source install/setup.bash
ros2 run tracetools status
```

应印出:

``` bash
Tracing enabled
```

如果有其它东西被打印出来,那么就出问题了.

<span id="disabling-tracing"></span>

## 无法追踪

如果在构建时安装和找到 LTTng 用户空间跟踪器 `tracetools`或者,从ROS 2中建立并完全去除跟踪点和追查仪器。 `TRACETOOLS_DISABLED` CMake 选项到 `ON`:

``` bash
colcon build --cmake-args -DTRACETOOLS_DISABLED=ON --no-warn-unused-cli
```
