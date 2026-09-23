<span id="using-ros1-bridge-with-upstream-ros-on-ubuntu-22-04"></span>
# 在 Ubuntu 22.04 上将 ros1_bridge 与系统仓库中的 ROS 配合使用

ROS 2 Humble（以及 Rolling）在 Ubuntu 22.04 Jammy Jellyfish 上发布，标志着 ROS 2 首次支持一个没有官方 ROS 1 发行版的平台。ROS 1 Noetic 会在其[长期支持周期](https://reps.openrobotics.org/rep-0003/#noetic-ninjemys-may-2020---may-2025)内继续获得支持，但仅面向 Ubuntu 20.04。另一种选择是 Debian 和 Ubuntu 提供的 [ROS 1 软件包变体](https://packages.ubuntu.com/jammy/ros-desktop)，这些软件包并非由 ROS 维护者作为官方发行版维护。

本指南介绍在 Ubuntu 22.04 Jammy Jellyfish 上，将 ROS 2 发行版与这些系统仓库软件包桥接的方法。这为仍依赖 ROS 1、但希望迁移到较新 ROS 2 和 Ubuntu 版本的用户提供了一条迁移路径。

<span id="ros-2-via-deb-packages"></span>
## 通过 deb 软件包安装 ROS 2

在 Ubuntu Jammy 上，当前无法通过[安装 ROS 2 deb 软件包](../Installation/Ubuntu-Install-Debs.md)来实现上述配置。Ubuntu 仓库提供的 `catkin-pkg-modules` 版本与 ROS 2 软件包仓库中的版本存在冲突。

如果可用的 apt 仓库列表（`/etc/apt/sources.list.d`）中包含 ROS 2 apt 仓库，就无法安装 ROS 1 软件包。错误如下：

```console
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

要解决此问题，需要从 `sources.list` 中移除 packages.ros.org。如果此前遵循 ROS 2 安装指南进行配置，只需删除 `/etc/apt/sources.list.d/ros2.list`。

目前，要支持 `ros1_bridge`，请按照下文从源码构建 ROS 2。

<span id="ros-2-from-source"></span>
## 从源码构建 ROS 2

在 Ubuntu Jammy 上，[从源码安装 ROS 2](../Installation/Alternatives/Ubuntu-Development-Setup.md) 是唯一可以实现上述配置的方式。

下面概括了源码构建指南中的必要步骤。主要区别是：为避免软件包冲突，不使用 ROS 2 apt 仓库。

<span id="install-development-tools-and-ros-tools"></span>
### 安装开发工具和 ROS 工具

由于不使用 ROS 2 apt 仓库，必须通过 `pip` 安装 `colcon`。

```console
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

从这里开始，继续按照[源码安装指南](../Installation/Alternatives/Ubuntu-Development-Setup.md)构建 ROS 2。

<span id="install-ros-1-from-ubuntu-packages"></span>
### 从 Ubuntu 软件包安装 ROS 1

```console
$ sudo apt update && sudo apt install -y ros-core-dev
```

<span id="build-ros1-bridge"></span>
### 构建 ros1_bridge

```console
$ mkdir -p ~/ros1_bridge/src # Create a workspace for the ros1_bridge
$ cd ~/ros1_bridge/src
$ git clone https://github.com/ros2/ros1_bridge
$ cd ~/ros1_bridge
$. ~/ros2_humble/install/local_setup.bash # Source the ROS 2 workspace
$ colcon build # Build
```

完成 `ros1_bridge` 的构建后，其余 [ros1_bridge 示例](https://github.com/ros2/ros1_bridge#example-1-run-the-bridge-and-the-example-talker-and-listener)应可在新安装的环境中运行。
