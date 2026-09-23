<span id="beta-1-asphalt"></span>

# Beta 1（`Asphalt`）

<span id="supported-platforms"></span>

## 支持的平台

ROS 2 Beta 1 支持三类平台：Ubuntu 16.04（Xenial）、Mac OS X 10.11（El Capitan），以及 Windows 8.1 和 10。三类平台都提供二进制包和源码编译说明。

<span id="features"></span>

## 功能



<span id="improvements-since-alpha-8-release"></span>

### 相较 Alpha 8 的改进

- 支持在编译、链接或运行时组合节点。
- 为受管理节点提供标准生命周期。
- 改进服务质量（QoS）调优和测试支持。
- [新增和更新的设计文档](https://design.ros2.org/)。
- 更多[教程](../Tutorials.md)和[示例](https://github.com/ros2/examples)。
- 除话题外，也支持与 ROS 1 双向桥接服务。

<span id="selected-features-from-previous-alpha-releases"></span>

### 之前 Alpha 版本的部分功能

完整列表见[早期发行说明](../Releases.md)。

- C++ 和 Python 客户端库提供以下 API：发布和订阅 ROS 话题；请求和响应 ROS 服务（同步方式仅支持 C++，也支持异步方式）；获取和设置 ROS 参数（仅支持 C++，包括同步和异步方式）；定时器回调。
- 支持多个 DDS/RTPS 实现之间的互操作。eProsima Fast RTPS 是默认实现，包含在二进制包中；支持 RTI Connext，可从源码构建以试用。最初支持 PrismTech OpenSplice，但后来决定移除其支持。
- 面向网络事件的计算图 API。
- 分布式发现。
- 使用兼容 DDS 实现时，发布和订阅具有实时安全的代码路径，目前仅适用于 Connext；支持自定义分配器。
- ROS 1 ↔ ROS 2 动态桥接节点。
- C++ 执行器线程模型。

- 扩展 `.msg` 格式，支持有界数组和默认值。

<span id="known-issues"></span>

### 已知问题

- 问题分散在各仓库跟踪，主要入口是 [ros2/ros2 问题跟踪器](https://github.com/ros2/ros2/issues)。
- 正在与 eProsima 合作解决一个[已知问题](https://github.com/ros2/rmw_fastrtps/issues/81)：FastRTPS 处理大型消息时性能明显下降，在使用较高图像分辨率的演示中可以观察到。
