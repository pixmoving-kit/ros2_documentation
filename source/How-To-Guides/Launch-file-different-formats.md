---
translation_status: machine_translated
source: How-To-Guides/Launch-file-different-formats.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-xml-yaml-and-python-for-ros-2-launch-files"></span>

# 使用 XML、YAML 和 Python 编写 ROS 2 启动文件

ROS 2 发射文件可以用 XML, YAML, 和 Python 写成。 本指南显示如何使用这些不同格式来完成相同的任务, 以及讨论何时使用每个格式 。

<span id="launch-file-examples"></span>

## 启动文件示例

下面是一个在XML,YAML,和Python中执行的发射文件. 每个发射文件都执行以下动作:

- 设置带有默认的命令行参数

- 包含另一个发射文件

- 在另一个命名空间中包含另一个发射文件

- 启动节点并设置其命名空间

- 启动节点, 设置命名空间, 并在节点中设置参数( 使用参数)

- 创建一个将信件从一个主题重映射到另一个主题的节点

##### XML 数据

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <!-- args that can be set from the command line or a default will be used -->
  <arg name="background_r" default="0" />
  <arg name="background_g" default="255" />
  <arg name="background_b" default="0" />
  <arg name="chatter_ns" default="my/chatter/ns" />

  <!-- include another launch file -->
  <include file="$(find-pkg-share demo_nodes_cpp)/launch/topics/talker_listener.launch.py" />

  <!-- include another launch file in the chatter_ns namespace-->
  <group>
    <!-- push_ros_namespace to set namespace of included nodes -->
    <push_ros_namespace namespace="$(var chatter_ns)" />
    <include file="$(find-pkg-share demo_nodes_cpp)/launch/topics/talker_listener.launch.py" />
  </group>

  <!-- start a turtlesim_node in the turtlesim1 namespace and use args to set the log level -->
  <node pkg="turtlesim" exec="turtlesim_node" name="sim" namespace="turtlesim1" args="--ros-args --log-level info" />

  <!-- start another turtlesim_node in the turtlesim2 namespace, use ros_args to set the log level, and child elements to set the parameters -->
  <node pkg="turtlesim" exec="turtlesim_node" name="sim" namespace="turtlesim2" ros_args="--log-level warn">
    <param name="background_r" value="$(var background_r)" />
    <param name="background_g" value="$(var background_g)" />
    <param name="background_b" value="$(var background_b)" />
  </node>

  <!-- perform remap so both turtles listen to the same command topic -->
  <node pkg="turtlesim" exec="mimic" name="mimic">
    <remap from="/input/pose" to="/turtlesim1/turtle1/pose" />
    <remap from="/output/cmd_vel" to="/turtlesim2/turtle1/cmd_vel" />
  </node>
</launch>
```

##### 也门

``` yaml
%YAML 1.2
---
launch:
# args that can be set from the command line or a default will be used
- arg:
    name: "background_r"
    default: "0"
- arg:
    name: "background_g"
    default: "255"
- arg:
    name: "background_b"
    default: "0"
- arg:
    name: "chatter_ns"
    default: "my/chatter/ns"

# include another launch file
- include:
    file: "$(find-pkg-share demo_nodes_cpp)/launch/topics/talker_listener.launch.py"

# include another launch file in the chatter_ns namespace
- group:
    - push_ros_namespace:
        namespace: "$(var chatter_ns)"
    - include:
        file: "$(find-pkg-share demo_nodes_cpp)/launch/topics/talker_listener.launch.py"

# start a turtlesim_node in the turtlesim1 namespace and use args to set the log level
- node:
    pkg: "turtlesim"
    exec: "turtlesim_node"
    name: "sim"
    namespace: "turtlesim1"
    args: "--ros-args --log-level info"

# start another turtlesim_node in the turtlesim2 namespace, use ros_args to set the log level, and param to set the parameters
- node:
    pkg: "turtlesim"
    exec: "turtlesim_node"
    name: "sim"
    namespace: "turtlesim2"
    ros_args: "--log-level warn"
    param:
    - name: "background_r"
      value: "$(var background_r)"
    - name: "background_g"
      value: "$(var background_g)"
    - name: "background_b"
      value: "$(var background_b)"

# perform remap so both turtles listen to the same command topic
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

``` python
from launch import LaunchDescription
from launch.actions import DeclareLaunchArgument, GroupAction, IncludeLaunchDescription
from launch.substitutions import LaunchConfiguration, PathJoinSubstitution
from launch_ros.actions import Node, PushRosNamespace
from launch_ros.substitutions import FindPackageShare


def generate_launch_description():
    launch_dir = PathJoinSubstitution([FindPackageShare('demo_nodes_cpp'), 'launch', 'topics'])
    return LaunchDescription([
        # args that can be set from the command line or a default will be used
        DeclareLaunchArgument('background_r', default_value='0'),
        DeclareLaunchArgument('background_g', default_value='255'),
        DeclareLaunchArgument('background_b', default_value='0'),
        DeclareLaunchArgument('chatter_ns', default_value='my/chatter/ns'),

        # include another launch file
        IncludeLaunchDescription(
            PathJoinSubstitution([launch_dir, 'talker_listener.launch.py'])
        ),

        # include a Python launch file in the chatter_py_ns namespace
        GroupAction(
            actions=[
                # push_ros_namespace first to set namespace of included nodes for following actions
                PushRosNamespace(LaunchConfiguration('chatter_ns')),
                IncludeLaunchDescription(
                    PathJoinSubstitution([launch_dir, 'talker_listener.launch.py'])),
            ]
        ),

        # include a xml launch file in the chatter_xml_ns namespace
        GroupAction(
            actions=[
                # push_ros_namespace first to set namespace of included nodes for following actions
                PushRosNamespace('chatter_xml_ns'),
                IncludeLaunchDescription(
                    PathJoinSubstitution([launch_dir, 'talker_listener.launch.xml'])),
            ]
        ),

        # include a yaml launch file in the chatter_yaml_ns namespace
        GroupAction(
            actions=[
                # push_ros_namespace first to set namespace of included nodes for following actions
                PushRosNamespace('chatter_yaml_ns'),
                IncludeLaunchDescription(
                    PathJoinSubstitution([launch_dir, 'talker_listener.launch.yaml'])),
            ]
        ),

        # start a turtlesim_node in the turtlesim1 namespace and use arguments to set the log level
        Node(
            package='turtlesim',
            namespace='turtlesim1',
            executable='turtlesim_node',
            name='sim',
            arguments=['--ros-args', '--log-level', 'info']
        ),

        # start another turtlesim_node in the turtlesim2 namespace,
        # use ros_arguments to set the log level, and parameters to set the parameters
        Node(
            package='turtlesim',
            namespace='turtlesim2',
            executable='turtlesim_node',
            name='sim',
            ros_arguments=['--log-level', 'warn'],
            parameters=[{
                'background_r': LaunchConfiguration('background_r'),
                'background_g': LaunchConfiguration('background_g'),
                'background_b': LaunchConfiguration('background_b'),
            }]
        ),

        # perform remap so both turtles listen to the same command topic
        Node(
            package='turtlesim',
            executable='mimic',
            name='mimic',
            remappings=[
                ('/input/pose', '/turtlesim1/turtle1/pose'),
                ('/output/cmd_vel', '/turtlesim2/turtle1/cmd_vel'),
            ]
        ),
    ])
```

<span id="using-the-launch-files-from-the-command-line"></span>

## 使用命令行的发射文件

<span id="launching"></span>

### 发射

上面的任何发射文件都可以用 `ros2 launch`。要在本地尝试它们,可以创建新的软件包并使用

``` console
$ ros2 launch <package_name> <launch_file_name>
```

或通过指定发射文件的路径直接运行文件

``` console
$ ros2 launch <path_to_launch_file>
```

<span id="setting-arguments"></span>

### 设置参数

要设置传递到发射文件中的参数,请使用 `key:=value` 语法。例如,您可以设置 `background_r` 以下列方式:

``` console
$ ros2 launch <package_name> <launch_file_name> background_r:=255
```

或 时 间

``` console
$ ros2 launch <path_to_launch_file> background_r:=255
```

<span id="controlling-the-turtles"></span>

### 控制海龟

为了测试重映射是否有效,您可以通过在另一个终端运行以下命令来控制龟类:

``` console
$ ros2 run turtlesim turtle_teleop_key --ros-args --remap __ns:=/turtlesim1
```

<span id="xml-yaml-or-python-which-should-i-use"></span> <span id="launch-file-different-formats-which"></span>

## XML, YAML, 或 Python: 我应该用哪一种?

> **说明**
>
> ROS 1中的发射文件是用XML写的,所以XML可能是ROS 1中的人最熟悉的. . . [迁移启动文件](Migrating-from-ROS1/Migrating-Launch-Files.md).

对于大多数应用程序来说,选择哪个ROS 2发射格式会降为开发者的偏好。但是,如果您的发射文件需要灵活性,而你无法用XML或YAML实现,那么您可以使用Python来写入您的发射文件。使用Python来进行ROS 2发射会更加灵活,原因有二:

- Python是一种脚本语言,因此您可以在您的启动文件中对语言及其库进行杠杆化.

- [ros2/launch](https://github.com/ros2/launch) (一般发射特征)和 [ros2/launch_ros](https://github.com/ros2/launch_ros) (ROS 2 特定发射特征)用 Python 写成,因此您对可能不会被 XML 和 YAML 曝光的发射特征的级别较低.

尽管如此,用Python写成的发射文件可能比XML或YAML中的一个更为复杂和动词化.
