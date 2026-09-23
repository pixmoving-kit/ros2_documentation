---
translation_status: machine_translated
source: How-To-Guides/Node-arguments.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="passing-ros-arguments-to-nodes-via-the-command-line"></span>

# 通过命令行为节点传递 ROS 参数

所有 ROS 节点都采用一组参数,允许重新配置各种属性。实例包括配置节点的名称/名称空间、所使用的主题/服务名称以及节点上的参数。所有 ROS 特定参数必须先于 `--ros-args` 旗帜 :

``` console
$ ros2 run my_package node_executable --ros-args ...
```

详情见 [此设计文件](https://design.ros2.org/articles/ros_command_line_arguments.html).

<span id="name-remapping"></span>

## 名称重映射

节点中的名称( 如话题/ 服务) 可以使用语法重映 `-r <old name>:=<new name>`。节点本身的名称/名称空间可以使用 `-r __node:=<new node name>` 财务报告和财务报告 `-r __ns:=<new node namespace>`.

请注意,这些重新绘图是“静态”的重新绘图,因为它们适用于节点的寿命。 节点开始后对名称的“动态”重新绘图尚未得到支持。

见 [此设计文件](https://design.ros2.org/articles/static_remapping.html) 关于重映射参数的更多细节(并非所有功能都可用).

<span id="example"></span>

### 示例

以下援引将造成: `talker` 节点将在节点名称下启动 `my_talker`,关于命名主题的出版 `my_topic` 而不是默认 `chatter`。命名空间必须从向前斜线开始,它将被设定为 `/demo`,这意味着在命名空间中创建主题(`/demo/my_topic`),而不是全球范围(A/CN.9/WG.III/WP.18)。`/my_topic`).

``` console
$ ros2 run demo_nodes_cpp talker --ros-args -r __ns:=/demo -r __node:=my_talker -r chatter:=my_topic
```

<span id="passing-remapping-arguments-to-specific-nodes"></span>

#### 将重映射参数传递到特定节点

如果多个节点正在一个进程内运行(例如使用) [组件组合](../Concepts/Intermediate/About-Composition.md),),重映射参数可以使用它的名称作为前缀传递给特定的节点. 例如,下面将重映射参数传递给指定的节点:

``` console
$ ros2 run composition manual_composition --ros-args -r talker:__node:=my_talker -r listener:__node:=my_listener
```

以下示例既会更改节点名称, 也会重新绘制一个主题( 节点和命名空间的更改总是应用) 。 *在此之前* 主题重映射 :

``` console
$ ros2 run composition manual_composition --ros-args -r talker:__node:=my_talker -r my_talker:chatter:=my_topic -r listener:__node:=my_listener -r my_listener:chatter:=my_topic
```

<span id="logger-configuration"></span>

## Logger 配置

见 `--log-level` 参数用法 [日志页面](../Tutorials/Demos/Logging-and-logger-configuration.md).

<span id="parameters"></span>

## 参数

<span id="setting-parameters-directly-from-the-command-line"></span> <span id="nodeargsparameters"></span>

### 直接从命令行设置参数

您可以使用以下语法从命令行直接设置参数:

``` console
$ ros2 run package_name executable_name --ros-args -p param_name:=param_value
```

例如,您可以运行 :

``` console
$ ros2 run demo_nodes_cpp parameter_blackboard --ros-args -p some_int:=42 -p "a_string:=Hello world" -p "some_lists.some_integers:=[1, 2, 3, 4]" -p "some_lists.some_doubles:=[3.14, 2.718]"
```

其他节点将能够检索参数值,例如:

``` console
$ ros2 param list parameter_blackboard
a_string
qos_overrides./parameter_events.publisher.depth
qos_overrides./parameter_events.publisher.durability
qos_overrides./parameter_events.publisher.history
qos_overrides./parameter_events.publisher.reliability
some_int
some_lists.some_doubles
some_lists.some_integers
use_sim_time
```

<span id="setting-parameters-from-yaml-files"></span>

### 从 YAML 文件设置参数

参数可以从命令行中以yaml文件的形式设定.

[看这里](https://github.com/ros2/rcl/tree/rolling/rcl_yaml_param_parser) 用于 Yaml 文件语法的示例。

举例来说,将以下内容保存为 `demo_params.yaml`:

``` yaml
parameter_blackboard:
    ros__parameters:
        some_int: 42
        a_string: "Hello world"
        some_lists:
            some_integers: [1, 2, 3, 4]
            some_doubles : [3.14, 2.718]

/**:
  ros__parameters:
    wildcard_full: "Full wildcard for any namespaces and any node names"

/**/parameter_blackboard:
  ros__parameters:
    wildcard_namespace: "Wildcard for a specific node name under any namespace"

/*:
  ros__parameters:
    wildcard_nodename_root_namespace: "Wildcard for any node names, but only in root namespace"
```

> **说明**
>
> 结卡可用于节点名称和命名空间. `*` 匹配划线划定的单个符号(`/`). `**` 匹配零或更多的按划线划分的符号。部分匹配是不允许的(如: `foo*`).

然后在节点内宣布参数 [declare_parameter](http://docs.ros.org/en/rolling/p/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4N6rclcpp4Node17declare_parameterERKNSt6stringERKN6rclcpp14ParameterValueERKN14rcl_interfaces3msg19ParameterDescriptorEb) 或 时 间 [declare_parameters](http://docs.ros.org/en/rolling/p/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4I0EN6rclcpp4Node18declare_parametersENSt6vectorI10ParameterTEERKNSt6stringERKNSt3mapINSt6stringENSt4pairI10ParameterTN14rcl_interfaces3msg19ParameterDescriptorEEEEEb),或 [设置自动声明参数的节点](http://docs.ros.org/en/rolling/p/rclcpp/generated/classrclcpp_1_1NodeOptions.html#_CPPv4NK6rclcpp11NodeOptions47automatically_declare_parameters_from_overridesEv) 如果它们通过命令行控制器被传入。

然后运行如下:

``` console
$ ros2 run demo_nodes_cpp parameter_blackboard --ros-args --params-file demo_params.yaml
```

其他节点将能够检索参数值,例如:

``` console
$ ros2 param list parameter_blackboard
a_string
qos_overrides./parameter_events.publisher.depth
qos_overrides./parameter_events.publisher.durability
qos_overrides./parameter_events.publisher.history
qos_overrides./parameter_events.publisher.reliability
some_int
some_lists.some_doubles
some_lists.some_integers
use_sim_time
wildcard_full
wildcard_namespace
wildcard_nodename_root_namespace
```
