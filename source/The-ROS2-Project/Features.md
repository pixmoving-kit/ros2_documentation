---
translation_status: machine_translated
source: The-ROS2-Project/Features.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="features-status"></span> <span id="features"></span>

# 功能状态

以下的功能可以在当前 ROS 2 发布中获取。 除非另有说明, 特性可以提供给所有支持的平台( Ubuntu 22.04 (Jammy), Windows 10), DDS 执行(eProsima Fast DDS, RTI Connext DDS, 和 Eclipse Circon DDS) 和编程语言客户端库(C++和Python) 。关于计划中的未来开发,请参见 [路线图](Roadmap.md).

| 功能性 | 链接 | 精细打印 |
|----|----|----|
| DS的发现、运输和序列化 | [第 条](https://design.ros2.org/articles/ros_on_dds.html) |  |
| 支助 [多个DDS执行](../Concepts/Intermediate/About-Different-Middleware-Vendors.md),在运行时间选中 | [概念](../Concepts/Intermediate/About-Different-Middleware-Vendors.md), [如何向导](../How-To-Guides/Working-with-multiple-RMW-implementations.md) | 目前,Eclipse气旋DDS,eProsima Fast DDS,以及RTI Connext DDS都得到充分支持. |
| 通用核心客户端库,由特定语言的库包涵 | [细节](../Concepts/Basic/About-Client-Libraries.md) |  |
| 主题的出版/订阅 | [样本代码](https://github.com/ros2/examples), [第 条](https://design.ros2.org/articles/topic_and_service_names.html) |  |
| 客户和服务 | [样本代码](https://github.com/ros2/examples) |  |
| 设置/获取参数 | [样本代码](https://github.com/ros2/demos/tree/0.5.1/demo_nodes_cpp/src/parameters) |  |
| ROS 1 - ROS 2 通信桥 | [教学](https://github.com/ros2/ros1_bridge/blob/master/README.md) | 可用于专题和服务,尚未可供采取行动。 |
| 处理非理想网络的服务质量 | [演示](../Tutorials/Demos/Quality-of-Service.md) |  |
| 使用同一API进行流程间和流程内通信 | [演示](../Tutorials/Demos/Intra-Process-Communication.md) | 目前仅在C++中使用. |
| 编译、链接、加载或运行时间时节点组件的构成 | [演示](../Tutorials/Intermediate/Composition.md) | 目前仅在C++中使用. |
| 同一节点的多个执行器( 调用组级别) | [演示](https://github.com/ros2/examples/tree/rolling/rclcpp/executors/cbg_executor) | 仅在C++中出现. |
| 支持有管理寿命周期的节点 | [演示](../Tutorials/Demos/Managed-Nodes.md) | 目前仅在C++中使用. |
| DDS- 安全支助 | [演示](https://github.com/ros2/sros2) |  |
| 使用可扩展框架的命令行反省工具 | [概念](../Concepts/Basic/About-Command-Line-Tools.md) |  |
| 协调多个节点的发射系统 | [教学](../Tutorials/Intermediate/Launch/Launch-system.md) |  |
| 节点和主题的命名空间支持 | [第 条](https://design.ros2.org/articles/topic_and_service_names.html) |  |
| 静态重新绘制ROS名称 | [如何向导](../How-To-Guides/Node-arguments.md) |  |
| 全ROS 2移动机器人的演示文稿 | [演示](https://github.com/ros2/turtlebot2_demo) |  |
| 对实时代码的初步支持 | [演示](../Tutorials/Demos/Real-Time-Programming.md), [演示](../Tutorials/Advanced/Allocator-Template-Tutorial.md) | 仅限 Linux 。 无法为 Fast RTPS 提供 。 |
| 对“光金属”微控制器的初步支持 | [维基](https://github.com/ros2/freertps/wiki) |  |
| 内容过滤订阅 | [演示](../Tutorials/Demos/Content-Filtering-Subscription.md) | 目前仅在C++中使用. |

除了平台的核心特征外,ROS的最大影响来自其可用的软件包. 以下是几个高知名度的软件包,在最新发行版本中可以提供: 互联网档案馆的存檔,存档日期2013-09-02., 互联网档案馆的存檔,存档日期2013-09-02., 互联网档案馆的存檔,存档日期2014-09-02., 互联网档案馆的存檔,存档日期2014-03-02.

- [gazebo_ros_pkgs](https://index.ros.org/r/gazebo_ros_pkgs/)

- [image_transport](https://index.ros.org/r/image_common)

- [导航2](https://index.ros.org/r/navigation2/)

- [rosbag2](https://index.ros.org/r/rosbag2/)

- [RQt 语录](https://index.ros.org/r/rqt/)

- [RViz2 数据](https://index.ros.org/r/rviz/)
