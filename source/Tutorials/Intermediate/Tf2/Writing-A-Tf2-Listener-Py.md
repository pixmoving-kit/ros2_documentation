<span id="writing-a-listener-python"></span>

# 编写监听器（Python）

**目标：** 学习通过 tf2 获取坐标系变换。

**教程级别：** 中级

**预计耗时：** 10 分钟

<span id="background"></span>

## 背景

此前已经创建 tf2 广播器来发布海龟位姿。本教程创建监听器，开始使用这些变换。

<span id="prerequisites"></span>

## 前提条件

应已完成[静态广播器](Writing-A-Tf2-Static-Broadcaster-Py.md)和[广播器](Writing-A-Tf2-Broadcaster-Py.md)教程。本篇继续在此前的 `learning_tf2_py` 包中开发。

<span id="tasks"></span>

## 任务

<span id="write-the-listener-node"></span>

### 1 编写监听器节点

进入 `src/learning_tf2_py/learning_tf2_py`，下载监听器源码。

Linux：

```console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_listener.py
```

macOS：

```console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_listener.py
```

Windows 命令提示符：

```console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_listener.py -o turtle_tf2_listener.py
```

或 PowerShell：

```console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_listener.py -o turtle_tf2_listener.py
```

用编辑器打开 `turtle_tf2_listener.py`：

```python
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

#### 1.1 分析代码

生成海龟所用服务的工作原理见[编写简单服务端和客户端](../../Beginner-Client-Libraries/Writing-A-Simple-Py-Service-And-Client.md)。

下面重点介绍获取坐标系变换的代码。`tf2_ros` 提供 `TransformListener`，简化接收变换的操作：

```python
from tf2_ros.transform_listener import TransformListener
```

创建监听器后，它就会开始接收网络中的 tf2 变换，缓存最长 10 秒的数据：

```python
self.tf_listener = TransformListener(self.tf_buffer, self)
```



查询特定变换时，向 `lookup_transform` 传入目标坐标系、源坐标系和所需时间。传入 `rclpy.time.Time()` 即获取最新可用变换。用异常处理块包裹查询，以处理可能发生的异常：

```python
t = self.tf_buffer.lookup_transform(
    to_frame_rel,
    from_frame_rel,
    rclpy.time.Time())
```



<span id="add-an-entry-point"></span>

#### 1.2 添加入口点

为了让 `ros2 run` 能运行节点，在 `src/learning_tf2_py/setup.py` 的 `'console_scripts':` 方括号内加入：

```python
'turtle_tf2_listener = learning_tf2_py.turtle_tf2_listener:main',
```

<span id="update-the-launch-file"></span>

### 2 更新启动文件

打开 `src/learning_tf2_py/launch` 中的 `turtle_tf2_demo_launch.xml`、`.yaml` 或 `.py`，加入两个新节点、一个启动参数，以及所需导入。更新后的完整示例：

<span id="turtle-tf2-demo-launch-xml"></span>

- [XML](launch/listener_py_launch.xml)

<span id="turtle-tf2-demo-launch-yaml"></span>

- [YAML](launch/listener_py_launch.yaml)

<span id="turtle-tf2-demo-launch-py"></span>

- [Python](launch/listener_py_launch.py)

这会声明 `target_frame` 启动参数，为即将生成的第二只海龟启动广播器，并启动订阅这些变换的监听器。

<span id="build"></span>

### 3 构建

在工作空间根目录运行 `rosdep` 检查依赖。

Linux：

```console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

本教程的 macOS 和 Windows 流程需自行安装 `geometry_msgs`、`turtlesim`，因为此处的 rosdep 步骤仅用于 Linux。

仍在工作空间根目录构建。

Linux：

```console
$ colcon build --packages-select learning_tf2_py
```

macOS：

```console
$ colcon build --packages-select learning_tf2_py
```

Windows：

```console
$ colcon build --merge-install --packages-select learning_tf2_py
```

打开新终端，进入工作空间根目录并加载环境。

Linux：

```console
$ . install/setup.bash
```

macOS：

```console
$ . install/setup.bash
```

Windows 命令提示符：

```console
$ call install\setup.bat
```

或 PowerShell：

```console
$ .\install\setup.ps1
```

<span id="run"></span>

### 4 运行

启动完整海龟示例。

XML：

```console
$ ros2 launch learning_tf2_py turtle_tf2_demo_launch.xml
```

YAML：

```console
$ ros2 launch learning_tf2_py turtle_tf2_demo_launch.yaml
```

Python：

```console
$ ros2 launch learning_tf2_py turtle_tf2_demo_launch.py
```

仿真中应出现两只海龟。在第二个终端运行：

```console
$ ros2 run turtlesim turtle_teleop_key
```

让终端而非仿真窗口处于焦点，用方向键控制第一只海龟，应看到第二只跟随它。

<span id="summary"></span>

## 小结

本教程介绍了通过 tf2 获取坐标系变换的方法，并完成了你在 [tf2 入门](Introduction-To-Tf2.md)中体验过的海龟仿真示例。
