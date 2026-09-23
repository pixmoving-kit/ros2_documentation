---
translation_status: machine_translated
source: Tutorials/Advanced/Security/Security-on-Two.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="ensuring-security-across-machines"></span> <span id="security-on-two"></span>

# 确保跨计算机通信安全

**目标：** 使两台不同的机器安全地交流.

**教程级别：** 高级

**用时：** 5分钟

<span id="background"></span>

## 背景

之前的教程已经在同一台机器上使用了两个ROS节点,通过本地主机接口发送所有网络通信。 让我们把这种情景扩大到多台机器,因为认证和加密的好处会变得更加明显。

假设在前一个演示中创建的键盘的机器有一个主机名 `Alice`,而且我们也想要使用另一个有主机名的机器 `Bob` 我们的多机 `talker/listener` 演示。我们需要移动一些密钥从 `Alice` 改为: `Bob` 允许 SROS 2 认证和加密传输。

<span id="create-the-second-keystore"></span>

## 创建第二个密钥托

开始于创建空密钥托 `Bob`;键盘实际上只是一个空目录 :

##### Linux

``` console
$ ssh Bob
$ mkdir ~/sros2_demo
$ exit
```

##### 麦克OS

``` console
$ ssh Bob
$ mkdir ~/sros2_demo
$ exit
```

##### Windows

``` console
$ ssh Bob
$ md C:\dev\ros2\sros2_demo
$ exit
```

<span id="copy-files"></span>

## 复制文件

下次复制密钥和证书 `talker` 程序从 `Alice` 改为: `Bob`。由于密钥只是文本文件,我们可以使用 `scp` 以复制它们。

##### Linux

``` console
$ cd ~/sros2_demo/demo_keystore
$ scp -r talker USERNAME@Bob:~/sros2_demo/demo_keystore
```

##### 麦克OS

``` console
$ cd ~/sros2_demo/demo_keystore
$ scp -r talker USERNAME@Bob:~/sros2_demo/demo_keystore
```

##### Windows

``` console
$ cd C:\dev\ros2\sros2_demo\demo_keystore
$ scp -r talker USERNAME@Bob:/dev/ros2/sros2_demo/demo_keystore
```

> **警告**
>
> 请注意,在这种情况下,整个键盘由不同机器共享,可能不是理想的行为,因为这可能造成安全风险。 [部署指南](Deployment-Guidelines.md) 请提供这方面的更多信息。

这将是非常快的,因为它只是复制一些非常小的文本文件。 现在,我们准备运行一个多机器的谈话者/听众演示。

<span id="launch-the-nodes"></span>

## 启动节点

环境一旦建立, 运行谈话者 `Bob`:

``` console
$ ros2 run demo_nodes_cpp talker --ros-args --enclave /talker_listener/talker
```

并启动听者 `Alice`:

``` console
$ ros2 run demo_nodes_py listener --ros-args --enclave /talker_listener/listener
```

爱丽丝现在会收到鲍勃的加密信息

由于两台机器同时使用加密和认证手段成功进行通信,可以使用同样的程序在你的ROS图中添加更多的机器.
