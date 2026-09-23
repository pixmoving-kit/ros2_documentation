---
translation_status: machine_translated
source: Tutorials/Demos/Wait-for-Acknowledgment.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="wait-for-acknowledgment"></span>

# 等待确认

**目标：** 等待一个出版商发来的消息的确认.

**教程级别：** 高级

**用时：** 10分钟

<span id="overview"></span>

## 概述

在出版商-订阅商架构中,信息从出版商发送到订阅者,而出版商没有任何内置机制来确认订阅者收到了信息。这个功能可以让出版商等待其发送的信息被认可。在出版商需要确保订阅者收到信息后再采取进一步行动,如发送更多信息或进行其他操作的情况下,这样做是有用的。

<span id="rmw-support"></span>

## RAMW 支持

等待承认需要《保护所有移徙工人及其家庭成员权利国际公约》的实施支持。

<span id="id1"></span>

|                |        |
|----------------|--------|
| rmw_fastrtps   | 已支持 |
| rmw_connextdds | 已支持 |
| rmw_cyclonedds | 已支持 |

等待确认支持状态 {.docutils .align-default}

出版社 [QoS可靠性政策](../../Concepts/Intermediate/About-Quality-of-Service-Settings.md#about-qos-policies) 有必要 `RELIABLE` 否则出版商不会等待承认。

<span id="installing-the-demo"></span>

## 安装演示

见 [安装指令](../../Installation.md) 关于安装ROS 2的详情。

如果您已经从软件包中安装了 ROS 2, 请确保您已经安装过 `ros-rolling-examples-rclcpp-minimal-publisher` 财务报告和财务报告 `ros-rolling-examples-rclcpp-minimal-subscriber` 已安装。如果从源头下载归档或建立ROS 2,它就已经是安装的一部分。

<span id="running-the-demo"></span>

## 运行演示

此演示演示如何在出版商中使用等待承认功能,以确保所有订阅商都认可出版商发送的信息.

<https://github.com/ros2/examples/blob/rolling/rclcpp/topics/minimal_publisher/member_function_with_wait_for_all_acked.cpp>

出版商可以使用 `wait_for_all_acked` 用于在指定超时内等待消息确认的方法,然后由信号关闭。

我们可以开始演示 通过运行 `publisher_wait_for_all_acked` 财务报告和财务报告 `subscriber_member_function` 执行文件 `examples_rclcpp_minimal_publisher` 软件包( 不要忘记先从设置文件源出) :

在一个终端中启动订阅者 :

``` console
$ ros2 run examples_rclcpp_minimal_subscriber subscriber_member_function
[INFO] [1743121567.030751270] [minimal_subscriber]: I heard: 'Hello, world! 0'
[INFO] [1743121567.530981660] [minimal_subscriber]: I heard: 'Hello, world! 1'
[INFO] [1743121568.031032935] [minimal_subscriber]: I heard: 'Hello, world! 2'
[INFO] [1743121568.531048458] [minimal_subscriber]: I heard: 'Hello, world! 3'
[INFO] [1743121569.031049351] [minimal_subscriber]: I heard: 'Hello, world! 4'
[INFO] [1743121569.530980327] [minimal_subscriber]: I heard: 'Hello, world! 5'
[INFO] [1743121570.030825871] [minimal_subscriber]: I heard: 'Hello, world! 6'
...
```

然后在另一个终端开始出版商:

``` console
$ ros2 run examples_rclcpp_minimal_publisher publisher_wait_for_all_acked
[INFO] [1743121567.030353553] [minimal_publisher_with_wait_for_all_acked]: Publishing: 'Hello, world! 0'
[INFO] [1743121567.530420788] [minimal_publisher_with_wait_for_all_acked]: Publishing: 'Hello, world! 1'
[INFO] [1743121568.030461599] [minimal_publisher_with_wait_for_all_acked]: Publishing: 'Hello, world! 2'
[INFO] [1743121568.530435646] [minimal_publisher_with_wait_for_all_acked]: Publishing: 'Hello, world! 3'
[INFO] [1743121569.030431263] [minimal_publisher_with_wait_for_all_acked]: Publishing: 'Hello, world! 4'
[INFO] [1743121569.530447106] [minimal_publisher_with_wait_for_all_acked]: Publishing: 'Hello, world! 5'
[INFO] [1743121570.030353934] [minimal_publisher_with_wait_for_all_acked]: Publishing: 'Hello, world! 6'
^C[INFO] [1743121570.344981639] [rclcpp]: signal_handler(signum=2)
[INFO] [1743121570.345398788] [minimal_publisher_with_wait_for_all_acked]: All subscribers acknowledge messages
```

当出版商终止时(例如,按 <span class="kbd kbd docutils literal notranslate">编译</span>-<span class="kbd kbd docutils literal notranslate">C</span>),它会等待所有信件在关闭前的确认。如果所有订阅者都承认信件,出版商会打印一条信件,表明所有订阅者都承认信件。如果不是,它会打印一条信件,表明并非所有订阅者都在指定的超时时间内承认信件。

<span id="related-content"></span>

## 相关内容

- [使用 rclpy 等待确认实例](https://github.com/ros2/examples/blob/rolling/rclpy/topics/minimal_publisher/examples_rclpy_minimal_publisher/publisher_member_function_with_wait_for_all_acked.py).
