<span id="understanding-parameters"></span> <span id="ros2params"></span>
# 理解参数

**目标：** 学习在 ROS 2 中获取、设置、保存和重新加载参数。

**教程级别：** 初学者

**预计用时：** 5 分钟

<span id="background"></span>
## 背景

参数是节点的配置值，可以理解为节点的设置项。节点可以保存整数、浮点数、布尔值、字符串和列表类型的参数。在 ROS 2 中，每个节点维护自己的参数。更多背景信息见[参数概念文档](../../../Concepts/Basic/About-Parameters.md)。

<span id="prerequisites"></span>
## 前提条件

本教程使用 [turtlesim 软件包](../Introducing-Turtlesim/Introducing-Turtlesim.md)。

与往常一样，不要忘记在[每个新打开的终端](../Configuring-ROS2-Environment.md)中加载 ROS 2 环境。

<span id="tasks"></span>
## 操作步骤

<span id="setup"></span>
### 1 准备工作

启动 turtlesim 的两个节点：`/turtlesim` 和 `/teleop_turtle`。

打开新终端，运行：

```console
$ ros2 run turtlesim turtlesim_node
```

打开另一个终端，运行：

```console
$ ros2 run turtlesim turtle_teleop_key
```

<span id="ros2-param-list"></span>
### 2 ros2 param list

要查看节点的参数，打开新终端并输入：

```console
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

每个节点都有 `use_sim_time` 参数，它不是 turtlesim 特有的。

从名称可以看出，`/turtlesim` 的几个参数通过 RGB 颜色值决定 turtlesim 窗口的背景颜色。

可以使用 `ros2 param get` 确定参数的类型。

<span id="ros2-param-get"></span>
### 3 ros2 param get

使用以下命令显示参数的类型和当前值：

```console
$ ros2 param get <node_name> <parameter_name>
```

查看 `/turtlesim` 的 `background_g` 参数当前值：

```console
$ ros2 param get /turtlesim background_g
Integer value is: 86
```

现在可以确定，`background_g` 保存的是整数值。

对 `background_r` 和 `background_b` 运行同样的命令，会分别得到 `69` 和 `255`。

<span id="ros2-param-set"></span>
### 4 ros2 param set

使用以下命令在运行时修改参数值：

```console
$ ros2 param set <node_name> <parameter_name> <value>
```

修改 `/turtlesim` 的背景颜色：

```console
$ ros2 param set /turtlesim background_r 150
Set parameter successful
```

turtlesim 窗口的背景颜色应该会随之改变：

![修改参数后的背景颜色](images/set.png)

通过 `set` 命令修改参数只对当前会话有效，不会永久保存。不过，可以保存这些设置，并在下次启动节点时重新加载。

<span id="ros2-param-dump"></span>
### 5 ros2 param dump

使用以下命令查看节点当前的全部参数值：

```console
$ ros2 param dump <node_name>
```

默认情况下，命令会将内容打印到标准输出（stdout）。也可以将参数值重定向到文件中，留待以后使用。运行以下命令，将 `/turtlesim` 的当前参数配置保存为 `turtlesim.yaml`：

```console
$ ros2 param dump /turtlesim > turtlesim.yaml
```

在 shell 当前工作目录中会出现一个新文件。打开它，可以看到以下内容：

```YAML
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

如果希望以后以相同参数重新启动节点，导出参数就很方便。

<span id="ros2-param-load"></span>
### 6 ros2 param load

使用以下命令，将文件中的参数加载到正在运行的节点：

```console
$ ros2 param load <node_name> <parameter_file>
```

将 `ros2 param dump` 生成的 `turtlesim.yaml` 加载到 `/turtlesim` 节点：

```console
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

!!! note "注意"
    只读参数只能在启动时修改，启动后不能再改。因此，上面的输出会提示部分 `qos_overrides` 参数设置失败。

<span id="load-parameter-file-on-node-startup"></span>
### 7 启动节点时加载参数文件

使用以下命令，以保存的参数值启动同一个节点：

```console
$ ros2 run <package_name> <executable_name> --ros-args --params-file <file_name>
```

这与平常启动 turtlesim 的命令相同，只是增加了 `--ros-args` 和 `--params-file` 选项，并在后面指定要加载的文件。

停止当前运行的 turtlesim 节点，然后尝试用保存的参数重新启动：

```console
$ ros2 run turtlesim turtlesim_node --ros-args --params-file turtlesim.yaml
```

turtlesim 窗口应正常打开，但背景会变为之前设置的紫色。

!!! note "注意"
    在节点启动时使用参数文件，会更新所有参数，包括只读参数。

<span id="summary"></span>
## 小结

节点通过参数定义默认配置值。可以在命令行中使用 `get` 和 `set` 获取或设置参数值，也可以将参数设置保存到文件，以便在以后的会话中重新加载。

<span id="next-steps"></span>
## 后续步骤

接下来回到 ROS 2 的通信方式，学习[动作](../Understanding-ROS2-Actions/Understanding-ROS2-Actions.md)。
