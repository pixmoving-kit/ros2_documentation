---
translation_status: machine_translated
source: Tutorials/Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-turtlesim-ros2-and-rqt"></span> <span id="turtlesim"></span>

# 使用( E) `turtlesim`, `ros2`,以及 `rqt`

**目标：** 安装和使用龟兹包和 rqt 工具来准备即将到来的教程 。

**教程级别：** 入门

**用时：** 15分钟

<span id="background"></span>

## 背景

Turtlesim是用于学习ROS 2. 的轻量级模拟器,它说明了ROS 2在最基本的层面上所做的,可以让你了解你以后会如何对待真正的机器人或机器人模拟.

ros2 工具是用户如何管理,内观,并与 ROS 系统交互。 它支持多个命令, 瞄准系统的不同方面及其操作。 人们可能用它来启动节点, 设置参数, 听话题, 以及更多。 ros2 工具是 ROS 2 核心安装的一部分 。

rqt是ROS 2. 用rqt完成的每件事都可以在命令行上完成的图形用户界面(GUI)工具,但rqt提供了更方便用户的方式来操纵ROS 2元素.

此教程触及核心 ROS 2 概念, 如节点、 话题和服务。 所有这些概念将在以后的教程中详细阐述; 现在, 您只需要设置工具, 并感受它们 。

<span id="prerequisites"></span>

## 前提条件

上一个辅导, [配置环境](../Configuring-ROS2-Environment.md),会告诉你如何设置你的环境。

<span id="tasks"></span>

## 操作步骤

<span id="install-turtlesim"></span>

### 1 安装龟兹

像往常一样,开始在一个新的终端中获取您的设置文件,如 [上一个教程](../Configuring-ROS2-Environment.md).

为您的 ROS 2 Distro 安装龟兹包 :

##### Linux

``` console
$ sudo apt update
$ sudo apt install ros-rolling-turtlesim
```

##### macOS

只要您安装了 ROS 2 的归档中包含 `ros_tutorials` 寄存器, 您应该已经安装了 topsim 。

##### Windows

只要您安装了 ROS 2 的归档中包含 `ros_tutorials` 寄存器, 您应该已经安装了 topsim 。

要检查是否安装了软件包, 请运行以下命令, 命令应该返回龟兹的可执行文件列表 :

``` console
$ ros2 pkg executables turtlesim
turtlesim draw_square
turtlesim mimic
turtlesim turtle_teleop_key
turtlesim turtlesim_node
```

<span id="start-turtlesim"></span>

### 2 开始龟语

要启动龟兹姆,请在终端中输入以下命令:

``` console
$ ros2 run turtlesim turtlesim_node
[INFO] [turtlesim]: Starting turtlesim with node name /turtlesim
[INFO] [turtlesim]: Spawning turtle [turtle1] at x=[5.544445], y=[5.544445], theta=[0.000000]
```

在命令下,您将看到节点发送的信息。在那里您可以看到默认的龟名及其产物坐标。

模拟器窗口应该出现,中间有随机龟.

![](images/turtlesim.png) <span id="use-turtlesim"></span>

### 3 使用龟语

再次打开一个新的终端和源ROS 2.

现在你将运行一个新的节点来控制第一个节点中的龟:

``` console
$ ros2 run turtlesim turtle_teleop_key
```

此时您应该打开三个窗口:一个终端运行 `turtlesim_node`,一个终端运行 `turtle_teleop_key` 和龟头窗。把这些窗子排列好,以便你可以看到龟头窗,但也让终端运行 `turtle_teleop_key` 活动可以控制龟兹中的龟.

使用键盘上的箭头键来控制海龟。它会绕着屏幕移动,使用它所附的“笔”绘制它迄今所走过的路径。

> **说明**
>
> 按箭头键只会让龟类移动一段短距离,然后停下来。 这是因为,实际上,如果操作员失去了与机器人的连接,你不希望机器人继续执行指令。

您可以使用“节点”来查看节点及其相关主题、服务和行动。 `list` 各命令的子命令 :

``` console
$ ros2 node list
$ ros2 topic list
$ ros2 service list
$ ros2 action list
```

您将在未来的教程中更多地了解这些概念。 由于此教程的目的只是为了获得对龟兹的概括性概述, 您会使用 rqt 调用一些龟兹服务, 并与它们互动 。 `turtlesim_node`.

<span id="install-rqt"></span>

### 4 安装 rqt

打开新终端以安装 `rqt` 及其插件 :

##### 乌本图 Linux

``` console
$ sudo apt update
$ sudo apt install ros-rolling-rqt ros-rolling-rqt-common-plugins
```

##### macOS

在 macOS 上安装 ROS 2 的标准归档包含 `rqt` 及其插件,所以您应该已经拥有 `rqt` 已安装。

##### Windows

在 Windows 上安装 ROS 2 的标准归档包含 `rqt` 及其插件,所以您应该已经拥有 `rqt` 已安装。

运行 rqt :

``` console
$ rqt
```

<span id="use-rqt"></span>

### 5 使用 rqt

当第一次运行 rqt 时, 窗口会是空白的 。 不担心; 请选择 **插件** \> **服务** \> **服务呼叫器** 从菜单栏的顶部。

> **说明**
>
> rqt 可能需要一些时间来定位所有插件。 如果您点击的话 **插件** 但看不见 **服务** 或任何其它选项,您应当关闭 rqt 并输入命令 `rqt --force-discover` 在你的终端。

![](images/rqt.png)

使用刷新按钮到左侧 **服务** 下拉列表,以确保您龟兹节点的所有服务都可用 。

点击 **服务** 下拉列表以查看龟兹的服务,并选择 `/spawn` 服务。

<span id="try-the-spawn-service"></span>

#### 5.1 尝试产卵服务

让我们使用 rqt 呼叫 `/spawn` 服务。您可以从它的名称中猜测 `/spawn` 将在龟眼窗内再造一只龟。

给新海龟一个独特的名字,像 `turtle2`中,通过双击在 **表达式** 。您可以看到,此表达式与 **名称** 类型 **字符串**.

接下来输入一些有效的坐标, 用于培育新龟, 如 `x = 1.0` 财务报告和财务报告 `y = 1.0`.

![](images/spawn.png)

> **说明**
>
> 如果你尝试产下一只与现存海龟同名的新海龟,就像默认 `turtle1`在终端运行时,您会收到错误消息 `turtlesim_node`:
>
> ``` console
> [ERROR] [turtlesim]: A turtle named [turtle1] already exists
> ```

去产卵 `turtle2`,然后需要点击 **调用** 按钮位于 rqt 窗口右上侧。

如果服务呼叫成功, 您应该看到一个新的海龟( 也是随机设计) 在坐标处产卵 。 @ info/ plain **x** 财务报告和财务报告 **y**.

如果你用rqt刷新服务列表,你也会看到,现在有与新龟相关的服务, `/turtle2/...`,除此之外, `/turtle1/...`.

<span id="try-the-set-pen-service"></span>

#### 5.2 尝试 set_pen服务

现在让我们给 `turtle1` 使用 `/set_pen` 服务 :

![](images/set_pen.png)

值为 **r**, **g** 财务报告和财务报告 **b**,在 0 到 255 之间,设置笔的颜色 `turtle1` 与绘图,以及 **宽度** 设置线条的厚度。

拥有 `turtle1` 用明显的红线绘制,更改值为 **r** 的值,以及 **宽度** 至 5. 别忘了在更新值后拨打服务电话。

如果你回到终点站 `turtle_teleop_key` 正在运行并按箭头键,你会看到 `turtle1`笔声已经变了。

![](images/new_pen.png)

你可能也注意到, `turtle2`。那是因为没有电信节点 `turtle2`.

<span id="remapping"></span>

### 6 重新绘图

你需要第二个电信节点来控制 `turtle2`但是,如果你尝试运行相同的命令,你会注意到,这个命令也控制着 `turtle1`。改变这种行为的方法是重新绘制 `cmd_vel` A. 专题和专题 `rotate_absolute` 行动。

在一个新的终端,源ROS 2,并运行:

``` console
$ ros2 run turtlesim turtle_teleop_key --ros-args --remap turtle1/cmd_vel:=turtle2/cmd_vel --remap turtle1/rotate_absolute:=turtle2/rotate_absolute
```

现在,你可以动了 `turtle2` 当此终端运行时, 以及 `turtle1` 当另一个终端运行时 `turtle_teleop_key` 正在激活中。

![](images/remap.png) <span id="close-turtlesim"></span>

### 7 关闭龟语

为了阻止模拟,你可以进入 `Ctrl + C` 输入 `turtlesim_node` 终端,以及 `q` 输入 `turtle_teleop_key` 终端。

<span id="summary"></span>

## 小结

使用龟兹和rqt是学习ROS 2的核心概念的伟大方法.

<span id="next-steps"></span>

## 后续步骤

既然你有了乌龟和Rqt 运行, 以及一个如何工作的想法, 让我们潜入第一个核心 ROS 2 的概念 与下一个教程, [理解节点](../Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md).

<span id="related-content"></span>

## 相关内容

龟兹包可以在 [ros_tutorials](https://github.com/ros/ros_tutorials/tree/rolling/turtlesim) 复传.
