---
translation_status: machine_translated
source: Tutorials/Advanced/Simulators/Webots/Installation-Ubuntu.rst
---

<span id="installation-ubuntu"></span>

# 安装（Ubuntu）

**目标：** 安装 `webots_ros2` 软件包和运行 Ubuntu 上的模拟示例。

**教程级别：** 高级

**用时：** 10分钟

<span id="background"></span>

## 背景

那个... `webots_ros2` 软件包提供了ROS 2 和 Webots 之间的接口。它包括几个子软件包,包括 `webots_ros2_driver`,它允许您启动 Webots 并与之通信。此界面被用于以下大多数教程,因此需要事先安装。其他子软件包主要是使用该界面显示多个可能执行的实例。在此教程中,您将安装该软件包并学习如何运行其中的一个实例。

<span id="prerequisites"></span>

## 前提条件

建议理解初学者所包括的基本ROS原则。 [教程](../../../../Tutorials.md)特别是, [创建工作空间](../../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md) 财务报告和财务报告 [创建软件包](../../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md) 是有用的先决条件。

应安装Webots软件,以便使用 `webots_ros2` 界面。您可以跟随 [安装程序](https://cyberbotics.com/doc/guide/installation-procedure) 或 时 间 [从源头构建它](https://github.com/cyberbotics/webots/wiki/Linux-installation/).

或者,你也可以让 `webots_ros2` 自动下载并安装 Webots。当您启动软件包实例时,此选项将出现,但没有找到 Webots 安装。

<span id="multiple-installations-of-webots"></span>

### 多个 Webots 安装

如果你在电脑上安装了不同版本的 Webots, `webots_ros2` 将在下列地点寻找 Webots(按此顺序):

1.  如果说 `ROS2_WEBOTS_HOME` 设置环境变量, ROS 2 将使用此文件夹中的 Webots, 不管其版本如何 。

2.  如果说 `WEBOTS_HOME` 设置环境变量, ROS 2 将使用此文件夹中的 Webots, 不管其版本如何 。

3.  如果这些变量没有设定的话, `webots_ros2` 将查找默认安装路径中的 Webots , 以获取兼容的版本 : `/usr/local/webots` 财务报告和财务报告 `/snap/webots/current/usr/share/webots`.

4.  如果找不到Webots, `webots_ros2` 将显示一个窗口,提供Webots最新兼容版本的自动安装。

<span id="tasks"></span>

## 操作步骤

<span id="install-webots-ros2"></span>

### 1 安装 `webots_ros2`

您可以安装官方发布的软件包,也可以从最新来源安装该软件包。 [吉图布](https://github.com/cyberbotics/webots_ros2).

##### 安装 Webots\_ ros2 分布式软件包

在终端中运行以下命令 。

``` console
$ sudo apt-get install ros-rolling-webots-ros2
```

##### 从源头安装webots_ros2

创建 ROS 2 工作空间 `src` 目录。

``` console
$ mkdir -p ~/ros2_ws/src
```

来源 ROS 2 环境.

``` console
$ source /opt/ros/rolling/setup.bash
```

从Github那里获取情报

``` console
$ cd ~/ros2_ws
$ git clone --recurse-submodules https://github.com/cyberbotics/webots_ros2.git src/webots_ros2
```

安装软件包的依赖性 。

``` console
$ sudo apt install python3-pip python3-rosdep python3-colcon-common-extensions
$ sudo rosdep init && rosdep update
$ rosdep install --from-paths src --ignore-src --rosdistro rolling
```

使用 `colcon`.

``` console
$ colcon build
```

源此工作空间 。

``` console
$ source install/local_setup.bash
```

<span id="launch-the-webots-ros2-universal-robot-example"></span>

### 2 发射 `webots_ros2_universal_robot` 实例

以下指示解释了如何开始一个已提供的例子。

第一个源 ROS 2 环境,如果还没有完成的话。

``` console
$ source /opt/ros/rolling/setup.bash
```

设置 `WEBOTS_HOME` 环境变量允许您启动特定的 Webots 安装 。

``` console
$ export WEBOTS_HOME=/usr/local/webots
```

如果安装了源头, 请源代码为 ROS 2 工作空间, 如果还没有完成的话 。

``` console
$ cd ~/ros2_ws
$ source install/local_setup.bash
```

使用ROS 2发射命令开始演示包(例如. `webots_ros2_universal_robot`).

``` console
$ ros2 launch webots_ros2_universal_robot multirobot_launch.py
```
