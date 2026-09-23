---
translation_status: machine_translated
source: How-To-Guides/Using-Custom-Rosdistro.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-custom-rosdistro-version"></span>

# 使用自定义 Rosdistro 版本

<span id="overview"></span>

## 概述

[rostro 维基月球](https://github.com/ros/rosdistro) 包含所有分发的ROS软件包的中央索引 `rosdep` 用于安装的二进制依赖性。当您引用时 `rosdep install ...`,它正在从 rosdistro 中检查本地缓存索引(在 `rosdep update`)在 a 中关联键 `package.xml` 到 ROS 软件包、 python 模块或要安装的二进制。因此,这个索引是 ROS 生态系统的重要组成部分。

然而,有时用户会希望对该索引的进一步控制在自己的专有密钥中添加,或使用之前的rosdistro状态。此指南将贯穿如何设置一个版本的rosdistro,以便在您的系统中使用。

本指南将采用的激励性实例是,由于开发计算机或连续集成系统出现故障,希望使用先前版本的Rolling。在从一个操作系统向另一个操作系统的过渡期间,由于支持向新的操作系统(即移动到Ubuntu 22.04至24.04)转变,在旧操作系统上滚动可能会变得无法使用。因此,我们希望设定一个先前版本的rodistro,与在特定操作系统上工作的Rolling分布保持一致,以便在升级到新的操作系统之前保持我们的系统运作。

<span id="important-preliminaries"></span>

## 重要序言

罗斯德普从它所设置的地点充斥它的缓存 `/etc/ros/rosdep/sources.list.d/20-default.list` 设置 rosdep 时使用 `rosdep init`,它充斥着 `20-default.list` 与主rosdistro URL( 缩写) ([从此文件](https://github.com/ros/rosdistro/blob/master/rosdep/sources.list.d/20-default.list)。生成的缓存由 `rosdep update` 设于 `~/.ros/rosdep/sources.cache` 并且不应用手修改。

否时 `ROSDISTRO_INDEX_URL` 环境变量值是在 rosdep 更新时设定的,它使用主要的公共 rosdistro 索引。然而,当该值被设定时,您可能使用自定义的 rosdisto 索引,该索引可以是公共索引的快照,也可以是完全独立的、包含您的专有软件包的索引。

若您想了解更多情况, [ros_buildfarm 软件包中的文档](https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/custom_rosdistro.rst).

<span id="how-to-use-a-custom-rosdistro-version"></span>

## 如何使用自定义的 Rosdistro 版本

要在您的 CI, docker 构建,本地环境,机器人,或其他应用中使用自定义版本,我们需要首先识别感兴趣的rosdistro版本.

作为激励性的例子,我们希望在第一次同步之前使用最后一个指数状态。 `rolling` 在此情况下,我们操作系统的最后一次同步是在2024年2月28日。 `rolling/2024-02-28` 标记的树枝。

因此,我们需要更新 `20-default.list` 使用我们标记的分支值,而不是使用主寄存器的当前状态。使用脚本可以实现如下。如果运行在本地主机上,您可能需要包含 `sudo`。此选项将更新列表以使用我们标记的分支而不是主分支 。

``` console
$ sed -i "s|ros\/rosdistro\/master|ros\/rosdistro\/rolling\/2024-02-28|" /etc/ros/rosdep/sources.list.d/20-default.list
```

之后,我们现在必须更新环境变量 `ROSDISTRO_INDEX_URL` 以指向我们新的罗盘指数。

``` console
$ export ROSDISTRO_INDEX_URL=https://raw.githubusercontent.com/ros/rosdistro/rolling/2024-02-28/index-v4.yaml
```

如果你打算用这个 在一个本地的宿主上很长一段时间, 它可能是明智的, `~/.bashrc` 让所有新的终端自动完成此任务。 `v4` 在我们的索引中指向索引格式的新版本。以前的索引也存在,但没有 `v4` 历史原因和遗留系统 仍然存在,但你不应该使用它。

之后,你还可以 `rosdep update`,它现在将使用修改来根据2024年2月28日的滚动分布状态更新索引。在断裂开始前,您可以在操作中看到这一点。 [Nav2 的圆环CI](https://github.com/ros-planning/navigation2/commit/80bb5bff1488c0677efcc4254b7a89908c853ba0) 财务报告和财务报告 [ros_gz 的 GitHub 动作](https://github.com/gazebosim/ros_gz/pull/522/files) 为了绕过他们的CI系统中的暂时滚出.

> **说明**
>
> 如果您正在使用自定义的 rosdistro 版本, 您可以用您的叉子或索引位置来替换默认列表中的最后 URL 和索引 URL 。
