---
translation_status: machine_translated
source: Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="understanding-nodes"></span> <span id="ros2nodes"></span>

# 理解节点

**目标：** 学习ROS 2中节点的功能,以及与之互动的工具.

**教程级别：** 入门

**用时：** 10分钟

<span id="background"></span>

## 背景

<span id="the-ros-2-graph"></span>

### 1 ROS 2 图

在接下来的几个教程中,你们将了解一系列核心ROS 2概念,这些概念构成了所谓的“ROS(2)图”。

ROS 图是 ROS 2 元素同时处理数据的网络。 它包含所有可执行文件以及它们之间的连接, 如果您要将其全部映射出来并可视化的话 。

<span id="nodes-in-ros-2"></span>

### 2个ROS节点

ROS中的每个节点应负责单一的模块化目的,例如控制轮动机或发布激光测距器的传感器数据. 每个节点可以通过专题,服务,动作或参数发送和接收来自其他节点的数据.

![](images/Nodes-TopicandService.gif)

一个完整的机器人系统由许多节点在协奏中工作组成. ROS 2中,一个单一的可执行程序(C++程序,Python程序等)可以包含一个或多个节点.

<span id="prerequisites"></span>

## 前提条件

那个... [上一个教程](../Introducing-Turtlesim/Introducing-Turtlesim.md) 演示如何安装 `turtlesim` 这里使用的软件包 。

与往常一样, [您打开的每个新终端](../Configuring-ROS2-Environment.md).

<span id="tasks"></span>

## 操作步骤

<span id="ros2-run"></span>

### 1 ros2 运行

命令 `ros2 run` 从软件包中发射可执行文件。

``` console
$ ros2 run <package_name> <executable_name>
```

要运行龟兹姆,打开一个新的终端,并输入以下命令:

``` console
$ ros2 run turtlesim turtlesim_node
```

乌龟之窗会打开,就像你从... [上一个教程](../Introducing-Turtlesim/Introducing-Turtlesim.md).

这里,软件包的名字是 `turtlesim` 可执行名称为 `turtlesim_node`.

但我们仍然不知道节点名称。您可以通过使用 `ros2 node list`

<span id="ros2-node-list"></span>

### 2 ros2 节点列表

`ros2 node list` 将显示所有运行的节点的名称。当您想要与节点交互时,或者当您有一个系统运行了许多节点并需要跟踪这些节点时,这尤其有用。

在龟兹姆仍在另一端运行时打开一个新的终端,然后输入以下命令。终端将返回节点名称 :

``` console
$ ros2 node list
/turtlesim
```

打开另一个新终端, 并用命令启动 Teleop 节点 :

``` console
$ ros2 run turtlesim turtle_teleop_key
```

在这里,我们指的是 `turtlesim` 软件包,但这次我们瞄准名为可执行文件的 `turtle_teleop_key`.

回到你跑的终点站 `ros2 node list` 您将看到两个活动节点的名称 :

``` console
$ ros2 node list
/turtlesim
/teleop_turtle
```

<span id="remapping"></span>

#### 2.1 重新绘图

[重新绘图](https://design.ros2.org/articles/ros_command_line_arguments.html#name-remapping-rules) 允许您重新指定默认节点属性, 如节点名称、 主题名称、 服务名称等, 用于自定义值。 在上一个教程中, 您在 `turtle_teleop_key` 更改 cmd_vel 主题和目标 **乌龟2**.

现在,让我们重新指定我们的名字 `/turtlesim` 节点。在新的终端中,运行以下命令:

``` console
$ ros2 run turtlesim turtlesim_node --ros-args --remap __node:=my_turtle
```

既然你打来电话 `ros2 run` 不过,现在如果你回到你运行的终点站 `ros2 node list`,然后再次运行,你会看到三个节点名称:

``` console
/my_turtle
/turtlesim
/teleop_turtle
```

<span id="ros2-node-info"></span>

### 3 个 ros2 节点信息

现在你知道节点的名称了,可以通过以下方式获取更多有关它们的信息:

``` console
$ ros2 node info <node_name>
```

为了检查你最新的节点 `my_turtle`,运行以下命令:

``` console
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

`ros2 node info` 返回订阅者、出版商、服务和动作的列表,即与该节点相互作用的ROS图表连接。

现在尝试运行相同的命令在 `/teleop_turtle` 节点,并查看其连接与 `my_turtle`.

您将更多地了解 ROS 图表连接概念, 包括即将到来的教程中的信息类型 。

<span id="summary"></span>

## 小结

节点是机器人系统中服务于单一模块化目的的ROS 2基本元素.

在此教程中, 您使用了在 `turtlesim` 运行可执行文件的软件包 `turtlesim_node` 财务报告和财务报告 `turtle_teleop_key`.

你学会了如何使用 `ros2 node list` 以发现活动节点名称和 `ros2 node info` 这些工具对于了解复杂、现实世界的机器人系统中的数据流动至关重要。

<span id="next-steps"></span>

## 后续步骤

现在你知道ROS 2中的节点了,你可以继续前进到 [主题教程](../Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)。主题是连接节点的通信类型之一。

<span id="related-content"></span>

## 相关内容

那个... [概念](../../../Concepts.md) 页面为节点的概念增加了一些细节。
