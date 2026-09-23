---
translation_status: machine_translated
source: Tutorials/Intermediate/Tf2/Adding-A-Frame-Py.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="adding-a-frame-python"></span>

# 添加坐标系（Python）

**目标：** 学习如何在 tf2 中添加一个额外的框架.

**教程级别：** 中级

**用时：** 15分钟

<span id="background"></span>

## 背景

在之前的教程中,我们通过写一个 [tf2 广播机](Writing-A-Tf2-Broadcaster-Py.md) 备注a [tf2 收听器](Writing-A-Tf2-Listener-Py.md)。此教程将教你如何在变换树上添加额外的固定和动态框架。事实上,在 tf2 中添加一个框架与创建 tf2 广播机非常相似,但这个例子将显示 tf2 的一些附加功能.

对于许多与变换有关的任务,比较容易在局部帧内思考. 例如,在激光扫描仪中心一个帧内进行激光扫描测量是最容易解释的. tf2允许您定义每个传感器,链接,或连接在您的系统中的局部帧. tf2在从一个帧转换到另一个帧时,会照顾所有引入的隐藏中间帧变换.

<span id="tf2-tree"></span>

## tf2 树

tf2 构建了框架的树状结构,因此不允许在框架结构中有一个闭环。这意味着框架只有一个单亲,但可以有多个孩子。目前,我们的 tf2 树包含三个框架: `world`, `turtle1` 财务报告和财务报告 `turtle2`。两只龟框是... `world` 框。如果我们想要在 tf2 中添加一个新的框,那么现有的三个框之一就需要是父框,而新的框将成为其子框。

![](images/turtlesim_frames.png) <span id="tasks"></span>

## 操作步骤

<span id="write-the-fixed-frame-broadcaster"></span>

### 1 写入固定帧播放器

以海龟为例,我们将增加一个新的框架 `carrot1`,这将是孩子 `turtle1`这个框架将成为第二只龟的目标。

让我们首先创建源文件。请到 `learning_tf2_py` 我们在以前的教程中创建了软件包。 `src/learning_tf2_py/learning_tf2_py` 目录通过输入以下命令来下载固定帧广播器代码:

##### Linux

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/fixed_frame_tf2_broadcaster.py
```

##### macOS

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/fixed_frame_tf2_broadcaster.py
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/fixed_frame_tf2_broadcaster.py -o fixed_frame_tf2_broadcaster.py
```

或于权壳中:

``` console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/fixed_frame_tf2_broadcaster.py -o fixed_frame_tf2_broadcaster.py
```

现在打开名为 `fixed_frame_tf2_broadcaster.py`.

``` python
from geometry_msgs.msg import TransformStamped

import rclpy
from rclpy.node import Node

from tf2_ros import TransformBroadcaster


class FixedFrameBroadcaster(Node):

   def __init__(self):
       super().__init__('fixed_frame_tf2_broadcaster')
       self.tf_broadcaster = TransformBroadcaster(self)
       self.timer = self.create_timer(0.1, self.broadcast_timer_callback)

   def broadcast_timer_callback(self):
       t = TransformStamped()

       t.header.stamp = self.get_clock().now().to_msg()
       t.header.frame_id = 'turtle1'
       t.child_frame_id = 'carrot1'
       t.transform.translation.x = 0.0
       t.transform.translation.y = 2.0
       t.transform.translation.z = 0.0
       t.transform.rotation.x = 0.0
       t.transform.rotation.y = 0.0
       t.transform.rotation.z = 0.0
       t.transform.rotation.w = 1.0

       self.tf_broadcaster.sendTransform(t)


def main():
    rclpy.init()
    node = FixedFrameBroadcaster()
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        pass

    rclpy.shutdown()
```

代码与tf2广播机的教程示例非常相似,唯一的区别在于这里的变换不会随着时间的变化而改变.

<span id="examine-the-code"></span>

#### 1.1 审查守则

让我们看看这个代码中的关键线条。在这里,我们从父代码创建了新的转换 `turtle1` 给新孩子的 `carrot1`。该词 `carrot1` 框架以 y 轴表示 `turtle1` 边框。

``` python
t = TransformStamped()

t.header.stamp = self.get_clock().now().to_msg()
t.header.frame_id = 'turtle1'
t.child_frame_id = 'carrot1'
t.transform.translation.x = 0.0
t.transform.translation.y = 2.0
t.transform.translation.z = 0.0
```

<span id="add-an-entry-point"></span>

#### 1.2 添加一个切入点

允许 `ros2 run` 命令来运行您的节点,您必须添加切入点到 `setup.py` 页:1 `src/learning_tf2_py` 目录).

将下行添加到 `'console_scripts':` 括号 :

``` python
'fixed_frame_tf2_broadcaster = learning_tf2_py.fixed_frame_tf2_broadcaster:main',
```

<span id="write-the-launch-file"></span>

#### 1.3 编写发射文件

现在让我们为这个例子创建一个启动文件。 使用您的文本编辑器, 创建一个名为新文件 `turtle_tf2_fixed_frame_demo_launch` 带有扩展的 `.py`, `.xml`,或 `.yaml` 输入 `src/learning_tf2_py/launch` 目录,并添加以下行:

##### Python

<span id="turtle-tf2-fixed-frame-demo-launch-py"></span>

``` python
from launch import LaunchDescription
from launch.actions import IncludeLaunchDescription
from launch.substitutions import PathJoinSubstitution
from launch_ros.actions import Node
from launch_ros.substitutions import FindPackageShare


def generate_launch_description():
    return LaunchDescription([
        IncludeLaunchDescription(
            PathJoinSubstitution([
                FindPackageShare('learning_tf2_py'), 'launch', 'turtle_tf2_demo.launch.py'])
        ),
        Node(
            package='learning_tf2_py',
            executable='fixed_frame_tf2_broadcaster',
            name='fixed_broadcaster',
        ),
    ])
```

##### XML 数据

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <include file="$(find-pkg-share learning_tf2_py)/launch/turtle_tf2_demo_launch.py" />
  <node pkg="learning_tf2_py" exec="fixed_frame_tf2_broadcaster" name="fixed_broadcaster" />
</launch>
```

##### 也门

``` yaml
%YAML 1.2
---
launch:
  - include:
      file: "$(find-pkg-share learning_tf2_py)/launch/turtle_tf2_demo_launch.py"
  - node:
      pkg: "learning_tf2_py"
      exec: "fixed_frame_tf2_broadcaster"
      name: "fixed_broadcaster"
```

此发射文件导入所需的软件包, 然后创建一个 `demo_nodes` 变量将存储我们在上一个教程的启动文件中创建的节点。

代码的最后一部分会加入我们的固定 `carrot1` 利用我们 `fixed_frame_tf2_broadcaster` 节点。

##### Python

``` python
        Node(
            package='learning_tf2_py',
            executable='fixed_frame_tf2_broadcaster',
            name='fixed_broadcaster',
        ),
```

##### XML 数据

``` xml
  <include file="$(find-pkg-share learning_tf2_py)/launch/turtle_tf2_demo_launch.py" />
  <node pkg="learning_tf2_py" exec="fixed_frame_tf2_broadcaster" name="fixed_broadcaster" />
```

##### 也门

``` yaml
  - node:
      pkg: "learning_tf2_py"
      exec: "fixed_frame_tf2_broadcaster"
      name: "fixed_broadcaster"
```

<span id="build"></span>

#### 1.4 建设

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
$ call install\setup.bat
```

<span id="run"></span>

#### 1.5 运行

现在你可以开始海龟播音员演示:

``` console
$ ros2 launch learning_tf2_py turtle_tf2_fixed_frame_demo_launch.xml # .py or .yaml are also acceptable
```

你应该注意的是,新的 `carrot1` 框架出现在变换树上。

![](images/turtlesim_frames_carrot.png)

如果您在周围驱动第一只龟,您应该注意到,尽管我们增加了一个新的框架,但行为并没有改变,这是因为添加一个额外的框架不会影响其他框架,而我们的听众仍在使用先前定义的框架。

因此,如果我们想要我们的第二只海龟 跟着胡萝卜而不是第一只海龟, 我们需要改变价值 `target_frame`。这可以有两种方法,一种方法是通过 `target_frame` 参数直接从控制台到发射文件:

``` console
$ ros2 launch learning_tf2_py turtle_tf2_fixed_frame_demo_launch.xml target_frame:=carrot1 # .py or .yaml are also acceptable
```

第二种方法是更新发射文件。 `turtle_tf2_fixed_frame_demo.launch.py` 文档,并添加 `'target_frame': 'carrot1'` 参数通过 `launch_arguments` 参数。

``` python
def generate_launch_description():
    demo_nodes = IncludeLaunchDescription(
        ...,
        launch_arguments={'target_frame': 'carrot1'}.items(),
        )
```

现在重建软件包,重新启动 `turtle_tf2_fixed_frame_demo.launch.py`将看到第二只海龟跟随胡萝卜,

![](images/carrot_static.png) <span id="write-the-dynamic-frame-broadcaster"></span>

### 2 写入动态帧播放器

我们在此教程中公布的额外框架是一个固定的框架,与父框架相比不会随时间而改变。但是,如果想要发布一个移动的框架,您可以编码广播机,以随时间而改变框架。让我们改变我们 `carrot1` 框架,使其相对于 `turtle1` 框,随时间推移。跳转到 `learning_tf2_py` 我们在上一个教程中创建的软件包。 `src/learning_tf2_py/learning_tf2_py` 目录通过输入以下命令来下载动态帧广播器代码:

##### Linux

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/dynamic_frame_tf2_broadcaster.py
```

##### macOS

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/dynamic_frame_tf2_broadcaster.py
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/dynamic_frame_tf2_broadcaster.py -o dynamic_frame_tf2_broadcaster.py
```

或于权壳中:

``` console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_py/turtle_tf2_py/dynamic_frame_tf2_broadcaster.py -o dynamic_frame_tf2_broadcaster.py
```

现在打开名为 `dynamic_frame_tf2_broadcaster.py`:

``` python
import math

from geometry_msgs.msg import TransformStamped

import rclpy
from rclpy.node import Node

from tf2_ros import TransformBroadcaster


class DynamicFrameBroadcaster(Node):

    def __init__(self):
        super().__init__('dynamic_frame_tf2_broadcaster')
        self.tf_broadcaster = TransformBroadcaster(self)
        self.timer = self.create_timer(0.1, self.broadcast_timer_callback)

    def broadcast_timer_callback(self):
        seconds, _ = self.get_clock().now().seconds_nanoseconds()
        x = seconds * math.pi

        t = TransformStamped()
        t.header.stamp = self.get_clock().now().to_msg()
        t.header.frame_id = 'turtle1'
        t.child_frame_id = 'carrot1'
        t.transform.translation.x = 10 * math.sin(x)
        t.transform.translation.y = 10 * math.cos(x)
        t.transform.translation.z = 0.0
        t.transform.rotation.x = 0.0
        t.transform.rotation.y = 0.0
        t.transform.rotation.z = 0.0
        t.transform.rotation.w = 1.0

        self.tf_broadcaster.sendTransform(t)


def main():
    rclpy.init()
    node = DynamicFrameBroadcaster()
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        pass

    rclpy.shutdown()
```

<span id="id1"></span>

#### 2.1 审查守则

而不是一个固定的定义 我们的x和y抵消, 我们正在使用 `sin()` 财务报告和财务报告 `cos()` 函数,以抵消当前时间 `carrot1` 正在不断改变。

``` python
seconds, _ = self.get_clock().now().seconds_nanoseconds()
x = seconds * math.pi
...
t.transform.translation.x = 10 * math.sin(x)
t.transform.translation.y = 10 * math.cos(x)
```

<span id="id2"></span>

#### 2.2 增加一个切入点

允许 `ros2 run` 命令来运行您的节点,您必须添加切入点到 `setup.py` 页:1 `src/learning_tf2_py` 目录).

将下行添加到 `'console_scripts':` 括号 :

``` python
'dynamic_frame_tf2_broadcaster = learning_tf2_py.dynamic_frame_tf2_broadcaster:main',
```

<span id="id3"></span>

#### 2.3 编写发射文件

要测试此代码, 请创建一个新的发射文件 `turtle_tf2_dynamic_frame_demo_launch` 带有扩展的 `.py`, `.xml`,或 `.yaml` 输入 `src/learning_tf2_py/launch` 目录并粘贴以下代码:

##### Python

<span id="turtle-tf2-dynamic-frame-demo-launch-py"></span>

``` default
from launch import LaunchDescription
from launch.actions import IncludeLaunchDescription
from launch.substitutions import PathJoinSubstitution
from launch_ros.actions import Node
from launch_ros.substitutions import FindPackageShare


def generate_launch_description():
    return LaunchDescription([
        IncludeLaunchDescription(
            PathJoinSubstitution([
                FindPackageShare('learning_tf2_py'), 'launch', 'turtle_tf2_demo.launch.py']),
            launch_arguments={'target_frame': 'carrot1'}.items(),
        ),
        Node(
            package='learning_tf2_py',
            executable='dynamic_frame_tf2_broadcaster',
            name='dynamic_broadcaster',
        ),
    ])
```

##### XML 数据

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <include file="$(find-pkg-share learning_tf2_py)/launch/turtle_tf2_demo_launch.py">
    <let name="target_frame" value="carrot1" />
  </include>
  <node pkg="learning_tf2_py" exec="dynamic_frame_tf2_broadcaster" name="dynamic_broadcaster" />
</launch>
```

##### 也门

``` yaml
%YAML 1.2
---
launch:
  - include:
      file: "$(find-pkg-share learning_tf2_py)/launch/turtle_tf2_demo_launch.py"
      let:
      - name: "target_frame"
        value: "carrot1"
  - node:
      pkg: "learning_tf2_py"
      exec: "dynamic_frame_tf2_broadcaster"
      name: "dynamic_broadcaster"
```

<span id="id4"></span>

#### 2.4 构建

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

<span id="id5"></span>

#### 1.5 运行

现在你可以开始动态帧演示:

``` console
$ ros2 launch learning_tf2_py turtle_tf2_dynamic_frame_demo_launch.xml # .py or .yaml are also acceptable
```

第二只乌龟遵循着胡萝卜的姿势,

![](images/carrot_dynamic.png) <span id="summary"></span>

## 小结

在这个教程中,你学到了 tf2 变换树、其结构及其特征。你还学会了在本地框架内思考最容易,并学会了为本地框架添加额外的固定和动态框架。
