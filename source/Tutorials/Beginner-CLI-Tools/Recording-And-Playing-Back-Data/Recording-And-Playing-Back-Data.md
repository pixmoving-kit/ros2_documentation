---
translation_status: machine_translated
source: Tutorials/Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="recording-and-playing-back-data"></span> <span id="ros2bag"></span>

# 录制与回放数据

**目标：** 记录一个话题上公布的数据,以便您可以随时重放和检查.

**教程级别：** 入门

**用时：** 10分钟

<span id="background"></span>

## 背景

`ros2 bag` 是一个命令行工具,用于记录在您系统中发布的主题上的数据。它可以累积传递到任意几个主题上的数据,并将其保存在一个数据库中。然后可以重放数据来复制测试和实验的结果。记录主题也是分享您的工作并允许其他人重新创建它的一个大方法。

<span id="prerequisites"></span>

## 前提条件

你应该有 `ros2 bag` 作为常规ROS 2设置的一部分安装.

如果需要安装ROS 2,请查看 [安装指令](../../../Installation.md).

这个教程讲述了以前教程中包含的概念, 比如: [节点](../Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md) 财务报告和财务报告 [话题](../Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)。它还使用 [龟兹包](../Introducing-Turtlesim/Introducing-Turtlesim.md).

与往常一样, [您打开的每个新终端](../Configuring-ROS2-Environment.md).

<span id="tasks"></span>

## 操作步骤

<span id="setup"></span>

### 1 设置

您将会将您的键盘输入录入 `turtlesim` 用于保存和稍后重播的系统,所以从启动 `/turtlesim` 财务报告和财务报告 `/teleop_turtle` 节点。

打开新的终端并运行 :

``` console
$ ros2 run turtlesim turtlesim_node
```

打开另一个终端并运行 :

``` console
$ ros2 run turtlesim turtle_teleop_key
```

让我们再做一个新目录来保存保存的录音,

##### Linux

``` console
$ mkdir bag_files
$ cd bag_files
```

##### macOS

``` console
$ mkdir bag_files
$ cd bag_files
```

##### Windows

``` console
$ md bag_files
$ cd bag_files
```

<span id="choose-a-topic"></span>

### 2 选择主题

`ros2 bag` 只能记录主题中已发布消息的数据。要查看您的系统主题列表,请打开一个新的终端并运行命令 :

``` console
$ ros2 topic list
/parameter_events
/rosout
/turtle1/cmd_vel
/turtle1/color_sensor
/turtle1/pose
```

在专题辅导中,你学到了 `/turtle_teleop` 节点发布命令 `/turtle1/cmd_vel` 使龟在龟兹移动的话题。

来查看数据 `/turtle1/cmd_vel` 正在发布, 运行命令 :

``` console
$ ros2 topic echo /turtle1/cmd_vel
```

起初,由于Teleop没有发布数据,所以不会出现任何东西。返回运行Teleop的终端,然后选择它。使用箭头键来移动龟类,你会看到终端运行中的数据正在发布 `ros2 topic echo`.

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

<span id="ros2-bag-record"></span>

### 3个罗斯2袋记录

<span id="record-a-single-topic"></span>

#### 3.1 记录一个单一专题

要记录发布到一个主题的数据,使用命令语法:

``` console
$ ros2 bag record <topic_name>
```

在运行您所选主题的命令之前, 请打开一个新的终端并移动到 `bag_files` 您早些时候创建的目录, 因为 Rosbag 文件会保存在您运行时的目录中 。

运行命令 :

``` console
$ ros2 bag record /turtle1/cmd_vel
[INFO] [rosbag2_storage]: Opened database 'rosbag2_2019_10_11-05_18_45'.
[INFO] [rosbag2_transport]: Listening for topics...
[INFO] [rosbag2_transport]: Subscribed to topic '/turtle1/cmd_vel'
[INFO] [rosbag2_transport]: All requested topics are subscribed. Stopping discovery...
```

现在 `ros2 bag` 正在录制该数据库公布的数据。 `/turtle1/cmd_vel` 主题 。 返回到 Teleop 终端, 再移动龟类 。 这些移动无关紧要, 但尝试做一个可识别的图案, 以查看您何时重播数据 。

![](images/record.png)

新闻 `Ctrl+C` 停止录音。

数据将累积在一个新的包目录中,其名称为: `rosbag2_year_month_day-hour_minute_second`。此目录将包含 `metadata.yaml` 与记录格式的袋文件一起。

<span id="record-multiple-topics"></span>

#### 3.2 记录多个专题

您也可以记录多个主题, 以及更改文件名称 `ros2 bag` 保存为。

运行以下命令 :

``` console
$ ros2 bag record -o subset /turtle1/cmd_vel /turtle1/pose
[INFO] [rosbag2_storage]: Opened database 'subset'.
[INFO] [rosbag2_transport]: Listening for topics...
[INFO] [rosbag2_transport]: Subscribed to topic '/turtle1/cmd_vel'
[INFO] [rosbag2_transport]: Subscribed to topic '/turtle1/pose'
[INFO] [rosbag2_transport]: All requested topics are subscribed. Stopping discovery...
```

那个... `-o` 选项允许您为您的包文件选择一个独有的名称。在此情况下,下面的字符串 `subset`,是文件名。

要一次记录一个以上的话题,只需列出每个被空格分隔的话题。在这种情况下,上面的命令输出确认这两个话题都在记录中。

你可以移动海龟 周围按 `Ctrl+C` 当您完成时。

> **说明**
>
> 还有一个选项你可以添加到命令中, `-a`,它记录了您系统中的所有话题。

<span id="ros2-bag-info"></span>

### 4个ROS2袋信息

您可以通过运行查看您的录音细节 :

``` console
$ ros2 bag info <bag_file_name>
```

运行此命令 `subset` 包文件将返回文件中的信息列表 :

``` console
$ ros2 bag info subset
Files:             subset.db3
Bag size:          228.5 KiB
Storage id:        sqlite3
Duration:          48.47s
Start:             Oct 11 2019 06:09:09.12 (1570799349.12)
End                Oct 11 2019 06:09:57.60 (1570799397.60)
Messages:          3013
Topic information: Topic: /turtle1/cmd_vel | Type: geometry_msgs/msg/Twist | Count: 9 | Serialization Format: cdr
                   Topic: /turtle1/pose | Type: turtlesim/msg/Pose | Count: 3004 | Serialization Format: cdr
```

<span id="ros2-bag-play"></span>

### 5个罗斯2袋游戏

在重放包文件之前, 请输入 `Ctrl+C` 在 teleop 运行的终端中。然后确保您的 topsim 窗口可见, 以便您看到正在操作的 bag 文件 。

输入命令 :

``` console
$ ros2 bag play subset
[INFO] [rosbag2_storage]: Opened database 'subset'.
```

您的海龟会遵循您在录制时输入的同样路径( 虽然并非100% ; 龟头对系统时间的微小变化敏感 ) 。

![](images/playback.png)

因为 `subset` 记录文件 `/turtle1/pose` 专题,主题 `ros2 bag play` 命令不会退出, 只要你有龟兹姆运行, 即使你没有移动。

这是因为,只要 `/turtlesim` 节点活动,它发布关于该节点的数据 `/turtle1/pose` 时段主题。您可能在 `ros2 bag info` 以上实例结果 `/turtle1/cmd_vel` 专题 `Count` 仅九次; 这就是我们记录时按箭头键的次数。

请注意: `/turtle1/pose` 拥有 `Count` 价值超过3000;在我们录制时,已公布了3000次有关该主题的数据。

要了解位置数据的发布频率, 您可以运行命令 :

``` console
$ ros2 topic hz /turtle1/pose
```

<span id="summary"></span>

## 小结

您可以使用 ROS 2 系统记录所传送的主题数据 。 `ros2 bag` 命令。无论你与他人分享你的工作,还是回顾自己的实验,它都是了解的伟大工具。

<span id="next-steps"></span>

## 后续步骤

您已完成了“ 初学者: CLI 工具” 教程。 下一步是解决“ 初学者: 客户端库” 教程, 首先是 [创建工作空间](../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md).

<span id="related-content"></span>

## 相关内容

更彻底的解释 `ros2 bag` 可在 README 中找到 [这儿](https://github.com/ros2/rosbag2)。关于 QoS 兼容性和 `ros2 bag`,见 [rosbag2：覆盖 QoS 策略](../../../How-To-Guides/Overriding-QoS-Policies-For-Recording-And-Playback.md).
