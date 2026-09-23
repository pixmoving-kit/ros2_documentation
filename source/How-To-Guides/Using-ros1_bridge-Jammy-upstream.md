---
translation_status: machine_translated
source: How-To-Guides/Using-ros1_bridge-Jammy-upstream.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-ros1-bridge-with-upstream-ros-on-ubuntu-22-04"></span>

# 使用( E) `ros1_bridge` 在Ubuntu 22.04上加上上游ROS

在Ubuntu 22.04上发布的ROS 2 Humble(和滚动)标志着第一个ROS 2在平台上发布,没有正式的ROS 1发布. ROS 1 Noetic将在其持续期间继续得到支持. [长期支持窗口](https://reps.openrobotics.org/rep-0003/#noetic-ninjemys-may-2020---may-2025),它只针对Ubuntu 20.04。 [ROS 1包的上游变体](https://packages.ubuntu.com/jammy/ros-desktop) 在Debian和Ubuntu中,由ROS维护者不作为官方发行。

本指南概述了目前在Ubuntu 22.04 Jammy Jellyfish上用这些上游软件包连接ROS 2发行机的机制。 这为仍然依赖ROS 1但渴望移动到较新的ROS 2和Ubuntu发行机的用户提供了一个迁移路径。

<span id="ros-2-via-deb-packages"></span>

## ROS 2 通过Deb 包

安装 [Deb 软件包中的 ROS 2](../Installation/Ubuntu-Install-Debs.md) 目前对 Ubuntu Jammy 的 ROS 2 不工作。 `catkin-pkg-modules` 可用 Ubuntu 寄存器与ROS 2 软件包寄存器中的相冲突 。

如果 ROS 2 pt 存储器位于可用的 apt 存储器中 (`/etc/apt/sources.list.d`),没有 ROS 1 软件包可以安装。错误是:

``` console
$ apt install ros-core-dev
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 ros-core-dev : Depends: catkin but it is not installable
E: Unable to correct problems, you have held broken packages.
```

要纠正这一点,请从您的软件包中移除 packages.ros.org `sources.list`。如果您遵循 ROS 2 安装指南,则只需删除 `/etc/apt/sources.list.d/ros2.list`

现在,要支持 `ros1_bridge`,遵循以下指令从源头构建ROS 2。

<span id="ros-2-from-source"></span>

## 来源:ROS 2

安装 [资料来源:ROS 2](../Installation/Alternatives/Ubuntu-Development-Setup.md) 是在Ubuntu Jammy上工作的唯一配置。

下面是源代码构建指令的必要指令摘要。 实质性的偏差是, 我们跳过使用 ROS 2 的 pt 寄存器, 因为软件包相互冲突 。

<span id="install-development-tools-and-ros-tools"></span>

### 安装开发工具和ROS工具

因为我们没有使用 ROS 2 的储物箱, `colcon` 必须通过 `pip`.

``` console
$ sudo apt update && sudo apt install -y \
  build-essential \
  cmake \
  git \
  python3-flake8 \
  python3-flake8-blind-except \
  python3-flake8-builtins \
  python3-flake8-class-newline \
  python3-flake8-comprehensions \
  python3-flake8-deprecated \
  python3-flake8-docstrings \
  python3-flake8-import-order \
  python3-flake8-quotes \
  python3-pip \
  python3-pytest \
  python3-pytest-cov \
  python3-pytest-repeat \
  python3-pytest-rerunfailures \
  python3-rosdep \
  python3-setuptools \
  wget

# Install colcon from PyPI, rather than apt packages
python3 -m pip install -U colcon-common-extensions vcstool
```

从这里开始,继续 [源安装指南](../Installation/Alternatives/Ubuntu-Development-Setup.md) 以建造 ROS 2 。

<span id="install-ros-1-from-ubuntu-packages"></span>

### 从 Ubuntu 软件包安装 ROS 1

``` console
$ sudo apt update && sudo apt install -y ros-core-dev
```

<span id="build-ros1-bridge"></span>

### 构建 `ros1_bridge`

``` console
$ mkdir -p ~/ros1_bridge/src # Create a workspace for the ros1_bridge
$ cd ~/ros1_bridge/src
$ git clone https://github.com/ros2/ros1_bridge
$ cd ~/ros1_bridge
$. ~/ros2_humble/install/local_setup.bash # Source the ROS 2 workspace
$ colcon build # Build
```

盖完所有房子后 `ros1_bridge`,剩余部分 [ros1\_ 桥式示例](https://github.com/ros2/ros1_bridge#example-1-run-the-bridge-and-the-example-talker-and-listener) 应当与您的新安装工作
