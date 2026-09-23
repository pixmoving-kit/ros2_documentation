---
translation_status: machine_translated
source: Tutorials/Advanced/Discovery-Server/Discovery-Server.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-fast-dds-discovery-server-as-discovery-protocol-community-contributed"></span>

# 使用 Fast DDS Discovery Server 发现协议（社区贡献）

**目标：** 此教程将显示如何使用 ROS 2 节点启动 。 **快速 DDS 发现服务器** 发现协议。

**教程级别：** 高级

**用时：** 20分钟

<span id="background"></span>

## 背景

从ROS 2开始 口号Elusor, **快速 DDS 发现服务器** 协议是一个提供集中动态发现机制的功能,而不是默认在DDS中使用的分布式机制. 这个教程解释如何使用快速DDS发现服务器功能来运行一些ROS 2实例作为发现通信.

为了获得更多关于现有发现配置的信息,请检查 [随函附上文件](https://fast-dds.docs.eprosima.com/en/v2.1.0/fastdds/discovery/discovery.html) 或读取 [快速 DDS 发现服务器特定文档](https://fast-dds.docs.eprosima.com/en/v2.1.0/fastdds/discovery/discovery_server.html#discovery-server).

那个... [简单发现协议](https://fast-dds.docs.eprosima.com/en/v2.1.0/fastdds/discovery/simple.html) 是定义在 [DDS 标准](https://www.omg.org/omg-dds-portal/)然而,在某些设想中,它知道有缺点。

- 没有,没有 **缩放** 随着交换包数量随着新节点的加入而大幅增加,这些交换包将高效地增加。

- 这需要 **多播** 在某些情况下可能无法可靠发挥作用的能力,例如WiFi。

那个... **快速 DDS 发现服务器** 提供客户端- 服务器架构, 允许节点使用中间服务器连接。 每个节点都起到一个功能 *发现客户端*,将其信息与一个或多个共享 *发现服务器* 这减少了与发现有关的网络流量,也不需要多播能力。

![](figures/ds_explanation.svg)

这些发现服务器可以独立,重复或互相连接,以便建立网络上的冗余,避免出现单一的故障点.

<span id="fast-dds-discovery-server-v2"></span>

## 快速 DDS 发现服务器 v2

最新的ROS 2 Foxy Fitzroy发布(2020年12月)包括了新版本,即Fast DDS Discovery Server的第二版,该版本包括了一个新的过滤功能,可以进一步减少发送的发现消息的数量. 这个版本利用不同节点的主题来决定两个节点是否希望通信,或者它们是否可以被留作不匹配(即不互相发现). 下图显示了发现消息的减少:

![](figures/ds1vs2.svg)

此架构会大幅降低服务器和客户端之间发送的信件数量。 在下图中, 网络流量在发现阶段的减少 。 [RMF 临床演示](https://github.com/open-rmf/rmf_demos#Clinic-World) 显示 :

![](figures/discovery_server_v2_performance.svg)

为了使用此功能, 发现服务器可以使用 [参与者的 XML 配置](https://fast-dds.docs.eprosima.com/en/v2.1.0/fastdds/discovery/discovery_server.html#discovery-server)。也可以使用 `fastdds` [工具](https://fast-dds.docs.eprosima.com/en/v2.1.0/fastddscli/cli/cli.html#discovery) 备注: [环境变量](https://fast-dds.docs.eprosima.com/en/v2.1.0/fastdds/env_vars/env_vars.html),这是此教程中所用的方法。关于发现服务器配置的更详细解释,请访问 [Fast DDS 发现服务器文档](https://fast-dds.docs.eprosima.com/en/v2.1.0/fastdds/discovery/discovery_server.html#discovery-server).

<span id="prerequisites"></span>

## 前提条件

此教程假设您拥有 ROS 2 Foxy( 或更新) 。 [安装](../../../Installation.md)。如果安装时使用的是比Foxy低的ROS 2版本,则不能使用 `fastdds` 因此,为了使用“发现服务器”,您可以更新您的存储器,使用不同的快速DS版本,或者使用“发现服务器”配置“发现服务器”。 [快速 DDS XML QoS 配置](https://fast-dds.docs.eprosima.com/en/v2.1.0/fastdds/discovery/discovery_server.html#discovery-server).

<span id="run-this-tutorial"></span>

## 运行此教程

那个... `talker-listener` ROS 2 演示创建一个 `talker` 每秒发布一个“你好世界”信息的节点,以及 `listener` 收听这些消息的节点.

以 [采购ROS 2](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md) 您将会访问 CLI 工具 `fastdds`。该工具允许访问 [发现工具](https://fast-dds.docs.eprosima.com/en/v2.1.0/fastddscli/cli/cli.html#discovery),用于启动发现服务器。此服务器将管理连接到它的节点的发现过程。

> **重要**
>
> 别忘了 [来源 ROS 2](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md) 在每一个新终端打开。

<span id="setup-discovery-server"></span>

### 设置发现服务器

首先启动一个带有 id 0 的发现服务器, 端口 11811 (默认端口) , 并监听所有可用的接口 。

打开新的终端并运行 :

``` console
$ fastdds discovery --server-id 0
```

<span id="launch-listener-node"></span>

### 启动收听器节点

执行监听演示,以听 `/chatter` 主题。

在新的终端中设置环境变量 `ROS_DISCOVERY_SERVER` 到发现服务器的位置 。 (不要忘记在每个新终端中源ROS 2)

##### Linux

``` console
$ export ROS_DISCOVERY_SERVER=127.0.0.1:11811
```

##### Windows

``` console
$ set ROS_DISCOVERY_SERVER=127.0.0.1:11811
```

启动收听器节点。 使用参数 `--remap __node:=listener_discovery_server` 以更改此教程的节点名称。

``` console
$ ros2 run demo_nodes_cpp listener --ros-args --remap __node:=listener_discovery_server
```

这将创建一个ROS 2节点,该节点将自动为发现服务器创建客户端,并与之前为进行发现而创建的服务器连接,而不是使用多播.

<span id="launch-talker-node"></span>

### 发射谈话器节点

打开一个新的终端并设置 `ROS_DISCOVERY_SERVER` 环境变量和以前一样,使节点启动一个发现客户端。

##### Linux

``` console
$ export ROS_DISCOVERY_SERVER=127.0.0.1:11811
```

##### Windows

``` console
$ set ROS_DISCOVERY_SERVER=127.0.0.1:11811
```

``` console
$ ros2 run demo_nodes_cpp talker --ros-args --remap __node:=talker_discovery_server
```

现在,你应该看到演讲者发表“你好世界”信息,而听众则收到这些信息。

<span id="demonstrate-discovery-server-execution"></span>

### 演示发现服务器执行

迄今为止,没有证据表明这个例子和标准谈话者-听众的例子运行不同。要明确显示这一点,请运行另一个没有连接到发现服务器的节点。运行一个新的倾听者(请输入) `/chatter` 默认的话题)在新终端中,并检查它是否没有连接到已经运行的谈话者.

``` console
$ ros2 run demo_nodes_cpp listener --ros-args --remap __node:=simple_listener
```

新的听众节点不应收到“你好世界”的消息。

为了最终核实一切运行的正确性,可以使用简单的发现协议(默认的DDS分布式发现机制)来创建一个新的聊天器来进行发现.

``` console
$ ros2 run demo_nodes_cpp talker --ros-args --remap __node:=simple_talker
```

现在你应该看看 `simple_listener` 节点接收来自“你好世界”的消息 `simple_talker` 但不包括来自 `talker_discovery_server`.

<span id="visualization-tool-rqt-graph"></span>

### 可视化工具 `rqt_graph`

那个... `rqt_graph` 工具可以用来验证此示例的节点和结构。请记住,以便使用 `rqt_graph` 与发现服务器协议(即查看 `listener_discovery_server` 财务报告和财务报告 `talker_discovery_server` 节点) `ROS_DISCOVERY_SERVER` 在启动之前必须设定环境变量。

<span id="advanced-use-cases"></span>

## 高级使用案件

以下各节显示了发现服务器的不同功能,这些功能允许您在网络上构建一个强大的发现服务器.

<span id="server-redundancy"></span>

### 服务器冗余

通过使用 `fastdds` 工具,可以创建多个发现服务器。发现客户端(ROS节点)可以像期望的那样连接到许多服务器。这允许我们有一个冗余的网络,即使一些服务器或节点意外关闭也会工作。下图显示了一个提供服务器冗余的简单架构。

![](figures/ds_redundancy_example.svg)

在多个终端中,运行以下代码以建立与冗余服务器的通信.

``` console
$ fastdds discovery --server-id 0 --udp-address 127.0.0.1 --udp-port 11811
```

``` console
$ fastdds discovery --server-id 1 --udp-address 127.0.0.1 --udp-port 11888
```

> **重要**
>
> **了解服务器 ID 映射**
>
> 那个... `ROS_DISCOVERY_SERVER` 环境变量使用a **分号分隔列表** 其中每个位置对应服务器 ID。服务器 ID 由 **指数位置** (0-基于)在这个分号限制列表中,Not by the order servers show.
>
> - 服务器 `--server-id 0`: 第一个位置(不需要主分号)
>
> - 服务器 `--server-id 1`: 第二个位置(一个主要分号)
>
> - 服务器 `--server-id 2`: 第三个位置(两个主要分号)
>
> **实例:**
>
> - 为: `--server-id 0`: `ROS_DISCOVERY_SERVER="127.0.0.1:11811"`
>
> - 为: `--server-id 1`: `ROS_DISCOVERY_SERVER=";127.0.0.1:11888"`
>
> - 为: `--server-id 2`: `ROS_DISCOVERY_SERVER=";;127.0.0.1:11999"`
>
> - 用于多个服务器(0和1): `ROS_DISCOVERY_SERVER="127.0.0.1:11811;127.0.0.1:11888"`
>
> 如果服务器ID不匹配环境变量中的位置,客户端将无法连接到服务器.

##### Linux

``` console
$ export ROS_DISCOVERY_SERVER="127.0.0.1:11811;127.0.0.1:11888"
```

##### Windows

``` console
$ set ROS_DISCOVERY_SERVER="127.0.0.1:11811;127.0.0.1:11888"
```

``` console
$ ros2 run demo_nodes_cpp talker --ros-args --remap __node:=talker
```

##### Linux

``` console
$ export ROS_DISCOVERY_SERVER="127.0.0.1:11811;127.0.0.1:11888"
```

##### Windows

``` console
$ set ROS_DISCOVERY_SERVER="127.0.0.1:11811;127.0.0.1:11888"
```

``` console
$ ros2 run demo_nodes_cpp listener --ros-args --remap __node:=listener
```

现在,如果其中一个服务器失败,仍然会有发现能力,节点仍然会互相发现.

<span id="backup-server"></span>

### 备份服务器

Fast DDS 发现服务器允许创建具有备份功能的服务器。 这样服务器就可以恢复它保存的最后状态, 以防关闭 。

![](figures/ds_backup_example.svg)

在不同的终端中,运行以下代码以建立与备份服务器的通信.

``` console
$ fastdds discovery --server-id 0 --udp-address 127.0.0.1 --udp-port 11811 --backup
```

##### Linux

``` console
$ export ROS_DISCOVERY_SERVER="127.0.0.1:11811"
```

##### Windows

``` console
$ set ROS_DISCOVERY_SERVER="127.0.0.1:11811"
```

``` console
$ ros2 run demo_nodes_cpp talker --ros-args --remap __node:=talker
```

##### Linux

``` console
$ export ROS_DISCOVERY_SERVER="127.0.0.1:11811"
```

##### Windows

``` console
$ set ROS_DISCOVERY_SERVER="127.0.0.1:11811"
```

``` console
$ ros2 run demo_nodes_cpp listener --ros-args --remap __node:=listener
```

在发现服务器的工作目录(它推出的目录)中创建了若干备份文件。 `SQLite` 文件和两个 `json` 文件包含启动新服务器和在失败时恢复失败服务器状态所需的信息,避免了发现过程再次发生的必要性,并且不丢失信息.

<span id="discovery-partitions"></span>

### 发现分区

与发现服务器的通信可以被分割,以便在发现信息中创建虚拟分区。 这意味着两个端点只有在它们之间有共享的发现服务器或发现服务器网络时才能互相了解。 我们将用两个独立的服务器执行一个实例。 下图显示了架构 。

![](figures/ds_partition_example.svg)

用这个计谋 `Listener 1` 将连接到 `Talker 1` 财务报告和财务报告 `Talker 2`,作为他们共享的 `Server 1`. `Listener 2` 将连接到 `Talker 1` 他们分享的 `Server 2`。但是,我们没有。 `Listener 2` 将无法听到来自 `Talker 2` 因为它们不共享任何发现服务器或发现服务器,包括通过冗余发现服务器之间的连接间接分享.

用11811的默认端口运行第一个在本地主机上监听的服务器.

``` console
$ fastdds discovery --server-id 0 --udp-address 127.0.0.1 --udp-port 11811
```

在另一个终端运行第二台服务器,使用另一个端口在本地主机上监听,在此情况下是端口11888.

``` console
$ fastdds discovery --server-id 1 --udp-address 127.0.0.1 --udp-port 11888
```

现在,在不同的终端运行每个节点。使用 `ROS_DISCOVERY_SERVER` 环境变量来决定它们连接到哪个服务器。请注意 [编号必须匹配](https://fast-dds.docs.eprosima.com/en/v2.1.0/fastdds/env_vars/env_vars.html).

##### Linux

``` console
$ export ROS_DISCOVERY_SERVER="127.0.0.1:11811;127.0.0.1:11888"
```

##### Windows

``` console
$ set ROS_DISCOVERY_SERVER="127.0.0.1:11811;127.0.0.1:11888"
```

``` console
$ ros2 run demo_nodes_cpp talker --ros-args --remap __node:=talker_1
```

##### Linux

``` console
$ export ROS_DISCOVERY_SERVER="127.0.0.1:11811;127.0.0.1:11888"
```

##### Windows

``` console
$ set ROS_DISCOVERY_SERVER="127.0.0.1:11811;127.0.0.1:11888"
```

``` console
$ ros2 run demo_nodes_cpp listener --ros-args --remap __node:=listener_1
```

##### Linux

``` console
$ export ROS_DISCOVERY_SERVER="127.0.0.1:11811"
```

##### Windows

``` console
$ set ROS_DISCOVERY_SERVER="127.0.0.1:11811"
```

``` console
$ ros2 run demo_nodes_cpp talker --ros-args --remap __node:=talker_2
```

##### Linux

``` console
$ export ROS_DISCOVERY_SERVER=";127.0.0.1:11888"
```

##### Windows

``` console
$ set ROS_DISCOVERY_SERVER=";127.0.0.1:11888"
```

``` console
$ ros2 run demo_nodes_cpp listener --ros-args --remap __node:=listener_2
```

我们应该看看如何 `Listener 1` 正在接收来自两个谈话者节点的消息,同时 `Listener 2` 在一个不同的分区中, `Talker 2` 绝不然,他们绝没有从乐园获得任何消息。

> **说明**
>
> 一旦发现两个端点(ROS节点),它们就不需要它们之间的发现服务器网络来聆听对方的消息.

<span id="large-number-of-participants"></span>

### 人数众多

当在一个单一主机上运行超过100个DDS参与者时(例如,推出超过100个ROS 2). [语境](http://design.ros2.org/articles/Node_to_Participant_mapping.html) 同时,参与者可能无法发现彼此并失去响应。这既适用于发现服务器协议,也适用于简单发现协议。

> **说明**
>
> 每个数据DS *参加者* 对应 ROS 2 *一. 背景情况*,而不是一个ROS 2 *节点*。多个节点可以共享单个上下文,每个进程通常默认创建一个上下文。因此,参与者的数量取决于进程的数量(contexts),而不是节点的数量。

根本原因是 `mutation_tries` Fast DDS 中的参数,默认为 `100`。此参数控制了为每个参与者寻找唯一单一的监听端口的 Fast DDS 尝试次数。当参与者人数超过时 `mutation_tries`,端口分配被用尽,新参与者无法听从来客流量,实际上成为聋子.

> **警告**
>
> 在同一主机上拥有119名以上的参与者在一个域内,会导致其监听端口与下一个域ID的端口相撞.

为支助更多的参与者,增加 `mutation_tries` 通过下列 XML 配置 `FASTDDS_DEFAULT_PROFILES_FILE` 环境变量 :

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<dds xmlns="http://www.eprosima.com">
    <profiles>
        <participant profile_name="participant_profile" is_default_profile="true">
            <rtps>
                <builtin>
                    <mutation_tries>1000</mutation_tries>
                </builtin>
            </rtps>
        </participant>
    </profiles>
</dds>
```

保存此文件( 例如 ) `large_scale_configuration.xml`在启动节点之前设置环境变量 :

##### Linux

``` console
$ export FASTDDS_DEFAULT_PROFILES_FILE=large_scale_configuration.xml
```

##### Windows

``` console
$ set FASTDDS_DEFAULT_PROFILES_FILE=large_scale_configuration.xml
```

> **说明**
>
> 那个... `mutation_tries` 值应当设定为您想要运行在单一主机上的参与者数量。 超过需要时, 其增加不会产生负面的副作用。 此配置必须应用到 **全部( E)** 系统参与者,除了发现服务器,在发射时已经为此配置了特定的独角兽端口.

详情请参见: [关于参与者配置的快速 DDS 文件](https://fast-dds.docs.eprosima.com/en/latest/fastdds/xml_configuration/xml_configuration.html).

<span id="ros-2-introspection"></span>

## ROS 2 回顾

那个... [ROS 2 命令行接口](https://github.com/ros2/ros2cli) 支持多个内向工具来分析ROS 2网络的行为。这些工具(即: `ros2 bag record`, `ros2 topic list`))对于了解ROS 2工作网络非常有帮助.

这些工具大多使用 DDS 简单发现与每个现有参与者交换主题信息( 使用简单的发现, 网络中的每个参与者相互连接) 。 然而, 新的发现服务器 v2 执行了一个网络流量减少方案, 限制不共享主题的参与者之间的发现数据 。 这意味着节点只有在拥有此主题的作者或阅读器时才会接收主题的发现数据 。 由于大多数 ROS 2 CLI 需要网络中的节点( 有些依赖于运行中的 ROS 2 守护进程, 有些则创建自己的节点 ) , 使用 发现服务器 v2 这些节点不会拥有所有网络信息, 因此其功能将受到限制 。

发现服务器 v2 功能允许每个参与者作为一个运行 **超级客户端**,一种类型的 **客户端** 连接到 a **服务器**,它从中获取所有可用的发现信息(而不是它所需要的信息)。从这个意义上讲,ROS 2 的反演工具可以配置为 **超级客户端**,从而能够发现网络中每一个正在使用发现服务器协议的实体。

> **说明**
>
> 本节中,我们使用这个术语 *参加者* 作为 DDS 实体。每个 DDS *参加者* 对应于ROS 2 *一. 背景情况*一个ROS 2 的缩写 相对于 DDS。 [节点](../../Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md#ros2nodes) 是依赖于 DDS 通信接口的ROS 2 实体 : `DataWriter` 财务报告和财务报告 `DataReader`每一个 *参加者* 有关这些概念的进一步详情,请访问 [向参与者提供绘图设计文件的节点](http://design.ros2.org/articles/Node_to_Participant_mapping.html)

<span id="daemon-s-related-tools"></span>

### 守护进程的相关工具

ROS 2 守护进程用于多个ROS 2 CLI 内向检查工具中。 它创建了自己的参与者, 在网络图中添加一个ROS 2 节点, 以便接收发送的所有数据。 为了使 ROS 2 CLI 在使用发现服务器机制时工作, ROS 2 守护进程需要配置为 **超级客户端**因此,本节专门解释如何使用ROS 2 CLI,将ROS 2 Daemon作为程序运行。 **超级客户端**。这将使守护进程能够发现整个节点图,并接收所有主题和端点信息。为此,使用了快速DS XML配置文件来配置ROS 2守护进程和 CLI 工具。

您可以在下面找到 XML 配置配置配置文件, 对于此教程, 应在工作目录中保存为 `` `super_client_configuration_file.xml` `` 文件。此文件将配置每一个使用它的新参与者, 作为 **超级客户端**.

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
 <dds>
     <profiles xmlns="http://www.eprosima.com/XMLSchemas/fastRTPS_Profiles">
         <participant profile_name="super_client_profile" is_default_profile="true">
             <rtps>
                 <builtin>
                     <discovery_config>
                         <discoveryProtocol>SUPER_CLIENT</discoveryProtocol>
                         <discoveryServersList>
                             <RemoteServer prefix="44.53.00.5f.45.50.52.4f.53.49.4d.41">
                                 <metatrafficUnicastLocatorList>
                                     <locator>
                                         <udpv4>
                                             <address>127.0.0.1</address>
                                             <port>11811</port>
                                         </udpv4>
                                     </locator>
                                 </metatrafficUnicastLocatorList>
                             </RemoteServer>
                         </discoveryServersList>
                     </discovery_config>
                 </builtin>
             </rtps>
         </participant>
     </profiles>
 </dds>
```

> **说明**
>
> 下层 *远程服务器* 标签,该 *前缀* 属性值应该根据 CLI 上传的服务器 ID 进行更新(参见 [快速 DDS CLI](https://fast-dds.docs.eprosima.com/en/latest/fastddscli/cli/cli.html#discovery)。在显示的 XML 片断中指定的值与值 0 的ID相对应。

首先, 立即使用发现服务器 [快速 DDS CLI](https://fast-dds.docs.eprosima.com/en/latest/fastddscli/cli/cli.html#discovery) 指定值为 0 的标识。

``` console
$ fastdds discovery -i 0 -l 127.0.0.1 -p 11811
```

运行一个会通过服务器互相发现的谈话者和听众(通知) `ROS_DISCOVERY_SERVER` 配置与其中的配置相同 `super_client_configuration_file.xml`).

##### Linux

``` console
$ export ROS_DISCOVERY_SERVER="127.0.0.1:11811"
```

##### Windows

``` console
$ set ROS_DISCOVERY_SERVER="127.0.0.1:11811"
```

``` console
$ ros2 run demo_nodes_cpp listener --ros-args --remap __node:=listener
```

##### Linux

``` console
$ export ROS_DISCOVERY_SERVER="127.0.0.1:11811"
```

##### Windows

``` console
$ set ROS_DISCOVERY_SERVER="127.0.0.1:11811"
```

``` console
$ ros2 run demo_nodes_cpp talker --ros-args --remap __node:=talker
```

然后,立即使用 ROS 2 守护进程 **超级客户端** 配置(记住每个新终端的源ROS 2安装).

##### Linux

``` console
$ export FASTRTPS_DEFAULT_PROFILES_FILE=super_client_configuration_file.xml
```

##### Windows

``` console
$ set FASTRTPS_DEFAULT_PROFILES_FILE=super_client_configuration_file.xml
```

``` console
$ ros2 daemon stop
$ ros2 daemon start
$ ros2 topic list
$ ros2 node info /talker
$ ros2 topic info /chatter
$ ros2 topic echo /chatter
```

我们还可以使用 ROS 2 工具看到节点图 `rqt_graph` 如下(可能需要按刷新按钮):

##### Linux

``` console
$ export FASTRTPS_DEFAULT_PROFILES_FILE=super_client_configuration_file.xml
```

##### Windows

``` console
$ set FASTRTPS_DEFAULT_PROFILES_FILE=super_client_configuration_file.xml
```

``` console
$ ros2 run rqt_graph rqt_graph
```

<span id="no-daemon-tools"></span>

### 没有守护进程工具

一些ROS 2 CLI 工具不使用 ROS 2 守护进程。 要让这些工具与发现服务器连接, 并接收所有所需的主题信息, 需要作为即时 **超级客户端** 连接到 **服务器**.

遵循之前的配置, 构建一个简单的系统, 并配有说话者和听众。 首先, 运行一个 **服务器**:

``` console
$ fastdds discovery -i 0 -l 127.0.0.1 -p 11811
```

然后,在不同的终端上运行说话者和听众:

##### Linux

``` console
$ export ROS_DISCOVERY_SERVER="127.0.0.1:11811"
```

##### Windows

``` console
$ set ROS_DISCOVERY_SERVER="127.0.0.1:11811"
```

``` console
$ ros2 run demo_nodes_cpp listener --ros-args --remap __node:=listener
```

##### Linux

``` console
$ export ROS_DISCOVERY_SERVER="127.0.0.1:11811"
```

##### Windows

``` console
$ set ROS_DISCOVERY_SERVER="127.0.0.1:11811"
```

``` console
$ ros2 run demo_nodes_cpp talker --ros-args --remap __node:=talker
```

继续使用 ROS 2 CLI 与 `--no-daemon` 选项。新的节点将与现有的服务器连接,并了解每个主题。导出 `ROS_DISCOVERY_SERVER` 不需要,因为 ROS 2 工具将通过 `FASTRTPS_DEFAULT_PROFILES_FILE`.

##### Linux

``` console
$ export FASTRTPS_DEFAULT_PROFILES_FILE=super_client_configuration_file.xml
```

##### Windows

``` console
$ set FASTRTPS_DEFAULT_PROFILES_FILE=super_client_configuration_file.xml
```

``` console
$ ros2 topic list --no-daemon
$ ros2 node info /talker --no-daemon --spin-time 2
```

<span id="compare-fast-dds-discovery-server-with-simple-discovery-protocol"></span>

## 将快速 DDS 发现服务器与简单发现协议进行比较

为了比较使用 *简单发现* 协议(分布式发现的默认 DDS 机制)或 *发现服务器*,提供了两个脚本,用于执行一个说话者和许多听众,并分析在此期间的网络流量。对于这个实验, `tshark` 需要安装在您的系统中。为了避免使用进程内部模式,配置文件是强制性的。

> **说明**
>
> 这些脚本只支持在Linux上,并且需要一个发现服务器的关闭功能,这个功能只能从ROS 2 Foxy中提供的版本中获取更新的版本. 为了使用这个功能,用快DDS v2.1.0或更高版本编译ROS 2.

这些脚本的特性是高级用途的参考文献,其研究留给用户进行.

- [`bash network traffic generator`](scripts/generate_discovery_packages.bash)

- [`python3 graph generator`](scripts/discovery_packets.py)

- [`XML configuration`](scripts/no_intraprocess_configuration.xml)

以路径运行 bash 脚本 `setup.bash` 文件以源 ROS 2 作为参数。 这将生成流量追踪以进行简单的发现。 用第二个参数执行相同的脚本 `SERVER`。它将生成用于使用发现服务器的跟踪。

> **说明**
>
> 取决于您的配置 `tcpdump`,此脚本可能需要 `sudo` 权限读取网络设备的流量。

在两次处决完成后,运行Python脚本生成一个类似于下面的图.

``` console
$ export FASTRTPS_DEFAULT_PROFILES_FILE="no_intraprocess_configuration.xml"
$ sudo bash generate_discovery_packages.bash ~/ros2/install/local_setup.bash
$ sudo bash generate_discovery_packages.bash ~/ros2/install/local_setup.bash SERVER
$ python3 discovery_packets.py
```

![](figures/discovery_packets.svg)

这个图是实验的特定运行的结果,读者可以执行脚本并产生自己的结果进行比较,可以很容易地看到在使用发现服务时网络流量会减少.

流量的减少是避免每个节点宣布自己,等待网络上其他每个节点的响应的结果,这在大型架构中创造了大量的流量,从这种方法中减少的节点数量会随着节点数量的增加而增加,使得这个架构比简单的发现协议方法更具可扩展性.

新的快速 DDS 发现服务器 v2 自此可供使用 *Fast DDS* v2.0.2, 替换旧的发现服务器。 在这个新版本中, 不共享话题的节点会自动无法发现彼此, 保存连接它们及其端点所需的全部发现数据。 上面的实验没有显示这个案例, 但即使如此, 由于ROS 2 节点隐藏的基础设施主题, 流量的大幅下降也能得到赞赏 。
