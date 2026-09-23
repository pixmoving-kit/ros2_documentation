<span id="synchronous-vs-asynchronous-service-clients"></span>
<span id="syncasync"></span>
# 同步与异步服务客户端

**难度：** 中级

**用时：** 10 分钟

<span id="introduction"></span>
## 简介

本指南说明 Python 同步服务客户端 `call()` API 的使用风险。同步调用服务很容易意外造成死锁，因此不建议使用 `call()`。

对于了解这些陷阱、仍希望使用同步调用的有经验用户，本指南给出了正确使用 `call()` 的示例，同时说明可能导致死锁的情况。

由于建议避免同步调用，本文也介绍推荐的替代方案——异步调用（`call_async()`）——的特性和用法。

C++ 服务调用 API 仅提供异步形式，因此本文的比较和示例针对 Python 服务与客户端。这里对异步的定义大体适用于 C++，但存在一些例外。

<span id="synchronous-calls"></span>
## 1 同步调用

同步客户端向服务发送请求后，会阻塞调用线程，直至收到响应。在调用期间，该线程无法执行其他操作。调用完成所需时间没有固定上限；完成后，响应会直接返回给客户端。

以下示例展示如何在客户端节点中正确执行同步服务调用，与[简单服务和客户端](../Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Service-And-Client.md)教程中的异步节点类似。

```python
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

注意 `main()` 中的客户端在独立线程里调用 `rclpy.spin`。`send_request` 和 `rclpy.spin` 都会阻塞，因此必须分别放在不同线程中。

<span id="sync-deadlock"></span>
## 1.1 同步调用死锁

同步 `call()` API 可在多种情况下造成死锁。

如上例注释所述，没有在独立线程中运行 `rclpy` 的 spin 就是一个原因。当客户端阻塞线程等待响应，而响应又只能在同一线程上返回时，客户端将永远等待，其他操作也无法进行。

另一个原因是在订阅回调、定时器回调或服务回调中同步调用服务，阻塞了 `rclpy.spin`。例如，将同步客户端的 `send_request` 放进回调：

```python
def trigger_request(msg):
    response = minimal_client.send_request()  # This will cause deadlock
    minimal_client.get_logger().info(
        'Result of add_two_ints: for %d + %d = %d' %
        (minimal_client.req.a, minimal_client.req.b, response.sum))
subscription = minimal_client.create_subscription(String, 'trigger', trigger_request, 10)

rclpy.spin(minimal_client)
```

此时会死锁，因为 `rclpy.spin` 不会抢占正在执行 `send_request` 的回调。通常，回调应只执行轻量且快速的操作。

!!! warning "警告"
    死锁发生时，不会有任何信息提示服务已被阻塞：没有警告，不抛出异常，堆栈跟踪中没有提示，调用也不会以失败返回。

<span id="asynchronous-calls"></span>
## 2 异步调用

`rclpy` 中的异步调用是推荐的安全服务调用方式。与同步调用不同，可以在任何位置发起异步调用，不会因此阻塞其他 ROS 或非 ROS 处理过程。

异步客户端向服务发送请求后会立即返回 `future`。它表示调用和响应是否已经完成，并不是响应值本身。可以随时通过返回的 `future` 查询响应。

由于发送请求不会阻塞，可以在同一个线程的循环中同时执行 `rclpy` 的 spin 并检查 `future`，例如：

```python
while rclpy.ok():
    rclpy.spin_once(node)
    if future.done():
        #Get response
```

Python [简单服务和客户端](../Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Service-And-Client.md)教程展示了如何执行异步服务调用，并在循环中获取 `future` 的结果。

也可以通过定时器或回调（见[此示例](https://github.com/ros2/examples/blob/rolling/rclpy/services/minimal_client/examples_rclpy_minimal_client/client_async_callback.py)）、专用线程或其他方式获取 `future` 的结果。作为调用者，你可以自行决定如何保存 `future`、检查其状态并获取响应。

<span id="summary"></span>
## 总结

不建议实现同步服务客户端，因为它容易发生死锁，而且死锁时不会给出任何问题提示。如果必须使用同步调用，[1 同步调用](#synchronous-calls)中的示例是一种安全用法；同时应了解 [1.1 同步调用死锁](#sync-deadlock)列出的触发条件。建议使用异步服务客户端。
