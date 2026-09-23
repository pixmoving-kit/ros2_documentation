<span id="using-urdf-with-robot-state-publisher-python"></span> <span id="urdfplusrsppython"></span>

# 将 URDF 与 robot_state_publisher 配合使用（Python）

**目标：** 仿真一个用 URDF 建模的行走机器人，并在 RViz 中查看。

**教程级别：** 中级

**预计耗时：** 15 分钟

<span id="background"></span>

## 背景

本教程介绍如何为行走机器人建模，以 [tf2](https://wiki.ros.org/tf2) 消息发布状态，并在 RViz 中查看仿真。首先创建描述机器人组成结构的 URDF 模型，然后编写模拟运动并发布 JointState 和变换的节点，再用 `robot_state_publisher` 将整个机器人的状态发布到 `/tf2`。

![](images/r2d2_rviz_demo.gif)

<span id="prerequisites"></span>

## 前提条件

- [rviz2](https://index.ros.org/p/rviz2/)

与往常一样，别忘记在[每个新打开的终端](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)中加载 ROS 2 环境。

<span id="tasks"></span>

## 任务

<span id="create-a-package"></span>

### 1 创建软件包

创建目录。

Linux：

```console
$ mkdir -p second_ros2_ws/src
```

macOS：

```console
$ mkdir -p second_ros2_ws/src
```

Windows：

```console
$ md second_ros2_ws/src
```

然后创建软件包：

```console
$ cd second_ros2_ws/src
$ ros2 pkg create --build-type ament_python --license Apache-2.0 urdf_tutorial_r2d2 --dependencies rclpy
$ cd urdf_tutorial_r2d2
```

现在应能看到 `urdf_tutorial_r2d2` 文件夹，接下来将对其进行多项修改。

<span id="create-the-urdf-file"></span>

### 2 创建 URDF 文件

创建存放资源的目录。

Linux：

```console
$ mkdir -p urdf
```

macOS：

```console
$ mkdir -p urdf
```

Windows：

```console
$ md urdf
```

下载 [URDF 文件](documents/r2d2.urdf.xml)，保存为 `second_ros2_ws/src/urdf_tutorial_r2d2/urdf/r2d2.urdf.xml`。下载 [RViz 配置文件](documents/r2d2.rviz)，保存为 `second_ros2_ws/src/urdf_tutorial_r2d2/urdf/r2d2.rviz`。

<span id="publish-the-state"></span>

### 3 发布状态

现在需要描述机器人当前的状态，为此必须指定三个关节以及整体里程计信息。

打开编辑器，将以下代码粘贴到 `second_ros2_ws/src/urdf_tutorial_r2d2/urdf_tutorial_r2d2/state_publisher.py`：

```python
from math import sin, cos, pi
import rclpy
from rclpy.node import Node
from rclpy.qos import QoSProfile
from geometry_msgs.msg import Quaternion
from sensor_msgs.msg import JointState
from tf2_ros import TransformBroadcaster, TransformStamped

class StatePublisher(Node):

    def __init__(self):
        rclpy.init()
        super().__init__('state_publisher')

        qos_profile = QoSProfile(depth=10)
        self.joint_pub = self.create_publisher(JointState, 'joint_states', qos_profile)
        self.broadcaster = TransformBroadcaster(self, qos=qos_profile)
        self.nodeName = self.get_name()
        self.get_logger().info("{0} started".format(self.nodeName))

        degree = pi / 180.0
        loop_rate = self.create_rate(30)

        # robot state
        tilt = 0.
        tinc = degree
        swivel = 0.
        angle = 0.
        height = 0.
        hinc = 0.005

        # message declarations
        odom_trans = TransformStamped()
        odom_trans.header.frame_id = 'odom'
        odom_trans.child_frame_id = 'axis'
        joint_state = JointState()

        try:
            while rclpy.ok():
                rclpy.spin_once(self)

                # update joint_state
                now = self.get_clock().now()
                joint_state.header.stamp = now.to_msg()
                joint_state.name = ['swivel', 'tilt', 'periscope']
                joint_state.position = [swivel, tilt, height]

                # update transform
                # (moving in a circle with radius=2)
                odom_trans.header.stamp = now.to_msg()
                odom_trans.transform.translation.x = cos(angle)*2
                odom_trans.transform.translation.y = sin(angle)*2
                odom_trans.transform.translation.z = 0.7
                odom_trans.transform.rotation = \
                    euler_to_quaternion(0, 0, angle + pi/2) # roll,pitch,yaw

                # send the joint state and transform
                self.joint_pub.publish(joint_state)
                self.broadcaster.sendTransform(odom_trans)

                # Create new robot state
                tilt += tinc
                if tilt < -0.5 or tilt > 0.0:
                    tinc *= -1
                height += hinc
                if height > 0.2 or height < 0.0:
                    hinc *= -1
                swivel += degree
                angle += degree/4

                # This will adjust as needed per iteration
                loop_rate.sleep()

        except KeyboardInterrupt:
            pass

def euler_to_quaternion(roll, pitch, yaw):
    qx = sin(roll/2) * cos(pitch/2) * cos(yaw/2) - cos(roll/2) * sin(pitch/2) * sin(yaw/2)
    qy = cos(roll/2) * sin(pitch/2) * cos(yaw/2) + sin(roll/2) * cos(pitch/2) * sin(yaw/2)
    qz = cos(roll/2) * cos(pitch/2) * sin(yaw/2) - sin(roll/2) * sin(pitch/2) * cos(yaw/2)
    qw = cos(roll/2) * cos(pitch/2) * cos(yaw/2) + sin(roll/2) * sin(pitch/2) * sin(yaw/2)
    return Quaternion(x=qx, y=qy, z=qz, w=qw)

def main():
    node = StatePublisher()

if __name__ == '__main__':
    main()
```

<span id="create-a-launch-file"></span>

### 4 创建启动文件

创建 `second_ros2_ws/src/urdf_tutorial_r2d2/launch` 文件夹，将[启动文件示例](launch/demo_launch.py)保存为 `second_ros2_ws/src/urdf_tutorial_r2d2/launch/demo.launch.py`。

<span id="edit-the-setup-py-file"></span>

### 5 编辑 setup.py

必须告诉 **colcon** 如何安装 Python 包。编辑 `second_ros2_ws/src/urdf_tutorial_r2d2/setup.py`：

添加以下导入语句：

```python
import os
from glob import glob
from setuptools import setup
from setuptools import find_packages
```

在 `data_files` 中添加以下两行：

```python
data_files=[
  ...
  (os.path.join('share', package_name, 'launch'), glob('launch/*')),
  (os.path.join('share', package_name), glob('urdf/*')),
],
```

修改 `entry_points`，以便之后从控制台运行 `state_publisher`：

```python
'console_scripts': [
    'state_publisher = urdf_tutorial_r2d2.state_publisher:main'
],
```

保存修改后的 `setup.py`。

<span id="install-the-package"></span>

### 6 安装软件包

```console
$ cd second_ros2_ws
$ colcon build --symlink-install --packages-select urdf_tutorial_r2d2
```

加载环境设置文件。

Linux：

```console
$ source install/setup.bash
```

macOS：

```console
$ source install/setup.bash
```

Windows：

```console
$ call install/setup.bat
```

<span id="view-the-results"></span>

### 7 查看结果

启动软件包：

```console
$ ros2 launch urdf_tutorial_r2d2 demo.launch.py
```

打开新终端并运行 RViz：

```console
$ rviz2 -d `ros2 pkg prefix urdf_tutorial_r2d2 --share`/r2d2.rviz
```

RViz 的使用方法见[用户指南](http://wiki.ros.org/rviz/UserGuide)。

<span id="summary"></span>

## 小结

你已创建一个 `JointState` 发布节点，并将它与 `robot_state_publisher` 配合使用，仿真了一个行走机器人。示例代码最初来自[此仓库](https://github.com/benbongalon/ros2-migration/tree/master/urdf_tutorial)。

本教程复用了部分 [ROS 1 教程](http://wiki.ros.org/urdf/Tutorials/Using%20urdf%20with%20robot_state_publisher)内容，感谢原作者。
