<span id="understanding-nodes"></span> <span id="ros2nodes"></span>
# 理解节点

**目标：** 了解节点在 ROS 2 中的作用，以及与节点交互的工具。

**教程级别：** 初学者

**预计用时：** 10 分钟

<span id="background"></span>
## 背景

<span id="the-ros-2-graph"></span>
### 1 ROS 2 计算图

在接下来的几篇教程中，你将学习一系列 ROS 2 核心概念，它们共同构成所谓的“ROS（2）计算图”。

ROS 计算图是由共同处理数据的 ROS 2 要素组成的网络。如果将它们绘制出来，图中会包含所有可执行程序及其相互连接。

<span id="nodes-in-ros-2"></span>
### 2 ROS 2 中的节点

ROS 中的每个节点都应该负责一项独立、模块化的功能，例如控制车轮电机，或发布激光测距仪的传感器数据。每个节点都可以通过话题、服务、动作或参数与其他节点交换数据。

![节点通过话题和服务通信](images/Nodes-TopicandService.gif)

完整的机器人系统由许多协同工作的节点组成。在 ROS 2 中，单个可执行程序（C++ 程序、Python 程序等）可以包含一个或多个节点。

<span id="prerequisites"></span>
## 前提条件

[上一篇教程](../Introducing-Turtlesim/Introducing-Turtlesim.md)介绍了如何安装本教程使用的 `turtlesim` 软件包。

与往常一样，不要忘记在[每个新打开的终端](../Configuring-ROS2-Environment.md)中加载 ROS 2 环境。

<span id="tasks"></span>
## 操作步骤

<span id="ros2-run"></span>
### 1 ros2 run

`ros2 run` 命令用于启动软件包中的可执行程序：

```console
$ ros2 run <package_name> <executable_name>
```

打开新终端，输入以下命令运行 turtlesim：

```console
$ ros2 run turtlesim turtlesim_node
```

与[上一篇教程](../Introducing-Turtlesim/Introducing-Turtlesim.md)一样，turtlesim 窗口会打开。

这里，软件包名称是 `turtlesim`，可执行程序名称是 `turtlesim_node`。

不过，我们还不知道节点名称。可以使用 `ros2 node list` 查找节点名称。

<span id="ros2-node-list"></span>
### 2 ros2 node list

`ros2 node list` 会显示所有正在运行的节点名称。当你想与某个节点交互，或系统中运行着许多节点、需要了解有哪些节点时，这个命令尤其有用。

保持 turtlesim 在原终端中运行，打开新终端并输入以下命令。终端会返回节点名称：

```console
$ ros2 node list
/turtlesim
```

再打开一个新终端，运行以下命令启动遥控节点：

```console
$ ros2 run turtlesim turtle_teleop_key
```

这里仍然使用 `turtlesim` 软件包，但执行的是 `turtle_teleop_key`。

回到运行过 `ros2 node list` 的终端，再次执行该命令。现在会看到两个活动节点的名称：

```console
$ ros2 node list
/turtlesim
/teleop_turtle
```

<span id="remapping"></span>
#### 2.1 重映射

[重映射](https://design.ros2.org/articles/ros_command_line_arguments.html#name-remapping-rules)允许你为节点名称、话题名称、服务名称等默认属性指定自定义值。在上一篇教程中，你对 `turtle_teleop_key` 进行了重映射，修改 `cmd_vel` 话题，使它控制 **turtle2**。

现在修改 `/turtlesim` 节点的名称。在新终端中运行：

```console
$ ros2 run turtlesim turtlesim_node --ros-args --remap __node:=my_turtle
```

由于再次通过 `ros2 run` 启动了 turtlesim，会打开另一个 turtlesim 窗口。回到运行过 `ros2 node list` 的终端，再次执行该命令，将看到三个节点名称：

```console
/my_turtle
/turtlesim
/teleop_turtle
```

<span id="ros2-node-info"></span>
### 3 ros2 node info

知道节点名称后，可以用以下命令获取更多信息：

```console
$ ros2 node info <node_name>
```

运行以下命令，查看刚启动的 `my_turtle` 节点：

```console
$ ros2 node info /my_turtle
/my_turtle
  Subscribers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /turtle1/cmd_vel: geometry_msgs/msg/Twist
  Publishers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /rosout: rcl_interfaces/msg/Log
    /turtle1/color_sensor: turtlesim/msg/Color
    /turtle1/pose: turtlesim/msg/Pose
  Service Servers:
    /clear: std_srvs/srv/Empty
    /kill: turtlesim/srv/Kill
    /my_turtle/describe_parameters: rcl_interfaces/srv/DescribeParameters
    /my_turtle/get_parameter_types: rcl_interfaces/srv/GetParameterTypes
    /my_turtle/get_parameters: rcl_interfaces/srv/GetParameters
    /my_turtle/list_parameters: rcl_interfaces/srv/ListParameters
    /my_turtle/set_parameters: rcl_interfaces/srv/SetParameters
    /my_turtle/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically
    /reset: std_srvs/srv/Empty
    /spawn: turtlesim/srv/Spawn
    /turtle1/set_pen: turtlesim/srv/SetPen
    /turtle1/teleport_absolute: turtlesim/srv/TeleportAbsolute
    /turtle1/teleport_relative: turtlesim/srv/TeleportRelative
  Service Clients:

  Action Servers:
    /turtle1/rotate_absolute: turtlesim/action/RotateAbsolute
  Action Clients:
```

`ros2 node info` 会列出订阅者、发布者、服务和动作，也就是 ROS 计算图中与该节点交互的连接。

现在试着对 `/teleop_turtle` 节点运行同样的命令，观察它的连接与 `my_turtle` 有何不同。

后续教程将进一步介绍 ROS 计算图中的连接，以及消息类型等概念。

<span id="summary"></span>
## 小结

节点是 ROS 2 的基本组成要素，在机器人系统中承担一项独立、模块化的功能。

本教程通过运行 `turtlesim_node` 和 `turtle_teleop_key`，使用了 `turtlesim` 软件包创建的节点。

你学习了使用 `ros2 node list` 查找活动节点名称，以及使用 `ros2 node info` 查看单个节点的内部信息。这些工具对于理解复杂的真实机器人系统中的数据流至关重要。

<span id="next-steps"></span>
## 后续步骤

理解 ROS 2 节点之后，可以继续学习[话题教程](../Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)。话题是连接节点的一种通信方式。

<span id="related-content"></span>
## 相关内容

[概念页面](../../../Concepts.md)提供了关于节点概念的更多说明。
