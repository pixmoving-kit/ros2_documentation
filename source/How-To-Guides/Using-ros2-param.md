<span id="using-the-ros2-param-command-line-tool"></span>
# 使用 ros2 param 命令行工具

ROS 2 可以通过一组服务读取、设置、列出参数，并获取参数描述，详见[参数概念文档](../Concepts/Basic/About-Parameters.md)。`ros2 param` 命令行工具封装了这些服务调用，方便从命令行操作参数。

<span id="ros2-param-list"></span>
## ros2 param list

此命令列出指定节点上所有可用的参数。如果未指定节点，则列出所有可发现节点的参数。

列出指定节点的所有参数：

```console
$ ros2 param list /my_node
```

列出系统中所有节点的全部参数；网络复杂时可能需要较长时间：

```console
$ ros2 param list
```

<span id="ros2-param-get"></span>
## ros2 param get

此命令读取指定节点上某个参数的值：

```console
$ ros2 param get /my_node use_sim_time
```

<span id="ros2-param-set"></span>
## ros2 param set

此命令设置指定节点上某个参数的值。对于大多数参数，新值的类型必须与现有类型相同。

设置节点上的参数值：

```console
$ ros2 param set /my_node use_sim_time false
```

命令行传入的值使用 YAML 格式，因此可以使用任意 YAML 表达式。不过，这也意味着某些表达式的解释结果可能与预期不同。例如，假设节点 `my_node` 的参数 `my_string` 是字符串类型，以下命令就无法生效：

```console
$ ros2 param set /my_node my_string off
```

这是因为 YAML 将 `off` 解释为布尔值，而 `my_string` 是字符串类型。可以使用 YAML 的显式字符串语法解决这一问题，例如：

```console
$ ros param set /my_node my_string '!!str off'
```

另外，YAML 支持混合类型的列表，例如同时包含字符串、布尔值和整数的列表。但 ROS 2 参数不支持混合类型列表，因此包含多种类型的 YAML 列表会被解释为字符串。假设节点 `my_node` 的参数 `my_int_array` 是整数数组类型，以下命令就无法生效：

```console
$ ros param set /my_node my_int_array '[foo,off,1]'
```

如果参数是字符串类型，则可以使用以下方式：

```console
$ ros param set /my_node my_string '[foo,off,1]'
```

<span id="ros2-param-delete"></span>
## ros2 param delete

此命令删除指定节点上的某个参数。请注意，它只能删除动态参数，不能删除已声明的参数。更多信息见[参数概念文档](../Concepts/Basic/About-Parameters.md)。

```console
$ ros2 param delete /my_node my_string
```

<span id="ros2-param-describe"></span>
## ros2 param describe

此命令提供指定节点上某个参数的文字描述：

```console
$ ros2 param describe /my_node use_sim_time
```

<span id="ros2-param-dump"></span>
## ros2 param dump

此命令以 YAML 文件格式输出指定节点的所有参数。之后可以使用这些输出，以相同参数重新运行节点：

```console
$ ros2 param dump /my_node
```

<span id="ros2-param-load"></span>
## ros2 param load

此命令将 YAML 文件中的参数值加载到指定节点中，也就是在运行时重新加载通过 `ros2 param dump` 导出的值：

```console
$ ros2 param load /my_node my_node.yaml
```
