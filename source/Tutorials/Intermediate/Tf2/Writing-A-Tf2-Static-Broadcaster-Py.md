<span id="writing-a-static-broadcaster-python"></span>

# 编写静态广播器（Python）

**目标：** 学习向 tf2 广播静态坐标系。

**教程级别：** 中级

**预计耗时：** 15 分钟

<span id="background"></span>

## 背景

静态变换用于描述机器人基座与传感器或不动部件之间的关系。例如，在以激光扫描器中心为原点的坐标系中理解扫描测量值最为方便。

这是介绍静态变换基础的独立教程，分为两部分：先编写代码发布静态变换，再介绍 `tf2_ros` 中的命令行工具 `static_transform_publisher`。

后面两篇教程会编写代码，复现 [tf2 入门](Introduction-To-Tf2.md)中的示例，再进一步扩展更高级的 tf2 功能。

<span id="prerequisites"></span>

## 前提条件

应已学习[创建工作空间](../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md)和[创建软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md)。

<span id="tasks"></span>

## 任务

<span id="create-a-package"></span>

### 1 创建软件包

创建本篇及后续教程使用的 `learning_tf2_py` 包，它依赖 `geometry_msgs`、`python3-numpy`、`rclpy`、`tf2_ros_py` 和 `turtlesim`。完整代码见[源文件](https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/static_turtle_tf2_broadcaster.py)。

打开新终端，[加载 ROS 2 环境](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)，进入工作空间的 `src` 目录并创建软件包：

```console
$ ros2 pkg create --build-type ament_python --license Apache-2.0 -- learning_tf2_py
```

终端会确认 `learning_tf2_py` 及所需文件和目录已创建。

<span id="write-the-static-broadcaster-node"></span>

### 2 编写静态广播器节点

在 `src/learning_tf2_py/learning_tf2_py` 目录中下载示例源码。

Linux：

```console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/static_turtle_tf2_broadcaster.py
```

macOS：

```console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/static_turtle_tf2_broadcaster.py
```

Windows 命令提示符：

```console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/static_turtle_tf2_broadcaster.py -o static_turtle_tf2_broadcaster.py
```

或 PowerShell：

```console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/static_turtle_tf2_broadcaster.py -o static_turtle_tf2_broadcaster.py
```

用编辑器打开 `static_turtle_tf2_broadcaster.py`：

```python
import math
import sys

from geometry_msgs.msg import TransformStamped

import numpy as np

import rclpy
from rclpy.node import Node

from tf2_ros.static_transform_broadcaster import StaticTransformBroadcaster


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


class StaticFramePublisher(Node):
    """
    Broadcast transforms that never change.

    This example publishes transforms from `world` to a static turtle frame.
    The transforms are only published once at startup, and are constant for all
    time.
    """

    def __init__(self, transformation):
        super().__init__('static_turtle_tf2_broadcaster')

        self.tf_static_broadcaster = StaticTransformBroadcaster(self)

        # Publish static transforms once at startup
        self.make_transforms(transformation)

    def make_transforms(self, transformation):
        t = TransformStamped()

        t.header.stamp = self.get_clock().now().to_msg()
        t.header.frame_id = 'world'
        t.child_frame_id = transformation[1]

        t.transform.translation.x = float(transformation[2])
        t.transform.translation.y = float(transformation[3])
        t.transform.translation.z = float(transformation[4])
        quat = quaternion_from_euler(
            float(transformation[5]), float(transformation[6]), float(transformation[7]))
        t.transform.rotation.x = quat[0]
        t.transform.rotation.y = quat[1]
        t.transform.rotation.z = quat[2]
        t.transform.rotation.w = quat[3]

        self.tf_static_broadcaster.sendTransform(t)


def main():
    logger = rclpy.logging.get_logger('logger')

    # obtain parameters from command line arguments
    if len(sys.argv) != 8:
        logger.info('Invalid number of parameters. Usage: \n'
                    '$ ros2 run learning_tf2_py static_turtle_tf2_broadcaster'
                    'child_frame_name x y z roll pitch yaw')
        sys.exit(1)

    if sys.argv[1] == 'world':
        logger.info('Your static turtle name cannot be "world"')
        sys.exit(2)

    # pass parameters and initialize node
    rclpy.init()
    node = StaticFramePublisher(sys.argv)
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        pass

    rclpy.shutdown()
```



<span id="examine-the-code"></span>

#### 2.1 分析代码

下面重点介绍向 tf2 发布海龟静态位姿的部分。首先引入 `TransformStamped` 消息类型，用于向变换树发布消息：

```python
from geometry_msgs.msg import TransformStamped
```

再引入 `rclpy`，以使用其节点类：

```python
import rclpy
from rclpy.node import Node
```

从 `tf2_ros` 导入 `StaticTransformBroadcaster`，以方便地发布静态变换。

```python
from tf2_ros.static_transform_broadcaster import StaticTransformBroadcaster
```

`StaticFramePublisher` 构造函数将节点名设为 `static_turtle_tf2_broadcaster`，然后创建 `StaticTransformBroadcaster`，在启动时发送一次静态变换。

```python
self.tf_static_broadcaster = StaticTransformBroadcaster(self)
self.make_transforms(transformation)
```

创建待发送的 `TransformStamped` 对象。在填写实际变换值之前，先设置元数据：

1. 用 `self.get_clock().now()` 设置当前时间戳。
2. 将父坐标系设为 `world`。
3. 设置子坐标系名称。

```python
t = TransformStamped()

t.header.stamp = self.get_clock().now().to_msg()
t.header.frame_id = 'world'
t.child_frame_id = transformation[1]
```

填入海龟的六维位姿，即平移和旋转：

```python
t.transform.translation.x = float(transformation[2])
t.transform.translation.y = float(transformation[3])
t.transform.translation.z = float(transformation[4])
quat = quaternion_from_euler(
    float(transformation[5]), float(transformation[6]), float(transformation[7]))
t.transform.rotation.x = quat[0]
t.transform.rotation.y = quat[1]
t.transform.rotation.z = quat[2]
t.transform.rotation.w = quat[3]
```

最后通过 `sendTransform()` 广播静态变换：

```python
self.tf_static_broadcaster.sendTransform(t)
```

<span id="update-package-xml"></span>

#### 2.2 更新 package.xml

返回 `src/learning_tf2_py`，其中已有 `setup.py`、`setup.cfg` 和 `package.xml`。用编辑器打开 `package.xml`，按照[创建软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md)教程，填写 `<description>`、`<maintainer>` 和 `<license>`：

```xml
<description>Learning tf2 with rclpy</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

在上述内容后加入对应导入语句的运行依赖：

```xml
<exec_depend>geometry_msgs</exec_depend>
<exec_depend>python3-numpy</exec_depend>
<exec_depend>rclpy</exec_depend>
<exec_depend>tf2_ros_py</exec_depend>
<exec_depend>turtlesim</exec_depend>
```

这声明了执行代码所需的 `geometry_msgs`、`python3-numpy`、`rclpy`、`tf2_ros_py` 和 `turtlesim`。保存文件。

<span id="add-an-entry-point"></span>

#### 2.3 添加入口点

为了让 `ros2 run` 能运行节点，在 `src/learning_tf2_py/setup.py` 的 `'console_scripts':` 方括号内加入：

```python
'static_turtle_tf2_broadcaster = learning_tf2_py.static_turtle_tf2_broadcaster:main',
```

<span id="build"></span>

### 3 构建

构建前，建议在工作空间根目录运行 `rosdep` 检查缺失依赖。

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

运行静态广播器节点：

```console
$ ros2 run learning_tf2_py static_turtle_tf2_broadcaster mystaticturtle 0 0 1 0 0 0
```

这将发布 `mystaticturtle` 的位姿，使其位于地面上方 1 米。

查看 `tf_static` 话题以验证发布成功，正常情况下应看到一个静态变换：

```console
$ ros2 topic echo /tf_static
transforms:
- header:
   stamp:
      sec: 1622908754
      nanosec: 208515730
   frame_id: world
child_frame_id: mystaticturtle
transform:
   translation:
      x: 0.0
      y: 0.0
      z: 1.0
   rotation:
      x: 0.0
      y: 0.0
      z: 0.0
      w: 1.0
```

<span id="the-proper-way-to-publish-static-transforms"></span>

## 推荐的静态变换发布方式

本教程通过编写代码展示 `StaticTransformBroadcaster` 的用法。实际开发中，通常无须自己编写这些代码，可直接使用 `tf2_ros` 提供的 `static_transform_publisher`，既能从命令行运行，也可作为节点加入启动文件。

以下命令发布 `world` 和 `mystaticturtle` 之间的静态变换：z 方向偏移 1 米，无旋转。ROS 2 中 roll、pitch、yaw 分别表示绕 x、y、z 轴的旋转，单位为弧度。

```console
$ ros2 run tf2_ros static_transform_publisher --x 0 --y 0 --z 1 --yaw 0 --pitch 0 --roll 0 --frame-id world --child-frame-id mystaticturtle
```

下面使用四元数表示旋转，发布相同的变换：

```console
$ ros2 run tf2_ros static_transform_publisher --x 0 --y 0 --z 1 --qx 0 --qy 0 --qz 0 --qw 1 --frame-id world --child-frame-id mystaticturtle
```

启动文件中的使用示例：

- [XML](launch/static_transform_publisher_launch.xml)
- [YAML](launch/static_transform_publisher_launch.yaml)
- [Python](launch/static_transform_publisher_launch.py)

除 `--frame-id` 和 `--child-frame-id` 外，其余参数均可省略。未指定的选项按单位变换的相应分量处理。

<span id="summary"></span>

## 小结

本教程介绍了如何用静态变换定义坐标系间固定的关系，例如 `mystaticturtle` 相对于 `world` 的关系；也介绍了将激光扫描器等传感器数据关联到公共坐标系的用途。你编写了静态变换发布节点，并学习了通过 `static_transform_publisher` 和启动文件发布所需变换。
