---
translation_status: machine_translated
source: Tutorials/Demos/Rosbag-with-ROS1-Bridge.rst
---

<span id="recording-and-playing-back-data-with-rosbag-using-the-ros-1-bridge"></span>

# 记录和播放回放数据 `rosbag` 使用 ROS 1 桥

这个教程是后续的 *ROS 1和ROS 2之间的桥梁通信* 演示可找到 [这儿](https://github.com/ros2/ros1_bridge/blob/master/README.md),下文假定您已经完成了该教程。

罗斯1_桥可以建自 [来源](../../How-To-Guides/Using-ros1_bridge-Jammy-upstream.md) 为这些例子。

下面是一系列额外的例子,如上述最后的例子。 *ROS 1和ROS 2之间的桥梁通信* 演示。

<span id="recording-topic-data-with-rosbag-and-ros-1-bridge"></span>

## 正在用 ROS Bag 和 ROS 1 桥记录主题数据

在这个例子中,我们将使用 `cam2image` 演示程序与 ROS 2 和一个 Python 脚本一起来模拟一个简单的 类似 Urpbot 的机器人传感器数据, 这样我们就可以将其连接到 ROS 1 并使用 rosbag 来记录它 。

我们首先要运行一个 ROS 1 `roscore` 在新外壳中 :

##### Linux

``` console
$ . /opt/ros/kinetic/setup.bash
$ roscore
```

##### macOS

``` console
$ . ~/ros_catkin_ws/install_isolated/setup.bash
$ rocore
```

然后,我们将运行 ROS 1 = ROS 2 `dynamic_bridge` 与 `--bridge-all-topics` 选项(这样我们就可以这样做) `rostopic list` 且在另一面的壳里看见他们。

> **说明**
>
> 如果您从源头安装了rosbridge, 则将路径相应调整为设置文件 : `. <workspace-with-bridge>/install/setup.bash`.

##### Linux

``` console
$ . /opt/ros/kinetic/setup.bash
$ . /opt/ros/ardent/setup.bash
$ export ROS_MASTER_URI=http://localhost:11311
$ ros2 run ros1_bridge dynamic_bridge --bridge-all-topics
```

##### macOS

``` console
$ . ~/ros_catkin_ws/install_isolated/setup.bash
$ . /opt/ros/ardent/setup.bash
$ export ROS_MASTER_URI=http://localhost:11311
$ ros2 run ros1_bridge dynamic_bridge --bridge-all-topics
```

------------------------------------------------------------------------

现在,我们可以启动ROS 2程序,以效仿我们的龟机器人。 `cam2image` 程序与 `-b` 选项, 这样它不需要相机工作。 在另一个外壳中:

``` console
$ . /opt/ros/ardent/setup.bash
$ ros2 run image_tools cam2image -- -b
```

TODO: 使用名称空格主题名称

然后我们用简单的 Python 脚本来模仿 `odom` 财务报告和财务报告 `imu_data` 科布基的话题,我会用更准确的 `~sensors/imu_data` imu 数据的主题名称, 但我们还没有在 ROS 2 中命名空间支持( 即将到来 ) 。 把这个脚本放到一个名为 ROS 2 的文件中 ! `emulate_kobuki_node.py`:

``` python
#!/usr/bin/env python3

import sys
import time

import rclpy

from nav_msgs.msg import Odometry
from sensor_msgs.msg import Imu

def main():
    rclpy.init(args=sys.argv)

    node = rclpy.create_node('emulate_kobuki_node')

    imu_publisher = node.create_publisher(Imu, 'imu_data')
    odom_publisher = node.create_publisher(Odometry, 'odom')

    imu_msg = Imu()
    odom_msg = Odometry()
    counter = 0
    while True:
        counter += 1
        now = time.time()
        if (counter % 50) == 0:
            odom_msg.header.stamp.sec = int(now)
            odom_msg.header.stamp.nanosec = int(now * 1e9) % 1000000000
            odom_publisher.publish(odom_msg)
        if (counter % 100) == 0:
            imu_msg.header.stamp.sec = int(now)
            imu_msg.header.stamp.nanosec = int(now * 1e9) % 1000000000
            imu_publisher.publish(imu_msg)
            counter = 0
        time.sleep(0.001)


if __name__ == '__main__':
    sys.exit(main())
```

您可以在新的 ROS 2 外壳中运行此 python 脚本 :

``` console
$ . /opt/ros/ardent/setup.bash
$ python3 emulate_kobuki_node.py
```

> **说明**
>
> 如果从源代码构建 ROS 2 , 则将路径相应调整为设置文件 : `<workspace-with-bridge>/install/setup.bash`.

------------------------------------------------------------------------

现在所有的数据源和动态桥正在运行,我们可以在一个新的ROS 1外壳中查看现有的话题:

##### Linux

``` console
$ . /opt/ros/kinetic/setup.bash
$ rostopic list
/image
/imu_data
/odom
/rosout
/rosout_agg
```

##### macOS

``` console
$ . ~/ros_catkin_ws/install_isolated/setup.bash
$ rostopic list
/image
/imu_data
/odom
/rosout
/rosout_agg
```

我们现在可以记录这个数据了 `rosbag record` 在同一贝壳中:

``` console
$ rosbag record /image /imu_data /odom
```

几秒钟后,你可以 `Ctrl-c` 编号 `rosbag` 命令并做一个 `ls -lh` 看文件有多大,你可能会看到这样的东西:

``` console
$ ls -lh
total 0
-rw-rw-r-- 1 william william  12M Feb 23 16:59 2017-02-23-16-59-47.bag
```

尽管文件名对于您的包来说是不同的(因为它来自日期和时间).

<span id="playing-back-topic-data-with-rosbag-and-ros-1-bridge"></span>

## 使用 rosbag 和 ROS 1 桥播放后题数据

现在我们有了一个包文件 你可以使用任何 ROS 1 工具 来回顾包文件,比如 `rosbag info <bag file>`, `rostopic list -b <bag file>`,或 `rqt_bag <bag file>`然而,我们也可以使用 ROS 2 播放回放袋数据。 `rosbag play` (原始内容存档于2019-09-31) (英语). ROS 1 + ROS 2 `dynamic_bridge`.

首先关闭您为上一个教程打开的所有 shells, 停止任何运行中的程序 。

然后在一个新的外壳开始 `roscore`:

##### Linux

``` console
$ . /opt/ros/kinetic/setup.bash
$ roscore
```

##### macOS

``` console
$ . ~/ros_catkin_ws/install_isolated/setup.bash
$ roscore
```

那就运行 `dynamic_bridge` 在另一个外壳中 :

##### Linux

``` console
$ . /opt/ros/kinetic/setup.bash
$ . /opt/ros/ardent/setup.bash
$ export ROS_MASTER_URI=http://localhost:11311
$ ros2 run ros1_bridge dynamic_bridge --bridge-all-topics
```

##### macOS

``` console
$ . ~/ros_catkin_ws/install_isolated/setup.bash
$ . /opt/ros/ardent/setup.bash
$ export ROS_MASTER_URI=http://localhost:11311
$ ros2 run ros1_bridge dynamic_bridge --bridge-all-topics
```

然后播放包数据回来 `rosbag play` 在另一个新外壳中,使用 `--loop` 选项, 这样我们不必继续为短袋重启 :

##### Linux

``` console
$ . /opt/ros/kinetic/setup.bash
$ rosbag play --loop path/to/bag_file
```

##### macOS

``` console
$ . ~/ros_catkin_ws/install_isolated/setup.bash
$ rosbag play --loop path/to/bag_file
```

> **说明**
>
> 确保替换 `path/to/bag_file` 与要播放的袋文件的路径。

------------------------------------------------------------------------

现在数据被播放回来 桥正在运行 我们可以看到数据通过ROS 2。

``` console
$ . /opt/ros/ardent/setup.bash
$ ros2 topic list
/clock
/image
/imu_data
/odom
/parameter_events
$ ros2 topic echo /odom
```

您也可以通过使用 `showimage` 工具 :

``` console
$ ros2 run image_tools showimage
```
