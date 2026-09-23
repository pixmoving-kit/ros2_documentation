---
translation_status: machine_translated
source: Tutorials/Intermediate/Launch/Using-Substitutions.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-substitutions"></span>

# 使用替换表达式

**目标：** 了解ROS 2发射文件中的替代.

**教程级别：** 中级

**用时：** 15分钟

<span id="background"></span>

## 背景

启动文件用于启动节点、服务和执行过程。 这套动作可能具有参数, 影响其行为。 在描述可重复使用的发射文件时, 可用替换来提供更大的灵活性 。 替换是在执行发射描述时才评价的变量, 并可用于获取特定信息, 如发射配置、 环境变量, 或评估任意的 Python 表达式 。

此教程显示ROS 2 发射文件中的替换用例 。

<span id="prerequisites"></span>

## 前提条件

此教程使用 [乌龟](../../Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md) 软件包。此教程还假定您熟悉 [创建软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md).

与往常一样, [您打开的每个新终端](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md).

<span id="id1"></span>

## 使用替换表达式

<span id="create-and-setup-the-package"></span>

### 1 创建和设置软件包

首先, 创建新软件包, 并命名 `launch_tutorial`:

##### Python 软件包

创建新构建软件包_类型 `ament_python`:

``` console
$ ros2 pkg create --build-type ament_python --license Apache-2.0 launch_tutorial
```

##### C++ 软件包

创建新构建软件包_类型 `ament_cmake`:

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 launch_tutorial
```

在该软件包内创建一个名为 `launch`:

##### Linux

``` console
$ mkdir launch_tutorial/launch
```

##### macOS

``` console
$ mkdir launch_tutorial/launch
```

##### Windows

``` console
$ md launch_tutorial/launch
```

最后,确保安装发射文件:

##### Python 软件包

在以下更改中添加 `setup.py` 软件包中:

``` python
import os
from glob import glob
from setuptools import find_packages, setup

package_name = 'launch_tutorial'

setup(
    # Other parameters ...
    data_files=[
        # ... Other data files
        # Include all launch files.
        (os.path.join('share', package_name, 'launch'), glob('launch/*'))
    ]
)
```

##### C++ 软件包

附加以下代码到 `CMakeLists.txt` 刚才 `ament_package()`:

``` cmake
install(DIRECTORY
        launch
        DESTINATION share/${PROJECT_NAME}/
)
```

<span id="parent-launch-file"></span>

### 2 父发射文件

让我们创建一个发射文件,将调用参数传递到另一个发射文件。这个发射文件可以使用YAML、XML或Python。

要做到这一点,请在 `launch` 文件夹 `launch_tutorial` 软件包。

##### XML 数据

复制并粘贴完整的代码到 `launch/example_main_launch.xml` 文件 :

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <let name="background_r" value="200" />
  <include file="$(find-pkg-share launch_tutorial)/launch/example_substitutions_launch.xml">
    <let name="turtlesim_ns" value="turtlesim2" />
    <let name="use_provided_red" value="True" />
    <let name="new_background_r" value="$(var background_r)" />
  </include>
</launch>
```

那个... `$(find-pkg-share launch_tutorial)` 用于查找路径 `launch_tutorial` 软件包。然后将路径替换与 `example_substitutions_launch.xml` 文件名 。

``` xml
  <include file="$(find-pkg-share launch_tutorial)/launch/example_substitutions_launch.xml">
```

那个... `background_r` 变量为 `turtlesim_ns` 财务报告和财务报告 `use_provided_red` 参数传递到 `include` 动作。 `$(var background_r)` 替换用于定义 `new_background_r` 参数与该参数的值 `background_r` 变量。

``` xml
    <let name="turtlesim_ns" value="turtlesim2" />
    <let name="use_provided_red" value="True" />
    <let name="new_background_r" value="$(var background_r)" />
```

##### 也门

复制并粘贴完整的代码到 `launch/example_main_launch.yaml` 文件 :

``` yaml
%YAML 1.2
---
launch:
  - let:
      name: "background_r"
      value: "200"
  - include:
      file: "$(find-pkg-share launch_tutorial)/launch/example_substitutions_launch.yaml"
      let:
        - name: "turtlesim_ns"
          value: "turtlesim2"
        - name: "use_provided_red"
          value: "True"
        - name: "new_background_r"
          value: "$(var background_r)"
```

那个... `$(find-pkg-share launch_tutorial)` 用于查找路径 `launch_tutorial` 软件包。然后将路径替换与 `example_substitutions_launch.yaml` 文件名 。

``` yaml
      file: "$(find-pkg-share launch_tutorial)/launch/example_substitutions_launch.yaml"
```

那个... `background_r` 变量为 `turtlesim_ns` 财务报告和财务报告 `use_provided_red` 参数传递到 `include` 动作。 `$(var background_r)` 替换用于定义 `new_background_r` 参数与该参数的值 `background_r` 变量。

``` yaml
      let:
        - name: "turtlesim_ns"
          value: "turtlesim2"
        - name: "use_provided_red"
          value: "True"
        - name: "new_background_r"
          value: "$(var background_r)"
```

##### Python

复制并粘贴完整的代码到 `launch/example_main.launch.py` 文件 :

``` python
from launch import LaunchDescription
from launch.actions import IncludeLaunchDescription
from launch.substitutions import PathJoinSubstitution
from launch_ros.substitutions import FindPackageShare


def generate_launch_description():
    colors = {
        'background_r': '200'
    }

    return LaunchDescription([
        IncludeLaunchDescription(
            PathJoinSubstitution([
                FindPackageShare('launch_tutorial'),
                'launch',
                'example_substitutions.launch.py'
            ]),
            launch_arguments={
                'turtlesim_ns': 'turtlesim2',
                'use_provided_red': 'True',
                'new_background_r': colors['background_r'],
            }.items()
        )
    ])
```

那个... `FindPackageShare` 用于查找路径 `launch_tutorial` 软件包。 `PathJoinSubstitution` 然后使用替换来加入该软件包路径的路径 。 `example_substitutions.launch.py` 文件名 。

``` python
            PathJoinSubstitution([
                FindPackageShare('launch_tutorial'),
                'launch',
                'example_substitutions.launch.py'
            ]),
```

那个... `launch_arguments` 词典为 `turtlesim_ns` 财务报告和财务报告 `use_provided_red` 参数传递到 `IncludeLaunchDescription` 行动。

``` python
            launch_arguments={
                'turtlesim_ns': 'turtlesim2',
                'use_provided_red': 'True',
                'new_background_r': colors['background_r'],
            }.items()
```

<span id="substitutions-example-launch-file"></span>

### 3 替换实例发射文件

现在在同一文件夹中创建替代发射文件 :

##### XML 数据

创建文件 `launch/example_substitutions_launch.xml` 并插入以下代码:

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <arg name="turtlesim_ns" default="turtlesim1" />
  <arg name="use_provided_red" default="False" />
  <arg name="new_background_r" default="200" />

  <node pkg="turtlesim" namespace="$(var turtlesim_ns)" exec="turtlesim_node" name="sim" />
  <executable cmd="ros2 service call $(var turtlesim_ns)/spawn turtlesim/srv/Spawn '{x: 5, y: 2, theta: 0.2}'" />
  <executable cmd="ros2 param set $(var turtlesim_ns)/sim background_r 120" />
  <timer period="2.0">
    <executable cmd="ros2 param set $(var turtlesim_ns)/sim background_r $(var new_background_r)"
      if="$(eval '$(var new_background_r) == 200 and $(var use_provided_red)')" />
  </timer>
</launch>
```

那个... `turtlesim_ns`, `use_provided_red`,以及 `new_background_r` 发射配置被定义。 用于将发射参数的值存储在上述变量中, 并传递到所需的动作中。 发射配置参数可以随后与 `$(var <name>)` 在发射说明的任何部分获得发射论据的价值。

那个... `arg` 标记用于定义可以从上述发射文件或控制台上传递的发射参数。

``` xml
  <arg name="turtlesim_ns" default="turtlesim1" />
  <arg name="use_provided_red" default="False" />
  <arg name="new_background_r" default="200" />
```

那个... `turtlesim_node` 带节点 `namespace` 设置为 `turtlesim_ns` 使用 `$(var <name>)` 替代的定义。

``` xml
  <node pkg="turtlesim" namespace="$(var turtlesim_ns)" exec="turtlesim_node" name="sim" />
```

之后,一个 `executable` 动作定义为相应的 `cmd` 标记。这个命令给龟兹节点的产卵服务打个电话。

此外, `$(var <name>)` 替换用于获取 `turtlesim_ns` 用于构建命令字符串的启动参数 。

``` xml
  <executable cmd="ros2 service call $(var turtlesim_ns)/spawn turtlesim/srv/Spawn '{x: 5, y: 2, theta: 0.2}'" />
```

采用同样的方法处理 `ros2 param` `executable` 动作改变图案背景的红色参数。不同的是,计时器内部的第二个动作只有在提供时才执行 `new_background_r` 参数等同 `200` 页:1 `use_provided_red` 启动参数设定为 `True`评估《公约》执行情况 `if` 上游是使用 `$(eval <python-expression>)` 替换。

``` xml
  <executable cmd="ros2 param set $(var turtlesim_ns)/sim background_r 120" />
  <timer period="2.0">
    <executable cmd="ros2 param set $(var turtlesim_ns)/sim background_r $(var new_background_r)"
      if="$(eval '$(var new_background_r) == 200 and $(var use_provided_red)')" />
  </timer>
```

##### 也门

创建文件 `launch/example_substitutions_launch.yaml` 并插入以下代码:

``` yaml
%YAML 1.2
---
launch:
  - arg:
      name: "turtlesim_ns"
      default: "turtlesim1"
  - arg:
      name: "use_provided_red"
      default: "False"
  - arg:
      name: "new_background_r"
      default: "200"

  - node:
      pkg: "turtlesim"
      namespace: "$(var turtlesim_ns)"
      exec: "turtlesim_node"
      name: "sim"
  - executable:
      cmd: 'ros2 service call $(var turtlesim_ns)/spawn turtlesim/srv/Spawn "{x: 5, y: 2, theta: 0.2}"'
  - executable:
      cmd: "ros2 param set $(var turtlesim_ns)/sim background_r 120"
  - timer:
      period: 2.0
      children:
        - executable:
            cmd: "ros2 param set $(var turtlesim_ns)/sim background_r $(var new_background_r)"
            if: '$(eval "$(var new_background_r) == 200 and $(var use_provided_red)")'
```

那个... `turtlesim_ns`, `use_provided_red`,以及 `new_background_r` 发射配置被定义。 用于将发射参数的值存储在上述变量中, 并传递到所需的动作中。 发射配置参数可以随后与 `$(var <name>)` 在发射说明的任何部分获得发射论据的价值。

那个... `arg` 标记用于定义可以从上述发射文件或控制台上传递的发射参数。

``` yaml
  - arg:
      name: "turtlesim_ns"
      default: "turtlesim1"
  - arg:
      name: "use_provided_red"
      default: "False"
  - arg:
      name: "new_background_r"
      default: "200"
```

那个... `turtlesim_node` 带节点 `namespace` 设置为 `turtlesim_ns` 使用 `$(var <name>)` 替代的定义。

``` yaml
  - node:
      pkg: "turtlesim"
      namespace: "$(var turtlesim_ns)"
      exec: "turtlesim_node"
      name: "sim"
```

之后,一个 `executable` 动作定义为相应的 `cmd` 标记。这个命令给龟兹节点的产卵服务打个电话。

此外, `$(var <name>)` 替换用于获取 `turtlesim_ns` 用于构建命令字符串的启动参数 。

``` yaml
  - executable:
      cmd: 'ros2 service call $(var turtlesim_ns)/spawn turtlesim/srv/Spawn "{x: 5, y: 2, theta: 0.2}"'
```

采用同样的方法处理 `ros2 param` `executable` 动作改变图案背景的红色参数。不同的是,计时器内部的第二个动作只有在提供时才执行 `new_background_r` 参数等同 `200` 页:1 `use_provided_red` 启动参数设定为 `True`评估《公约》执行情况 `if` 上游是使用 `$(eval <python-expression>)` 替换。

``` yaml
  - executable:
      cmd: "ros2 param set $(var turtlesim_ns)/sim background_r 120"
  - timer:
      period: 2.0
      children:
        - executable:
            cmd: "ros2 param set $(var turtlesim_ns)/sim background_r $(var new_background_r)"
            if: '$(eval "$(var new_background_r) == 200 and $(var use_provided_red)")'
```

##### Python

创建文件 `launch/example_substitutions.launch.py` 并插入以下代码:

``` python
from launch import LaunchDescription
from launch.actions import DeclareLaunchArgument, ExecuteProcess, TimerAction
from launch.conditions import IfCondition
from launch.substitutions import LaunchConfiguration, PythonExpression
from launch_ros.actions import Node


def generate_launch_description():
    turtlesim_ns = LaunchConfiguration('turtlesim_ns')
    use_provided_red = LaunchConfiguration('use_provided_red')
    new_background_r = LaunchConfiguration('new_background_r')

    return LaunchDescription([
        DeclareLaunchArgument(
            'turtlesim_ns',
            default_value='turtlesim1'
        ),
        DeclareLaunchArgument(
            'use_provided_red',
            default_value='False'
        ),
        DeclareLaunchArgument(
            'new_background_r',
            default_value='200'
        ),
        Node(
            package='turtlesim',
            namespace=turtlesim_ns,
            executable='turtlesim_node',
            name='sim'
        ),
        ExecuteProcess(
            cmd=[[
                'ros2 service call ',
                turtlesim_ns,
                '/spawn ',
                'turtlesim/srv/Spawn ',
                '"{x: 2, y: 2, theta: 0.2}"'
            ]],
            shell=True
        ),
        ExecuteProcess(
            cmd=[[
                'ros2 param set ',
                turtlesim_ns,
                '/sim background_r ',
                '120'
            ]],
            shell=True
        ),
        TimerAction(
            period=2.0,
            actions=[
                ExecuteProcess(
                    condition=IfCondition(
                        PythonExpression([
                            new_background_r,
                            ' == 200',
                            ' and ',
                            use_provided_red
                        ])
                    ),
                    cmd=[[
                        'ros2 param set ',
                        turtlesim_ns,
                        '/sim background_r ',
                        new_background_r
                    ]],
                    shell=True
                ),
            ],
        )
    ])
```

那个... `turtlesim_ns`, `use_provided_red`,以及 `new_background_r` 发射配置被定义,用于代表上述变量中发射参数的值并将其传递给所需的动作。 `LaunchConfiguration` 替代方法使我们能够在发射说明的任何部分中获得发射论据的价值。

`DeclareLaunchArgument` 用于定义可以从上述发射文件或控制台上传递的发射参数。

``` python
        DeclareLaunchArgument(
            'turtlesim_ns',
            default_value='turtlesim1'
        ),
        DeclareLaunchArgument(
            'use_provided_red',
            default_value='False'
        ),
        DeclareLaunchArgument(
            'new_background_r',
            default_value='200'
        ),
```

那个... `turtlesim_node` 带节点 `namespace` 设置为 `turtlesim_ns` `LaunchConfiguration` 替代的定义。

``` python
        Node(
            package='turtlesim',
            namespace=turtlesim_ns,
            executable='turtlesim_node',
            name='sim'
        ),
```

接下来的行动, `ExecuteProcess`,用相应的 `cmd` 参数称为龟兹节点的产卵服务。

此外, `LaunchConfiguration` 替换用于提供该物质的值。 `turtlesim_ns` 命令字符串中的启动参数 。

``` python
        ExecuteProcess(
            cmd=[[
                'ros2 service call ',
                turtlesim_ns,
                '/spawn ',
                'turtlesim/srv/Spawn ',
                '"{x: 2, y: 2, theta: 0.2}"'
            ]],
            shell=True
        ),
```

采用同样的方法处理 `change_background_r` 财务报告和财务报告 `change_background_r_conditioned` 动作会改变tolsim背景的红色参数。不同的是,下一个动作只有在提供了 `new_background_r` 参数等同 `200` 页:1 `use_provided_red` 启动参数设定为 `True`内部评价。 `IfCondition` 使用 `PythonExpression` 替换。

``` python
        TimerAction(
            period=2.0,
            actions=[
                ExecuteProcess(
                    condition=IfCondition(
                        PythonExpression([
                            new_background_r,
                            ' == 200',
                            ' and ',
                            use_provided_red
                        ])
                    ),
                    cmd=[[
                        'ros2 param set ',
                        turtlesim_ns,
                        '/sim background_r ',
                        new_background_r
                    ]],
                    shell=True
                ),
            ],
        )
```

<span id="build-the-package"></span>

### 4 构建软件包

转到工作空间的根,然后构建软件包:

``` console
$ colcon build
```

还记得在建完后提供工作空间的源头.

<span id="launching-example"></span>

## 启动实例

现在,你可以使用 `ros2 launch` 命令。 命令。

##### 也门

``` console
$ ros2 launch launch_tutorial example_main.launch.yaml
```

##### XML 数据

``` console
$ ros2 launch launch_tutorial example_main_launch.xml
```

##### Python

``` console
$ ros2 launch launch_tutorial example_main.launch.py
```

这将实现以下目标:

1.  启动蓝色背景的龟形节点

2.  第二只乌龟生了出来

3.  将颜色改为紫色

4.  如果提供了二秒后将颜色改为粉红色 `background_r` 参数是 `200` 财务报告和财务报告 `use_provided_red` 参数是 `True`

<span id="modifying-launch-arguments"></span>

## 修改启动参数

##### 也门

如果您想要更改提供的启动参数, 您可以更新 `background_r` 变量在 `example_main.launch.yaml` 或发射 `example_substitutions.launch.yaml` 要查看可能提供给发射文件的参数,请运行以下命令:

``` console
$ ros2 launch launch_tutorial example_substitutions.launch.yaml --show-args
```

##### XML 数据

如果您想要更改提供的启动参数, 您可以更新 `background_r` 变量在 `example_main_launch.xml` 或发射 `example_substitutions_launch.xml` 要查看可能提供给发射文件的参数,请运行以下命令:

``` console
$ ros2 launch launch_tutorial example_substitutions_launch.xml --show-args
```

##### Python

如果您想要更改提供的启动参数, 您可以在 `launch_arguments` 词典 `example_main.launch.py` 或发射 `example_substitutions.launch.py` 要查看可能提供给发射文件的参数,请运行以下命令:

``` console
$ ros2 launch launch_tutorial example_substitutions.launch.py --show-args
```

这将显示可能给发射文件的参数及其默认值。

``` console
Arguments (pass arguments as '<name>:=<value>'):

    'turtlesim_ns':
        no description given
        (default: 'turtlesim1')

    'use_provided_red':
        no description given
        (default: 'False')

    'new_background_r':
        no description given
        (default: '200')
```

现在您可以将想要的参数传递给发射文件如下:

##### 也门

``` console
$ ros2 launch launch_tutorial example_substitutions.launch.yaml turtlesim_ns:='turtlesim3' use_provided_red:='True' new_background_r:=200
```

##### XML 数据

``` console
$ ros2 launch launch_tutorial example_substitutions_launch.xml turtlesim_ns:='turtlesim3' use_provided_red:='True' new_background_r:=200
```

##### Python

``` console
$ ros2 launch launch_tutorial example_substitutions.launch.py turtlesim_ns:='turtlesim3' use_provided_red:='True' new_background_r:=200
```

<span id="documentation"></span>

## 文档

[发射文件](https://docs.ros.org/en/rolling/p/launch/doc/source/architecture.html) 提供关于现有替代物的详细资料。

<span id="summary"></span>

## 小结

在此教程中, 你学到了在发射文件中使用替代物。 你学到了它们创建可重复使用的发射文件的可能性和能力 。

你现在可以多学点了 [在启动文件中使用事件处理器](Using-Event-Handlers.md) 用于定义一套复杂的规则,这些规则可用于动态修改发射文件。
