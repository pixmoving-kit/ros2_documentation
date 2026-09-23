---
translation_status: machine_translated
source: Tutorials/Intermediate/Writing-an-Action-Server-Client/Py.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="writing-an-action-server-and-client-python"></span> <span id="actionspy"></span>

# 编写动作服务端与客户端（Python）

**目标：** 在 Python 中执行动作服务器和客户端 。

**教程级别：** 中级

**用时：** 15分钟

<span id="background"></span>

## 背景

动作是ROS 2中的一种同步通信形式. *行动客户* 发送目标请求到 *动作服务器*. *动作服务器* 将目标反馈和结果发送给 *动作客户端*.

<span id="prerequisites"></span>

## 前提条件

你需要那个... `action_tutorials_interfaces` 软件包和 `Fibonacci.action` 在上一个教程中定义的界面, [创建动作](../Creating-an-Action.md).

<span id="tasks"></span>

## 操作步骤

<span id="writing-an-action-server"></span>

### 1 写入动作服务器

让我们集中力量写一个动作服务器,利用我们创建的动作来计算Fibonacci序列 [创建动作](../Creating-an-Action.md) 教学。

直到现在,你已经创建了软件包并使用了 `ros2 run` 来运行您的节点。 但是, 要使这个教程中的事情简单化, 我们将会将动作服务器扩展为单个文件 。 如果您想要看到操作教程的完整软件包是什么样子的, 请检查 [action_tutorials](https://github.com/ros2/demos/tree/rolling/action_tutorials).

在您的主目录中打开新文件, 让我们称之为 `fibonacci_action_server.py`,并添加以下代码:

``` python
import rclpy
from rclpy.action import ActionServer
from rclpy.node import Node

from action_tutorials_interfaces.action import Fibonacci


class FibonacciActionServer(Node):

    def __init__(self):
        super().__init__('fibonacci_action_server')
        self._action_server = ActionServer(
            self,
            Fibonacci,
            'fibonacci',
            self.execute_callback)

    def execute_callback(self, goal_handle):
        self.get_logger().info('Executing goal...')
        result = Fibonacci.Result()
        return result


def main(args=None):
    rclpy.init(args=args)

    fibonacci_action_server = FibonacciActionServer()

    rclpy.spin(fibonacci_action_server)


if __name__ == '__main__':
    main()
```

第8行定义了类 `FibonacciActionServer` 那是一个子类 `Node`。该类的初始化是通过调用 `Node` 构造器,命名我们的节点 `fibonacci_action_server`:

``` python
        super().__init__('fibonacci_action_server')
```

在构造器中,我们还即时开发了一个新的动作服务器:

``` python
        self._action_server = ActionServer(
            self,
            Fibonacci,
            'fibonacci',
            self.execute_callback)
```

一个动作服务器需要四个参数:

1.  一个ROS 2节点将动作服务器添加到: `self`.

2.  动作类型 : `Fibonacci` (输入于第5行).

3.  动作名称 : `'fibonacci'`.

4.  用于执行公认目标的回调功能: `self.execute_callback`。这个召回 **必须** 返回动作类型的结果信息。

我们还定义一个 `execute_callback` 我们班上的方法:

``` python
    def execute_callback(self, goal_handle):
        self.get_logger().info('Executing goal...')
        result = Fibonacci.Result()
        return result
```

一旦目标被接受,将要求使用这种方法来执行目标。

让我们尝试运行动作服务器:

##### Linux

``` console
$ python3 fibonacci_action_server.py
```

##### macOS

``` console
$ python3 fibonacci_action_server.py
```

##### Windows

``` console
$ python fibonacci_action_server.py
```

在另一个终端,我们可以使用命令行接口发送一个目标:

``` console
$ ros2 action send_goal fibonacci action_tutorials_interfaces/action/Fibonacci "{order: 5}"
```

在运行动作服务器的终端中,您应该看到一个已登录的信息“ 执行目标... ” , 并随后警告该目标状态尚未设定。 默认情况下, 如果执行中未设定目标处理状态, 它会假设 *已中止* 状态。

我们可以用这个方法 [成功( )](http://docs.ros2.org/latest/api/rclpy/api/actions.html#rclpy.action.server.ServerGoalHandle.succeed) 在目标控件上表明该目标是成功的:

``` python
    def execute_callback(self, goal_handle):
        self.get_logger().info('Executing goal...')
        goal_handle.succeed()
        result = Fibonacci.Result()
        return result
```

现在,如果你重新启动动作服务器并发送另一个目标,你应该看到目标完成状态 `SUCCEEDED`.

让我们实际计算目标执行并返回所要求的Fibonaci序列:

``` python
    def execute_callback(self, goal_handle):
        self.get_logger().info('Executing goal...')

        sequence = [0, 1]

        for i in range(1, goal_handle.request.order):
            sequence.append(sequence[i] + sequence[i-1])

        goal_handle.succeed()

        result = Fibonacci.Result()
        result.sequence = sequence
        return result
```

在计算序列后,我们在返回前将其分配到结果信息字段.

重新启动动作服务器并发送另一个目标。 您应该看到目标以适当的结果序列完成 。

<span id="publishing-feedback"></span>

#### 1.2 发表反馈意见

动作的好东西之一是在目标执行过程中向动作客户端提供反馈的能力。 我们可以通过调用目标控件来让动作服务器发布动作客户端的反馈 。 [publish_feedback()](http://docs.ros2.org/latest/api/rclpy/api/actions.html#rclpy.action.server.ServerGoalHandle.publish_feedback) 方法。

我们将取代 `sequence` 变量,并使用反馈消息来存储序列。每次更新搜索中反馈消息后,我们都会发布反馈消息并睡眠以产生戏剧效果:

``` python
import time

import rclpy
from rclpy.action import ActionServer
from rclpy.node import Node

from action_tutorials_interfaces.action import Fibonacci


class FibonacciActionServer(Node):

    def __init__(self):
        super().__init__('fibonacci_action_server')
        self._action_server = ActionServer(
            self,
            Fibonacci,
            'fibonacci',
            self.execute_callback)

    def execute_callback(self, goal_handle):
        self.get_logger().info('Executing goal...')

        feedback_msg = Fibonacci.Feedback()
        feedback_msg.partial_sequence = [0, 1]

        for i in range(1, goal_handle.request.order):
            feedback_msg.partial_sequence.append(
                feedback_msg.partial_sequence[i] + feedback_msg.partial_sequence[i-1])
            self.get_logger().info('Feedback: {0}'.format(feedback_msg.partial_sequence))
            goal_handle.publish_feedback(feedback_msg)
            time.sleep(1)

        goal_handle.succeed()

        result = Fibonacci.Result()
        result.sequence = feedback_msg.partial_sequence
        return result


def main(args=None):
    rclpy.init(args=args)

    fibonacci_action_server = FibonacciActionServer()

    rclpy.spin(fibonacci_action_server)


if __name__ == '__main__':
    main()
```

在重新启动动作服务器后,我们可以通过使用命令行工具确认反馈现在已经发布。 `--feedback` 选项 :

``` console
$ ros2 action send_goal --feedback fibonacci action_tutorials_interfaces/action/Fibonacci "{order: 5}"
```

<span id="writing-an-action-client"></span>

### 2 写入动作客户端

我们也会将动作客户端扩展为单一文件。打开新文件,让我们称之为 `fibonacci_action_client.py`,并添加以下锅炉板代码:

``` python
import rclpy
from rclpy.action import ActionClient
from rclpy.node import Node

from action_tutorials_interfaces.action import Fibonacci


class FibonacciActionClient(Node):

    def __init__(self):
        super().__init__('fibonacci_action_client')
        self._action_client = ActionClient(self, Fibonacci, 'fibonacci')

    def send_goal(self, order):
        goal_msg = Fibonacci.Goal()
        goal_msg.order = order

        self._action_client.wait_for_server()

        return self._action_client.send_goal_async(goal_msg)


def main(args=None):
    rclpy.init(args=args)

    action_client = FibonacciActionClient()

    future = action_client.send_goal(10)

    rclpy.spin_until_future_complete(action_client, future)


if __name__ == '__main__':
    main()
```

我们已定义了一个阶级 `FibonacciActionClient` 那是一个子类 `Node`。该类的初始化是通过调用 `Node` 构造器,命名我们的节点 `fibonacci_action_client`:

``` python
        super().__init__('fibonacci_action_client')
```

同样在类构建器中,我们使用上一个教程上的自定义动作客户端 [创建动作](../Creating-an-Action.md):

``` python
        self._action_client = ActionClient(self, Fibonacci, 'fibonacci')
```

我们创建一个 `ActionClient` 提出三个论点:

1.  一个ROS 2节点将动作客户端添加到: `self`

2.  动作类型 : `Fibonacci`

3.  动作名称 : `'fibonacci'`

我们的动作客户端将能够与相同动作名称和类型的动作服务器进行通信.

我们还定义了一种方法 `send_goal` 输入 `FibonacciActionClient` 类 :

``` python
    def send_goal(self, order):
        goal_msg = Fibonacci.Goal()
        goal_msg.order = order

        self._action_client.wait_for_server()

        return self._action_client.send_goal_async(goal_msg)
```

此方法等待动作服务器可用, 然后向服务器发送一个目标。 它返回一个我们以后可以等待的未来 。

类定义后,我们定义一个函数 `main()` 并创造出我们 `FibonacciActionClient` 节点,然后发送一个目标,等待目标完成。

最后,我们叫 `main()` 在我们的Python程序入口处。

让我们先运行先前创建的动作服务器,

##### Linux

``` console
$ python3 fibonacci_action_server.py
```

##### macOS

``` console
$ python3 fibonacci_action_server.py
```

##### Windows

``` console
$ python fibonacci_action_server.py
```

在另一个终端,运行动作客户端.

##### Linux

``` console
$ python3 fibonacci_action_client.py
```

##### macOS

``` console
$ python3 fibonacci_action_client.py
```

##### Windows

``` console
$ python fibonacci_action_client.py
```

您应该看到动作服务器在成功执行目标时打印的消息 :

``` console
[INFO] [fibonacci_action_server]: Executing goal...
[INFO] [fibonacci_action_server]: Feedback: array('i', [0, 1, 1])
[INFO] [fibonacci_action_server]: Feedback: array('i', [0, 1, 1, 2])
[INFO] [fibonacci_action_server]: Feedback: array('i', [0, 1, 1, 2, 3])
[INFO] [fibonacci_action_server]: Feedback: array('i', [0, 1, 1, 2, 3, 5])
~ etc.
```

动作客户端应该启动,然后快速完成。 此时,我们有一个运行中的动作客户端,但我们没有看到任何结果或获得任何反馈。

<span id="getting-a-result"></span>

#### 2.1 取得结果

因此,我们可以发送一个目标,但是我们如何知道它何时完成?我们可以用几个步骤获得结果信息。首先,我们需要为所发送的目标获得一个目标手柄,然后,我们可以使用目标手柄来请求结果。

以下是这个例子的完整代码:

``` python
import rclpy
from rclpy.action import ActionClient
from rclpy.node import Node

from action_tutorials_interfaces.action import Fibonacci


class FibonacciActionClient(Node):

    def __init__(self):
        super().__init__('fibonacci_action_client')
        self._action_client = ActionClient(self, Fibonacci, 'fibonacci')

    def send_goal(self, order):
        goal_msg = Fibonacci.Goal()
        goal_msg.order = order

        self._action_client.wait_for_server()

        self._send_goal_future = self._action_client.send_goal_async(goal_msg)

        self._send_goal_future.add_done_callback(self.goal_response_callback)

    def goal_response_callback(self, future):
        goal_handle = future.result()
        if not goal_handle.accepted:
            self.get_logger().info('Goal rejected :(')
            return

        self.get_logger().info('Goal accepted :)')

        self._get_result_future = goal_handle.get_result_async()
        self._get_result_future.add_done_callback(self.get_result_callback)

    def get_result_callback(self, future):
        result = future.result().result
        self.get_logger().info('Result: {0}'.format(result.sequence))
        rclpy.shutdown()


def main(args=None):
    rclpy.init(args=args)

    action_client = FibonacciActionClient()

    action_client.send_goal(10)

    rclpy.spin(action_client)


if __name__ == '__main__':
    main()
```

那个... [ActionClient.send_goal_async()](http://docs.ros2.org/latest/api/rclpy/api/actions.html#rclpy.action.client.ActionClient.send_goal_async) 方法返回未来到目标控件。首先,当未来完成时,我们登记回调:

``` python
        self._send_goal_future.add_done_callback(self.goal_response_callback)
```

请注意, 当动作服务器接受或拒绝目标请求时, 未来将完成。 让我们看看 `goal_response_callback` 我们可以检查一下目标是否被拒绝,然后提前返回,因为我们知道没有结果:

``` python
    def goal_response_callback(self, future):
        goal_handle = future.result()
        if not goal_handle.accepted:
            self.get_logger().info('Goal rejected :(')
            return

        self.get_logger().info('Goal accepted :)')
```

现在,我们有了目标手柄, 我们可以用它来要求结果的方法 [get_result_async()](http://docs.ros2.org/latest/api/rclpy/api/actions.html#rclpy.action.client.ClientGoalHandle.get_result_async)类似发送目标,我们将会有一个在结果准备好后完成的未来。 让我们像对目标的反应一样登记回调:

``` python
        self._get_result_future = goal_handle.get_result_async()
        self._get_result_future.add_done_callback(self.get_result_callback)
```

在回话中,我们登录结果序列 并关闭ROS 2 一个干净的退出:

``` python
    def get_result_callback(self, future):
        result = future.result().result
        self.get_logger().info('Result: {0}'.format(result.sequence))
        rclpy.shutdown()
```

在单独的终端中运行动作服务器,请继续运行我们的Fibonacci动作客户端!

##### Linux

``` console
$ python3 fibonacci_action_client.py
```

##### macOS

``` console
$ python3 fibonacci_action_client.py
```

##### Windows

``` console
$ python fibonacci_action_client.py
```

您应该看到被接受的目标及最终结果的登录信息 。

<span id="getting-feedback"></span>

#### 2.2 获得反馈

我们的动作客户端可以发送目标。 不错! 但如果我们能从动作服务器获得一些关于我们发送的目标的反馈, 那将会很好 。

以下是这个例子的完整代码:

``` python
import rclpy
from rclpy.action import ActionClient
from rclpy.node import Node

from action_tutorials_interfaces.action import Fibonacci


class FibonacciActionClient(Node):

    def __init__(self):
        super().__init__('fibonacci_action_client')
        self._action_client = ActionClient(self, Fibonacci, 'fibonacci')

    def send_goal(self, order):
        goal_msg = Fibonacci.Goal()
        goal_msg.order = order

        self._action_client.wait_for_server()

        self._send_goal_future = self._action_client.send_goal_async(goal_msg, feedback_callback=self.feedback_callback)

        self._send_goal_future.add_done_callback(self.goal_response_callback)

    def goal_response_callback(self, future):
        goal_handle = future.result()
        if not goal_handle.accepted:
            self.get_logger().info('Goal rejected :(')
            return

        self.get_logger().info('Goal accepted :)')

        self._get_result_future = goal_handle.get_result_async()
        self._get_result_future.add_done_callback(self.get_result_callback)

    def get_result_callback(self, future):
        result = future.result().result
        self.get_logger().info('Result: {0}'.format(result.sequence))
        rclpy.shutdown()

    def feedback_callback(self, feedback_msg):
        feedback = feedback_msg.feedback
        self.get_logger().info('Received feedback: {0}'.format(feedback.partial_sequence))


def main(args=None):
    rclpy.init(args=args)

    action_client = FibonacciActionClient()

    action_client.send_goal(10)

    rclpy.spin(action_client)


if __name__ == '__main__':
    main()
```

以下是反馈消息的回调功能:

``` python
    def feedback_callback(self, feedback_msg):
        feedback = feedback_msg.feedback
        self.get_logger().info('Received feedback: {0}'.format(feedback.partial_sequence))
```

在回话中,我们得到回信部分,并打印 `partial_sequence` 字段到屏幕。

我们需要用动作客户端注册回调。 当我们发送目标时, 还要将回调传递给动作客户端 :

``` python
        self._send_goal_future = self._action_client.send_goal_async(goal_msg, feedback_callback=self.feedback_callback)
```

我们都准备好了。如果我们运行我们的动作客户端,你应该看到反馈被打印到屏幕上。

<span id="summary"></span>

## 小结

在这个教程中,您将一个 Python 动作服务器和动作客户端行逐行组合起来,并配置它们来交换目标,反馈和结果.

<span id="related-content"></span>

## 相关内容

- 您可以在 Python 中写一个动作服务器和客户端。 请检查 `minimal_action_server` 财务报告和财务报告 `minimal_action_client` 软件包中 [ros2/examples](https://github.com/ros2/examples/tree/rolling/rclpy/actions) 复传.

- 欲了解关于ROS行动的更详细资料,请参见: [设计文章](http://design.ros2.org/articles/actions.html).
