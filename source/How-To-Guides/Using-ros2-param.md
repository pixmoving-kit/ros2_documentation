---
translation_status: machine_translated
source: How-To-Guides/Using-ros2-param.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-the-ros2-param-command-line-tool"></span>

# 使用 `ros2 param` 命令行工具

ROS 2中的参数可以通过以下描述的一组服务获得、设定、列出和描述: [概念文件](../Concepts/Basic/About-Parameters.md)。该词 `ros2 param` 命令行工具是一个围绕这些服务呼叫的包装器,这使得从命令行操作参数变得容易.

<span id="ros2-param-list"></span>

## `ros2 param list`

此命令将列出指定节点上的所有可用参数, 如果给出了节点, 则列出所有可发现节点上的所有参数 。

要在给定的节点上获得所有参数:

``` console
$ ros2 param list /my_node
```

要在系统中的所有节点上获得所有参数(这需要很长的时间在一个复杂的网络上):

``` console
$ ros2 param list
```

<span id="ros2-param-get"></span>

## `ros2 param get`

此命令将在特定节点上获得特定参数的值.

要在节点上获得参数值 :

``` console
$ ros2 param get /my_node use_sim_time
```

<span id="ros2-param-set"></span>

## `ros2 param set`

此命令将在特定节点上设置特定参数的值。 对于大多数参数,新值的类型必须与现有类型相同 。

要设置节点上参数的值 :

``` console
$ ros2 param set /my_node use_sim_time false
```

命令行上传递的值在 YAML 中, 它允许使用任意的 YAML 表达式。 但是, 这也意味着某些表达式的解释会不同于预期。 例如, 如果参数 `my_string` 节点 `my_node` 是类型字符串,以下将无法工作 :

``` console
$ ros2 param set /my_node my_string off
```

这是因为YAML将“关闭”解释为布尔语, `my_string` 是字符串类型。这可以通过使用YAML语法来明确设置字符串来工作,例如:

``` console
$ ros param set /my_node my_string '!!str off'
```

此外, YAML 支持不同列表, 包含( say) 字符串、 布尔 和整数。 然而, ROS 2 参数不支持不同列表, 因此任何具有多种类型的 YAML 列表都将被解释为字符串 。 假设参数 `my_int_array` 节点 `my_node` 是类型整数阵列,以下将无法工作 :

``` console
$ ros param set /my_node my_int_array '[foo,off,1]'
```

下列字符串键入参数会有效 :

``` console
$ ros param set /my_node my_string '[foo,off,1]'
```

<span id="ros2-param-delete"></span>

## `ros2 param delete`

此命令将删除特定节点的参数。 然而, 请注意, 这只能删除动态参数( 而非已声明的参数 ) 。 See [概念文件](../Concepts/Basic/About-Parameters.md) 以获取更多信息。

``` console
$ ros2 param delete /my_node my_string
```

<span id="ros2-param-describe"></span>

## `ros2 param describe`

此命令将为特定节点上的特定参数提供文字描述:

``` console
$ ros2 param describe /my_node use_sim_time
```

<span id="ros2-param-dump"></span>

## `ros2 param dump`

此命令将以 YAML 文件格式打印出特定节点上的所有参数。 此命令的输出随后可用于以相同的参数重运行节点 :

``` console
$ ros2 param dump /my_node
```

<span id="ros2-param-load"></span>

## `ros2 param load`

此命令将从 YAML 文件将参数的值加载到特定节点。 也就是说, 此命令可以在运行时重新加载被丢弃的值 `ros2 param dump`:

``` console
$ ros2 param load /my_node my_node.yaml
```
