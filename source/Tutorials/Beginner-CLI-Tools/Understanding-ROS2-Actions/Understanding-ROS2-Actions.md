<span id="understanding-actions"></span> <span id="ros2actions"></span>
# 理解动作

**目标：** 查看 ROS 2 动作及其内部信息。

**教程级别：** 初学者

**预计用时：** 15 分钟

<span id="background"></span>
## 背景

动作是 ROS 2 的一种通信方式，主要用于耗时较长的任务。它包含三个部分：目标、反馈和结果。

动作基于话题和服务构建。它的功能与服务类似，但动作可以取消，还能持续提供反馈，而服务只返回一次响应。

动作采用客户端/服务端模型，与[话题教程](../Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)介绍的发布者/订阅者模型类似。“动作客户端”节点向“动作服务端”节点发送目标，服务端确认目标后，返回一系列反馈以及最终结果。

![动作客户端与服务端之间的通信](images/Action-SingleActionClient.gif)

<span id="prerequisites"></span>
## 前提条件

本教程以之前介绍的[节点](../Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md)、[话题](../Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)等概念为基础。

本教程使用 [turtlesim 软件包](../Introducing-Turtlesim/Introducing-Turtlesim.md)。

与往常一样，不要忘记在[每个新打开的终端](../Configuring-ROS2-Environment.md)中加载 ROS 2 环境。

<span id="tasks"></span>
## 操作步骤

<span id="setup"></span>
### 1 准备工作

启动 turtlesim 的两个节点：`/turtlesim` 和 `/teleop_turtle`。

打开新终端，运行：

```console
$ ros2 run turtlesim turtlesim_node
```

打开另一个终端，运行：

```console
$ ros2 run turtlesim turtle_teleop_key
```

<span id="use-actions"></span>
### 2 使用动作

启动 `/teleop_turtle` 节点后，终端中会出现以下提示：

```console
Use arrow keys to move the turtle.
Use G|B|V|C|D|E|R|T keys to rotate to absolute orientations. 'F' to cancel a rotation.
```

这里重点关注第二行，它对应一个动作。第一行指令则对应 [话题教程](../Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)中介绍过的 `cmd_vel` 话题。

在美式 QWERTY 键盘上，`G|B|V|C|D|E|R|T` 这几个字母键围绕 `F` 键形成一圈。如果使用的不是 QWERTY 键盘，可以参照[这张键盘图](https://upload.wikimedia.org/wikipedia/commons/d/da/KB_United_States.svg)。各个按键相对于 `F` 的位置，对应 turtlesim 中的朝向。例如，按 `E` 会让海龟转向左上方。

观察运行 `/turtlesim` 节点的终端。每按下其中一个键，都会向 `/turtlesim` 节点中的动作服务端发送一个目标：让海龟旋转到指定方向。海龟完成旋转后，终端应显示目标执行结果：

```console
[INFO] [turtlesim]: Rotation goal completed successfully
```

`F` 键用于在执行过程中取消目标。

试着先按 `C`，再在海龟完成旋转前按 `F`。运行 `/turtlesim` 的终端会显示：

```console
[INFO] [turtlesim]: Rotation goal canceled
```

除了客户端可以通过遥控输入取消目标，服务端，也就是 `/turtlesim` 节点，也可以停止执行目标。服务端主动停止处理目标称为“中止”（abort）目标。

试着先按 `D`，在第一次旋转完成前再按 `G`。运行 `/turtlesim` 的终端会显示：

```console
[WARN] [turtlesim]: Rotation goal received before a previous goal finished. Aborting previous goal
```

这个动作服务端收到新目标后，选择中止原来的目标。它也可以采用其他策略，例如拒绝新目标，或等第一个目标完成后再执行第二个。不能假定所有动作服务端都会在收到新目标时中止当前目标。

<span id="ros2-node-info"></span>
### 3 ros2 node info

要查看某个节点提供的动作列表，例如 `/turtlesim`，打开新终端并运行：

```console
$ ros2 node info /turtlesim
/turtlesim
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
    /reset: std_srvs/srv/Empty
    /spawn: turtlesim/srv/Spawn
    /turtle1/set_pen: turtlesim/srv/SetPen
    /turtle1/teleport_absolute: turtlesim/srv/TeleportAbsolute
    /turtle1/teleport_relative: turtlesim/srv/TeleportRelative
    /turtlesim/describe_parameters: rcl_interfaces/srv/DescribeParameters
    /turtlesim/get_parameter_types: rcl_interfaces/srv/GetParameterTypes
    /turtlesim/get_parameters: rcl_interfaces/srv/GetParameters
    /turtlesim/list_parameters: rcl_interfaces/srv/ListParameters
    /turtlesim/set_parameters: rcl_interfaces/srv/SetParameters
    /turtlesim/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically
  Service Clients:

  Action Servers:
    /turtle1/rotate_absolute: turtlesim/action/RotateAbsolute
  Action Clients:
```

该命令列出了 `/turtlesim` 的订阅者、发布者、服务、动作服务端和动作客户端。

注意，`/turtlesim` 的 `/turtle1/rotate_absolute` 动作位于 `Action Servers` 下。这表明 `/turtlesim` 会响应该动作的目标，并为其提供反馈。

`/teleop_turtle` 节点的 `Action Clients` 下则包含 `/turtle1/rotate_absolute`，表示它会向这个动作发送目标。可以运行以下命令查看：

```console
$ ros2 node info /teleop_turtle
/teleop_turtle
  Subscribers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
  Publishers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /rosout: rcl_interfaces/msg/Log
    /turtle1/cmd_vel: geometry_msgs/msg/Twist
  Service Servers:
    /teleop_turtle/describe_parameters: rcl_interfaces/srv/DescribeParameters
    /teleop_turtle/get_parameter_types: rcl_interfaces/srv/GetParameterTypes
    /teleop_turtle/get_parameters: rcl_interfaces/srv/GetParameters
    /teleop_turtle/list_parameters: rcl_interfaces/srv/ListParameters
    /teleop_turtle/set_parameters: rcl_interfaces/srv/SetParameters
    /teleop_turtle/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically
  Service Clients:

  Action Servers:

  Action Clients:
    /turtle1/rotate_absolute: turtlesim/action/RotateAbsolute
```

<span id="ros2-action-list"></span>
### 4 ros2 action list

使用以下命令列出 ROS 计算图中的所有动作：

```console
$ ros2 action list
/turtle1/rotate_absolute
```

这是当前 ROS 计算图中唯一的动作，它用于控制海龟旋转，前面已经体验过。通过 `ros2 node info <node_name>`，你也已经知道，这个动作有一个属于 `/teleop_turtle` 的动作客户端，以及一个属于 `/turtlesim` 的动作服务端。

<span id="ros2-action-list-t"></span>
#### 4.1 ros2 action list -t

与话题和服务一样，动作也有类型。运行以下命令，查看 `/turtle1/rotate_absolute` 的类型：

```console
$ ros2 action list -t
/turtle1/rotate_absolute [turtlesim/action/RotateAbsolute]
```

每个动作名称右侧的方括号中显示动作类型。这里唯一的动作 `/turtle1/rotate_absolute` 的类型是 `turtlesim/action/RotateAbsolute`。通过命令行或代码执行动作时，需要知道这个类型。

<span id="ros2-action-info"></span>
### 5 ros2 action info

用以下命令进一步查看 `/turtle1/rotate_absolute` 的信息：

```console
$ ros2 action info /turtle1/rotate_absolute
Action: /turtle1/rotate_absolute
Action clients: 1
    /teleop_turtle
Action servers: 1
    /turtlesim
```

这与之前对各节点运行 `ros2 node info` 得到的信息一致：对于 `/turtle1/rotate_absolute` 动作，`/teleop_turtle` 节点包含动作客户端，`/turtlesim` 节点包含动作服务端。

<span id="ros2-interface-show"></span>
### 6 ros2 interface show

在自行发送或执行动作目标之前，还需要知道动作类型的数据结构。

之前通过 `ros2 action list -t` 已经确定了 `/turtle1/rotate_absolute` 的类型。在终端中输入以下命令：

```console
$ ros2 interface show turtlesim/action/RotateAbsolute
```

输出如下：

```text
# The desired heading in radians
float32 theta
---
# The angular displacement in radians to the starting position
float32 delta
---
# The remaining rotation in radians
float32 remaining
```

第一个 `---` 之前是目标请求的结构，包括数据类型和字段名称；接下来一部分是结果的结构；最后一部分是反馈的结构。

<span id="ros2-action-send-goal"></span>
### 7 ros2 action send_goal

现在使用以下语法，从命令行发送动作目标：

```console
$ ros2 action send_goal <action_name> <action_type> <values>
```

`<values>` 必须使用 YAML 格式。

观察 turtlesim 窗口，同时在终端输入：

```console
$ ros2 action send_goal /turtle1/rotate_absolute turtlesim/action/RotateAbsolute "{theta: 1.57}"
Waiting for an action server to become available...
Sending goal:
   theta: 1.57

Goal accepted with ID: f8db8f44410849eaa93d3feb747dd444

Result:
  delta: -1.568000316619873

Goal finished with status: SUCCEEDED
```

应该能看到海龟旋转。

每个目标都有唯一 ID，显示在返回的信息中。输出还包含结果字段 `delta`，表示相对于起始位置的角位移。

要查看这个目标的反馈，为 `ros2 action send_goal` 命令添加 `--feedback`：

```console
$ ros2 action send_goal /turtle1/rotate_absolute turtlesim/action/RotateAbsolute "{theta: -1.57}" --feedback
Sending goal:
   theta: -1.57

Goal accepted with ID: e6092c831f994afda92f0086f220da27

Feedback:
  remaining: -3.1268222332000732

Feedback:
  remaining: -3.1108222007751465

…

Result:
  delta: 3.1200008392333984

Goal finished with status: SUCCEEDED
```

在目标完成之前，你会持续收到反馈，内容是剩余旋转角度，单位为弧度。

<span id="summary"></span>
## 小结

动作类似于服务，但允许执行耗时较长的任务、定期提供反馈，并且支持取消。

机器人系统通常会使用动作实现导航。一个动作目标可以要求机器人移动到某个位置。导航过程中，机器人可以持续发送进度更新，也就是反馈；到达目的地后，再发送最终结果。

Turtlesim 提供了一个动作服务端，动作客户端可以向它发送让海龟旋转的目标。本教程查看了这个 `/turtle1/rotate_absolute` 动作，帮助你理解动作是什么，以及它如何工作。

<span id="next-steps"></span>
## 后续步骤

至此，你已经学习了 ROS 2 的核心概念。本系列最后几篇教程将介绍一些工具和技巧，让 ROS 2 更易于使用，首先是[使用 rqt_console 查看日志](../Using-Rqt-Console/Using-Rqt-Console.md)。

<span id="related-content"></span>
## 相关内容

有关 ROS 2 动作设计决策的更多说明，请参阅[动作设计文档](https://design.ros2.org/articles/actions.html)。
