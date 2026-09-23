---
translation_status: machine_translated
source: Concepts/Intermediate/About-Different-Middleware-Vendors.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="different-ros-2-middleware-vendors"></span>

# 不同的 ROS 2 中间件供应商

ROS 2是作为它的中间软件DDS/RTPS的顶部建造的,它提供发现,序列化和运输. [本条](https://design.ros2.org/articles/ros_on_dds.html) 详细解释使用DDS执行和/或DDS的RTPS电线协议背后的动机。简言之,DDS是一个端到端的中间软件,提供与ROS系统相关的特性,如分布式发现(不像ROS 1 那样集中)和对不同的运输“服务质量”选项的控制。

[DDS](https://www.omg.org/omg-dds-portal) 行业标准,由一系列销售商实施,例如RTI的 [Connext DDS](https://www.rti.com/products/),eProsima计划 [Fast DDS](https://fast-dds.docs.eprosima.com/)Eclipse 语录 [Cyclone DDS](https://projects.eclipse.org/projects/iot.cyclonedds),或古鲁姆网络公司 [GurumDDS (古罗马语)](https://gurum.cc/index_eng). RTPS(中文(简体) ). [DDSI - RTPS 软件](https://www.omg.org/spec/DDSI-RTPS/About-DDSI-RTPS/))是DDS用于通过网络通信的线条协议.

ROS 2 支持多个 DDS/RTPS 执行,因为它在选择供应商/执行时不一定“一刀切 ” 。 在选择中间软件执行时,您可能会考虑许多因素:许可证等后勤因素,或平台可用性或计算脚印等技术因素。供应商可以提供不止一个旨在满足不同需求的DDS或RTPS执行。例如,RITI 的Connext 执行有一些不同的目的,比如一个是专门针对微控制器的,另一个是针对需要特殊安全认证的应用程序的(我们此时只支持其标准桌面版本)。

为了与ROS 2一起使用DDS/RTPS的执行,一个“**R**业务办 **M**中间**w**是接口” (a.k.a) `rmw` 接口或只是 `rmw`) 需要创建使用 DDS 或 RTPS 执行的 API 和工具执行抽象 ROS 中间软件接口的软件包。 执行和维护 RMW 软件包以支持 DDS 执行,但支持至少几个执行对于确保 ROS 2 代码库不与任何特定执行捆绑很重要,因为用户可能希望根据项目需要切换执行。

<span id="supported-rmw-implementations"></span>

## 支持落实《保护所有移徙工人及其家庭成员权利国际公约》

| 产品名称 | 许可证 | RMW 执行情况 | 状态 |
|----|----|----|----|
| eProsima 软件 *Fast DDS* | 阿帕奇2型导弹 | `rmw_fastrtps_cpp` | 完全支持 默认的 RMW 已装有二进制版本的套件 。 |
| 剪贴画 *Cyclone DDS* | Eclipse 公共许可证 v2.0 | `rmw_cyclonedds_cpp` | 完全支持,装有二进制版本 |
| RTI 广播电视网 *Connext DDS* | 商业、研究 | `rmw_connextdds` | 完全支持。 支持包含在二进制中, 但Connext 单独安装 。 |
| 古鲁姆网络 *GurumDDS (古罗马语)* | 商业 | `rmw_gurumdds_cpp` | 社区支持 支持包含在二进制中,但GurumDDS单独安装. |

关于与多项《保护移栖物种公约》实施工作合作的实用信息,见《保护移栖物种公约》。 [“与多项《保护移栖物种公约》的实施合作”](../../How-To-Guides/Working-with-multiple-RMW-implementations.md) 教学。

<span id="multiple-rmw-implementations"></span>

## 多项落实《保护所有移徙工人及其家庭成员权利国际公约》

目前活跃的 Distros 的 ROS 2 二进制发布已经内置支持了数个 RMW 执行出框( Fast DDS, RTI Connext Pro, Eclipse Circle DDS, GurumNetworks GurumDDS) 。 默认是 Fast DDS , 因为我们用我们的二进制包来分配它,所以没有额外的安装步骤。

其他RMW,如气旋DDS,Connext或GurumDDS都可以通过 [安装额外软件包](../../Installation/RMW-Implementations.md),但无需重建任何软件包或替换任何现有的软件包。

从源头构建的 ROS 2 工作空间可以同时构建和安装多个 RMW 执行。在编译 ROS 2 核心代码的同时,如果相关的 DDS/RTPS 执行得到妥善安装,并配置了相关的环境变量,则会构建任何 RMW 执行。例如,如果 [RTI Connext DDS 的 RMW 软件包](https://github.com/ros2/rmw_connextdds) 如果也可以找到 RTI 的 Connext Pro 的安装,则会建起来 。

在很多情况下,您会发现使用不同 RMW 执行的节点能够进行通信,但并非在所有情况下都是如此。这里列出了不支持的供应商间通信配置 :

- 快速 DDS \< - \> 连接  
  - `WString` Fast DDS 发布的 Connext 无法在 macOS 上正确接收

- 连接 \<- \> 气旋DDS  
  - 不支持 pub/sub 通信用于 `WString`

<span id="default-rmw-implementation"></span>

## 默认的 RMW 执行

如果 ROS 2 工作空间有多个 RMW 执行, 快速 DDS 会被选为默认的 RMW 执行, 如果无法安装 Fast DDS  RMW 执行, 则将使用首个按字母顺序排列的 RMW 执行标识符 。 执行标识符是提供 RMW 执行的 ROS 包的名称, 例如 。 `rmw_cyclonedds_cpp`。例如,如果两者兼有 `rmw_cyclonedds_cpp` 财务报告和财务报告 `rmw_connextdds` ROS软件包已经安装, `rmw_connextdds` 将会是默认的。如果 `rmw_fastrtps_cpp` 被安装, 这将是默认的。

见 [指南](../../How-To-Guides/Working-with-multiple-RMW-implementations.md) 在运行 ROS 2 实例时,如何指定使用哪些 RMW 执行 。

<span id="cross-vendor-communication"></span> <span id="different-middleware-vendors-cross-vendor-communication"></span>

## 交叉风云通信

虽然在有限的情况下,不同的RMW执行可能兼容,但这一点并不得到保证,因此建议用户确保分布式系统的所有部分使用相同的ROS版本和相同的RMW执行.
