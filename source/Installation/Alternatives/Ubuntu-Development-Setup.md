---
translation_status: machine_translated
source: Installation/Alternatives/Ubuntu-Development-Setup.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="ubuntu-source"></span> <span id="linux-latest"></span>

# Ubuntu（源码安装）

<span id="system-requirements"></span>

## 系统要求

目前基于Debian的目标平台为"滚滚"(Rolling Ridley).

- 一级: Ubuntu Linux - Jammy (22.04) 64位

- 第3级:Ubuntu Linux - 焦距(20.04) 64位

- 第3级:Debian Linux - 公牛(11) 64位

支持程度不同的其他Linux平台包括:

- Arch Linux, 见 [备选指令](https://wiki.archlinux.org/index.php/ROS#ROS_2)

- 费多拉·利努,看 [备选指令](Fedora-Development-Setup.md)

- OpenEmbed / webOS OSE, 见 [备选指令](https://github.com/ros/meta-ros/wiki/OpenEmbedded-Build-Instructions)

定义 [REP 2000 环境方案](https://reps.openrobotics.org/rep-2000/).

<span id="system-setup"></span>

## 系统设置

<span id="set-locale"></span>

### 设置区域

确定您有支持的地址 `UTF-8`。如果您处于一个最小的环境(例如一个插座容器),那么当地可能就是最小的环境,比如: `POSIX`。我们用以下设置进行测试。但是,如果您使用不同的UTF-8支持的语境,则应该没问题。

``` console
$ locale  # check for UTF-8

$ sudo apt update && sudo apt install locales
$ sudo locale-gen en_US en_US.UTF-8
$ sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
$ export LANG=en_US.UTF-8

$ locale  # verify settings
```

<span id="add-the-ros-2-apt-repository"></span>

### 添加 ROS 2 pt 存储器

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

<span id="install-development-tools-and-ros-tools"></span>

### 安装开发工具和ROS工具

安装常见的软件包 。

``` console
$ sudo apt update && sudo apt install -y \
  python3-flake8-docstrings \
  python3-pip \
  python3-pytest-cov \
  ros-dev-tools
```

根据您的 Ubuntu 版本安装软件包 。

##### Ubuntu 22.04 LTS 以及后来

``` console
$ sudo apt install -y \
   python3-flake8-blind-except \
   python3-flake8-builtins \
   python3-flake8-class-newline \
   python3-flake8-comprehensions \
   python3-flake8-deprecated \
   python3-flake8-import-order \
   python3-flake8-quotes \
   python3-pytest-repeat \
   python3-pytest-rerunfailures
```

##### Ubuntu 20.04 LTS (英语).

``` console
$ python3 -m pip install -U \
   flake8-blind-except \
   flake8-builtins \
   flake8-class-newline \
   flake8-comprehensions \
   flake8-deprecated \
   flake8-import-order \
   flake8-quotes \
   "pytest>=5.3" \
   pytest-repeat \
   pytest-rerunfailures \
   empy==3.3.4
```

<span id="get-ros-2-code"></span> <span id="linux-dev-get-ros2-code"></span>

## 获取 ROS 2 代码

创建工作空间并复制全部重置 :

``` console
$ mkdir -p ~/ros2_rolling/src
$ cd ~/ros2_rolling
$ vcs import --input https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos src
```

<span id="install-dependencies-using-rosdep"></span> <span id="linux-development-setup-install-dependencies-using-rosdep"></span>

## 使用 rosdep 安装依赖性

ROS 2 软件包建立在经常更新的 Ubuntu 系统上。总是建议您在安装新软件包之前确保您的系统是最新的。

``` console
$ sudo apt upgrade
```

``` console
$ sudo rosdep init
$ rosdep update
$ rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-6.0.1 urdfdom_headers"
```

**说明**: 如果您使用基于 Ubuntu( 类似 Linux Mint) 的分布, 但没有表明您的身份, 您将会收到一个错误消息, 如 `Unsupported OS [mint]`。在这种情况下,附件 `--os=ubuntu:jammy` 给上面的命令。

<span id="install-additional-dds-implementations-optional"></span>

## 安装额外的 DDS 执行( 可选)

如果您想要在默认之外使用另一个 DDS 或 RTPS 供应商, 您可以找到指令 [这儿](../RMW-Implementations.md).

<span id="install-colcon-mixins"></span>

### 安装曲折混音器

``` console
$ colcon mixin add default https://github.com/colcon/colcon-mixin-repository/raw/master/index.yaml
$ colcon mixin update default
```

<span id="build-the-code-in-the-workspace"></span>

## 在工作空间中构建代码

如果您已经安装了 ROS 2 另一种方式( 无论是通过 debs 或二进制分配), 请确保您在新的环境中运行以下命令, 并且没有其它设备的源代码 。 `source /opt/ros/${ROS_DISTRO}/setup.bash` 在您的帐号中 `.bashrc`。您可以确保ROS 2没有命令源代码 `printenv | grep -i ROS`。输出应为空的。

关于与ROS工作空间合作的更多信息,请访问 [此教程](../../Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.md).

``` console
$ cd ~/ros2_rolling/
$ colcon build --symlink-install --mixin release
```

注意: 如果您无法汇编所有实例, 并且这阻碍了您完成一个成功的构建, 您可以使用 。 `COLCON_IGNORE` 以同样方式 [CATKIN_IGNORE](https://github.com/ros-infrastructure/rep/blob/master/rep-0128.rst) 以忽略子树或从工作空间中删除文件夹。例如,您想要避免安装大型的 OpenCV 库。那么只需运行 `touch COLCON_IGNORE` 输入 `cam2image` 演示目录将其排除在构建进程之外 。

<span id="environment-setup"></span>

## 环境设置

<span id="source-the-setup-script"></span>

### 来源设置脚本

通过获取以下文件来设置您的环境 。

``` console
$ . ~/ros2_rolling/install/local_setup.bash
```

> **说明**
>
> 替换 `.bash` 如果您没有使用 bash , 请使用您的外壳 。 可能的值是 : `setup.bash`, `setup.sh`, `setup.zsh`.

<span id="try-some-examples"></span> <span id="talker-listener"></span>

## 尝试一些例子

在一个终端中, 源代码设置文件, 然后运行 C++ `talker`:

``` console
$ . ~/ros2_rolling/install/local_setup.bash
$ ros2 run demo_nodes_cpp talker
```

在另一个终端源代码中, 设置文件然后运行 Python `listener`:

``` console
$ . ~/ros2_rolling/install/local_setup.bash
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

<span id="alternate-compilers"></span>

## 其它编译器

除 gcc 外, 使用另一个编译器来编译 ROS 2 很容易。 如果您设置了环境变量 `CC` 财务报告和财务报告 `CXX` 以分别用于正在工作的 C 和 C++ 编译器的可执行文件, 并重新触发 CMake 配置( 通过使用 `--cmake-force-configure` CMake将重新配置和使用不同的编译器。

<span id="clang"></span>

### 弯曲

要配置 CMake 来检测和使用 Clang :

``` console
$ sudo apt install clang
$ export CC=clang
$ export CXX=clang++
$ colcon build --cmake-force-configure
```

<span id="stay-up-to-date"></span>

## 保持最新进展

见 [维护源码工作副本](../Maintaining-a-Source-Checkout.md) 以定期刷新您的源安装。

<span id="troubleshooting"></span>

## 麻烦的解决

可以找到解决问题的技巧 [这儿](../../How-To-Guides/Installation-Troubleshooting.md#linux-troubleshooting).

<span id="uninstall"></span>

## 卸载

1.  如果您按照上述指示将工作空间安装在colcon上, " 完全取消 " 可能只是打开一个新的终端,而不是提供工作空间。 `setup` 文件。这样,您的环境将表现为没有在您的系统中安装滚动。

2.  如果您还试图腾出空间, 您可以删除整个工作空间目录 :

    ``` console
    $ rm -rf ~/ros2_rolling
    ```
