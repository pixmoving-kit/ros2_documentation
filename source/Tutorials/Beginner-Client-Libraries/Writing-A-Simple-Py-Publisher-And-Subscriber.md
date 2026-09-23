---
translation_status: machine_translated
source: Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Publisher-And-Subscriber.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="writing-a-simple-publisher-and-subscriber-python"></span> <span id="pypubsub"></span>

# 编写简单的发布者与订阅者（Python）

**目标：** 使用 Python 创建并运行一个出版商和订阅者节点.

**教程级别：** 入门

**用时：** 20分钟

<span id="background"></span>

## 背景

在此教程中, 您将创建 [节点](../Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md) 以字符串信息的形式传递信息 [话题](../Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md). 这里使用的例子是一个简单的“谈话者”和“听众”系统;一个节点公布数据,另一个节点订阅这个专题,以便接收数据。

这些示例中使用的代码可以找到 [这儿](https://github.com/ros2/examples/tree/rolling/rclpy/topics).

<span id="prerequisites"></span>

## 前提条件

在之前的教程中,你学会了如何 [创建工作空间](Creating-A-Workspace/Creating-A-Workspace.md) 财务报告和财务报告 [创建软件包](Creating-Your-First-ROS2-Package.md).

建议对Python有一个基本的理解,但并不完全必要.

<span id="tasks"></span>

## 操作步骤

<span id="create-a-package"></span>

### 1 创建软件包

打开一个新的终端 [源代码 ROS 2 安装](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md) 这样一来 `ros2` 命令会起作用的。

导航到 `ros2_ws` 在 a 中创建目录 [上一个教程](Creating-A-Workspace/Creating-A-Workspace.md#new-directory).

回顾 应在 `src` 目录,不是工作空间的根。所以,导航到 `ros2_ws/src`,并运行软件包创建命令:

``` console
$ ros2 pkg create --build-type ament_python --license Apache-2.0 py_pubsub
```

您的终端将返回一个消息, 以验证您的软件包的创建 `py_pubsub` 以及所有必要的文件和文件夹。

<span id="write-the-publisher-node"></span>

### 2 写入出版商节点

导航进入 `ros2_ws/src/py_pubsub/py_pubsub`。回顾此目录是 [Python 软件包](https://docs.python.org/3/tutorial/modules.html#packages) 与ROS 2 套件同名,

输入以下命令, 下载“ 举例谈话者” 代码 :

##### Linux

``` console
$ wget https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_publisher/examples_rclpy_minimal_publisher/publisher_member_function.py
```

##### macOS

``` console
$ wget https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_publisher/examples_rclpy_minimal_publisher/publisher_member_function.py
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ curl -sk https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_publisher/examples_rclpy_minimal_publisher/publisher_member_function.py -o publisher_member_function.py
```

或于权壳中:

``` console
$ curl https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_publisher/examples_rclpy_minimal_publisher/publisher_member_function.py -o publisher_member_function.py
```

现在有一个新的文件命名 `publisher_member_function.py` 邻接 `__init__.py`.

使用您首选的文本编辑器打开文件 。

``` python
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

#### 2.1 审查守则

导入注释后的首行代码 [rclpy](https://docs.ros.org/en/rolling/p/rclpy/) 因此,它会 [节点](https://docs.ros.org/en/rolling/p/rclpy/api/node.html) 可使用类。

``` python
import rclpy
from rclpy.node import Node
```

下一个语句导入内置 [std_msgs/msg/String](https://docs.ros.org/en/rolling/p/std_msgs/msg/String.html) 消息类型,节点用于构建它传递的关于该主题的数据。

``` python
from std_msgs.msg import String
```

这些线条代表了节点的附属关系。 提醒注意, 附属关系必须添加到 `package.xml`,您将在下一节中这样做。

接下来, `MinimalPublisher` 类是创建的,它继承(或是一个子类) [节点](https://docs.ros.org/en/rolling/p/rclpy/api/node.html).

``` python
class MinimalPublisher(Node):
```

以下是该类建筑师的定义 。 `super().__init__` 呼唤 [节点](https://docs.ros.org/en/rolling/p/rclpy/api/node.html) 类的建构器, 并给出您的节点名称, 在这种情况下 `minimal_publisher`.

[create_publisher](https://docs.ros.org/en/rolling/p/rclpy/api/node.html#rclpy.node.Node.create_publisher) 声明节点发布类型消息 [std_msgs/msg/String](https://docs.ros.org/en/rolling/p/std_msgs/msg/String.html) (进口自 `std_msgs.msg` 模块),在一个命名的主题之上 `topic`,且“队列大小”为10。队列大小是必需的。 [服务质量](../../Concepts/Intermediate/About-Quality-of-Service-Settings.md) (QoS) 设定, 如果订阅者接收速度不够快, 则限制排队信件的数量 。

下一个 [create_timer](https://docs.ros.org/en/rolling/p/rclpy/api/node.html#rclpy.node.Node.create_timer) 用于创建每0.5秒执行一次的回调。 `self.i` 是一个在召回中使用的计数器。

``` python
def __init__(self):
    super().__init__('minimal_publisher')
    self.publisher_ = self.create_publisher(String, 'topic', 10)
    timer_period = 0.5  # seconds
    self.timer = self.create_timer(timer_period, self.timer_callback)
    self.i = 0
```

`timer_callback` 创建带有对应值的信件,将其发布,并打印到控制台 [get_logger()](https://docs.ros.org/en/rolling/p/rclpy/api/node.html#rclpy.node.Node.get_logger)’s [信息( )](https://docs.ros.org/en/rolling/p/rclpy/rclpy.impl.rcutils_logger.html#rclpy.impl.rcutils_logger.RcutilsLogger.info) 函数。

``` python
def timer_callback(self):
    msg = String()
    msg.data = 'Hello World: %d' % self.i
    self.publisher_.publish(msg)
    self.get_logger().info('Publishing: "%s"' % msg.data)
    self.i += 1
```

最后,确定了主要职能。

``` python
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

首先是 [rclpy](https://docs.ros.org/en/rolling/p/rclpy/) 库初始化,然后创建节点,然后“spins”节点(使用 [旋转( )](https://docs.ros.org/en/rolling/p/rclpy/api/init_shutdown.html#rclpy.spin))所以它的召回被称作.

<span id="add-dependencies"></span>

#### 2.2 增加依附关系

导航一个关卡返回 `ros2_ws/src/py_pubsub` 目录,其中 `setup.py`, `setup.cfg`,以及 `package.xml` 已经为您创建文件 。

打开 `package.xml` 与您的文本编辑器。

如本报告所述, [上一个教程](Creating-Your-First-ROS2-Package.md)中,确保填写 `<description>`, `<maintainer>` 财务报告和财务报告 `<license>` 标签 :

``` xml
<description>Examples of minimal publisher/subscriber using rclpy</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

在以上各行之后,添加与您节点的导入语句相对应的以下依赖性:

``` xml
<exec_depend>rclpy</exec_depend>
<exec_depend>std_msgs</exec_depend>
```

此声明软件包需要 [rclpy](https://docs.ros.org/en/rolling/p/rclpy/) 财务报告和财务报告 [std_msgs](https://docs.ros.org/en/rolling/p/std_msgs/) 当它的代码被执行时。

确保保存文件 。

<span id="add-an-entry-point"></span>

#### 2.3 增加一个切入点

打开 `setup.py` 文档。再次,匹配 `maintainer`, `maintainer_email`, `description` 财务报告和财务报告 `license` 字段为您 `package.xml`:

``` python
maintainer='YourName',
maintainer_email='you@email.com',
description='Examples of minimal publisher/subscriber using rclpy',
license='Apache License 2.0',
```

在下行中添加以下行 `console_scripts` 括号 [entry_points](https://setuptools.pypa.io/en/latest/userguide/entry_point.html) 字段 :

``` python
entry_points={
        'console_scripts': [
                'talker = py_pubsub.publisher_member_function:main',
        ],
},
```

不要忘记拯救。

<span id="check-setup-cfg"></span>

#### 2.4 检查设置。cfg

报告的内容 `setup.cfg` 文件应自动正确配置, 像这样 :

``` ini
[develop]
script_dir=$base/lib/py_pubsub
[install]
install_scripts=$base/lib/py_pubsub
```

这仅仅是在说 [设置工具](https://setuptools.pypa.io/en/latest/userguide) 将您的可执行文件放入 `lib`,因为 `ros2 run` 会在那里寻找他们。

您现在可以构建您的软件包, 源代码本地设置文件, 并运行它, 但让我们先创建用户节点, 这样您就可以在工作时看到完整的系统 。

<span id="write-the-subscriber-node"></span>

### 3 写入订阅者节点

返回到 `ros2_ws/src/py_pubsub/py_pubsub` 创建下一个节点。在终端中输入以下代码:

##### Linux

``` console
$ wget https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_subscriber/examples_rclpy_minimal_subscriber/subscriber_member_function.py
```

##### macOS

``` console
$ wget https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_subscriber/examples_rclpy_minimal_subscriber/subscriber_member_function.py
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ curl -sk https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_subscriber/examples_rclpy_minimal_subscriber/subscriber_member_function.py -o subscriber_member_function.py
```

或于权壳中:

``` console
$ curl https://raw.githubusercontent.com/ros2/examples/rolling/rclpy/topics/minimal_subscriber/examples_rclpy_minimal_subscriber/subscriber_member_function.py -o subscriber_member_function.py
```

现在目录应该有这些文件 :

``` console
__init__.py  publisher_member_function.py  subscriber_member_function.py
```

<span id="id4"></span>

#### 3.1 审查守则

打开 `subscriber_member_function.py` 与您的文本编辑器。

``` python
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

订阅者节点的代码与出版者几乎完全相同。 构建者创建的订阅者与出版者使用相同的参数。 [create_subscription](https://docs.ros.org/en/rolling/p/rclpy/api/node.html#rclpy.node.Node.create_subscription)。从 [主题教程](../Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md) ,出版商和订阅商使用的主题名称和消息类型必须匹配,以允许它们进行通信。

``` python
self.subscription = self.create_subscription(
    String,
    'topic',
    self.listener_callback,
    10)
```

用户的构建者和召回者并不包括任何定时器定义,因为它不需要定时器定义。 它的召回一旦收到消息就会被调用。

回调定义只是向控制台打印一条信息信息,连同它收到的数据。请回顾,出版商定义了 `msg.data = 'Hello World: %d' % self.i`

``` python
def listener_callback(self, msg):
    self.get_logger().info('I heard: "%s"' % msg.data)
```

那个... `main` 定义几乎完全相同,用订阅者取代了出版商的创建和旋转。

``` python
minimal_subscriber = MinimalSubscriber()

rclpy.spin(minimal_subscriber)
```

因为这个节点和出版商有相同的依赖关系,所以没有什么新内容可以补充 `package.xml`。该词 `setup.cfg` 文件也可以保持不变。

<span id="id5"></span>

#### 3.2 增加一个切入点

重新打开 `setup.py` ,并在出版商的切入点下添加订阅者节点的切入点。 [entry_points](https://setuptools.pypa.io/en/latest/userguide/entry_point.html) 字段现在应该是这样的:

``` python
entry_points={
        'console_scripts': [
                'talker = py_pubsub.publisher_member_function:main',
                'listener = py_pubsub.subscriber_member_function:main',
        ],
},
```

确保保存文件,然后你的酒吧/子系统应该准备好.

<span id="build-and-run"></span>

### 4 构建和运行

你可能已经拥有了 [rclpy](https://docs.ros.org/en/rolling/p/rclpy/) 财务报告和财务报告 [std_msgs](https://docs.ros.org/en/rolling/p/std_msgs/) 作为 ROS 2 系统的一部分安装的软件包。运行是好的做法 [rosdep](https://docs.ros.org/en/independent/api/rosdep/html/) (请检查date=中的日期值) [罗斯德教程](../Intermediate/Rosdep.md))在工作空间的根部(`ros2_ws`在建构前检查缺失的依赖性 :

##### Linux

``` console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

##### macOS

rosdep只运行在Linux上,所以可以提前跳到下一步.

##### Windows

rosdep只运行在Linux上,所以可以提前跳到下一步.

还在你工作空间的根部 `ros2_ws`,构建您的新软件包 :

##### Linux

``` console
$ colcon build --packages-select py_pubsub
```

##### macOS

``` console
$ colcon build --packages-select py_pubsub
```

##### Windows

``` console
$ colcon build --merge-install --packages-select py_pubsub
```

打开新终端, 导航到 `ros2_ws`,并源代码设置文件 :

##### Linux

``` console
$ source install/setup.bash
```

##### macOS

``` console
$ . install/setup.bash
```

##### Windows

``` console
$ call install/setup.bat
```

现在运行谈话者节点。终端应该开始每0.5秒发布一次信息信息,比如:

``` console
$ ros2 run py_pubsub talker
[info] [minimal_publisher]: publishing: "hello world: 0"
[info] [minimal_publisher]: publishing: "hello world: 1"
[info] [minimal_publisher]: publishing: "hello world: 2"
[info] [minimal_publisher]: publishing: "hello world: 3"
[info] [minimal_publisher]: publishing: "hello world: 4"
...
```

打开另一个终端, 从内部源出设置文件 `ros2_ws` 然后启动收听器节点。收听器将开始向控制台打印消息,从任何信息开始计算出版商当时的状态,就像这样:

``` console
$ ros2 run py_pubsub listener
[INFO] [minimal_subscriber]: I heard: "Hello World: 10"
[INFO] [minimal_subscriber]: I heard: "Hello World: 11"
[INFO] [minimal_subscriber]: I heard: "Hello World: 12"
[INFO] [minimal_subscriber]: I heard: "Hello World: 13"
[INFO] [minimal_subscriber]: I heard: "Hello World: 14"
```

输入 `Ctrl+C` 在每个终端中阻止节点旋转。

<span id="summary"></span>

## 小结

您创建了两个节点来在一个话题上发布和订阅数据。 在运行之前, 您会在软件包配置文件中添加它们的依赖性和切入点 。

<span id="next-steps"></span>

## 后续步骤

您接下来会使用服务/客户端模式创建另一个简单的ROS 2 软件包。 您也可以选择将其写入其中之一 [C++](Writing-A-Simple-Cpp-Service-And-Client.md) 或 时 间 [Python](Writing-A-Simple-Py-Service-And-Client.md).

<span id="related-content"></span>

## 相关内容

您可以在 Python 中写一个出版商和订阅商。 `minimal_publisher` 财务报告和财务报告 `minimal_subscriber` 软件包中 [ros2/examples](https://github.com/ros2/examples/tree/rolling/rclpy/topics) 复传.
