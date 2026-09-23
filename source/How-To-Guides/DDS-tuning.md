---
translation_status: machine_translated
source: How-To-Guides/DDS-tuning.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="dds-tuning-information"></span>

# DDS 调优

本页面就参数调试提供了一些指导,这些参数在现实世界情况下使用 Linux 上的各种 DDS 执行程序处理所面临的问题。 Linux 上或使用一个供应商时,我们发现的问题可能会出现于此处没有记录的其他平台和供应商。

下面的建议是调试的起点;它们为特定的系统和环境工作,但调试可能因若干因素而异。您可能需要在调试相对于消息大小,网络地形等因素时增加或降低数值.

必须认识到,调整参数可能牺牲资源,并可能影响所期望的改进范围以外的部分系统。 提高可靠性的好处应当与每个案例的任何不利因素权衡。

<span id="cross-vendor-tuning"></span> <span id="id1"></span>

## 交叉报仇调制

**问题:** 在丢失(通常是WiFi)连接上发送数据,当一些IP片段被丢弃时就会出现问题,可能导致接收方的内核缓冲器满载.

当一个 UDP 包丢失至少一个IP 片段时, 其余接收的片段会填充内核缓冲器。 默认情况下, Linux 内核会在试图重压缩包片段的30s 后超时。 由于此时内核缓冲器是满的( 默认大小为 256KB) , 无法输入新的片段, 因此连接会长时期看似“ 挂” 。

这个问题在所有DDS供应商中都是通用的,因此解决方案涉及调整内核参数.

**解决方案 :** 使用最佳的QoS设置而不是可靠 。

最佳设置会减少网络流量,因为DDS执行不需要支付可靠通信的间接费用,因为出版商需要向订阅者发送消息的确认,必须重新发送未正确接收的样本.

如果IP碎片的内核缓冲器满了,那么症状还是一样(阻塞30s ) 。 这个解决方案应该能在一定程度上改善问题,而不必调整参数。

**解决方案 :** 降低其价值 `ipfrag_time` 参数。

`net.ipv4.ipfrag_time / /proc/sys/net/ipv4/ipfrag_time` (默认 30s) : 将IP片段保存在内存中的时间为秒.

例如,通过运行将数值降低到 3s :

``` console
$ sudo sysctl net.ipv4.ipfrag_time=3
```

减少这个参数的值也减少了没有收到碎片的时间之窗。 这个参数对于所有进入的碎片都是全球性的,因此每个环境都需要考虑降低其值的可行性。

**解决方案 :** 增加该表的价值 `ipfrag_high_thresh` 参数。

`net.ipv4.ipfrag_high_thresh / /proc/sys/net/ipv4/ipfrag_high_thresh` (默认: 262144字节):用于重新组装IP片段的最大内存.

例如,通过运行将值提高到128MB:

``` console
$ sudo sysctl net.ipv4.ipfrag_high_thresh=134217728     # (128 MB)
```

大幅提高这个参数的值是为了确保缓冲器永远不会完全满载。 但是,要保存在时间窗口里收到的所有数据,其值很可能是很高的。 `ipfrag_time`,假设每个UDP包缺少一个片段.

**问题:** 发送自定义消息时带有大量非原生类型的可变大小阵列, 会导致高序化/ 淡化率和CPU 负载。 这可能导致出版商的延迟, 因为花费过多的时间 `publish()` 和工具,例如: `ros2 topic hz` 举例来说,请注意: `builtin_interfaces/Time` 由于串行管理费增加,当天真地将自定义信件类型从ROS 1 转换为ROS 2 时,可以观察到严重性能退化。

**工作间:** 使用多个原始数组,而不是一个自定义类型的单数组,或者按下列方式将数组组合成字节数组: `PointCloud2` 。例如,而不是定义一个 `FooArray` 消息为:

``` bash
Foo[] my_large_array
```

与 `Foo` 定义如下:

``` bash
uint64 foo_1
uint32 foo_2
```

相反,定义 `FooArray` 成为:

``` bash
uint64[] foo_1_array
uint32[] foo_2_array
```

<span id="fast-rtps-tuning"></span>

## 快速RTPS 调制

**问题:** 快速RTPS在WiFi上运行时将大量数据或快速发布的数据淹没在网络中.

见下面的解决方案 [交叉报仇调制](#cross-vendor-tuning).

<span id="cyclone-dds-tuning"></span> <span id="cyclonedds-tuning"></span>

## 旋风 DDS 调制

**问题:** 尽管使用了可靠的设置和通过有线网络传输,气旋DDS并没有可靠地发送大型信息.

这个问题应该是: [不久后发函](https://github.com/eclipse-cyclonedds/cyclonedds/issues/484)。在那之前,我们已经想出了以下解决方案(调试使用 [此测试程序](https://github.com/jacobperron/pc_pipe)):

**解决方案 :** 增加最大Linux内核接收缓冲大小,最小套接字接收气旋使用的缓冲大小.

*9MB 消息的解析调整 :*

设置最大接收缓冲大小, `rmem_max`,通过运行:

> ``` console
> $ sudo sysctl -w net.core.rmem_max=2147483647
> ```

或者通过编辑永久设定它 `/etc/sysctl.d/10-cyclone-max.conf` 要包含的文件 :

> ``` bash
> net.core.rmem_max=2147483647
> ```

其次,要设置最小套接字接收气旋所要求的缓冲大小,请写出一个配置文件供气旋在启动时使用,比如:

``` xml
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

然后,每当您要运行一个节点时,设置以下环境变量:

``` bash
CYCLONEDDS_URI=file:///absolute/path/to/config_file.xml
```

<span id="rti-connext-tuning"></span>

## RTI 连接调制

**问题:** Connext虽然使用可靠的设置和通过有线网络传输,但无法可靠地发送大消息.

**解决方案 :** 这个 [连接QoS 配置文件](https://github.com/jacobperron/pc_pipe/blob/master/etc/ROS2TEST_QOS_PROFILES.xml),同时增加 `rmem_max` 参数。

设置最大接收缓冲大小, `rmem_max`,通过运行:

> ``` console
> $ sudo sysctl -w net.core.rmem_max=4194304
> ```

通过调音 `net.core.rmem_max` 到 Linux 内核中的 4MB, QoS profile 可以产生真正可靠的行为.

这种配置已被证明通过SHMEMQUDPv4可靠地传送消息,并且单机上只有UDPv4。还测试了多机配置 。 `rmem_max` 在4MB和20MB(两台与1Gbpseternet连接的机器),没有投放消息,平均消息发送时间分别为700ms和371ms.

不配置内核 `rmem_max`,同样的 Connext QoS 剖面图需要12秒才能交付数据。然而,它总是至少能够完成交付。

**解决方案 :** 使用该 [连接QoS 配置文件](https://github.com/jacobperron/pc_pipe/blob/master/etc/ROS2TEST_QOS_PROFILES.xml) *不含* 调整 `rmem_max`.

ROS2TESTQOS_PROFILES.xml 文件是使用 RTI 的文档配置的。 [配置流量控制器](https://community.rti.com/forum-topic/transfering-large-data-over-dds). 它有慢,中和快速的流量控制器(见Connext QoS profile链接).

中流控制器为我们的情况带来了最佳结果。 然而, 控制器仍然需要为它们正在操作的特定机器/ 网络/ 环境调制。 Connext 流控制器可用于调制带宽及其发送数据的积极性, 尽管一个特定设置的带宽一旦通过, 性能将开始下降 。
