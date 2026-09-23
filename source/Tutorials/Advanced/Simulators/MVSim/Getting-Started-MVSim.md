<span id="getting-started-with-mvsim"></span>
<span id="background"></span>
<span id="prerequisites"></span>
<span id="tasks"></span>
<span id="launch-demo-worlds-with-the-standalone-cli"></span>
<span id="controlling-the-robot"></span>
<span id="launch-with-ros-2"></span>
<span id="inspect-ros-2-topics"></span>
<span id="visualize-in-rviz2"></span>
<span id="headless-mode"></span>
<span id="summary"></span>

# MVSim 入门

**目标：** 独立运行及通过 ROS 2 运行 MVSim 演示世界，并学习与仿真机器人交互。

**教程级别：** 高级

**耗时：** 20 分钟

## 背景

MVSim 自带一组演示世界，展示多机器人仿真、传感器配置、地形类型、人物、铰接车辆及环境布局等功能。可以使用 `mvsim` 命令行工具独立运行，也可以将其作为 ROS 2 节点，通过标准 ROS 2 话题发布传感器数据并接收速度指令。

![MVSim 演示截图](Image/mvsim_demos_screenshot.png)

## 前提条件

请先按照[在 Ubuntu 上安装](Installation-Ubuntu.md)教程安装 MVSim。

## 任务

### 1 使用独立命令行工具启动演示世界

MVSim 包含不依赖 ROS 2 的独立启动工具，适合快速测试世界文件或非 ROS 使用场景。

启动仓库演示：

```console
$ mvsim launch ~/ros2_ws/src/mvsim/mvsim_tutorial/demo_warehouse.world.xml
```

如果通过二进制软件包安装，演示文件通常位于 `/opt/ros/rolling/share/mvsim/mvsim_tutorial/`。

还可以尝试以下世界：

- `demo_turtlebot_world.world.xml`：带有障碍物的经典 ROS 风格环境中的 TurtleBot3。
- `demo_2robots.world.xml`：两台机器人在家具块之间行驶。
- `demo_elevation_map.world.xml`：Jackal 机器人在带高程数据的地形上行驶。
- `demo_greenhouse.world.xml`：复杂的温室环境，展示使用 XML 循环以程序化方式生成内容。

### 2 控制机器人

世界启动后，可以通过以下方式控制机器人：

- **键盘：** W/S 前进／后退，A/D 左转／右转，空格停止。如果世界中有多台机器人，先在图形界面中点击选中机器人，再使用键盘控制。
- **操纵杆：** 已连接的游戏手柄会被自动检测到。

![MVSim 图形界面操作参考](Image/mvsim_gui_controls.jpg)

图形界面还提供摄像机视角、仿真速度和可视化选项。你可以在正交视图和透视视图之间切换，并直接在三维窗口中启用传感器数据可视化。

### 3 使用 ROS 2 启动

使用提供的启动文件，将 MVSim 作为 ROS 2 节点运行：

```console
$ source /opt/ros/rolling/setup.bash
$ ros2 launch mvsim demo_warehouse.launch.py
```

这会启动仿真器，并为每辆车和每个传感器创建 ROS 2 话题。

### 4 检查 ROS 2 话题

保持演示运行，打开新终端并列出可用话题：

```console
$ ros2 topic list
```

应能看到以下话题：

- `/robot1/cmd_vel`：发送 `geometry_msgs/msg/Twist` 指令来控制机器人。
- `/robot1/odom`：轮式编码器里程计（`nav_msgs/msg/Odometry`）。
- `/robot1/base_pose_ground_truth`：无误差的真实位姿。
- `/robot1/<sensor_name>`：各传感器对应的话题，例如三维激光雷达点云 `/robot1/lidar1_points` 和二维扫描 `/robot1/laser1`。
- `/tf` 和 `/tf_static`：遵循 [REP-105](https://www.ros.org/reps/rep-0105.html) 的 TF2 变换（`map` → `odom` → `base_link`）。

可以从命令行发送速度指令：

```console
$ ros2 topic pub /robot1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 0.5}, angular: {z: 0.3}}"
```

也可以使用 `teleop_twist_keyboard` 进行交互控制：

```console
$ ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -r cmd_vel:=/robot1/cmd_vel
```

### 5 在 RViz2 中可视化

可以在 RViz2 中显示 MVSim 传感器数据。部分启动文件提供 `use_rviz` 选项：

```console
$ ros2 launch mvsim demo_warehouse.launch.py use_rviz:=True
```

也可以手动打开 RViz2，为关注的话题添加显示项，例如 `LaserScan`、`PointCloud2`、`Image` 和 `Odometry`。

![MVSim 深度摄像头可视化](Image/mvsim_depth_camera_demo.png)

### 6 无界面模式

对于 CI 流水线或没有显示器的远程服务器，MVSim 支持无界面运行：

```console
$ ros2 launch mvsim demo_warehouse.launch.py headless:=True
```

这样会运行完整仿真，但不打开图形界面窗口。

## 总结

本教程中，你独立运行并通过 ROS 2 运行了 MVSim 演示世界，学习了通过键盘和 ROS 2 话题控制机器人、检查发布的话题，以及在 RViz2 中可视化数据。下一教程将介绍如何定义带有自定义机器人和传感器的世界。
