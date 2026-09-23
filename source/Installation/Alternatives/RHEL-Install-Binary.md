---
translation_status: machine_translated
source: Installation/Alternatives/RHEL-Install-Binary.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="rhel-binary"></span>

# RHEL（二进制安装）

本页面解释如何从一个预建的二进制包在RHEL上安装ROS 2.

> **说明**
>
> 预建的二进制不包含所有 ROS 2 套件。所有套件都在 [ROS 基变型](https://reps.openrobotics.org/rep-2001/#ros-base) 包含,并且只包含在 [ROS 桌面变体](https://reps.openrobotics.org/rep-2001/#desktop-variants) 包的确切清单由以下所列寄存器描述: [此 ros2. repos 文件](https://github.com/ros2/ros2/blob/rolling/ros2.repos).

还有 [RPM 软件包](../RHEL-Install-RPMs.md) 备有.

<span id="system-requirements"></span>

## 系统要求

目前支持RHEL 8 64位.

<span id="enable-required-repositories"></span>

## 启用所需的寄存器

rosdep 数据库包含来自 IPEL 和 PowerTools 寄存器的软件包, 默认无法启用。 它们可以通过运行来启用 :

``` console
$ sudo dnf install 'dnf-command(config-manager)' epel-release -y
$ sudo dnf config-manager --set-enabled powertools
```

> **说明**
>
> 这个步骤可能因您使用的分布而略有不同 。 [检查 EPEL 文档](https://docs.fedoraproject.org/en-US/epel/#_quickstart)

<span id="installing-prerequisites"></span>

## 安装前置依赖

有一些软件包必须安装才能获得和解锁二进制释放.

``` console
$ sudo dnf install tar bzip2 wget -y
```

<span id="downloading-ros-2"></span>

## 正在下载 ROS 2

- 转到 [新闻稿页面](https://github.com/ros2/ros2/releases)

- 下载 RHEL 的最新软件包; 让我们假设它最终会是 `~/Downloads/ros2-package-linux-x86_64.tar.bz2`.

  - 注意: 可能有多个二进制下载选项, 可能导致文件名称不同 。

- 解开它:

  ``` console
  $ mkdir -p ~/ros2_rolling
  $ cd ~/ros2_rolling
  $ tar xf ~/Downloads/ros2-package-linux-x86_64.tar.bz2
  ```

<span id="installing-and-initializing-rosdep"></span>

## 安装和初始化 rosdep

``` console
$ sudo dnf install -y python3-rosdep
$ sudo rosdep init
$ rosdep update
```

<span id="installing-the-missing-dependencies"></span> <span id="rhel-install-binary-install-missing-dependencies"></span>

## 安装缺失的依赖关系

ROS 2 软件包建立在经常更新的 RHEL 系统上。总是建议您在安装新软件包之前确保您的系统更新。

``` console
$ sudo dnf update
```

根据您下载的发行量设定您的 rodistro 。

``` bash
rosdep install --from-paths ~/ros2_rolling/ros2-linux/share --ignore-src -y --skip-keys "asio cyclonedds fastcdr fastrtps ignition-cmake2 ignition-math6 python3-babeltrace python3-mypy rti-connext-dds-6.0.1 urdfdom_headers"
```

<span id="install-additional-dds-implementations-optional"></span>

### 安装额外的 DDS 执行( 可选)

如果您想要在默认之外使用另一个 DDS 或 RTPS 供应商, 您可以找到指令 [这儿](../RMW-Implementations.md).

<span id="environment-setup"></span>

## 环境设置

<span id="source-the-setup-script"></span>

### 来源设置脚本

通过获取以下文件来设置您的环境 。

``` console
$ . ~/ros2_rolling/ros2-linux/setup.bash
```

> **说明**
>
> 替换 `.bash` 如果您没有使用 bash , 请使用您的外壳 。 可能的值是 : `setup.bash`, `setup.sh`, `setup.zsh`.

<span id="try-some-examples"></span>

## 尝试一些例子

在一个终端中, 源代码设置文件, 然后运行 C++ `talker`:

``` console
$ . ~/ros2_rolling/ros2-linux/setup.bash
$ ros2 run demo_nodes_cpp talker
```

在另一个终端源代码中, 设置文件然后运行 Python `listener`:

``` console
$ . ~/ros2_rolling/ros2-linux/setup.bash
$ ros2 run demo_nodes_py listener
```

你应该看看 `talker` 说,这是 `Publishing` 信件和资料 `listener` 说 `I heard` 这证明C++和Python API都正常工作。万岁!

<span id="next-steps-after-installing"></span>

## 安装后的下一步

继续 [教程和演示](../../Tutorials.md) 来配置环境,创建自己的工作空间和软件包,并学习ROS 2核心概念。

<span id="additional-rmw-implementations-optional"></span>

## 更多落实RMW(可选)

ROS 2 使用的默认中间软件是 `Fast DDS`,但中间软件(RMW)可以在运行时替换。 [指南](../../How-To-Guides/Working-with-multiple-RMW-implementations.md) 如何与多个RMW合作。

<span id="troubleshooting"></span>

## 麻烦的解决

可以找到解决问题的技巧 [这儿](../../How-To-Guides/Installation-Troubleshooting.md).

<span id="uninstall"></span>

## 卸载

1.  如果您按照上述指示将工作空间安装在colcon上, " 完全取消 " 可能只是打开一个新的终端,而不是提供工作空间。 `setup` 文件。这样,您的环境将表现为没有在您的系统中安装滚动。

2.  如果您还试图腾出空间, 您可以删除整个工作空间目录 :

    ``` console
    $ rm -rf ~/ros2_rolling
    ```
