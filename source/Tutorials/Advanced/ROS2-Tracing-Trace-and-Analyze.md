---
translation_status: machine_translated
source: Tutorials/Advanced/ROS2-Tracing-Trace-and-Analyze.rst
---

<span id="how-to-use-ros2-tracing-to-trace-and-analyze-an-application"></span>

# 使用 ros2_tracing 追踪与分析应用

此教程显示如何使用 [ros2_tracing](https://github.com/ros2/ros2_tracing) 用于跟踪和分析 ROS 2 应用程序。对于此教程,应用程序将是 [performance_test](https://gitlab.com/ApexAI/performance_test).

<span id="overview"></span>

## 概述

此教程覆盖 :

1.  安装与追查有关的工具,并使用核心仪器建造ROS 2

2.  运行和跟踪 a `performance_test` 运行

3.  分析微量数据 [tracetools_analysis](https://github.com/ros-tracing/tracetools_analysis) 绘制回调时长 [Jupyter 笔记本](https://jupyter.org/)

<span id="prerequisites"></span>

## 前提条件

此教程面向实时 Linux 系统。 请参看 [实时系统设置教程](../Miscellaneous/Building-Realtime-rt_preempt-kernel-for-ROS-2.md)然而,如果您正在使用非实时的 Linux 系统, 教程将会起作用 。

<span id="installing-and-building"></span>

## 安装和建造

> **说明**
>
> 此教程一般应该与所有支持的 Linux 分布一起工作。 然而, 您可能需要修改一些命令 。

通过跟随 Linux 安装 ROS 2 上的所有依赖性 [源安装指令](../../Installation/Alternatives/Ubuntu-Development-Setup.md)。停止之前 *在工作空间中构建代码* 节。

安装 [LTTNG (英语).](https://lttng.org/docs/v2.13/) 财务报告和财务报告 `babeltrace`.

``` console
$ sudo apt-get update
$ sudo apt-get install -y lttng-tools liblttng-ust-dev python3-lttng python3-babeltrace babeltrace
```

然后创建一个工作空间,导入 ROS 2 滚动代码,并复制 `performance_test` 财务报告和财务报告 `tracetools_analysis`.

``` console
$ cd ~/
$ mkdir -p tracing_ws/src
$ cd tracing_ws/
$ vcs import src/ --input https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos
$ cd src/
$ git clone https://gitlab.com/ApexAI/performance_test.git
$ git clone https://github.com/ros-tracing/tracetools_analysis.git -b rolling
$ cd ..
```

用 rosdep 安装依赖性 。

``` console
$ rosdep update
$ rosdep install --rosdistro rolling --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-6.0.1 urdfdom_headers"
```

然后积聚到 `performance_test` 并配置 ROS 2. 见其 [文档](https://gitlab.com/ApexAI/performance_test/-/tree/master/performance_test#performance_test)我们还需要建立 `ros2trace` 利用该工具建立追踪系统 `ros2 trace` 命令和命令 `tracetools_analysis` 分析数据。

``` console
$ colcon build --packages-up-to ros2trace ros2run tracetools_analysis performance_test --cmake-args -DPERFORMANCE_TEST_RCLCPP_ENABLED=ON
```

提供安装源,并核实是否允许追踪:

``` bash
$ source install/setup.bash
$ ros2 run tracetools status
```

你应该看看 `Tracing enabled` 。这证实了LTTng被正确检测到,并启用了ROS 2核心中包含的仪器。

接下来,我们将运行一个 `performance_test` 实验和追踪它。

<span id="tracing"></span>

## 追踪

<span id="step-1-trace"></span>

### 步骤1:追踪

在一个终端中, 查找工作空间并设置追踪。 当运行命令时, 将会打印 ROS 2 用户空间事件列表。 它也会打印目录的路径, 该目录将包含由此生成的跟踪( 下) `~/.ros/tracing`。在1号航站楼运行中:

``` console
$ cd ~/tracing_ws
$ source install/setup.bash
$ ros2 trace --session-name perf-test --list
```

按下输入开始追踪 。

<span id="step-2-run-application"></span>

### 步骤2: 运行应用程序

在第二航站楼,寻找工作空间。在第二航站楼运行:

``` console
$ cd ~/tracing_ws
$ source install/setup.bash
```

那就运行 `performance_test` 实验( 或你自己的应用程序 ) 。 我们只是创建一个实验, 用第二高的实时优先级, 将 ~ 1 MB 消息尽可能快地发布到另一个节点。 这样我们就不会干扰关键的内核线程 。 我们需要运行 。 `performance_test` 作为 `root` 能够使用实时优先级。在2号航站楼运行:

``` console
$ sudo ./install/performance_test/lib/performance_test/perf_test -c rclcpp-single-threaded-executor -p 1 -s 1 -r 0 -m Array1m --reliability RELIABLE --max-runtime 60 --use-rt-prio 98
```

如果最后一个命令对您无效( 错误如“ 装入共享库时有错误 ) , 请在下面运行略微不同的命令 。 这是因为出于安全原因, 我们需要手动通过 。 `*PATH` 将找到一些共享库的环境变量(见 [这一解释](https://unix.stackexchange.com/a/251374)在2号航站楼运行时:

``` console
$ sudo env PATH="$PATH" LD_LIBRARY_PATH="$LD_LIBRARY_PATH" ./install/performance_test/lib/performance_test/perf_test -c rclcpp-single-threaded-executor -p 1 -s 1 -r 0 -m Array1m --reliability RELIABLE --max-runtime 60 --use-rt-prio 98
```

> **说明**
>
> 如果您没有使用实时内核, 请只需运行 : 在终端 2 运行 :
>
> ``` console
> $ ./install/performance_test/lib/performance_test/perf_test -c rclcpp-single-threaded-executor -p 1 -s 1 -r 0 -m Array1m --reliability RELIABLE --max-runtime 60
> ```

<span id="step-3-validate-trace"></span>

### 步骤3:验证追踪

一旦实验完成,在第一个终端中,按下再次输入以停止追踪。使用 `babeltrace` 以快速的观察 由此产生的踪迹。

``` console
$ babeltrace ~/.ros/tracing/perf-test | less
```

以上命令的输出是原始共同追踪格式(CTF)数据的人可读版本,它是微量事件列表,每个事件都有时间戳,事件类型,一些关于生成事件的过程的信息,以及指定事件类型的字段的值.

使用箭头键滚动,或按 `q` 准备离开。

接下来,我们将分析追踪。

<span id="analysis"></span>

## 分析

[tracetools_analysis](https://github.com/ros-tracing/tracetools_analysis) 提供 Python API 以方便分析痕迹 。 我们可以在 [Jupyter笔记本](https://jupyter.org/) 与 [来,来,来,来,来](https://docs.bokeh.org/en/latest/index.html) 来绘制数据。 `tracetools_analysis` 仓库包含一个 [很少的样本笔记本](https://github.com/ros-tracing/tracetools_analysis/tree/rolling/tracetools_analysis/analysis),包括: [分析订阅回调时间的笔记本](https://github.com/ros-tracing/tracetools_analysis/blob/rolling/tracetools_analysis/analysis/callback_duration.ipynb).

对于此教程,我们将在订阅者节点中绘制订阅回调的持续时间 。

安装bokeh,然后打开样本笔记本。

``` console
$ pip3 install bokeh
$ jupyter notebook ~/tracing_ws/src/tracetools_analysis/tracetools_analysis/analysis/callback_duration.ipynb
```

这将打开浏览器中的笔记本 。

替换“%s”的值 `path` 微量目录路径的第二个单元格中的变量:

``` python
path = '~/.ros/tracing/perf-test'
```

点击“% 1” 运行笔记本 *运行* 每个单元格的按钮。运行进行跟踪处理的单元格在第一次运行时可能需要几分钟,但随后的运行会更快。

你应该得到一个类似这个的情节:

![回调持续时间结果图](images/ros2_tracing_guide_result_plot.png)

我们可以看到,大多数回调量都不到0.01毫秒,但有一些外线占据了0.02或0.03毫秒.

<span id="conclusion"></span>

## 结论

这个教程演示了如何安装与追踪有关的工具,并用追踪仪器构建ROS 2。 [performance_test](https://gitlab.com/ApexAI/performance_test) 实验使用 [ros2_tracing](https://github.com/ros2/ros2_tracing) 并使用 [tracetools_analysis](https://github.com/ros-tracing/tracetools_analysis).

对于更多的痕量分析,请看看 [其他样本笔记本](https://github.com/ros-tracing/tracetools_analysis/tree/rolling/tracetools_analysis/analysis) 页:1 [微量工具\_ 分析 API 文档](https://ros-tracing.gitlab.io/tracetools_analysis-api/master/tracetools_analysis/)。该词 [ros2_跟踪设计文件](https://github.com/ros2/ros2_tracing/blob/rolling/doc/design_ros_2.md) 也包含很多信息.
