<span id="migrating-launch-files"></span> <span id="migratinglaunch"></span>
# 迁移启动文件

ROS 1 的启动文件始终采用 [XML](https://wiki.ros.org/roslaunch/XML) 格式，ROS 2 则同时支持 XML 和 YAML。ROS 2 还支持 Python 启动脚本，以提供更高的灵活性，参见 [launch 软件包](https://github.com/ros2/launch/tree/rolling/launch)。不过，对于典型使用场景，应优先选择 XML 或 YAML。

本指南介绍如何编写 ROS 2 XML 启动文件，以便从 ROS 1 迁移。

<span id="background"></span>
## 背景

ROS 2 启动系统的说明见[启动系统教程](../../Tutorials/Intermediate/Launch/Launch-system.md)。

<span id="migrating-tags"></span>
## 迁移标签

<span id="launch"></span>
### launch

[ROS 1 中也有此标签](https://wiki.ros.org/roslaunch/XML/launch)。`launch` 是所有 ROS 2 XML 启动文件的根元素。

<span id="node"></span>
### node

[ROS 1 中也有此标签](https://wiki.ros.org/roslaunch/XML/node)，用于启动一个新节点。与 ROS 1 的区别：

- `type` 属性改为 `exec`。
- `ns` 属性改为 `namespace`。
- `required="true"` 改为 `on_exit="shutdown"`。
- 不支持 `machine`、`respawn_delay` 和 `clear_params` 属性。

<span id="example"></span>
#### 示例

```xml
<launch>
   <node pkg="demo_nodes_cpp" exec="talker"/>
   <node pkg="demo_nodes_cpp" exec="listener"/>
</launch>
```

<span id="param"></span>
### param

[ROS 1 中也有此标签](https://wiki.ros.org/roslaunch/XML/param)，用于向节点传递参数。ROS 2 没有全局参数概念，因此 `param` 只能嵌套在 `node` 标签内。ROS 2 不支持 `type`、`textfile`、`binfile` 和 `executable` 属性。原 `command` 属性改为 `value="$(command '...' )"`。

<span id="id1"></span>
#### 示例

```xml
<launch>
   <node pkg="demo_nodes_cpp" exec="parameter_event">
      <param name="foo" value="5"/>
   </node>
</launch>
```

<span id="type-inference-rules"></span>
#### 类型推断规则

以下示例展示了参数的不同写法：

```xml
<node pkg="my_package" exec="my_executable" name="my_node">
   <!--A string parameter with value "1"-->
   <param name="a_string" value="'1'"/>
   <!--A integer parameter with value 1-->
   <param name="an_int" value="1"/>
   <!--A float parameter with value 1.0-->
   <param name="a_float" value="1.0"/>
   <!--A string parameter with value "asd"-->
   <param name="another_string" value="asd"/>
   <!--Another string parameter, with value "asd"-->
   <param name="string_with_same_value_as_above" value="'asd'"/>
   <!--Another string parameter, with value "'asd'"-->
   <param name="quoted_string" value="\'asd\'"/>
   <!--A list of strings, with value ["asd", "bsd", "csd"]-->
   <param name="list_of_strings" value="asd, bsd, csd" value-sep=", "/>
   <!--A list of ints, with value [1, 2, 3]-->
   <param name="list_of_ints" value="1,2,3" value-sep=","/>
   <!--Another list of strings, with value ["1", "2", "3"]-->
   <param name="another_list_of_strings" value="'1';'2';'3'" value-sep=";"/>
   <!--A list of strings using an strange separator, with value ["1", "2", "3"]-->
   <param name="strange_separator" value="'1'//'2'//'3'" value-sep="//"/>
</node>
```

<span id="parameter-grouping"></span>
#### 参数分组

ROS 2 允许嵌套 `param` 标签，例如：

```xml
<node pkg="my_package" exec="my_executable" name="my_node" namespace="/an_absoulute_ns">
   <param name="group1">
      <param name="group2">
         <param name="my_param" value="1"/>
      </param>
      <param name="another_param" value="2"/>
   </param>
</node>
```

这会为节点 `/an_absolute_ns/my_node` 创建两个参数：`group1.group2.my_param`，值为 `1`；`group1.another_param`，值为 `2`。

也可以使用完整的参数名称：

```xml
<node pkg="my_package" exec="my_executable" name="my_node" namespace="/an_absoulute_ns">
   <param name="group1.group2.my_param" value="1"/>
   <param name="group1.another_param" value="2"/>
</node>
```

<span id="rosparam"></span>
### rosparam

[ROS 1 中的 rosparam](https://wiki.ros.org/roslaunch/XML/rosparam) 用于从 YAML 文件加载参数。ROS 2 使用 `param` 标签的 `from` 属性替代它。

<span id="id2"></span>
#### 示例

```xml
<node pkg="my_package" exec="my_executable" name="my_node" namespace="/an_absoulute_ns">
   <param from="/path/to/file"/>
</node>
```

<span id="remap"></span>
### remap

[ROS 1 中也有此标签](https://wiki.ros.org/roslaunch/XML/remap)，用于向节点传递重映射规则，只能在 `node` 标签内使用。

<span id="id3"></span>
#### 示例

```xml
<launch>
   <node pkg="demo_nodes_cpp" exec="talker">
      <remap from="chatter" to="my_topic"/>
   </node>
   <node pkg="demo_nodes_cpp" exec="listener">
      <remap from="chatter" to="my_topic"/>
   </node>
</launch>
```

<span id="include"></span>
### include

[ROS 1 中也有此标签](https://wiki.ros.org/roslaunch/XML/include)，用于包含另一个启动文件。与 ROS 1 的区别：

- ROS 1 中被包含的内容有独立作用域，ROS 2 中则没有。因此，`arg` 标签的值会传播到被包含的文件中，类似 ROS 1 中设置了 `pass_all_args="true"`。但只有在被包含文件中带有默认值的参数才会这样传播；必填参数仍须显式传递。可以将 `include` 嵌套在 `group` 中来限定作用域，另见 `group` 的 `scoped` 和 `forwarding` 属性。
- 不支持 `ns` 属性，可以使用 `push_ros_namespace`，参见后文示例。
- 嵌套在 `include` 中的 `arg` 标签改为 `let`，不过目前仍支持 `arg`。
- 嵌套在 `include` 中的 `let` 不支持条件属性 `if`、`unless`，也不支持 `description`。
- 不支持嵌套 `env`，可以改用 `set_env` 和 `unset_env`。
- 不支持 `clear_params` 和 `pass_all_args` 属性。ROS 2 launch 的行为类似于将 `pass_all_args` 设为 true，具体限制见上文。

<span id="examples"></span>
#### 示例

见[替换 include 标签](#replacing-an-include-tag)。

<span id="arg"></span>
### arg

[ROS 1 中也有此标签](https://wiki.ros.org/roslaunch/XML/arg)。`arg` 用于声明启动参数，或在使用 `include` 时传递参数。与 ROS 1 的区别：

- 不允许使用 `value` 属性，应改用 `let` 标签。
- `doc` 改为 `description`。
- 嵌套在 `include` 中时，应使用 `let` 替代 `arg`，并且不允许使用 `if`、`unless`、`description` 属性。

<span id="id4"></span>
#### 示例

```xml
<launch>
   <arg name="topic_name" default="chatter"/>
   <node pkg="demo_nodes_cpp" exec="talker">
      <remap from="chatter" to="$(var topic_name)"/>
   </node>
   <node pkg="demo_nodes_cpp" exec="listener">
      <remap from="chatter" to="$(var topic_name)"/>
   </node>
</launch>
```

<span id="passing-an-argument-to-the-launch-file"></span>
#### 向启动文件传递参数

上面的 XML 文件中，`topic_name` 默认为 `chatter`，但可以从命令行配置。假设文件名为 `mylaunch.xml`，可以通过以下命令使用其他话题名称：

```console
$ ros2 launch mylaunch.xml topic_name:=custom_topic_name
```

传递命令行参数的更多信息见[使用替换表达式](../../Tutorials/Intermediate/Launch/Using-Substitutions.md)。

<span id="env"></span>
### env

[ROS 1 中的 env](https://wiki.ros.org/roslaunch/XML/env) 用于设置环境变量。ROS 2 将其替换为 `env`、`set_env` 和 `unset_env`：

- `env` 只能嵌套在 `node` 或 `executable` 中，不支持 `if` 和 `unless`。
- `set_env` 可以嵌套在根标签 `launch` 或 `group` 中，接受与 `env` 相同的属性，并支持 `if` 和 `unless`。
- `unset_env` 用于取消设置环境变量，接受 `name` 属性和条件属性。

<span id="id5"></span>
#### 示例

```xml
<launch>
   <set_env name="MY_ENV_VAR" value="MY_VALUE" if="CONDITION_A"/>
   <set_env name="ANOTHER_ENV_VAR" value="ANOTHER_VALUE" unless="CONDITION_B"/>
   <set_env name="SOME_ENV_VAR" value="SOME_VALUE"/>
   <node pkg="MY_PACKAGE" exec="MY_EXECUTABLE" name="MY_NODE">
      <env name="NODE_ENV_VAR" value="SOME_VALUE"/>
   </node>
   <unset_env name="MY_ENV_VAR" if="CONDITION_A"/>
   <node pkg="ANOTHER_PACKAGE" exec="ANOTHER_EXECUTABLE" name="ANOTHER_NODE"/>
   <unset_env name="ANOTHER_ENV_VAR" unless="CONDITION_B"/>
   <unset_env name="SOME_ENV_VAR"/>
</launch>
```

<span id="group"></span>
### group

[ROS 1 中也有此标签](https://wiki.ros.org/roslaunch/XML/group)，用于限制启动配置的作用域，通常与 `let`、`include` 和 `push_ros_namespace` 一起使用。与 ROS 1 的区别：

- 不支持 `ns` 属性，可以改用新增的 `push_ros_namespace`。
- 不支持 `clear_params` 属性。
- 不接受 `remap` 或 `param` 作为子标签。
- 新增 `scoped` 和 `forwarding` 属性，默认均为 true。`scoped` 为 false 时，分组不会创建新的变量作用域，因此组内的变量操作也会影响组外。`forwarding` 为 false 时，组外的启动配置（`arg`）在组内不可用，这可以隔离被包含的启动文件，避免参数名冲突。

<span id="launch-prefix-example"></span> <span id="id6"></span>
#### 示例

`launch-prefix` 配置同时影响 `executable` 和 `node` 标签对应的动作。以下示例在 `use_time_prefix_in_talker` 为 `1` 时，仅为 talker 添加 `time` 前缀：

```xml
<launch>
   <arg name="use_time_prefix_in_talker" default="0"/>
   <group>
      <let name="launch-prefix" value="time" if="$(var use_time_prefix_in_talker)"/>
      <node pkg="demo_nodes_cpp" exec="talker"/>
   </group>
   <node pkg="demo_nodes_cpp" exec="listener"/>
</launch>
```

<span id="machine"></span>
### machine

目前不支持。

<span id="test"></span>
### test

目前不支持。

<span id="new-tags-in-ros-2"></span>
## ROS 2 新增的标签

<span id="set-env-and-unset-env"></span>
### set_env 与 unset_env

见 [env 标签说明](#env)。

<span id="push-ros-namespace"></span>
### push_ros_namespace

`include` 和 `group` 不接受 `ns` 属性，可以使用以下动作替代：

```xml
<!-Other tags-->
<group>
   <push_ros_namespace namespace="my_ns"/>
   <!--Nodes here are namespaced with "my_ns".-->
   <!--If there is an include action here, its nodes will also be namespaced.-->
   <push_ros_namespace namespace="another_ns"/>
   <!--Nodes here are namespaced with "another_ns/my_ns".-->
   <push_ros_namespace namespace="/absolute_ns"/>
   <!--Nodes here are namespaced with "/absolute_ns".-->
   <!--The following node receives an absolute namespace, so it will ignore the others previously pushed.-->
   <!--The full path of the node will be /asd/my_node.-->
   <node pkg="my_pkg" exec="my_executable" name="my_node" namespace="/asd"/>
</group>
<!--Nodes outside the group action won't be namespaced.-->
<!-Other tags-->
```

<span id="let"></span>
### let

它替代了带 `value` 属性的 `arg` 标签：

```xml
<let name="foo" value="asd"/>
```

ROS 2 中，`let` 和 `arg` 的用途不同：

- `let` 设置一个启动配置值。
- `arg` 声明一个启动参数或配置，并可以提供默认值。值可以通过命令行或包含该启动文件时单独设置。如果没有设置值，则使用已提供的默认值；没有默认值时会报错。

<span id="executable"></span>
### executable

用于运行任意可执行程序。

<span id="id7"></span>
#### 示例

```xml
<executable cmd="ls -las" cwd="/var/log" name="my_exec" launch-prefix="something" output="screen" shell="true">
   <env name="LD_LIBRARY" value="/lib/some.so"/>
</executable>
```

<span id="replacing-an-include-tag"></span>
## 替换 include 标签

如果希望像 ROS 1 一样，在某个**命名空间**内包含启动文件，需要将 `include` 嵌套在 `group` 中：

```xml
<group>
   <include file="another_launch_file"/>
</group>
```

然后添加 `push_ros_namespace` 动作来指定命名空间，替代 `ns` 属性：

```xml
<group>
   <push_ros_namespace namespace="my_ns"/>
   <include file="another_launch_file"/>
</group>
```

只有需要指定命名空间时，才必须将 `include` 嵌套在 `group` 中。

<span id="substitutions"></span>
## 替换表达式

ROS 1 替换表达式的文档见 [roslaunch XML wiki](https://wiki.ros.org/roslaunch/XML)。基本语法没有变化，仍采用 `$(substitution-name arg1 arg2 ...)` 形式，但有以下区别：

- `env` 和 `optenv` 统一改为 `env`。环境变量不存在时，`$(env <NAME>)` 会失败。`$(env <NAME> '')` 等价于 ROS 1 的 `$(optenv <NAME>)`。`$(env <NAME> <DEFAULT>)` 等价于 ROS 1 的 `$(env <NAME> <DEFAULT>)` 或 `$(optenv <NAME> <DEFAULT>)`。
- `find` 改为 `find-pkg-share`，返回已安装软件包的 share 目录。另一个选择是 `find-pkg-prefix`，返回已安装软件包的根目录。
- 新增 `exec-in-pkg`，例如 `$(exec-in-pkg <exec_name> <package_name>)`。
- 新增 `find-exec`。
- `arg` 改为 `var`，读取通过 `arg` 或 `let` 标签定义的配置。
- `eval` 和 `dirname` 中的字符串值需要转义，例如 `if="$(eval '\'$(var variable)\' == \'val1\'')"`，也可以使用 `&quot;` 等 HTML 转义。
- `eval` 不再将配置（`arg`）作为 Python 局部变量传入，必须通过 `$(var name)` 访问。
- ROS 2 中 `eval` 的参数必须是带引号的字符串，这也是表达式内部引号需要转义的原因。

<span id="id8"></span>
## 类型推断规则

前文 `param` 的“类型推断规则”同样适用于任意属性。例如：

```xml
<!--Setting a string value to an attribute expecting an int will raise an error.-->
<tag1 attr-expecting-an-int="'1'"/>
<!--Correct version.-->
<tag1 attr-expecting-an-int="1"/>
<!--Setting an integer in an attribute expecting a string will raise an error.-->
<tag2 attr-expecting-a-str="1"/>
<!--Correct version.-->
<tag2 attr-expecting-a-str="'1'"/>
<!--Setting a list of strings in an attribute expecting a string will raise an error.-->
<tag3 attr-expecting-a-str="asd, bsd" str-attr-sep=", "/>
<!--Correct version.-->
<tag3 attr-expecting-a-str="don't use a separator"/>
```

某些属性接受多种类型，例如 `param` 的 `value` 属性。通常，类型为 `int` 或 `float` 的参数也接受 `str`，动作稍后会执行替换，并尝试将结果转换为 `int` 或 `float`。
