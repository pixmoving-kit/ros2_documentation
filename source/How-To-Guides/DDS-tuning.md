<span id="id1"></span>

<span id="dds-tuning-information"></span>
# DDS 调优说明

本页提供一些参数调优建议，用于解决在 Linux 上实际使用不同 DDS 实现时遇到的问题。在其他平台或未在此记录的厂商实现中，也可能遇到类似问题。

以下建议是调优的起点：这些设置在特定系统和环境中有效，但具体取值受多种因素影响。调试时，可能需要根据消息大小、网络拓扑等因素调高或调低参数。

应注意，调整参数可能消耗更多资源，并影响预期改进范围之外的系统功能。应根据具体情况权衡可靠性提升与其他不利影响。

<span id="cross-vendor-tuning"></span>
## 适用于不同厂商的调优

**问题：** 在有丢包的连接（通常是 Wi-Fi）上传输数据时，部分 IP 分片丢失可能导致接收端内核缓冲区被填满。

当一个 UDP 数据包缺失至少一个 IP 分片时，已收到的其余分片会占用内核缓冲区。默认情况下，Linux 内核尝试重组分片的超时时间为 30 秒。此时缓冲区可能已经满了（默认大小为 256 KB），无法再接收新分片，因此连接会表现为长时间“卡住”。

这一问题影响所有 DDS 厂商的实现，因此解决方法涉及内核参数调整。

**解决方法：使用尽力而为（best-effort）QoS，而不是可靠（reliable）QoS。**

尽力而为设置可减少网络流量，因为 DDS 无须承担可靠通信的开销：可靠模式下，发布者需要确认订阅者已收到消息，并重发未正确接收的样本。

但如果 IP 分片内核缓冲区被填满，仍会出现相同症状，即阻塞 30 秒。这种方法无需调整参数，就能在一定程度上改善问题。

**解决方法：减小 `ipfrag_time`。**

`net.ipv4.ipfrag_time`（对应 `/proc/sys/net/ipv4/ipfrag_time`，默认 30 秒）规定 IP 分片在内存中的保留时间。

例如，将其降为 3 秒：

```console
$ sudo sysctl net.ipv4.ipfrag_time=3
```

降低此值也会缩短无法接收新分片的时间窗口。该参数全局影响所有接收的分片，因此需要针对具体环境评估是否适合降低。

**解决方法：增大 `ipfrag_high_thresh`。**

`net.ipv4.ipfrag_high_thresh`（对应 `/proc/sys/net/ipv4/ipfrag_high_thresh`，默认 262144 字节）规定重组 IP 分片可使用的最大内存。

例如，增大到 128 MB：

```console
$ sudo sysctl net.ipv4.ipfrag_high_thresh=134217728     # (128 MB)
```

大幅提高该值，是为了尽量避免缓冲区被完全填满。不过，假如每个 UDP 数据包都缺少一个分片，要保存 `ipfrag_time` 时间窗口内收到的全部数据，该值可能需要设得非常高。

**问题：** 发送包含大型、变长、非基本类型数组的自定义消息，会产生很高的序列化和反序列化开销及 CPU 负载。这可能使发布者在 `publish()` 中耗时过长而停滞，也会使 `ros2 topic hz` 等工具报告的接收频率低于实际值。注意，`builtin_interfaces/Time` 也属于非基本类型，同样会增加序列化开销。因此，将 ROS 1 自定义消息类型直接迁移到 ROS 2 时，可能出现严重的性能下降。

**变通方法：** 用多个基本类型数组替代单个自定义类型数组，或像 `PointCloud2` 消息那样打包到字节数组中。例如，不要将 `FooArray` 定义为：

```bash
Foo[] my_large_array
```

其中 `Foo` 定义为：

```bash
uint64 foo_1
uint32 foo_2
```

而是将 `FooArray` 定义为：

```bash
uint64[] foo_1_array
uint32[] foo_2_array
```

<span id="fast-rtps-tuning"></span>
## Fast RTPS 调优

**问题：** 通过 Wi-Fi 传输大块数据或高频发布数据时，Fast RTPS 可能产生过多网络流量。

参见[适用于不同厂商的调优](#cross-vendor-tuning)中的解决方法。

<span id="cyclone-dds-tuning"></span>
<span id="cyclonedds-tuning"></span>
## Cyclone DDS 调优

**问题：** 即使采用可靠设置和有线网络，Cyclone DDS 仍无法可靠地传递大消息。

该问题[计划得到解决](https://github.com/eclipse-cyclonedds/cyclonedds/issues/484)。在此之前，可以采用以下解决方法，其调试使用了[这个测试程序](https://github.com/jacobperron/pc_pipe)。

**解决方法：** 增大 Linux 内核接收缓冲区上限，以及 Cyclone 使用的套接字接收缓冲区下限。

*以下调整用于处理 9 MB 消息：*

设置最大接收缓冲区大小 `rmem_max`：

```console
$ sudo sysctl -w net.core.rmem_max=2147483647
```

或者编辑 `/etc/sysctl.d/10-cyclone-max.conf`，写入以下内容以永久设置：

```bash
net.core.rmem_max=2147483647
```

接着，创建 Cyclone 启动时使用的配置文件，设置它请求的套接字接收缓冲区下限：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<CycloneDDS xmlns="https://cdds.io/config" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://cdds.io/config
https://raw.githubusercontent.com/eclipse-cyclonedds/cyclonedds/master/etc/cyclonedds.xsd">
    <Domain id="any">
        <Internal>
            <SocketReceiveBufferSize min="10MB"/>
        </Internal>
    </Domain>
</CycloneDDS>
```

之后每次运行节点前，设置以下环境变量：

```bash
CYCLONEDDS_URI=file:///absolute/path/to/config_file.xml
```

<span id="rti-connext-tuning"></span>
## RTI Connext 调优

**问题：** 即使采用可靠设置和有线网络，Connext 仍无法可靠地传递大消息。

**解决方法：** 使用此 [Connext QoS 配置](https://github.com/jacobperron/pc_pipe/blob/master/etc/ROS2TEST_QOS_PROFILES.xml)，并提高 `rmem_max`。

设置最大接收缓冲区大小：

```console
$ sudo sysctl -w net.core.rmem_max=4194304
```

将 Linux 内核的 `net.core.rmem_max` 调到 4 MB 后，该 QoS 配置可以实现真正可靠的传输。

测试证明，这一配置在单机上通过 SHMEM|UDPv4 或仅 UDPv4 都能可靠传递消息。两台通过 1 Gbps 以太网连接的机器也进行了测试：`rmem_max` 分别设为 4 MB 和 20 MB 时均无丢包，平均消息传递时间分别为 700 毫秒和 371 毫秒。

未调整内核 `rmem_max` 时，相同 Connext QoS 配置传递数据最长需要 12 秒，但至少总能完成传输。

**解决方法：** 使用上述 [Connext QoS 配置](https://github.com/jacobperron/pc_pipe/blob/master/etc/ROS2TEST_QOS_PROFILES.xml)，但**不调整** `rmem_max`。

ROS2TEST_QOS_PROFILES.xml 根据 RTI 的[流量控制器配置文档](https://community.rti.com/forum-topic/transfering-large-data-over-dds)设置，包含慢速、中速和快速流量控制器。

在我们的测试中，中速控制器效果最好。不过，仍需根据具体机器、网络和运行环境调优。Connext 流量控制器可用于调整带宽和发送数据的积极程度，但超过当前环境的带宽能力后，性能就会开始下降。
