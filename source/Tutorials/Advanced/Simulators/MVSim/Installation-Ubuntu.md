<span id="installation-ubuntu"></span>
<span id="background"></span>
<span id="prerequisites"></span>
<span id="tasks"></span>
<span id="install-mvsim"></span>
<span id="verify-the-installation"></span>
<span id="launch-a-demo"></span>
<span id="summary"></span>

# 安装（Ubuntu）

**目标：** 在 Ubuntu 上安装 `mvsim` 软件包，并验证它能正常工作。

**教程级别：** 高级

**耗时：** 10 分钟

## 背景

[MVSim](https://mvsimulator.readthedocs.io/)（MultiVehicle Simulator）是面向移动机器人的轻量级开源仿真器。它提供基于物理的二维仿真和三维可视化，支持差速驱动与阿克曼转向车辆、多种传感器（激光雷达、摄像头、IMU、GPS），并通过标准消息类型原生集成 ROS 2。

MVSim 使用 BSD 三条款许可证。

## 前提条件

建议先了解[入门教程](../../../../Tutorials.md)中的 ROS 基本原理，尤其是[创建工作空间](../../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md)。

你需要一个可正常使用的 ROS 2 安装环境。如有需要，请按照 [ROS 2 安装说明](../../../../Installation.md)进行安装。

## 任务

### 1 安装 `mvsim`

可以安装已发布的二进制软件包，也可以从源码构建。

#### 安装 ROS 二进制软件包

在终端运行：

```console
$ sudo apt install ros-rolling-mvsim
```

#### 从源码构建

如果还没有 ROS 2 工作空间，先创建一个：

```console
$ mkdir -p ~/ros2_ws/src
```

加载 ROS 2 环境：

```console
$ source /opt/ros/rolling/setup.bash
```

克隆 MVSim 仓库：

```console
$ cd ~/ros2_ws/src
$ git clone https://github.com/MRPT/mvsim.git --recursive
```

使用 `rosdep` 安装依赖项：

```console
$ cd ~/ros2_ws
$ rosdep install --from-paths src --ignore-src -r -y
```

构建软件包：

```console
$ colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```

加载工作空间：

```console
$ source install/setup.bash
```

### 2 验证安装

检查 `mvsim` 命令行工具是否可用：

```console
$ mvsim --version
```

终端应输出已安装的版本号。

!!! warning "警告"

    `mvsim` 软件包提供两个可执行程序：

    - `mvsim`：独立运行仿真器的主命令行工具。
    - `mvsim_node`：ROS 2 节点封装，用于运行仿真器并连接其他 ROS 2 节点。

### 3 启动演示

使用 ROS 2 启动仓库演示，快速检查安装是否正常：

```console
$ ros2 launch mvsim demo_warehouse.launch.py
```

MVSim 图形界面应打开，显示仓库环境中的 Jackal 机器人。使用键盘 W/A/S/D 键驾驶机器人。

## 总结

你已安装 MVSim，并通过启动演示世界验证它能正常运行。下一教程将介绍如何启动不同的演示场景，并通过 ROS 2 话题与它们交互。
