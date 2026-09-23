---
translation_status: machine_translated
source: Tutorials/Advanced/Recording-A-Bag-From-Your-Own-Node-Py.rst
---

<span id="recording-a-bag-from-a-node-python"></span> <span id="ros2bagownnodepython"></span>

# 在节点中录制 bag（Python）

**目标：** 从自己的Python节点记录数据到一个袋子.

**教程级别：** 高级

**用时：** 20分钟

<span id="background"></span>

## 背景

`rosbag2` 不只是提供 `ros2 bag` 命令行工具。它还提供了一个 Python API , 用于从您的源代码读取和写入一个包。 这使得您可以订阅一个主题, 并将所收到的数据保存到一个包中, 同时进行您选择的其他处理。 例如, 您可以这样做, 从一个主题中保存数据以及处理数据的结果, 而不需要将处理过的数据发送到一个主题上, 只记录它。 由于任何数据都可以记录在包中, 也可以将另一个来源生成的数据保存下来, 例如用于培训集的合成数据。 这对于快速生成一个包含大量样本的包很有帮助 。

<span id="prerequisites"></span>

## 前提条件

你应该有 `rosbag2` 作为常规 ROS 2 设置的一部分而安装的软件包。

如果您已经从 Linux 上的 deb 软件包中安装, 它可能默认会被安装。 如果不是, 您可以使用此命令安装 。

``` console
$ sudo apt install ros-rolling-rosbag2
```

此教程使用 ROS 2 袋讨论, 包括来自终端。 您应该已经完成 [基本 ROS 2 袋教程](../Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.md).

<span id="tasks"></span>

## 操作步骤

<span id="create-a-package"></span>

### 1 创建软件包

打开一个新的终端 [源代码 ROS 2 安装](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md) 这样一来 `ros2` 命令会起作用的。

跟着 [这些指示](../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md#new-directory) 创建新工作空间 `ros2_ws`.

导航到 `ros2_ws/src` 目录和创建新软件包 :

``` console
$ ros2 pkg create --build-type ament_python --license Apache-2.0 bag_recorder_nodes_py --dependencies rclpy rosbag2_py example_interfaces std_msgs
```

您的终端将返回一个消息, 以验证您的软件包的创建 `bag_recorder_nodes_py` 及其所有必要的文件和文件夹。 `--dependencies` 参数将自动在 `package.xml`。在这种情况下,软件包将使用 `rosbag2_py` 软件包和软件包 `rclpy` 软件包。 `example_interfaces` 信息定义也需要软件包。

<span id="update-package-xml-and-setup-py"></span>

#### 1.1 最新情况 `package.xml` 财务报告和财务报告 `setup.py`

因为你用了 `--dependencies` 在创建软件包时,您不需要手动添加依赖性到 `package.xml`但是,一如既往,确保添加描述、维护者电子邮件和姓名,并给信息发放许可证。 `package.xml`.

``` xml
<description>Python bag writing tutorial</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

还要确保将这一信息添加到 `setup.py` 档案也一样。

``` Python
maintainer='Your Name',
maintainer_email='you@email.com',
description='Python bag writing tutorial',
license='Apache License 2.0',
```

<span id="write-the-python-node"></span>

### 2 写入 Python 节点

内侧 `ros2_ws/src/bag_recorder_nodes_py/bag_recorder_nodes_py` 目录,创建名为新文件 `simple_bag_recorder.py` 并粘贴下面的代码。

``` Python
import rclpy
from rclpy.node import Node
from rclpy.serialization import serialize_message
from std_msgs.msg import String

import rosbag2_py

class SimpleBagRecorder(Node):
    def __init__(self):
        super().__init__('simple_bag_recorder')
        self.writer = rosbag2_py.SequentialWriter()

        storage_options = rosbag2_py.StorageOptions(
            uri='my_bag',
            storage_id='sqlite3')
        converter_options = rosbag2_py.ConverterOptions('', '')
        self.writer.open(storage_options, converter_options)

        topic_info = rosbag2_py.TopicMetadata(
            name='chatter',
            type='std_msgs/msg/String',
            serialization_format='cdr')
        self.writer.create_topic(topic_info)

        self.subscription = self.create_subscription(
            String,
            'chatter',
            self.topic_callback,
            10)
        self.subscription

    def topic_callback(self, msg):
        self.writer.write(
            'chatter',
            serialize_message(msg),
            self.get_clock().now().nanoseconds)


def main(args=None):
    rclpy.init(args=args)
    sbr = SimpleBagRecorder()
    rclpy.spin(sbr)
    rclpy.shutdown()


if __name__ == '__main__':
    main()
```

<span id="examine-the-code"></span>

#### 2.1 审查守则

那个... `import` 上方的语句是软件包的依赖关系。请注意导入 `rosbag2_py` 用于处理袋文件所需的函数和结构的软件包。

在类构建器中,我们首先创建我们用来写给包的编剧对象。我们正在创建一个 `SequentialWriter`,将信件按接收顺序写入包中。其他行为不同的作者可以在 [rosbag2](https://github.com/ros2/rosbag2/tree/rolling/rosbag2_cpp/include/rosbag2_cpp/writers).

``` Python
self.writer = rosbag2_py.SequentialWriter()
```

现在我们有了写作对象,就可以用它打开包。我们指定要创建的包的URI和格式(`sqlite3`),在默认情况下留下其他选项。使用了默认转换选项,该选项将不进行转换,并将信件以序列化格式存储。

``` Python
storage_options = rosbag2_py.StorageOptions(
    uri='my_bag',
    storage_id='sqlite3')
converter_options = rosbag2_py.ConverterOptions('', '')
self.writer.open(storage_options, converter_options)
```

接下来,我们需要告诉作者我们希望存储的主题。 `TopicMetadata` 此对象指定了所使用的主题名称、主题数据类型和序列化格式。

``` Python
topic_info = rosbag2_py.TopicMetadata(
    name='chatter',
    type='std_msgs/msg/String',
    serialization_format='cdr')
self.writer.create_topic(topic_info)
```

随着编剧的设立来记录我们传递到它的数据,我们创建一个订阅并指定一个回调。我们将在回调中将数据写到包中。

``` Python
self.subscription = self.create_subscription(
    String,
    'chatter',
    self.topic_callback,
    10)
self.subscription
```

回调以无序列化形式接收信件(按照标准) `rclpy` API)并将此消息传递给编剧, 指定数据的主题和与信息一起记录的时间戳。 然而, 编剧需要序列化消息存储在包中。 这意味着我们需要在将数据传递给编剧之前序列化数据 。 为此原因, 我们调用 `serialize_message()` 将结果传递给作者,而不是直接在信息中传递。

``` Python
def topic_callback(self, msg):
    self.writer.write(
        'chatter',
        serialize_message(msg),
        self.get_clock().now().nanoseconds)
```

文件以 `main` 函数用于创建节点实例并启动ROS处理。

``` Python
def main(args=None):
    rclpy.init(args=args)
    sbr = SimpleBagRecorder()
    rclpy.spin(sbr)
    rclpy.shutdown()
```

<span id="add-entry-point"></span>

#### 2.2 添加切入点

打开 `setup.py` 文档中 `bag_recorder_nodes_py` 软件包,并为您的节点添加一个切入点。

``` Python
entry_points={
    'console_scripts': [
        'simple_bag_recorder = bag_recorder_nodes_py.simple_bag_recorder:main',
    ],
},
```

<span id="build-and-run"></span>

### 3 构建和运行

导航回你工作空间的根, `ros2_ws`,并构建您的新软件包。

##### Linux

``` console
$ colcon build --packages-select bag_recorder_nodes_py
```

##### macOS

``` console
$ colcon build --packages-select bag_recorder_nodes_py
```

##### Windows

``` console
$ colcon build --merge-install --packages-select bag_recorder_nodes_py
```

打开新终端, 导航到 `ros2_ws`,并源代码设置文件。

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

现在运行节点:

``` console
$ ros2 run bag_recorder_nodes_py simple_bag_recorder
```

打开第二个终端并运行 `talker` 实例节点。

``` console
$ ros2 run demo_nodes_py talker
```

这将开始发布关于 `chatter` 主题。当袋写节点收到此数据时,它会将其写入 `my_bag` 包,如果有的话 `my_bag` 目录已经存在,您必须在运行前先删除它 `simple_bag_recorder` 节点,因为... `rosbag2` 默认不会覆盖已有的包,因此目的目录无法存在。

终止两个节点。 然后,在一个终端开始 `listener` 实例节点。

``` console
$ ros2 run demo_nodes_py listener
```

在另一个终端,使用 `ros2 bag` 播放您节点记录的包。

``` console
$ ros2 bag play my_bag
```

你会看到从包里收到的信息 被人们收到 `listener` 节点。

如果您希望再次运行包写节点, 您首先需要删除 `my_bag` 目录。

<span id="record-synthetic-data-from-a-node"></span>

### 4 记录一个节点的合成数据

任何数据都可以被记录到一个包中, 而不仅仅是在某个话题下收到的数据。 从您自己的节点写入一个包的常用例是生成和存储合成数据。 在本节中, 您将学习如何写入一个生成一些数据的节点, 并将其存储在一个包中。 我们将演示两种方法。 第一个方法是使用带有定时器的节点; 如果数据生成在节点之外, 您将使用这种方法, 如直接从硬件( 如相机) 读取数据, 第二种方法是不使用节点; 这是不需要使用ROS 基础设施的任何功能时您可以使用的方法 。

<span id="write-a-python-node"></span>

#### 4.1 写入 Python 节点

内侧 `ros2_ws/src/bag_recorder_nodes_py/bag_recorder_nodes_py` 目录,创建名为新文件 `data_generator_node.py` 并粘贴下面的代码。

``` Python
import rclpy
from rclpy.node import Node
from rclpy.serialization import serialize_message
from example_interfaces.msg import Int32

import rosbag2_py

class DataGeneratorNode(Node):
    def __init__(self):
        super().__init__('data_generator_node')
        self.data = Int32()
        self.data.data = 0
        self.writer = rosbag2_py.SequentialWriter()

        storage_options = rosbag2_py.StorageOptions(
            uri='timed_synthetic_bag',
            storage_id='sqlite3')
        converter_options = rosbag2_py.ConverterOptions('', '')
        self.writer.open(storage_options, converter_options)

        topic_info = rosbag2_py.TopicMetadata(
            name='synthetic',
            type='example_interfaces/msg/Int32',
            serialization_format='cdr')
        self.writer.create_topic(topic_info)

        self.timer = self.create_timer(1, self.timer_callback)

    def timer_callback(self):
        self.writer.write(
            'synthetic',
            serialize_message(self.data),
            self.get_clock().now().nanoseconds)
        self.data.data += 1


def main(args=None):
    rclpy.init(args=args)
    dgn = DataGeneratorNode()
    rclpy.spin(dgn)
    rclpy.shutdown()


if __name__ == '__main__':
    main()
```

<span id="id1"></span>

#### 4.2 审查守则

此代码大部分与第一例相同,重要差异在此描述.

一,包头改名.

``` Python
storage_options = rosbag2_py.StorageOptions(
    uri='timed_synthetic_bag',
    storage_id='sqlite3')
```

主题名称也随之更改,存储的数据类型也随之改变.

``` Python
topic_info = rosbag2_py.TopicMetadata(
    name='synthetic',
    type='example_interfaces/msg/Int32',
    serialization_format='cdr')
self.writer.create_topic(topic_info)
```

此节点没有订阅一个话题,而是有一个定时器。 定时器有1秒钟的时间段起火, 并在指定成员时调用其功能 。

``` Python
self.timer = self.create_timer(1, self.timer_callback)
```

在计时器回调内,我们生成(或者以其他方式获得,比如从连接到某些硬件的序列端口读取)我们希望存储在包中的数据。和前一个例子一样,数据还没有序列化,所以我们必须先进行序列化,然后再将其传递给编剧.

``` Python
self.writer.write(
    'synthetic',
    serialize_message(self.data),
    self.get_clock().now().nanoseconds)
```

<span id="add-executable"></span>

#### 4.3 添加可执行文件

打开 `setup.py` 文档中 `bag_recorder_nodes_py` 软件包,并为您的节点添加一个切入点。

``` Python
entry_points={
    'console_scripts': [
        'simple_bag_recorder = bag_recorder_nodes_py.simple_bag_recorder:main',
        'data_generator_node = bag_recorder_nodes_py.data_generator_node:main',
    ],
},
```

<span id="id2"></span>

#### 4.4 建设和运行

导航回你工作空间的根, `ros2_ws`,并构建您的软件包。

##### Linux

``` console
$ colcon build --packages-select bag_recorder_nodes_py
```

##### macOS

``` console
$ colcon build --packages-select bag_recorder_nodes_py
```

##### Windows

``` console
$ colcon build --merge-install --packages-select bag_recorder_nodes_py
```

打开新终端, 导航到 `ros2_ws`,并源代码设置文件。

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

如果说 `timed_synthetic_bag` 目录已经存在,您必须在运行节点前先删除它。

现在运行节点:

``` console
$ ros2 run bag_recorder_nodes_py data_generator_node
```

等待30秒左右,然后终止节点 <span class="kbd kbd docutils literal notranslate">缩略语</span>-<span class="kbd kbd docutils literal notranslate">c</span>。接下来,播放创建的包。

``` console
$ ros2 bag play timed_synthetic_bag
```

打开第二个终端并回声 `/synthetic` 主题。

``` console
$ ros2 topic echo /synthetic
```

您将看到生成并存储在打印到控制台的包中的数据, 速度为每秒一信。 Name

<span id="record-synthetic-data-from-an-executable"></span>

### 5 从可执行文件记录合成数据

现在,你可以创建一个包,存储来自一个非主题来源的数据,你会学习如何生成和记录一个非节点执行器的合成数据。这种方法的优点是更简单的代码和快速创建大量数据。

<span id="write-a-python-executable"></span>

#### 5.1 写入 Python 可执行文件

内侧 `ros2_ws/src/bag_recorder_nodes_py/bag_recorder_nodes_py` 目录,创建名为新文件 `data_generator_executable.py` 并粘贴下面的代码。

``` Python
from rclpy.clock import Clock
from rclpy.duration import Duration
from rclpy.serialization import serialize_message
from example_interfaces.msg import Int32

import rosbag2_py


def main(args=None):
    writer = rosbag2_py.SequentialWriter()

    storage_options = rosbag2_py.StorageOptions(
        uri='big_synthetic_bag',
        storage_id='sqlite3')
    converter_options = rosbag2_py.ConverterOptions('', '')
    writer.open(storage_options, converter_options)

    topic_info = rosbag2_py.TopicMetadata(
        name='synthetic',
        type='example_interfaces/msg/Int32',
        serialization_format='cdr')
    writer.create_topic(topic_info)

    time_stamp = Clock().now()
    for ii in range(0, 100):
        data = Int32()
        data.data = ii
        writer.write(
            'synthetic',
            serialize_message(data),
            time_stamp.nanoseconds)
        time_stamp += Duration(seconds=1)

if __name__ == '__main__':
    main()
```

<span id="id3"></span>

#### 5.2 审查守则

将这个样本和之前的样本进行比较,可以发现它们没有那么不同。唯一显著的区别是使用循环驱动数据生成而不是定时器。

请注意, 我们现在正在为数据生成时间戳, 而不是依赖当前每个样本的系统时间。 时间戳可以是您需要的任意值。 数据会按这些时间戳给出的速度播放, 所以这是控制样本默认播放速度的有用方法 。 请注意, 虽然每个样本之间的间隔是完整的第二时间, 但是这个可执行文件不需要在每一个样本之间等待第二时间 。 这样我们就可以在比重播要短得多的时间里生成大量涵盖广泛时间段的数据 。

``` Python
time_stamp = Clock().now()
for ii in range(0, 100):
    data = Int32()
    data.data = ii
    writer.write(
        'synthetic',
        serialize_message(data),
        time_stamp.nanoseconds)
    time_stamp += Duration(seconds=1)
```

<span id="id4"></span>

#### 5.3 添加可执行文件

打开 `setup.py` 文档中 `bag_recorder_nodes_py` 软件包,并为您的节点添加一个切入点。

``` Python
entry_points={
    'console_scripts': [
        'simple_bag_recorder = bag_recorder_nodes_py.simple_bag_recorder:main',
        'data_generator_node = bag_recorder_nodes_py.data_generator_node:main',
        'data_generator_executable = bag_recorder_nodes_py.data_generator_executable:main',
    ],
},
```

<span id="id5"></span>

#### 5.4 构建和运行

导航回你工作空间的根, `ros2_ws`,并构建您的软件包。

##### Linux

``` console
$ colcon build --packages-select bag_recorder_nodes_py
```

##### macOS

``` console
$ colcon build --packages-select bag_recorder_nodes_py
```

##### Windows

``` console
$ colcon build --merge-install --packages-select bag_recorder_nodes_py
```

打开终端, 导航到 `ros2_ws`,并源代码设置文件。

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

如果说 `big_synthetic_bag` 目录已经存在, 您必须在运行可执行文件前先删除它 。

现在运行可执行文件 :

``` console
$ ros2 run bag_recorder_nodes_py data_generator_executable
```

注意可执行文件运行和完成速度非常快 。

现在回放创造出来的包。

``` console
$ ros2 bag play big_synthetic_bag
```

打开第二个终端并回声 `/synthetic` 主题。

``` console
$ ros2 topic echo /synthetic
```

您将会看到在打印到控制台的袋子中生成和存储的数据, 速度为每秒一信。 尽管袋是迅速生成的, 但仍按邮票显示的速度播放 。

<span id="summary"></span>

## 小结

您创建了一个节点, 将它接收到的数据记录在一个包中。 您测试了使用节点记录一个包, 并且通过回放这个包来验证数据。 这种方法可以用来记录一个包, 里面有比它收到的更多数据, 例如处理收到数据的结果。 您接着创建了一个节点和一个可执行文件, 生成合成数据并存储在一个包中。 后一种方法对于生成合成数据特别有用, 这些数据可以用作培训集。
