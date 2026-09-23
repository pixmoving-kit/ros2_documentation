---
translation_status: machine_translated
source: Tutorials/Advanced/Simulators/Gazebo/Gazebo.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="setting-up-a-robot-simulation-gazebo"></span>

# 配置机器人仿真（Gazebo）

**目标：** 与Gazebo和ROS 2进行模拟

**教程级别：** 高级

**用时：** 20分钟

<span id="prerequisites"></span>

## 前提条件

首先,你应该安装ROS 2和Gazebo。

> - 从 deb 包安装。 要检查哪些版本可以从 deb 包中获取, 请检查此 [表格显示](https://github.com/gazebosim/ros_ign).
>
> - 来源汇编:
>
>   - [ROS 2 安装指令](../../../../Installation.md)
>
>   - [Gazebo 安装指令](https://gazebosim.org/docs)

<span id="tasks"></span>

## 操作步骤

<span id="launch-the-simulation"></span>

### 1 启动模拟

在此演示中, 您将在 Gazebo 模拟一个简单的 diff 驱动机器人。 您将使用 Gazebo 示例中定义的世界之一 。 [visualize_lidar.sdf](https://github.com/gazebosim/gz-sim/blob/main/examples/worlds/visualize_lidar.sdf)。要运行此示例,您应该在终端中执行以下命令:

[ROS REP-2000](https://reps.openrobotics.org/rep-2000/) 规范每个ROS发行的Gazebo默认版本是什么.

##### Linux

``` console
$ ign gazebo -v 4 -r visualize_lidar.sdf
```

![](Image/gazebo_diff_drive.png)

当模拟运行时,您可以用 `ign` 命令行工具 :

##### Linux

``` console
$ ign topic -l
/clock
/gazebo/resource_paths
/gui/camera/pose
/gui/record_video/stats
/model/vehicle_blue/odometry
/model/vehicle_blue/tf
/stats
/world/visualize_lidar_world/clock
/world/visualize_lidar_world/dynamic_pose/info
/world/visualize_lidar_world/pose/info
/world/visualize_lidar_world/scene/deletion
/world/visualize_lidar_world/scene/info
/world/visualize_lidar_world/state
/world/visualize_lidar_world/stats
```

由于您尚未启动 ROS 2 节点, 输出来自 `ros2 topic list` 应无任何机器人主题:

##### Linux

``` console
$ ros2 topic list
/parameter_events
/rosout
```

<span id="configuring-ros-2"></span>

### 2 配置 ROS 2

为了能够与ROS 2 进行模拟,你需要使用一个名为“模拟”的软件包。 `ros_gz_bridge`。本软件包提供了一个网络桥,可以让ROS 2和Gazebo Transport之间交换消息。您可以通过打字安装此软件包:

##### Linux

``` console
$ sudo apt-get install ros-rolling-ros-ign-bridge
```

此时,您准备启动从ROS到Gazebo的桥梁,特别是您将为此主题创建一座桥梁。 `/model/vehicle_blue/cmd_vel`:

##### Linux

``` console
$ source /opt/ros/rolling/setup.bash
$ ros2 run ros_gz_bridge parameter_bridge /model/vehicle_blue/cmd_vel@geometry_msgs/msg/Twist]ignition.msgs.Twist
```

欲了解更多关于 `ros_gz_bridge` 请检查一下 [读取](https://github.com/gazebosim/ros_gz/tree/ros2/ros_gz_bridge) .

一旦桥开始运行,机器人就能够遵循你的运动指令。有两种选择:

- 使用 `ros2 topic pub`

> ##### Linux
>
> ``` console
> $ ros2 topic pub /model/vehicle_blue/cmd_vel geometry_msgs/Twist "linear: { x: 0.1 }"
> ```

- `teleop_twist_keyboard` 软件包。此节点从键盘中取出按键后作为Twist消息发布。 您可以安装它打字 :

> ##### Linux
>
> ``` console
> $ sudo apt-get install ros-rolling-teleop-twist-keyboard
> ```
>
> 默认主题 `teleop_twist_keyboard` 正在发布 Twist 消息 `/cmd_vel` 但您可以重新绘制这个话题, 以利用桥中所使用的主题 :
>
> ##### Linux
>
> ``` console
> $ source /opt/ros/rolling/setup.bash
> $ ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -r /cmd_vel:=/model/vehicle_blue/cmd_vel
> This node takes keypresses from the keyboard and publishes them
> as Twist messages. It works best with a US keyboard layout.
> ---------------------------
> Moving around:
>    u    i    o
>    j    k    l
>    m    ,    .
>
> For Holonomic mode (strafing), hold down the shift key:
> ---------------------------
>    U    I    O
>    J    K    L
>    M    <    >
>
> t : up (+z)
> b : down (-z)
>
> anything else : stop
>
> q/z : increase/decrease max speeds by 10%
> w/x : increase/decrease only linear speed by 10%
> e/c : increase/decrease only angular speed by 10%
>
> CTRL-C to quit
>
> currently:      speed 0.5       turn 1.0
> ```

<span id="visualizing-lidar-data-in-ros-2"></span>

### 3 可视化 ROS 2 的 lidar 数据

diff 驱动机器人有一个 lidar 。 要将 Gazebo 生成的数据发送到 ROS 2, 您需要启用另一座桥。 如果 lidar 的数据是在 Gazebo  Transport 主题中提供的 `/lidar2`,您将在桥上重新绘制。此主题将在主题下提供 `/lidar_scan`:

##### Linux

``` console
$ source /opt/ros/rolling/setup.bash
$ ros2 run ros_gz_bridge parameter_bridge /lidar2@sensor_msgs/msg/LaserScan[ignition.msgs.LaserScan --ros-args -r /lidar2:=/laser_scan
```

要可视化 ROS 2 中的 lidar 数据, 您可以使用 Rviz2 :

##### Linux

``` console
$ source /opt/ros/rolling/setup.bash
$ rviz2
```

那你需要配置 `fixed frame`:

![](Image/fixed_frame.png)

然后点击按钮“ 添加” , 包含一个可视化 lidar 的显示:

![](Image/add_lidar.png)

现在你应该看看Rviz2的Lidar的数据:

![](../../../Intermediate/RViz/RViz-Custom-Panel/images/RViz2.png) <span id="summary"></span>

## 小结

在这个教程中,你与Gazebo一起推出了机器人模拟,用起动器和传感器发射桥梁,从传感器中可视化数据,并移动了diff驱动机器人.
