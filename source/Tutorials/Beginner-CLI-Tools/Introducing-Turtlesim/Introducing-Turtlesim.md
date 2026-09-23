<span id="using-turtlesim-ros2-and-rqt"></span> <span id="turtlesim"></span>
# 使用 turtlesim、ros2 和 rqt

**目标：** 安装并使用 turtlesim 软件包和 rqt 工具，为后续教程做好准备。

**教程级别：** 初学者

**预计用时：** 15 分钟

<span id="background"></span>
## 背景

Turtlesim 是用于学习 ROS 2 的轻量级仿真器。它展示了 ROS 2 最基本的工作方式，帮助你了解今后操作真实机器人或机器人仿真系统时会做些什么。

`ros2` 工具用于管理、查看 ROS 系统的内部状态，以及与系统交互。它支持多种命令，分别针对系统及其运行的不同方面，例如启动节点、设置参数、监听话题等。`ros2` 工具包含在 ROS 2 核心安装中。

rqt 是 ROS 2 的图形用户界面（GUI）工具。rqt 中的所有操作都可以通过命令行完成，而 rqt 为操作 ROS 2 各种要素提供了更友好的界面。

本教程会涉及节点、话题和服务等 ROS 2 核心概念。后续教程会详细讲解这些概念；现在只需配置好工具，并熟悉它们的基本使用方式。

<span id="prerequisites"></span>
## 前提条件

上一篇教程[配置环境](../Configuring-ROS2-Environment.md)介绍了如何设置环境。

<span id="tasks"></span>
## 操作步骤

<span id="install-turtlesim"></span>
### 1 安装 turtlesim

与往常一样，先打开新终端，按照[上一篇教程](../Configuring-ROS2-Environment.md)加载环境设置文件。

为所用 ROS 2 发行版安装 turtlesim 软件包：

**Linux**

```console
$ sudo apt update
$ sudo apt install ros-rolling-turtlesim
```

**macOS 和 Windows**

只要安装 ROS 2 时使用的归档包包含 `ros_tutorials` 仓库，就应该已经安装了 turtlesim。

运行以下命令检查软件包是否安装成功。它应返回 turtlesim 中的可执行文件列表：

```console
$ ros2 pkg executables turtlesim
turtlesim draw_square
turtlesim mimic
turtlesim turtle_teleop_key
turtlesim turtlesim_node
```

<span id="start-turtlesim"></span>
### 2 启动 turtlesim

在终端中输入以下命令，启动 turtlesim：

```console
$ ros2 run turtlesim turtlesim_node
[INFO] [turtlesim]: Starting turtlesim with node name /turtlesim
[INFO] [turtlesim]: Spawning turtle [turtle1] at x=[5.544445], y=[5.544445], theta=[0.000000]
```

命令下方会显示节点输出的信息，其中包括默认海龟的名称和生成位置坐标。

随后应出现仿真器窗口，中央显示一只外观随机的海龟。

![turtlesim 仿真器窗口](images/turtlesim.png)

<span id="use-turtlesim"></span>
### 3 使用 turtlesim

打开新终端，再次加载 ROS 2 环境。

运行一个新节点，用来控制第一个节点中的海龟：

```console
$ ros2 run turtlesim turtle_teleop_key
```

此时应有三个窗口：运行 `turtlesim_node` 的终端、运行 `turtle_teleop_key` 的终端，以及 turtlesim 窗口。调整窗口位置，确保可以看到 turtlesim 窗口，同时让运行 `turtle_teleop_key` 的终端保持焦点，以便控制海龟。

使用键盘方向键控制海龟。海龟会在窗口中移动，并用附带的“画笔”画出经过的路径。

!!! note "注意"
    按一次方向键只会让海龟移动一小段距离，随后便会停止。在实际应用中，例如操作员与机器人失去连接时，我们通常不希望机器人继续执行此前的指令。

可以使用各命令的 `list` 子命令查看节点，以及相关的话题、服务和动作：

```console
$ ros2 node list
$ ros2 topic list
$ ros2 service list
$ ros2 action list
```

后续教程会进一步介绍这些概念。本教程只需大致了解 turtlesim，因此接下来使用 rqt 调用 turtlesim 的一些服务，与 `turtlesim_node` 交互。

<span id="install-rqt"></span>
### 4 安装 rqt

打开新终端，安装 `rqt` 及其插件：

**Ubuntu Linux**

```console
$ sudo apt update
$ sudo apt install ros-rolling-rqt ros-rolling-rqt-common-plugins
```

**macOS 和 Windows**

这两个平台的标准 ROS 2 安装归档包都包含 `rqt` 及其插件，因此应该已经安装好 `rqt`。

运行 rqt：

```console
$ rqt
```

<span id="use-rqt"></span>
### 5 使用 rqt

首次运行 rqt 时，窗口是空白的。只需从顶部菜单栏选择 **Plugins > Services > Service Caller**。

!!! note "注意"
    rqt 可能需要一些时间查找所有插件。如果点击 **Plugins** 后看不到 **Services** 或其他选项，请关闭 rqt，然后在终端中运行 `rqt --force-discover`。

![rqt 服务调用界面](images/rqt.png)

点击 **Service** 下拉列表左侧的刷新按钮，确保 turtlesim 节点的所有服务都已列出。

打开 **Service** 下拉列表查看服务，并选择 `/spawn`。

<span id="try-the-spawn-service"></span>
#### 5.1 尝试 spawn 服务

用 rqt 调用 `/spawn` 服务。从名称可以猜到，它会在 turtlesim 窗口中生成另一只海龟。

双击 **Expression** 列中空单引号之间的位置，为新海龟输入一个唯一名称，例如 `turtle2`。该表达式对应 **name** 的值，类型为 **string**。

接下来输入有效的生成位置坐标，例如 `x = 1.0`、`y = 1.0`。

![设置新海龟的名称与坐标](images/spawn.png)

!!! note "注意"
    如果尝试使用已有海龟的名称，例如默认的 `turtle1`，运行 `turtlesim_node` 的终端会显示错误信息：

```console
[ERROR] [turtlesim]: A turtle named [turtle1] already exists
```

点击 rqt 窗口右上方的 **Call** 按钮调用服务，生成 `turtle2`。

调用成功后，一只新的海龟会出现在输入的 **x**、**y** 坐标处，其外观同样是随机的。

刷新 rqt 服务列表后，除了 `/turtle1/...`，还会看到与新海龟相关的 `/turtle2/...` 服务。

<span id="try-the-set-pen-service"></span>
#### 5.2 尝试 set_pen 服务

现在通过 `/set_pen` 服务，为 `turtle1` 设置一支特别的画笔：

![画笔设置服务](images/set_pen.png)

**r**、**g** 和 **b** 的取值范围为 0 到 255，用于设置 `turtle1` 画笔的颜色；**width** 用于设置线条粗细。

将 **r** 改为 255，将 **width** 改为 5，即可让 `turtle1` 画出明显的红色线条。修改值后别忘了调用服务。

回到运行 `turtle_teleop_key` 的终端，按方向键移动海龟，就能看到 `turtle1` 的画笔已经改变。

![修改后的红色画笔](images/new_pen.png)

你可能也发现了，现在还无法移动 `turtle2`，因为尚未为它启动遥控节点。

<span id="remapping"></span>
### 6 重映射

要控制 `turtle2`，需要第二个遥控节点。不过，如果直接重复之前的命令，新节点控制的仍然是 `turtle1`。要改变这一行为，需要重映射 `cmd_vel` 话题和 `rotate_absolute` 动作。

打开新终端，加载 ROS 2 环境，然后运行：

```console
$ ros2 run turtlesim turtle_teleop_key --ros-args --remap turtle1/cmd_vel:=turtle2/cmd_vel --remap turtle1/rotate_absolute:=turtle2/rotate_absolute
```

现在，当这个终端获得焦点时，可以控制 `turtle2`；当另一个运行 `turtle_teleop_key` 的终端获得焦点时，则可以控制 `turtle1`。

![分别控制两只海龟](images/remap.png)

<span id="close-turtlesim"></span>
### 7 关闭 turtlesim

在运行 `turtlesim_node` 的终端按 `Ctrl + C`，在运行 `turtle_teleop_key` 的终端按 `q`，即可结束仿真。

<span id="summary"></span>
## 小结

使用 turtlesim 和 rqt 是学习 ROS 2 核心概念的好方法。

<span id="next-steps"></span>
## 后续步骤

现在 turtlesim 和 rqt 都已运行起来，你也了解了它们的基本用法。接下来通过[理解节点](../Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md)，深入学习第一个 ROS 2 核心概念。

<span id="related-content"></span>
## 相关内容

turtlesim 软件包位于 [ros_tutorials 仓库](https://github.com/ros/ros_tutorials/tree/rolling/turtlesim)。
