---
translation_status: machine_translated
source: Concepts/Intermediate/About-Domain-ID.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="the-ros-domain-id"></span>

# ROS_DOMAIN_ID

<span id="overview"></span>

## 概述

如其他解释,ROS 2用于通信的默认中间软件是DDS. 在DDS中,拥有不同逻辑网络共享物理网络的主要机制被称为域名ID. 同一域上的ROS 2节点可以自由发现并发送消息,而不同域上的ROS 2节点则不能. 所有ROS 2节点默认使用域名ID 0. 为了避免在同一网络上运行ROS 2的不同组计算机之间的干扰,应当为每个组设置不同的域名ID.

<span id="choosing-a-domain-id-short-version"></span>

## 选择一个域名( 短版本)

下面的文字解释了应该用于ROS 2. 的域名ID范围的衍生,要跳过这个背景并仅仅选择一个安全号码,只需在0到101之间选择一个域名ID,包含在内.

<span id="choosing-a-domain-id-long-version"></span>

## 选择域名 ID( 长版本)

域名ID 被 DDS 用于计算用于发现和通信的 UDP 端口。参见 [本条](https://community.rti.com/content/forum-topic/statically-configure-firewall-let-omg-dds-traffic-through) 用于详细了解端口的计算方式。考虑到我们的基本网络, UDP 端口是一个 [无符号 16 位整数](https://en.wikipedia.org/wiki/User_Datagram_Protocol#Ports)。因此,可以分配的最高端口号是65535。用上面文章中的公式进行一些数学,这意味着可以分配的最高域ID是232,而可以分配的最低域ID是0。

<span id="platform-specific-constraints"></span>

### 特定平台的制约因素

对于最大程度的兼容性,在选择域名ID时应当遵循一些额外的特定平台限制。特别是,最好避免在操作系统中分配域名ID。 [电子端口范围](https://en.wikipedia.org/wiki/Ephemeral_port)。这避免了ROS 2节点所使用的端口与计算机上的其他联网服务之间可能发生的冲突。

以下是一些针对平台的关于麻黄口的注释.

##### Linux

默认情况下, Linux 内核会使用端口 32768- 60999 来表示麻黄端口。 这意味着域名 ID 0- 101 和 215-232 可以在不与麻黄端口相撞的情况下安全使用。 麻黄端口范围可以通过设置自定义值来在 Linux 中配置 。 `/proc/sys/net/ipv4/ip_local_port_range`。如果使用自定义的麻黄口岸范围,上述数字可能必须作相应调整。

##### macOS

默认情况下, macOS 上的 ephemeral port 范围为 49152-65535。 这意味着 域名 ID 0-166 可以安全使用而不与 ephemeral port 相冲突。 ephemeral port 范围可以通过设置自定义 sysctl 值来在 macOS 中配置 。 `net.inet.ip.portrange.first` 财务报告和财务报告 `net.inet.ip.portrange.last`。如果使用自定义的麻黄口岸范围,上述数字可能必须作相应调整。

##### Windows

默认情况下, Windows 上的 ephemeral port 范围为 49152-65535。 这意味着 域名 ID 0-166 可以安全使用而不与 ephemeral port 相撞 。 ephemeral port 范围可以在 Windows 中配置 。 [使用净值sh](https://docs.microsoft.com/en-us/troubleshoot/windows-server/networking/default-dynamic-port-range-tcpip-chang)。如果使用自定义的麻黄口岸范围,上述数字可能必须作相应调整。

<span id="participant-constraints"></span>

### B. 参加者的制约因素

对于运行在计算机上的每个ROS 2进程,都会创建一个DDS“参与者 ” 。由于每个DDS参与者在计算机上占据两个端口,运行在一个计算机上的120多个ROS 2进程可能会溢入其他域名ID或电子端口。

要了解原因,请考虑域名ID1和2.

- 域名ID 1使用端口7650和7651进行多播.

- 域ID 2使用端口7900和7901进行多播.

- 在创建域ID1的第1个进程(零参与者)时,端口7660和7661用于unicast.

- 在创建域ID1的第120个流程(119名参与者)时,端口7898和7899被用于unicast.

- 在创建域ID1的第121程序(120位参与者)时,端口7900和7901用于独播,并与域ID2重叠.

如果知道计算机一次只会在单个域名ID上出现,且域名ID足够低,那么创建比这个更多的ROS 2进程是安全的.

在选择一个接近平台特定域ID范围的域ID时,应当考虑另一个约束.

例如,假设一个Linux计算机,其域号为101:

- 计算机上的零ROS 2过程将连接到32650,32651,32660,和32661的港口.

- 计算机上的第一个ROS 2过程将连接到32650,32651,32662,和32663的港口.

- 计算机上的第53ROS 2流程将连接到32650,32651,32766,和32767的港口.

- 计算机上的第54个ROS 2流程将连接到32650,32651,32768,和32769的港口,运行于麻黄口岸范围.

因此,在Linux上使用域名ID 101时所应创建的流程的最大数量是54. 同样,在Linux上使用域名ID 232时所应当创建的流程的最大数量是63,因为最大端口数量是65535.

macOS和Windows上的情况相似,虽然数字不同. 在macOS和Windows上,当选择166域ID(范围最顶端)时,在运行到ephemeral端口范围前可以在计算机上创建的ROS 2进程的最大数量为120个.

<span id="domain-id-to-udp-port-calculator"></span>

### 域名ID 到 UDP 端口计算器

|            |                                  |
|-----------:|----------------------------------|
|     域名 : | <span id="domainID"></span>      |
| 参会人数 : | <span id="participantID"></span> |

------------------------------------------------------------------------

|                     |     |
|--------------------:|-----|
|      发现多播端口 : |     |
|      用户多播端口 : |     |
|  发现Unicast 端口 : |     |
| 用户 Unicast 端口 : |     |

\
\
