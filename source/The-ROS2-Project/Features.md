<span id="features-status"></span><span id="features"></span>

# 功能状态

以下功能可在当前 ROS 2 发行版中使用。除非另有说明，它们适用于所有受支持的平台（Ubuntu 22.04 Jammy、Windows 10）、DDS 实现（eProsima Fast DDS、RTI Connext DDS、Eclipse Cyclone DDS）以及客户端库语言（C++、Python）。未来开发计划参见[路线图](Roadmap.md)。

| 功能 | 参考链接 | 补充说明 |
| --- | --- | --- |
| 基于 DDS 的发现、传输和序列化 | [文章](https://design.ros2.org/articles/ros_on_dds.html) | |
| 支持在运行时选择[多种 DDS 实现](../Concepts/Intermediate/About-Different-Middleware-Vendors.md) | [概念](../Concepts/Intermediate/About-Different-Middleware-Vendors.md)、[操作指南](../How-To-Guides/Working-with-multiple-RMW-implementations.md) | 目前完整支持 Eclipse Cyclone DDS、eProsima Fast DDS 和 RTI Connext DDS。 |
| 各语言客户端库共同封装的核心客户端库 | [详情](../Concepts/Basic/About-Client-Libraries.md) | |
| 通过话题发布和订阅 | [示例代码](https://github.com/ros2/examples)、[文章](https://design.ros2.org/articles/topic_and_service_names.html) | |
| 客户端与服务 | [示例代码](https://github.com/ros2/examples) | |
| 设置和获取参数 | [示例代码](https://github.com/ros2/demos/tree/0.5.1/demo_nodes_cpp/src/parameters) | |
| ROS 1 与 ROS 2 通信桥 | [教程](https://github.com/ros2/ros1_bridge/blob/master/README.md) | 支持话题和服务，尚不支持动作。 |
| 应对非理想网络的服务质量设置 | [演示](../Tutorials/Demos/Quality-of-Service.md) | |
| 使用同一套 API 进行进程间和进程内通信 | [演示](../Tutorials/Demos/Intra-Process-Communication.md) | 目前仅支持 C++。 |
| 在编译、链接、加载或运行时组合节点组件 | [演示](../Tutorials/Intermediate/Composition.md) | 目前仅支持 C++。 |
| 在同一节点内按回调组使用多个执行器 | [演示](https://github.com/ros2/examples/tree/rolling/rclcpp/executors/cbg_executor) | 仅支持 C++。 |
| 支持生命周期受管理的节点 | [演示](../Tutorials/Demos/Managed-Nodes.md) | 目前仅支持 C++。 |
| DDS-Security 支持 | [演示](https://github.com/ros2/sros2) | |
| 基于可扩展框架的命令行内省工具 | [概念](../Concepts/Basic/About-Command-Line-Tools.md) | |
| 协调多个节点的启动系统 | [教程](../Tutorials/Intermediate/Launch/Launch-system.md) | |
| 节点和话题的命名空间支持 | [文章](https://design.ros2.org/articles/topic_and_service_names.html) | |
| ROS 名称的静态重映射 | [操作指南](../How-To-Guides/Node-arguments.md) | |
| 完全基于 ROS 2 的移动机器人演示 | [演示](https://github.com/ros2/turtlebot2_demo) | |
| 对实时代码的初步支持 | [演示](../Tutorials/Demos/Real-Time-Programming.md)、[分配器演示](../Tutorials/Advanced/Allocator-Template-Tutorial.md) | 仅支持 Linux，不支持 Fast RTPS。 |
| 对裸机微控制器的初步支持 | [Wiki](https://github.com/ros2/freertps/wiki) | |
| 内容过滤订阅 | [演示](../Tutorials/Demos/Content-Filtering-Subscription.md) | 目前仅支持 C++。 |

除了平台核心功能之外，ROS 的最大影响力还来自丰富的软件包。最新发行版中一些广受关注的软件包包括：

- [gazebo_ros_pkgs](https://index.ros.org/r/gazebo_ros_pkgs/)
- [image_transport](https://index.ros.org/r/image_common)
- [navigation2](https://index.ros.org/r/navigation2/)
- [rosbag2](https://index.ros.org/r/rosbag2/)
- [RQt](https://index.ros.org/r/rqt/)
- [RViz2](https://index.ros.org/r/rviz/)
