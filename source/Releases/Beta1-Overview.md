---
translation_status: machine_translated
source: Releases/Beta1-Overview.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="beta-1-asphalt"></span>

# Beta 1 (中文(简体) ).`Asphalt`)

<span id="supported-platforms"></span>

## 支持的平台

我们支持三个平台上的ROS 2 Beta 1:Ubuntu 16.04(Xenial),Mac OS X 10.11(El Capitan),以及Windows 8.1和10. 我们同时提供二进制软件包和如何从源头为所有3个平台编译的指令.

<span id="features"></span>

## 特征

<span id="improvements-since-alpha-8-release"></span>

### 自阿尔法8发布以来的改进

- 在编译、链接或运行时支持节点组成 。

- 管理节点的标准寿命周期 。

- 加强对服务质量调试和测试的支持.

- [新的和更新的设计文件](https://design.ros2.org/)

- 更多( E) [教程](../Tutorials.md) 财务报告和财务报告 [实例](https://github.com/ros2/examples)

- 将服务与/从1号业务标准连接起来(除专题外)

<span id="selected-features-from-previous-alpha-releases"></span>

### 先前 Alpha 发布中选取的特性

完整名单见 [较早的发布注释](../index.md).

- C++ 和 Python 执行 ROS 2 客户端库,包括 API 用于:

  - 出版和签署ROS专题

  - 请求和答复ROS服务(仅同步(C++)和同步)

  - 获取和设置ROS参数(仅C++,同步和同步)

  - 计时器回调

  - 支持多个DDS/RTPS执行之间的互操作性

  - eProsima Fast RTPS 是默认执行, 并包含在二进制包中

  - 支持 RTI Connext : 从源创建以尝试它

  - 我们起初支持PrismTech OpenSplice 但最终决定放弃

- 用于网络活动的图表 API

- 分布式发现

- 在兼容的 DDS 执行下发布和订阅的实时安全代码路径( 目前只有 Connext )

  - 支持自定义分配器

- ROS 1 \< \> ROS 2动态桥节点

- C++ 中的执行器线程模型

- 扩展 `.msg` 具有新特性的格式 :

  - 边界阵列

  - 默认值

<span id="known-issues"></span>

### 已知问题

- 我们追踪各种寄存器的问题, 但主要的切入点是 [ros2/ros2 问题跟踪器](https://github.com/ros2/ros2/issues)

- 我们想强调一个 [已知问题](https://github.com/ros2/rmw_fastrtps/issues/81) 我们正与 eProsima 合作,在 FastRTPS 下修复大型信件的显著性能。在运行一些图像分辨率较大的演示时,将观察到这一点。
