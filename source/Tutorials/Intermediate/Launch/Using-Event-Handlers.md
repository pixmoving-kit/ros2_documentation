---
translation_status: machine_translated
source: Tutorials/Intermediate/Launch/Using-Event-Handlers.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-event-handlers"></span>

# 使用事件处理器

**目标：** 在 ROS 2 发射文件中学习事件处理器

**教程级别：** 中级

**用时：** 15分钟

<span id="background"></span>

## 背景

在 ROS 2 中启动是一个执行和管理用户定义的进程的系统,它负责监测它所启动的进程的状态,以及对这些进程状态的变化进行汇报和反应。这些变化被称为事件,可以通过在发射系统登记事件处理器来处理。事件处理器可以为特定事件注册,并可用于监测进程状态。此外,还可以用来定义一套复杂的规则,用于动态修改发射文件。

此教程在 ROS 2 发射文件中显示事件处理器的用法实例 。

<span id="prerequisites"></span>

## 前提条件

此教程使用 [乌龟](../../Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md) 软件包。此教程还假定您有 [创建新软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md) 结构类型 `ament_python` 调用 `launch_tutorial`.

此教程扩展显示于 [在发射文件中使用替换](Using-Substitutions.md) 教学。

<span id="id1"></span>

## 使用事件处理器

<span id="event-handlers-example-launch-file"></span>

### 1 事件处理器实例发射文件

创建名为的新文件 `example_event_handlers.launch.py` 输入 `launch` 文件夹 `launch_tutorial` 软件包。

``` python

from launch import LaunchDescription
from launch.actions import (
    DeclareLaunchArgument,
    EmitEvent,
    ExecuteProcess,
    LogInfo,
    RegisterEventHandler,
    TimerAction
)
from launch.conditions import IfCondition
from launch.event_handlers import (
    OnExecutionComplete,
    OnProcessExit,
    OnProcessIO,
    OnProcessStart,
    OnShutdown
)
from launch.events import Shutdown
from launch.substitutions import (
    EnvironmentVariable,
    FindExecutable,
    LaunchConfiguration,
    LocalSubstitution,
    PythonExpression
)
from launch_ros.actions import Node


def generate_launch_description():
    turtlesim_ns = LaunchConfiguration('turtlesim_ns')
    use_provided_red = LaunchConfiguration('use_provided_red')
    new_background_r = LaunchConfiguration('new_background_r')

    turtlesim_ns_launch_arg = DeclareLaunchArgument(
        'turtlesim_ns',
        default_value='turtlesim1'
    )
    use_provided_red_launch_arg = DeclareLaunchArgument(
        'use_provided_red',
        default_value='False'
    )
    new_background_r_launch_arg = DeclareLaunchArgument(
        'new_background_r',
        default_value='200'
    )

    turtlesim_node = Node(
        package='turtlesim',
        namespace=turtlesim_ns,
        executable='turtlesim_node',
        name='sim'
    )
    spawn_turtle = ExecuteProcess(
        cmd=[[
            FindExecutable(name='ros2'),
            ' service call ',
            turtlesim_ns,
            '/spawn ',
            'turtlesim/srv/Spawn ',
            '"{x: 2, y: 2, theta: 0.2}"'
        ]],
        shell=True
    )
    change_background_r = ExecuteProcess(
        cmd=[[
            FindExecutable(name='ros2'),
            ' param set ',
            turtlesim_ns,
            '/sim background_r ',
            '120'
        ]],
        shell=True
    )
    change_background_r_conditioned = ExecuteProcess(
        condition=IfCondition(
            PythonExpression([
                new_background_r,
                ' == 200',
                ' and ',
                use_provided_red
            ])
        ),
        cmd=[[
            FindExecutable(name='ros2'),
            ' param set ',
            turtlesim_ns,
            '/sim background_r ',
            new_background_r
        ]],
        shell=True
    )
    return LaunchDescription([
        turtlesim_ns_launch_arg,
        use_provided_red_launch_arg,
        new_background_r_launch_arg,
        turtlesim_node,
        RegisterEventHandler(
            OnProcessStart(
                target_action=turtlesim_node,
                on_start=[
                    LogInfo(msg='Turtlesim started, spawning turtle'),
                    spawn_turtle
                ]
            )
        ),
        RegisterEventHandler(
            OnProcessIO(
                target_action=spawn_turtle,
                on_stdout=lambda event: LogInfo(
                    msg='Spawn request says "{}"'.format(
                        event.text.decode().strip())
                )
            )
        ),
        RegisterEventHandler(
            OnExecutionComplete(
                target_action=spawn_turtle,
                on_completion=[
                    LogInfo(msg='Spawn finished'),
                    change_background_r,
                    TimerAction(
                        period=2.0,
                        actions=[change_background_r_conditioned],
                    )
                ]
            )
        ),
        RegisterEventHandler(
            OnProcessExit(
                target_action=turtlesim_node,
                on_exit=[
                    LogInfo(msg=(EnvironmentVariable(name='USER'),
                            ' closed the turtlesim window')),
                    EmitEvent(event=Shutdown(
                        reason='Window closed'))
                ]
            )
        ),
        RegisterEventHandler(
            OnShutdown(
                on_shutdown=[LogInfo(
                    msg=['Launch was asked to shutdown: ', LocalSubstitution('event.reason')]
                )]
            )
        ),
    ])
```

`RegisterEventHandler` 行动的执行情况 `OnProcessStart`, `OnProcessIO`, `OnExecutionComplete`, `OnProcessExit`,以及 `OnShutdown` 发射说明对事件作了界定。

那个... `OnProcessStart` 事件处理器用于注册一个在龟兹节点启动时执行的调用函数。它记录到控制台的一条消息并执行 `spawn_turtle` 当龟穴节点开始时动作。

``` python
        RegisterEventHandler(
            OnProcessStart(
                target_action=turtlesim_node,
                on_start=[
                    LogInfo(msg='Turtlesim started, spawning turtle'),
                    spawn_turtle
                ]
            )
        ),
```

那个... `OnProcessIO` 事件处理器用于注册调用函数,该函数在 `spawn_turtle` 动作写入其标准输出。它记录产卵请求的结果。

``` python
        RegisterEventHandler(
            OnProcessIO(
                target_action=spawn_turtle,
                on_stdout=lambda event: LogInfo(
                    msg='Spawn request says "{}"'.format(
                        event.text.decode().strip())
                )
            )
        ),
```

那个... `OnExecutionComplete` 事件处理器用于注册调用函数,该函数在 `spawn_turtle` 操作完成。它会登录到控制台并执行 `change_background_r` 财务报告和财务报告 `change_background_r_conditioned` 当产卵动作完成时行动。

``` python
        RegisterEventHandler(
            OnExecutionComplete(
                target_action=spawn_turtle,
                on_completion=[
                    LogInfo(msg='Spawn finished'),
                    change_background_r,
                    TimerAction(
                        period=2.0,
                        actions=[change_background_r_conditioned],
                    )
                ]
            )
        ),
```

那个... `OnProcessExit` 事件处理器用于注册一个在龟兹节点退出时执行的召回功能。它记录到控制台的一条消息并执行 `EmitEvent` 发射导弹的行动 `Shutdown` 。这意味着当龟兹姆窗口关闭时,发射过程将关闭。

``` python
        RegisterEventHandler(
            OnProcessExit(
                target_action=turtlesim_node,
                on_exit=[
                    LogInfo(msg=(EnvironmentVariable(name='USER'),
                            ' closed the turtlesim window')),
                    EmitEvent(event=Shutdown(
                        reason='Window closed'))
                ]
            )
        ),
```

最后, `OnShutdown` 事件处理器用于注册一个在发射文件被请求关闭时执行的调用函数。它会登录一个消息到控制台,因为为什么要求发射文件关闭。它会登录消息,并有关闭原因,如关闭龟兹窗口或 <span class="kbd kbd docutils literal notranslate">缩略语</span>-<span class="kbd kbd docutils literal notranslate">c</span> 由用户制作的信号。

``` python
        RegisterEventHandler(
            OnShutdown(
                on_shutdown=[LogInfo(
                    msg=['Launch was asked to shutdown: ', LocalSubstitution('event.reason')]
                )]
            )
        ),
```

<span id="build-the-package"></span>

## 构建软件包

转到工作空间的根,然后构建软件包:

``` console
$ colcon build
```

还记得在建完后提供工作空间的源头.

<span id="launching-example"></span>

## 启动实例

现在你可以发射 `example_event_handlers.launch.py` 使用 `ros2 launch` 命令。 命令。

``` console
$ ros2 launch launch_tutorial example_event_handlers.launch.py turtlesim_ns:='turtlesim3' use_provided_red:='True' new_background_r:=200
```

这将实现以下目标:

1.  启动蓝色背景的龟形节点

2.  第二只乌龟生了出来

3.  将颜色改为紫色

4.  如果提供了二秒后将颜色改为粉红色 `background_r` 参数是 `200` 财务报告和财务报告 `use_provided_red` 参数是 `True`

5.  关闭龟兹窗口时关闭发射文件

此外,它将在下列时间将消息登录到控制台:

1.  龟头节点开始

2.  产卵动作被执行

3.  那个... `change_background_r` 动作已执行

4.  那个... `change_background_r_conditioned` 动作已执行

5.  乌龟节点出口

6.  发射过程被要求关闭。

<span id="documentation"></span>

## 文档

[发射文件](https://github.com/ros2/launch/blob/rolling/launch/doc/source/architecture.rst) 提供关于现有事件处理者的详细资料。

<span id="summary"></span>

## 小结

在此教程中, 您学到了在发射文件中使用事件处理器。 您学到了它们的语法和用法实例来定义一套复杂的规则来动态修改发射文件 。
