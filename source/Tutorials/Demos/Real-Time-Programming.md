---
translation_status: machine_translated
source: Tutorials/Demos/Real-Time-Programming.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="understanding-real-time-programming"></span>

# 理解实时编程

<span id="background"></span>

## 背景

实时计算是许多机器人系统的一个关键特征,特别是安全性和任务关键应用,如自主飞行器、航天器和工业制造。 我们正设计并原型ROS 2,同时考虑到实时性能限制,因为这在ROS 1的早期阶段没有被考虑过,而且现在很难将ROS 1 重构为实时友好型。

[本文件](https://design.ros2.org/articles/realtime_background.html) 概述软件工程师实时计算的要求和最佳做法。

为了建立一个实时计算机系统,我们的实时循环必须定期更新以达到最后期限。 我们只能容忍这些最后期限的微小误差幅度(我们的最大允许时速 ) 。 为了做到这一点,我们必须避免执行路径中的非决定性操作,比如:页断事件、动态内存分配/处理位置、以及无限期阻挡的同步原始。

一个通常通过实时计算解决的控制问题的典型例子是平衡一个 [倒转的笔](https://en.wikipedia.org/wiki/Inverted_pendulum)。如果控制器被屏蔽了很长一段时间,则顶点会倒塌或不稳定。但如果控制器以比控制顶点的电动机更快的速度可靠地更新,顶点会成功地适应传感器数据,以平衡顶点。

现在,你知道实时计算的一切了,让我们试试演示吧!

<span id="install-and-run-the-demo"></span>

## 安装和运行演示

实时演示是用Linux操作系统来写成的,因为ROS社区许多从事实时计算的成员使用Xenomai或RT_PREEMPT作为其实时解决方案,由于演示中为优化性能而进行的许多操作都是OS特异性的,所以演示只建立在Linux系统上并运行. **如果你是OSX或Windows用户,**

还必须使用静态的DDS API从源头构建此功能. **目前唯一得到支持的执行是ConnextDDS**.

首先,遵循建造ROS 2的指示 [来源](../../Installation/Alternatives/Ubuntu-Development-Setup.md) 使用 Connext DDS 作为中间软件。

<span id="run-the-tests"></span>

### 运行测试

**在运行前, 请确定您至少有 8Gb 的 RAM 免费。 随着内存锁定, 互换将失效 。**

源代码 ROS 2 `setup.bash`:

``` console
$ source ./install/setup.bash
```

运行演示二进制。 您可能想要使用 `sudo` 以防您获得许可错误 :

``` console
$ ros2 run pendulum_control pendulum_demo
Initial major pagefaults: 518
Initial minor pagefaults: 2139466
No results filename given, not writing results
rttest statistics:
- Minor pagefaults: 0
- Major pagefaults: 0
Latency (time after deadline was missed):
   - Min: 1851 ns
   - Max: 166796 ns
   - Mean: 14229.182000 ns
   - Standard deviation: 12288.040996
```

您可以在控制台上看到以下错误输出( 从 stderr) :

``` console
mlockall failed: Cannot allocate memory
Couldn't lock all cached virtual memory.
Pagefaults from reading pages not yet mapped into RAM will be recorded.
```

在演示程序初始化阶段之后,它将尝试将所有缓存内存锁定到RAM中,并防止未来使用动态内存分配 `mlockall`。这是为了防止页错将许多新内存装入RAM。 (见) [实时设计文章](https://design.ros2.org/articles/realtime_background.html#memory-management) 更多信息。 )

演示会像往常一样继续进行。 您也可以看到输出如下, 这意味着执行过程中遇到的页面故障数 :

``` default
rttest statistics:
  - Minor pagefaults: 20
  - Major pagefaults: 0
```

如果我们想让这些错误消失,

<span id="adjust-permissions-for-memory-locking"></span>

### 调整内存锁定权限

添加为 `/etc/security/limits.conf` (作为sudo):

``` default
<your username>    -   memlock   <limit in kB>
```

限额 `-1` 无限。如果选择此选项,您可能需要随附此选项 `ulimit -l unlimited` 在编辑文件后。

保存文件后, 登录并重新登录。 然后重运行 `pendulum_demo` 援引。

您会看到输出文件中的零页错误, 或者错误地说一个错误的\_ alloc 例外被抓住了。 如果发生这种情况, 您没有足够的自由内存可以将分配给进程的内存锁定到内存中。 您需要在您的计算机中安装更多的内存, 以便看到零页错误 !

<span id="output-overview"></span>

### 产出概览

要看到更多的输出,我们必须运行 `pendulum_logger` 节点。

和你的一发子弹 `install/setup.bash` 来源,援引:

``` console
$ ros2 run pendulum_control pendulum_logger
```

您应该看到输出消息 :

``` default
Logger node initialized.
```

在另一个带有设置的外壳中, bash 源代码, 引用 `pendulum_demo` 再来一次

此可执行文件一启动, 您就应该看到另一个 shell 不断打印输出 :

``` default
Commanded motor angle: 1.570796
Actual motor angle: 1.570796
Mean latency: 210144.000000 ns
Min latency: 4805 ns
Max latency: 578137 ns
Minor pagefaults during execution: 0
Major pagefaults during execution: 0
```

此演示正在控制一个非常简单的倒转式笔鼓模拟。 笔鼓模拟会计算它在自己线程中的位置。 一个ROS节点为笔鼓模拟一个运动编码器传感器并公布其位置。 另一个ROS节点充当一个简单的PID控制器并计算下一个命令消息。

记录器节点会定期打印该演示的状态以及演示执行阶段的运行时间性能统计。

之后 `pendulum_demo` 完成后, 您必须退出日志节点的 CTRL- C 。

<span id="latency"></span>

### 延迟

在那个 `pendulum_demo` 执行时,您将看到为演示收集的最后统计数据:

``` default
rttest statistics:
  - Minor pagefaults: 0
  - Major pagefaults: 0
  Latency (time after deadline was missed):
    - Min: 3354 ns
    - Max: 2752187 ns
    - Mean: 19871.8 ns
    - Standard deviation: 1.35819e+08

PendulumMotor received 985 messages
PendulumController received 987 messages
```

延迟字段以纳米秒显示更新循环的最小、最大和平均延迟。在这里,延迟意味着更新预期发生的时间。

实时系统的要求取决于应用程序,但让我们在演示中说,我们有一个1kHz(1毫秒)更新循环,我们的目标是实现我们更新期的5%的最大允许延迟。

因此,我们的平均延迟在这场比赛中是非常好的,但是最大的延迟是无法接受的,因为它实际上超过了我们更新的循环!发生了什么事?

我们可能会遇到一个非决定性的调度器。 如果您运行着一个 Vanilla Linux 系统而你没有安装 RT\_ PREEMPT 内核, 你可能无法实现我们为自己设定的实时目标, 因为 Linux 调度器不允许您任意在用户级别上预设线程 。

见 [实时设计文章](https://design.ros2.org/articles/realtime_background.html#multithreaded-programming-and-synchronization) 以获取更多信息。

演示试图设置演示的调度器和线程优先级以适合实时性能。 如果操作失败, 您将会看到一个错误消息 : “ 无法设置调度优先级和策略 : 操作不允许 ” 。 您可以在下一节中遵循指令, 获得稍好的业绩 :

<span id="setting-permissions-for-the-scheduler"></span>

### 设置调度器的权限

添加为 `/etc/security/limits.conf` (作为sudo):

``` default
<your username>    -   rtprio   98
```

rtprio(实时优先级)字段的范围为0-99。 但是, 请不要将限制设定为99, 因为这样您的进程可能会干扰在最高优先级运行的重要系统进程( 如监控) 。 此演示会尝试在优先级98 运行控制循环 。

<span id="plotting-results"></span>

### 绘制结果

您可以在演示运行后绘制此演示中收集的间隔和页断层统计 。

因为这个代码是用... [测试](https://github.com/ros2/rttest),有有用的命令行参数:

<table class="docutils align-default">
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="row-odd">
<td><p>命令</p></td>
<td><p>说明</p></td>
<td><p>默认值</p></td>
</tr>
<tr class="row-even">
<td><p>-i</p></td>
<td><p>指定运行实时循环的迭代数</p></td>
<td><p>1000</p></td>
</tr>
<tr class="row-odd">
<td><p>-u</p></td>
<td><p>指定更新期间, 默认单位为微秒</p>
<p>使用后缀“s”为秒,“ms”为毫秒,</p>
<p>微秒的“us”和纳秒的“ns”</p></td>
<td><p>1分钟</p></td>
</tr>
<tr class="row-even">
<td><p>-f</p></td>
<td><p>指定用于写入所收集数据的文件名称</p></td>
<td></td>
</tr>
</tbody>
</table>

使用文件名再次运行演示以保存结果 :

``` console
$ ros2 run pendulum_control pendulum_demo -f pendulum_demo_results
```

那就运行 `rttest_plot` 由此产生的文件中的脚本 :

``` console
$ ros2 run rttest rttest_plot pendulum_demo_results
Writing results to file: pendulum_demo_results
...
```

此脚本将生成三个文件 :

``` default
pendulum_demo_results_plot_latency.svg
pendulum_demo_results_plot_majflts.svg
pendulum_demo_results_plot_minflts.svg
```

您可以在您选择的图像查看器中查看这些图案 。
