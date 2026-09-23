---
translation_status: machine_translated
source: Installation/Alternatives/RHEL-Development-Setup.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="rhel-source"></span> <span id="rhel-latest"></span>

# RHEL（源码安装）

<span id="system-requirements"></span>

## 系统要求

目前为滚车Ridley提供的目标红帽平台是:

- 二级: RHEL 8 64 位

定义 [REP 2000 环境方案](https://reps.openrobotics.org/rep-2000/).

<span id="system-setup"></span>

## 系统设置

<span id="set-locale"></span>

### 设置区域

确定您有支持的地址 `UTF-8`。如果您处于一个最小的环境(例如一个插座容器),那么当地可能就是最小的环境,比如: `C`。我们用以下设置进行测试。但是,如果您使用不同的UTF-8支持的语境,则应该没问题。

``` console
$ locale  # check for UTF-8

$ sudo dnf install langpacks-en glibc-langpack-en
$ export LANG=en_US.UTF-8

$ locale  # verify settings
```

<span id="enable-required-repositories"></span>

### 启用所需的寄存器

rosdep 数据库包含来自 IPEL 和 PowerTools 寄存器的软件包, 默认无法启用。 它们可以通过运行来启用 :

``` console
$ sudo dnf install 'dnf-command(config-manager)' epel-release -y
$ sudo dnf config-manager --set-enabled powertools
```

> **说明**
>
> 这个步骤可能因您使用的分布而略有不同 。 [检查 EPEL 文档](https://docs.fedoraproject.org/en-US/epel/#_quickstart)

<span id="install-development-tools-and-ros-tools"></span>

### 安装开发工具和ROS工具

``` console
$ sudo dnf install -y \
  cmake \
  gcc-c++ \
  git \
  make \
  patch \
  python3-colcon-common-extensions \
  python3-pip \
  python3-pydocstyle \
  python3-pytest \
  python3-pytest-repeat \
  python3-pytest-rerunfailures \
  python3-rosdep \
  python3-setuptools \
  python3-vcstool

~ install some pip packages needed for testing and
~ not available as RPMs
$ python3 -m pip install -U --user \
  flake8-blind-except==0.1.1 \
  flake8-builtins \
  flake8-class-newline \
  flake8-comprehensions \
  flake8-deprecated \
  flake8-docstrings \
  flake8-import-order==0.18.2 \
  flake8-quotes \
  mypy==0.931
```

<span id="get-ros-2-code"></span> <span id="rhel-dev-get-ros2-code"></span>

## 获取 ROS 2 代码

创建工作空间并复制全部重置 :

``` console
$ mkdir -p ~/ros2_rolling/src
$ cd ~/ros2_rolling
$ vcs import --input https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos src
```

<span id="install-dependencies-using-rosdep"></span> <span id="rhel-development-setup-install-dependencies-using-rosdep"></span>

## 使用 rosdep 安装依赖性

ROS 2 软件包建立在经常更新的 RHEL 系统上。总是建议您在安装新软件包之前确保您的系统更新。

``` console
$ sudo dnf update
```

``` console
$ sudo rosdep init
$ rosdep update
$ rosdep install --from-paths src --ignore-src -y --skip-keys "asio cyclonedds fastcdr fastrtps ignition-cmake2 ignition-math6 python3-babeltrace python3-mypy rti-connext-dds-6.0.1 urdfdom_headers"
```

<span id="install-additional-dds-implementations-optional"></span>

## 安装额外的 DDS 执行( 可选)

如果您想要在默认之外使用另一个 DDS 或 RTPS 供应商, 您可以找到指令 [这儿](../RMW-Implementations.md).

<span id="build-the-code-in-the-workspace"></span>

## 在工作空间中构建代码

如果您已经安装了 ROS 2 另一种方式( 通过 RPM 或二进制分布), 请确保您在没有其他设备源的新环境中运行下面的命令 。 同时确保您没有 `source /opt/ros/${ROS_DISTRO}/setup.bash` 在您的帐号中 `.bashrc`。您可以确保ROS 2没有命令源代码 `printenv | grep -i ROS`。输出应为空的。

关于与ROS工作空间合作的更多信息,请访问 [此教程](../../Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.md).

``` console
$ cd ~/ros2_rolling/
$ colcon build --symlink-install --cmake-args -DTHIRDPARTY_Asio=ON -DPython3_EXECUTABLE=/usr/bin/python3 --no-warn-unused-cli
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

<span id="try-some-examples"></span> <span id="rhel-talker-listener"></span>

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

<span id="additional-rmw-implementations-optional"></span>

## 更多落实RMW(可选)

ROS 2 使用的默认中间软件是 `Fast DDS`,但中间软件(RMW)可以在运行时替换。 [指南](../../How-To-Guides/Working-with-multiple-RMW-implementations.md) 如何与多个RMW合作。

<span id="alternate-compilers"></span>

## 其它编译器

除 gcc 外, 使用另一个编译器来编译 ROS 2 很容易。 如果您设置了环境变量 `CC` 财务报告和财务报告 `CXX` 以分别用于正在工作的 C 和 C++ 编译器的可执行文件, 并重新触发 CMake 配置( 通过使用 `--force-cmake-config` CMake将重新配置和使用不同的编译器。

<span id="clang"></span>

### 弯曲

要配置 CMake 来检测和使用 Clang :

``` console
$ sudo dnf install clang
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
