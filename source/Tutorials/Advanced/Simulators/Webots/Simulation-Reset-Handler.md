---
translation_status: machine_translated
source: Tutorials/Advanced/Simulators/Webots/Simulation-Reset-Handler.rst
---

<span id="setting-up-a-reset-handler"></span>

# 配置重置处理器

**目标：** 使用重置处理器扩展机器人模拟,以便在按下Webots重置按钮时重新启动节点.

**教程级别：** 高级

**用时：** 10分钟

<span id="background"></span>

## 背景

在此教程中, 您将学习如何在使用 Webots 的机器人模拟中执行重置处理器。 Webots 重置按钮将世界恢复到初始状态并重新启动控制器。 快速重置模拟是方便的, 但是在 ROS 2 的背景下, 机器人控制器不会再次启动模拟停止 。 重置处理器允许您在 Webots 重置按钮时重新启动特定节点或执行附加动作 。 这对于您需要重置模拟状态或重启特定组件而不完全重启完整的 ROS 系统的情况可能有用 。

<span id="prerequisites"></span>

## 前提条件

在进行此教程之前, 请确认您已完成以下工作 :

- 了解初步分析中涵盖的ROS 2节点和专题 [教程](../../../../Tutorials.md).

- 了解Webots和ROS 2及其接口软件包.

- 熟悉情况 [配置机器人仿真（基础）](Setting-Up-Simulation-Webots-Basic.md).

<span id="reset-handler-for-simple-cases-controllers-only"></span>

## 简易案件重置处理器( 仅限控制器)

在您的软件包的发射文件中,添加 `respawn` 参数。

``` python
def generate_launch_description():
    robot_driver = WebotsController(
        robot_name='my_robot',
        parameters=[
            {'robot_description': robot_description_path}
        ],

        # Every time one resets the simulation the controller is automatically respawned
        respawn=True
    )

    # Starts Webots
    webots = WebotsLauncher(world=PathJoinSubstitution([package_dir, 'worlds', world]))

    return LaunchDescription([
        webots,
        robot_driver
    ])
```

重设时, Webots 将杀死所有驱动节点。 因此, 要在重设后重新开始, 您应该设置 。 `respawn` 驱动程序节点的属性 `True`。它将确保驱动程序节点在重设后启动并运行。

<span id="reset-handler-for-multiple-nodes-no-shutdown-required"></span>

## 重置多个节点的处理器( 不需要关闭)

如果您拥有一些其他的节点,这些节点必须随驱动程序节点一同启动(例如. `ros2_control` 节点,然后可以使用 `OnProcessExit` 事件处理器 :

``` python
def get_ros2_control_spawners(*args):
    # Declare here all nodes that must be restarted at simulation reset
    ros_control_node = Node(
        package='controller_manager',
        executable='spawner',
        arguments=['diffdrive_controller']
    )
    return [
        ros_control_node
    ]

def generate_launch_description():
    robot_driver = WebotsController(
        robot_name='my_robot',
        parameters=[
            {'robot_description': robot_description_path}
        ],

        # Every time one resets the simulation the controller is automatically respawned
        respawn=True
    )

    # Starts Webots
    webots = WebotsLauncher(world=PathJoinSubstitution([package_dir, 'worlds', world]))

    # Declare the reset handler that respawns nodes when robot_driver exits
    reset_handler = launch.actions.RegisterEventHandler(
        event_handler=launch.event_handlers.OnProcessExit(
            target_action=robot_driver,
            on_exit=get_ros2_control_spawners,
        )
    )

    return LaunchDescription([
        webots,
        robot_driver,
        reset_handler
    ] + get_ros2_control_spawners())
```

无法使用 `respawn` 属性在 `ros2_control` 节点,作为产卵器在发射时退出,而不是在模拟重设时退出。相反,我们应该在一个函数中宣布一个节点列表(例如: `get_ros2_control_spawners`。执行发射文件时,此列表的节点会沿着其他节点开始。 `reset_handler`,该函数也声明为动作,以在 `robot_driver` 节点退出,该节点对应模拟在Webots接口中重设的时刻。 `robot_driver` 节点仍然有 `respawn` 设置为属性 `True`,以便它与 `ros2_control` 节点。

<span id="reset-handler-requiring-node-shutdown"></span>

## 重置处理器, 要求节点关闭

由于目前的ROS 2发射API,无法使发射文件中的重置工作在重启前需要关闭节点(例如. `Nav2` 或 时 间 `RViz`原因在于, ROS 2 目前不允许关闭发射文件中的特定节点。 有一个解决方案,但它需要在按下重置按钮后手动重启节点。

Webots需要在没有其他节点的特定发射文件中启动.

``` python
def generate_launch_description():
    # Starts Webots
    webots = WebotsLauncher(world=PathJoinSubstitution([package_dir, 'worlds', world]))

    return LaunchDescription([
        webots
    ])
```

需要从另一个进程启动第二个发射文件。这个发射文件包含所有其他节点,包括机器人控制器/插件,导航2节点,RViz,状态发布器等.

``` python
def generate_launch_description():
    robot_driver = WebotsController(
        robot_name='my_robot',
        parameters=[
            {'robot_description': robot_description_path}
        ]
    )

    ros_control_node = Node(
        package='controller_manager',
        executable='spawner',
        arguments=['diffdrive_controller']
    )

    nav2_node = IncludeLaunchDescription(
        PythonLaunchDescriptionSource(os.path.join(
            get_package_share_directory('nav2_bringup'), 'launch', 'bringup_launch.py')),
        launch_arguments=[
            ('map', nav2_map),
            ('params_file', nav2_params),
        ],
    )

    rviz = Node(
        package='rviz2',
        executable='rviz2',
        output='screen'
    )

    # Declare the handler that shuts all nodes down when robot_driver exits
    shutdown_handler = launch.actions.RegisterEventHandler(
        event_handler=launch.event_handlers.OnProcessExit(
            target_action=robot_driver,
            on_exit=[launch.actions.EmitEvent(event=launch.events.Shutdown())],
        )
    )

    return LaunchDescription([
        robot_driver,
        ros_control_node,
        nav2_node,
        rviz,
        shutdown_handler
    ])
```

第二个发射文件包含一个处理器,在驱动器节点退出时触发关机事件(模拟重设时就是这种情况),这个第二个发射文件在按下重置按钮后必须手动从命令行中重新启动.

<span id="summary"></span>

## 小结

在此教程中, 您学会了如何在使用 Webots 的机器人模拟中执行重置处理器 。 重置处理器允许您在 Webots 重置按钮时重新启动特定节点或执行附加动作 。 您根据模拟的复杂性和节点的要求, 探索了不同的方法 。
