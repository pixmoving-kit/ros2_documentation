---
translation_status: machine_translated
source: How-To-Guides/Migrating-from-ROS1/Migrating-Launch-Files.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="migrating-launch-files"></span> <span id="migratinglaunch"></span>

# 迁移启动文件

虽然ROS 1中的发射文件总是使用 [XML 数据](https://wiki.ros.org/roslaunch/XML) 文件, ROS 2 既支持 XML 文件,也支持 YAML 文件. ROS 2 也支持 Python 启动脚本,以便实现更大的灵活性(参见 [发射软件包](https://github.com/ros2/launch/tree/rolling/launch)然而,对于典型的使用案例,XML和YAML应该比Python优先.

本指南描述了如何编写ROS 2 XML 发射文件,以便于从ROS 1中迁移.

<span id="background"></span>

## 背景

有关ROS 2发射系统的描述可见于 [发射系统教程](../../Tutorials/Intermediate/Launch/Launch-system.md).

<span id="migrating-tags"></span>

## 移动标记

<span id="launch"></span>

### 发射

- [原文为ROS 1。](https://wiki.ros.org/roslaunch/XML/launch).

- `launch` 是任意 ROS 2 发射 XML 文件的根元素。

<span id="node"></span>

### 节点

- [原文为ROS 1。](https://wiki.ros.org/roslaunch/XML/node).

- 启动一个新的节点。

- 与ROS 1的区别:

  > - `type` 属性是现在 `exec`.
  >
  > - `ns` 属性是现在 `namespace`.
  >
  > - `required="true"` 现在 `on_exit="shutdown"`.
  >
  > - 以下属性不可用 : `machine`, `respawn_delay`, `clear_params`.

<span id="example"></span>

#### 示例

``` xml
<launch>
   <node pkg="demo_nodes_cpp" exec="talker"/>
   <node pkg="demo_nodes_cpp" exec="listener"/>
</launch>
```

<span id="param"></span>

### 参数

- [原文为ROS 1。](https://wiki.ros.org/roslaunch/XML/param).

- 用于将参数传递到节点.

- ROS 2. 没有任何全球参数概念,因此,它只能用在巢穴中 `node` 标签。一些属性在 ROS 2: 中不支持 。 `type`, `textfile`, `binfile`, `executable`.

- 那个... `command` 属性是现在 `value="$(command '...' )"`.

<span id="id1"></span>

#### 示例

``` xml
<launch>
   <node pkg="demo_nodes_cpp" exec="parameter_event">
      <param name="foo" value="5"/>
   </node>
</launch>
```

<span id="type-inference-rules"></span>

#### 推论规则类型

以下是一些如何写入参数的例子:

``` xml
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

#### 参数组

在ROS 2中, `param` 标记被允许嵌入。例如:

``` xml
<node pkg="my_package" exec="my_executable" name="my_node" namespace="/an_absoulute_ns">
   <param name="group1">
      <param name="group2">
         <param name="my_param" value="1"/>
      </param>
      <param name="another_param" value="2"/>
   </param>
</node>
```

这将产生两个参数:

- A `group1.group2.my_param` 价值 `1`,由节点托管 `/an_absolute_ns/my_node`.

- A `group1.another_param` 价值 `2` 由节点托管 `/an_absolute_ns/my_node`.

也有可能使用完整参数名称:

``` xml
<node pkg="my_package" exec="my_executable" name="my_node" namespace="/an_absoulute_ns">
   <param name="group1.group2.my_param" value="1"/>
   <param name="group1.another_param" value="2"/>
</node>
```

<span id="rosparam"></span>

### 罗什帕拉姆

- [原文为ROS 1。](https://wiki.ros.org/roslaunch/XML/rosparam).

- 从 Yaml 文件装入参数 。

- 现改为: `from` 属性在 `param` 标记。

<span id="id2"></span>

#### 示例

``` xml
<node pkg="my_package" exec="my_executable" name="my_node" namespace="/an_absoulute_ns">
   <param from="/path/to/file"/>
</node>
```

<span id="remap"></span>

### 重新绘制地图

- [原文为ROS 1。](https://wiki.ros.org/roslaunch/XML/remap).

- 曾经将重映规则传到节点.

- 它只能在内部使用 `node` 标记。

<span id="id3"></span>

#### 示例

``` xml
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

### 包含

- [原文为ROS 1。](https://wiki.ros.org/roslaunch/XML/include).

- 允许包含另一个发射文件 。

- 与ROS 1的区别:

  > - ROS 1 中包含的内容被限定范围。在ROS 2 中则没有。这意味着 `arg` 标记被传播到包含的发射文件中,好像 `pass_all_args="true"` 但是,这种传播只用于具有默认值的参数(在内部/包含的发射文件中)。 `group` 标记以显示范围(另见 `group` 属性 `scoped` 财务报告和财务报告 `forwarding` ).
  >
  > - `ns` 属性不支持。请参见 `push_ros_namespace` 标记一个工作四周。
  >
  > - `arg` 标记嵌入一个 `include` 标记是现在 `let`然而, `arg` 目前仍然得到支持。
  >
  > - `let` 标记嵌入一个 `include` 标签不支持条件( Q)`if`, `unless`)或为: `description` 属性。
  >
  > - 没有人支持筑巢 `env` 标记。 `set_env` 财务报告和财务报告 `unset_env` 可改为使用。
  >
  > - 两者 `clear_params` 财务报告和财务报告 `pass_all_args` 属性不支持。 ROS 2 发射表现为 `pass_all_args` 被确定为真实的(见上文)。

<span id="examples"></span>

#### 实例

见 [替换包含标记](#replacing-an-include-tag).

<span id="arg"></span>

### 参数

- [原文为ROS 1。](https://wiki.ros.org/roslaunch/XML/arg).

- `arg` 用于宣布发射理由,或用于在使用 `include` 标记。

- 与ROS 1的区别:

  > - `value` 属性不允许。使用 `let` 标记此选项。
  >
  > - `doc` 现在 `description`.
  >
  > - 当筑巢于一个 `include` 标签 :
  >
  >   > - 使用 `let` 改为 `arg`.
  >   >
  >   > - `if`, `unless`,以及 `description` 属性不被允许。

<span id="id4"></span>

#### 示例

``` xml
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

#### 向发射文件传递参数

在上面的 XML 发射文件中, `topic_name` 默认名称 `chatter`,但可以在命令行上配置。假设上面的发射配置在一个名为文件的文件中 `mylaunch.xml`,可以使用不同的主题名称,其启动方式如下:

``` console
$ ros2 launch mylaunch.xml topic_name:=custom_topic_name
```

有关通过命令行参数的更多信息,请访问 [使用替换](../../Tutorials/Intermediate/Launch/Using-Substitutions.md).

<span id="env"></span>

### 内置

- [原文为ROS 1。](https://wiki.ros.org/roslaunch/XML/env).

- 设置环境变量。

- 现改为: `env`, `set_env` 财务报告和财务报告 `unset_env`:

  > - `env` 只能用在一个巢穴中 `node` 或 时 间 `executable` 标记 。 `if` 财务报告和财务报告 `unless` 标签不支持 。
  >
  > - `set_env` 可以在根标记内嵌入 `launch` 或以内 `group` 标记。它接受的属性与 `env`,以及 `if` 财务报告和财务报告 `unless` 标记。
  >
  > - `unset_env` 取消设置环境变量。它接受一个 `name` 属性和条件。

<span id="id5"></span>

#### 示例

``` xml
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

### 组

- [原文为ROS 1。](https://wiki.ros.org/roslaunch/XML/group).

- 允许限制发射配置的范围。通常与 `let`, `include` 财务报告和财务报告 `push_ros_namespace` 标记。

- 与ROS 1的区别:

  > - 没有 `ns` 属性。见新 `push_ros_namespace` 标记为工作。
  >
  > - `clear_params` 属性不可用 。
  >
  > - 它不接受 `remap` 也无 `param` 作为孩子的标签。
  >
  > - 它有两个新的属性: `scoped` 财务报告和财务报告 `forwarding` (两者在默认情况下都是真实的)。如果 `scoped` 错误,该组不引入新的变量范围,因此对组内变量采取的行动也会影响外部变量。如果 `forwarding` 假的,没有外部发射配置( `arg` 用于分离包含的发射文件,从而防止在参数名称中发生碰撞。

<span id="launch-prefix-example"></span> <span id="id6"></span>

#### 示例

`launch-prefix` 配置对两者都有影响 `executable` 财务报告和财务报告 `node` 标记的动作。此示例将使用 `time` 作为前缀,如果 `use_time_prefix_in_talker` 参数是 `1`只能给说话的人

``` xml
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

### 机器

它目前没有得到支持。

<span id="test"></span>

### 测试

它目前没有得到支持。

<span id="new-tags-in-ros-2"></span>

## ROS 2 中的新标签

<span id="set-env-and-unset-env"></span>

### 设置_env 和 未设置_env

见 [内置](#env) 标签描述。

<span id="push-ros-namespace"></span>

### push_ros_namespace

`include` 财务报告和财务报告 `group` 标签不接受 `ns` 属性。此动作可用作工作环路 :

``` xml
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

### 开始

换成是 `arg` 带有值属性的标记。

``` xml
<let name="foo" value="asd"/>
```

`let` 财务报告和财务报告 `arg` 服务于ROS 2:两个不同目的:

- `let` 设置发射配置值。

- `arg` 声明一个发射参数/配置,并选择提供默认值。该值可以与 CLI 单独设定,也可以在包含指定发射文件时设定。如果没有设定值,则使用默认值,否则报告错误。

<span id="executable"></span>

### 可执行文件

它允许运行任何可执行文件 。

<span id="id7"></span>

#### 示例

``` xml
<executable cmd="ls -las" cwd="/var/log" name="my_exec" launch-prefix="something" output="screen" shell="true">
   <env name="LD_LIBRARY" value="/lib/some.so"/>
</executable>
```

<span id="replacing-an-include-tag"></span>

## 替换包含标记

为了把发射文件列入 **命名空间** 与《规则》第1条相同,然后 `include` 标记必须嵌入一个 `group` 标记 。

``` xml
<group>
   <include file="another_launch_file"/>
</group>
```

然后, 而不是使用 `ns` 属性,添加 `push_ros_namespace` 指定命名空间的动作标记 :

``` xml
<group>
   <push_ros_namespace namespace="my_ns"/>
   <include file="another_launch_file"/>
</group>
```

缠绕 `include` a 下标记 `group` 标记仅在指定命名空间时需要

<span id="substitutions"></span>

## 替代

有关ROS 1的替代文件可见于 [ros 发射 XML 维基](https://wiki.ros.org/roslaunch/XML)替代语法没有改变, `$(substitution-name arg1 arg2 ...)` 但是,有些变化是W.r.t. ROS 1:

- `env` 财务报告和财务报告 `optenv` 标记已被替换为 `env` 标记 。 `$(env <NAME>)` 如果环境变量不存在, 将会失败 。 `$(env <NAME> '')` 与ROS 1 相同 `$(optenv <NAME>)`. `$(env <NAME> <DEFAULT>)` 与ROS 1 相同 `$(env <NAME> <DEFAULT>)` 或 时 间 `$(optenv <NAME> <DEFAULT>)`.

- `find` 已替换为 `find-pkg-share` (替换已安装软件包的共享目录)。或者 `find-pkg-prefix` 将返回已安装软件包的根。

- 有一个新的 `exec-in-pkg` 替换,例如: `$(exec-in-pkg <exec_name> <package_name>)`.

- 有一个新的 `find-exec` 替换。

- `arg` 已替换为 `var`。它查看定义的配置 `arg` 或 时 间 `let` 标记 。

- `eval` 财务报告和财务报告 `dirname` 替换需要字符串值的逃逸字符,例如. `if="$(eval '\'$(var variable)\' == \'val1\'')"`。您也可以使用 HTML 类的逃逸 `&quot;` .

- `eval` 不通过配置( N) `arg` )作为本地 Python 变量。它们必须通过 `$(var name)`.

- 论点是: `eval` 必须在ROS 2. 中引用字符串,这也是为什么必须逃避表达式中的引用的原因.

<span id="id8"></span>

## 推论规则类型

显示的规则 `Type inference rules` 分节 `param` 标记适用于任意属性。例如:

``` xml
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

有些属性接受不止一个类型, 例如 `value` 属性 `param` 标签。常见的参数是类型 `int` (或 减) `float`) 并接受 `str`中,后将替换,并试图转换为 `int` (或 减) `float`由行动进行。
