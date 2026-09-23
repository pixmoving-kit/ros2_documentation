---
translation_status: machine_translated
source: Tutorials/Demos/Quality-of-Service.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-quality-of-service-settings-for-lossy-networks"></span>

# 在丢包网络中配置服务质量

<span id="background"></span>

## 背景

请阅读文档页 [关于 QoS 设置](../../Concepts/Intermediate/About-Quality-of-Service-Settings.md) 用于提供ROS 2中现有支持的背景资料。

在这个演示中, 我们将生成一个节点, 发布一个相机图像, 和一个订阅图像并在屏幕上显示的节点。 然后, 我们将模拟它们之间的一个丢失的网络连接, 并显示不同的服务设置如何处理不良的链接 。

<span id="prerequisites"></span>

## 前提条件

这个教程假设你有一个 [正在运行的 ROS 2 安装](../../Installation.md) 和 OpenCV。参见 [OpenCV 文档](http://docs.opencv.org/doc/tutorials/introduction/table_of_content_introduction/table_of_content_introduction.html#table-of-content-introduction) 您还需要ROS 软件包 `image_tools`.

##### Linux 二进制

``` console
$ sudo apt-get install ros-rolling-image-tools
```

##### 从源

克隆并使用与您的安装匹配的分支构建演示重播.

``` console
$ git clone https://github.com/ros2/demos.git -b rolling
```

<span id="run-the-demo"></span>

## 运行演示

在运行演示之前,请确保您有一个工作网络摄像头与您的计算机连接.

一旦安装了 ROS 2, 源代码您的设置文件 :

##### Linux

``` console
$ . <path to ROS 2 install space>/setup.bash
```

##### macOS

``` console
$ . <path to ROS 2 install space>/setup.bash
```

##### Windows

``` console
$ call <path to ROS 2 install space>/local_setup.bat
```

然后运行 :

``` console
$ ros2 run image_tools showimage
```

还没发生什么 `showimage` 是一个用户节点, 正在等待该网站上的出版商 。 `image` 主题。

注意: 你必须关闭 `showimage` 进程与 `Ctrl-C` 等会,你不能把窗户关上。

在单独的终端中, 源代码为安装文件并运行发布器节点 :

``` console
$ ros2 run image_tools cam2image
Publishing image #1
Publishing image #2
Publishing image #3
```

这将从您的网络摄像头中发布一个图像。 如果您的电脑上没有相机, 将会有一个命令行选项来发布预定义的图像 。

一个窗口将出现标题“ 视图 ” , 显示您的相机种子。 在第一个窗口中, 您可以看到用户的输出 :

``` console
Received image #1
Received image #2
Received image #3
...
```

> **说明**
>
> macOS 用户: 如果这些示例无效, 或者您收到错误, 如 `ddsi_conn_write failed -1` 然后您需要增加您的系统宽度 UDP 包大小 :
>
> ``` console
> $ sudo sysctl -w net.inet.udp.recvspace=209715
> $ sudo sysctl -w net.inet.udp.maxdgram=65500
> ```
>
> 这些更改不会持续重新启动。 如果您想要这些更改持续下去, 请将这些行添加到 `/etc/sysctl.conf` (如果文件不存在, 则创建该文件 ) :
>
> ``` bash
> net.inet.udp.recvspace=209715
> net.inet.udp.maxdgram=65500
> ```

<span id="command-line-options"></span>

### 命令行选项

在您的终端中, 在原命令中添加- h 旗 :

``` console
$ ros2 run image_tools showimage -h
```

<span id="add-network-traffic"></span>

### 添加网络流量

> **警告**
>
> 演示的这个部分不会在 RTI 的 Connext DDS 上工作。 当在同一主机运行多个节点时, RTI Connext DDS 执行会使用共享内存和回路接口。 降低回路接口的吞吐量不会影响共享内存, 因此两个节点之间的流量不会受到影响 。

> **说明**
>
> 此下一节为 Linux 特制.
>
> 然而,对于 macOS 和 Windows 来说,您可以实现类似效果的“ 网络链接条件化器”( Xcode 工具套件的一部分) 和 [“笨拙”](http://jagt.github.io/clumsy/index.html),但不会包含在本教程中。

我们要使用Linux网络交通管制系统 `tc` ([男性页面](http://linux.die.net/man/8/tc)) .

``` console
$ sudo tc qdisc add dev lo root netem loss 5%
```

这种神奇的咒语会模拟本地回路设备上5%的包丢失。 如果您使用更高的图像分辨率( 例如 ) 。 `--ros-args -p width:=640 -p height:=480`)您可能想要尝试一个较低的包丢失率(例如. `1%`).

接下来我们开始 `cam2image` 财务报告和财务报告 `showimage`,我们很快会注意到,这两个程序似乎都减缓了图像传输的速度。这是默认的QoS设置行为造成的。在丢失的频道上强制可靠性意味着出版商(在这种情况下, `cam2image`)将重新发送网络包,直到其收到消费者的确认(即. `showimage`).

现在让我们尝试运行两个程序,但设置更合适。 首先,我们将使用 `-p reliability:=best_effort` 选项来进行最佳努力的交流。现在,出版商将试图提供网络数据包,而不要指望消费者的承认。我们现在看到,一些框架在网络上。 `showimage` 侧面被丢掉,所以外壳中的框架数字正在运行 `showimage` 将不再连续 :

[![最佳工作图像传输](https://raw.githubusercontent.com/ros2/demos/rolling/image_tools/doc/qos-best-effort.png)](https://raw.githubusercontent.com/ros2/demos/rolling/image_tools/doc/qos-best-effort.png)

当您完成排队时, 请记住删除排队纪律 :

``` console
$ sudo tc qdisc delete dev lo root netem loss 5%
```
