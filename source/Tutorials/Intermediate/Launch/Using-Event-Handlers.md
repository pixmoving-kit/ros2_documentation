<span id="using-event-handlers"></span>

# 使用事件处理器

**目标：** 了解 ROS 2 启动文件中的事件处理器。

**教程级别：** 中级

**预计耗时：** 15 分钟

<span id="background"></span>

## 背景

ROS 2 的启动系统负责执行和管理用户定义的进程，监控已启动进程的状态，并报告和响应状态变化。这些变化称为事件，可以通过向启动系统注册事件处理器来处理。事件处理器可针对特定事件注册，既能监控进程状态，也能定义一组复杂规则，动态调整启动文件的行为。

本教程展示 ROS 2 启动文件中事件处理器的使用示例。

<span id="prerequisites"></span>

## 前提条件

本教程使用 [turtlesim](../../Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md) 包，并假设你已[创建](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md)名为 `launch_tutorial`、构建类型为 `ament_python` 的包。

本教程在[启动文件中使用替换](Using-Substitutions.md)教程的代码基础上继续扩展。

<span id="id1"></span>

## 使用事件处理器

<span id="event-handlers-example-launch-file"></span>

### 1 事件处理器示例启动文件

在 `launch_tutorial` 包的 `launch` 目录中创建 `example_event_handlers.launch.py`，使用[完整示例代码](launch/example_event_handlers_launch.py)。

启动描述通过 `RegisterEventHandler` 动作，为 `OnProcessStart`、`OnProcessIO`、`OnExecutionComplete`、`OnProcessExit` 和 `OnShutdown` 事件注册处理器。

`OnProcessStart` 注册的回调在 turtlesim 节点启动时执行，向控制台记录消息，并执行 `spawn_turtle` 动作。参见[示例第 98–106 行](launch/example_event_handlers_launch.py)。

`OnProcessIO` 注册的回调在 `spawn_turtle` 动作写入标准输出时执行，记录生成海龟请求的结果。参见[示例第 107–115 行](launch/example_event_handlers_launch.py)。

`OnExecutionComplete` 注册的回调在 `spawn_turtle` 动作完成时执行，向控制台记录消息，并执行 `change_background_r` 和 `change_background_r_conditioned` 动作。参见[示例第 116–128 行](launch/example_event_handlers_launch.py)。

`OnProcessExit` 注册的回调在 turtlesim 节点退出时执行，向控制台记录消息，并通过 `EmitEvent` 动作发出 `Shutdown` 事件。因此，关闭 turtlesim 窗口时，整个启动进程也会关闭。参见[示例第 129–139 行](launch/example_event_handlers_launch.py)。

最后，`OnShutdown` 注册的回调在启动文件收到关闭请求时执行，向控制台记录关闭原因，例如 turtlesim 窗口关闭，或用户按下 `Ctrl+C`。参见[示例第 140–146 行](launch/example_event_handlers_launch.py)。

<span id="build-the-package"></span>

## 构建软件包

进入工作空间根目录并构建软件包：

```console
$ colcon build
```

构建后记得加载工作空间环境。

<span id="launching-example"></span>

## 运行示例

现在可以通过 `ros2 launch` 运行 `example_event_handlers.launch.py`：

```console
$ ros2 launch launch_tutorial example_event_handlers.launch.py turtlesim_ns:='turtlesim3' use_provided_red:='True' new_background_r:=200
```

它将执行以下操作：

1. 启动背景为蓝色的 turtlesim 节点。
2. 生成第二只海龟。
3. 将背景改为紫色。
4. 如果提供的 `background_r` 参数为 `200` 且 `use_provided_red` 为 `True`，则在两秒后将背景改为粉色。
5. 关闭 turtlesim 窗口时，关闭启动文件。

此外，以下情况会向控制台记录消息：

1. turtlesim 节点启动。
2. 生成海龟的动作执行。
3. `change_background_r` 动作执行。
4. `change_background_r_conditioned` 动作执行。
5. turtlesim 节点退出。
6. 启动进程收到关闭请求。

<span id="documentation"></span>

## 文档

[launch 文档](https://github.com/ros2/launch/blob/rolling/launch/doc/source/architecture.rst)提供了可用事件处理器的详细信息。

<span id="summary"></span>

## 小结

本教程介绍了启动文件中事件处理器的语法和用法，以及如何用它们定义复杂规则，动态调整启动文件的行为。
