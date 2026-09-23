---
translation_status: machine_translated
source: Tutorials/Demos/dummy-robot-demo.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="experimenting-with-a-dummy-robot"></span>

# 使用虚拟机器人进行实验

在这个演示中,我们展示了一个简单的演示机器人,其中包含从发布联合州到发布假激光数据的所有组件,直到在RViz的地图上可视化机器人模型.

<span id="launching-the-demo"></span>

## 启动演示

我们假设您的 ROS 2 安装目录为 `~/ros2_ws`。请根据您的平台更改目录。

要开始演示,我们执行演示带程启动文件,我们将在下一节中对此进行更详细的解释。您应该看到终端内的一些指纹,大致如下:

``` console
$ source ~/ros2_ws/install/setup.bash
$ ros2 launch dummy_robot_bringup dummy_robot_bringup.launch.py
[INFO] [launch]: process[dummy_map_server-1]: started with pid [25812]
[INFO] [launch]: process[robot_state_publisher-2]: started with pid [25813]
[INFO] [launch]: process[dummy_joint_states-3]: started with pid [25814]
[INFO] [launch]: process[dummy_laser-4]: started with pid [25815]
Initialize urdf model from file: /home/mikael/work/ros2/bouncy_ws/install_debug_isolated/dummy_robot_bringup/share/dummy_robot_bringup/launch/single_rrbot.urdf
Parsing robot urdf xml string.
Link single_rrbot_link1 had 1 children
Link single_rrbot_link2 had 1 children
Link single_rrbot_link3 had 2 children
Link single_rrbot_camera_link had 0 children
Link single_rrbot_hokuyo_link had 0 children
got segment single_rrbot_camera_link
got segment single_rrbot_hokuyo_link
got segment single_rrbot_link1
got segment single_rrbot_link2
got segment single_rrbot_link3
got segment world
Adding fixed segment from world to single_rrbot_link1
Adding moving segment from single_rrbot_link1 to single_rrbot_link2
[INFO] [dummy_laser]: angle inc:    0.004363
[INFO] [dummy_laser]: scan size:    1081
[INFO] [dummy_laser]: scan time increment:  0.000028
Adding moving segment from single_rrbot_link2 to single_rrbot_link3
Adding fixed segment from single_rrbot_link3 to single_rrbot_camera_link
Adding fixed segment from single_rrbot_link3 to single_rrbot_hokuyo_link
```

如果你现在在一个新的终端打开 RViz2, 你会看到你的机器人。 。 。 @ @ @ @ @ @ title

``` console
$ source <ROS2_INSTALL_FOLDER>/setup.bash
$ rviz2
```

打开 RViz2. 假设您仍然启用您的假人\_ robot\_ bringup, 您现在可以添加 TF 显示插件并配置您的全局框架到 `world`。一旦你这样做,你应该看到类似的照片:

![](images/rviz-dummy-robot.png) <span id="what-s-happening"></span>

### 情况如何?

如果你仔细看看发射文件 我们同时开始几个节点

- dummy_map_server

- dummy_laser

- dummy_joint_states

- robot_state_publisher

前两个软件包相对简单。 `dummy_map_server` 不断发布带有周期性更新的空地图。 `dummy_laser` 基本相同; 发布假冒激光扫描。

那个... `dummy_joint_states` 节点正在发布假联合状态数据。 当我们发布一个只有两个关节的简单的RRbot时,这个节点会发布这两个关节的联合状态值。

那个... `robot_state_publisher` 正在做实际的有趣工作。它解析给定的URDF文件,提取机器人模型,并聆听即将到来的联合状态。通过这些信息,它会发布我们在RViz中可视化的机器人的TF值。

万岁!
