---
translation_status: machine_translated
source: How-To-Guides/Getting-Backtraces-in-ROS-2.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="getting-backtraces-in-ros-2"></span>

# 获取 ROS 2 调用栈

**目标：** 在 ROS 2 中显示获取回溯跟踪的各种方法

**教程级别：** 中级

**用时：** 15分钟

以下步骤显示ROS 2用户遇到问题时如何获得回溯追踪.

<span id="overview"></span>

## 概述

**什么是回溯追踪?**

- 想象一下,你的程序就像一叠煎饼,每一份煎饼都代表着它目前正在执行的功能。 回溯追踪就像一张倒塌的煎饼堆的照片,向你们展示它们所加入的顺序,揭示程序是如何以失败告终的。

- 它列出了被称作的函数的顺序,一个在另一个之上,导致失败的点.

**为什么它有用?**

- **标点问题 :** 反向追踪不是在猜测代码中出错的地方,而是显示对崩溃负责的准确行号.

- **启示背景 :** 您可以看到最终触发失败的事件链( 函数调用其他函数) 。 这不仅有助于您理解哪里出错, 也有利于您理解原因 。

**视觉分析**: 煎饼堆叠

1.  每个煎饼是一个函数: 想象一个堆栈中的每个煎饼代表您程序正在执行的函数。 底部的煎饼是您的主要( ) 函数, 全部开始的地方 。

2.  添加煎饼:每次一个函数调用另一个函数时,都会将一个新的煎饼放在堆栈的顶部.

3.  崩溃:崩溃就像盘子从堆栈底部滑出 — — 目前执行的功能发生了灾难性的错误。

4.  回溯追踪:回溯追踪就像一张坠落的煎饼堆的照片。它显示了煎饼(功能)从上到下排列的顺序,揭示你是如何在坠机地点结束的。

**代码示例:**

``` cpp
void functionC() {
  // Something bad happens here, causing a crash
}

void functionB() {
    functionC();
}

void functionA() {
    functionB();
}

int main() {
    functionA();
    return 0;
}
```

**从崩溃中返回追踪 :**

``` bash
#0  functionC() at file.cpp:3 // Crash occurred here
#1  functionB() at file.cpp:8
#2  functionA() at file.cpp:13
#3  main() at file.cpp:18
```

**回溯追踪是如何帮助的:**

- **崩溃起源 :** 显示精确的行 `functionC()` 这引发了坠机事件。

- **调用序列 :** 揭露 `main()` 调用 `functionA()`,它呼吁 `functionB()`,这最终导致了其中的错误 `functionC()`.

上面的例子让我们清楚地了解了回溯追踪是什么,以及它如何有用。现在,以下步骤显示ROS 2 用户遇到问题时如何从特定节点获取痕迹。这个教程既适用于模拟机器人,也适用于物理机器人。

这将涵盖如何从特定节点获取回溯跟踪 `ros2 run`,来自代表单一节点的发射文件,使用 `ros2 launch`,并且从更复杂的节点管弦。到此教程结束时,当您注意到一个节点在ROS 2中崩溃时,您应该可以得到回溯跟踪。

<span id="preliminaries"></span>

## 初步内容

GDB 是 Unix 系统中最流行的 C/C++ 调试器。 它可用于确定崩溃和跟踪线程的原因。 它也可以用于在您的代码中添加断点, 在您的软件中的特定点检查内存中的值 。

使用 GDB 是所有在 C/C++ 上工作的软件开发者的关键技能。 虽然许多IDE 都建有某种调试器或配置器, 但重要的是要了解如何使用您已有的这些原始工具, 而不是依赖一个IDE来提供这些工具。 理解这些工具是 C/C++ 开发的基本技能, 如果您改变角色, 并且不再能够访问它, 或者正在通过 ssh 会话开发到远程资产, 留给您的 IDE , 可能会有问题 。

使用 GDB 幸运的是, 在您带下基本内容之后相当简单。 以下是如何确保您的 ROS2 代码可以调试 :

- 通过使用 `--cmake-args`: 包含调试符号的最简单方法是添加 `--cmake-args -DCMAKE_BUILD_TYPE=Debug` 给您的 `colcon build` 命令 :

``` console
$ colcon build --packages-up-to <package_name> --cmake-args -DCMAKE_BUILD_TYPE=Debug
```

- 通过编辑 `CMakeLists.txt` : 另一种方式是添加 `-g` 到要配置/ 调试的 ROS 软件包的编译器标记。 此标记会构建调试符号, GDB 可以读取这些符号来告诉你项目中的特定代码行正在失败, 以及为什么。 如果您不设置此标记, 您仍然可以得到回溯追踪, 但是它不会为失败提供行号 。

现在您准备调试您的代码 。 如果这是非 ROS 工程, 您可能会在下面做一些类似的事情 。 我们在此启动一个 GDB 会话, 并让程序立即运行 。 一旦程序崩溃, 它将会返回一个 gdb 会话, 由 GDB 提示 。 `(gdb)`。 在这一时刻,您可以访问您感兴趣的信息。然而,由于这是一个ROS项目,它有很多节点配置和其他正在发生的事情,对于初学者或不喜欢数吨命令线工作和了解文件系统的人来说,这不是一个好主意。

``` console
$ gdb ex run --args /path/to/exe/program
```

下面是描述基于ROS 2 的系统可能遇到的三个主要情况的章节。 读一下最能描述你试图解决的问题的章节。

<span id="debugging-a-specific-node-with-gdb"></span>

## 用 GDB 调试特定节点

在启动ROS 2节点之前,为方便设置GDB会话,利用 `--prefix` 选项在启动 ROS 2 节点前轻松设置 GDB 会话。对于 GDB 调试,请使用如下:

> **说明**
>
> 铭记ROS 2可执行文件可能包含多个节点。 `--prefix` 方法保证您正在调试进程内的正确节点 。

**为什么直接的 GDB 用法可能很诡异**

`--prefix` 将在ROS 2 命令之前执行一些位码, 允许我们插入一些信息。 如果您尝试这样做 。 `gdb ex run --args ros2 run <pkg> <node>` 仿照我们的例子, 你会发现它找不到 `ros2` 命令。此外,试图在 GDB 中查找您的工作空间也会因类似原因而失败。这是因为 GDB 以这种方式启动时,缺乏通常使 GDB 运行的环境设置。 `ros2` 命令可用。

**用 - 前缀简化进程**

与其重新找到可执行文件的安装路径并将其全部输入,不如使用 `--prefix`。这允许我们使用相同的 `ros2 run` 你习惯的语法不需要担心一些 GDB 细节。

``` console
$ ros2 run --prefix 'gdb -ex run --args' <pkg> <node> --all-other-launch arguments
```

**全球开发银行的经验**

和以前一样, 此前缀将启动 GDB 会话, 并运行您请求的节点, 并附加所有命令行参数。 您现在应该运行您的节点, 并且应该与一些调试打印一起切换 。

<span id="reading-the-stack-trace"></span>

## 读取堆栈追踪

在使用 GDB 获取回溯跟踪后, 以下是如何解释:

- 从底部开始: 回溯跟踪列表函数按反向时间顺序调用。底部的函数是崩溃源头。

- 跟随 Stack 向上 : 上面的每行都代表它下面的函数。 向上追踪直到您在自己的工程中到达一条代码线。 这往往会揭示问题在哪里启动 。

- 调试 Clues:函数名称及其参数可以提供有价值的线索,说明出错的原因.

**节点崩溃时如何调试**

一旦你的节点崩溃,你就会看到下面这样的提示。 此时你可以得到回溯跟踪。

``` bash
(gdb)
```

在此会话中, 类型 `backtrace` 它会给你一个回溯追踪 复制这个供您需要

**示例回溯跟踪**

``` bash
(gdb) backtrace
#0  __GI_raise (sig=sig@entry=6) at ../sysdeps/unix/sysv/linux/raise.c:50
#1  0x00007ffff79cc859 in __GI_abort () at abort.c:79
#2  0x00007ffff7c52951 in ?? () from /usr/lib/x86_64-linux-gnu/libstdc++.so.6
#3  0x00007ffff7c5e47c in ?? () from /usr/lib/x86_64-linux-gnu/libstdc++.so.6
#4  0x00007ffff7c5e4e7 in std::terminate() () from /usr/lib/x86_64-linux-gnu/libstdc++.so.6
#5  0x00007ffff7c5e799 in __cxa_throw () from /usr/lib/x86_64-linux-gnu/libstdc++.so.6
#6  0x00007ffff7c553eb in ?? () from /usr/lib/x86_64-linux-gnu/libstdc++.so.6
#7  0x000055555555936c in std::vector<int, std::allocator<int> >::_M_range_check (
    this=0x5555555cfdb0, __n=100) at /usr/include/c++/9/bits/stl_vector.h:1070
#8  0x0000555555558e1d in std::vector<int, std::allocator<int> >::at (this=0x5555555cfdb0,
    __n=100) at /usr/include/c++/9/bits/stl_vector.h:1091
#9  0x000055555555828b in GDBTester::VectorCrash (this=0x5555555cfb40)
    at /home/steve/Documents/nav2_ws/src/gdb_test_pkg/src/gdb_test_node.cpp:44
#10 0x0000555555559cfc in main (argc=1, argv=0x7fffffffc108)
    at /home/steve/Documents/nav2_ws/src/gdb_test_pkg/src/main.cpp:25
```

在此例子中,您应该从下面开始按以下方式阅读:

- 在主功能中,在25号线上,我们称为函数矢量Crash.

- 在矢量崩溃,在44号线, 我们在矢量坠毁 `at()` 输入方法 `100`.

- 坠落了 `at()` 在 STL 矢量行 1091 上,在投放范围检查失败的例外后。

这些痕迹需要一些时间才能习惯阅读, 但一般情况下, 从底部开始, 跟踪到堆栈, 直到看到它撞上的线条。 然后可以推断它坠毁的原因 。 当你用 GDB 完成时, 键入 `quit` 它会退出会话并杀死任何尚未结束的进程。它可能会问您是否想要在结尾处杀死一些线程, 说是。

<span id="from-a-launch-file"></span>

## 从启动文件

和我们非ROS的例子一样,我们需要在发射我们的ROS 2发射文件之前先设置一个GDB会话。虽然我们可以通过命令线来设置它,但我们可以使用我们在操作中所做的同样的机械。 `ros2 run` 节点示例,现在使用发射文件.

在您的发射文件中, 请找到您感兴趣的调试节点 。 对于此部分, 我们假设您的发射文件只包含一个节点( 以及可能的其他信息 ) 。 `Node` 函数 `launch_ros` 软件包将使用一个字段前缀,并列出一个前缀参数。我们将在此插入 GDB 片段。

**根据您的设置考虑以下办法:**

- **使用 GUI 本地调试 :** 如果您正在本地调试并有可用的 GUI 系统, 请使用 :

``` python
prefix=['xterm -e gdb -ex run --args']
```

这将提供一个更具互动性的调试体验。 用于调试的示例 `'start_sync_slam_toolbox_node'` -

``` python
start_sync_slam_toolbox_node = Node(
  parameters=[
      get_package_share_directory("slam_toolbox") + '/config/mapper_params_online_sync.yaml',
      {'use_sim_time': use_sim_time}
  ],
  package='slam_toolbox',
  executable='sync_slam_toolbox_node',
  name='slam_toolbox',
  prefix=['xterm -e gdb -ex run --args'],  # For interactive GDB in a separate window/GUI
  output='screen')
```

- **远程调试( 不包含 GUI ) :** 如果不使用 GUI 调试, 请省略 `xterm -e` :

``` bash
prefix=['gdb -ex run --args']
```

GDB 的输出和交互会发生在您启动 ROS 2 应用程序的终端会话中。 以下是类似的例子 。 `'start_sync_slam_toolbox_node'` -

``` python
start_sync_slam_toolbox_node = Node(
  parameters=[
      get_package_share_directory("slam_toolbox") + '/config/mapper_params_online_sync.yaml',
      {'use_sim_time': use_sim_time}
  ],
  package='slam_toolbox',
  executable='sync_slam_toolbox_node',
  name='slam_toolbox',
  prefix=['gdb -ex run --args'],  # For GDB within the launch terminal
  output='screen')
```

和以前一样,这个前缀将启动一个 GDB 会话,现在在 `xterm` 运行您要求的发射文件, 并附加所有定义的发射参数 。

一旦节点坠落, `xterm` 语句。此时您可以得到回溯跟踪,然后使用其中的指令读取 [读取堆栈追踪](#reading-the-stack-trace).

<span id="from-a-large-project"></span>

## 来自大型项目

与多个节点的发射文件合作是有点不同的,这样你就可以与您的GDB会话互动,而不会被同一终端的其他记录所困住。 为此原因,在与更大的发射文件合作时,可以抽出你感兴趣的特定节点并单独发射。

如果您感兴趣的节点是从一个嵌入式发射文件(例如包含的发射文件)发射的,您可能希望做以下工作:

- 从母发射文件注释发射文件包含

- 将一揽子利益调整为 `-g` 调试符号的旗子

- 在终端中发射母发射文件

- 将节点的发射文件在另一个终端中按下列指示发射: [从启动文件](#from-a-launch-file).

或者,如果您感兴趣的节点直接在这些文件中启动(例如,您看到一个) `Node`, `LifecycleNode`,或在一个 `ComponentContainer`),你需要将这一点与其它内容分开:

- 从母发射文件中评论节点的包含

- 将一揽子利益调整为 `-g` 调试符号的旗子

- 在终端中发射母发射文件

- 在另一个终端中按照下列指令启动节点: [用 GDB 调试特定节点](#debugging-a-specific-node-with-gdb).

> **说明**
>
> 在这种情况下,如果此节点以前是由发射文件提供的,则可能需要重新绘制或提供该节点的参数文件。使用 `--ros-args` 您可以为它提供新参数文件的路径、 回图或名称。见 [此教程](Node-arguments.md) 用于所需的命令行参数。
>
> 我们知道这可能很痛苦,因此它可能鼓励您将每个节点作为单独包含的发射文件,以便更容易调试。 `--ros-args -r __node:=<node_name> --params-file /absolute/path/to/params.yaml` (作为模板).

一旦节点崩溃,您就会看到一个提示,比如在具体节点的终端上。此时您可以得到回溯跟踪,然后使用其中的指令读取。 [读取堆栈追踪](#reading-the-stack-trace).

<span id="debugging-tests-with-gdb"></span>

## 使用 GDB 调试测试

如果 C++ 测试失败, GDB 可以在构建目录中的测试可执行文件上直接使用。 确保以调试模式构建代码。 由于之前的构建类型可能由 CMake 缓存, 清理缓存并重建 。

``` console
$ colcon build --cmake-clean-cache --mixin debug
```

为了让 GDB 为所调用的任何共享库装入调试符号, 请确保源代码到您的环境 。 `LD_LIBRARY_PATH`.

``` console
$ source install/setup.bash
```

最后,直接通过 GDB 运行测试。例如:

``` console
$ gdb -ex run ./build/rcl/test/test_logging
```

如果代码正在丢出一个未处理的例外,可以在gtest处理之前在GDB中捕捉到它.

``` console
$ gdb ./build/rcl/test/test_logging
$ catch throw
$ run
```

<span id="automatic-backtrace-on-crash"></span>

## 崩溃时自动回溯跟踪

那个... [向后切换](https://github.com/pal-robotics/backward_ros) 库提供了美丽的堆栈痕迹, [backward_ros](https://github.com/pal-robotics/backward_ros) 包装简化了它的整合.

只要加上它作为一个依赖和 `find_package` 在您的 CMakeLists 和 落后的库中, 将会被注入到您的所有可执行文件和库中 。
