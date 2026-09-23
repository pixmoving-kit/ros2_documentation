<span id="introducing-tf2"></span> <span id="intrototf2"></span>

# tf2 入门

**目标：** 运行 turtlesim 示例，了解 tf2 在多机器人系统中的一些能力。

**教程级别：** 中级

**预计耗时：** 10 分钟

<span id="installing-the-demo"></span>

## 安装示例

先安装演示包及其依赖。

Ubuntu 软件包：

```console
$ sudo apt-get install ros-rolling-rviz2 ros-rolling-turtle-tf2-py ros-rolling-tf2-ros ros-rolling-tf2-tools ros-rolling-turtlesim
```

RHEL 软件包：

```console
$ sudo dnf install ros-rolling-rviz2 ros-rolling-turtle-tf2-py ros-rolling-tf2-ros ros-rolling-tf2-tools ros-rolling-turtlesim
```

从源码安装：

```console
$ git clone https://github.com/ros/geometry_tutorials.git -b ros2
```

<span id="running-the-demo"></span>

## 运行示例

安装 `turtle_tf2_py` 后，打开新终端并[加载 ROS 2 环境](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)，使 `ros2` 命令可用，然后运行：

```console
$ ros2 launch turtle_tf2_py turtle_tf2_demo.launch.py
```

turtlesim 启动后会出现两只海龟。

![](images/turtlesim_follow1.png)

在第二个终端运行：

```console
$ ros2 run turtlesim turtle_teleop_key
```

将焦点切换到第二个终端，使其接收按键，即可用方向键控制中央的海龟。

![](images/turtlesim_follow2.png)

可以看到另一只海龟不断移动，跟随你控制的海龟。

<span id="what-is-happening"></span>

## 背后发生了什么？

示例通过 tf2 创建三个坐标系：`world`、`turtle1` 和 `turtle2`。tf2 广播器发布海龟坐标系，tf2 监听器计算两个海龟坐标系之间的差异，并控制一只海龟跟随另一只。

<span id="tf2-tools"></span>

## tf2 工具

接下来用 `tf2_tools` 查看 tf2 如何实现这个示例。

<span id="using-view-frames"></span>

### 1 使用 view_frames

`view_frames` 为 ROS 中广播的 tf2 坐标系生成关系图。该工具仅适用于 Linux；Windows 用户可跳到下一节“使用 tf2_echo”。

```console
$ ros2 run tf2_tools view_frames
Listening to tf data during 5 seconds...
Generating graph in frames.pdf file...
```

这里的 tf2 监听器接收 ROS 中广播的坐标系，并绘制连接关系树。用 PDF 查看器打开生成的 `frames.pdf`：

![](images/turtlesim_frames.png)

图中有 `world`、`turtle1` 和 `turtle2` 三个坐标系，`world` 是另外两个的父坐标系。`view_frames` 还提供用于调试的诊断信息，包括最早和最新收到的变换时间、变换发布频率等。

<span id="using-tf2-echo"></span>

### 2 使用 tf2_echo

`tf2_echo` 显示 ROS 中任意两个已广播坐标系之间的变换。

用法：

```console
$ ros2 run tf2_ros tf2_echo [source_frame] [target_frame]
```

查看 `turtle2` 相对于 `turtle1` 的变换：

```console
$ ros2 run tf2_ros tf2_echo turtle2 turtle1
At time 1683385337.850619099
- Translation: [2.157, 0.901, 0.000]
- Rotation: in Quaternion [0.000, 0.000, 0.172, 0.985]
- Rotation: in RPY (radian) [0.000, -0.000, 0.345]
- Rotation: in RPY (degree) [0.000, -0.000, 19.760]
- Matrix:
  0.941 -0.338  0.000  2.157
  0.338  0.941  0.000  0.901
  0.000  0.000  1.000  0.000
  0.000  0.000  0.000  1.000
At time 1683385338.841997774
- Translation: [1.256, 0.216, 0.000]
- Rotation: in Quaternion [0.000, 0.000, -0.016, 1.000]
- Rotation: in RPY (radian) [0.000, 0.000, -0.032]
- Rotation: in RPY (degree) [0.000, 0.000, -1.839]
- Matrix:
  0.999  0.032  0.000  1.256
 -0.032  0.999 -0.000  0.216
 -0.000  0.000  1.000  0.000
  0.000  0.000  0.000  1.000
```

监听器接收到 ROS 2 中广播的坐标系后，就会显示变换。控制海龟移动时，随着两只海龟的相对位置和朝向变化，输出也会变化。

<span id="rviz2-and-tf2"></span>

## rviz2 与 tf2

`rviz2` 是便于检查 tf2 坐标系的可视化工具。通过 `-d` 加载配置文件，查看海龟坐标系。

Linux：

```console
$ ros2 run rviz2 rviz2 -d $(ros2 pkg prefix --share turtle_tf2_py)/rviz/turtle_rviz.rviz
```

Windows：

```console
$ for /f "usebackq tokens=*" %a in (`ros2 pkg prefix --share turtle_tf2_py`) do rviz2 -d %a/rviz/turtle_rviz.rviz
```

![](images/turtlesim_rviz.png)

侧栏会列出 tf2 广播的坐标系。控制海龟移动时，可以看到 RViz 中对应坐标系随之运动。
