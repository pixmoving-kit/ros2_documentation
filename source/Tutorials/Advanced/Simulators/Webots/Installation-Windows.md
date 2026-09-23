---
translation_status: machine_translated
source: Tutorials/Advanced/Simulators/Webots/Installation-Windows.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="installation-windows"></span>

# 安装（Windows）

**目标：** 安装 `webots_ros2` 包和运行Windows上的模拟示例。

**教程级别：** 高级

**用时：** 10分钟

<span id="background"></span>

## 背景

那个... `webots_ros2` 软件包提供了ROS 2 和 Webots 之间的接口。它包括几个子软件包,包括 `webots_ros2_driver`,它允许ROS节点与 Webots 通信。其他子软件包主要是实例,可以显示使用接口的多个可能的实现。在此教程中,您要安装软件包并学习如何运行其中的一个实例。

<span id="prerequisites"></span>

## 前提条件

建议理解初学者所包括的基本ROS原则。 [教程](../../../../Tutorials.md)特别是, [创建工作空间](../../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md) 财务报告和财务报告 [创建软件包](../../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md) 是有用的先决条件。

Webots是使用 `webots_ros2` 软件包。您可以跟随 [安装程序](https://cyberbotics.com/doc/guide/installation-procedure) 或 时 间 [从源头构建它](https://github.com/cyberbotics/webots/wiki/Windows-installation/).

或者,你也可以让 `webots_ros2` 自动下载 Webots。当您启动软件包实例时,此选项会出现,但没有找到 Webots 安装。

<span id="multiple-installations-of-webots"></span>

### 多个 Webots 安装

如果您安装了多个 Webots, ROS 2 将在以下地点寻找 Webots( 按此顺序):

1.  如果说 `ROS2_WEBOTS_HOME` 设置环境变量, ROS 2 将使用此文件夹中的 Webots, 不管其版本如何 。

2.  如果说 `WEBOTS_HOME` 设置环境变量, ROS 2 将使用此文件夹中的 Webots, 不管其版本如何 。

3.  如果前几个点没有设置/安装 ROS 2, 将会在默认的安装路径中寻找Webots, 以获取一个兼容的版本 : `C:\Program Files\Webots`.

4.  如果找不到Webots, `webots_ros2` 将显示一个窗口,并提供最后一个兼容版本的自动Webots安装。

<span id="tasks"></span>

## 操作步骤

<span id="install-wsl2"></span>

### 1 安装 WSL2

在Windows上,WSL(Windows Subsystem for Linux)在Linux平台运行时,比起本地的Windows安装,改进了ROS 2的用户体验. 安装WSL的Ubuntu版本与您的ROS发行兼容,并在之后升级到WSL2 [微软官方教程](https://learn.microsoft.com/en-us/windows/wsl/install).

<span id="install-ros-2-in-wsl"></span>

### 2 在 WSL 安装 ROS 2

在 Ubuntu WSL 内部安装 ROS 2, 如下 [Ubuntu（deb 软件包）](../../../../Installation/Ubuntu-Install-Debs.md).

<span id="install-webots-ros2"></span>

### 3 安装 `webots_ros2`

然后可以安装 `webots_ros2` 从官方发布的软件包中安装,或从最新来源安装。 [吉图布](https://github.com/cyberbotics/webots_ros2).

以下命令必须在WSL环境内运行.

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

### 发射 `webots_ros2_universal_robot` 实例

WSL 不支持硬件加速( yet) 。 因此, Webots 应该在 Windows 上启动, 而 ROS 部分则在 WSL 内运行。 要做到这一点, 以下命令必须在 WSL 环境内运行 。

第一个源 ROS 2 环境,如果还没有完成的话。

``` console
$ source /opt/ros/rolling/setup.bash
```

设置 `WEBOTS_HOME` 环境变量允许您启动特定的 Webots 安装(例如. `C:\Program Files\Webots`。使用挂载点“/mnt”来指代本地Windows上的路径。

``` console
$ export WEBOTS_HOME=/mnt/c/Program\ Files/Webots
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

<span id="rviz-troubleshooting"></span>

### 5 RViz 排除故障

随着最近版本的WSL2,RViz应该从框中解脱出来.

您可以通过运行任何使用 RViz 的示例来检查它是否正确 :

``` console
$ sudo apt install ros-rolling-slam-toolbox
$ ros2 launch webots_ros2_tiago robot_launch.py rviz:=true slam:=true
```

提亚戈机器人可以使用:

``` console
$ ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

随着更古老的WSL版本,由于无法显示,RViz2可能无法直接工作. 要使用RViz,可以升级WSL或者启用X11转发.

##### 升级 WSL

在 Windows 外壳中:

``` console
$ wsl --update
```

##### 启用 X11 转发

对于较旧版本的WSL,可遵循以下步骤:

1.  安装 [VcXsrv 维基百科中的相关条目: 维基语录链接:名人名言](https://sourceforge.net/projects/vcxsrv/).

2.  启动 VcXsrv。您可以离开大多数参数默认值,但 `Extra settings` 页面,您必须设置的位置 `Clipboard`, `Primary Selection` 财务报告和财务报告 `Disable access control` 和未设置 `Native opengl`.

3.  可以保存配置用于未来的发射.

4.  点击 `Finish`,您可以看到 X11 服务器在图标托盘中运行。

5.  在您的 WSL 环境中,导出 `DISPLAY` 变量。

    > ``` console
    > $ export DISPLAY=$(ip route list default | awk '{print }'):0
    > ```
    >
    > 你可以把这个加进你的 `.bashrc`,这样它就可以为未来的WSL环境设定.
    >
    > ``` console
    > $ echo "export DISPLAY=$(ip route list default | awk '{print }'):0" >> ~/.bashrc
    > ```
