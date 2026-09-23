<span id="ensuring-security-across-machines"></span>
<span id="security-on-two"></span>
<span id="background"></span>
<span id="create-the-second-keystore"></span>
<span id="copy-files"></span>
<span id="launch-the-nodes"></span>

# 确保跨机器通信安全

**目标：** 让两台不同的机器安全通信。

**教程级别：** 高级

**耗时：** 5 分钟

## 背景

前面的教程在同一台机器上运行两个 ROS 节点，所有网络通信都通过本地主机接口进行。现在将这一场景扩展到多台机器，以便更直观地体现身份认证和加密的作用。

假设前一演示中创建密钥库的机器主机名为 `Alice`，我们希望再使用主机名为 `Bob` 的机器运行跨机器 `talker/listener` 演示。需要把部分密钥从 `Alice` 复制到 `Bob`，使 SROS 2 能够认证并加密传输。

## 创建第二个密钥库

首先在 `Bob` 上创建一个空密钥库；实际上只需创建一个空目录。

### Linux

```console
$ ssh Bob
$ mkdir ~/sros2_demo
$ exit
```

### macOS

```console
$ ssh Bob
$ mkdir ~/sros2_demo
$ exit
```

### Windows

```console
$ ssh Bob
$ md C:\dev\ros2\sros2_demo
$ exit
```

## 复制文件

接下来，把 `talker` 程序的密钥和证书从 `Alice` 复制到 `Bob`。密钥都是文本文件，可以使用 `scp` 复制。

### Linux

```console
$ cd ~/sros2_demo/demo_keystore
$ scp -r talker USERNAME@Bob:~/sros2_demo/demo_keystore
```

### macOS

```console
$ cd ~/sros2_demo/demo_keystore
$ scp -r talker USERNAME@Bob:~/sros2_demo/demo_keystore
```

### Windows

```console
$ cd C:\dev\ros2\sros2_demo\demo_keystore
$ scp -r talker USERNAME@Bob:/dev/ros2/sros2_demo/demo_keystore
```

!!! warning "警告"

    注意，此例在不同机器之间共享了整个密钥库，这可能不符合实际需求，并可能造成安全风险。有关说明，请参阅[部署指南](Deployment-Guidelines.md)。

文件都是很小的文本文件，因此复制很快即可完成。现在可以运行跨机器 talker/listener 演示了！

## 启动节点

设置好环境后，在 `Bob` 上运行 talker：

```console
$ ros2 run demo_nodes_cpp talker --ros-args --enclave /talker_listener/talker
```

在 `Alice` 上启动 listener：

```console
$ ros2 run demo_nodes_py listener --ros-args --enclave /talker_listener/listener
```

Alice 现在会接收来自 Bob 的加密消息。

两台机器成功通过加密和身份认证进行通信后，可以采用同样的步骤向 ROS 图中添加更多机器。
