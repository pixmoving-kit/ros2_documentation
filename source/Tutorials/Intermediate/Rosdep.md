<span id="managing-dependencies-with-rosdep"></span>

# 使用 rosdep 管理依赖

**目标：** 使用 `rosdep` 管理外部依赖。

**教程级别：** 中级

**预计耗时：** 5 分钟

本教程介绍如何使用 `rosdep` 管理外部依赖。

> 目前 rosdep 仅支持 Linux 和 macOS，不支持 Windows。[rosdep 项目](https://github.com/ros-infrastructure/rosdep)有长期计划增加 Windows 支持。

<span id="what-is-rosdep"></span>

## 什么是 rosdep？

`rosdep` 是处理软件包和外部库的依赖管理命令行工具，负责识别并安装构建或安装软件包所需的依赖。它本身不是直接安装软件包的包管理器，而是元包管理器：根据系统和依赖信息，确定特定平台需要安装的包，再交给系统包管理器实际安装，例如 Debian/Ubuntu 的 `apt`、Fedora/RHEL 的 `dnf`。

通常在构建工作空间前运行它，安装其中各包的依赖。既可针对单个包，也可针对包含多个包的目录，例如工作空间。

> 名称虽包含 ROS，`rosdep` 并非完全依赖 ROS。可将它作为独立 Python 包安装，用于非 ROS 项目。正常运行需要可用的 rosdep 键，只需几条命令即可从公开 Git 仓库下载。

<span id="a-little-about-package-xml-files"></span>

## package.xml 文件简介

`rosdep` 从 `package.xml` 中读取依赖。依赖列表必须完整、正确，才能让各类工具确定软件包所需的依赖。缺失或错误的声明可能导致用户无法使用软件包、工作空间中的包构建顺序错误，或无法发布软件包。

`package.xml` 中的依赖通常称为“rosdep 键”，由包的创建者手动填写，应完整列出需要的所有非内置库和软件包。

依赖通过以下标签表示，完整规范见 [REP-149](https://reps.openrobotics.org/rep-0149/)。

<span id="depend"></span>

### depend

`<depend>` 表示构建和运行时都需要的依赖。对于 C++ 包，不确定时可使用此标签。纯 Python 包通常没有构建阶段，应使用 `<exec_depend>`，而不是此标签。

<span id="build-depend"></span>

### build_depend

`<build_depend>` 用于只在构建时需要、运行时不需要的依赖。安装好的二进制包无须再安装这些依赖。

但若导出的头文件包含该依赖的头文件，下游用户仍会需要它，此时还要声明 `<build_export_depend>`。

<span id="build-export-depend"></span>

### build_export_depend

若导出的头文件包含某依赖的头文件，那么对你的包声明 `<build_depend>` 的其他包也需要该依赖。这主要适用于头文件和 CMake 配置文件。

导出的库所引用的其他库通常应声明为 `<depend>`，因为运行时也需要它们。

<span id="exec-depend"></span>

### exec_depend

`<exec_depend>` 声明运行软件包时所需的共享库、可执行程序、Python 模块、启动脚本及其他文件的依赖。

<span id="test-depend"></span>

### test_depend

`<test_depend>` 声明仅测试需要的依赖，不应与 `<build_depend>`、`<exec_depend>` 或 `<depend>` 中已有的键重复。

<span id="how-does-rosdep-work"></span>

## rosdep 如何工作？

`rosdep` 检查指定路径或软件包的 `package.xml`，读取其中的键，再查询中央索引，找到各包管理器中对应的 ROS 包或软件库，然后安装。

中央索引会下载到本机，避免每次运行都联网。Debian/Ubuntu 的配置位于 `/etc/ros/rosdep/sources.list.d/20-default.list`。

该中央索引称为 `rosdistro`，可在[网上查看](https://github.com/ros/rosdistro)，下一节会进一步介绍。

<span id="how-do-i-know-what-keys-to-put-in-my-package-xml"></span>

## package.xml 应填写哪些键？

<span id="id1"></span>

- 如果依赖是基于 ROS 且已发布到 ROS 生态的软件包[¹](#id2)，例如 `nav2_bt_navigator`，直接使用包名。对应发行版的全部已发布包可在 [rosdistro](https://github.com/ros/rosdistro) 的 `<distro>/distribution.yaml` 中找到，例如 `humble/distribution.yaml`。
- 如果依赖非 ROS 软件包，通常称为系统依赖，则需要查找对应库的键。主要查看 [rosdep/base.yaml](https://github.com/ros/rosdistro/blob/master/rosdep/base.yaml)，其中包含 `apt` 系统依赖，以及 [rosdep/python.yaml](https://github.com/ros/rosdistro/blob/master/rosdep/python.yaml)，其中包含 Python 依赖。

在这些文件中搜索所需库，找到的键名就是应填入 `package.xml` 的名称。

例如，软件包重视文档质量，因此依赖 `doxygen`。在 `rosdep/base.yaml` 中搜索后可见：

```yaml
doxygen:
  arch: [doxygen]
  debian: [doxygen]
  fedora: [doxygen]
  freebsd: [doxygen]
  gentoo: [app-doc/doxygen]
  macports: [doxygen]
  nixos: [doxygen]
  openembedded: [doxygen@meta-oe]
  opensuse: [doxygen]
  rhel: [doxygen]
  ubuntu: [doxygen]
```

这里的 rosdep 键是 `doxygen`，它会根据操作系统解析为各包管理器中的相应名称。

<span id="what-if-my-library-isn-t-in-rosdistro"></span>

## 库不在 rosdistro 中怎么办？

这正是参与开源开发的机会：可以自己添加！rosdistro 的 PR 通常在一周内即可合并。贡献新键的方法见[详细说明](https://github.com/ros/rosdistro/blob/master/CONTRIBUTING.md#rosdep-rules-contributions)。

如果由于某种原因无法公开贡献，还可以：

1. Fork rosdistro，维护包含额外键的替代索引，参见[使用自定义 rosdistro](../../How-To-Guides/Using-Custom-Rosdistro.md)。
2. 新建包含自定义键的文件，让 rosdep 在构建本地索引时读取它，参见[补充自定义 rosdep 键](../Advanced/Supplementing-Custom-Rosdep-Keys.md)。

<span id="how-do-i-use-the-rosdep-tool"></span>

## 如何使用 rosdep？

<span id="rosdep-installation"></span>

### 安装 rosdep

结合 ROS 使用时，推荐安装随 ROS 发行版提供的系统软件包。

Ubuntu：

```console
$ sudo apt install python3-rosdep
```

RHEL：

```console
$ sudo dnf install python3-rosdep
```

> Debian 和 Ubuntu 还有一个名称相似的包 `python3-rosdep2`。若已安装，应先卸载它，再安装 `python3-rosdep`。

在非 ROS 项目中使用时，系统软件包可能不可用，可以从 [PyPI](https://pypi.org) 直接安装：

```console
$ pip install rosdep
```

<span id="rosdep-operation"></span>

### 运行 rosdep

了解 rosdep、package.xml 和 rosdistro 后，就可以开始使用。首次使用时需要初始化：

```console
$ sudo rosdep init
$ rosdep update
```

这会初始化 rosdep，`update` 则更新本地缓存的 rosdistro 索引。建议定期运行 `update`，获取最新索引。

最后用 `rosdep install` 安装依赖。通常在包含多个软件包的工作空间中，一次安装全部依赖。在工作空间根目录、源码位于 `src` 时执行：

```console
$ rosdep install --from-paths src -y --ignore-src
```

参数含义：

- `--from-paths src`：指定查找 `package.xml` 并解析依赖键的路径。
- `-y`：对包管理器的所有确认默认回答“是”，以免交互提示。
- `--ignore-src`：若依赖包本身已存在于工作空间中，即使有对应 rosdep 键，也不再安装。

其他选项可通过 `rosdep -h` 查看，或阅读[完整文档](http://docs.ros.org/en/independent/api/rosdep/html/)。

<span id="id2"></span>

¹ “发布到 ROS 生态”是指软件包已列入 [rosdistro 数据库](https://github.com/ros/rosdistro)中至少一个发行版的 `<distro>/distribution.yaml`。
