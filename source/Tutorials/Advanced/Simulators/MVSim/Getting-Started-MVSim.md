---
translation_status: machine_translated
source: Tutorials/Advanced/Simulators/MVSim/Getting-Started-MVSim.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="getting-started-with-mvsim"></span>

# MVSim 入门

**目标：** 发射MVSim演示世界既独立化,又与ROS 2一起,并学习如何与模拟机器人互动.

**教程级别：** 高级

**用时：** 20分钟

<span id="background"></span>

## 背景

MVSim 飞船的演示世界集显示不同的特性,例如多机器人模拟、传感器配置、地形类型、人类演员、清晰的车辆和环境布局。您可以使用此功能作为独立应用程序运行这些演示。 `mvsim` CLI,或作为一个ROS 2节点,通过标准ROS 2主题发布传感器数据并接受速度指令.

![MVSim 演示截图](Image/mvsim_demos_screenshot.png) <span id="prerequisites"></span>

## 前提条件

你应该安装MVSim跟随 [安装（Ubuntu）](Installation-Ubuntu.md) 教学。

<span id="tasks"></span>

## 操作步骤

<span id="launch-demo-worlds-with-the-standalone-cli"></span>

### 1 带有独立 CLI 的启动演示世界

MVSim包括一个不需要ROS 2. 的独立的发射装置,这对快速测试世界文件或对非ROS使用案例有用.

要启动仓库演示:

``` console
$ mvsim launch ~/ros2_ws/src/mvsim/mvsim_tutorial/demo_warehouse.world.xml
```

如果您从二进制包安装, 演示文件通常在下面找到 。 `/opt/ros/rolling/share/mvsim/mvsim_tutorial/`.

其它的演示世界,你可以尝试:

- `demo_turtlebot_world.world.xml` – 在有障碍的经典ROS风格环境中的TurtleBot3.

- `demo_2robots.world.xml` ——两台机器人在家具区块之间导航.

- `demo_elevation_map.world.xml` – 一个有海拔数据的杰克机器人驾车飞越地形.

- `demo_greenhouse.world.xml` – 一个复杂的温室环境,显示程序内容的XML环路.

<span id="controlling-the-robot"></span>

### 2 控制机器人

一旦一个世界运行,你可以控制机器人使用:

- **键盘 :** 按 W/ S 向前/ 向后移动, A/ D 向左/ 右转, 空间栏向后停止。 如果世界有多个机器人, 请点击 GUI 中的机器人在使用键盘控制前选择它 。

- **欢乐棒:** 如果一个游戏板被连接,它将被自动检测到.

![MVSim GUI 控制引用](Image/mvsim_gui_controls.jpg)

图形用户界面还为相机视图、模拟速度和可视化选项提供控制。您可以切换正图/透视视图,并直接在3D窗口中实现传感器数据的可视化。

<span id="launch-with-ros-2"></span>

### 3 运载火箭2发射

发射MVSim作为ROS 2节点,使用所提供的发射文件:

``` console
$ source /opt/ros/rolling/setup.bash
$ ros2 launch mvsim demo_warehouse.launch.py
```

这开始模拟器,并为每个车辆和传感器创建ROS 2主题.

<span id="inspect-ros-2-topics"></span>

### 4 检查ROS 2个专题

随着演示的运行,打开一个新的终端并列出可用的主题:

``` console
$ ros2 topic list
```

您应该看到以下主题:

- `/robot1/cmd_vel` - 发送 `geometry_msgs/msg/Twist` 命令来控制机器人。

- `/robot1/odom` - 从轮式编码器得到的测量仪(`nav_msgs/msg/Odometry`).

- `/robot1/base_pose_ground_truth` - 完美的地面真实姿势。

- `/robot1/<sensor_name>` - 传感器专题(例如: `/robot1/lidar1_points` 为3D LiDAR 点云, `/robot1/laser1` 2D扫描).

- `/tf` 财务报告和财务报告 `/tf_static` - TF2在之后的变换 [REP-105 (韩语)](https://www.ros.org/reps/rep-0105.html) (`map` → `odom` → `base_link`).

您可以从命令行发送速度命令 :

``` console
$ ros2 topic pub /robot1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 0.5}, angular: {z: 0.3}}"
```

或使用( E) `teleop_twist_keyboard` 用于交互式控制 :

``` console
$ ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -r cmd_vel:=/robot1/cmd_vel
```

<span id="visualize-in-rviz2"></span>

### 5 在 RViz2 可视化

您可以在 RViz2 中可视化 MVSim 传感器数据 。 `use_rviz` 选项 :

``` console
$ ros2 launch mvsim demo_warehouse.launch.py use_rviz:=True
```

或者,手动打开RViz2,并为感兴趣的主题添加显示(例如, `LaserScan`, `PointCloud2`, `Image`, `Odometry`).

![MVSim深度相机可视化](Image/mvsim_depth_camera_demo.png) <span id="headless-mode"></span>

### 6 无头模式

对于没有显示的CI管道或远程服务器,MVSim支持无头操作:

``` console
$ ros2 launch mvsim demo_warehouse.launch.py headless:=True
```

这在不打开GUI窗口的情况下运行了完整的模拟.

<span id="summary"></span>

## 小结

在这个教程中,你推出了MVSim演示世界,既独立化,又与ROS 2. 你学会了如何用键盘和ROS 2主题来控制机器人,检查已发布的主题,并在RViz2中可视化数据. 下一个教程涵盖了如何用定制机器人和传感器来定义你自己的世界.
