---
translation_status: machine_translated
source: Tutorials/Intermediate/Rosdep.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="managing-dependencies-with-rosdep"></span>

# 使用 rosdep 管理依赖

**目标：** 使用 `rosdep`.

**教程级别：** 中级

**用时：** 5分钟

此教程将解释如何使用 `rosdep`.

> **警告**
>
> 目前 rosdep 只工作于 Linux 和 macOS; Windows 不支持。 有长期计划将支持 Windows 添加到 <https://github.com/ros-infrastructure/rosdep>.

<span id="what-is-rosdep"></span>

## 罗斯德是什么?

`rosdep` 是一个依赖性管理工具,可以与软件包和外部库合作。它是一个命令行工具,用于识别和安装依赖性以构建或安装软件包。 `rosdep` 是,这是 *没有* 软件包管理器本身; 是元软件包管理器, 使用自身对系统和依赖性的知识来寻找合适的软件包, 以便在某个特定平台上安装。 实际安装使用系统软件包管理器( 例如) 。 `apt` 在Debian/Ubuntu网站上, `dnf` (见Fedora/RHEL等)。

在建立工作空间之前,它最常被引用,用来在工作空间内安装软件包的依赖性.

它具有在单个软件包上工作或在一个软件包目录上工作的能力(如工作空间).

> **说明**
>
> 虽然这个名字是给ROS的, `rosdep` 在非ROS软件项目中可以使用这个强大的工具,将它安装为独立的 Python 软件包。成功运行 `rosdep` 依赖 `rosdep keys` ,可以从一个带有几个简单命令的公共 git 寄存器中下载。

<span id="a-little-about-package-xml-files"></span>

## 关于 package.xml 文件的一点

那个... `package.xml` 是您软件中的文件, 其中 `rosdep` 找到一组依存关系。重要的是,该目录必须列出依存关系。 `package.xml` 完整而正确,这使得所有工具都能够确定软件包的依赖性。缺失或不正确的依赖性可能导致用户无法使用您的软件包,无法在正在建设的工作空间中运行软件包,以及无法发布软件包。

二、《公约》的依赖性 `package.xml` 文件一般称为“ rosdep 密钥”。 这些依赖性是手动拼接的。 `package.xml` 由软件包的创建者编写的文件,并且应当详尽列出它需要的任何非构建的库和软件包。

这些标签在下列标签中有所体现(见: [REP-149号文件](https://reps.openrobotics.org/rep-0149/) 详细规格:

<span id="depend"></span>

### `<depend>`

这些是您软件包在构建时间和运行时间中都应该提供的依赖性。 对于 C++ 软件包, 如果有疑问, 请使用此标签 。 纯 Python 软件包一般没有构建阶段, 所以永远不应该使用, 并且应该使用 。 `<exec_depend>` 换句话说。

<span id="build-depend"></span>

### `<build_depend>`

如果您只使用特定的依赖来构建您的软件包,而不是在执行时,您可以使用 `<build_depend>` 标记 。

由于这种依赖性,您的软件包的已安装二进制不需要安装该特定软件包 。

但是,如果您的软件包导出一个包含来自此依赖的页眉的页眉,则会产生问题。在这种情况下,您还需要一个 `<build_export_depend>`.

<span id="build-export-depend"></span>

### `<build_export_depend>`

如果导出一个包含来自依赖的页眉的页眉,则其他包将需要它。 `<build_depend>` 。这主要适用于信头和 CMake 配置文件。您导出库引用的库软件包通常应该指定 `<depend>`,因为在执行时也需要这些设备。

<span id="exec-depend"></span>

### `<exec_depend>`

此标签声明共享库、 可执行文件、 Python 模块、 启动脚本以及运行您的软件包所需的其他文件的依赖性 。

<span id="test-depend"></span>

### `<test_depend>`

此标签只通过测试来声明依赖性 。 这里的依赖性应该 *没有* 与指定的密钥复制 `<build_depend>`, `<exec_depend>`,或 `<depend>`.

<span id="how-does-rosdep-work"></span>

## 罗斯德是怎么工作的?

`rosdep` 将检查 `package.xml` 在路径或特定软件包中找到存储在其中的 rosdep 密钥。然后,这些密钥与中央索引交叉引用,以便在各种软件包管理器中找到合适的ROS 软件包或软件库。最后,一旦找到软件包,它们就被安装并准备出发 !

`rosdep` 工作方式是将中央索引检索到您的本地机器上, 这样它就不必每次运行时访问网络( 在 Debian/ Ubuntu 上, 它的配置存储在其中) `/etc/ros/rosdep/sources.list.d/20-default.list`).

中央指数称为: `rosdistro`,哪个 [可在网上找到](https://github.com/ros/rosdistro)我们将在下一节探讨更多的问题。

<span id="how-do-i-know-what-keys-to-put-in-my-package-xml"></span>

## 我怎么知道把什么钥匙放进我的包里?

问得好, 我很高兴你问!

- 如果您想要依赖的软件包基于ROS, 并已释放到ROS生态系统中 <span id="id1"></span>[\[1\]](#id2), e.g. `nav2_bt_navigator`,您可以简单地使用软件包的名称。您可以在其中找到所有已发布的ROS软件包的清单。 <https://github.com/ros/rosdistro> 现时 `<distro>/distribution.yaml` (e.g. `humble/distribution.yaml`你给的ROS分配。

- 如果您想要依赖一个非ROS软件包, 经常被称为“ 系统依赖性 ” , 您需要为特定库找到密钥 。 一般来说, 有两个文件值得关注 :

  - [rosdep/base.yaml](https://github.com/ros/rosdistro/blob/master/rosdep/base.yaml) 包含 `apt` 系统依赖关系

  - [rosdep/python.yaml](https://github.com/ros/rosdistro/blob/master/rosdep/python.yaml) 包含 Python 依赖性

要找到密钥, 请在这些文件中搜索您的库并找到名称。 这是要放入的密钥 。 `package.xml` 文档。

例如,想象一个软件包依赖 `doxygen` 因为它是一个伟大的软件,它关心高质量的文档(提示)。我们会搜索 `rosdep/base.yaml` (单位:千美元) `doxygen` 并来到这里:

``` yaml
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

这意味着我们的玫瑰花钥匙是 `doxygen`,将确定不同操作系统软件包管理器中的各种名称,以供安装。

<span id="what-if-my-library-isn-t-in-rosdistro"></span>

## 如果我的库不在罗盘里呢?

如果您的库不在, `rosdistro`,您可以体验开源软件开发的伟大性:您可以自己加入它! Pull for rosdistro 请求通常在一周内被很好地合并。

[详细说明请参见此处。](https://github.com/ros/rosdistro/blob/master/CONTRIBUTING.md#rosdep-rules-contributions) 如果出于某种原因这些可能无法公开提供,则存在其他选项:

1.  伪造rosdistro并维持包含额外密钥的替代索引([使用自定义 Rosdistro 版本](../../How-To-Guides/Using-Custom-Rosdistro.md))

2.  创建包含自定义密钥的新文件并指示 `rosdep` 当输入本地索引时检查它( T) :[补充自定义 rosdep 键](../Advanced/Supplementing-Custom-Rosdep-Keys.md))

<span id="how-do-i-use-the-rosdep-tool"></span>

## 我该怎么用Rostep工具?

<span id="rosdep-installation"></span>

### 罗斯德普安装

如果你在用的话 `rosdep` 使用ROS,可以方便地与ROS的分布进行包装。 `rosdep`。您可以安装它 :

##### Ubuntu

``` console
$ sudo apt install python3-rosdep
```

##### RHEL

``` console
$ sudo dnf install python3-rosdep
```

> **说明**
>
> 在Debian和Ubuntu上,还有一个类似命名的软件包叫做 `python3-rosdep2`。如果安装了该软件包,请在安装前确保将其删除 `python3-rosdep`.

如果你在用的话 `rosdep` 在ROS之外,系统软件包可能不可用。在这种情况下,您可以直接从 <https://pypi.org>:

``` console
$ pip install rosdep
```

<span id="rosdep-operation"></span>

### 罗斯德操作

现在,我们有些理解 `rosdep`, `package.xml`,以及 `rosdistro`首先,如果这是第一次使用 `rosdep`,必须通过以下方式初始化:

``` console
$ sudo rosdep init
$ rosdep update
```

这将初始化 rosdep 和 `update` 将更新本地缓存的 rodistro 索引。 `update` 罗斯德普偶尔会得到最新的索引.

我们终于可以跑步了 `rosdep install` 以安装依赖性。通常情况下,它会运行在一个工作空间上,在一个单一调用中包含许多软件包,以安装所有依赖性。如果在工作空间的根部有目录,这样的调用将显示如下: `src` 包含源代码。

``` console
$ rosdep install --from-paths src -y --ignore-src
```

打破了这一点:

- `--from-paths src` 指定要检查的路径 `package.xml` 用于解析密钥的文件

- `-y` 表示默认是, 以不提示即从软件包管理器安装到所有提示

- `--ignore-src` 表示忽略安装依赖性,即使存在一个rosdep密钥,如果包本身也在工作空间中.

还有其他参数和选项。 使用 `rosdep -h` ,或查看罗斯德更完整的文档 at <http://docs.ros.org/en/independent/api/rosdep/html/> .

<span id="id2"></span>

\[[1](#id1)\]

“释放到ROS生态系统”是指该包被列在其中一种或多种 `<distro>/distribution.yaml` 目录 [rodistro 数据库](https://github.com/ros/rosdistro).
