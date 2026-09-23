---
translation_status: machine_translated
source: Tutorials/Beginner-CLI-Tools/Using-Rqt-Console/Using-Rqt-Console.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-rqt-console-to-view-logs"></span> <span id="rqt-console"></span>

# 使用( E) `rqt_console` 查看日志

**目标：** 来了解一下 `rqt_console`,用于回顾日志信息的工具。

**教程级别：** 入门

**用时：** 5分钟

<span id="background"></span>

## 背景

`rqt_console` 是一个 GUI 工具, 用于在 ROS 2. 中对日志信息进行回顾 。 通常情况下, 日志信息会出现在您的终端中 。 `rqt_console`,您可以随时间而收集这些消息,仔细查看,并且更有条理地查看,过滤,保存,甚至将保存的文件重新装入到不同的时段进行回顾.

节点使用日志以各种方式输出关于事件和状态的信息,其内容通常是信息化的,为用户着想.

<span id="prerequisites"></span>

## 前提条件

你需要帮助 [rqt\_ console 和 龟兹姆](../Introducing-Turtlesim/Introducing-Turtlesim.md) 已安装。

与往常一样, [您打开的每个新终端](../Configuring-ROS2-Environment.md).

<span id="tasks"></span>

## 操作步骤

<span id="setup"></span>

### 1 设置

开始 `rqt_console` 在一个新的终端中,有以下命令:

``` console
$ ros2 run rqt_console rqt_console
```

那个... `rqt_console` 窗口将打开 :

![](images/console.png)

控制台的第一部分是显示您系统中的日志信息。

在中间,您可以选择通过排除重度级别来过滤信件。您还可以使用加号按钮在右侧添加更多的排除过滤器。

下一节用于突出显示包含您输入的字符串的信件。您也可以在本节中添加更多的过滤器 。

现在开始 `turtlesim` 在一个新的终端中,有以下命令:

``` console
$ ros2 run turtlesim turtlesim_node
```

<span id="messages-on-rqt-console"></span>

### 2 封在 rqt\_ 控制台上的信件

生成日志信件用于 `rqt_console` 让乌龟进入墙壁。在一个新的终端中,请输入 `ros2 topic pub` 命令(详细讨论于 [主题教程](../Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)(a) 将:

``` console
$ ros2 topic pub -r 1 /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0,y: 0.0,z: 0.0}}"
```

由于以上命令正在以稳定的速度发布这个话题,龟形不断奔向墙壁.  in. `rqt_console` 你将会看到同样的信息 与 `Warn` 重度水平反复显示, 像这样 :

![](images/warn.png)

新闻 `Ctrl+C` 在终端里你运行 `ros2 topic pub` 命令阻止你的乌龟进入墙壁。

<span id="logger-levels"></span>

### 3 伐木工级别

ROS 2的对流水平按严重程度排序:

> 1.  致命的
>
> 2.  错误
>
> 3.  警告
>
> 4.  资讯
>
> 5.  调试

每个级别都没有确切的标准,但可以假定:

- `Fatal` 消息显示系统将终止 试图保护自己免受损害。

- `Error` 讯息指出一些未必会破坏系统,

- `Warn` 但不要直接损害功能。

- `Info` 信件中显示事件和状态更新,作为系统运行如预期那样的直观验证。

- `Debug` 消息详细介绍了系统执行的整个逐步过程。

默认关卡是 `Info`。您将只看到默认重度级别和更严重级别的信息。

正常情况下,只有 `Debug` 信件之所以被隐藏,是因为它们是唯一比 `Info`。例如,如果设置默认关卡为 `Warn`,你只会看到严重的信息 `Warn`, `Error`,以及 `Fatal`.

<span id="set-the-default-logger-level"></span>

#### 3.1 设置默认日志级别

您可以在您第一次运行时设置默认的日志级别 `/turtlesim` 使用重映射的节点。 在终端中输入以下命令 :

``` console
$ ros2 run turtlesim turtlesim_node --ros-args --log-level WARN
```

现在,你不会看到开头 `Info` 上次在控制台上传的关卡消息 `turtlesim`. 那是因为 `Info` 消息的优先权低于新的默认重度, `Warn`.

<span id="summary"></span>

## 小结

`rqt_console` 如果您需要仔细检查您的系统中的日志消息, 将会很有帮助。 您可能出于各种原因想要检查日志消息, 通常是为了找出出错的地方和导致这种情况的一系列事件 。

<span id="next-steps"></span>

## 后续步骤

下一个教程会教你如何同时启动多个节点 [ROS 2 发射](../Launching-Multiple-Nodes/Launching-Multiple-Nodes.md).
