<span id="understanding-topics"></span> <span id="ros2topics"></span>
# 理解话题

**目标：** 使用 rqt_graph 和命令行工具查看 ROS 2 话题及其内部信息。

**教程级别：** 初学者

**预计用时：** 20 分钟

<span id="background"></span>
## 背景

ROS 2 将复杂系统拆分为多个模块化节点。话题是 ROS 计算图的重要组成部分，充当节点之间交换消息的总线。

![单个发布者和单个订阅者](images/Topic-SinglePublisherandSingleSubscriber.gif)

一个节点可以向任意数量的话题发布数据，同时订阅任意数量的话题。

![多个发布者和多个订阅者](images/Topic-MultiplePublisherandMultipleSubscriber.gif)

话题是节点之间传递数据的主要方式之一，因此也是系统不同部分之间传递数据的主要方式之一。

<span id="prerequisites"></span>
## 前提条件

本教程会用到[上一篇教程](../Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md)中介绍的节点基础知识。

与往常一样，不要忘记在[每个新打开的终端](../Configuring-ROS2-Environment.md)中加载 ROS 2 环境。

<span id="tasks"></span>
## 操作步骤

<span id="setup"></span>
### 1 准备工作

现在你应该已经熟悉如何启动 turtlesim。

打开新终端，运行：

```console
$ ros2 run turtlesim turtlesim_node
```

打开另一个终端，运行：

```console
$ ros2 run turtlesim turtle_teleop_key
```

根据[上一篇教程](../Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md)，这两个节点的默认名称分别是 `/turtlesim` 和 `/teleop_turtle`。

<span id="rqt-graph"></span>
### 2 rqt_graph

本教程使用 `rqt_graph`，以图形方式观察节点、话题及其连接的变化。

[turtlesim 教程](../Introducing-Turtlesim/Introducing-Turtlesim.md)介绍了如何安装 rqt 及全部插件，其中包括 `rqt_graph`。

打开新终端，输入以下命令运行 rqt_graph：

```console
$ ros2 run rqt_graph rqt_graph
```

也可以打开 `rqt`，选择 **Plugins > Introspection > Node Graph**。

![rqt_graph 中的节点与话题](images/rqt_graph.png)

你应当能看到上图中的节点和话题，以及图外围的两个动作，暂时可以忽略这些动作。将鼠标悬停在中央的话题上，会看到与上图类似的高亮效果。

图中展示了 `/turtlesim` 节点与 `/teleop_turtle` 节点如何通过话题通信。`/teleop_turtle` 将你用于移动海龟的按键所对应的数据发布到 `/turtle1/cmd_vel` 话题，`/turtlesim` 则订阅该话题以接收数据。

对于包含许多节点和话题、连接关系复杂的系统，rqt_graph 的高亮功能非常有助于检查连接。

rqt_graph 是图形化的内部状态查看工具。接下来介绍用于查看话题的命令行工具。

<span id="ros2-topic-list"></span>
### 3 ros2 topic list

在新终端中运行 `ros2 topic list`，会返回系统中当前所有活动话题的列表：

```console
$ ros2 topic list
/parameter_events
/rosout
/turtle1/cmd_vel
/turtle1/color_sensor
/turtle1/pose
```

`ros2 topic list -t` 会返回相同的话题列表，并在每个话题后面的方括号中附上话题类型：

```console
$ ros2 topic list -t
/parameter_events [rcl_interfaces/msg/ParameterEvent]
/rosout [rcl_interfaces/msg/Log]
/turtle1/cmd_vel [geometry_msgs/msg/Twist]
/turtle1/color_sensor [turtlesim/msg/Color]
/turtle1/pose [turtlesim/msg/Pose]
```

节点通过这些属性，尤其是类型，确认话题上传递的信息具有一致的含义。

如果想在 rqt_graph 中看到这些话题，可以取消勾选 **Hide:** 下的所有选项：

![显示隐藏的话题](images/unhide.png)

不过，为避免混淆，目前可以让这些选项保持勾选状态。

<span id="ros2-topic-echo"></span>
### 4 ros2 topic echo

使用以下命令查看话题中发布的数据：

```console
$ ros2 topic echo <topic_name>
```

已经知道 `/teleop_turtle` 通过 `/turtle1/cmd_vel` 话题向 `/turtlesim` 发布数据，因此可以用 `echo` 查看这个话题：

```console
$ ros2 topic echo /turtle1/cmd_vel
```

最初命令不会显示任何数据，因为它正在等待 `/teleop_turtle` 发布消息。

回到运行 `turtle_teleop_key` 的终端，用方向键移动海龟。同时观察运行 `echo` 的终端，就能看到每次移动时发布的数据：

```console
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

现在回到 rqt_graph，取消勾选 **Debug**：

![显示 echo 命令创建的节点](images/debug.png)

`/_ros2cli_26646` 是刚才执行 `echo` 命令时创建的节点，具体数字可能不同。现在可以看到，一个发布者通过 `cmd_vel` 话题发布数据，而两个订阅者订阅了这个话题。

<span id="ros2-topic-info"></span>
### 5 ros2 topic info

话题不局限于一对一通信，也可以是一对多、多对一或多对多。

还可以通过以下命令查看这种关系：

```console
$ ros2 topic info /turtle1/cmd_vel
Type: geometry_msgs/msg/Twist
Publisher count: 1
Subscription count: 2
```

<span id="ros2-topic-info-verbose"></span>
#### 5.1 ros2 topic info --verbose

要查看话题的更多细节，可以使用 `--verbose` 参数，或其缩写 `-v`：

```console
$ ros2 topic info /turtle1/cmd_vel --verbose
```

输出会包含以下额外信息：

- 发布者和订阅者所属节点的名称与命名空间
- 话题类型
- QoS 配置

```console
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
### 6 ros2 interface show

节点通过话题发送消息来传递数据。发布者和订阅者必须使用相同的消息类型才能通信。

之前运行 `ros2 topic list -t` 时看到的话题类型，就是各个话题所使用的消息类型。回顾一下，`cmd_vel` 话题的类型为：

```console
geometry_msgs/msg/Twist
```

这表示 `geometry_msgs` 软件包中有一个名为 `Twist` 的消息（`msg`）类型。

现在可以对该类型执行 `ros2 interface show <msg_type>`，了解它的详细定义，特别是消息要求的数据结构：

```console
$ ros2 interface show geometry_msgs/msg/Twist
```

输出如下：

```text
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

这表明 `/turtlesim` 节点需要的消息包含 `linear` 和 `angular` 两个向量，每个向量各有三个元素。回顾通过 `echo` 命令看到的、由 `/teleop_turtle` 传给 `/turtlesim` 的数据，其结构与这里一致：

```console
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
### 7 ros2 topic pub

了解消息结构后，可以直接通过命令行向话题发布数据：

```console
$ ros2 topic pub <topic_name> <msg_type> '<args>'
```

`'<args>'` 是实际传给话题的数据，结构应符合上一节中查看到的消息定义。

海龟需要持续接收命令才能连续运动，它所模拟的真实机器人通常也是如此。因此，要让海龟开始并持续移动，可以使用以下命令。消息参数必须使用 YAML 语法。输入完整命令：

```console
$ ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
```

不添加其他命令行选项时，`ros2 topic pub` 会以 1 Hz 的固定频率持续发布命令。

![持续发布命令时海龟的轨迹](images/pub_stream.png)

有时只需向话题发布一次数据，而不希望持续发布。此时可添加 `--once` 选项：

```console
$ ros2 topic pub --once -w 2 /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
```

`--once` 是可选参数，表示“发布一条消息后退出”。

`-w 2` 也是可选参数，表示“等待两个匹配的订阅”。这里需要它，是因为 turtlesim 和 topic echo 都订阅了该话题。

终端中会显示：

```console
Waiting for at least 2 matching subscription(s)...
publisher: beginning loop
publishing #1: geometry_msgs.msg.Twist(linear=geometry_msgs.msg.Vector3(x=2.0, y=0.0, z=0.0), angular=geometry_msgs.msg.Vector3(x=0.0, y=0.0, z=1.8))
```

海龟会像下图那样移动：

![只发布一次命令后的海龟](images/pub_once.png)

刷新 rqt_graph，可以从图中观察通信情况。`ros2 topic pub ...` 创建的节点 `/_ros2cli_30358` 正在向 `/turtle1/cmd_vel` 话题发布消息；`ros2 topic echo ...` 创建的节点 `/_ros2cli_26646` 和 `/turtlesim` 节点都在接收这些消息。

![命令行发布者和两个订阅者](images/rqt_graph2.png)

最后，对 `pose` 话题运行 `echo`，再检查 rqt_graph：

```console
$ ros2 topic echo /turtle1/pose
```

![订阅 pose 话题](images/rqt_graph3.png)

可以看到，`/turtlesim` 节点还会向 `pose` 话题发布数据，而新创建的 `echo` 节点订阅了这个话题。

发布带时间戳的消息时，`pub` 提供两种自动填入当前时间的方法。如果消息包含 `std_msgs/msg/Header`，可以将 header 字段设为 `auto`，自动填写其中的 `stamp`：

```console
$ ros2 topic pub /pose geometry_msgs/msg/PoseStamped '{header: "auto", pose: {position: {x: 1.0, y: 2.0, z: 3.0}}}'
```

如果消息没有完整的 header，而是只有一个 `builtin_interfaces/msg/Time` 类型的字段，可以将该字段设为 `now`：

```console
$ ros2 topic pub /reference sensor_msgs/msg/TimeReference '{header: "auto", time_ref: "now", source: "dumy"}'
```

<span id="ros2-topic-hz"></span>
### 8 ros2 topic hz

还可以用以下命令查看数据发布频率：

```console
$ ros2 topic hz /turtle1/pose
average rate: 59.354
  min: 0.005s max: 0.027s std dev: 0.00284s window: 58
```

输出反映了 `/turtlesim` 节点向 `pose` 话题发布数据的频率。

`ros2 topic pub --rate 1` 可以将 `turtle1/cmd_vel` 的发布频率设为固定的 1 Hz。将上面命令中的 `turtle1/pose` 替换为 `turtle1/cmd_vel`，就会看到与该频率相应的平均值。

!!! note "注意"
    这里显示的是 `ros2 topic hz` 创建的订阅实际接收消息的频率。它可能受到平台资源和 QoS 配置的影响，不一定与发布者的频率完全一致。

<span id="ros2-topic-bw"></span>
### 9 ros2 topic bw

使用以下命令查看话题占用的带宽：

```console
$ ros2 topic bw /turtle1/pose
Subscribed to [/turtle1/pose]
1.51 KB/s from 62 messages
    Message size mean: 0.02 KB min: 0.02 KB max: 0.02 KB
```

输出包含 `/turtle1/pose` 话题的带宽使用情况和消息数量。

!!! note "注意"
    这里显示的是 `ros2 topic bw` 创建的订阅实际接收消息时的带宽。它可能受到平台资源和 QoS 配置的影响，不一定与发布者使用的带宽完全一致。

<span id="ros2-topic-find"></span>
### 10 ros2 topic find

使用以下命令列出指定类型的可用话题：

```console
$ ros2 topic find <topic_type>
```

回顾一下，`cmd_vel` 话题的类型是：

```console
geometry_msgs/msg/Twist
```

将消息类型传给 `find` 命令，即可列出对应的可用话题：

```console
$ ros2 topic find geometry_msgs/msg/Twist
/turtle1/cmd_vel
```

<span id="clean-up"></span>
### 11 清理

此时已经运行了许多节点。别忘了在各个终端中按 `Ctrl+C` 停止它们。

<span id="summary"></span>
## 小结

节点通过话题发布信息，任意数量的其他节点都可以通过订阅获取这些信息。本教程使用 rqt_graph 和命令行工具，检查了多个节点通过话题建立的连接。现在你应该已经基本了解数据如何在 ROS 2 系统中流动。

<span id="next-steps"></span>
## 后续步骤

接下来通过[理解服务](../Understanding-ROS2-Services/Understanding-ROS2-Services.md)，学习 ROS 计算图中的另一种通信方式。
