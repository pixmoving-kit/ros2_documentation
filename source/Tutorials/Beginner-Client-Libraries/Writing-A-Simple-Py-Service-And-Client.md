---
translation_status: machine_translated
source: Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Service-And-Client.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="writing-a-simple-service-and-client-python"></span> <span id="pysrvcli"></span>

# 编写简单的服务端与客户端（Python）

**目标：** 使用 Python 创建并运行服务和客户端节点.

**教程级别：** 入门

**用时：** 20分钟

<span id="background"></span>

## 背景

何时 [节点](../Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md) 使用 [服务](../Beginner-CLI-Tools/Understanding-ROS2-Services/Understanding-ROS2-Services.md),发送数据请求的节点称为客户端节点,响应请求的节点为服务节点。请求和响应的结构由一个 `.srv` 文档。

这里使用的例子是一个简单的整数加法系统;一个节点请求两个整数的总和,另一个响应结果.

<span id="prerequisites"></span>

## 前提条件

在之前的教程中,你学会了如何 [创建工作空间](Creating-A-Workspace/Creating-A-Workspace.md) 财务报告和财务报告 [创建软件包](Creating-Your-First-ROS2-Package.md).

<span id="tasks"></span>

## 操作步骤

<span id="create-a-package"></span>

### 1 创建软件包

打开一个新的终端 [源代码 ROS 2 安装](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md) 这样一来 `ros2` 命令会起作用的。

导航到 `ros2_ws` 在 a 中创建目录 [上一个教程](Creating-A-Workspace/Creating-A-Workspace.md#new-directory).

回顾 应在 `src` 目录,不是工作空间的根。导航到 `ros2_ws/src` 并创建新软件包 :

``` console
$ ros2 pkg create --build-type ament_python --license Apache-2.0 py_srvcli --dependencies rclpy example_interfaces
```

您的终端将返回一个消息, 以验证您的软件包的创建 `py_srvcli` 以及所有必要的文件和文件夹。

那个... `--dependencies` 参数将自动添加必要的依赖线到 `package.xml`. `example_interfaces` 是包含以下内容的软件包 [.srv 文件](https://github.com/ros2/example_interfaces/blob/rolling/srv/AddTwoInts.srv) 您需要组织您的请求和答复:

``` bash
int64 a
int64 b
---
int64 sum
```

前两行是请求的参数,短线以下是响应.

<span id="update-package-xml"></span>

#### 1.1 最新情况 `package.xml`

因为你用了 `--dependencies` 在创建软件包时,您不需要手动添加依赖性到 `package.xml`.

但是,与往常一样,确保添加描述、维护者电子邮件和姓名,并给信息发放许可证。 `package.xml`.

``` xml
<description>Python client server tutorial</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

<span id="update-setup-py"></span>

#### 1.2 最新情况 `setup.py`

将同样信息添加到 `setup.py` 用于文档 `maintainer`, `maintainer_email`, `description` 财务报告和财务报告 `license` 字段 :

``` python
maintainer='Your Name',
maintainer_email='you@email.com',
description='Python client server tutorial',
license='Apache License 2.0',
```

<span id="write-the-service-node"></span>

### 2 写入服务节点

内侧 `ros2_ws/src/py_srvcli/py_srvcli` 目录,创建名为新文件 `service_member_function.py` 并粘贴下列编码:

``` python
from example_interfaces.srv import AddTwoInts

import rclpy
from rclpy.node import Node


class MinimalService(Node):

    def __init__(self):
        super().__init__('minimal_service')
        self.srv = self.create_service(AddTwoInts, 'add_two_ints', self.add_two_ints_callback)

    def add_two_ints_callback(self, request, response):
        response.sum = request.a + request.b
        self.get_logger().info('Incoming request\na: %d b: %d' % (request.a, request.b))

        return response


def main():
    rclpy.init()

    minimal_service = MinimalService()

    rclpy.spin(minimal_service)

    rclpy.shutdown()


if __name__ == '__main__':
    main()
```

<span id="examine-the-code"></span>

#### 2.1 审查守则

第一个 `import` 语句导入 `AddTwoInts` 服务类型 `example_interfaces` 软件包。以下 `import` 语句导入 ROS 2 Python 客户端库,特别是 `Node` 班级。

``` python
from example_interfaces.srv import AddTwoInts

import rclpy
from rclpy.node import Node
```

那个... `MinimalService` 类构造器以名称初始化节点 `minimal_service`。然后,它创建一个服务并定义类型、名称和召回。

``` python
def __init__(self):
    super().__init__('minimal_service')
    self.srv = self.create_service(AddTwoInts, 'add_two_ints', self.add_two_ints_callback)
```

服务召回的定义接收了请求数据,总和,并返回总和作为响应.

``` python
def add_two_ints_callback(self, request, response):
    response.sum = request.a + request.b
    self.get_logger().info('Incoming request\na: %d b: %d' % (request.a, request.b))

    return response
```

最后,主课初始化ROS 2 Python 客户端库,即时化 `MinimalService` 类来创建服务节点,并旋转节点来处理调回。

<span id="add-an-entry-point"></span>

#### 2.2 增加一个切入点

允许 `ros2 run` 命令来运行您的节点,您必须添加切入点到 `setup.py` 页:1 `ros2_ws/src/py_srvcli` 目录).

将下行添加到 `'console_scripts':` 括号 :

``` python
'service = py_srvcli.service_member_function:main',
```

<span id="write-the-client-node"></span>

### 3 写入客户端节点

内侧 `ros2_ws/src/py_srvcli/py_srvcli` 目录,创建名为新文件 `client_member_function.py` 并粘贴下列编码:

``` python
import sys

from example_interfaces.srv import AddTwoInts
import rclpy
from rclpy.node import Node


class MinimalClientAsync(Node):

    def __init__(self):
        super().__init__('minimal_client_async')
        self.cli = self.create_client(AddTwoInts, 'add_two_ints')
        while not self.cli.wait_for_service(timeout_sec=1.0):
            self.get_logger().info('service not available, waiting again...')
        self.req = AddTwoInts.Request()

    def send_request(self, a, b):
        self.req.a = a
        self.req.b = b
        return self.cli.call_async(self.req)


def main():
    rclpy.init()

    minimal_client = MinimalClientAsync()
    future = minimal_client.send_request(int(sys.argv[1]), int(sys.argv[2]))
    rclpy.spin_until_future_complete(minimal_client, future)
    response = future.result()
    minimal_client.get_logger().info(
        'Result of add_two_ints: for %d + %d = %d' %
        (int(sys.argv[1]), int(sys.argv[2]), response.sum))

    minimal_client.destroy_node()
    rclpy.shutdown()


if __name__ == '__main__':
    main()
```

<span id="id1"></span>

#### 3.1 审查守则

和服务代码一样,我们先 `import` 拥有必要的库。

``` python
import sys

from example_interfaces.srv import AddTwoInts
import rclpy
from rclpy.node import Node
```

那个... `MinimalClientAsync` 类构造器以名称初始化节点 `minimal_client_async`。构建器定义创建了与服务节点相同类型和名称的客户端。类型和名称必须匹配客户端和服务才能进行通信。 `while` 循环在构建器中检查一个匹配客户端类型和名称的服务是否有一次可用。 `AddTwoInts` 请求对象。

``` python
def __init__(self):
    super().__init__('minimal_client_async')
    self.cli = self.create_client(AddTwoInts, 'add_two_ints')
    while not self.cli.wait_for_service(timeout_sec=1.0):
        self.get_logger().info('service not available, waiting again...')
    self.req = AddTwoInts.Request()
```

构造器下面是 `send_request` 方法,它会发送请求并旋转,直到它收到答复或失败。

``` python
def send_request(self, a, b):
    self.req.a = a
    self.req.b = b
    return self.cli.call_async(self.req)
```

我们终于有了 `main` 方法,用于构建一个 `MinimalClientAsync` 对象,使用通过的命令行参数发送请求 `rclpy.spin_until_future_complete` 以等待结果,并记录结果。

``` python
def main():
    rclpy.init()

    minimal_client = MinimalClientAsync()
    future = minimal_client.send_request(int(sys.argv[1]), int(sys.argv[2]))
    rclpy.spin_until_future_complete(minimal_client, future)
    response = future.result()
    minimal_client.get_logger().info(
        'Result of add_two_ints: for %d + %d = %d' %
        (int(sys.argv[1]), int(sys.argv[2]), response.sum))

    minimal_client.destroy_node()
    rclpy.shutdown()
```

> **警告**
>
> 不使用 `rclpy.spin_until_future_complete` 更多详情请参见 [同步僵局文章](../../How-To-Guides/Sync-Vs-Async.md).

<span id="id2"></span>

#### 3.2 增加一个切入点

与服务节点一样,您还需要添加一个切入点才能运行客户端节点.

那个... `entry_points` 区域 `setup.py` 文件应该像这样 :

``` python
entry_points={
    'console_scripts': [
        'service = py_srvcli.service_member_function:main',
        'client = py_srvcli.client_member_function:main',
    ],
},
```

<span id="build-and-run"></span>

### 4 构建和运行

运行是好的做法 `rosdep` 在工作空间的根部(`ros2_ws`在建构前检查缺失的依赖性 :

##### Linux

``` console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

##### macOS

rosdep只运行在Linux上,所以可以提前跳到下一步.

##### Windows

rosdep只运行在Linux上,所以可以提前跳到下一步.

导航回你工作空间的根, `ros2_ws`,并构建您的新软件包:

``` console
$ colcon build --packages-select py_srvcli
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

现在运行服务节点:

``` console
$ ros2 run py_srvcli service
```

节点将等待客户端的要求.

打开另一个终端, 从内部源代码创建文件 `ros2_ws` 。启动客户端节点,然后用空格分隔任意两个整数。如果您选择 `2` 财务报告和财务报告 `3`例如,客户会收到这样的回复:

``` console
$ ros2 run py_srvcli client 2 3
[INFO] [minimal_client_async]: Result of add_two_ints: for 2 + 3 = 5
```

返回您的服务节点运行所在的终端。 您会看到它收到请求时已发布日志消息 :

``` console
[INFO] [minimal_service]: Incoming request
a: 2 b: 3
```

输入 `Ctrl+C` 在服务器终端中阻止节点旋转。

<span id="summary"></span>

## 小结

您创建了两个节点来通过服务请求和响应数据。 您在软件包配置文件中添加了它们的依赖性和可执行性, 以便您构建和运行它们, 允许您在工作时看到一个服务/ 客户端系统 。

<span id="next-steps"></span>

## 后续步骤

在最近几次的辅导中,您一直在使用接口来传递数据,以跨越主题和服务。接下来,您将学习如何 [创建自定义接口](Custom-ROS2-Interfaces.md).

<span id="related-content"></span>

## 相关内容

- 您可以在 Python 中写一个服务和客户端有几种方法; 请检查 `minimal_client` 财务报告和财务报告 `minimal_service` 软件包中 [ros2/examples](https://github.com/ros2/examples/tree/rolling/rclpy/services) 复传.

- 在这个教程中,你使用了 `call_async()` API 在您的客户端节点中调用服务。 Python 有另一个服务名为 API 。 我们不建议使用同步调用, 但是如果您想要了解更多有关这些调用的信息, 请读取指南 。 [同步对同步客户端](../../How-To-Guides/Sync-Vs-Async.md).
