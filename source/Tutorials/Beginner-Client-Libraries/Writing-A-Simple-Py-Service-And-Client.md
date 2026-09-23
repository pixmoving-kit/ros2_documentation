<span id="writing-a-simple-service-and-client-python"></span> <span id="pysrvcli"></span>
# 编写简单的服务端和客户端（Python）

**目标：** 使用 Python 创建并运行服务端和客户端节点。

**教程级别：** 初学者

**预计用时：** 20 分钟

<span id="background"></span>
## 背景

[节点](../Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md)通过[服务](../Beginner-CLI-Tools/Understanding-ROS2-Services/Understanding-ROS2-Services.md)通信时，发送数据请求的节点称为客户端节点，响应请求的节点称为服务端节点。请求和响应的结构由 `.srv` 文件决定。

本例实现简单的整数加法系统：一个节点请求计算两个整数的和，另一个返回结果。

<span id="prerequisites"></span>
## 前提条件

此前教程介绍了[创建工作空间](Creating-A-Workspace/Creating-A-Workspace.md)和[创建软件包](Creating-Your-First-ROS2-Package.md)。

<span id="tasks"></span>
## 操作步骤

<span id="create-a-package"></span>
### 1 创建软件包

打开新终端，[加载 ROS 2 安装环境](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)，使 `ros2` 命令可用。

进入[此前创建](Creating-A-Workspace/Creating-A-Workspace.md#new-directory)的 `ros2_ws` 工作空间。软件包应放在 `src` 中，而非工作空间根目录。进入 `ros2_ws/src` 并创建新软件包：

```console
$ ros2 pkg create --build-type ament_python --license Apache-2.0 py_srvcli --dependencies rclpy example_interfaces
```

终端会确认已创建 `py_srvcli` 及其必需文件和目录。

`--dependencies` 会自动将所需依赖写入 `package.xml`。`example_interfaces` 包含本例定义请求和响应结构所需的 [.srv 文件](https://github.com/ros2/example_interfaces/blob/rolling/srv/AddTwoInts.srv)：

```bash
int64 a
int64 b
---
int64 sum
```

前两行是请求参数，分隔线下方是响应。

<span id="update-package-xml"></span>
#### 1.1 更新 package.xml

创建时使用了 `--dependencies`，所以无需手动添加依赖。不过，仍需填写软件包说明、维护者邮箱和姓名，以及许可证：

```xml
<description>Python client server tutorial</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

<span id="update-setup-py"></span>
#### 1.2 更新 setup.py

在 `setup.py` 的 `maintainer`、`maintainer_email`、`description` 和 `license` 字段中填写相同信息：

```python
maintainer='Your Name',
maintainer_email='you@email.com',
description='Python client server tutorial',
license='Apache License 2.0',
```

<span id="write-the-service-node"></span>
### 2 编写服务端节点

在 `ros2_ws/src/py_srvcli/py_srvcli` 中创建 `service_member_function.py`，粘贴以下代码：

```python
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
#### 2.1 分析代码

第一条 `import` 从 `example_interfaces` 导入 `AddTwoInts` 服务类型，接下来的语句导入 ROS 2 Python 客户端库及其 `Node` 类：

```python
from example_interfaces.srv import AddTwoInts

import rclpy
from rclpy.node import Node
```

`MinimalService` 构造函数初始化名为 `minimal_service` 的节点，再创建服务，指定服务类型、名称和回调：

```python
def __init__(self):
    super().__init__('minimal_service')
    self.srv = self.create_service(AddTwoInts, 'add_two_ints', self.add_two_ints_callback)
```

服务回调接收请求数据，计算两数之和，并将结果作为响应返回：

```python
def add_two_ints_callback(self, request, response):
    response.sum = request.a + request.b
    self.get_logger().info('Incoming request\na: %d b: %d' % (request.a, request.b))

    return response
```

最后，主函数初始化 ROS 2 Python 客户端库，实例化 `MinimalService` 创建服务端节点，并通过 spin 处理回调。

<span id="add-an-entry-point"></span>
#### 2.2 添加入口点

为了通过 `ros2 run` 运行节点，必须在 `ros2_ws/src/py_srvcli/setup.py` 中添加入口点。在 `'console_scripts':` 对应列表中加入：

```python
'service = py_srvcli.service_member_function:main',
```

<span id="write-the-client-node"></span>
### 3 编写客户端节点

在 `ros2_ws/src/py_srvcli/py_srvcli` 中创建 `client_member_function.py`，粘贴以下代码：

```python
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
#### 3.1 分析代码

与服务端一样，先导入所需库：

```python
import sys

from example_interfaces.srv import AddTwoInts
import rclpy
from rclpy.node import Node
```

`MinimalClientAsync` 构造函数初始化名为 `minimal_client_async` 的节点，创建与服务端类型和名称一致的客户端。两者必须匹配才能通信。构造函数中的 `while` 循环每秒检查一次是否已有匹配的服务，最后创建 `AddTwoInts` 请求对象。

```python
def __init__(self):
    super().__init__('minimal_client_async')
    self.cli = self.create_client(AddTwoInts, 'add_two_ints')
    while not self.cli.wait_for_service(timeout_sec=1.0):
        self.get_logger().info('service not available, waiting again...')
    self.req = AddTwoInts.Request()
```

构造函数下方的 `send_request` 方法填写并发送请求，返回用于等待响应或失败的 future：

```python
def send_request(self, a, b):
    self.req.a = a
    self.req.b = b
    return self.cli.call_async(self.req)
```

`main` 创建 `MinimalClientAsync` 对象，使用命令行传入的参数发送请求，再调用 `rclpy.spin_until_future_complete` 等待结果，并记录结果日志：

```python
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

!!! warning "警告"
    不要在 ROS 2 回调中使用 `rclpy.spin_until_future_complete`。更多说明见[同步调用与死锁](../../How-To-Guides/Sync-Vs-Async.md)。

<span id="id2"></span>
#### 3.2 添加入口点

与服务端一样，客户端也需要入口点才能运行。`setup.py` 的 `entry_points` 应为：

```python
entry_points={
    'console_scripts': [
        'service = py_srvcli.service_member_function:main',
        'client = py_srvcli.client_member_function:main',
    ],
},
```

<span id="build-and-run"></span>
### 4 构建并运行

推荐构建前在工作空间根目录 `ros2_ws` 运行 `rosdep`，检查缺失依赖。

**Linux**

```console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

**macOS 和 Windows**

本教程的 rosdep 步骤仅适用于 Linux，可跳到下一步。

返回工作空间根目录 `ros2_ws`，构建新软件包：

```console
$ colcon build --packages-select py_srvcli
```

打开新终端，进入 `ros2_ws` 并加载环境设置文件。

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

运行服务端节点：

```console
$ ros2 run py_srvcli service
```

节点将等待客户端请求。

打开另一个终端，再次从 `ros2_ws` 加载环境。启动客户端节点，在命令后添加两个以空格分隔的整数。例如输入 `2` 和 `3`，客户端会收到：

```console
$ ros2 run py_srvcli client 2 3
[INFO] [minimal_client_async]: Result of add_two_ints: for 2 + 3 = 5
```

回到服务端终端，可以看到它收到请求时输出的日志：

```console
[INFO] [minimal_service]: Incoming request
a: 2 b: 3
```

在服务端终端按 `Ctrl+C` 停止节点。

<span id="summary"></span>
## 小结

你创建了两个通过服务发送请求和响应数据的节点，将依赖和可执行程序配置加入软件包文件，完成构建和运行，并观察了服务端/客户端系统的工作方式。

<span id="next-steps"></span>
## 后续步骤

最近几篇教程使用接口通过话题和服务传递数据。接下来学习[创建自定义接口](Custom-ROS2-Interfaces.md)。

<span id="related-content"></span>
## 相关内容

- Python 服务端和客户端有多种写法，可参阅 [ros2/examples](https://github.com/ros2/examples/tree/rolling/rclpy/services) 中的 `minimal_client` 和 `minimal_service` 软件包。
- 本教程在客户端使用 `call_async()` 调用服务。Python 还提供同步服务调用 API，但不推荐使用。更多信息见[同步与异步客户端](../../How-To-Guides/Sync-Vs-Async.md)。
