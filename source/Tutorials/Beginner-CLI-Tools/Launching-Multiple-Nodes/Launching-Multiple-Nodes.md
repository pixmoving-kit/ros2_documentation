<span id="launching-nodes"></span> <span id="ros2launch"></span>
# 启动节点

**目标：** 使用命令行工具一次启动多个节点。

**教程级别：** 初级

**预计用时：** 5 分钟

<span id="background"></span>
## 背景

在之前大多数入门教程中，每运行一个新节点，都需要打开一个新终端。随着系统变得复杂、同时运行的节点越来越多，不断打开终端并重新输入配置会变得繁琐。

启动文件可以同时启动和配置多个包含 ROS 2 节点的可执行程序。

使用 `ros2 launch` 命令运行一个启动文件，就能一次启动整个系统，包括所有节点及其配置。

<span id="prerequisites"></span>
## 前提条件

开始本教程前，请按照 ROS 2 [安装页面](../../../Installation.md)的说明安装 ROS 2。

本教程中的命令假设你按照操作系统对应的二进制软件包安装指南完成了安装，Linux 使用 deb 包。如果从源码构建，也可以继续学习，但环境设置文件的路径可能不同。从源码安装时，也不能使用初级教程中常见的 `sudo apt install ros-<distro>-<package>` 命令。

如果使用 Linux 且不熟悉 shell，可以阅读[这篇教程](https://www.linux.com/training-tutorials/bash-101-working-cli/)。

和之前一样，不要忘记在[每个新打开的终端](../Configuring-ROS2-Environment.md)中加载（source）ROS 2 环境。

<span id="tasks"></span>
## 任务

<span id="running-a-launch-file"></span>
### 运行启动文件

打开新终端并运行：

```console
$ ros2 launch turtlesim multisim.launch.py
```

该命令将运行启动文件 [`multisim.launch.py`](launch/multisim.launch.py)。

!!! note "说明"
    这个启动文件使用 Python 编写，也可以使用 XML 或 YAML 创建启动文件。不同 ROS 2 启动文件格式的比较见[不同格式的启动文件](../../../How-To-Guides/Launch-file-different-formats.md)。

该命令会运行两个 turtlesim 节点：

![两个 turtlesim 节点的窗口](images/turtlesim_multisim.png)

目前不必深究启动文件的内容。有关 ROS 2 launch 的更多信息，参见 [ROS 2 launch 教程](../../Intermediate/Launch/Launch-Main.md)。

<span id="optional-control-the-turtlesim-nodes"></span>
### 可选：控制 turtlesim 节点

节点启动后，可以像控制其他 ROS 2 节点一样控制它们。例如，再打开两个终端，运行以下命令，让两只海龟沿相反方向运动。

在第二个终端运行：

```console
$ ros2 topic pub  /turtlesim1/turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
```

在第三个终端运行：

```console
$ ros2 topic pub  /turtlesim2/turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: -1.8}}"
```

运行后，应看到类似以下画面：

![两只海龟沿相反方向运动](images/turtlesim_multisim_spin.png)

<span id="summary"></span>
## 小结

本教程的关键是使用一条命令运行了两个 turtlesim 节点。学会编写自己的启动文件后，就可以同样使用 `ros2 launch` 命令，一次运行多个节点并设置它们的配置。

更多 ROS 2 启动文件教程见[启动文件教程主页](../../Intermediate/Launch/Launch-Main.md)。

<span id="next-steps"></span>
## 后续步骤

下一篇教程[录制与回放数据](../Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.md)将介绍另一个实用工具 `ros2 bag`。
