---
translation_status: machine_translated
source: Tutorials/Intermediate/Tf2/Writing-A-Tf2-Listener-Py.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="writing-a-listener-python"></span>

# 编写监听器（Python）

**目标：** 学习如何使用 tf2 来获取帧变换的存取.

**教程级别：** 中级

**用时：** 10分钟

<span id="background"></span>

## 背景

在之前的教程中,我们创建了tf2广播机,以发布龟到tf2的姿势.

在此教程中, 我们将创建一个 tf2 的听众开始使用 tf2 。

<span id="prerequisites"></span>

## 前提条件

此教程假设您已完成 [tf2 静态播音员辅导( Python)](Writing-A-Tf2-Static-Broadcaster-Py.md) 财务报告和财务报告 [tf2 播音员辅导( Python)](Writing-A-Tf2-Broadcaster-Py.md)。在之前的教程中,我们创建了一个 `learning_tf2_py` 我们将继续从这个角度开展工作。

<span id="tasks"></span>

## 操作步骤

<span id="write-the-listener-node"></span>

### 1 写入收听器节点

让我们首先创建源文件。请到 `learning_tf2_py` 我们在上一个教程中创建的软件包。 `src/learning_tf2_py/learning_tf2_py` 目录通过输入以下命令下载示例听器代码 :

##### Linux

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_listener.py
```

##### macOS

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_listener.py
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_listener.py -o turtle_tf2_listener.py
```

或于权壳中:

``` console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_listener.py -o turtle_tf2_listener.py
```

现在打开名为 `turtle_tf2_listener.py` 使用您首选的文本编辑器。

``` python
import math

from geometry_msgs.msg import Twist

import rclpy
from rclpy.node import Node

from tf2_ros import TransformException
from tf2_ros.buffer import Buffer
from tf2_ros.transform_listener import TransformListener

from turtlesim.srv import Spawn


class FrameListener(Node):

    def __init__(self):
        super().__init__('turtle_tf2_frame_listener')

        # Declare and acquire `target_frame` parameter
        self.target_frame = self.declare_parameter(
          'target_frame', 'turtle1').get_parameter_value().string_value

        self.tf_buffer = Buffer()
        self.tf_listener = TransformListener(self.tf_buffer, self)

        # Create a client to spawn a turtle
        self.spawner = self.create_client(Spawn, 'spawn')
        # Boolean values to store the information
        # if the service for spawning turtle is available
        self.turtle_spawning_service_ready = False
        # if the turtle was successfully spawned
        self.turtle_spawned = False

        # Create turtle2 velocity publisher
        self.publisher = self.create_publisher(Twist, 'turtle2/cmd_vel', 1)

        # Call on_timer function every second
        self.timer = self.create_timer(1.0, self.on_timer)

    def on_timer(self):
        # Store frame names in variables that will be used to
        # compute transformations
        from_frame_rel = self.target_frame
        to_frame_rel = 'turtle2'

        if self.turtle_spawning_service_ready:
            if self.turtle_spawned:
                # Look up for the transformation between target_frame and turtle2 frames
                # and send velocity commands for turtle2 to reach target_frame
                try:
                    t = self.tf_buffer.lookup_transform(
                        to_frame_rel,
                        from_frame_rel,
                        rclpy.time.Time())
                except TransformException as ex:
                    self.get_logger().info(
                        f'Could not transform {to_frame_rel} to {from_frame_rel}: {ex}')
                    return

                msg = Twist()
                scale_rotation_rate = 1.0
                msg.angular.z = scale_rotation_rate * math.atan2(
                    t.transform.translation.y,
                    t.transform.translation.x)

                scale_forward_speed = 0.5
                msg.linear.x = scale_forward_speed * math.sqrt(
                    t.transform.translation.x ** 2 +
                    t.transform.translation.y ** 2)

                self.publisher.publish(msg)
            else:
                if self.result.done():
                    self.get_logger().info(
                        f'Successfully spawned {self.result.result().name}')
                    self.turtle_spawned = True
                else:
                    self.get_logger().info('Spawn is not finished')
        else:
            if self.spawner.service_is_ready():
                # Initialize request with turtle name and coordinates
                # Note that x, y and theta are defined as floats in turtlesim/srv/Spawn
                request = Spawn.Request()
                request.name = 'turtle2'
                request.x = float(4)
                request.y = float(2)
                request.theta = float(0)
                # Call request
                self.result = self.spawner.call_async(request)
                self.turtle_spawning_service_ready = True
            else:
                # Check if the service is ready
                self.get_logger().info('Service is not ready')


def main():
    rclpy.init()
    node = FrameListener()
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        pass

    rclpy.shutdown()
```

<span id="examine-the-code"></span>

#### 1.1 审查守则

为了了解产卵海龟背后的服务如何运作,请参见: [写入简单的服务和客户端( Python)](../../Beginner-Client-Libraries/Writing-A-Simple-Py-Service-And-Client.md) 教学。

现在,让我们看看与获取帧转换相关的代码。 `tf2_ros` 软件包提供一种执行 `TransformListener` 帮助完成接受改造的任务。

``` python
from tf2_ros.transform_listener import TransformListener
```

在这里,我们创建一个 `TransformListener` 对象。一旦创建了监听器,它就会开始接收Tf2在电线上的变换,并缓冲它们长达10秒。

``` python
self.tf_listener = TransformListener(self.tf_buffer, self)
```

最后,我们向听众询问一个具体的转变。 `lookup_transform` 使用下列参数的方法:

1.  目标框架

2.  来源框架

3.  我们想要改变的时刻

提供 `rclpy.time.Time()` 将会让我们得到最新的变换。所有这一切都被包在一个例外的尝试区块中,以便处理可能的例外。

``` python
t = self.tf_buffer.lookup_transform(
    to_frame_rel,
    from_frame_rel,
    rclpy.time.Time())
```

<span id="add-an-entry-point"></span>

#### 1.2 添加一个切入点

允许 `ros2 run` 命令来运行您的节点,您必须添加切入点到 `setup.py` 页:1 `src/learning_tf2_py` 目录).

将下行添加到 `'console_scripts':` 括号 :

``` python
'turtle_tf2_listener = learning_tf2_py.turtle_tf2_listener:main',
```

<span id="update-the-launch-file"></span>

### 2 更新发射文件

打开所谓的发射文件 `turtle_tf2_demo_launch` 带有扩展的 `.py`, `.xml`,或 `.yaml` 输入 `src/learning_tf2_py/launch` 带有文本编辑器的目录, 在发射描述中添加两个新的节点, 添加发射参数, 并添加导入。 由此生成的文件应该看起来像 :

##### XML 数据

<span id="turtle-tf2-demo-launch-xml"></span>

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <node pkg="turtlesim" exec="turtlesim_node" name="sim" />
  <node pkg="learning_tf2_py" exec="turtle_tf2_broadcaster" name="broadcaster1">
    <param name="turtlename" value="turtle1" />
  </node>
  <arg name="target_frame" default="turtle1" description="Target frame name." />
  <node pkg="learning_tf2_py" exec="turtle_tf2_broadcaster" name="broadcaster2">
    <param name="turtlename" value="turtle2" />
  </node>
  <node pkg="learning_tf2_py" exec="turtle_tf2_listener" name="listener">
    <param name="target_frame" value="$(var target_frame)" />
  </node>
</launch>
```

##### 也门

<span id="turtle-tf2-demo-launch-yaml"></span>

``` yaml
%YAML 1.2
---
launch:
  - node:
      pkg: "turtlesim"
      exec: "turtlesim_node"
      name: "sim"
  - node:
      pkg: "learning_tf2_py"
      exec: "turtle_tf2_broadcaster"
      name: "broadcaster1"
      param:
      - name: "turtlename"
        value: "turtle1"
  - arg:
      name: "target_frame"
      default: "turtle1"
      description: "Target frame name."
  - node:
      pkg: "learning_tf2_py"
      exec: "turtle_tf2_broadcaster"
      name: "broadcaster2"
      param:
      - name: "turtlename"
        value: "turtle2"
  - node:
      pkg: "learning_tf2_py"
      exec: "turtle_tf2_listener"
      name: "listener"
      param:
      - name: "target_frame"
        value: "$(var target_frame)"
```

##### Python

<span id="turtle-tf2-demo-launch-py"></span>

``` python
from launch import LaunchDescription
from launch.actions import DeclareLaunchArgument
from launch.substitutions import LaunchConfiguration

from launch_ros.actions import Node


def generate_launch_description():
    return LaunchDescription([
        Node(
            package='turtlesim',
            executable='turtlesim_node',
            name='sim'
        ),
        Node(
            package='learning_tf2_py',
            executable='turtle_tf2_broadcaster',
            name='broadcaster1',
            parameters=[
                {'turtlename': 'turtle1'}
            ]
        ),
        DeclareLaunchArgument(
            'target_frame', default_value='turtle1',
            description='Target frame name.'
        ),
        Node(
            package='learning_tf2_py',
            executable='turtle_tf2_broadcaster',
            name='broadcaster2',
            parameters=[
                {'turtlename': 'turtle2'}
            ]
        ),
        Node(
            package='learning_tf2_py',
            executable='turtle_tf2_listener',
            name='listener',
            parameters=[
                {'target_frame': LaunchConfiguration('target_frame')}
            ]
        ),
    ])
```

这个将宣布 `target_frame` 启动辩论,启动第二只乌龟的播音员, 我们将产卵和听众, 同意这些转变。

<span id="build"></span>

### 3 构建

运行 `rosdep` 在工作空间的根中检查缺失的依赖性。

##### Linux

``` console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

##### macOS

rosdep 只运行在 Linux 上, 因此您需要安装 `geometry_msgs` 财务报告和财务报告 `turtlesim` 依附关系

##### Windows

rosdep 只运行在 Linux 上, 因此您需要安装 `geometry_msgs` 财务报告和财务报告 `turtlesim` 依附关系

仍然在工作区根部,构建您的软件包:

##### Linux

``` console
$ colcon build --packages-select learning_tf2_py
```

##### macOS

``` console
$ colcon build --packages-select learning_tf2_py
```

##### Windows

``` console
$ colcon build --merge-install --packages-select learning_tf2_py
```

打开新终端, 导航到您工作空间的根, 并源代码设置文件 :

##### Linux

``` console
$ . install/setup.bash
```

##### macOS

``` console
$ . install/setup.bash
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ call install\setup.bat
```

或于权壳中:

``` console
$ .\install\setup.ps1
```

<span id="run"></span>

### 4 运行

现在,你准备开始你的全龟演示:

##### XML 数据

``` console
$ ros2 launch learning_tf2_py turtle_tf2_demo_launch.xml
```

##### 也门

``` console
$ ros2 launch learning_tf2_py turtle_tf2_demo_launch.yaml
```

##### Python

``` console
$ ros2 launch learning_tf2_py turtle_tf2_demo_launch.py
```

您应该看到两只龟的图案。 在第二个终端窗口中, 命令如下 :

``` console
$ ros2 run turtlesim turtle_teleop_key
```

看事情是否可行, 请用箭头键在第一只龟周围开车( 确定您的终端窗口是活动的, 而不是模拟窗口) , 而您会看到第二只龟在第一只龟之后!

<span id="summary"></span>

## 小结

在此教程中, 您学会了如何使用 tf2 来访问框架转换 。 您也已完成了您自己首次尝试的龟兹演示文件的编写工作 。 [tf2 介绍](Introduction-To-Tf2.md) 教学。
