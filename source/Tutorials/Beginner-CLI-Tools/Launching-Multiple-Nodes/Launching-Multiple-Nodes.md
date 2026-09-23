---
translation_status: machine_translated
source: Tutorials/Beginner-CLI-Tools/Launching-Multiple-Nodes/Launching-Multiple-Nodes.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="launching-nodes"></span> <span id="ros2launch"></span>

# 启动节点

**目标：** 使用命令行工具立即启动多个节点.

**教学级别 :** 入门

**用时：** 5分钟

<span id="background"></span>

## 背景

在大多数入门教程中,您都在为您运行的每一个新节点打开新的终端。由于您创建了更加复杂的系统,越来越多的节点同时运行,打开终端和重新进入配置细节变得乏味。

启动文件允许您同时启动并配置包含 ROS 2 节点的若干可执行文件 。

运行一个带有 `ros2 launch` 命令将立即启动整个系统——所有节点及其配置。

<span id="prerequisites"></span>

## 前提条件

在开始这些教程前, 遵循 ROS 2 上的指示安装 ROS 2 [安装](../../../Installation.md) 页面。

此教程中使用的命令假设您遵循操作系统的二进制包安装指南( 用于 Linux 的 DEb 包) 。 如果您从源头构建的话, 您仍然可以遵循, 但是您的设置文件的路径可能不同 。 您也将无法使用 。 `sudo apt install ros-<distro>-<package>` 如果从源头安装,则命令(在初学者级教程中经常使用)。

如果你正在使用Linux 并且还没有熟悉的外壳, [此教程](https://www.linux.com/training-tutorials/bash-101-working-cli/) 将会有所帮助。

与往常一样, [您打开的每个新终端](../Configuring-ROS2-Environment.md).

<span id="tasks"></span>

## 操作步骤

<span id="running-a-launch-file"></span>

### 运行一个启动文件

打开新的终端并运行 :

``` console
$ ros2 launch turtlesim multisim.launch.py
```

此命令将运行以下发射文件 :

``` python
from launch import LaunchDescription
import launch_ros.actions


def generate_launch_description():
    return LaunchDescription([
        launch_ros.actions.Node(
            namespace='turtlesim1', package='turtlesim',
            executable='turtlesim_node', output='screen'),
        launch_ros.actions.Node(
            namespace='turtlesim2', package='turtlesim',
            executable='turtlesim_node', output='screen'),
    ])
```

> **说明**
>
> 上面的发射文件是用 Python 写的,但您也可以使用 XML 和 YAML 创建发射文件。您可以看到这些不同的 ROS 2 发射格式在 [使用 XML、YAML 和 Python 编写 ROS 2 启动文件](../../../How-To-Guides/Launch-file-different-formats.md).

这将运行两个龟形节点:

![](images/turtlesim_multisim.png)

现在,不要担心这个发射文件的内容。 您可以在发射时找到更多关于ROS 2发射的信息 。 [ROS 2 发射教程](../../Intermediate/Launch/Launch-Main.md).

<span id="optional-control-the-turtlesim-nodes"></span>

### (可选)控制Turtlsim节点

现在这些节点在运行,你可以像任何其他ROS 2节点一样控制它们. 例如,你可以通过打开两个额外的终端并运行以下命令,使龟群向相反的方向驱动:

在第二航站楼:

``` console
$ ros2 topic pub  /turtlesim1/turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
```

在第三航站楼:

``` console
$ ros2 topic pub  /turtlesim2/turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: -1.8}}"
```

运行这些命令后,您应该看到以下内容:

![](images/turtlesim_multisim_spin.png) <span id="summary"></span>

## 小结

目前为止,你所做的事情的意义在于你用一个命令运行了两个龟兹节点。 一旦你学会写自己的发射文件,你就能够以类似的方式运行多个节点,并设置它们的配置。 `ros2 launch` 命令。 命令。

关于ROS 2 发射文件的更多教程,请参见: [主发射文件教程页面](../../Intermediate/Launch/Launch-Main.md).

<span id="next-steps"></span>

## 后续步骤

在接下来的辅导中, [录制与回放数据](../Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.md),你会知道另一个有用的工具, `ros2 bag`.
