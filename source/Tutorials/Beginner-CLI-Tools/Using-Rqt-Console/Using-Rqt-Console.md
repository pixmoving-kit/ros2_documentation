<span id="using-rqt-console-to-view-logs"></span> <span id="rqt-console"></span>
# 使用 rqt_console 查看日志

**目标：** 了解用于查看日志消息的工具 `rqt_console`。

**教程级别：** 初学者

**预计用时：** 5 分钟

<span id="background"></span>
## 背景

`rqt_console` 是 ROS 2 中用于查看日志消息的图形界面工具。通常，日志消息显示在终端中。使用 `rqt_console`，可以持续收集这些消息，以更有条理的方式仔细查看、筛选和保存它们，还可以在以后重新加载保存的文件继续分析。

节点通过日志输出与事件和状态相关的各类消息，通常是为了向用户提供信息。

<span id="prerequisites"></span>
## 前提条件

需要安装 [rqt_console 和 turtlesim](../Introducing-Turtlesim/Introducing-Turtlesim.md)。

与往常一样，不要忘记在[每个新打开的终端](../Configuring-ROS2-Environment.md)中加载 ROS 2 环境。

<span id="tasks"></span>
## 操作步骤

<span id="setup"></span>
### 1 准备工作

在新终端中运行以下命令，启动 `rqt_console`：

```console
$ ros2 run rqt_console rqt_console
```

随后会打开 `rqt_console` 窗口：

![rqt_console 日志查看界面](images/console.png)

控制台上方区域显示系统的日志消息。

中间区域可以按严重程度排除消息，从而筛选日志。点击右侧的加号按钮，还可以添加更多排除过滤器。

底部区域用于高亮包含指定字符串的消息，也可以添加更多过滤器。

现在打开新终端，运行以下命令启动 `turtlesim`：

```console
$ ros2 run turtlesim turtlesim_node
```

<span id="messages-on-rqt-console"></span>
### 2 在 rqt_console 中查看消息

让海龟撞向墙壁，产生一些可供 `rqt_console` 显示的日志消息。在新终端中输入以下 `ros2 topic pub` 命令，其详细用法见[话题教程](../Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)：

```console
$ ros2 topic pub -r 1 /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0,y: 0.0,z: 0.0}}"
```

由于命令以固定频率持续向话题发布数据，海龟会不断撞向墙壁。`rqt_console` 中会反复显示相同的 `Warn` 级别消息：

![反复出现的警告日志](images/warn.png)

在运行 `ros2 topic pub` 的终端中按 `Ctrl+C`，停止让海龟撞墙。

<span id="logger-levels"></span>
### 3 日志级别

ROS 2 的日志级别按严重程度从高到低排列如下：

1. Fatal：致命错误
2. Error：错误
3. Warn：警告
4. Info：信息
5. Debug：调试

各级别没有严格统一的含义标准，但通常可以这样理解：

- `Fatal`：系统即将终止运行，以避免进一步损害。
- `Error`：出现了严重问题，虽然不一定会损坏系统，但已经妨碍系统正常工作。
- `Warn`：出现了意外行为或不理想的结果，可能意味着更深层的问题，但尚未直接破坏功能。
- `Info`：报告事件和状态更新，让用户直观看到系统正在按预期运行。
- `Debug`：详细记录系统执行过程中的各个步骤。

默认日志级别是 `Info`。只会显示所设级别以及比它更严重的消息。

通常只有 `Debug` 消息被隐藏，因为它是唯一低于 `Info` 的级别。例如，将默认级别设为 `Warn` 后，就只会看到 `Warn`、`Error` 和 `Fatal` 消息。

<span id="set-the-default-logger-level"></span>
#### 3.1 设置默认日志级别

首次运行 `/turtlesim` 节点时，可以通过命令行指定默认日志级别。在终端中输入：

```console
$ ros2 run turtlesim turtlesim_node --ros-args --log-level WARN
```

现在不会再看到上次启动 `turtlesim` 时控制台显示的初始 `Info` 消息，因为 `Info` 的严重程度低于新设置的默认级别 `Warn`。

<span id="summary"></span>
## 小结

需要仔细检查系统日志时，`rqt_console` 很有帮助。查看日志的原因有很多，通常是为了找出问题发生的位置，以及导致问题的一系列事件。

<span id="next-steps"></span>
## 后续步骤

下一篇教程将介绍使用 [ROS 2 Launch](../Launching-Multiple-Nodes/Launching-Multiple-Nodes.md)一次启动多个节点。
