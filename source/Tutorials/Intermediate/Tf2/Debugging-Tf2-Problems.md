---
translation_status: machine_translated
source: Tutorials/Intermediate/Tf2/Debugging-Tf2-Problems.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="debugging"></span> <span id="debuggingtf2problems"></span>

# 调试

**目标：** 学习如何使用系统的方法调试 tf2 相关问题.

**教程级别：** 中级

**用时：** 10分钟

<span id="background"></span>

## 背景

此教程会带您通过一些步骤来调试一个典型的 tf2 问题。 它还会使用很多 tf2 调试工具, 例如 。 `tf2_echo`, `tf2_monitor`,以及 `view_frames`。这个教程假设您已完成了 [学习 tf2](Tf2-Main.md) 课程。

<span id="debugging-example"></span>

## 调试示例

<span id="setting-and-starting-the-example"></span>

### 1 设定和开始实例

对于这个教程, 我们将设置一个有一系列问题的演示应用程序。 这个教程的目标是应用一个系统的方法来查找和解决这些问题。 首先, 让我们创建源文件 。

转到 `learning_tf2_cpp` 我们创建的软件包 [tf2 教程](Tf2-Main.md)内部 `src` 目录生成源文件副本 `turtle_tf2_listener.cpp` 并重命名为 `turtle_tf2_listener_debug.cpp`.

使用您首选的文本编辑器打开文件,并更改第67行

``` C++
std::string toFrameRel = "turtle2";
```

改为:

``` C++
std::string toFrameRel = "turtle3";
```

更改 `lookupTransform()` 呼叫75-79行从

``` C++
try {
   t = tf_buffer_->lookupTransform(
     toFrameRel,
     fromFrameRel,
     tf2::TimePointZero);
} catch (tf2::TransformException & ex) {
```

改为:

``` C++
try {
   t = tf_buffer_->lookupTransform(
     toFrameRel,
     fromFrameRel,
     this->now());
} catch (tf2::TransformException & ex) {
```

并保存文件的更改。 要运行此演示, 我们需要创建启动文件 `start_tf2_debug_demo_launch` 带有扩展的 `.py`, `.xml`,或 `.yaml` 输入 `launch` 软件包子目录 `learning_tf2_cpp`:

##### Python

``` python
from launch import LaunchDescription
from launch.actions import DeclareLaunchArgument
from launch.substitutions import LaunchConfiguration
from launch_ros.actions import Node


def generate_launch_description():
    return LaunchDescription([
        DeclareLaunchArgument(
            'target_frame', default_value='turtle1',
            description='Target frame name.'
        ),
        Node(
            package='turtlesim',
            executable='turtlesim_node',
            name='sim',
            output='screen'
        ),
        Node(
            package='learning_tf2_cpp',
            executable='turtle_tf2_broadcaster',
            name='broadcaster1',
            parameters=[
                {'turtlename': 'turtle1'}
            ]
        ),
        Node(
            package='learning_tf2_cpp',
            executable='turtle_tf2_broadcaster',
            name='broadcaster2',
            parameters=[
                {'turtlename': 'turtle2'}
            ]
        ),
        Node(
            package='learning_tf2_cpp',
            executable='turtle_tf2_listener_debug',
            name='listener_debug',
            parameters=[
                {'target_frame': LaunchConfiguration('target_frame')}
            ]
        ),
    ])
```

##### XML 数据

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <arg name="target_frame" default="turtle1" description="Target frame name." />
  <node pkg="turtlesim" exec="turtlesim_node" name="sim" output="screen" />
  <node pkg="learning_tf2_cpp" exec="turtle_tf2_broadcaster" name="broadcaster1">
    <param name="turtlename" value="turtle1" />
  </node>
  <node pkg="learning_tf2_cpp" exec="turtle_tf2_broadcaster" name="broadcaster2">
    <param name="turtlename" value="turtle2" />
  </node>
  <node pkg="learning_tf2_cpp" exec="turtle_tf2_listener_debug" name="listener_debug">
    <param name="target_frame" value="$(var target_frame)" />
  </node>
</launch>
```

##### 也门

``` yaml
%YAML 1.2
---
launch:
  - arg:
      name: "target_frame"
      default: "turtle1"
      description: "Target frame name."
  - node:
      pkg: "turtlesim"
      exec: "turtlesim_node"
      name: "sim"
      output: "screen"
  - node:
      pkg: "learning_tf2_cpp"
      exec: "turtle_tf2_broadcaster"
      name: "broadcaster1"
      param:
      - name: "turtlename"
        value: "turtle1"
  - node:
      pkg: "learning_tf2_cpp"
      exec: "turtle_tf2_broadcaster"
      name: "broadcaster2"
      param:
      - name: "turtlename"
        value: "turtle2"
  - node:
      pkg: "learning_tf2_cpp"
      exec: "turtle_tf2_listener_debug"
      name: "listener_debug"
      param:
      - name: "target_frame"
        value: "$(var target_frame)"
```

不要忘记添加 `turtle_tf2_listener_debug` 可执行到 `CMakeLists.txt` 并构建软件包。

现在让我们看看会发生什么:

##### XML 数据

``` console
$ ros2 launch learning_tf2_cpp start_tf2_debug_demo_launch.xml
```

##### 也门

``` console
$ ros2 launch learning_tf2_cpp start_tf2_debug_demo_launch.yaml
```

##### Python

``` console
$ ros2 launch learning_tf2_cpp start_tf2_debug_demo_launch.py
```

你会看到乌龟的出现 同时,如果你运行 `turtle_teleop_key` 在另一个终端窗口中,您可以使用箭头键来驱动 `turtle1` 周围。

``` console
$ ros2 run turtlesim turtle_teleop_key
[turtle_tf2_listener_debug-4] [INFO] [1630223454.942322623] [listener_debug]: Could not
transform turtle3 to turtle1: "turtle3" passed to lookupTransform argument target_frame
does not exist
```

您也会注意到在左角下方有第二只乌龟。 如果演示会正确工作, 这第二只乌龟应该跟随您用箭头键命令的乌龟。 然而, 事实并非如此, 因为我们必须先解决一些问题 。

<span id="finding-the-tf2-request"></span>

### 2 查找 tf2 请求

首先,我们需要找出我们究竟要求 tf2 做什么。因此,我们进入使用 tf2 的代码部分。 `src/turtle_tf2_listener_debug.cpp` 文档中,请查看第67行:

``` C++
std::string toFrameRel = "turtle3";
```

以及第75-79行:

``` C++
try {
   t = tf_buffer_->lookupTransform(
     toFrameRel,
     fromFrameRel,
     this->now());
} catch (tf2::TransformException & ex) {
```

在这里,我们实际要求 tf2. 三个参数直接告诉我们我们要求 tf2: 从框架转换 `turtle3` 创建框架 `turtle1` 时间 `now`.

让我们看看为什么Tf2申请失败了。

<span id="checking-the-frames"></span>

### 3 检查框架

首先,看看Tf2是否知道我们之间的转变 `turtle3` 财务报告和财务报告 `turtle1`,我们将使用 `tf2_echo` 工具。

``` console
$ ros2 run tf2_ros tf2_echo turtle3 turtle1
[INFO] [1630223557.477636052] [tf2_echo]: Waiting for transform turtle3 ->  turtle1:
Invalid frame ID "turtle3" passed to canTransform argument target_frame - frame does
not exist
```

产出告诉我们这个框架 `turtle3` 不存在。

那么, 哪些框架存在? 如果您想要获得这个的图形化的描述, 请使用 `view_frames` 工具。

``` console
$ ros2 run tf2_tools view_frames
```

打开生成的 `frames.pdf` 查看以下输出的文件 :

![](images/turtlesim_frames.png)

所以很明显的问题是 我们要求从框架进行改造 `turtle3`,不存在。要修复此错误,只需替换 `turtle3` 与 `turtle2` 在67号线上。

现在停止运行演示,建造,再运行一遍:

##### XML 数据

``` console
$ ros2 launch learning_tf2_cpp start_tf2_debug_demo_launch.xml
[turtle_tf2_listener_debug-4] [INFO] [1630223704.617382464] [listener_debug]: Could not
transform turtle2 to turtle1: Lookup would require extrapolation into the future. Requested
time 1630223704.617054 but the latest data is at time 1630223704.616726, when looking up
transform from frame [turtle1] to frame [turtle2]
```

##### 也门

``` console
$ ros2 launch learning_tf2_cpp start_tf2_debug_demo_launch.yaml
[turtle_tf2_listener_debug-4] [INFO] [1630223704.617382464] [listener_debug]: Could not
transform turtle2 to turtle1: Lookup would require extrapolation into the future. Requested
time 1630223704.617054 but the latest data is at time 1630223704.616726, when looking up
transform from frame [turtle1] to frame [turtle2]
```

##### Python

``` console
$ ros2 launch learning_tf2_cpp start_tf2_debug_demo_launch.py
[turtle_tf2_listener_debug-4] [INFO] [1630223704.617382464] [listener_debug]: Could not
transform turtle2 to turtle1: Lookup would require extrapolation into the future. Requested
time 1630223704.617054 but the latest data is at time 1630223704.616726, when looking up
transform from frame [turtle1] to frame [turtle2]
```

我们马上会遇到下一个问题。

<span id="checking-the-timestamp"></span>

### 4 检查时间戳

现在我们解决了框名问题,是时候看看时间戳了。记住,我们正在尝试在中间实现转换。 `turtle2` 财务报告和财务报告 `turtle1` 在当前时间(即 `now`)为了获得有关时间的统计,拨打 `tf2_monitor` 带有相应的帧。

``` console
$ ros2 run tf2_ros tf2_monitor turtle2 turtle1
RESULTS: for turtle2 to turtle1
Chain is: turtle1
Net delay     avg = 0.00287347: max = 0.0167241

Frames:
Frame: turtle1, published by <no authority available>, Average Delay: 0.000295833, Max Delay: 0.000755072

All Broadcasters:
Node: <no authority available> 125.246 Hz, Average Delay: 0.000290237 Max Delay: 0.000786781
```

这里的关键部分是链条从 `turtle2` 改为: `turtle1`。输出显示平均延迟约3毫秒。这意味着 tf2 只能在3毫秒过后才能在海龟之间变换。所以,如果我们是在3毫秒之前要求 tf2 在海龟之间变换,而不是在3毫秒之前。 `now`, tf2 将有时能够给出一个答案。让我们通过将75-79行改为:

``` C++
try {
   t = tf_buffer_->lookupTransform(
     toFrameRel,
     fromFrameRel,
     this->now() - rclcpp::Duration::from_seconds(0.1));
} catch (tf2::TransformException & ex) {
```

在新代码中,我们要求100毫秒前龟之间的变换。通常使用更长的时间,只是为了确保变换到达。停止演示、构建和运行:

##### XML 数据

``` console
$ ros2 launch learning_tf2_cpp start_tf2_debug_demo_launch.xml
```

##### 也门

``` console
$ ros2 launch learning_tf2_cpp start_tf2_debug_demo_launch.yaml
```

##### Python

``` console
$ ros2 launch learning_tf2_cpp start_tf2_debug_demo_launch.py
```

你终于该看看海龟的动作了!

![](images/turtlesim_follow1.png)

我们最后的修补并不是你真正想要做的,它只是为了确保那是我们的问题。真正的修补会是这样的:

``` C++
try {
   t = tf_buffer_->lookupTransform(
     toFrameRel,
     fromFrameRel,
     tf2::TimePointZero);
} catch (tf2::TransformException & ex) {
```

或者像这样:

``` C++
try {
   t = tf_buffer_->lookupTransform(
     toFrameRel,
     fromFrameRel,
     tf2::TimePoint());
} catch (tf2::TransformException & ex) {
```

你可以多学点超时技术 [利用时间](Learning-About-Tf2-And-Time-Cpp.md) 教程,并按以下方式使用:

``` C++
try {
   t = tf_buffer_->lookupTransform(
     toFrameRel,
     fromFrameRel,
     this->now(),
     rclcpp::Duration::from_seconds(0.05));
} catch (tf2::TransformException & ex) {
```

<span id="summary"></span>

## 小结

在此教程中, 您学会了如何使用系统方法调试 tf2 相关问题 。 您也学会了如何使用 tf2 调试工具, 例如 。 `tf2_echo`, `tf2_monitor`,以及 `view_frames` 帮助您调试 tf2 问题。
