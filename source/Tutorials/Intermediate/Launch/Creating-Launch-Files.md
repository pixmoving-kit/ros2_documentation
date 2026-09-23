---
translation_status: machine_translated
source: Tutorials/Intermediate/Launch/Creating-Launch-Files.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="creating-a-launch-file"></span>

# 创建启动文件

**目标：** 创建一个启动文件来运行一个复杂的ROS 2系统.

**教程级别：** 中级

**用时：** 10分钟

<span id="prerequisites"></span>

## 前提条件

此教程使用 [rqt_图解和图解sim](../../Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md) 软件包。

您还需要使用您喜欢的文本编辑器 。

与往常一样, [您打开的每个新终端](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md).

<span id="background"></span>

## 背景

ROS 2中的发射系统负责帮助用户描述其系统的配置,然后按描述执行. 系统配置包括运行什么程序,运行在哪里,传递什么参数,以及ROS特定的常规,它们通过给每个组件一个不同的配置,使得整个系统更容易再利用组件. 它还负责监控所启动的流程的状况,报告并/或对这些流程状态的变化作出反应.

用 XML 、 YAML 或 Python 编写的启动文件可以启动和停止不同的节点, 也可以触发和对各种事件采取行动 。 见 [使用 XML、YAML 和 Python 编写 ROS 2 启动文件](../../../How-To-Guides/Launch-file-different-formats.md) 用于描述不同格式。提供此框架的软件包是: `launch_ros`,使用非 ROS 特性 `launch` 下方的框架。

那个... [设计文件](https://design.ros2.org/articles/roslaunch.html) 详细介绍了ROS 2发射系统设计的目标(目前并非所有功能都可用).

<span id="tasks"></span>

## 操作步骤

<span id="setup"></span>

### 1 设置

创建存储您启动文件的新目录 :

``` console
$ mkdir launch
```

<span id="write-the-launch-file"></span>

### 2 写入发射文件

让我们用“ROS 2”发射文件组合起来。 `turtlesim` 软件包及其可执行文件。如上所述,可以是XML、YAML,也可以是Python。

##### XML 数据

复制并粘贴完整的代码到 `launch/turtlesim_mimic_launch.xml` 文件 :

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <node pkg="turtlesim" exec="turtlesim_node" name="sim" namespace="turtlesim1" args="--ros-args --log-level info" />
  <node pkg="turtlesim" exec="turtlesim_node" name="sim" namespace="turtlesim2" ros_args="--log-level warn" />
  <node pkg="turtlesim" exec="mimic" name="mimic">
    <remap from="/input/pose" to="/turtlesim1/turtle1/pose" />
    <remap from="/output/cmd_vel" to="/turtlesim2/turtle1/cmd_vel" />
  </node>
</launch>
```

##### 也门

复制并粘贴完整的代码到 `launch/turtlesim_mimic_launch.yaml` 文件 :

``` yaml
%YAML 1.2
---
launch:
  - node:
      pkg: "turtlesim"
      exec: "turtlesim_node"
      name: "sim"
      namespace: "turtlesim1"
      args: "--ros-args --log-level info"

  - node:
      pkg: "turtlesim"
      exec: "turtlesim_node"
      name: "sim"
      namespace: "turtlesim2"
      ros_args: "--log-level warn"

  - node:
      pkg: "turtlesim"
      exec: "mimic"
      name: "mimic"
      remap:
        - from: "/input/pose"
          to: "/turtlesim1/turtle1/pose"
        - from: "/output/cmd_vel"
          to: "/turtlesim2/turtle1/cmd_vel"
```

##### Python

复制并粘贴完整的代码到 `launch/turtlesim_mimic_launch.py` 文件 :

``` python
from launch import LaunchDescription
from launch_ros.actions import Node


def generate_launch_description():
    return LaunchDescription([
        Node(
            package='turtlesim',
            namespace='turtlesim1',
            executable='turtlesim_node',
            name='sim',
            arguments=['--ros-args', '--log-level', 'info']
        ),
        Node(
            package='turtlesim',
            namespace='turtlesim2',
            executable='turtlesim_node',
            name='sim',
            ros_arguments=['--log-level', 'warn']
        ),
        Node(
            package='turtlesim',
            executable='mimic',
            name='mimic',
            remappings=[
                ('/input/pose', '/turtlesim1/turtle1/pose'),
                ('/output/cmd_vel', '/turtlesim2/turtle1/cmd_vel'),
            ]
        )
    ])
```

<span id="examine-the-launch-file"></span>

#### 2.1 检查发射档案

上面所有发射文件都发射一个由三个节点组成的系统,全部来自 `turtlesim` 包。系统的目标是启动两个龟形窗,并有一个龟形模仿另一个龟形的移动。

当启动两个龟语节点时,它们之间的主要区别是它们的名称空间值. 独特的名称空间允许系统启动两个节点,而无需节点名称或主题名称冲突. 此系统中的两个龟语都在同一主题上接收命令,并在同一主题上发布其姿势. 有了独特的名称空间,针对不同龟语的信息可以区分.

两个龟兹节点还演示了将参数传递到节点的不同方式. 第一个节点使用 `args` 将参数直接传递给可执行文件,要求 `--ros-args` 用于 ROS 特定参数的旗帜。第二个节点使用 `ros_args` (`ros_arguments` 在 Python 中),专门为ROS 参数设计。使用 `args` 当混合ROS和非ROS参数时(例如, `my_custom_arg --ros-args --log-level info`),或 `ros_args` 用于更清洁的语法,只有ROS参数,如重映射,参数,或日志级别.

最后的节点也是来自 `turtlesim` 软件包,但是不同的可执行文件 : `mimic`。这个节点以重映射的形式添加了配置细节。 `mimic`’s `/input/pose` 重映射为主题 `/turtlesim1/turtle1/pose` 并且它会是 `/output/cmd_vel` 专题至 `/turtlesim2/turtle1/cmd_vel`。这意味着 `mimic` 将订阅 `/turtlesim1/sim`摆出话题并重刊 `/turtlesim2/sim`要订阅的速度命令主题。换句话说, `turtlesim2` 将模仿 `turtlesim1`动静。

##### XML 数据

前两个动作启动了两个龟兹窗口, 并有不同的参数通过方法:

``` xml
  <node pkg="turtlesim" exec="turtlesim_node" name="sim" namespace="turtlesim1" args="--ros-args --log-level info" />
  <node pkg="turtlesim" exec="turtlesim_node" name="sim" namespace="turtlesim2" ros_args="--log-level warn" />
```

最终行动发射模仿的节点 与复刻图:

``` xml
  <node pkg="turtlesim" exec="mimic" name="mimic">
    <remap from="/input/pose" to="/turtlesim1/turtle1/pose" />
    <remap from="/output/cmd_vel" to="/turtlesim2/turtle1/cmd_vel" />
  </node>
```

##### 也门

前两个动作启动了两个龟兹窗口, 并有不同的参数通过方法:

``` yaml
  - node:
      pkg: "turtlesim"
      exec: "turtlesim_node"
      name: "sim"
      namespace: "turtlesim1"
      args: "--ros-args --log-level info"

  - node:
      pkg: "turtlesim"
      exec: "turtlesim_node"
      name: "sim"
      namespace: "turtlesim2"
      ros_args: "--log-level warn"
```

最终行动发射模仿的节点 与复刻图:

``` yaml
  - node:
      pkg: "turtlesim"
      exec: "mimic"
      name: "mimic"
      remap:
        - from: "/input/pose"
          to: "/turtlesim1/turtle1/pose"
        - from: "/output/cmd_vel"
          to: "/turtlesim2/turtle1/cmd_vel"
```

##### Python

这些导入语句拉入一些 Python `launch` 模块。

``` python
from launch import LaunchDescription
from launch_ros.actions import Node
```

接下来,发射描述本身开始:

``` python
def generate_launch_description():
    return LaunchDescription([
    ])
```

发射描述的前两个动作 发射两个龟兹窗口 不同的论点通过方法:

``` python
        Node(
            package='turtlesim',
            namespace='turtlesim1',
            executable='turtlesim_node',
            name='sim',
            arguments=['--ros-args', '--log-level', 'info']
        ),
        Node(
            package='turtlesim',
            namespace='turtlesim2',
            executable='turtlesim_node',
            name='sim',
            ros_arguments=['--log-level', 'warn']
        ),
```

最终行动发射模仿的节点 与复刻图:

``` python
        Node(
            package='turtlesim',
            executable='mimic',
            name='mimic',
            remappings=[
                ('/input/pose', '/turtlesim1/turtle1/pose'),
                ('/output/cmd_vel', '/turtlesim2/turtle1/cmd_vel'),
            ]
        )
```

<span id="ros2-launch"></span>

### 3架罗斯2型发射机

要运行上面创建的发射文件,请输入您早先创建的目录,并运行以下命令:

##### XML 数据

``` console
$ cd launch
$ ros2 launch turtlesim_mimic_launch.xml
```

##### 也门

``` console
$ cd launch
$ ros2 launch turtlesim_mimic_launch.yaml
```

##### Python

``` console
$ cd launch
$ ros2 launch turtlesim_mimic_launch.py
```

> **说明**
>
> 可以直接发射一个发射文件(如我们以上所做的那样),或由包提供。如果由包提供,语法是:
>
> ``` console
> $ ros2 launch <package_name> <launch_file_name>
> ```
>
> 您学到了创建软件包 [创建软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md).

> **说明**
>
> 对于带有发射文件的软件包,一个好主意是添加一个 `exec_depend` 依赖 `ros2launch` 软件包中的软件包 `package.xml`:
>
> ``` xml
> <exec_depend>ros2launch</exec_depend>
> ```
>
> 这有助于确保 `ros2 launch` 命令在构建您的软件包后可以使用。它也确保了所有 [启动文件格式](../../../How-To-Guides/Launch-file-different-formats.md) 被确认。

打开两扇乌龟窗,你会看到以下 `[INFO]` 通知您启动文件时的节点:

``` console
[INFO] [launch]: Default logging verbosity is set to INFO
[INFO] [turtlesim_node-1]: process started with pid [11714]
[INFO] [turtlesim_node-2]: process started with pid [11715]
[INFO] [mimic-3]: process started with pid [11716]
```

要看到系统在运行,打开一个新的终端并运行 `ros2 topic pub` 命令在 `/turtlesim1/turtle1/cmd_vel` 要让第一个乌龟移动的话题 :

``` console
$ ros2 topic pub -r 1 /turtlesim1/turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: -1.8}}"
```

你会看到两只海龟都走同一条路

![](images/mimic.png) <span id="introspect-the-system-with-rqt-graph"></span>

### 4 用 rqt_graph 对系统进行回顾

当系统还在运行时, 打开一个新的终端并运行 `rqt_graph` 以更好地了解您发射文件中的节点之间的关系。

运行命令 :

``` console
$ ros2 run rqt_graph rqt_graph
```

![](images/mimic_graph.png)

隐藏节点( e) `ros2 topic pub` 命令(您运行)正在将数据发布到 `/turtlesim1/turtle1/cmd_vel` 左侧的主题, `/turtlesim1/sim` 节点被订阅。 其余图表显示了前面描述的内容 : `mimic` 已订阅 `/turtlesim1/sim`将话题摆出,并出版给 `/turtlesim2/sim`速度命令主题 。

<span id="summary"></span>

## 小结

启动文件简化了运行中的复杂系统, 并附有许多节点和特定配置细节。 您可以使用 XML、 YAML 或 Python 创建启动文件, 并使用 `ros2 launch` 命令。 命令。
