<span id="writing-a-simple-publisher-and-subscriber-python"></span> <span id="pypubsub"></span>
# 编写简单的发布者和订阅者（Python）

**目标：** 使用 Python 创建并运行发布者和订阅者节点。

**教程级别：** 初学者

**预计用时：** 20 分钟

<span id="background"></span>
## 背景

本教程创建通过[话题](../Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)传递字符串消息的[节点](../Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md)。示例是简单的 talker（发布者）和 listener（订阅者）系统：一个节点发布数据，另一个订阅话题并接收数据。

示例代码见 [ros2/examples](https://github.com/ros2/examples/tree/rolling/rclpy/topics)。

<span id="prerequisites"></span>
## 前提条件

之前的教程介绍了[创建工作空间](Creating-A-Workspace/Creating-A-Workspace.md)和[创建软件包](Creating-Your-First-ROS2-Package.md)。

建议具备基本的 Python 知识，但这不是硬性要求。

<span id="tasks"></span>
## 操作步骤

<span id="create-a-package"></span>
### 1 创建软件包

打开新终端，[加载 ROS 2 安装环境](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)，使 `ros2` 命令可用。

进入[此前创建](Creating-A-Workspace/Creating-A-Workspace.md#new-directory)的 `ros2_ws` 工作空间。软件包应创建在 `src` 中，而非工作空间根目录，因此进入 `ros2_ws/src` 并运行：

```console
$ ros2 pkg create --build-type ament_python --license Apache-2.0 py_pubsub
```

终端会显示消息，确认 `py_pubsub` 及其必需文件和目录已经创建。

<span id="write-the-publisher-node"></span>
### 2 编写发布者节点

进入 `ros2_ws/src/py_pubsub/py_pubsub`。这是一个与外层 ROS 2 软件包同名的 [Python 包](https://docs.python.org/3/tutorial/modules.html#packages)。

运行对应命令，下载 talker 示例代码。

**Linux**

```console
$ wget https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_publisher/examples_rclpy_minimal_publisher/publisher_member_function.py
```

**macOS**

```console
$ wget https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_publisher/examples_rclpy_minimal_publisher/publisher_member_function.py
```

**Windows 命令提示符**

```console
$ curl -sk https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_publisher/examples_rclpy_minimal_publisher/publisher_member_function.py -o publisher_member_function.py
```

**Windows PowerShell**

```console
$ curl https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_publisher/examples_rclpy_minimal_publisher/publisher_member_function.py -o publisher_member_function.py
```

`__init__.py` 旁边会出现新文件 `publisher_member_function.py`。用文本编辑器打开：

```python
import rclpy
from rclpy.node import Node

from std_msgs.msg import String


class MinimalPublisher(Node):

    def __init__(self):
        super().__init__('minimal_publisher')
        self.publisher_ = self.create_publisher(String, 'topic', 10)
        timer_period = 0.5  # seconds
        self.timer = self.create_timer(timer_period, self.timer_callback)
        self.i = 0

    def timer_callback(self):
        msg = String()
        msg.data = 'Hello World: %d' % self.i
        self.publisher_.publish(msg)
        self.get_logger().info('Publishing: "%s"' % msg.data)
        self.i += 1


def main(args=None):
    rclpy.init(args=args)

    minimal_publisher = MinimalPublisher()

    rclpy.spin(minimal_publisher)

    # Destroy the node explicitly
    # (optional - otherwise it will be done automatically
    # when the garbage collector destroys the node object)
    minimal_publisher.destroy_node()
    rclpy.shutdown()


if __name__ == '__main__':
    main()
```

<span id="examine-the-code"></span>
#### 2.1 分析代码

开头导入 [rclpy](https://docs.ros.org/en/rolling/p/rclpy/)，以便使用其中的 [Node](https://docs.ros.org/en/rolling/p/rclpy/api/node.html) 类：

```python
import rclpy
from rclpy.node import Node
```

下一条语句导入内置的 [std_msgs/msg/String](https://docs.ros.org/en/rolling/p/std_msgs/msg/String.html) 消息类型，节点使用它组织通过话题传递的数据：

```python
from std_msgs.msg import String
```

这些导入语句体现了节点的依赖，下一节需要将它们添加到 `package.xml`。

接下来定义 `MinimalPublisher` 类，它继承 [Node](https://docs.ros.org/en/rolling/p/rclpy/api/node.html)：

```python
class MinimalPublisher(Node):
```

随后是构造函数。`super().__init__` 调用 `Node` 的构造函数，并指定节点名称为 `minimal_publisher`。

[create_publisher](https://docs.ros.org/en/rolling/p/rclpy/api/node.html#rclpy.node.Node.create_publisher) 创建发布者，向名为 `topic` 的话题发布从 `std_msgs.msg` 导入的 `String` 消息，队列大小为 10。队列大小是一项必要的[服务质量（QoS）](../../Concepts/Intermediate/About-Quality-of-Service-Settings.md)设置，用于在订阅者接收不够快时限制排队消息的数量。

[create_timer](https://docs.ros.org/en/rolling/p/rclpy/api/node.html#rclpy.node.Node.create_timer) 创建定时器，每 0.5 秒执行一次回调。`self.i` 是回调使用的计数器。

```python
def __init__(self):
    super().__init__('minimal_publisher')
    self.publisher_ = self.create_publisher(String, 'topic', 10)
    timer_period = 0.5  # seconds
    self.timer = self.create_timer(timer_period, self.timer_callback)
    self.i = 0
```

`timer_callback` 创建一条附有计数值的消息并发布，然后通过 [get_logger()](https://docs.ros.org/en/rolling/p/rclpy/api/node.html#rclpy.node.Node.get_logger) 返回的日志器的 [info()](https://docs.ros.org/en/rolling/p/rclpy/rclpy.impl.rcutils_logger.html#rclpy.impl.rcutils_logger.RcutilsLogger.info) 将其打印到控制台：

```python
def timer_callback(self):
    msg = String()
    msg.data = 'Hello World: %d' % self.i
    self.publisher_.publish(msg)
    self.get_logger().info('Publishing: "%s"' % msg.data)
    self.i += 1
```

最后定义主函数：

```python
def main(args=None):
    rclpy.init(args=args)

    minimal_publisher = MinimalPublisher()

    rclpy.spin(minimal_publisher)

    # Destroy the node explicitly
    # (optional - otherwise it will be done automatically
    # when the garbage collector destroys the node object)
    minimal_publisher.destroy_node()
    rclpy.shutdown()
```

先初始化 `rclpy`，再创建节点，然后调用 [spin()](https://docs.ros.org/en/rolling/p/rclpy/api/init_shutdown.html#rclpy.spin)，让节点处理回调。

<span id="add-dependencies"></span>
#### 2.2 添加依赖

返回上一级 `ros2_ws/src/py_pubsub`，其中已经有 `setup.py`、`setup.cfg` 和 `package.xml`。

用编辑器打开 `package.xml`。按照[上一篇教程](Creating-Your-First-ROS2-Package.md)，填写 `<description>`、`<maintainer>` 和 `<license>`：

```xml
<description>Examples of minimal publisher/subscriber using rclpy</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

在这些行后添加与节点导入语句对应的依赖：

```xml
<exec_depend>rclpy</exec_depend>
<exec_depend>std_msgs</exec_depend>
```

这表示软件包运行时需要 [rclpy](https://docs.ros.org/en/rolling/p/rclpy/) 和 [std_msgs](https://docs.ros.org/en/rolling/p/std_msgs/)。保存文件。

<span id="add-an-entry-point"></span>
#### 2.3 添加入口点

打开 `setup.py`，使 `maintainer`、`maintainer_email`、`description` 和 `license` 与 `package.xml` 保持一致：

```python
maintainer='YourName',
maintainer_email='you@email.com',
description='Examples of minimal publisher/subscriber using rclpy',
license='Apache License 2.0',
```

在 [entry_points](https://setuptools.pypa.io/en/latest/userguide/entry_point.html) 字段的 `console_scripts` 列表中添加：

```python
entry_points={
        'console_scripts': [
                'talker = py_pubsub.publisher_member_function:main',
        ],
},
```

保存文件。

<span id="check-setup-cfg"></span>
#### 2.4 检查 setup.cfg

`setup.cfg` 应已自动生成以下正确内容：

```ini
[develop]
script_dir=$base/lib/py_pubsub
[install]
install_scripts=$base/lib/py_pubsub
```

这会让 [setuptools](https://setuptools.pypa.io/en/latest/userguide) 将可执行程序安装到 `lib`，因为 `ros2 run` 会到那里查找它们。

现在已经可以构建软件包、加载本地环境并运行。不过，先创建订阅者节点，便能观察整个系统的运行情况。

<span id="write-the-subscriber-node"></span>
### 3 编写订阅者节点

返回 `ros2_ws/src/py_pubsub/py_pubsub`，运行以下命令获取下一个节点。

**Linux**

```console
$ wget https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_subscriber/examples_rclpy_minimal_subscriber/subscriber_member_function.py
```

**macOS**

```console
$ wget https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_subscriber/examples_rclpy_minimal_subscriber/subscriber_member_function.py
```

**Windows 命令提示符**

```console
$ curl -sk https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_subscriber/examples_rclpy_minimal_subscriber/subscriber_member_function.py -o subscriber_member_function.py
```

**Windows PowerShell**

```console
$ curl https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_subscriber/examples_rclpy_minimal_subscriber/subscriber_member_function.py -o subscriber_member_function.py
```

现在目录应包含以下文件：

```console
__init__.py  publisher_member_function.py  subscriber_member_function.py
```

<span id="id4"></span>
#### 3.1 分析代码

用编辑器打开 `subscriber_member_function.py`：

```python
import rclpy
from rclpy.node import Node

from std_msgs.msg import String


class MinimalSubscriber(Node):

    def __init__(self):
        super().__init__('minimal_subscriber')
        self.subscription = self.create_subscription(
            String,
            'topic',
            self.listener_callback,
            10)
        self.subscription  # prevent unused variable warning

    def listener_callback(self, msg):
        self.get_logger().info('I heard: "%s"' % msg.data)


def main(args=None):
    rclpy.init(args=args)

    minimal_subscriber = MinimalSubscriber()

    rclpy.spin(minimal_subscriber)

    # Destroy the node explicitly
    # (optional - otherwise it will be done automatically
    # when the garbage collector destroys the node object)
    minimal_subscriber.destroy_node()
    rclpy.shutdown()


if __name__ == '__main__':
    main()
```

订阅者代码与发布者非常相似。构造函数通过 [create_subscription](https://docs.ros.org/en/rolling/p/rclpy/api/node.html#rclpy.node.Node.create_subscription) 创建订阅，使用与发布者匹配的参数。根据[话题教程](../Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)，双方的话题名称和消息类型必须一致才能通信。

```python
self.subscription = self.create_subscription(
    String,
    'topic',
    self.listener_callback,
    10)
```

订阅者不需要定时器，因此构造函数和回调中没有定时器定义。收到消息时就会调用回调。

回调仅以 Info 级别将收到的数据打印到控制台。发布者设置的数据是 `msg.data = 'Hello World: %d' % self.i`。

```python
def listener_callback(self, msg):
    self.get_logger().info('I heard: "%s"' % msg.data)
```

`main` 的定义几乎相同，只是改为创建订阅者并对其调用 spin：

```python
minimal_subscriber = MinimalSubscriber()

rclpy.spin(minimal_subscriber)
```

这个节点与发布者具有相同依赖，因此无需向 `package.xml` 添加新依赖，也无需修改 `setup.cfg`。

<span id="id5"></span>
#### 3.2 添加入口点

重新打开 `setup.py`，在发布者入口点下面添加订阅者入口点。[entry_points](https://setuptools.pypa.io/en/latest/userguide/entry_point.html) 应变为：

```python
entry_points={
        'console_scripts': [
                'talker = py_pubsub.publisher_member_function:main',
                'listener = py_pubsub.subscriber_member_function:main',
        ],
},
```

保存后，发布/订阅系统就准备好了。

<span id="build-and-run"></span>
### 4 构建并运行

ROS 2 安装中通常已经包含 `rclpy` 和 `std_msgs`。不过，推荐构建前在工作空间根目录 `ros2_ws` 运行 [rosdep](https://docs.ros.org/en/independent/api/rosdep/html/) 检查缺失依赖，详见 [rosdep 教程](../Intermediate/Rosdep.md)。

**Linux**

```console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

**macOS 和 Windows**

本教程的 rosdep 步骤仅适用于 Linux，可跳到下一步。

仍在 `ros2_ws` 根目录，构建新软件包。

**Linux**

```console
$ colcon build --packages-select py_pubsub
```

**macOS**

```console
$ colcon build --packages-select py_pubsub
```

**Windows**

```console
$ colcon build --merge-install --packages-select py_pubsub
```

打开新终端，进入 `ros2_ws`，加载环境设置文件。

**Linux**

```console
$ source install/setup.bash
```

**macOS**

```console
$ . install/setup.bash
```

**Windows**

```console
$ call install/setup.bat
```

运行 talker 节点，终端应每 0.5 秒输出一条 Info 消息：

```console
$ ros2 run py_pubsub talker
[info] [minimal_publisher]: publishing: "hello world: 0"
[info] [minimal_publisher]: publishing: "hello world: 1"
[info] [minimal_publisher]: publishing: "hello world: 2"
[info] [minimal_publisher]: publishing: "hello world: 3"
[info] [minimal_publisher]: publishing: "hello world: 4"
...
```

再打开一个终端，在 `ros2_ws` 中加载环境，然后启动 listener。它会从发布者当前的计数开始打印收到的消息：

```console
$ ros2 run py_pubsub listener
[INFO] [minimal_subscriber]: I heard: "Hello World: 10"
[INFO] [minimal_subscriber]: I heard: "Hello World: 11"
[INFO] [minimal_subscriber]: I heard: "Hello World: 12"
[INFO] [minimal_subscriber]: I heard: "Hello World: 13"
[INFO] [minimal_subscriber]: I heard: "Hello World: 14"
```

在两个终端分别按 `Ctrl+C`，停止节点。

<span id="summary"></span>
## 小结

你创建了两个通过话题发布和订阅数据的节点，并在运行前将它们的依赖和入口点添加到软件包配置文件。

<span id="next-steps"></span>
## 后续步骤

接下来创建另一个使用服务/客户端模型的简单 ROS 2 软件包，同样可以选择 [C++](Writing-A-Simple-Cpp-Service-And-Client.md) 或 [Python](Writing-A-Simple-Py-Service-And-Client.md)。

<span id="related-content"></span>
## 相关内容

Python 发布者和订阅者有多种写法，可参阅 [ros2/examples](https://github.com/ros2/examples/tree/rolling/rclpy/topics) 中的 `minimal_publisher` 和 `minimal_subscriber` 软件包。
