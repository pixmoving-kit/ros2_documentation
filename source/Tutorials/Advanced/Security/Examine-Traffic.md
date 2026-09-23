---
translation_status: machine_translated
source: Tutorials/Advanced/Security/Examine-Traffic.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="examining-network-traffic"></span> <span id="examine-traffic"></span>

# 检查网络流量

**目标：** 捕捉和检查原始ROS 2网络流量。

**教程级别：** 高级

**用时：** 20分钟

<span id="overview"></span>

## 概述

ROS 2 通信安全, 都是为了保护节点之间的通信。 先前的教程启用了安全, 但您如何 ? **痷** 如果流量被加密的话, 我们将会在此教程中查看如何捕捉网络直播流量, 以显示加密流量和未加密流量的区别 。

> **说明**
>
> `rmw_fastrtps_cpp` 用途 [共享内存传输](https://fast-dds.docs.eprosima.com/en/latest/fastdds/transport/shared_memory/shared_memory.html) 当端点在同一主机系统中时,默认会改善传输层的性能。安全飞地仍然被应用,数据将被加密。但是,由于数据不会在网络界面上,因此无法捕获直播网络流量。如果您正在使用 `rmw_fastrtps_cpp`,您需要通过此教程,在发布者和订阅者之间使用不同的主机系统,或者禁用共享内存传输。 [启用 UDP 运输](https://fast-dds.docs.eprosima.com/en/latest/fastdds/transport/udp/udp.html#enabling-udp-transport) 财务报告和财务报告 [如何设置快速 DDS XML 配置](https://github.com/ros2/rmw_fastrtps#full-qos-configuration).

<span id="prerequisites"></span>

## 前提条件

此指南仅运行在 Linux 上, 并假设您已经 [已安装 ROS 2](../../../Installation.md).

<span id="run-the-demo"></span>

## 运行演示

<span id="install-tcpdump"></span>

### 安装 `tcpdump`

通过安装在新终端窗口中开始 [tcpdump 调试器](https://www.tcpdump.org/manpages/tcpdump.1.html),用于捕捉和显示网络流量的命令行工具。尽管此教程描述 `tcpdump` 命令,您也可以使用 [线莎克](https://www.wireshark.org/),是用于捕捉和分析流量的类似图形工具。

``` console
$ sudo apt update
$ sudo apt install tcpdump
```

通过多个程序在单机上运行以下命令 `ssh` 届会。

<span id="start-the-talker-and-listener"></span>

### 开口听

重新启动谈话者和听众, 各自在自己的终端。 安全环境变量没有设置, 因此无法为这些会话设定安全性 。 在一次终端运行中 :

``` console
$ unset ROS_SECURITY_ENABLE
$ ros2 run demo_nodes_cpp talker --ros-args --enclave /talker_listener/talker
```

在另一个终端运行中 :

``` console
$ unset ROS_SECURITY_ENABLE
$ ros2 run demo_nodes_cpp listener --ros-args --enclave /talker_listener/listener
```

<span id="display-unencrypted-discovery-packets"></span>

### 显示未加密的发现包

随着说话者和听众的运行,打开另一个终端开始 `tcpdump` 以查看网络流量。您需要使用 `sudo` 因为读取原始网络流量是一种特权操作.

以下命令使用 `-X` 选项以打印数据包内容, `-i` 选项,用于在任何界面上收听数据包,并仅抓取 [UDP 维基百科](https://en.wikipedia.org/wiki/User_Datagram_Protocol) 蚌埠7400交通.

``` console
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

这是一个发现数据图 - 寻找订阅者的谈话者。 您可以看到节点名称( NAME OF TRANSLATORS)`/talker_listener/talker`飞地和飞地 `/talker_listener/talker`)以纯文本传递。您还应看到从“% 1”中获取的类似发现数据。 `listener` 节点。 典型的发现包的一些其他特性 :

- 目的地地址为239.255.01,是一个多播IP地址;ROS 2使用多播流量默认发现.

- UDP 7400是目的地港口,按照 [DDS-RTPS 规格](https://www.omg.org/spec/DDSI-RTPS/About-DDSI-RTPS/).

- 包中包含“RTPS”标记,也定义为DDS-RTPS规格。

<span id="display-unencrypted-data-packets"></span>

### 显示未加密的数据包

使用 `tcpdump` 通过过滤在 UDP 端口上超过 7400 来捕捉非发现的 RTPS 数据包。 您将看到很少不同的数据包类型, 但请注意类似以下的数据, 这些数据显然是从说话者发送到听众的 :

``` console
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

有关此包的一些特性 :

- 信息内容“Hello World: 2135”,以明确文本发送

- 源和目的地IP地址是: `localhost`:由于两个节点都在同一个机器上运行,因此节点在其中发现了彼此. `localhost` 接口

<span id="enable-encryption"></span>

### 启用加密

停止谈话者和收听者节点。 通过设置安全环境变量并再次运行它们, 启用两者的加密 。

在1号航站楼

``` console
$ export ROS_SECURITY_KEYSTORE=~/sros2_demo/demo_keystore
$ export ROS_SECURITY_ENABLE=true
$ export ROS_SECURITY_STRATEGY=Enforce
$ ros2 run demo_nodes_cpp talker --ros-args --enclave /talker_listener/talker
```

在2号航站楼

``` console
$ export ROS_SECURITY_KEYSTORE=~/sros2_demo/demo_keystore
$ export ROS_SECURITY_ENABLE=true
$ export ROS_SECURITY_STRATEGY=Enforce
$ ros2 run demo_nodes_cpp listener --ros-args --enclave /talker_listener/listener
```

<span id="display-encrypted-discovery-packets"></span>

### 显示加密的发现包

运行相同 `tcpdump` 命令先前用来通过加密检查发现流量的输出 允许 典型的发现包看起来有点像以下:

``` console
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

此包更大, 包括可用于在 ROS 节点间设置加密的信息 。 正如我们不久后看到的, 这实际上包括一些在我们启用安全时创建的安全配置文件 。 有兴趣学习更多吗 ? 请查看优秀的纸张 。 [网络侦察和脆弱性挖掘安全DS系统](https://arxiv.org/abs/1908.05310) 来理解为什么这很重要。

<span id="display-encrypted-data-packets"></span>

### 显示加密数据包

现在使用 `tcpdump` 以获取数据包。一个典型的数据包看起来像如下:

``` console
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

这个RTPS包中的数据都是加密的.

除此数据包外, 您应该看到带有节点和飞地名称的额外数据包; 这些数据包支持其他 ROS 特性, 如参数和服务。 这些数据包的加密选项也可以由安全政策控制 。
