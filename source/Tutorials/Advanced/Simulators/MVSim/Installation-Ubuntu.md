---
translation_status: machine_translated
source: Tutorials/Advanced/Simulators/MVSim/Installation-Ubuntu.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="installation-ubuntu"></span>

# 安装（Ubuntu）

**目标：** 安装 `mvsim` 在 Ubuntu 上的软件包并验证它是否有效。

**教程级别：** 高级

**用时：** 10分钟

<span id="background"></span>

## 背景

[MVSim 游戏](https://mvsimulator.readthedocs.io/) (MultiVehicle Simulator)是移动机器人的一种轻量级的开源模拟器,它通过3D可视化提供基于2D物理的模拟,支持差分驱动和Ackermann车辆,多种传感器类型(LiDAR,相机,IMU,GPS),并通过标准消息类型实现本地ROS 2集成.

MVSim根据BSD 3-clause许可证获得许可.

<span id="prerequisites"></span>

## 前提条件

建议理解初学者所包括的基本ROS原则。 [教程](../../../../Tutorials.md)特别是, [创建工作空间](../../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md) 这是一个有用的先决条件。

你应该有一个工作 ROS 2 的安装。 [ROS 2 安装指令](../../../../Installation.md) 如果需要的话。

<span id="tasks"></span>

## 操作步骤

<span id="install-mvsim"></span>

### 1 安装 `mvsim`

您可以安装释放的二进制包, 或者从源头构建 。

##### 从 ROS 二进制包安装

在终端中运行以下命令 :

``` console
$ sudo apt install ros-rolling-mvsim
```

##### 从源头建构

如果您还没有工作空间, 则创建 ROS 2 工作空间 :

``` console
$ mkdir -p ~/ros2_ws/src
```

来源: ROS 2 环境:

``` console
$ source /opt/ros/rolling/setup.bash
```

克隆MVSim寄存器 :

``` console
$ cd ~/ros2_ws/src
$ git clone https://github.com/MRPT/mvsim.git --recursive
```

使用 `rosdep`:

``` console
$ cd ~/ros2_ws
$ rosdep install --from-paths src --ignore-src -r -y
```

构建软件包 :

``` console
$ colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

工作空间来源 :

``` console
$ source install/setup.bash
```

<span id="verify-the-installation"></span>

### 2 核查安装

检查一下 `mvsim` 现有国家学习课程:

``` console
$ mvsim --version
```

您应该看到已安装的版本编号被打印到终端.

> **警告**
>
> 那个... `mvsim` 软件包提供了两个可执行文件:
>
> - `mvsim`: 用于独立运行模拟器的主要 CLI 工具
>
> - `mvsim_node`: 用于运行模拟器并连接到其他ROS 2 节点的ROS 2 节点包

<span id="launch-a-demo"></span>

### 3 启动演示

为了快速核实一切正常, 启动仓库演示 与ROS 2 :

``` console
$ ros2 launch mvsim demo_warehouse.launch.py
```

您应该看到 MVSim GUI 窗口在仓库环境中用 Jackal 机器人打开。 使用键盘( W/ A/ S/ D 键) 来驱动机器人 。

<span id="summary"></span>

## 小结

您已经安装了 MVSim, 并且通过启动演示世界来验证它的工作效果。 在下一个教程中, 您将学习如何启动不同的演示情景, 并通过ROS 2 主题与之互动 。
