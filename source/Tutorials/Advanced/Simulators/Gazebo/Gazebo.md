<span id="setting-up-a-robot-simulation-gazebo"></span>
<span id="prerequisites"></span>
<span id="tasks"></span>
<span id="launch-the-simulation"></span>
<span id="configuring-ros-2"></span>
<span id="visualizing-lidar-data-in-ros-2"></span>
<span id="summary"></span>

# 设置机器人仿真（Gazebo）

**目标：** 使用 Gazebo 和 ROS 2 启动仿真。

**教程级别：** 高级

**耗时：** 20 分钟

## 前提条件

首先安装 ROS 2 和 Gazebo，有两种选择：

- 安装 deb 软件包。各版本的可用情况见[此表](https://github.com/gazebosim/ros_ign)。
- 从源码编译，参阅 [ROS 2 安装说明](../../../../Installation.md)和 [Gazebo 安装说明](https://gazebosim.org/docs)。

## 任务

### 1 启动仿真

本演示在 Gazebo 中仿真一台简单的差速驱动机器人，使用 Gazebo 示例世界 [visualize_lidar.sdf](https://github.com/gazebosim/gz-sim/blob/main/examples/worlds/visualize_lidar.sdf)。在终端运行以下命令即可启动。

[ROS REP-2000](https://reps.openrobotics.org/rep-2000/) 规定了各 ROS 发行版默认使用的 Gazebo 版本。

Linux：

```console
$ ign gazebo -v 4 -r visualize_lidar.sdf
```

![Gazebo 差速驱动机器人](Image/gazebo_diff_drive.png)

仿真运行后，可以通过 `ign` 命令行工具查看 Gazebo 提供的话题。

Linux：

```console
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

此时尚未启动 ROS 2 节点，因此 `ros2 topic list` 输出中不应包含任何机器人话题。

Linux：

```console
$ ros2 topic list
/parameter_events
/rosout
```

### 2 配置 ROS 2

要让仿真与 ROS 2 通信，需要使用 `ros_gz_bridge` 软件包。它提供网络桥接功能，使 ROS 2 与 Gazebo Transport 能够交换消息。安装命令如下。

Linux：

```console
$ sudo apt-get install ros-rolling-ros-ign-bridge
```

现在可以启动从 ROS 到 Gazebo 的桥接。这里为 `/model/vehicle_blue/cmd_vel` 话题创建桥接。

Linux：

```console
$ source /opt/ros/rolling/setup.bash
$ ros2 run ros_gz_bridge parameter_bridge /model/vehicle_blue/cmd_vel@geometry_msgs/msg/Twist]ignition.msgs.Twist
```

有关 `ros_gz_bridge` 的详情，参阅 [README](https://github.com/gazebosim/ros_gz/tree/ros2/ros_gz_bridge)。

桥接运行后，机器人便能响应运动指令。可以选择以下两种方式。

使用 `ros2 topic pub` 向话题发送指令（Linux）：

```console
$ ros2 topic pub /model/vehicle_blue/cmd_vel geometry_msgs/Twist "linear: { x: 0.1 }"
```

或者使用 `teleop_twist_keyboard` 软件包。该节点接收键盘输入，并将其发布为 Twist 消息。安装命令（Linux）：

```console
$ sudo apt-get install ros-rolling-teleop-twist-keyboard
```

`teleop_twist_keyboard` 默认向 `/cmd_vel` 发布 Twist 消息，可以将该话题重映射到桥接使用的话题。

Linux：

```console
$ source /opt/ros/rolling/setup.bash
$ ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -r /cmd_vel:=/model/vehicle_blue/cmd_vel
This node takes keypresses from the keyboard and publishes them
as Twist messages. It works best with a US keyboard layout.
---------------------------
Moving around:
   u    i    o
   j    k    l
   m    ,    .

For Holonomic mode (strafing), hold down the shift key:
---------------------------
   U    I    O
   J    K    L
   M    <    >

t : up (+z)
b : down (-z)

anything else : stop

q/z : increase/decrease max speeds by 10%
w/x : increase/decrease only linear speed by 10%
e/c : increase/decrease only angular speed by 10%

CTRL-C to quit

currently:      speed 0.5       turn 1.0
```

### 3 在 ROS 2 中可视化激光雷达数据

差速驱动机器人配有激光雷达。要将 Gazebo 生成的数据发送到 ROS 2，需要启动另一个桥接。此例中，激光雷达数据通过 Gazebo Transport 的 `/lidar2` 话题提供，需要在桥接时重映射。原文将映射后的话题描述为 `/lidar_scan`，下面的命令实际使用 `/laser_scan`。

Linux：

```console
$ source /opt/ros/rolling/setup.bash
$ ros2 run ros_gz_bridge parameter_bridge /lidar2@sensor_msgs/msg/LaserScan[ignition.msgs.LaserScan --ros-args -r /lidar2:=/laser_scan
```

可以使用 RViz2 可视化 ROS 2 中的激光雷达数据。

Linux：

```console
$ source /opt/ros/rolling/setup.bash
$ rviz2
```

然后配置 `fixed frame`：

![配置固定坐标系](Image/fixed_frame.png)

点击“Add”按钮，添加激光雷达显示项：

![添加激光雷达显示项](Image/add_lidar.png)

现在应能在 RViz2 中看到激光雷达数据：

![RViz2 中的激光雷达数据](Image/rviz2.png)

## 总结

本教程中，你使用 Gazebo 启动了机器人仿真，为执行器和传感器启动了桥接，可视化了传感器数据，并控制差速驱动机器人移动。
