<span id="using-custom-rosdistro-version"></span>
# 使用自定义版本的 rosdistro

<span id="overview"></span>
## 概述

[rosdistro](https://github.com/ros/rosdistro) 包含所有 ROS 发行版的软件包中央索引，以及用于安装二进制依赖包的 `rosdep` 键。执行 `rosdep install ...` 时，它会查询 rosdistro 的本地缓存索引（由 `rosdep update` 填充），将 `package.xml` 中的键对应到要安装的 ROS 软件包、Python 模块或二进制包。因此，这个索引是 ROS 生态的重要组成部分。

但有时，用户希望进一步控制索引，例如添加自己的专有依赖键，或使用 rosdistro 的历史版本。本指南介绍如何指定系统使用的 rosdistro 版本。

本指南以开发计算机或持续集成环境因更新而出故障、需要使用旧版 Rolling 的情况为例。在从一个操作系统版本过渡到另一个版本时，支持重点会转向新系统（例如从 Ubuntu 22.04 迁移到 24.04），旧系统上的 Rolling 可能因而无法使用。为在升级操作系统前保持系统可用，我们希望指定一个与当前系统上可正常运行的 Rolling 相匹配的历史 rosdistro 版本。

<span id="important-preliminaries"></span>
## 重要基础知识

默认情况下，rosdep 根据 `/etc/ros/rosdep/sources.list.d/20-default.list` 中指定的位置填充缓存。执行 `rosdep init` 时，会使用主要 rosdistro URL 填充 `20-default.list`（内容来自[此文件](https://github.com/ros/rosdistro/blob/master/rosdep/sources.list.d/20-default.list)）。`rosdep update` 生成的缓存位于 `~/.ros/rosdep/sources.cache`，不应手动修改。

执行 rosdep 更新时，如果未设置环境变量 `ROSDISTRO_INDEX_URL`，就会使用主要的公共 rosdistro 索引。设置该变量后，可以改用自定义 rosdistro 索引：它既可以是公共索引的快照，也可以是包含你自己的专有软件包的独立索引。

更多信息请参阅 [ros_buildfarm 软件包中的文档](https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/custom_rosdistro.rst)。

<span id="how-to-use-a-custom-rosdistro-version"></span>
## 如何使用自定义版本

要在 CI、Docker 构建、本地环境、机器人或其他应用中使用自定义版本，首先需要确定所需的 rosdistro 版本。

在本例中，我们希望使用 `rolling` 在新操作系统上首次同步之前的最后一版索引。原操作系统的最后一次同步发生在 2024 年 2 月 28 日。同步会打上标签，因此可以通过 `rolling/2024-02-28` 标签取得相应版本。

接下来应更新 `20-default.list`，使用该标签对应的地址，而不使用主仓库的当前状态。下面的命令会将列表中的 master 分支替换为指定标签。在本地主机上执行时，可能需要加上 `sudo`。

```console
$ sed -i "s|ros\/rosdistro\/master|ros\/rosdistro\/rolling\/2024-02-28|" /etc/ros/rosdep/sources.list.d/20-default.list
```

然后，更新 `ROSDISTRO_INDEX_URL` 环境变量，使其指向新的 rosdistro 索引：

```console
$ export ROSDISTRO_INDEX_URL=https://raw.githubusercontent.com/ros/rosdistro/rolling/2024-02-28/index-v4.yaml
```

如果打算在本地主机上长期使用这个版本，可以将该设置加入 `~/.bashrc`，让每个新终端自动应用它。索引名称中的 `v4` 表示较新的索引格式。出于历史和旧系统兼容原因，不带 `v4` 的旧索引仍然存在，但不应再使用它。

之后执行 `rosdep update`，它就会按修改后的设置，将索引更新为故障发生前、2024 年 2 月 28 日的 Rolling 状态。[Nav2 的 CircleCI](https://github.com/ros-planning/navigation2/commit/80bb5bff1488c0677efcc4254b7a89908c853ba0) 和 [ros_gz 的 GitHub Actions](https://github.com/gazebosim/ros_gz/pull/522/files) 中都有实际示例，用于绕过 CI 系统中的 Rolling 临时故障。

!!! note "说明"
    如果使用自己的 rosdistro 版本，可以将默认列表中的最终 URL 和索引 URL 替换为你的派生仓库或索引所在位置。
