<span id="writing-an-action-server-and-client-python"></span> <span id="actionspy"></span>

# 编写动作服务端和客户端（Python）

**目标：** 使用 Python 实现动作服务端和客户端。

**教程级别：** 中级

**预计耗时：** 15 分钟

<span id="background"></span>

## 背景

动作是 ROS 2 中的一种异步通信形式。动作客户端向服务端发送目标请求，服务端向客户端返回目标反馈和结果。

<span id="prerequisites"></span>

## 前提条件

需要使用[创建动作](../Creating-an-Action.md)教程中的 `action_tutorials_interfaces` 包及 `Fibonacci.action` 接口。

<span id="tasks"></span>

## 任务

<span id="writing-an-action-server"></span>

### 1 编写动作服务端

使用[创建动作](../Creating-an-Action.md)中定义的接口，编写计算斐波那契数列的服务端。

此前的教程通过创建包和 `ros2 run` 运行节点。本教程为简化流程，将服务端放在单个文件中。完整软件包示例见 [action_tutorials](https://github.com/ros2/demos/tree/rolling/action_tutorials)。

在主目录新建 `fibonacci_action_server.py`，填入[初始服务端代码](scripts/server_0.py)。

代码第 8 行定义 `Node` 的子类 `FibonacciActionServer`；第 11 行调用 `Node` 构造函数，将节点命名为 `fibonacci_action_server`；第 12–16 行创建动作服务端，需要四个参数：

1. 承载服务端的 ROS 2 节点 `self`。
2. 第 5 行导入的动作类型 `Fibonacci`。
3. 动作名称 `'fibonacci'`。
4. 执行已接受目标的回调 `self.execute_callback`。此回调**必须**返回该动作类型的结果消息。

第 18–21 行定义 `execute_callback`，目标被接受后调用它执行任务。

运行服务端。

Linux：

```console
$ python3 fibonacci_action_server.py
```

macOS：

```console
$ python3 fibonacci_action_server.py
```

Windows：

```console
$ python fibonacci_action_server.py
```

在另一终端通过命令行发送目标：

```console
$ ros2 action send_goal fibonacci action_tutorials_interfaces/action/Fibonacci "{order: 5}"
```

服务端终端应显示 `Executing goal...`，随后警告目标状态尚未设置。如果执行回调没有设置目标句柄状态，默认将其视为 *aborted*（中止）。

可通过目标句柄的 [succeed()](http://docs.ros2.org/latest/api/rclpy/api/actions.html#rclpy.action.server.ServerGoalHandle.succeed) 标记成功，修改方式见 [server_1.py 第 18–22 行](scripts/server_1.py)。重启服务端并再次发送目标，应看到 `SUCCEEDED` 状态。

接着让执行回调实际计算并返回请求的斐波那契数列，见 [server_2.py 第 18–30 行](scripts/server_2.py)。计算后，将数列赋给结果消息字段再返回。重启服务端并发送目标，应得到正确结果。

<span id="publishing-feedback"></span>

#### 1.2 发布反馈

动作的一个优点是可在执行期间向客户端提供反馈。通过目标句柄的 [publish_feedback()](http://docs.ros2.org/latest/api/rclpy/api/actions.html#rclpy.action.server.ServerGoalHandle.publish_feedback) 方法即可发布。

用反馈消息替代 `sequence` 变量保存数列，在 for 循环每次更新后发布反馈，并暂停一会儿以便观察效果。完整代码见 [server_3.py](scripts/server_3.py)，主要变化位于第 1、23、24、27–31、36 行。

重启服务端，使用 `--feedback` 验证反馈发布：

```console
$ ros2 action send_goal --feedback fibonacci action_tutorials_interfaces/action/Fibonacci "{order: 5}"
```

<span id="writing-an-action-client"></span>

### 2 编写动作客户端

客户端也放在单个文件中。新建 `fibonacci_action_client.py`，填入[初始客户端代码](scripts/client_0.py)。

代码定义 `Node` 的子类 `FibonacciActionClient`，第 11 行调用 `Node` 构造函数，节点名为 `fibonacci_action_client`。第 12 行使用[上一篇教程](../Creating-an-Action.md)定义的接口创建 `ActionClient`，传入三个参数：

1. 承载客户端的 ROS 2 节点 `self`。
2. 动作类型 `Fibonacci`。
3. 动作名称 `'fibonacci'`。

客户端可以与动作名称和类型相同的服务端通信。

第 14–20 行定义 `send_goal`，等待服务端可用后发送目标，返回一个之后可以等待的 future。

类定义之后的 `main()` 初始化 ROS 2、创建客户端节点、发送目标并等待，然后由 Python 程序入口调用 `main()`。

先运行此前编写的服务端进行测试。

Linux：

```console
$ python3 fibonacci_action_server.py
```

macOS：

```console
$ python3 fibonacci_action_server.py
```

Windows：

```console
$ python fibonacci_action_server.py
```

在另一终端启动客户端。

Linux：

```console
$ python3 fibonacci_action_client.py
```

macOS：

```console
$ python3 fibonacci_action_client.py
```

Windows：

```console
$ python fibonacci_action_client.py
```

服务端执行目标时应显示：

```console
[INFO] [fibonacci_action_server]: Executing goal...
[INFO] [fibonacci_action_server]: Feedback: array('i', [0, 1, 1])
[INFO] [fibonacci_action_server]: Feedback: array('i', [0, 1, 1, 2])
[INFO] [fibonacci_action_server]: Feedback: array('i', [0, 1, 1, 2, 3])
[INFO] [fibonacci_action_server]: Feedback: array('i', [0, 1, 1, 2, 3, 5])
~ etc.
```

客户端会启动并很快结束。现在客户端虽然可用，但还看不到结果和反馈。

<span id="getting-a-result"></span>

#### 2.1 获取结果

要知道目标何时完成，先获取已发送目标的句柄，再用句柄请求结果。完整示例见 [client_1.py](scripts/client_1.py)。

[ActionClient.send_goal_async()](http://docs.ros2.org/latest/api/rclpy/api/actions.html#rclpy.action.client.ActionClient.send_goal_async) 返回结果为目标句柄的 future。第 22 行为 future 完成注册回调。

注意，此 future 在服务端接受或拒绝目标请求时完成。`goal_response_callback` 第 24–30 行检查目标是否被拒绝；若拒绝，则不会有结果，可直接返回。

取得目标句柄后，通过 [get_result_async()](http://docs.ros2.org/latest/api/rclpy/api/actions.html#rclpy.action.client.ClientGoalHandle.get_result_async) 请求结果，同样返回 future，并在结果就绪时完成。第 32–33 行为它注册回调，第 35–38 行在回调中打印结果数列并关闭 ROS 2，以便正常退出。

保持服务端在另一终端运行，启动更新后的客户端。

Linux：

```console
$ python3 fibonacci_action_client.py
```

macOS：

```console
$ python3 fibonacci_action_client.py
```

Windows：

```console
$ python fibonacci_action_client.py
```

应看到目标已接受及最终结果的日志。

<span id="getting-feedback"></span>

#### 2.2 获取反馈

客户端现在能够发送目标，还可以进一步接收服务端的执行反馈。完整示例见 [client_2.py](scripts/client_2.py)。

第 40–42 行定义反馈回调，从消息中取出反馈部分，并打印 `partial_sequence` 字段。

还需向客户端注册该回调：在第 20 行发送目标时，将回调一并传入。

再次运行客户端，就应能看到反馈输出。

<span id="summary"></span>

## 小结

本教程逐步编写了 Python 动作服务端和客户端，并配置它们交换目标、反馈和结果。

<span id="related-content"></span>

## 相关内容

- Python 动作服务端和客户端有多种写法，可参阅 [ros2/examples](https://github.com/ros2/examples/tree/rolling/rclpy/actions) 中的 `minimal_action_server` 和 `minimal_action_client`。
- 动作的更多细节见[设计文章](http://design.ros2.org/articles/actions.html)。
