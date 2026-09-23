---
translation_status: machine_translated
source: Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="understanding-parameters"></span> <span id="ros2params"></span>

# 理解参数

**目标：** 学习如何在ROS 2中获取,设定,保存和重新装入参数.

**教程级别：** 入门

**用时：** 5分钟

<span id="background"></span>

## 背景

参数是一个节点的配置值。 您可以将参数视为节点设置。 节点可以存储参数为整数、 浮点、 布尔、 字符串和列表。 在 ROS 2 中, 每个节点都保留自己的参数。 更多参数的背景请参见 。 [概念文件](../../../Concepts/Basic/About-Parameters.md).

<span id="prerequisites"></span>

## 前提条件

此教程使用 [龟兹包](../Introducing-Turtlesim/Introducing-Turtlesim.md).

与往常一样, [您打开的每个新终端](../Configuring-ROS2-Environment.md).

<span id="tasks"></span>

## 操作步骤

<span id="setup"></span>

### 1 设置

启动两个乌龟结点, `/turtlesim` 财务报告和财务报告 `/teleop_turtle`.

打开新的终端并运行 :

``` console
$ ros2 run turtlesim turtlesim_node
```

打开另一个终端并运行 :

``` console
$ ros2 run turtlesim turtle_teleop_key
```

<span id="ros2-param-list"></span>

### 2 ros2 参数列表

要查看属于您节点的参数, 请打开一个新的终端并输入命令 :

``` console
$ ros2 param list
/teleop_turtle:
  qos_overrides./parameter_events.publisher.depth
  qos_overrides./parameter_events.publisher.durability
  qos_overrides./parameter_events.publisher.history
  qos_overrides./parameter_events.publisher.reliability
  scale_angular
  scale_linear
  use_sim_time
/turtlesim:
  background_b
  background_g
  background_r
  qos_overrides./parameter_events.publisher.depth
  qos_overrides./parameter_events.publisher.durability
  qos_overrides./parameter_events.publisher.history
  qos_overrides./parameter_events.publisher.reliability
  use_sim_time
```

每个节点都有参数 `use_sim_time`这并非龟兹独有。

根据他们的名字,看起来是 `/turtlesim`其参数使用RGB颜色值确定龟兹窗口的背景颜色.

要确定参数的类型, 您可以使用 `ros2 param get`.

<span id="ros2-param-get"></span>

### 3 个 ros2 参数

要显示参数的类型和当前值,请使用命令:

``` console
$ ros2 param get <node_name> <parameter_name>
```

让我们找出当前值 `/turtlesim`参数 `background_g`:

``` console
$ ros2 param get /turtlesim background_g
Integer value is: 86
```

现在你知道了 `background_g` 持有整数。

如果你运行相同的命令在 `background_r` 财务报告和财务报告 `background_b`,你会得到数值 `69` 财务报告和财务报告 `255`分别是:

<span id="ros2-param-set"></span>

### 4个ROS2 参数集

要在运行时更改参数值, 请使用命令 :

``` console
$ ros2 param set <node_name> <parameter_name> <value>
```

让我们改变吧 `/turtlesim`背景颜色 :

``` console
$ ros2 param set /turtlesim background_r 150
Set parameter successful
```

您的龟形窗口的背景应该改变颜色 :

![](images/set.png)

设置参数与 `set` 命令将只在当前会话中更改它们,而不是永久更改。然而,您可以保存您的设置,并在下次启动节点时重新加载它们。

<span id="ros2-param-dump"></span>

### 5个Ros2 param 垃圾堆

您可以使用命令查看节点当前所有参数值 :

``` console
$ ros2 param dump <node_name>
```

命令打印到标准输出( stdout) 默认情况下, 您也可以将参数值重定向到文件中, 以保存到以后。 要保存您的当前配置 。 `/turtlesim`输入文件的参数 `turtlesim.yaml`,输入命令 :

``` console
$ ros2 param dump /turtlesim > turtlesim.yaml
```

您将在当前的工作目录中找到您 shell 正在运行的新文件 。 如果您打开此文件, 您将会看到以下内容 :

``` YAML
/turtlesim:
  ros__parameters:
    background_b: 255
    background_g: 86
    background_r: 150
    qos_overrides:
      /parameter_events:
        publisher:
          depth: 1000
          durability: volatile
          history: keep_last
          reliability: reliable
    use_sim_time: false
```

倾销参数有帮助,如果将来你想用相同的参数重新装入节点.

<span id="ros2-param-load"></span>

### 6 ros2 参数载荷

您可以使用命令从文件装入参数到当前运行的节点 :

``` console
$ ros2 param load <node_name> <parameter_file>
```

装入 `turtlesim.yaml` 文件生成于 `ros2 param dump` 输入 `/turtlesim` 节点的参数,输入命令:

``` console
$ ros2 param load /turtlesim turtlesim.yaml
Set parameter background_b successful
Set parameter background_g successful
Set parameter background_r successful
Set parameter qos_overrides./parameter_events.publisher.depth failed: parameter 'qos_overrides./parameter_events.publisher.depth' cannot be set because it is read-only
Set parameter qos_overrides./parameter_events.publisher.durability failed: parameter 'qos_overrides./parameter_events.publisher.durability' cannot be set because it is read-only
Set parameter qos_overrides./parameter_events.publisher.history failed: parameter 'qos_overrides./parameter_events.publisher.history' cannot be set because it is read-only
Set parameter qos_overrides./parameter_events.publisher.reliability failed: parameter 'qos_overrides./parameter_events.publisher.reliability' cannot be set because it is read-only
Set parameter use_sim_time successful
```

> **说明**
>
> 只读参数只能在启动时修改,而不是在启动后修改,这就是为什么对“qos_overrides”参数有一些警告。

<span id="load-parameter-file-on-node-startup"></span>

### 7 在节点启动时装入参数文件

要使用您保存的参数值启动相同的节点, 请使用 :

``` console
$ ros2 run <package_name> <executable_name> --ros-args --params-file <file_name>
```

这是你总是用来启动龟形的指令, 上面加了旗帜 `--ros-args` 财务报告和财务报告 `--params-file`,然后是您要装入的文件。

停止运行龟兹节点,并尝试用保存的参数重新装入,使用:

``` console
$ ros2 run turtlesim turtlesim_node --ros-args --params-file turtlesim.yaml
```

龟眼窗应该像往常一样出现,但有你先前设定的紫色背景.

> **说明**
>
> 当在节点启动时使用一个参数文件时,所有参数,包括只读参数都会更新.

<span id="summary"></span>

## 小结

节点有参数来定义其默认配置值。您可以 `get` 财务报告和财务报告 `set` 命令行中的参数值。您也可以将参数设置保存到文件,以便在未来的会话中重新加载它们。

<span id="next-steps"></span>

## 后续步骤

跳回 ROS 2 通讯方法, 在下一个课程中您会了解 [动作](../Understanding-ROS2-Actions/Understanding-ROS2-Actions.md).
