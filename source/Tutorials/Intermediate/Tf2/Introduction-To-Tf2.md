---
translation_status: machine_translated
source: Tutorials/Intermediate/Tf2/Introduction-To-Tf2.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="introducing-tf2"></span> <span id="intrototf2"></span>

# 一. 导言 `tf2`

**目标：** 运行一个龟座演示,在多机器人实例中使用龟座演示看到 tf2 的一些功率.

**教程级别：** 中级

**用时：** 10分钟

<span id="installing-the-demo"></span>

## 安装演示

让我们从安装演示软件包及其依赖性开始。

##### Ubuntu 软件包

``` console
$ sudo apt-get install ros-rolling-rviz2 ros-rolling-turtle-tf2-py ros-rolling-tf2-ros ros-rolling-tf2-tools ros-rolling-turtlesim
```

##### RHEL 软件包

``` console
$ sudo dnf install ros-rolling-rviz2 ros-rolling-turtle-tf2-py ros-rolling-tf2-ros ros-rolling-tf2-tools ros-rolling-turtlesim
```

##### 从源

``` console
$ git clone https://github.com/ros/geometry_tutorials.git -b ros2
```

<span id="running-the-demo"></span>

## 运行演示

现在,我们已经安装了 `turtle_tf2_py` 教程软件包让我们运行演示。首先打开一个新的终端和 [源代码 ROS 2 安装](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md) 这样一来 `ros2` 命令将有效。然后运行以下命令:

``` console
$ ros2 launch turtle_tf2_py turtle_tf2_demo.launch.py
```

你会看到乌龟从两只乌龟开始

![](images/turtlesim_follow1.png)

在第二个终端窗口中,以下命令:

``` console
$ ros2 run turtlesim turtle_teleop_key
```

龟兹启动后,可以使用键盘箭键在龟兹周围驱动中心龟,选择第二个终端窗口,以便捕捉到键盘中枢龟来驱动龟.

![](images/turtlesim_follow2.png)

你可以看到一只乌龟不停地移动 追随你所驾驶的乌龟

<span id="what-is-happening"></span>

## 发生什么事了?

此演示使用 tf2 库创建三个坐标框: a `world` 框架, a `turtle1` 边框,和 a `turtle2` 框架。此教程使用一个 *tf2 广播机* 以发布海龟坐标框架和 a *tf2 收听器* 计算龟框的区别,并移动一只龟跟随另一只龟。

<span id="tf2-tools"></span>

## tf2 工具

现在让我们看看Tf2是如何用来创建这个演示的。我们可以使用 `tf2_tools` 看看Tf2在幕后在做什么。

<span id="using-view-frames"></span>

### 1 使用视图框

`view_frames` 创建 tf2 在 ROS 上播放的框架图 。 请注意, 此工具只在 Linux 上工作; 如果您是 Windows, 请跳到下面的“ Using tf2\_ echo ” 。

``` console
$ ros2 run tf2_tools view_frames
Listening to tf data during 5 seconds...
Generating graph in frames.pdf file...
```

这里有一个 tf2 收听器正在收听正在ROS上播放的帧,并绘制帧连接方式的树。要查看此树,请打开由此生成的帧 。 `frames.pdf` 与你最喜欢的 PDF 查看器。

![](images/turtlesim_frames.png)

这里可以看到Tf2播放的三帧: `world`, `turtle1`,以及 `turtle2`。该词 `world` 框是该框的母体 `turtle1` 财务报告和财务报告 `turtle2` 边框。 `view_frames` 还报告了一些关于何时收到最古老和最新的帧变换的诊断信息,以及为了调试的目的,tf2帧的发布速度如何.

<span id="using-tf2-echo"></span>

### 2 使用 tf2\_ echo

`tf2_echo` 报告通过ROS广播的任意两个帧之间的变换.

用法 :

``` console
$ ros2 run tf2_ros tf2_echo [source_frame] [target_frame]
```

让我们看看变革 `turtle2` 框架的调整 `turtle1` 框架,相当于:

``` console
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

你会看到变换显示为 `tf2_echo` 收听者接收通过ROS 2广播的帧.

当你驾着乌龟环绕时,你会看到两只乌龟相对移动时的变迁变化.

<span id="rviz2-and-tf2"></span>

## rviz2 和 tf2 数据

`rviz2` 是一个可视化工具,可用于检查 tf2 框架。让我们使用 `rviz2` 以配置文件开头 `-d` 选项 :

##### Linux

``` console
$ ros2 run rviz2 rviz2 -d $(ros2 pkg prefix --share turtle_tf2_py)/rviz/turtle_rviz.rviz
```

##### Windows

``` console
$ for /f "usebackq tokens=*" %a in (`ros2 pkg prefix --share turtle_tf2_py`) do rviz2 -d %a/rviz/turtle_rviz.rviz
```

![](images/turtlesim_rviz.png)

在侧边栏中,你会看到框由 tf2 播放,当你在周围驱动龟时,你会看到框以rviz形式移动.
