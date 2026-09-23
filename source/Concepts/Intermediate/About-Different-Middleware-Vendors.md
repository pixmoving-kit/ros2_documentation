<span id="different-ros-2-middleware-vendors"></span>
# 不同的 ROS 2 中间件供应商

ROS 2 使用 DDS/RTPS 作为底层中间件，由其提供发现、序列化和传输功能。[这篇文章](https://design.ros2.org/articles/ros_on_dds.html)详细说明了采用 DDS 实现和／或 DDS 的 RTPS 网络传输协议的原因。概括而言，DDS 是一种端到端中间件，提供了与 ROS 系统相关的功能，例如分布式发现（与 ROS 1 的集中式发现不同），以及对传输中各种“服务质量”选项的控制。

[DDS](https://www.omg.org/omg-dds-portal) 是一项行业标准，有多家供应商提供实现，例如 RTI 的 [Connext DDS](https://www.rti.com/products/)、eProsima 的 [Fast DDS](https://fast-dds.docs.eprosima.com/)、Eclipse 的 [Cyclone DDS](https://projects.eclipse.org/projects/iot.cyclonedds)，以及 GurumNetworks 的 [GurumDDS](https://gurum.cc/index_eng)。RTPS（也称为 [DDSI-RTPS](https://www.omg.org/spec/DDSI-RTPS/About-DDSI-RTPS/)）是 DDS 在网络上通信时使用的传输协议。

ROS 2 支持多种 DDS/RTPS 实现，因为没有一种供应商或实现必然适合所有场景。选择中间件实现时，需要考虑许多因素，例如许可证等实际约束，以及平台支持、计算资源占用等技术因素。供应商可能提供多个 DDS 或 RTPS 实现，以满足不同需求。例如，RTI 的 Connext 实现有多个面向不同用途的版本，包括专门面向微控制器的版本，以及面向需要特殊安全认证的应用的版本；目前我们只支持其标准桌面版本。

要在 ROS 2 中使用一种 DDS/RTPS 实现，需要创建一个“ROS 中间件接口”（**R**OS **M**iddle**w**are interface，简称 `rmw` 接口或 `rmw`）包，使用该 DDS 或 RTPS 实现的 API 和工具来实现抽象的 ROS 中间件接口。实现和维护支持各种 DDS 实现的 RMW 包需要大量工作。不过，至少支持几种实现非常重要，这可以确保 ROS 2 代码库不会绑定到某一种特定实现，用户也能根据项目需求切换实现。

<span id="supported-rmw-implementations"></span>
## 支持的 RMW 实现

| 产品名称 | 许可证 | RMW 实现 | 状态 |
| --- | --- | --- | --- |
| eProsima *Fast DDS* | Apache 2 | `rmw_fastrtps_cpp` | 完整支持；默认 RMW；随二进制发行包提供。 |
| Eclipse *Cyclone DDS* | Eclipse Public License v2.0 | `rmw_cyclonedds_cpp` | 完整支持；随二进制发行包提供。 |
| RTI *Connext DDS* | 商业、研究许可证 | `rmw_connextdds` | 完整支持；二进制包包含对它的支持，但 Connext 需单独安装。 |
| GurumNetworks *GurumDDS* | 商业许可证 | `rmw_gurumdds_cpp` | 社区支持；二进制包包含对它的支持，但 GurumDDS 需单独安装。 |

有关使用多种 RMW 实现的实践说明，请参见[使用多种 RMW 实现](../../How-To-Guides/Working-with-multiple-RMW-implementations.md)教程。

<span id="multiple-rmw-implementations"></span>
## 多种 RMW 实现

目前仍受支持的 ROS 2 发行版，其二进制发行包内置了对多种 RMW 实现的支持，包括 Fast DDS、RTI Connext Pro、Eclipse Cyclone DDS 和 GurumNetworks GurumDDS。默认实现为 Fast DDS；它随二进制包一同分发，因此无需额外安装即可使用。

其他 RMW 实现，例如 Cyclone DDS、Connext 或 GurumDDS，可以通过[安装附加软件包](../../Installation/RMW-Implementations.md)来启用，无需重新构建任何内容，也无需替换已有软件包。

从源码构建的 ROS 2 工作空间可以同时构建并安装多种 RMW 实现。编译 ROS 2 核心代码时，只要相应 DDS/RTPS 实现已正确安装，且相关环境变量已配置好，找到的 RMW 实现就会参与构建。例如，如果工作空间中有 [RTI Connext DDS 的 RMW 包](https://github.com/ros2/rmw_connextdds)源码，并且能找到已安装的 RTI Connext Pro，就会构建这个包。

在许多情况下，使用不同 RMW 实现的节点可以互相通信，但并非所有情况都如此。以下跨供应商通信配置不受支持：

- Fast DDS ↔ Connext：在 macOS 上，Connext 无法正确接收 Fast DDS 发布的 `WString`。
- Connext ↔ Cyclone DDS：不支持 `WString` 的发布／订阅通信。

<span id="default-rmw-implementation"></span>
## 默认 RMW 实现

如果 ROS 2 工作空间中存在多种 RMW 实现，只要 Fast DDS 可用，就会选用它作为默认实现。如果没有安装 Fast DDS 的 RMW 实现，则按 RMW 实现标识符的字母顺序选择排在最前面的实现。实现标识符就是提供该 RMW 实现的 ROS 包名，例如 `rmw_cyclonedds_cpp`。

例如，同时安装 `rmw_cyclonedds_cpp` 和 `rmw_connextdds` 时，默认实现为 `rmw_connextdds`。如果安装了 `rmw_fastrtps_cpp`，则它会成为默认实现。

有关运行 ROS 2 示例时如何指定 RMW 实现，请参见[使用指南](../../How-To-Guides/Working-with-multiple-RMW-implementations.md)。

<span id="cross-vendor-communication"></span> <span id="different-middleware-vendors-cross-vendor-communication"></span>
## 跨供应商通信

不同的 RMW 实现在某些情况下可能兼容，但并无保证。因此，建议确保分布式系统的所有部分使用相同的 ROS 版本和相同的 RMW 实现。
