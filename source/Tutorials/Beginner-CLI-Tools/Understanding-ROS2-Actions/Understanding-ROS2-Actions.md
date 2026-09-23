---
translation_status: machine_translated
source: Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Actions/Understanding-ROS2-Actions.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="understanding-actions"></span> <span id="ros2actions"></span>

# 理解动作

**目标：** ROS 2中的内视动作.

**教程级别：** 入门

**用时：** 15分钟

<span id="background"></span>

## 背景

动作是ROS 2中的通信类型之一,用于长期运行的任务,由三个部分组成:目标、反馈和结果。

动作建立在主题和服务之上,其功能类似于服务,但动作可以取消,它们也提供稳定的反馈,而不是返回单一响应的服务。

动作使用一个客户端-服务器模型,类似于出版商-订阅者模型(描述于 [主题教程](../Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)“行动客户端”节点向“行动服务器”节点发送一个目标,该节点承认目标,并返回反馈流和结果。

![](images/Action-SingleActionClient.gif) <span id="prerequisites"></span>

## 前提条件

此教程基于概念, 如 [节点](../Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md) 财务报告和财务报告 [话题](../Understanding-ROS2-Topics/Understanding-ROS2-Topics.md),包含在以前的教程中.

此教程使用 [龟兹包](../Introducing-Turtlesim/Introducing-Turtlesim.md).

与往常一样, [您打开的每个新终端](../Configuring-ROS2-Environment.md).

<span id="tasks"></span>

## 操作步骤

<span id="setup"></span>

### 1 设置

启动两个乌龟结点, `/turtlesim` 财务报告和财务报告 `/teleop_turtle`.

打开新的终端并运行 :

``` console
$ ros2 run turtlesim turtlesim_node
```

打开另一个终端并运行 :

``` console
$ ros2 run turtlesim turtle_teleop_key
```

<span id="use-actions"></span>

### 2 使用动作

当你发射时 `/teleop_turtle` 节点,您将在终端中看到以下消息:

``` console
Use arrow keys to move the turtle.
Use G|B|V|C|D|E|R|T keys to rotate to absolute orientations. 'F' to cancel a rotation.
```

让我们集中关注第二行,该行与一项行动相对应。 (第一句话与上文讨论的“cmd_vel”专题相对应。) [主题教程](../Understanding-ROS2-Topics/Understanding-ROS2-Topics.md).)

注意字母密钥 `G|B|V|C|D|E|R|T` 将“框”改为“框”。 `F` 美国 QWERTY 键盘上的密钥( 如果您没有使用 QWERTY 键盘, 请参见 [此链接](https://upload.wikimedia.org/wikipedia/commons/d/da/KB_United_States.svg) 。每个键的位置周围 `F` 对应于龟语中的这种取向。 `E` 将海龟的方向旋转到左上角。

注意到终点站 `/turtlesim` 节点正在运行。每次按下其中的键时,您都会向作为动作服务器一部分的动作服务器发送一个目标。 `/turtlesim` 节点。 目标是旋转海龟以面对特定方向。 一旦海龟完成旋转, 转发目标结果的信息应该显示 :

``` console
[INFO] [turtlesim]: Rotation goal completed successfully
```

那个... `F` 键将取消中执行目标。

尝试按下 `C` 键,然后按下 `F` 在海龟完成旋转前键。在海龟完成旋转的终点处。 `/turtlesim` 节点正在运行, 您将会看到消息 :

``` console
[INFO] [turtlesim]: Rotation goal canceled
```

不仅客户端(您在Teleop中的输入)可以阻止一个目标,而且服务器端(the Server-side)也可以阻止一个目标. `/turtlesim` 当服务器端选择停止处理一个目标时,据说会“破坏”目标。

试试打 `D` 键,然后是 `G` 在第一个旋转完成前按键。 `/turtlesim` 节点正在运行, 您将会看到消息 :

``` console
[WARN] [turtlesim]: Rotation goal received before a previous goal finished. Aborting previous goal
```

这个动作服务器选择了中止第一个目标,因为它得到了一个新的目标。它可能选择了其他目标,比如拒绝新目标,或者在第一个目标完成后执行第二个目标。不要假设每个动作服务器都会在获得新目标时选择中止当前的目标。

<span id="ros2-node-info"></span>

### 3 个 ros2 节点信息

要看到节点提供的行动列表, `/turtlesim` 在这种情况下,打开一个新的终端并运行命令:

``` console
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

命令返回一个列表 `/turtlesim`用户、出版商、服务、行动服务器和行动客户端。

通知 `/turtle1/rotate_absolute` 用于 `/turtlesim` 下方为 `Action Servers`。这意味着 `/turtlesim` 答复和提供反馈 `/turtle1/rotate_absolute` 行动。

那个... `/teleop_turtle` 节点有名字 `/turtle1/rotate_absolute` 下级 `Action Clients` 表示它为动作名称发送目标。要看到它,请运行:

``` console
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

### 4 ros2 动作列表

为了识别ROS图中的所有动作,运行命令:

``` console
$ ros2 action list
/turtle1/rotate_absolute
```

这是ROS图中目前唯一一个动作。 它控制了龟的旋转, 正如您早些时候看到的。 您也已经知道有一个动作客户端( 部分) `/teleop_turtle`)和一个动作服务器(部分 `/turtlesim`用于此操作的 `ros2 node info <node_name>` 命令。 命令。

<span id="ros2-action-list-t"></span>

#### 4.1 ros2 动作列表 - t

动作有类型,类似于主题和服务。 `/turtle1/rotate_absolute`命令类型 :

``` console
$ ros2 action list -t
/turtle1/rotate_absolute [turtlesim/action/RotateAbsolute]
```

每个行动名称的右括号内(此处仅指 `/turtle1/rotate_absolute`)是动作类型, `turtlesim/action/RotateAbsolute`。当您想要从命令行或代码执行动作时,您需要此操作。

<span id="ros2-action-info"></span>

### 5 ros2 动作信息

你可以进一步回顾一下 `/turtle1/rotate_absolute` 与命令一起操作 :

``` console
$ ros2 action info /turtle1/rotate_absolute
Action: /turtle1/rotate_absolute
Action clients: 1
    /teleop_turtle
Action servers: 1
    /turtlesim
```

这告诉我们我们之前从跑步中学到了什么 `ros2 node info` 在每个节点上: `/teleop_turtle` 节点有一个动作客户端和 `/turtlesim` 节点有一个动作服务器 `/turtle1/rotate_absolute` 行动。

<span id="ros2-interface-show"></span>

### 6 ros2 接口显示

在发送或执行动作目标之前,您还需要的另外一条信息是动作类型的结构.

记得你指认过 `/turtle1/rotate_absolute`命令运行时的类型 `ros2 action list -t`中,输入以下命令,并在终端中输入动作类型:

``` console
$ ros2 interface show turtlesim/action/RotateAbsolute
```

将返回 :

``` text
# The desired heading in radians
float32 theta
---
# The angular displacement in radians to the starting position
float32 delta
---
# The remaining rotation in radians
float32 remaining
```

第一篇上面的这一节 `---` 是目标请求的结构(数据类型和名称)。下一节是结果的结构。最后一节是反馈的结构。

<span id="ros2-action-send-goal"></span>

### 7 ros2 动作发送_目标

现在让我们从命令行发出一个动作目标,其语法如下:

``` console
$ ros2 action send_goal <action_name> <action_type> <values>
```

`<values>` 需要使用 YAML 格式。

监视龟兹窗口,并将以下命令输入您的终端:

``` console
$ ros2 action send_goal /turtle1/rotate_absolute turtlesim/action/RotateAbsolute "{theta: 1.57}"
Waiting for an action server to become available...
Sending goal:
   theta: 1.57

Goal accepted with ID: f8db8f44410849eaa93d3feb747dd444

Result:
  delta: -1.568000316619873

Goal finished with status: SUCCEEDED
```

你应该看看海龟在旋转

所有目标都有一个独特的ID,在返回消息中显示。您也可以看到结果,一个有名字的字段 `delta`,这就是迁移到起始位置。

要了解这一目标的反馈,请添加: `--feedback` 页:1 `ros2 action send_goal` 命令 :

``` console
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

你们将继续收到反馈,其余的弧度,直到目标完成.

<span id="summary"></span>

## 小结

动作类似于允许您执行长期运行的任务,提供定期反馈,并且可以取消的服务.

机器人系统可能使用动作进行导航。 一个动作目标可以让机器人前往某个位置。 当机器人导航到该位置时,它可以沿途发送更新(即反馈),然后在到达目的地后发出最终结果信息。

Turtlesim 拥有一个动作服务器,动作客户端可以将目标发送给旋转龟。在此教程中,您对动作进行了回顾, `/turtle1/rotate_absolute`,以更好地了解什么是行动以及它们是如何运作的.

<span id="next-steps"></span>

## 后续步骤

现在您已经覆盖了 ROS 2 的全部核心概念。 本集的最后几套教程将会向您介绍一些工具和技术, 这些工具和技术将更容易使用 ROS 2, 首先 [使用 rqt\_ console 查看日志](../Using-Rqt-Console/Using-Rqt-Console.md).

<span id="related-content"></span>

## 相关内容

您可以在 ROS 2 中阅读更多关于行动背后的设计决定 [这儿](https://design.ros2.org/articles/actions.html).
