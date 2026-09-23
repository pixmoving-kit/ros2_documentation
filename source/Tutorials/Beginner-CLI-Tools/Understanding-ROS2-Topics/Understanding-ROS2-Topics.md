---
translation_status: machine_translated
source: Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="understanding-topics"></span> <span id="ros2topics"></span>

# 理解话题

**目标：** 使用 rqt_graph 和命令行工具来进行 ROS 2 主题的回顾.

**教程级别：** 入门

**用时：** 20分钟

<span id="background"></span>

## 背景

ROS 2 将复杂的系统分解成许多模块化的节点。主题是ROS图中的一个关键要素,它起到节点交换消息的总线的作用。

![](images/Topic-SinglePublisherandSingleSubscriber.gif)

节点可以发布任何数量主题的数据,同时订阅任何数量主题.

![](images/Topic-MultiplePublisherandMultipleSubscriber.gif)

主题是数据在节点之间移动,从而在系统不同部分之间移动的主要方式之一.

<span id="prerequisites"></span>

## 前提条件

那个... [上一个教程](../Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md) 提供关于在此基础上建立节点的一些有用的背景资料。

与往常一样, [您打开的每个新终端](../Configuring-ROS2-Environment.md).

<span id="tasks"></span>

## 操作步骤

<span id="setup"></span>

### 1 设置

现在,你应该很舒服 开始乌龟。

打开新的终端并运行 :

``` console
$ ros2 run turtlesim turtlesim_node
```

打开另一个终端并运行 :

``` console
$ ros2 run turtlesim turtle_teleop_key
```

召回从 [上一个教程](../Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md) 这些节点的名称是 `/turtlesim` 财务报告和财务报告 `/teleop_turtle` 默认。

<span id="rqt-graph"></span>

### 2 rqt_图

在整个教程中,我们将使用 `rqt_graph` 以可视化变化中的节点和主题,以及它们之间的连接.

那个... [龟兹教程](../Introducing-Turtlesim/Introducing-Turtlesim.md) 告诉你如何安装 rqt 及其所有插件, 包括 `rqt_graph`.

要运行 rqt_graph,请打开新的终端并输入命令 :

``` console
$ ros2 run rqt_graph rqt_graph
```

您也可以通过打开 rqt_graph 打开 `rqt` 选择 **插件** \> **内省** \> **节点图**.

![](images/rqt_graph.png)

您应该看到上面的节点和主题,以及围绕图表边缘的两个动作(让我们暂时忽略这些动作 ) 。 如果您在中央的话题上徘徊着鼠标, 您将会看到上面图像中突出的颜色 。

该图描述的是: `/turtlesim` 节点和 `/teleop_turtle` 节点在一个话题上互相通信。 `/teleop_turtle` 节点正在发布数据(您输入的键盘键来移动乌龟周围)到 `/turtle1/cmd_vel` 主题和主题 `/turtlesim` 节点加入该主题以接收数据。

rqt_graph的突出特征非常有助于审查许多节点和主题以多种不同方式相连的更为复杂的系统.

rqt_graph 是一个图形化的反省工具。 现在我们将查看一些用于反省话题的命令行工具 。

<span id="ros2-topic-list"></span>

### 3 ros2 主题列表

运行 `ros2 topic list` 命令在新终端中将返回当前在系统中活动的所有主题列表 :

``` console
$ ros2 topic list
/parameter_events
/rosout
/turtle1/cmd_vel
/turtle1/color_sensor
/turtle1/pose
```

`ros2 topic list -t` 将返回同样的专题清单,这次将把专题类型置于括号内:

``` console
$ ros2 topic list -t
/parameter_events [rcl_interfaces/msg/ParameterEvent]
/rosout [rcl_interfaces/msg/Log]
/turtle1/cmd_vel [geometry_msgs/msg/Twist]
/turtle1/color_sensor [turtlesim/msg/Color]
/turtle1/pose [turtlesim/msg/Pose]
```

这些属性,特别是类型, 节点如何知道他们在谈论 相同的信息,

如果您想知道这些话题在 rqt_graph 中的位置, 您可以解析所有下框 **隐藏 :**

![](images/unhide.png)

不过,现在请检查这些选项,以避免混淆。

<span id="ros2-topic-echo"></span>

### 4 ros2 主题回声

欲了解某一主题的数据是否得到公布,请使用:

``` console
$ ros2 topic echo <topic_name>
```

既然我们知道 `/teleop_turtle` 将数据发布到 `/turtlesim` 超过 `/turtle1/cmd_vel` 主题,让我们使用 `echo` 研究这一专题:

``` console
$ ros2 topic echo /turtle1/cmd_vel
```

起初,这个命令不会返回任何数据。 这是因为它正在等待 `/teleop_turtle` 发表一些东西。

回到终点站 `turtle_teleop_key` 正在运行并使用箭头来移动乌龟。当心您的终端 `echo` 并同时运行,您将会看到您所做的每一次运动的位置数据被发布:

``` console
linear:
  x: 2.0
  y: 0.0
  z: 0.0
angular:
  x: 0.0
  y: 0.0
  z: 0.0
  ---
```

现在返回 rqt_graph 并取消检查 **调试** 框中选择一个选项。

![](images/debug.png)

`/_ros2cli_26646` 是该 `echo` 命令我们刚刚运行(数字可能不同)。现在你可以看到,出版商正在将数据发布到 `cmd_vel` 并订阅了两个用户。

<span id="ros2-topic-info"></span>

### 5 ros2 专题信息

话题不一定只是一对一的交流;它们可以是一对一,多对一,或者多对一.

另一种看方式是运行:

``` console
$ ros2 topic info /turtle1/cmd_vel
Type: geometry_msgs/msg/Twist
Publisher count: 1
Subscription count: 2
```

<span id="ros2-topic-info-verbose"></span>

#### 5.1 ros2 主题信息 - 动词

欲了解一个主题的更详细信息,请使用 `--verbose` (或 减) `-v`) 旗帜:

``` console
$ ros2 topic info /turtle1/cmd_vel --verbose
```

这样做将得出更多细节,包括:

- 出版商和订户的节点名称和命名空间

- 主题类型

- QoS 简介

``` console
Type: geometry_msgs/msg/Twist

Publisher count: 1

Node name: teleop_turtle
Node namespace: /
Topic type: geometry_msgs/msg/Twist
Topic type hash: RIHS01_9c45bf16fe0983d80e3cfe750d6835843d265a9a6c46bd2e609fcddde6fb8d2a
Endpoint type: PUBLISHER
GID: 24.ba.3e.e7.c1.51.bb.46.21.41.de.36.1b.14.73.5e
QoS profile:
  Reliability: RELIABLE
  History (Depth): KEEP_LAST (7)
  Durability: VOLATILE
  Lifespan: Infinite
  Deadline: Infinite
  Liveliness: AUTOMATIC
  Liveliness lease duration: Infinite

Subscription count: 2

Node name: _ros2cli_300492
Node namespace: /
Topic type: geometry_msgs/msg/Twist
Topic type hash: RIHS01_9c45bf16fe0983d80e3cfe750d6835843d265a9a6c46bd2e609fcddde6fb8d2a
Endpoint type: SUBSCRIPTION
GID: cc.4d.98.79.29.91.fe.25.8a.0a.c9.03.db.1a.ec.81
QoS profile:
  Reliability: RELIABLE
  History (Depth): KEEP_LAST (5)
  Durability: VOLATILE
  Lifespan: Infinite
  Deadline: Infinite
  Liveliness: AUTOMATIC
  Liveliness lease duration: Infinite

Node name: turtlesim
Node namespace: /
Topic type: geometry_msgs/msg/Twist
Topic type hash: RIHS01_9c45bf16fe0983d80e3cfe750d6835843d265a9a6c46bd2e609fcddde6fb8d2a
Endpoint type: SUBSCRIPTION
GID: 9c.33.59.38.b2.f2.42.47.69.1b.7f.0e.5e.1d.86.f5
QoS profile:
  Reliability: RELIABLE
  History (Depth): KEEP_LAST (7)
  Durability: VOLATILE
  Lifespan: Infinite
  Deadline: Infinite
  Liveliness: AUTOMATIC
  Liveliness lease duration: Infinite
```

<span id="ros2-interface-show"></span>

### 6 ros2 接口显示

节点使用消息在主题上发送数据。 发布者和订阅者必须发送和接收相同类型的消息才能进行通信 。

运行后我们看到的话题类型 `ros2 topic list -t` 请让我们知道每个专题都使用什么信息类型。 `cmd_vel` 主题有类型 :

``` console
geometry_msgs/msg/Twist
```

这意味着在包里 `geometry_msgs` 有一个 `msg` 调用 `Twist`.

现在我们可以跑了 `ros2 interface show <msg_type>` 。具体地说,信息所期望的数据结构。

``` console
$ ros2 interface show geometry_msgs/msg/Twist
```

将返回 :

``` text
# This expresses velocity in free space broken into its linear and angular parts.
    Vector3  linear
            float64 x
            float64 y
            float64 z
    Vector3  angular
            float64 x
            float64 y
            float64 z
```

这告诉你, `/turtlesim` 节点正在等待一个带有两个向量的信息, `linear` 财务报告和财务报告 `angular`,每个元素中有三个元素。如果您记得我们看到的数据 `/teleop_turtle` 转至 `/turtlesim` 与 `echo` 命令,它在同一结构中:

``` console
linear:
  x: 2.0
  y: 0.0
  z: 0.0
angular:
  x: 0.0
  y: 0.0
  z: 0.0
  ---
```

<span id="ros2-topic-pub"></span>

### 7 ros2 主题酒吧

既然您有消息结构,您可以直接从命令行发布数据给一个话题,使用:

``` console
$ ros2 topic pub <topic_name> <msg_type> '<args>'
```

那个... `'<args>'` 参数是您将在前一节中发现的结构中传递到该主题的实际数据。

龟(以及通常用来模拟的真正的机器人)需要稳定的指令流来持续运行。所以,要让龟移动,并保持其移动,您可以使用以下命令。重要的是要注意,这个参数需要输入YAML语法。输入像这样的全部命令:

``` console
$ ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
```

没有命令行选项, `ros2 topic pub` 以 1 Hz 的稳流发布命令。

![](images/pub_stream.png)

有时,您可能只想要发布一次数据到您的话题中(而不是连续发布)。要发布您的命令,只需一次添加 `--once` 选项。

``` console
$ ros2 topic pub --once -w 2 /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
```

`--once` 是一个可选的参数,意思是“发出一条信息然后退出”。

`-w 2` 这是一种可选论点,意思是“等待两个匹配的订阅 ” 。 之所以需要这样做,是因为我们既有“龟兹”,又有“回声” 。

您将在终端中看到以下输出 :

``` console
Waiting for at least 2 matching subscription(s)...
publisher: beginning loop
publishing #1: geometry_msgs.msg.Twist(linear=geometry_msgs.msg.Vector3(x=2.0, y=0.0, z=0.0), angular=geometry_msgs.msg.Vector3(x=0.0, y=0.0, z=1.8))
```

你会看到你的乌龟这样移动:

![](images/pub_once.png)

您可以以图形方式刷新 rqt_graph 。 您可以看到 `ros2 topic pub ...` 节点( E)`/_ros2cli_30358`正在出版。 `/turtle1/cmd_vel` 专题,这两个专题都正在收到。 `ros2 topic echo ...` 节点( E)`/_ros2cli_26646`) 和 `/turtlesim` 现在节点。

![](images/rqt_graph2.png)

终于可以跑步了 `echo` 编辑 `pose` 主题并重新检查 rqt_graph :

``` console
$ ros2 topic echo /turtle1/pose
```

![](images/rqt_graph3.png)

你可以看到, `/turtlesim` 节点也正在发布到 `pose` 专题,新的 `echo` 节点已订阅 。

当发布带有时间戳的信息时, `pub` 有两种方法可以自动填入当前时间。对于带有一个消息的邮件, `std_msgs/msg/Header`,可以设置标题字段为 `auto` 以填写 `stamp` 字段键入。

``` console
$ ros2 topic pub /pose geometry_msgs/msg/PoseStamped '{header: "auto", pose: {position: {x: 1.0, y: 2.0, z: 3.0}}}'
```

如果信件没有使用完整的标题, 但只需有一个字段与类型 `builtin_interfaces/msg/Time`,可以设置为值 `now`.

``` console
$ ros2 topic pub /reference sensor_msgs/msg/TimeReference '{header: "auto", time_ref: "now", source: "dumy"}'
```

<span id="ros2-topic-hz"></span>

### 8 ros2 专题hz

您也可以使用下列方法查看数据公布的速度:

``` console
$ ros2 topic hz /turtle1/pose
average rate: 59.354
  min: 0.005s max: 0.027s std dev: 0.00284s window: 58
```

它将返回关于该物质的速率的数据。 `/turtlesim` 节点正在将数据发布到 `pose` 主题。

记得你设定了 `turtle1/cmd_vel` 以平稳的 1 Hz 发布 `ros2 topic pub --rate 1`。如果您用 `turtle1/cmd_vel` 改为 `turtle1/pose`中,您可以看到反映该比率的平均值。

> **说明**
>
> 该费率反映由基金创建的订阅费的收款率。 `ros2 topic hz` 命令,可能受平台资源和QoS配置的影响,也可能不完全符合出版商的比率。

<span id="ros2-topic-bw"></span>

### 9 ros2 专题体重

主题使用的带宽可以使用:

``` console
$ ros2 topic bw /turtle1/pose
Subscribed to [/turtle1/pose]
1.51 KB/s from 62 messages
    Message size mean: 0.02 KB min: 0.02 KB max: 0.02 KB
```

它返回正在发布的消息的带宽利用率和数量。 `/turtle1/pose` 主题。

> **说明**
>
> 带宽反映用户创建的订阅率。 `ros2 topic bw` 命令,可能受平台资源和QoS配置的影响,也可能不完全符合出版商的带宽。

<span id="ros2-topic-find"></span>

### 找到 10 ros2 主题

要列出一个特定类型使用的现有主题列表:

``` console
$ ros2 topic find <topic_type>
```

回顾: `cmd_vel` 主题有类型 :

``` console
geometry_msgs/msg/Twist
```

使用 `find` 当给定消息类型时命令输出主题 :

``` console
$ ros2 topic find geometry_msgs/msg/Twist
/turtle1/cmd_vel
```

<span id="clean-up"></span>

### 11 清理

此时,您将有很多节点运行。不要忘记通过进入来阻止它们。 `Ctrl+C` 在每个终端。

<span id="summary"></span>

## 小结

节点在主题上发布信息, 这样可以让其他节点订阅和访问该信息。 在此教程中, 您使用 rqt\_ graph 和命令行工具检查了多个主题的节点之间的关联 。 您现在应该对数据如何围绕 ROS 2 系统移动有一个很好的了解 。

<span id="next-steps"></span>

## 后续步骤

接下来您将会在 ROS 图表中与教程学习另一个通信类型 [理解服务](../Understanding-ROS2-Services/Understanding-ROS2-Services.md).
