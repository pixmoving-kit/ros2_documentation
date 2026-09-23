---
translation_status: machine_translated
source: Tutorials/Intermediate/Tf2/Writing-A-Tf2-Broadcaster-Py.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="writing-a-broadcaster-python"></span>

# 编写广播器（Python）

**目标：** 学习如何广播机器人状态到 tf2.

**教程级别：** 中级

**用时：** 15分钟

<span id="background"></span>

## 背景

在接下来的两个教程中,我们会写出代码来复制演示文稿 [tf2 介绍](Introduction-To-Tf2.md) 教程。在此之后,以下教程将侧重于扩展演示,使其具有更先进的 tf2 特性,包括在变换浏览和时间旅行中使用超时功能。

<span id="prerequisites"></span>

## 前提条件

这个教程假设你对ROS 2有工作知识,你已经完成了 [tf2 教程介绍](Introduction-To-Tf2.md) 财务报告和财务报告 [tf2 静态播音员辅导( Python)](Writing-A-Tf2-Static-Broadcaster-Py.md)。我们将重新使用 `learning_tf2_py` 软件包,从最后一个教程。

在之前的教程中,你学会了如何 [创建工作空间](../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md) 财务报告和财务报告 [创建软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md).

<span id="tasks"></span>

## 操作步骤

<span id="write-the-broadcaster-node"></span>

### 1 写入播音员节点

让我们首先创建源文件。请到 `learning_tf2_py` 我们在上一个教程中创建的软件包。 `src/learning_tf2_py/learning_tf2_py` 目录通过输入以下命令来下载实例播放器代码:

##### Linux

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_broadcaster.py
```

##### macOS

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_broadcaster.py
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_broadcaster.py -o turtle_tf2_broadcaster.py
```

或于权壳中:

``` console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/turtle_tf2_broadcaster.py -o turtle_tf2_broadcaster.py
```

现在打开名为 `turtle_tf2_broadcaster.py` 使用您首选的文本编辑器。

``` python
import math

from geometry_msgs.msg import TransformStamped

import numpy as np

import rclpy
from rclpy.node import Node

from tf2_ros import TransformBroadcaster

from turtlesim.msg import Pose


def quaternion_from_euler(ai, aj, ak):
    ai /= 2.0
    aj /= 2.0
    ak /= 2.0
    ci = math.cos(ai)
    si = math.sin(ai)
    cj = math.cos(aj)
    sj = math.sin(aj)
    ck = math.cos(ak)
    sk = math.sin(ak)
    cc = ci*ck
    cs = ci*sk
    sc = si*ck
    ss = si*sk

    q = np.empty((4, ))
    q[0] = cj*sc - sj*cs
    q[1] = cj*ss + sj*cc
    q[2] = cj*cs - sj*sc
    q[3] = cj*cc + sj*ss

    return q


class FramePublisher(Node):

    def __init__(self):
        super().__init__('turtle_tf2_frame_publisher')

        # Declare and acquire `turtlename` parameter
        self.turtlename = self.declare_parameter(
          'turtlename', 'turtle').get_parameter_value().string_value

        # Initialize the transform broadcaster
        self.tf_broadcaster = TransformBroadcaster(self)

        # Subscribe to a turtle{1}{2}/pose topic and call handle_turtle_pose
        # callback function on each message
        self.subscription = self.create_subscription(
            Pose,
            f'/{self.turtlename}/pose',
            self.handle_turtle_pose,
            1)
        self.subscription  # prevent unused variable warning

    def handle_turtle_pose(self, msg):
        t = TransformStamped()

        # Read message content and assign it to
        # corresponding tf variables
        t.header.stamp = self.get_clock().now().to_msg()
        t.header.frame_id = 'world'
        t.child_frame_id = self.turtlename

        # Turtle only exists in 2D, thus we get x and y translation
        # coordinates from the message and set the z coordinate to 0
        t.transform.translation.x = msg.x
        t.transform.translation.y = msg.y
        t.transform.translation.z = 0.0

        # For the same reason, turtle can only rotate around one axis
        # and this why we set rotation in x and y to 0 and obtain
        # rotation in z axis from the message
        q = quaternion_from_euler(0, 0, msg.theta)
        t.transform.rotation.x = q[0]
        t.transform.rotation.y = q[1]
        t.transform.rotation.z = q[2]
        t.transform.rotation.w = q[3]

        # Send the transformation
        self.tf_broadcaster.sendTransform(t)


def main():
    rclpy.init()
    node = FramePublisher()
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        pass

    rclpy.shutdown()
```

<span id="examine-the-code"></span>

#### 1.1 审查守则

现在,让我们看看与公布海龟的姿势相关的代码。 首先,我们定义并获得一个单一参数。 `turtlename`,它指定了龟名,例如: `turtle1` 或 时 间 `turtle2`.

``` python
self.turtlename = self.declare_parameter(
  'turtlename', 'turtle').get_parameter_value().string_value
```

之后, 节点订阅主题 `{self.turtlename}/pose` 运行函数 `handle_turtle_pose` 每一封来信上都写着

``` python
self .subscription = self.create_subscription(
    Pose,
    f'/{self.turtlename}/pose',
    self.handle_turtle_pose,
    1)
```

现在,我们创建一个 `TransformStamped` 对象并给出适当的元数据。

1.  我们需要给正在出版的变换图案一个时间戳, `self.get_clock().now()`中返回当前使用的时间。 `Node`.

2.  那么我们需要设定我们所创建的链接的父框架的名称,在这种情况下 `world`.

3.  最后,我们需要设定我们所创建的链接的儿童节点的名称,在这种情况下,这就是龟本身的名称。

龟的处理器功能将信息播放给龟的翻译和旋转,并将其出版为从框架的变换 `world` 创建框架 `turtleX`.

``` python
t = TransformStamped()

# Read message content and assign it to
# corresponding tf variables
t.header.stamp = self.get_clock().now().to_msg()
t.header.frame_id = 'world'
t.child_frame_id = self.turtlename
```

在这里,我们复制了来自3D龟姿势的信息进入3D变换.

``` python
# Turtle only exists in 2D, thus we get x and y translation
# coordinates from the message and set the z coordinate to 0
t.transform.translation.x = msg.x
t.transform.translation.y = msg.y
t.transform.translation.z = 0.0

# For the same reason, turtle can only rotate around one axis
# and this why we set rotation in x and y to 0 and obtain
# rotation in z axis from the message
q = quaternion_from_euler(0, 0, msg.theta)
t.transform.rotation.x = q[0]
t.transform.rotation.y = q[1]
t.transform.rotation.z = q[2]
t.transform.rotation.w = q[3]
```

最后,我们做了我们构建的转变, 并把它传递给 `sendTransform` 方法 `TransformBroadcaster` 那会照顾广播。

``` python
# Send the transformation
self.tf_broadcaster.sendTransform(t)
```

<span id="add-an-entry-point"></span>

#### 1.2 添加一个切入点

允许 `ros2 run` 命令来运行您的节点,您必须添加切入点到 `setup.py` 页:1 `src/learning_tf2_py` 目录).

将下行添加到 `'console_scripts':` 括号 :

``` python
'turtle_tf2_broadcaster = learning_tf2_py.turtle_tf2_broadcaster:main',
```

<span id="write-the-launch-file"></span>

### 2 写入发射文件

现在为此演示创建一个启动文件。 创建 `launch` 文件夹中 `src/learning_tf2_py` 目录。用您的文本编辑器创建新文件 `turtle_tf2_demo_launch` 带有扩展的 `.py`, `.xml`,或 `.yaml` 输入 `launch` 文件夹,并添加以下行:

##### XML 数据

<span id="turtle-tf2-demo-launch-xml"></span>

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <node pkg="turtlesim" exec="turtlesim_node" name="sim" />
  <node pkg="learning_tf2_py" exec="turtle_tf2_broadcaster" name="broadcaster1">
    <param name="turtlename" value="turtle1" />
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
```

##### Python

<span id="turtle-tf2-demo-launch-py"></span>

``` python
from launch import LaunchDescription
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
    ])
```

<span id="id1"></span>

#### 2.1 审查守则

让我们来检查一下发射文件的结构。每种格式都有自己设置发射文件的方法:

##### XML 数据

XML 启动文件从 XML 声明和根开始 `<launch>` 键。

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
```

##### 也门

YAML 启动文件从 YAML 版本声明开始, 以及一个 `launch:` 键。

``` yaml
%YAML 1.2
---
launch:
```

##### Python

在 Python 启动文件中,我们首先从 `launch` 财务报告和财务报告 `launch_ros` 软件包。应当指出, `launch` 是一个通用发射框架(而非ROS 2 具体内容),以及 `launch_ros` 有ROS 2 特殊的东西, 像节点,我们在这里导入。

``` python
from launch import LaunchDescription
from launch_ros.actions import Node
```

现在我们运行我们的节点 开始龟象模拟和广播 `turtle1` 状态到 tf2,使用我们 `turtle_tf2_broadcaster` 节点。

##### XML 数据

``` xml
  <node pkg="turtlesim" exec="turtlesim_node" name="sim" />
  <node pkg="learning_tf2_py" exec="turtle_tf2_broadcaster" name="broadcaster1">
    <param name="turtlename" value="turtle1" />
  </node>
```

##### 也门

``` yaml
  - node:
      pkg: "turtlesim"
      exec: "turtlesim_node"
      name: "sim"
  - node:
      pkg: "learning_tf2_py"
```

##### Python

``` python
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
    ])
```

<span id="add-dependencies"></span>

#### 2.2 增加依附关系

导航一个关卡返回 `learning_tf2_py` 目录,其中 `setup.py`, `setup.cfg`,以及 `package.xml` 文件已经找到 。

打开 `package.xml` 使用文本编辑器。添加以下与您的发射文件导入语句相对应的依赖性 :

``` xml
<exec_depend>launch</exec_depend>
<exec_depend>launch_ros</exec_depend>
```

这说明需要额外 `launch` 财务报告和财务报告 `launch_ros` 执行代码时的依赖性 。

确保保存文件 。

<span id="update-setup-py"></span>

#### 2.3 更新设置.py

重新打开 `setup.py` 并添加该行,使发射文件从 `launch/` 文件夹将安装。 `data_files` 字段现在应该是这样的:

``` python
data_files=[
    ...
    (os.path.join('share', package_name, 'launch'), glob('launch/*')),
],
```

在文件顶端添加适当的导入 :

``` python
import os
from glob import glob
```

您可以在下列情况下学习更多关于创建启动文件的知识: [此教程](../Launch/Creating-Launch-Files.md).

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

现在运行启动龟兹模拟节点的发射文件 `turtle_tf2_broadcaster` 节点 :

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

在第二个终端窗口中,以下命令:

``` console
$ ros2 run turtlesim turtle_teleop_key
```

你们现在可以看到,龟类模拟的开始 是一个可以控制龟类。

![](images/turtlesim_broadcast.png)

现在,用 `tf2_echo` 用于检查龟姿是否真的被播放到 tf2: 的工具 :

``` console
$ ros2 run tf2_ros tf2_echo world turtle1
```

这应该能让你看到第一只龟的姿势。用箭键绕龟行驶(确保您能够 `turtle_teleop_key` 终端窗口是活动窗口, 而不是模拟窗口。 在控制台输出中, 您可以看到类似此窗口的东西 :

``` console
At time 1714913843.708748879
- Translation: [4.541, 3.889, 0.000]
- Rotation: in Quaternion [0.000, 0.000, 0.999, -0.035]
- Rotation: in RPY (radian) [0.000, -0.000, -3.072]
- Rotation: in RPY (degree) [0.000, -0.000, -176.013]
- Matrix:
 -0.998  0.070  0.000  4.541
 -0.070 -0.998  0.000  3.889
  0.000  0.000  1.000  0.000
  0.000  0.000  0.000  1.000
```

如果你跑的话 `tf2_echo` 中间的变换 `world` 财务报告和财务报告 `turtle2`,你不应该看到变形,因为第二只龟还没有出现。但是,一旦我们把第二只龟加到下一个教程中,姿势就是: `turtle2` 将广播到 tf2。

<span id="summary"></span>

## 小结

在这个教程中,你学会了如何播放机器人的姿势(龟的姿势和方向)到 tf2,以及如何使用 `tf2_echo` 工具。要实际使用播放到 tf2 的变换,您应该转到下一个关于创建 [tf2 收听器](Writing-A-Tf2-Listener-Py.md).
