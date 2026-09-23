<span id="examining-network-traffic"></span>
<span id="examine-traffic"></span>
<span id="overview"></span>
<span id="prerequisites"></span>
<span id="run-the-demo"></span>
<span id="install-tcpdump"></span>
<span id="start-the-talker-and-listener"></span>
<span id="display-unencrypted-discovery-packets"></span>
<span id="display-unencrypted-data-packets"></span>
<span id="enable-encryption"></span>
<span id="display-encrypted-discovery-packets"></span>
<span id="display-encrypted-data-packets"></span>

# 检查网络流量

**目标：** 捕获并检查原始 ROS 2 网络流量。

**教程级别：** 高级

**耗时：** 20 分钟

## 概述

ROS 2 通信安全旨在保护节点之间的通信。前面的教程启用了安全功能，但如何**真正**判断流量是否已加密？本教程通过捕获实时网络流量，展示加密流量与未加密流量之间的区别。

!!! note "说明"

    当通信端点位于同一主机时，`rmw_fastrtps_cpp` 默认使用[共享内存传输](https://fast-dds.docs.eprosima.com/en/latest/fastdds/transport/shared_memory/shared_memory.html)提高传输层性能。安全隔离域仍然生效，数据也会加密，但数据不经过网络接口，因此无法捕获实时网络流量。使用 `rmw_fastrtps_cpp` 时，请在不同主机上运行发布者和订阅者来完成本教程，或者按照[启用 UDP 传输](https://fast-dds.docs.eprosima.com/en/latest/fastdds/transport/udp/udp.html#enabling-udp-transport)和[设置 Fast DDS XML 配置](https://github.com/ros2/rmw_fastrtps#full-qos-configuration)的说明禁用共享内存传输。

## 前提条件

本指南仅适用于 Linux，并假设你已[安装 ROS 2](../../../Installation.md)。

## 运行演示

### 安装 `tcpdump`

在新终端中安装 [tcpdump](https://www.tcpdump.org/manpages/tcpdump.1.html)，这是用于捕获和显示网络流量的命令行工具。本教程使用 `tcpdump` 命令，你也可以使用功能类似的图形化流量捕获和分析工具 [Wireshark](https://www.wireshark.org/)。

```console
$ sudo apt update
$ sudo apt install tcpdump
```

通过多个 `ssh` 会话，在同一台机器上运行下面的命令。

### 启动 talker 和 listener

再次分别在两个终端中启动 talker 和 listener。未设置安全环境变量，因此这些会话没有启用安全功能。在一个终端运行：

```console
$ unset ROS_SECURITY_ENABLE
$ ros2 run demo_nodes_cpp talker --ros-args --enclave /talker_listener/talker
```

在另一个终端运行：

```console
$ unset ROS_SECURITY_ENABLE
$ ros2 run demo_nodes_cpp listener --ros-args --enclave /talker_listener/listener
```

### 显示未加密的发现数据包

保持 talker 和 listener 运行，打开另一个终端并启动 `tcpdump` 查看网络流量。读取原始网络流量需要特权，因此必须使用 `sudo`。

下面的命令通过 `-X` 输出数据包内容，通过 `-i` 监听所有接口，并且只捕获 [UDP](https://en.wikipedia.org/wiki/User_Datagram_Protocol) 7400 端口的流量：

```console
$ sudo tcpdump -X -i any udp port 7400
20:18:04.400770 IP 8_xterm.46392 > 239.255.0.1.7400: UDP, length 252
  0x0000:  4500 0118 d48b 4000 0111 7399 c0a8 8007  E.....@...s.....
  0x0010:  efff 0001 b538 1ce8 0104 31c6 5254 5053  .....8....1.RTPS
  ...
  0x00c0:  5800 0400 3f0c 3f0c 6200 1c00 1800 0000  X...?.?.b.......
  0x00d0:  2f74 616c 6b65 725f 6c69 7374 656e 6572  /talker_listener
  0x00e0:  2f74 616c 6b65 7200 2c00 2800 2100 0000  /talker.,.(.!...
  0x00f0:  656e 636c 6176 653d 2f74 616c 6b65 725f  enclave=/talker_
  0x0100:  6c69 7374 656e 6572 2f74 616c 6b65 723b  listener/talker;
  0x0110:  0000 0000 0100 0000                      ........
```

这是一个发现数据报，表示 talker 正在寻找订阅者。可以看到，节点名 `/talker_listener/talker` 和隔离域名（也是 `/talker_listener/talker`）以明文传输。你还应能看到来自 `listener` 节点的类似发现数据报。

典型发现数据包还有以下特点：

- 目标地址为组播 IP 地址 239.255.0.1；ROS 2 默认使用组播进行发现。
- 根据 [DDS-RTPS 规范](https://www.omg.org/spec/DDSI-RTPS/About-DDSI-RTPS/)，目标端口为 UDP 7400。
- 数据包包含 DDS-RTPS 规范定义的 `RTPS` 标记。

### 显示未加密的数据包

使用 `tcpdump` 过滤高于 7400 的 UDP 端口，捕获非发现类 RTPS 数据包。你会看到几种不同的数据包，请留意类似下面的包，其中显然包含 talker 发给 listener 的数据：

```console
$ sudo tcpdump -i any -X udp portrange 7401-7500
20:49:17.927303 IP localhost.46392 > localhost.7415: UDP, length 84
  0x0000:  4500 0070 5b53 4000 4011 e127 7f00 0001  E..p[S@.@..'....
  0x0010:  7f00 0001 b538 1cf7 005c fe6f 5254 5053  .....8...\.oRTPS
  0x0020:  0203 010f 010f 4874 e752 0000 0100 0000  ......Ht.R......
  0x0030:  0901 0800 cdee b760 5bf3 5aed 1505 3000  .......`[.Z...0.
  0x0040:  0000 1000 0000 1204 0000 1203 0000 0000  ................
  0x0050:  5708 0000 0001 0000 1200 0000 4865 6c6c  W...........Hell
  0x0060:  6f20 576f 726c 643a 2032 3133 3500 0000  o.World:.2135...
```

注意该数据包的以下特点：

- 消息内容 `Hello World: 2135` 以明文发送。
- 源和目标 IP 地址均为 `localhost`。两个节点运行在同一台机器上，通过 `localhost` 接口发现了彼此。

### 启用加密

停止 talker 和 listener 节点。为两者设置安全环境变量以启用加密，然后重新运行。

在终端 1 中：

```console
$ export ROS_SECURITY_KEYSTORE=~/sros2_demo/demo_keystore
$ export ROS_SECURITY_ENABLE=true
$ export ROS_SECURITY_STRATEGY=Enforce
$ ros2 run demo_nodes_cpp talker --ros-args --enclave /talker_listener/talker
```

在终端 2 中：

```console
$ export ROS_SECURITY_KEYSTORE=~/sros2_demo/demo_keystore
$ export ROS_SECURITY_ENABLE=true
$ export ROS_SECURITY_STRATEGY=Enforce
$ ros2 run demo_nodes_cpp listener --ros-args --enclave /talker_listener/listener
```

### 显示启用加密后的发现数据包

再次运行之前的 `tcpdump` 命令，检查启用加密后的发现流量。典型发现数据包类似下面这样：

```console
$ sudo tcpdump -X -i any udp port 7400
21:09:07.336617 IP 8_xterm.60409 > 239.255.0.1.7400: UDP, length 596
  0x0000:  4500 0270 c2f6 4000 0111 83d6 c0a8 8007  E..p..@.........
  0x0010:  efff 0001 ebf9 1ce8 025c 331e 5254 5053  .........\3.RTPS
  0x0020:  0203 010f bbdd 199c 7522 b6cb 699f 74ae  ........u"..i.t.
  ...
  0x00c0:  5800 0400 3f0c ff0f 6200 2000 1a00 0000  X...?...b.......
  0x00d0:  2f74 616c 6b65 725f 6c69 7374 656e 6572  /talker_listener
  0x00e0:  2f6c 6973 7465 6e65 7200 0000 2c00 2800  /listener...,.(.
  0x00f0:  2300 0000 656e 636c 6176 653d 2f74 616c  #...enclave=/tal
  0x0100:  6b65 725f 6c69 7374 656e 6572 2f6c 6973  ker_listener/lis
  0x0110:  7465 6e65 723b 0000 0110 c400 1400 0000  tener;..........
  0x0120:  4444 533a 4175 7468 3a50 4b49 2d44 483a  DDS:Auth:PKI-DH:
  0x0130:  312e 3000 0400 0000 0c00 0000 6464 732e  1.0.........dds.
  ...
  0x0230:  1100 0000 6464 732e 7065 726d 5f63 612e  ....dds.perm_ca.
  0x0240:  616c 676f 0000 0000 0d00 0000 4543 4453  algo........ECDS
  0x0250:  412d 5348 4132 3536 0000 0000 0000 0000  A-SHA256........
  0x0260:  0510 0800 0700 0080 0600 0080 0100 0000  ................
```

数据包明显变大，包含了用于在 ROS 节点间建立加密通信的信息。正如接下来会看到的，其中实际上包含启用安全功能时创建的一些安全配置文件。想了解更多，可阅读论文 [Network Reconnaissance and Vulnerability Excavation of Secure DDS Systems](https://arxiv.org/abs/1908.05310)，了解这一点为何重要。

### 显示加密的数据包

现在使用 `tcpdump` 捕获数据包。典型的数据包如下：

```console
$ sudo tcpdump -i any -X udp portrange 7401-7500
21:18:14.531102 IP localhost.54869 > localhost.7415: UDP, length 328
  0x0000:  4500 0164 bb42 4000 4011 8044 7f00 0001  E..d.B@.@..D....
  0x0010:  7f00 0001 d655 1cf7 0150 ff63 5254 5053  .....U...P.cRTPS
  0x0020:  0203 010f daf7 10ce d977 449b bb33 f04a  .........wD..3.J
  0x0030:  3301 1400 0000 0003 492a 6066 8603 cdb5  3.......I*`f....
  0x0040:  9df6 5da6 8402 2136 0c01 1400 0000 0000  ..]...!6........
  0x0050:  0203 010f daf7 10ce d977 449b bb33 f04a  .........wD..3.J
  ...
  0x0130:  7905 d390 3201 1400 3ae5 0b60 3906 967e  y...2...:..`9..~
  0x0140:  5b17 fd42 de95 54b9 0000 0000 3401 1400  [..B..T.....4...
  0x0150:  42ae f04d 0559 84c5 7116 1c51 91ba 3799  B..M.Y..q..Q..7.
  0x0160:  0000 0000                                ....
```

该 RTPS 数据包中的数据已全部加密。

除此之外，你还会看到包含节点名和隔离域名的其他数据包，它们用于支持参数、服务等 ROS 功能。这些数据包的加密选项同样可以通过安全策略控制。
