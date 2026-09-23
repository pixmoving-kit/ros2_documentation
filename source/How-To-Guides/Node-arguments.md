<span id="passing-ros-arguments-to-nodes-via-the-command-line"></span>
# 通过命令行向节点传递 ROS 参数

所有 ROS 节点都接受一组命令行参数，用于重新配置各种属性，例如节点名称与命名空间、所使用的话题和服务名称，以及节点参数。所有 ROS 专用的命令行参数都必须放在 `--ros-args` 标志之后：

```console
$ ros2 run my_package node_executable --ros-args ...
```

更多详情见[设计文档](https://design.ros2.org/articles/ros_command_line_arguments.html)。

<span id="name-remapping"></span>
## 名称重映射

可以使用 `-r <old name>:=<new name>` 语法，重映射节点内部使用的名称，例如话题和服务名称。节点本身的名称与命名空间分别可以使用 `-r __node:=<new node name>` 和 `-r __ns:=<new node namespace>` 重映射。

这些重映射是“静态”的，在节点的整个生命周期中生效。目前尚不支持在节点启动后“动态”重映射名称。

重映射参数的更多详情见[设计文档](https://design.ros2.org/articles/static_remapping.html)，其中部分功能尚未实现。

<span id="example"></span>
### 示例

下面的命令以 `my_talker` 为节点名称启动 `talker`，并向 `my_topic` 话题发布消息，而不是默认的 `chatter`。命名空间必须以正斜杠开头，此处设为 `/demo`，因此话题会创建在该命名空间内，即 `/demo/my_topic`，而不是全局的 `/my_topic`。

```console
$ ros2 run demo_nodes_cpp talker --ros-args -r __ns:=/demo -r __node:=my_talker -r chatter:=my_topic
```

<span id="passing-remapping-arguments-to-specific-nodes"></span>
#### 向特定节点传递重映射参数

如果在同一进程中运行多个节点，例如使用[组件组合](../Concepts/Intermediate/About-Composition.md)，可以在重映射参数前加上节点名称前缀，使参数只作用于指定节点。例如：

```console
$ ros2 run composition manual_composition --ros-args -r talker:__node:=my_talker -r listener:__node:=my_listener
```

以下示例同时修改节点名称并重映射话题。节点名称和命名空间的更改总是*先于*话题重映射应用：

```console
$ ros2 run composition manual_composition --ros-args -r talker:__node:=my_talker -r my_talker:chatter:=my_topic -r listener:__node:=my_listener -r my_listener:chatter:=my_topic
```

<span id="logger-configuration"></span>
## 日志记录器配置

`--log-level` 参数的用法见[日志文档](../Tutorials/Demos/Logging-and-logger-configuration.md)。

<span id="parameters"></span>
## 参数

<span id="setting-parameters-directly-from-the-command-line"></span> <span id="nodeargsparameters"></span>
### 直接通过命令行设置参数

可以使用以下语法直接从命令行设置参数：

```console
$ ros2 run package_name executable_name --ros-args -p param_name:=param_value
```

例如，可以运行：

```console
$ ros2 run demo_nodes_cpp parameter_blackboard --ros-args -p some_int:=42 -p "a_string:=Hello world" -p "some_lists.some_integers:=[1, 2, 3, 4]" -p "some_lists.some_doubles:=[3.14, 2.718]"
```

其他节点随后就能够获取这些参数值，例如：

```console
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
### 通过 YAML 文件设置参数

可以从命令行指定 YAML 文件来设置参数。YAML 文件语法示例见 [rcl_yaml_param_parser](https://github.com/ros2/rcl/tree/rolling/rcl_yaml_param_parser)。

例如，将以下内容保存为 `demo_params.yaml`：

```yaml
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

!!! note "说明"
    节点名称和命名空间可以使用通配符。`*` 匹配一个由斜杠 `/` 分隔的片段，`**` 匹配零个或多个由斜杠分隔的片段。不支持局部匹配，例如 `foo*`。

然后，在节点中使用 [`declare_parameter`](http://docs.ros.org/en/rolling/p/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4N6rclcpp4Node17declare_parameterERKNSt6stringERKN6rclcpp14ParameterValueERKN14rcl_interfaces3msg19ParameterDescriptorEb) 或 [`declare_parameters`](http://docs.ros.org/en/rolling/p/rclcpp/generated/classrclcpp_1_1Node.html#_CPPv4I0EN6rclcpp4Node18declare_parametersENSt6vectorI10ParameterTEERKNSt6stringERKNSt3mapINSt6stringENSt4pairI10ParameterTN14rcl_interfaces3msg19ParameterDescriptorEEEEEb) 声明参数；也可以[配置节点自动声明参数](http://docs.ros.org/en/rolling/p/rclcpp/generated/classrclcpp_1_1NodeOptions.html#_CPPv4NK6rclcpp11NodeOptions47automatically_declare_parameters_from_overridesEv)，自动声明通过命令行覆盖值传入的参数。

随后运行：

```console
$ ros2 run demo_nodes_cpp parameter_blackboard --ros-args --params-file demo_params.yaml
```

其他节点就能够获取这些参数值，例如：

```console
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
