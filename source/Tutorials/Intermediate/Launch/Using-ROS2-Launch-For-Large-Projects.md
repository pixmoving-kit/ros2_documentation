---
translation_status: machine_translated
source: Tutorials/Intermediate/Launch/Using-ROS2-Launch-For-Large-Projects.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="managing-large-projects"></span> <span id="usingros2launchforlargeprojects"></span>

# 管理大型项目

**目标：** 学习使用ROS 2发射文件管理大型项目的最佳做法.

**教程级别：** 中级

**用时：** 20分钟

<span id="background"></span>

## 背景

此教程描述一些为大型项目编写发射文件的提示。 重点是如何构建发射文件, 以便在不同情况下尽可能多地重新使用它们。 此外, 它涵盖了不同的ROS 2 发射工具的用法实例, 如参数、 YAML 文件、 重映射、 命名空间、 默认参数和 RViz 配置 。

<span id="prerequisites"></span>

## 前提条件

此教程使用 [乌龟](../../Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md) 财务报告和财务报告 [turtle_tf2_py](../Tf2/Introduction-To-Tf2.md) 软件包。此教程还假定您有 [创建新软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md) 结构类型 `ament_python` 调用 `launch_tutorial`.

<span id="introduction"></span>

## 导言

机器人上的大型应用通常涉及几个互相连接的节点,每个节点可以有许多参数. 模拟龟模中的多只龟可以作为一个很好的例子. 龟模模拟由多个龟标节点,世界配置,以及TF播音器和收听器节点组成. 在所有节点中,有大量的ROS参数影响这些节点的行为和外观. ROS 2发射文件允许我们开始所有节点,并在一个地方设置相应的参数. 在一个教程结束时,你将构建这个参数. `launch_turtlesim_launch` 发射文件 `launch_tutorial` 。这个发射文件将提出不同的节点,负责模拟两个龟兹模拟,启动TF广播员和听众,加载参数,并启动一个 RViz 配置。在这个教程中,我们将查看这个发射文件和所使用的所有相关特性。

> **注意**
>
> 启动文件可以以 XML 、 YAML 或 Python 格式编写 。 在整个教程中, 启动文件都用所有三种格式使用制表符显示 。 您可以选择您喜欢的格式 - 它们是功能上等同的 。 任何您看到文件名的地方 `launch_turtlesim_launch` 确定您的发射文件类型(即: `launch_turtlesim_launch.py` 以蟒蛇盟誓, `launch_turtlesim_launch.xml` 用于 XML,以及 `launch_turtlesim_launch.yaml` 为YAML.

<span id="writing-launch-files"></span>

## 写入启动文件

<span id="top-level-organization"></span>

### 1个最高级别组织

写入发射文件过程中的目标之一应该是尽可能地使其可重复使用。 可以通过将相关的节点和配置组合成单独的发射文件来完成。 之后, 可以写入一个专门用于特定配置的顶级发射文件。 这样就可以在完全不改变发射文件的情况下在相同的机器人之间移动。 即使是从真正的机器人移动到模拟的机器人这样的改变, 也只能做几处修改 。

现在,我们将翻阅能够做到这一点的顶级发射文件结构。 首先,我们将创建一个发射文件,调用单独的发射文件。为此,让我们创建一个 `launch_turtlesim_launch` 文档中 `/launch` 我们的文件夹 `launch_tutorial` 软件包。

> **注意**
>
> 较早的发射系统版本可能不支持 `let` 内部 `include` 报表和要求 `arg` 相反,语法是相同的: `name` 财务报告和财务报告 `value` 属性保持不变(例如, `<arg name="target_frame" value="carrot1" />`).

##### XML 数据

复制并粘贴完整的代码到 `launch/launch_turtlesim_launch.xml` 文件 :

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <include file="$(find-pkg-share launch_tutorial)/launch/turtlesim_world_1_launch.xml" />
  <include file="$(find-pkg-share launch_tutorial)/launch/turtlesim_world_2_launch.xml" />
  <include file="$(find-pkg-share launch_tutorial)/launch/broadcaster_listener_launch.xml">
    <let name="target_frame" value="carrot1" />
  </include>
  <include file="$(find-pkg-share launch_tutorial)/launch/mimic_launch.xml" />
  <include file="$(find-pkg-share launch_tutorial)/launch/fixed_broadcaster_launch.xml" />
  <include file="$(find-pkg-share launch_tutorial)/launch/turtlesim_rviz_launch.xml" />
</launch>
```

##### 也门

复制并粘贴完整的代码到 `launch/launch_turtlesim_launch.yaml` 文件 :

``` yaml
%YAML 1.2
---
launch:
  - include:
      file: "$(find-pkg-share launch_tutorial)/launch/turtlesim_world_1_launch.yaml"
  - include:
      file: "$(find-pkg-share launch_tutorial)/launch/turtlesim_world_2_launch.yaml"
  - include:
      file: "$(find-pkg-share launch_tutorial)/launch/broadcaster_listener_launch.yaml"
      let:
        - name: "target_frame"
          value: "carrot1"
  - include:
      file: "$(find-pkg-share launch_tutorial)/launch/mimic_launch.yaml"
  - include:
      file: "$(find-pkg-share launch_tutorial)/launch/fixed_broadcaster_launch.yaml"
  - include:
      file: "$(find-pkg-share launch_tutorial)/launch/turtlesim_rviz_launch.yaml"
```

##### Python

复制并粘贴完整的代码到 `launch/launch_turtlesim_launch.py` 文件 :

``` python
from launch import LaunchDescription
from launch.actions import IncludeLaunchDescription
from launch.substitutions import PathJoinSubstitution
from launch_ros.substitutions import FindPackageShare


def generate_launch_description():
    launch_dir = PathJoinSubstitution([FindPackageShare('launch_tutorial'), 'launch'])
    return LaunchDescription([
        IncludeLaunchDescription(
            PathJoinSubstitution([launch_dir, 'turtlesim_world_1.launch.py'])
        ),
        IncludeLaunchDescription(
            PathJoinSubstitution([launch_dir, 'turtlesim_world_2.launch.py'])
        ),
        IncludeLaunchDescription(
            PathJoinSubstitution([launch_dir, 'broadcaster_listener.launch.py']),
            launch_arguments={'target_frame': 'carrot1'}.items()
        ),
        IncludeLaunchDescription(
            PathJoinSubstitution([launch_dir, 'mimic.launch.py'])
        ),
        IncludeLaunchDescription(
            PathJoinSubstitution([launch_dir, 'fixed_broadcaster.launch.py'])
        ),
        IncludeLaunchDescription(
            PathJoinSubstitution([launch_dir, 'turtlesim_rviz.launch.py'])
        ),
    ])
```

这个发射文件包括一组其他发射文件,每个文件包括发射文件包含节点、参数,也可能包括嵌入式文件,它们与系统的一个部分有关。精确地说,我们发射了两个龟象模拟世界、TF广播机、TF听众、模拟器、固定帧广播机和RViz节点。

> **说明**
>
> Design Tip:顶级发射文件应当简短,包含与应用程序子组件对应的其他文件,以及通常更改的参数.

以下列方式撰写发射文件使得我们很容易将系统的一个部件互换出来,我们以后会看到这一点。 但是,有时由于性能和使用原因,有些节点或发射文件必须单独发射。

> **说明**
>
> 设计提示(Design tip):在决定您应用程序需要多少顶级发射文件时,要注意权衡.

<span id="parameters"></span>

### 2 参数

<span id="setting-parameters-in-the-launch-file"></span>

#### 2.1 在发射文件中设置参数

我们将首先写一个启动文件,开始我们第一次龟兹模拟。 `turtlesim_world_1_launch`.

##### XML 数据

复制并粘贴完整的代码到 `launch/turtlesim_world_1_launch.xml` 文件 :

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <arg name="background_r" default="0" />
  <arg name="background_g" default="84" />
  <arg name="background_b" default="122" />
  <node pkg="turtlesim" exec="turtlesim_node" name="sim">
    <param name="background_r" value="$(var background_r)" />
    <param name="background_g" value="$(var background_g)" />
    <param name="background_b" value="$(var background_b)" />
  </node>
</launch>
```

##### 也门

复制并粘贴完整的代码到 `launch/turtlesim_world_1_launch.yaml` 文件 :

``` yaml
%YAML 1.2
---
launch:
  - arg:
      name: "background_r"
      default: "0"
  - arg:
      name: "background_g"
      default: "84"
  - arg:
      name: "background_b"
      default: "122"
  - node:
      pkg: "turtlesim"
      exec: "turtlesim_node"
      name: "sim"
      param:
        - name: "background_r"
          value: "$(var background_r)"
        - name: "background_g"
          value: "$(var background_g)"
        - name: "background_b"
          value: "$(var background_b)"
```

##### Python

复制并粘贴完整的代码到 `launch/turtlesim_world_1_launch.py` 文件 :

``` python
from launch import LaunchDescription
from launch.actions import DeclareLaunchArgument
from launch.substitutions import LaunchConfiguration
from launch_ros.actions import Node


def generate_launch_description():
    return LaunchDescription([
        DeclareLaunchArgument('background_r', default_value='0'),
        DeclareLaunchArgument('background_g', default_value='84'),
        DeclareLaunchArgument('background_b', default_value='122'),
        Node(
            package='turtlesim',
            executable='turtlesim_node',
            name='sim',
            parameters=[{
                'background_r': LaunchConfiguration('background_r'),
                'background_g': LaunchConfiguration('background_g'),
                'background_b': LaunchConfiguration('background_b'),
            }]
        ),
    ])
```

此发射文件启动 `turtlesim_node` 节点,它开始龟兹模拟,其模拟配置参数被定义并传递到节点.

<span id="loading-parameters-from-yaml-file"></span>

#### 2.2 从 YAML 文件装入参数

在第二次发射时,我们将用不同的配置开始第二次龟兹模拟。 `turtlesim_world_2_launch` 文档。

##### XML 数据

复制并粘贴完整的代码到 `launch/turtlesim_world_2_launch.xml` 文件 :

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <node pkg="turtlesim" exec="turtlesim_node" namespace="turtlesim2" name="sim">
    <param from="$(find-pkg-share launch_tutorial)/config/turtlesim.yaml" />
  </node>
</launch>
```

##### 也门

复制并粘贴完整的代码到 `launch/turtlesim_world_2_launch.yaml` 文件 :

``` yaml
%YAML 1.2
---
launch:
  - node:
      pkg: "turtlesim"
      exec: "turtlesim_node"
      namespace: "turtlesim2"
      name: "sim"
      param:
        - from: "$(find-pkg-share launch_tutorial)/config/turtlesim.yaml"
```

##### Python

复制并粘贴完整的代码到 `launch/turtlesim_world_2_launch.py` 文件 :

``` python
from launch import LaunchDescription
from launch.substitutions import PathJoinSubstitution
from launch_ros.actions import Node
from launch_ros.substitutions import FindPackageShare


def generate_launch_description():
    return LaunchDescription([
        Node(
            package='turtlesim',
            executable='turtlesim_node',
            namespace='turtlesim2',
            name='sim',
            parameters=[PathJoinSubstitution([
                FindPackageShare('launch_tutorial'), 'config', 'turtlesim.yaml'])
            ],
        ),
    ])
```

此发射文件将同样发射 `turtlesim_node` 含有直接从 YAML 配置文件加载的参数值。定义 YAML 文件中的参数和参数可以方便地存储和加载大量变量。同样值得注意的是,这个 YAML 文件不是另一个启动文件,而是用于此的配置文件 。 `turtlesim_node` 设置节点的参数。此外,YAML文件很容易从当前导出 `ros2 param` 列表。为了学习如何做到这一点,请参考 [理解参数](../../Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md) 教学。

让我们现在创建一个配置文件, `turtlesim.yaml`时, `/config` 我们包的文件夹, 将会被我们的发射文件加载。

``` YAML
/turtlesim2/sim:
   ros__parameters:
      background_b: 255
      background_g: 86
      background_r: 150
```

为了更多地了解使用参数和使用YAML文件,请查看 [理解参数](../../Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md) 教学。

<span id="using-wildcards-in-yaml-files"></span>

#### 2.3 在 YAML 文件中使用通配符

当我们想要在一个多个节点设置相同的参数时,有这样的情况。这些节点可能有不同的命名空间或名称,但仍有相同的参数。定义单独的YAML文件,明确定义命名空间和节点名称是无效的。一个解决方案是使用通配符字符,在文本值中作为未知字符的替代,将参数应用到几个不同的节点。

现在让我们创造一个新的 `turtlesim_world_3_launch` 类似文件 `turtlesim_world_2_launch` 包括一个 `turtlesim_node` 新命名空间中的节点 `turtlesim3`:

##### XML 数据

复制并粘贴完整的代码到 `launch/turtlesim_world_3_launch.xml` 文件 :

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <node pkg="turtlesim" exec="turtlesim_node" namespace="turtlesim3" name="sim">
    <param from="$(find-pkg-share launch_tutorial)/config/turtlesim.yaml" />
  </node>
</launch>
```

##### 也门

复制并粘贴完整的代码到 `launch/turtlesim_world_3_launch.yaml` 文件 :

``` yaml
%YAML 1.2
---
launch:
  - node:
      pkg: "turtlesim"
      exec: "turtlesim_node"
      namespace: "turtlesim3"
      name: "sim"
      param:
        - from: "$(find-pkg-share launch_tutorial)/config/turtlesim.yaml"
```

##### Python

复制并粘贴完整的代码到 `launch/turtlesim_world_3_launch.py` 文件 :

``` python
from launch import LaunchDescription
from launch.substitutions import PathJoinSubstitution
from launch_ros.actions import Node
from launch_ros.substitutions import FindPackageShare


def generate_launch_description():
    return LaunchDescription([
        Node(
            package='turtlesim',
            executable='turtlesim_node',
            namespace='turtlesim3',
            name='sim',
            parameters=[
                PathJoinSubstitution([
                    FindPackageShare('launch_tutorial'), 'config', 'turtlesim.yaml']),
            ],
        ),
    ])
```

然而,装入相同的YAML文件不会影响第三个龟兹世界的外观,其原因是其参数被存储在下面显示的另外一个命名空间中:

``` console
/turtlesim3/sim:
   background_b
   background_g
   background_r
```

因此,我们不用为使用相同参数的同一节点创建新的配置,而可以使用通配符语法. `/**` 将指定每个节点中的所有参数,尽管节点名称和命名空间有差异。

我们现在将更新 `turtlesim.yaml`时, `/config` 以下列方式编写的文件夹 :

``` YAML
/**:
   ros__parameters:
      background_b: 255
      background_g: 86
      background_r: 150
```

现在包括 `turtlesim_world_3_launch` 发射说明在发射主文件中,使用发射说明中的配置文件将指定 `background_b`, `background_g`,以及 `background_r` 参数到指定值 `turtlesim3/sim` 财务报告和财务报告 `turtlesim2/sim` 节点。

<span id="namespaces"></span>

### 3 个命名空间

正如你可能注意到的,我们已经定义了乌龟世界的名称空间 `turtlesim_world_2_launch` 文件。独特的命名空间允许系统启动两个类似的节点,而无需节点名称或主题名称冲突。

``` Python
namespace='turtlesim2',
```

然而,如果发射文件包含大量节点,那么为每个节点定义命名空间可能会变得乏味。 `PushRosNamespace` 动作可以用来定义每个发射文件描述的全局命名空间。每个嵌套节点将自动继承该命名空间。

> **注意**
>
> `PushRosNamespace` 必须是列表中用于应用命名空间的下列动作的第一个动作.

要做到这一点,首先,我们需要去除 `namespace='turtlesim2'` 从线条 `turtlesim_world_2_launch` 文件。之后,我们需要更新 `launch_turtlesim_launch` 将加入语句改为:

##### XML 数据

``` xml
<group>
  <push_ros_namespace namespace="turtlesim2" />
  <include file="$(find-pkg-share launch_tutorial)/launch/turtlesim_world_2_launch.xml" />
</group>
```

##### 也门

``` yaml
- group:
    - push_ros_namespace:
        namespace: "turtlesim2"
    - include:
        file: "$(find-pkg-share launch_tutorial)/launch/turtlesim_world_2_launch.yaml"
```

##### Python

``` python
from launch.actions import GroupAction
from launch_ros.actions import PushRosNamespace

   ...
   GroupAction(
     actions=[
         PushRosNamespace('turtlesim2'),
         IncludeLaunchDescription(PathJoinSubstitution([launch_dir, 'turtlesim_world_2_launch.py'])),
      ]
   ),
```

因此,每个节点在 `turtlesim_world_2_launch` 发射描述将有一个 `turtlesim2` 名称空间。

<span id="reusing-nodes"></span>

### 4 重用节点

现在创建一个 `broadcaster_listener_launch` 文档。

##### XML 数据

复制并粘贴完整的代码到 `launch/broadcaster_listener_launch.xml` 文件 :

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <arg name="target_frame" default="turtle1" description="Target frame name." />
  <node pkg="turtle_tf2_py" exec="turtle_tf2_broadcaster" name="broadcaster1">
    <param name="turtlename" value="turtle1" />
  </node>
  <node pkg="turtle_tf2_py" exec="turtle_tf2_broadcaster" name="broadcaster2">
    <param name="turtlename" value="turtle2" />
  </node>
  <node pkg="turtle_tf2_py" exec="turtle_tf2_listener" name="listener">
    <param name="target_frame" value="$(var target_frame)" />
  </node>
</launch>
```

##### 也门

复制并粘贴完整的代码到 `launch/broadcaster_listener_launch.yaml` 文件 :

``` yaml
%YAML 1.2
---
launch:
  - arg:
      name: "target_frame"
      default: "turtle1"
      description: "Target frame name."
  - node:
      pkg: "turtle_tf2_py"
      exec: "turtle_tf2_broadcaster"
      name: "broadcaster1"
      param:
        - name: "turtlename"
          value: "turtle1"
  - node:
      pkg: "turtle_tf2_py"
      exec: "turtle_tf2_broadcaster"
      name: "broadcaster2"
      param:
        - name: "turtlename"
          value: "turtle2"
  - node:
      pkg: "turtle_tf2_py"
      exec: "turtle_tf2_listener"
      name: "listener"
      param:
        - name: "target_frame"
          value: "$(var target_frame)"
```

##### Python

复制并粘贴完整的代码到 `launch/broadcaster_listener_launch.py` 文件 :

``` python
from launch import LaunchDescription
from launch.actions import DeclareLaunchArgument
from launch.substitutions import LaunchConfiguration

from launch_ros.actions import Node


def generate_launch_description():
    return LaunchDescription([
        DeclareLaunchArgument(
            'target_frame', default_value='turtle1',
            description='Target frame name.',
        ),
        Node(
            package='turtle_tf2_py',
            executable='turtle_tf2_broadcaster',
            name='broadcaster1',
            parameters=[
                {'turtlename': 'turtle1'}
            ],
        ),
        Node(
            package='turtle_tf2_py',
            executable='turtle_tf2_broadcaster',
            name='broadcaster2',
            parameters=[
                {'turtlename': 'turtle2'}
            ],
        ),
        Node(
            package='turtle_tf2_py',
            executable='turtle_tf2_listener',
            name='listener',
            parameters=[
                {'target_frame': LaunchConfiguration('target_frame')}
            ],
        ),
    ])
```

在这份档案中,我们声明 `target_frame` 发射参数,默认值为 `turtle1`。默认值意味着发射文件可以收到一个向它的节点转发的参数,或者如果没有提供该参数,它会将默认值传递到它的节点.

之后,我们用 `turtle_tf2_broadcaster` 发射时使用不同名称和参数的节点两次。 这样我们就可以在不发生冲突的情况下复制相同的节点 。

我们还开始一个 `turtle_tf2_listener` 节点和设置其 `target_frame` 我们在上面宣布并获得的参数。

<span id="parameter-overrides"></span>

### 5 参数覆盖

记得我们曾称 `broadcaster_listener_launch` 我们的顶级发射文件中的文件。除此之外,我们已经通过了它。 `target_frame` 发射理由如下:

##### XML 数据

``` xml
  <include file="$(find-pkg-share launch_tutorial)/launch/broadcaster_listener_launch.xml">
    <let name="target_frame" value="carrot1" />
  </include>
```

##### 也门

``` yaml
  - include:
      file: "$(find-pkg-share launch_tutorial)/launch/broadcaster_listener_launch.yaml"
      let:
        - name: "target_frame"
          value: "carrot1"
```

##### Python

``` python
        IncludeLaunchDescription(
            PathJoinSubstitution([launch_dir, 'broadcaster_listener.launch.py']),
            launch_arguments={'target_frame': 'carrot1'}.items()
        ),
```

此语法允许我们更改默认目标框架为 `carrot1`。如果您愿意的话 `turtle2` 接下来 `turtle1` 代替 `carrot1`,只要去掉通过 `target_frame` 参数。此选项将指定 `target_frame` 默认值,即 `turtle1`.

<span id="remapping"></span>

### 6 重新绘图

现在创建一个 `mimic_launch` 文档。

##### XML 数据

复制并粘贴完整的代码到 `launch/mimic_launch.xml` 文件 :

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <node pkg="turtlesim" exec="mimic" name="mimic">
    <remap from="/input/pose" to="/turtle2/pose" />
    <remap from="/output/cmd_vel" to="/turtlesim2/turtle1/cmd_vel" />
  </node>
</launch>
```

##### 也门

复制并粘贴完整的代码到 `launch/mimic_launch.yaml` 文件 :

``` yaml
%YAML 1.2
---
launch:
  - node:
      pkg: "turtlesim"
      exec: "mimic"
      name: "mimic"
      remap:
        - from: "/input/pose"
          to: "/turtle2/pose"
        - from: "/output/cmd_vel"
          to: "/turtlesim2/turtle1/cmd_vel"
```

##### Python

复制并粘贴完整的代码到 `launch/mimic_launch.py` 文件 :

``` python
from launch import LaunchDescription
from launch_ros.actions import Node


def generate_launch_description():
    return LaunchDescription([
        Node(
            package='turtlesim',
            executable='mimic',
            name='mimic',
            remappings=[
                ('/input/pose', '/turtle2/pose'),
                ('/output/cmd_vel', '/turtlesim2/turtle1/cmd_vel'),
            ]
        )
    ])
```

此发射文件将启动 `mimic` 节点,它会给一个龟兹姆命令跟随另一个龟兹。节点旨在接收主题上的目标姿势 `/input/pose`就我们而言,我们想重新绘制目标位置图 `/turtle2/pose` 最后,我们重新绘制 `/output/cmd_vel` 专题至 `/turtlesim2/turtle1/cmd_vel`这边 `turtle1` 在我们 `turtlesim2` 模拟世界将随之而来 `turtle2` 在我们最初的乌龟世界里

<span id="config-files"></span>

### 7 配置文件

让我们现在创建一个名为“ ” 的文件 `turtlesim_rviz_launch`.

##### XML 数据

复制并粘贴完整的代码到 `launch/turtlesim_rviz_launch.xml` 文件 :

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <node pkg="rviz2" exec="rviz2" name="rviz2"
        args="-d $(find-pkg-share turtle_tf2_py)/rviz/turtle_rviz.rviz" />
</launch>
```

##### 也门

复制并粘贴完整的代码到 `launch/turtlesim_rviz_launch.yaml` 文件 :

``` yaml
%YAML 1.2
---
launch:
  - node:
      pkg: "rviz2"
      exec: "rviz2"
      name: "rviz2"
      args: "-d $(find-pkg-share turtle_tf2_py)/rviz/turtle_rviz.rviz"
```

##### Python

复制并粘贴完整的代码到 `launch/turtlesim_rviz_launch.py` 文件 :

``` python
from launch import LaunchDescription
from launch.substitutions import PathJoinSubstitution
from launch_ros.actions import Node
from launch_ros.substitutions import FindPackageShare


def generate_launch_description():
    return LaunchDescription([
        Node(
            package='rviz2',
            executable='rviz2',
            name='rviz2',
            arguments=['-d', PathJoinSubstitution([
                FindPackageShare('turtle_tf2_py'), 'rviz', 'turtle_rviz.rviz'])],
        ),
    ])
```

此启动文件将启动 RViz 配置文件 。 `turtle_tf2_py` 软件包。这种 RViz 配置将设置世界框架,启用 TF 可视化,并以自上而下的视图启动 RViz 。

<span id="environment-variables"></span>

### 8 环境变量

让我们现在创建最后的发射文件 `fixed_broadcaster_launch` 在我们的包裹。

##### XML 数据

复制并粘贴完整的代码到 `launch/fixed_broadcaster_launch.xml` 文件 :

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <arg name="node_prefix" default="$(env USER '')_" description="prefix for node name" />
  <node pkg="turtle_tf2_py" exec="fixed_frame_tf2_broadcaster" name="$(var node_prefix)fixed_broadcaster" />
</launch>
```

##### 也门

复制并粘贴完整的代码到 `launch/fixed_broadcaster_launch.yaml` 文件 :

``` yaml
%YAML 1.2
---
launch:
  - arg:
      name: "node_prefix"
      default: "$(env USER '')_"
      description: "prefix for node name"
  - node:
      pkg: "turtle_tf2_py"
      exec: "fixed_frame_tf2_broadcaster"
      name: "$(var node_prefix)fixed_broadcaster"
```

##### Python

复制并粘贴完整的代码到 `launch/fixed_broadcaster_launch.py` 文件 :

``` python
from launch import LaunchDescription
from launch.actions import DeclareLaunchArgument
from launch.substitutions import EnvironmentVariable, LaunchConfiguration
from launch_ros.actions import Node


def generate_launch_description():
    return LaunchDescription([
        DeclareLaunchArgument(
            'node_prefix',
            default_value=[EnvironmentVariable('USER'), '_'],
            description='prefix for node name'
        ),
        Node(
            package='turtle_tf2_py',
            executable='fixed_frame_tf2_broadcaster',
            name=[LaunchConfiguration('node_prefix'), 'fixed_broadcaster'],
        ),
    ])
```

这个发射文件显示了在发射文件内部可以调用环境变量的方法. 环境变量可以用来定义或推动命名空间,以区分不同计算机或机器人上的节点.

> **说明**
>
> 如果你正在运行 发射文件在哪里 `USER` 环境变量没有定义(如ROS docker文件中),然后可以将上面的环境变量引用替换为任何其他你喜欢的单词.

<span id="running-launch-files"></span>

## 运行启动文件

<span id="update-setup-py"></span>

### 1 更新设置. py

打开 `setup.py` 并添加下列行,以便从 `launch/` 文件夹和配置文件 `config/` 将安装。 `data_files` 字段现在应该是这样的:

``` Python
import os
from glob import glob
from setuptools import setup
...

data_files=[
      ...
      (os.path.join('share', package_name, 'launch'),
         glob('launch/*')),
      (os.path.join('share', package_name, 'config'),
         glob('config/*.yaml')),
      (os.path.join('share', package_name, 'rviz'),
         glob('config/*.rviz')),
   ],
```

<span id="build-and-run"></span>

### 2 构建和运行

为了最终看到我们代码的结果,构建软件包,并使用以下命令发射顶级发射文件:

##### XML 数据

``` console
$ ros2 launch launch_tutorial launch_turtlesim_launch.xml
```

##### 也门

``` console
$ ros2 launch launch_tutorial launch_turtlesim_launch.yaml
```

##### Python

``` console
$ ros2 launch launch_tutorial launch_turtlesim_launch.py
```

现在你们将看到两只龟龟模拟开始。第一只龟有两只龟,第二只龟有一只龟。在第一只龟模拟中, `turtle2` 它的目的是为了到达世界最左边的地方。 `carrot1` 在X轴上距离5米的帧相对 `turtle1` 边框。

那个... `turtlesim2/turtle1` 在第二组中,它旨在模仿人类的行为 `turtle2`.

如果你想控制 `turtle1`运行电话节点。

``` console
$ ros2 run turtlesim turtle_teleop_key
```

因此,你会看到类似的情况:

![](images/turtlesim_worlds.png)

除此之外,RViz应该已经开始了。它会显示所有与该图相对的龟框。 `world` 框,其来源位于左下角。

![](images/turtlesim_rviz.png) <span id="summary"></span>

## 小结

在这个教程中,你学到了使用ROS 2发射文件管理大型项目的各种技巧和做法.
