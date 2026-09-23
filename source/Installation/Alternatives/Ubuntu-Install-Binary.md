---
translation_status: machine_translated
source: Installation/Alternatives/Ubuntu-Install-Binary.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="ubuntu-binary"></span>

# Ubuntu（二进制安装）

本页面解释如何从一个预建的二进制包在Ubuntu Linux上安装ROS 2.

> **说明**
>
> 预建的二进制不包含所有 ROS 2 套件。所有套件都在 [ROS 基变型](https://reps.openrobotics.org/rep-2001/#ros-base) 包含,并且只包含在 [ROS 桌面变体](https://reps.openrobotics.org/rep-2001/#desktop-variants) 包的确切清单由以下所列寄存器描述: [此 ros2. repos 文件](https://github.com/ros2/ros2/blob/rolling/ros2.repos).

还有 [deb 软件包](../Ubuntu-Install-Debs.md) 备有.

<span id="system-requirements"></span>

## 系统要求

目前我们支持Ubuntu Linux Jammy (22.04) 64位x86和64位ARM.

<span id="add-the-ros-2-apt-repository"></span>

## 添加 ROS 2 pt 存储器

您需要将 ROS 2 apt 存储器添加到您的系统中 。

首先是确保 [Ubuntu 宇宙存储器](https://help.ubuntu.com/community/Repositories/Ubuntu) 已启用。

``` console
$ sudo apt install software-properties-common
$ sudo add-apt-repository universe
```

那个... [ros- apt 源码](https://github.com/ros-infrastructure/ros-apt-source/) 软件包为各种ROS寄存器提供密钥和适当的源配置。

安装 ros2- apt 源软件包将为您的系统配置 ROS 2 寄存器。 当此软件包的新版本发布到 ROS 寄存器时, 将自动更新到寄存器配置 。

``` console
$ sudo apt update && sudo apt install curl -y
$ export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F'"' '{print $4}')
$ curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
$ sudo dpkg -i /tmp/ros2-apt-source.deb
```

<span id="downloading-ros-2"></span>

## 正在下载 ROS 2

- 转到 [新闻稿页面](https://github.com/ros2/ros2/releases)

- 下载 Ubuntu 的最新软件包; 让我们假设它最终是 `~/Downloads/ros2-package-linux-x86_64.tar.bz2`.

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
$ sudo apt update
$ sudo apt install -y python3-rosdep
$ sudo rosdep init
$ rosdep update
```

<span id="installing-the-missing-dependencies"></span> <span id="linux-install-binary-install-missing-dependencies"></span>

## 安装缺失的依赖关系

ROS 2 软件包建立在经常更新的 Ubuntu 系统上。总是建议您在安装新软件包之前确保您的系统是最新的。

``` console
$ sudo apt upgrade
```

根据您下载的发行量设定您的 rodistro 。

``` bash
rosdep install --from-paths ~/ros2_rolling/ros2-linux/share --ignore-src -y --skip-keys "cyclonedds fastcdr fastrtps rti-connext-dds-6.0.1 urdfdom_headers"
```

**说明**: 如果您使用基于 Ubuntu( 类似 Linux Mint) 的分布, 但没有表明您的身份, 您将会收到一个错误消息, 如 `Unsupported OS [mint]`。在这种情况下,附件 `--os=ubuntu:jammy` 给上面的命令。

<span id="install-development-tools-optional"></span>

### 安装开发工具( 可选)

如果您要构建ROS软件包或以其他方式进行开发,您也可以安装开发工具:

``` bash
sudo apt install ros-dev-tools
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

<span id="using-the-ros-1-bridge"></span>

## 使用 ROS 1 桥

ROS 1 桥可以连接 ROS 1 和 ROS 2 的主题,反之亦然。 [文档](https://github.com/ros2/ros1_bridge/blob/master/README.md) 如何建造和使用 ROS 1 桥。

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
