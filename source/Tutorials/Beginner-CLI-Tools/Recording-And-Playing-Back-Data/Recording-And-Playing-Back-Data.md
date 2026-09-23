<span id="recording-and-playing-back-data"></span> <span id="ros2bag"></span>
# 录制与回放数据

**目标：** 录制话题上发布的数据，以便随时回放和检查。

**教程级别：** 初级

**预计用时：** 10 分钟

<span id="background"></span>
## 背景

`ros2 bag` 是一个命令行工具，用于录制系统中话题上发布的数据。它可以收集任意数量话题上传递的数据，并保存到数据库中。之后可以回放这些数据，复现测试和实验结果。录制话题也是分享工作成果、让他人复现实验的好方法。

<span id="prerequisites"></span>
## 前提条件

正常安装 ROS 2 时，应已安装 `ros2 bag`。如果尚未安装 ROS 2，请参见[安装说明](../../../Installation.md)。

本教程涉及之前教程中介绍的[节点](../Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md)和[话题](../Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)等概念，也会使用 [turtlesim 软件包](../Introducing-Turtlesim/Introducing-Turtlesim.md)。

和之前一样，不要忘记在[每个新打开的终端](../Configuring-ROS2-Environment.md)中加载（source）ROS 2 环境。

<span id="tasks"></span>
## 任务

<span id="setup"></span>
### 1 准备工作

接下来将录制 `turtlesim` 系统中的键盘输入，保存后再回放。因此，先启动 `/turtlesim` 和 `/teleop_turtle` 节点。

打开新终端并运行：

```console
$ ros2 run turtlesim turtlesim_node
```

再打开一个终端并运行：

```console
$ ros2 run turtlesim turtle_teleop_key
```

为保持文件有序，再创建一个目录来保存录制的数据。

**Linux：**

```console
$ mkdir bag_files
$ cd bag_files
```

**macOS：**

```console
$ mkdir bag_files
$ cd bag_files
```

**Windows：**

```console
$ md bag_files
$ cd bag_files
```

<span id="choose-a-topic"></span>
### 2 选择话题

`ros2 bag` 只能录制话题上发布的消息数据。打开新终端并运行以下命令，查看系统中的话题列表：

```console
$ ros2 topic list
/parameter_events
/rosout
/turtle1/cmd_vel
/turtle1/color_sensor
/turtle1/pose
```

在话题教程中，你已经了解遥控节点会向 `/turtle1/cmd_vel` 话题发布命令，使 turtlesim 中的海龟运动。

运行以下命令，查看 `/turtle1/cmd_vel` 上发布的数据：

```console
$ ros2 topic echo /turtle1/cmd_vel
```

一开始没有任何输出，因为遥控节点尚未发布数据。回到运行遥控程序的终端，并选中该窗口，使其处于活动状态。使用方向键控制海龟运动，就会在运行 `ros2 topic echo` 的终端中看到发布的数据：

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

<span id="ros2-bag-record"></span>
### 3 ros2 bag record

<span id="record-a-single-topic"></span>
#### 3.1 录制单个话题

录制某个话题上发布的数据，使用以下命令语法：

```console
$ ros2 bag record <topic_name>
```

在对选定话题运行该命令前，先打开新终端，进入之前创建的 `bag_files` 目录，因为 rosbag 文件会保存在运行命令时所在的目录中。

运行：

```console
$ ros2 bag record /turtle1/cmd_vel
[INFO] [rosbag2_storage]: Opened database 'rosbag2_2019_10_11-05_18_45'.
[INFO] [rosbag2_transport]: Listening for topics...
[INFO] [rosbag2_transport]: Subscribed to topic '/turtle1/cmd_vel'
[INFO] [rosbag2_transport]: All requested topics are subscribed. Stopping discovery...
```

现在，`ros2 bag` 正在录制 `/turtle1/cmd_vel` 话题上发布的数据。回到遥控终端，再次控制海龟运动。具体如何移动并不重要，但可以尝试画出容易辨认的轨迹，方便之后回放时观察。

![录制海龟运动轨迹](images/record.png)

按 `Ctrl+C` 停止录制。

数据会保存在新建的 bag 目录中，目录名称采用 `rosbag2_year_month_day-hour_minute_second` 的形式。目录中包含一个 `metadata.yaml`，以及相应录制格式的 bag 文件。

<span id="record-multiple-topics"></span>
#### 3.2 录制多个话题

也可以同时录制多个话题，并修改 `ros2 bag` 保存的文件名称。运行：

```console
$ ros2 bag record -o subset /turtle1/cmd_vel /turtle1/pose
[INFO] [rosbag2_storage]: Opened database 'subset'.
[INFO] [rosbag2_transport]: Listening for topics...
[INFO] [rosbag2_transport]: Subscribed to topic '/turtle1/cmd_vel'
[INFO] [rosbag2_transport]: Subscribed to topic '/turtle1/pose'
[INFO] [rosbag2_transport]: All requested topics are subscribed. Stopping discovery...
```

`-o` 选项允许指定 bag 文件的名称，紧随其后的字符串就是文件名，这里是 `subset`。

要同时录制多个话题，只需用空格分隔并依次列出各话题。上面的命令输出确认了两个话题都已开始录制。

控制海龟运动，完成后按 `Ctrl+C`。

!!! note "说明"
    还可以为命令添加 `-a` 选项，录制系统中的所有话题。

<span id="ros2-bag-info"></span>
### 4 ros2 bag info

运行以下命令，可以查看录制数据的详细信息：

```console
$ ros2 bag info <bag_file_name>
```

对 `subset` bag 文件运行该命令，会返回以下文件信息：

```console
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
### 5 ros2 bag play

回放 bag 文件之前，在运行遥控程序的终端中按 `Ctrl+C`，然后确保 turtlesim 窗口可见，以便观察回放效果。

运行：

```console
$ ros2 bag play subset
[INFO] [rosbag2_storage]: Opened database 'subset'.
```

海龟会沿着录制时的路径运动，不过不会百分之百一致，因为 turtlesim 对系统时序的微小变化比较敏感。

![回放海龟运动轨迹](images/playback.png)

由于 `subset` 文件还录制了 `/turtle1/pose` 话题，`ros2 bag play` 会持续回放录制期间 turtlesim 运行的整段时间，即使其中某些时段海龟没有移动。

这是因为，只要 `/turtlesim` 节点处于活动状态，就会定期向 `/turtle1/pose` 话题发布数据。前面的 `ros2 bag info` 示例中，`/turtle1/cmd_vel` 的 `Count` 只有 9，这就是录制时按方向键的次数。

相比之下，`/turtle1/pose` 的 `Count` 超过 3000，说明录制期间该话题发布了 3000 多条数据。

要了解位置数据的发布频率，可以运行：

```console
$ ros2 topic hz /turtle1/pose
```

<span id="summary"></span>
## 小结

使用 `ros2 bag` 命令，可以录制 ROS 2 系统中通过话题传递的数据。无论是与他人分享成果，还是检查自己的实验过程，它都是一个实用工具。

<span id="next-steps"></span>
## 后续步骤

你已完成“初级：命令行工具”系列教程！接下来可以学习“初级：客户端库”系列，从[创建工作空间](../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md)开始。

<span id="related-content"></span>
## 相关内容

关于 `ros2 bag` 的更详细说明，见 [rosbag2 的 README](https://github.com/ros2/rosbag2)。有关 QoS 兼容性与 `ros2 bag` 的更多信息，见[覆盖录制和回放的 QoS 策略](../../../How-To-Guides/Overriding-QoS-Policies-For-Recording-And-Playback.md)。
