---
translation_status: machine_translated
source: Concepts/Intermediate/About-Security.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="ros-2-security"></span>

# ROS 2 安全

<span id="overview"></span>

## 概述

ROS 2 包含在 ROS 2 计算图中实现节点之间通信安全的能力。 与发现类似, 安全通过基础的 ROS 2 中件发生( 前提是它支持相应的安全插件 ) 。 不需要额外的软件安装来启动安全性; 但是, 中间件需要每个 ROS 图表参与者的配置文件。 这些文件允许加密和认证, 并为单个节点和整个 ROS 图表定义政策 。 ROS 2 也增加了一个“ 开关” 主切换来控制安全行为 。

ROS公用事业公司可以创建权威 [信任锚](https://en.wikipedia.org/wiki/Trust_anchor) 用于ROS应用程序,或者可以使用外部证书权威.

内建的ROS 2 安全特性可以控制整个ROS图中的通信,这不仅允许在ROS域参与者之间加密传输中的数据,还允许对参与者发送数据进行认证,确保数据发送的完整性,并实现全域访问控制.

安保服务由以下单位提供: [数据分发处(DDS)](https://www.omg.org/spec/DDS/) 用于节点之间的通信。DDS供应商提供与ROS合作的开源软件和商业的DDS执行。然而,为了建立符合规格的DDS执行,所有供应商都必须包括文件概述的安全插件。 [DDS 安全规格](https://www.omg.org/spec/DDS-SECURITY/About-DDS-SECURITY/). ROS安全特性利用这些DDS安全插件提供基于政策的加密,认证和访问控制. DDS和ROS安全是通过预定义的配置文件和环境变量实现的.

<span id="the-security-enclave"></span>

## 安全飞地

安全飞地封装了保护ROS通信的单一政策。 飞地可以设置多个节点、 整个ROS图、 或受保护的ROS 进程与设备组合的政策。 安全飞地可以灵活地映射到进程、 用户或部署时的设备。 调整这种默认行为对于优化通信和复杂的系统非常重要。 见 ROS 2 安全飞地 [设计文件](https://design.ros2.org/articles/ros2_security_enclaves.html) 详细情况。

<span id="security-files"></span>

## 安全文件

A [ROS 2 安全飞地](https://design.ros2.org/articles/ros2_security_enclaves.html) 其中3个文件定义了飞地的身份,而另外3个文件则定义了给予飞地的权限。所有6个文件都居住在一个单一的目录中,而发射的节点没有合格的飞地路径使用默认的根级飞地中的文件。

<span id="enclave-identity"></span>

### 飞地身份

身份证书管理局文件 `identity_ca.cert.pem` 作为用于识别参与者的信任锚点。每个飞地在文件中也持有其独特的识别证书。 `cert.pem`,以及文件中相关的私人密钥 `key.pem`因为 `cert.pem` 证书已经通过身份证书签署,当参与者向其他域成员提交此证书时,他们能够使用自己的身份证书副本验证参与者的身份。这种有效的证书交换使飞地能够安全地建立与其他参与者的可信通信。飞地不共享该证书。 `key.pem` 私钥,但仅用于解密和信件签名.

<span id="enclave-permissions"></span>

### 飞地权限

权限证书权限文件 `permissions_ca.cert.pem` 用作信任锚,为安全飞地授予权限。此证书用于创建签名文件 `governance.p7s`,一个XML文档,该文档定义了全域保护政策。类似地,XML文件 `permissions.p7s` 大纲此特定飞地的权限, 并且已经由权限 CA 签名。 域名成员使用权限 CA 的副本来验证这些已签名的文件并批准请求的访问 。

虽然这两个证书当局为身份和权限提供了不同的工作流程,但通常同一证书既是身份,又是权限当局.

<span id="private-keys"></span>

### 私钥

身份和权限证书也关联私人密钥文件。 以身份证书的私人密钥签署证书签名请求( CSR) , 从而在域中添加新的飞地 。 同样, 用权限证书的私人密钥签署 XML 文件, 以批准新的飞地 。

<span id="security-environment-variables"></span>

## 安全环境变量

环境变量 `ROS_SECURITY_ENABLE` 操作飞地的主“ 打开/ 关闭” 交换器, 用于ROS 2 安全特性。 安全默认已被关闭, 因此即使存在适当的安全文件, 安全特性也不会被启用 。 为了启用ROS 2 安全性, 请设置此环境变量到 `true` (案件敏感).

一旦安全启用,环境可变因素 `ROS_SECURITY_STRATEGY` 定义域参与者在发射参与者时如何处理问题 。 安全特性取决于证书和正确签名的配置文件, 但默认情况下, 配置不当的参与者仍然会成功发射, 但没有安全特性 。 为了强制严格遵守安全设置, 并且未能发射不符合要求的飞地, 请设置此环境变量 。 `Enforce` (案件敏感).

与安保有关的其他环境变量可见于 [ROS 2 DDS-安全整合设计文件](https://design.ros2.org/articles/ros2_dds_security.html)。这些变量一般有助于ROS管理飞地和查找安全文件。

<span id="learn-more"></span>

## 学习更多

更多信息和操作练习,使ROS 2通信安全,见: [配置安全机制](../../Tutorials/Advanced/Security/Introducing-ros2-security.md).
