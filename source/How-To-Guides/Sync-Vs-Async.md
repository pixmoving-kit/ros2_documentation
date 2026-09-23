---
translation_status: machine_translated
source: How-To-Guides/Sync-Vs-Async.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="synchronous-vs-asynchronous-service-clients"></span> <span id="syncasync"></span>

# 同步与异步服务客户端

**级别 :** 中级

**用时：** 10分钟

<span id="introduction"></span>

## 导言

本指南旨在提醒用户与Python同步服务客户端相关的风险 `call()` API. 在同步调用服务时很容易造成僵局, 所以我们不建议使用 `call()`.

我们以实例说明如何使用 `call()` 对于有经验的用户来说,他们希望使用同步电话,并意识到各种陷阱,这是正确无误的。 我们还强调随之而来的陷入僵局的可能情景。

由于我们建议避免同步呼叫,本指南还将述及所建议替代方式的特征和使用,即同步呼叫(Async calls)`call_async()`).

C++服务呼叫API只可用async,因此本指南中的比较和示例与Python服务和客户端相关. 这里给出的async的定义一般适用于C++,但有一些例外.

<span id="synchronous-calls"></span>

## 1次同步通话

同步客户端在向服务发送请求时会屏蔽调用线程, 直到收到回复; 调用时该线程上不会发生其他事。 调用需要任意的时间才能完成。 完成后, 回复会直接返回客户端 。

以下是如何正确执行来自客户端节点的同步服务调用的例子,类似于该节点中的async节点. [简单服务和客户端](../Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Service-And-Client.md) 教学。

``` python
import sys
from threading import Thread

from example_interfaces.srv import AddTwoInts
import rclpy
from rclpy.node import Node

class MinimalClientSync(Node):

    def __init__(self):
        super().__init__('minimal_client_sync')
        self.cli = self.create_client(AddTwoInts, 'add_two_ints')
        while not self.cli.wait_for_service(timeout_sec=1.0):
            self.get_logger().info('service not available, waiting again...')
        self.req = AddTwoInts.Request()

    def send_request(self):
        self.req.a = int(sys.argv[1])
        self.req.b = int(sys.argv[2])
        return self.cli.call(self.req)
        # This only works because rclpy.spin() is called in a separate thread below.
        # Another configuration, like spinning later in main() or calling this method from a timer callback, would result in a deadlock.

def main():
    rclpy.init()

    minimal_client = MinimalClientSync()

    spin_thread = Thread(target=rclpy.spin, args=(minimal_client,))
    spin_thread.start()

    response = minimal_client.send_request()
    minimal_client.get_logger().info(
        'Result of add_two_ints: for %d + %d = %d' %
        (minimal_client.req.a, minimal_client.req.b, response.sum))

    minimal_client.destroy_node()
    rclpy.shutdown()


if __name__ == '__main__':
    main()
```

内注 `main()` 客户打电话 `rclpy.spin` 在一个单独的线条中。 `send_request` 财务报告和财务报告 `rclpy.spin` 被阻断了,所以它们需要分开的线条。

<span id="sync-deadlock"></span>

## 1.1 同步陷入僵局

同步有几种方式 `call()` API会导致僵局.

如上例评论所述,未能创建单独的线程来旋转 `rclpy` 当一个客户端正在屏蔽一条等待响应的线条时, 但响应只能在同一线条上返回时, 客户端将永远不会停止等待, 其它的事情也不可能发生 。

造成僵局的另一个原因是阻碍 `rclpy.spin` 在订阅、计时器回调或服务回调中同步调用服务。例如,如果同步客户端 `send_request` 放在回调中 :

``` python
def trigger_request(msg):
    response = minimal_client.send_request()  # This will cause deadlock
    minimal_client.get_logger().info(
        'Result of add_two_ints: for %d + %d = %d' %
        (minimal_client.req.a, minimal_client.req.b, response.sum))
subscription = minimal_client.create_subscription(String, 'trigger', trigger_request, 10)

rclpy.spin(minimal_client)
```

死锁发生的原因是 `rclpy.spin` 将不预先取消 召回与 `send_request` 调用。一般情况下,调用只应进行轻快操作。

> **警告**
>
> 当陷入僵局时, 您不会收到任何服务被封杀的迹象 。 不会发出警告或例外, 也不会在堆栈跟踪中显示, 也不会失败 。

<span id="asynchronous-calls"></span>

## 2 同步呼叫

Async 调用 `rclpy` 它们是完全安全的,也是推荐的呼叫服务方法。它们可以在任何地方制造,而不会冒阻断其他ROS和非ROS进程的风险,与同步呼叫不同。

一个同步的客户端将立即返回 `future`,该值表示在向服务发送请求后,呼叫和响应是否完成(而不是响应本身的价值)。 `future` 可随时询问答复。

由于发送请求不会阻断任何东西,所以循环可以用于双旋 `rclpy` 检查 `future` 在同一线索中,例如:

``` python
while rclpy.ok():
    rclpy.spin_once(node)
    if future.done():
        #Get response
```

那个... [简单服务和客户端](../Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Service-And-Client.md) Python 的教程说明如何执行 Async 服务调用并检索 `future` 使用循环。

那个... `future` 也可以使用计时器或回调器检索,例如: [此示例](https://github.com/ros2/examples/blob/rolling/rclpy/services/minimal_client/examples_rclpy_minimal_client/client_async_callback.py),专用线程,或者用另一种方法。由您作为呼叫者决定如何存储 `future`,检查其状态,并获取您的回复。

<span id="summary"></span>

## 小结

不建议执行同步服务客户端。 它们容易陷入僵局, 但不会在僵局发生时显示任何问题 。 如果您必须使用同步调用, 请用节中的例子 。 [1次同步通话](#synchronous-calls) 这也是一种安全的方法。您还应当了解造成第1节概述的僵局的条件。 [1.1 同步陷入僵局](#sync-deadlock)我们建议使用Aync服务客户端。
