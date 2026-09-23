---
translation_status: machine_translated
source: The-ROS2-Project/Feature-Ideas.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="feature-ideas"></span> <span id="featureideas"></span>

# 功能建议

以下是没有具体顺序的特征构想,该列表包含我们认为重要的特征,可以为ROS 2做出良好贡献. [请联系我们](../Contact.md) 在挖掘新功能之前,我们可以提供指南,并与其他开发者连接。

<span id="design-concept"></span>

## 设计/概念

- IDL 格式

  - 利用新特性, 如将常数组合成元件

  - 扩展使用范围至 `.idl` 仅包含常数和/或有区域声明参数的文件

  - 重访IDL 界面命名的限制,参见 [ros2/design#220](https://github.com/ros2/design/pull/220)

- 为 ROS 1 - \> ROS 2 过渡创建迁移计划

- 节点名称的独特性,参见 [ros2/design#187](https://github.com/ros2/design/issues/187)

- 以描述性格式说明某一节点的主题/服务/等的具体“API”,见 [ros2/design#266](https://github.com/ros2/design/pull/266)

<span id="infrastructure-and-tools"></span>

## 基础设施和工具

- 大楼

  - 合并 <https://build.ros2.org> 财务报告和财务报告 <https://ci.ros2.org>

  - 提供 macOS

  - 窗口和macOS 软件包

  - 支持配置图 `colcon`

- 文档

  - 折旧 <https://design.ros2.org>内容应移动到环境方案,以便 <https://github.com/ros2/ros2_documentation>,或被删除。

  - 整治每件文件的建设者,能够记录建设文物,即信息,服务,行动等.

  - 制作( M) <https://docs.ros.org/en/ros2_documentation> 更改后自动重建到 <https://github.com/ros2/ros2_documentation>.

  - `ament` 文档

  - 添加使用 ROS 2 的文档示例,并配有 Jupyter 笔记本.

  - 增加执行新的《保护所有移徙工人及其家庭成员权利国际公约》的文件。

  - 提供三种不同的内容:

    - 显示特征并用测试遮盖的“演示”

    - “实例”以显示一种简单/最低限度的用途,这种用途可能有多种方法来做一些事情

    - 包含更多评论和维基语录的“教程”(教学推荐的方法)

<span id="new-features"></span>

## 新特性

后方的恒星表示粗糙的功率:1星为小星,2星为中星,3星为大星.

- 伐木改进 \[\* / \*\*\]

  - 文件指定的配置

  - 偶机配置(例如辅助配置). `rqt_logger_level`)

- 时间关系

  - 基于时钟的支持率和睡眠率

- 附加图 API 特性 \[\*\* / \*\*\*\]

  - 面向所有(特别是远程)主题的 QoS 设置

  - a la ROS 1 主 API: <https://wiki.ros.org/ROS/Master_API>

  - 基于事件的通知

  - 需要了解需要扩展的 Rmw 接口

- 执行器

  - 改进性能(主要是在等待器周围)

  - 定时命令( 公平排程)

  - 二进制等待器

- 信件生成

  - 不支持外框语言的快取信件生成

  - 信件中的字段名称以避开语言特定关键字

  - 通过在同一 Python 解释器中运行来提高生成器的性能

- 启动

  - 支持启动多节点可执行文件(即手工构成)

  - 扩展启动 XML/ YAML 支持:事件和事件处理器,标签命名空间和别名

- 罗斯巴格

  - 支助记录服务(和行动)

- ros1_bridge

  - 支持过渡行动

- RMW 配置

  - 配置中间软件的统一标准方式

- 重新绘图 \[\*\* / \*\*\*\]

  - 通过服务接口进行动态重映和化名

- 装模作样 \[\*\*\*\]

  - 一个ROS 1的信息特征: <https://wiki.ros.org/roscpp/Overview/MessagesSerializationAndAdaptingTypes>

  - 需要了解类型支持系统

- 实时安全扩展 \[\*\*\*\]

  - 服务、客户和参数

  - 展示与实时性能有关的服务质量参数

  - 实时安全流程内信息

- 多机器人支持功能和演示 \[\*\*\*\]

  - 不希望所有机器人上的所有节点都共享同一个域(并互相发现)

  - 设计如何将系统“分割”

- 支持更多的DDS/RTPS执行:

  - RTI Connext DDS Micro(已执行,默认无法启用或正式支持).

- 安全改进:

  - 安全配置的颗粒性更强(仅允许认证、认证和加密等) \[\*\]

  - 整合 DDS- Security 日志插件(将安全事件汇总并通过ROS 接口向用户报告的统一方式) \[\*\*\]

  - 密钥存储安全(现在,密钥只是存储在文件系统中) \[\*\*\]

  - 更方便用户的界面( 使指定安全配置更加容易) 。 也许是 Qt 图形用户界面 ? 此图形用户界面也可以帮助以某种方式分配密钥 \[\*\*\*\]

  - 一种“请确保这个运行系统的安全”的表达方式,使用一些可以自动生成当前运行的所有密钥和政策的用户界面\[\*\*\*\]

  - 如果有硬件特定功能来保护密钥或加速加密/签名消息,那么在DDS/RTPS执行中添加已经不用的功能可能很有趣 \[\*\*\*\]

<span id="reducing-technical-debt"></span>

## 减少技术债务

- 修复片状试验 <https://ci.ros2.org/view/nightly>.

- 能够使用工具进行(所有)单元测试,例如:Valgrind、cang-tidy、cang静态分析(scan-building)、ASAN、TSAN、UBSAN等。

- API 评论,具体为 Rclcpp 和 rclpy 的用户化 API

- 将 rclcpp API 重置为侧重于单个方面的单独软件包, rclcpp 之后仍应提供合并的 user-facing API

- 重访信件分配器, 考虑使用 std:: polymorphic\_ 分配器来解决问题

- 同步/ 对齐 [设计文件](https://design.ros2.org) 与执行有关。

- 地址/待售票分类

- 代码/文件中的地址待办事宜

- 删除小xml作为依赖
