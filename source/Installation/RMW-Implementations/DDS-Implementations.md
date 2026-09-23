---
translation_status: machine_translated
source: Installation/RMW-Implementations/DDS-Implementations.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="dds-implementations"></span>

# DDS 实现

这些是可用的DDS执行:

- [与 Eclipse 气旋 DDS 合作](DDS-Implementations/Working-with-Eclipse-CycloneDDS.md) 解释如何利用旋风DDS。

- [与 eProsima 快速 DDS 合作](DDS-Implementations/Working-with-eProsima-Fast-DDS.md) 解释如何使用快速DDS.

- [与 RTI Connext DDS 合作](DDS-Implementations/Working-with-RTI-Connext-DDS.md) 解释如何使用 RTI Connext DDS 。

- [与 GurumNetworks GurumDDS 合作](DDS-Implementations/Working-with-GurumNetworks-GurumDDS.md) 解释如何使用GurumDDS.

如果您想使用其他的供应商之一, 您需要在建置前分别安装他们的软件。 ROS 2 架构将自动为安装和源码正确的供应商构建支持 。

一旦您安装了一个新的 RMW 供应商, 您可以更改运行时使用的供应商 : [与多个RMW执行机构合作](../../How-To-Guides/Working-with-multiple-RMW-implementations.md).
