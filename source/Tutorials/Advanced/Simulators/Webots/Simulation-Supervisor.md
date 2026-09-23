---
translation_status: machine_translated
source: Tutorials/Advanced/Simulators/Webots/Simulation-Supervisor.rst
---

<span id="the-ros2supervisor-node"></span>

# Ros2Supervisor 节点

**目标：** 将接口延伸至默认的主管机器人, 命名为 `Ros2Supervisor`.

**教程级别：** 高级

**用时：** 10分钟

<span id="background"></span>

## 背景

在此教程中, 您将学习如何启用 `Ros2Supervisor` 节点通过创建额外的服务和主题来增强界面与模拟互动。 例如, 您可以在模拟运行时直接从ROS 2 界面记录动画或产卵 Webots 节点。 这些指令详细列出当前已执行的特性以及如何使用这些特性 。

<span id="prerequisites"></span>

## 前提条件

在进行此教程之前, 请确认您已完成以下工作 :

- 了解初步分析中涵盖的ROS 2节点和专题 [教程](../../../../Tutorials.md).

- 了解Webots和ROS 2及其接口软件包.

- 熟悉情况 [配置机器人仿真（基础）](Setting-Up-Simulation-Webots-Basic.md).

<span id="the-ros2supervisor"></span>

## 那个... `Ros2Supervisor`

那个... `Ros2Supervisor` 由两个主要部分组成:

- 一个Webots机器人节点加入了模拟世界,它的 `supervisor` 字段设置为 TRUE。

- 一个ROS 2节点连接到Webots Robot作为外部控制器(与你自己的机器人插件类似).

ROS 2节点起到控制器的作用,调用主管API功能来控制或与模拟世界互动. 用户与ROS 2节点的交互主要通过服务和话题进行.

这些节点可以在 Webots 发射时使用 `ros2_supervisor` 参数 `WebotsLauncher`.

``` python
webots = WebotsLauncher(
    world=PathJoinSubstitution([package_dir, 'worlds', world]),
    mode=mode,
    ros2_supervisor=True
)
```

那个... `webots._supervisor` 对象也必须包含在 `LaunchDescription` 由发射文件返回。

``` python
return LaunchDescription([
    webots,
    webots._supervisor,

    # This action will kill all nodes once the Webots simulation has exited
    launch.actions.RegisterEventHandler(
        event_handler=launch.event_handlers.OnProcessExit(
            target_action=webots,
            on_exit=[
                launch.actions.EmitEvent(event=launch.events.Shutdown())
            ],
        )
    )
])
```

关于发射文件的更多信息 `webots_ros2` 可在下列网址查找项目: [配置机器人仿真（基础）](Setting-Up-Simulation-Webots-Basic.md).

<span id="clock-topic"></span>

## 时钟主题

那个... `Ros2Supervisor` 节点负责获得 Webots 模拟时间并将其发布到 `/clock` 专题。这意味着,必须使该类动物成为人类的后代。 `Ros2Supervisor` 如果其他节点有它们 `use_sim_time` 参数设置为 `true`。关于 `/clock` 主题可见于 [ROS 维基](http://wiki.ros.org/Clock).

<span id="import-a-webots-node"></span>

## 导入 Webots 节点

那个... `Ros2Supervisor` 节点还允许您通过服务从字符串中生成 Webots 节点.

服务的名称 `/Ros2Supervisor/spawn_node_from_string` 类型 `webots_ros2_msgs/srv/SpawnNodeFromString`。该词 `SpawnNodeFromString` 类型需要一个 `data` 字符串作为输入并返回 a `success` 布尔。

从给定的字符串中,监理节点正在得到导入的节点的名称,并添加到内部列表中,以便日后可能移除(参见: [删除导入的 Webots 节点](#remove-a-webots-imported-node)).

节点是使用 `importMFNodeFromString(nodeString)` [API 函数](https://cyberbotics.com/doc/reference/supervisor?tab-language=python#wb_supervisor_field_import_mf_node_from_string).

以下是导入一个简单的机器人的例子 。 `imported_robot`:

``` console
$ ros2 service call /Ros2Supervisor/spawn_node_from_string webots_ros2_msgs/srv/SpawnNodeFromString "data: Robot { name \"imported_robot\" }"
```

> **说明**
>
> 如果您试图在节点字符串中导入一些 PROTO, 则必须在 `.wbt` 世界文件作为 EXTERNPROTO 或 作为 重要 EXTERNPROTO 。

<span id="remove-a-webots-imported-node"></span> <span id="id1"></span>

## 删除导入的 Webots 节点

一旦一个节点被导入 `/Ros2Supervisor/spawn_node_from_string` 服务,也可以删除。

通过将节点名称发送到命名的主题来实现 `/Ros2Supervisor/remove_node` 类型 `std_msgs/msg/String`.

如果该节点确实在输入的列表中,则与该节点一起删除。 `remove()` [API 方法](https://cyberbotics.com/doc/reference/supervisor?tab-language=python#wb_supervisor_node_remove).

这里有一个例子,说明如何删除 `imported_robot` 机器人:

``` console
$ ros2 topic pub --once /Ros2Supervisor/remove_node std_msgs/msg/String "{data: imported_robot}"
```

<span id="record-animations"></span>

## 记录动画

那个... `Ros2Supervisor` 节点还创建了两个额外的服务来记录HTML5动画.

那个... `/Ros2Supervisor/animation_start_recording` 服务类型 `webots_ros2_msgs/srv/SetString` 并允许开始动画。 `SetString` 类型需要一个 `value` 字符串作为输入并返回 a `success` 布尔。 输入 `value` 代表用于保存动画文件的目录的绝对路径。

以下是如何开始动画的一个例子:

``` console
$ ros2 service call /Ros2Supervisor/animation_start_recording webots_ros2_msgs/srv/SetString "{value: "<ABSOLUTE_PATH>/index.html"}"
```

那个... `/Ros2Supervisor/animation_stop_recording` 服务类型 `webots_ros2_msgs/srv/GetBool` 并允许停止动画.

``` console
$ ros2 service call /Ros2Supervisor/animation_stop_recording webots_ros2_msgs/srv/GetBool "{ask: True}"
```

<span id="summary"></span>

## 小结

在这个教程中,你学会了如何启用 `Ros2Supervisor` 以及如何扩展与 Webots 模拟的接口。节点创建多个服务和主题,以便与模拟进行互动和修改。
