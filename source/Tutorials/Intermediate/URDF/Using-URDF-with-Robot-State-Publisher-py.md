---
translation_status: machine_translated
source: Tutorials/Intermediate/URDF/Using-URDF-with-Robot-State-Publisher-py.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-urdf-with-robot-state-publisher-python"></span> <span id="urdfplusrsppython"></span>

# 使用URDF为 `robot_state_publisher` (彼贤).

**目标：** 模拟一个以URDF为模型的行走机器人,并在Rviz查看.

**教程级别：** 中级

**用时：** 15分钟

<span id="background"></span>

## 背景

这个教程会教你如何塑造一个行走的机器人, 将状态公布为 [tf2](https://wiki.ros.org/tf2) 首先,我们创建描述机器人组装的URDF模型。接下来我们写一个节点,模拟运动并发布联合状态和变换。然后我们使用 `robot_state_publisher` 以发布整个机器人状态 `/tf2`.

![](images/r2d2_rviz_demo.gif) <span id="prerequisites"></span>

## 前提条件

- [rviz2 (中文(简体) ).](https://index.ros.org/p/rviz2/)

与往常一样, [您打开的每个新终端](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md).

<span id="tasks"></span>

## 操作步骤

<span id="create-a-package"></span>

### 1 创建软件包

创建目录 :

##### Linux

``` console
$ mkdir -p second_ros2_ws/src
```

##### macOS

``` console
$ mkdir -p second_ros2_ws/src
```

##### Windows

``` console
$ md second_ros2_ws/src
```

然后创建软件包 :

``` console
$ cd second_ros2_ws/src
$ ros2 pkg create --build-type ament_python --license Apache-2.0 urdf_tutorial_r2d2 --dependencies rclpy
$ cd urdf_tutorial_r2d2
```

你现在应该看看 `urdf_tutorial_r2d2` 文件夹。接下来将对它进行若干修改。

<span id="create-the-urdf-file"></span>

### 2 创建 URDF 文件

创建目录, 用于存储一些资产 :

##### Linux

``` console
$ mkdir -p urdf
```

##### macOS

``` console
$ mkdir -p urdf
```

##### Windows

``` console
$ md urdf
```

下载 [`URDF file`](documents/r2d2.urdf.xml) 并保存为 `second_ros2_ws/src/urdf_tutorial_r2d2/urdf/r2d2.urdf.xml`下载 [`Rviz configuration file`](documents/r2d2.rviz) 并保存为 `second_ros2_ws/src/urdf_tutorial_r2d2/urdf/r2d2.rviz`.

<span id="publish-the-state"></span>

### 3 公布国家

现在我们需要一种方法来说明机器人处于什么状态。要做到这一点,我们必须确定所有三个关节和整体的线程。

点燃您最喜欢的编辑器并粘贴以下代码 `second_ros2_ws/src/urdf_tutorial_r2d2/urdf_tutorial_r2d2/state_publisher.py`

``` python
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

### 4 创建发射文件

创建新 `second_ros2_ws/src/urdf_tutorial_r2d2/launch` 文件夹。打开编辑器并粘贴以下代码,保存为 `second_ros2_ws/src/urdf_tutorial_r2d2/launch/demo.launch.py`

``` python
from launch import LaunchDescription
from launch.actions import DeclareLaunchArgument
from launch.substitutions import FileContent, LaunchConfiguration, PathJoinSubstitution
from launch_ros.actions import Node
from launch_ros.substitutions import FindPackageShare


def generate_launch_description():
    use_sim_time = LaunchConfiguration('use_sim_time', default='false')
    urdf = FileContent(
        PathJoinSubstitution([FindPackageShare('urdf_tutorial_r2d2'), 'r2d2.urdf.xml']))

    return LaunchDescription([
        DeclareLaunchArgument(
            'use_sim_time',
            default_value='false',
            description='Use simulation (Gazebo) clock if true'),
        Node(
            package='robot_state_publisher',
            executable='robot_state_publisher',
            name='robot_state_publisher',
            output='screen',
            parameters=[{'use_sim_time': use_sim_time, 'robot_description': urdf}],
            arguments=[urdf]),
        Node(
            package='urdf_tutorial_r2d2',
            executable='state_publisher',
            name='state_publisher',
            output='screen'),
    ])
```

<span id="edit-the-setup-py-file"></span>

### 5 编辑设置.py文件

你必须告诉 **colcon** 构建如何安装您的 Python 软件包的工具。 编辑 `second_ros2_ws/src/urdf_tutorial_r2d2/setup.py` 文件如下:

- 包含这些导入语句

``` python
import os
from glob import glob
from setuptools import setup
from setuptools import find_packages
```

- 将这2行附加在内部 `data_files`

``` python
data_files=[
  ...
  (os.path.join('share', package_name, 'launch'), glob('launch/*')),
  (os.path.join('share', package_name), glob('urdf/*')),
],
```

- 修改 `entry_points` 表格,以便您日后从控制台运行“ state_publisher ”

``` python
'console_scripts': [
    'state_publisher = urdf_tutorial_r2d2.state_publisher:main'
],
```

保存该 `setup.py` 带有更改的文档。

<span id="install-the-package"></span>

### 6 安装软件包

``` console
$ cd second_ros2_ws
$ colcon build --symlink-install --packages-select urdf_tutorial_r2d2
```

来源设置文件 :

##### Linux

``` console
$ source install/setup.bash
```

##### macOS

``` console
$ source install/setup.bash
```

##### Windows

``` console
$ call install/setup.bat
```

<span id="view-the-results"></span>

### 7 查看结果

启动软件包

``` console
$ ros2 launch urdf_tutorial_r2d2 demo.launch.py
```

打开新终端, 使用 Rviz 运行

``` console
$ rviz2 -d `ros2 pkg prefix urdf_tutorial_r2d2 --share`/r2d2.rviz
```

见 [用户指南](http://wiki.ros.org/rviz/UserGuide) 详细介绍如何使用Rviz。

<span id="summary"></span>

## 小结

你创造了一个 `JointState` 出版商节点并结合 `robot_state_publisher` 用于模拟行走机器人。这些示例中使用的代码最初来自 [这儿](https://github.com/benbongalon/ros2-migration/tree/master/urdf_tutorial).

这一点的作者得到了肯定。 [ROS 1 教程](http://wiki.ros.org/urdf/Tutorials/Using%20urdf%20with%20robot_state_publisher) 从中重新使用某些内容。
