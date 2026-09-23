<span id="ros-2-security"></span>
# ROS 2 安全机制

<span id="overview"></span>
## 概述

ROS 2 能够保护其计算图中节点之间的通信。与发现机制类似，安全功能由 ROS 2 的底层中间件提供，前提是中间件支持相应的安全插件。启用安全功能不需要安装额外软件，但中间件需要为 ROS 图中的每个参与者提供配置文件。这些文件用于启用加密和身份认证，并为单个节点及整个 ROS 图定义策略。ROS 2 还提供了控制安全行为的总开关。

可以使用 ROS 工具为 ROS 应用创建权威的[信任锚](https://en.wikipedia.org/wiki/Trust_anchor)，也可以使用外部证书颁发机构。

ROS 2 内置的安全功能能够控制整个 ROS 图中的通信。它不仅可以加密 ROS 域参与者之间传输的数据，还可以认证发送数据的参与者身份，确保所发送数据的完整性，并在整个域内实施访问控制。

ROS 2 的安全服务由负责节点间通信的底层[数据分发服务（DDS）](https://www.omg.org/spec/DDS/)提供。DDS 供应商提供可与 ROS 配合使用的开源实现和商业实现。为了创建符合规范的 DDS 实现，所有供应商都必须包含 [DDS 安全规范](https://www.omg.org/spec/DDS-SECURITY/About-DDS-SECURITY/)中规定的安全插件。ROS 安全功能利用这些 DDS 安全插件，提供基于策略的加密、身份认证和访问控制。DDS 和 ROS 的安全功能通过预定义的配置文件和环境变量启用。

<span id="the-security-enclave"></span>
## 安全域（Security Enclave）

安全域封装了一套用于保护 ROS 通信的策略。这套策略可以适用于多个节点、整个 ROS 图，或者受保护的 ROS 进程与设备的任意组合。部署时，可以将安全域灵活地映射到进程、用户或设备。对于通信优化和复杂系统，调整这一默认行为十分重要。更多详情请参见 [ROS 2 安全域设计文档](https://design.ros2.org/articles/ros2_security_enclaves.html)。

<span id="security-files"></span>
## 安全文件

按照 DDS 规范，[ROS 2 安全域](https://design.ros2.org/articles/ros2_security_enclaves.html)由六个文件建立。其中三个文件定义安全域的身份，另外三个文件定义授予安全域的权限。所有六个文件都位于同一目录中。启动节点时，如果未指定完整的安全域路径，就会使用默认根级安全域中的文件。

<span id="enclave-identity"></span>
### 安全域身份

身份认证 CA 文件 `identity_ca.cert.pem` 是用于识别参与者的信任锚。每个安全域还通过 `cert.pem` 文件保存其唯一的身份证书，通过 `key.pem` 文件保存对应的私钥。由于 `cert.pem` 证书已由身份认证 CA 签名，参与者向域内其他成员出示该证书时，其他成员可以使用各自持有的身份认证 CA 证书副本来验证参与者身份。通过这种有效的证书交换，安全域可以与其他参与者安全地建立可信通信。安全域不会共享 `key.pem` 私钥，只会将其用于解密和消息签名。

<span id="enclave-permissions"></span>
### 安全域权限

权限 CA 文件 `permissions_ca.cert.pem` 是为安全域授予权限的信任锚。该证书用于生成签名文件 `governance.p7s`，它是一个定义域级保护策略的 XML 文档。同样，XML 文件 `permissions.p7s` 描述此安全域的权限，并由权限 CA 签名。域内成员使用权限 CA 证书的副本来验证这些签名文件，并授予所请求的访问权限。

虽然这两个证书颁发机构允许将身份认证和权限管理分成独立的工作流程，但通常会使用同一个证书同时承担身份认证和权限授权两种角色。

<span id="private-keys"></span>
### 私钥

身份认证 CA 证书和权限 CA 证书也各自具有对应的私钥文件。使用身份认证 CA 证书的私钥签署新安全域的证书签名请求（CSR），即可将其加入域中。同样，使用权限 CA 证书的私钥签署权限 XML 文档，即可向新安全域授予权限。

<span id="security-environment-variables"></span>
## 安全相关环境变量

环境变量 `ROS_SECURITY_ENABLE` 是安全域中 ROS 2 安全功能的总开关。默认情况下安全功能处于关闭状态，因此即使存在正确的安全文件，也不会启用安全功能。要启用 ROS 2 安全功能，请将此环境变量设为 `true`，注意区分大小写。

启用安全功能后，环境变量 `ROS_SECURITY_STRATEGY` 决定域参与者在启动时如何处理问题。安全功能依赖证书和正确签名的配置文件，但默认情况下，配置不正确的参与者仍然可以成功启动，只是不会启用安全功能。要严格遵守安全设置，并禁止不符合要求的安全域启动，请将此环境变量设为 `Enforce`，注意区分大小写。

其他安全相关环境变量见 [ROS 2 DDS-Security 集成设计文档](https://design.ros2.org/articles/ros2_dds_security.html)。这些变量主要帮助 ROS 管理安全域并定位安全文件。

<span id="learn-more"></span>
## 了解更多

有关启用 ROS 2 通信安全的更多信息和实践练习，请参见 [ROS 2 安全入门](../../Tutorials/Advanced/Security/Introducing-ros2-security.md)。
