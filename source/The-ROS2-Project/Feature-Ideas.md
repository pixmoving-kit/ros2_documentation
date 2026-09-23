<span id="feature-ideas"></span><span id="featureideas"></span>

# 功能建议

以下建议不分先后。这些功能被认为很重要，也适合作为对 ROS 2 的贡献。开始开发新功能之前，请先[联系我们](../Contact.md)，我们可以提供指导，并帮助你联系其他开发者。

<span id="design-concept"></span>

## 设计与概念

- IDL 格式：利用将常量归入枚举等新特性；扩展对仅包含常量的 `.idl` 文件以及带取值范围的参数声明的支持；重新审视 IDL 接口命名限制，参见 [ros2/design#220](https://github.com/ros2/design/pull/220)。
- 制定 ROS 1 → ROS 2 迁移计划。
- 节点名称的唯一性，参见 [ros2/design#187](https://github.com/ros2/design/issues/187)。
- 使用描述性格式定义节点的话题、服务等具体“API”，参见 [ros2/design#266](https://github.com/ros2/design/pull/266)。

<span id="infrastructure-and-tools"></span>

## 基础设施与工具

### 构建

- 整合 <https://build.ros2.org> 和 <https://ci.ros2.org>。
- 配置 macOS 构建环境。
- 提供 Windows 和 macOS 软件包。
- 在 `colcon` 中支持配置档案（profile）。

### 文档

- 停用 <https://design.ros2.org>，将内容迁移至 REP 或 <https://github.com/ros2/ros2_documentation>，或予以移除。
- 修复软件包文档构建器，使其能够为消息、服务、动作等构建产物生成文档。
- 当 <https://github.com/ros2/ros2_documentation> 发生变化时，自动重新构建 <https://docs.ros.org/en/ros2_documentation>。
- 编写 `ament` 文档。
- 增加在 Jupyter notebook 中使用 ROS 2 的文档示例。
- 增加实现新 RMW 的文档。
- 提供三类不同内容：演示（demos）用于展示功能并用测试覆盖它们；示例（examples）用于展示简单或最小用法，同一任务可能有多种实现；教程（tutorials）提供更多注释和供 Wiki 引用的锚点，教授一种推荐做法。

<span id="new-features"></span>

## 新功能

星号表示大致工作量：一颗星为小，两颗星为中，三颗星为大。

### 日志改进（★ / ★★）

- 通过文件指定配置。
- 按日志记录器分别配置，例如支持 `rqt_logger_level`。

### 时间相关功能

- 支持基于时钟的频率控制和休眠。

### 更多计算图 API 功能（★★ / ★★★）

- 内省所有话题（尤其是远端话题）的 QoS 设置。
- 提供类似 [ROS 1 Master API](https://wiki.ros.org/ROS/Master_API) 的功能。
- 基于事件的通知。
- 需要了解并扩展 rmw 接口。

### 执行器

- 改进性能，重点是等待集。
- 确定性的执行顺序，即公平调度。
- 解耦 waitable 对象。

### 消息生成

- 为尚未开箱支持的语言补充消息生成支持。
- 对消息字段名进行改写，避免与特定语言的关键字冲突。
- 在同一个 Python 解释器中运行生成器以改善性能。

### 启动系统

- 支持启动包含多个节点的可执行程序，即手动组合。
- 扩展 launch 的 XML/YAML 支持，包括事件、事件处理器、标签命名空间和别名。

### Rosbag

- 支持录制服务和动作。

### ros1_bridge

- 支持桥接动作。

### RMW 配置

- 以统一的标准方式配置中间件。

### 重映射（★★ / ★★★）

- 通过服务接口实现动态重映射和别名。

### 类型伪装（★★★）

- 提供类似 [ROS 1 消息特征（message traits）](https://wiki.ros.org/roscpp/Overview/MessagesSerializationAndAdaptingTypes)的机制。
- 需要了解类型支持系统。

### 扩展实时安全性（★★★）

- 覆盖服务、客户端和参数。
- 暴露更多与实时性能有关的服务质量参数。
- 提供实时安全的进程内消息传递。

### 多机器人支持功能及演示（★★★）

- 所有机器人的全部节点共享同一个域并互相发现，并不理想。
- 设计如何对系统进行分区。

### 支持更多 DDS / RTPS 实现

- RTI Connext DDS Micro：已经实现，但默认未启用，也未获得官方支持。

### 安全性改进

- 提供更细粒度的安全配置，例如仅认证、认证加加密等。（★）
- 集成 DDS-Security 日志插件，以统一方式汇总安全事件，并通过 ROS 接口向用户报告。（★★）
- 提高密钥存储安全性，目前密钥只是保存在文件系统中。（★★）
- 提供更友好的界面，使安全配置更容易指定；例如 Qt 图形界面，也可协助分发密钥。（★★★）
- 提供界面，让用户为当前运行的系统启用安全机制，自动为运行中的全部实体生成密钥和策略。（★★★）
- 如果硬件提供密钥保护或加速消息加密、签名的功能，可以考虑将其加入尚未使用这些功能的 DDS/RTPS 实现。（★★★）

<span id="reducing-technical-debt"></span>

## 减少技术债务

- 修复 <https://ci.ros2.org/view/nightly> 中不稳定的测试。
- 支持使用 valgrind、clang-tidy、Clang 静态分析（scan-build）、ASAN、TSAN、UBSAN 等工具运行全部单元测试。
- 审查 API，尤其是 rclcpp 和 rclpy 面向用户的 API。
- 将 rclcpp API 重构为各自专注单一方面的软件包，同时仍由 rclcpp 提供完整的用户层 API。
- 重新审视消息分配器，考虑使用 `std::polymorphic_allocator` 解决问题。
- 使[设计文档](https://design.ros2.org)与实现保持同步、一致。
- 处理或分类待解决的 issue。
- 处理代码和文档中的 TODO。
- 移除对 tinyxml 的依赖。
