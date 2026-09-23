---
translation_status: machine_translated
source: Tutorials.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="tutorials"></span> <span id="id1"></span>

# 教程

教程是一系列分步指示的集合,目的是在ROS 2中稳步培养技能.

处理教程的最佳方式是第一次走过教程,因为教程是相互积累的,并非是全面的文献。

关于较具体问题的快速解决办法,见A/C.5/49/L.18号文件。 [操作指南](How-To-Guides.md).

- [ROS 入门学习路径](First-Steps.md)
  - [小结](First-Steps.md#summary)
  - [前提条件](First-Steps.md#prerequisites)
  - [步骤](First-Steps.md#steps)
  - [后续步骤](First-Steps.md#next-steps)
- [入门：命令行工具](Tutorials/Beginner-CLI-Tools.md)
  - [配置环境](Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.md)
  - [使用( E) `turtlesim`, `ros2`,以及 `rqt`](Tutorials/Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md)
  - [理解节点](Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md)
  - [理解话题](Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)
  - [理解服务](Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Services/Understanding-ROS2-Services.md)
  - [理解参数](Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md)
  - [理解动作](Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Actions/Understanding-ROS2-Actions.md)
  - [使用( E) `rqt_console` 查看日志](Tutorials/Beginner-CLI-Tools/Using-Rqt-Console/Using-Rqt-Console.md)
  - [启动节点](Tutorials/Beginner-CLI-Tools/Launching-Multiple-Nodes/Launching-Multiple-Nodes.md)
  - [录制与回放数据](Tutorials/Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.md)
- [入门：客户端库](Tutorials/Beginner-Client-Libraries.md)
  - [使用( E) `colcon` 创建软件包](Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.md)
  - [创建工作空间](Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md)
  - [创建软件包](Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md)
  - [编写简单的发布者与订阅者（C++）](Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Cpp-Publisher-And-Subscriber.md)
  - [编写简单的发布者与订阅者（Python）](Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Publisher-And-Subscriber.md)
  - [编写简单的服务端与客户端（C++）](Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Cpp-Service-And-Client.md)
  - [编写简单的服务端与客户端（Python）](Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Service-And-Client.md)
  - [创建自定义 msg 和 srv 文件](Tutorials/Beginner-Client-Libraries/Custom-ROS2-Interfaces.md)
  - [实现自定义接口](Tutorials/Beginner-Client-Libraries/Single-Package-Define-And-Use-Interface.md)
  - [在类中使用参数（C++）](Tutorials/Beginner-Client-Libraries/Using-Parameters-In-A-Class-CPP.md)
  - [在类中使用参数（Python）](Tutorials/Beginner-Client-Libraries/Using-Parameters-In-A-Class-Python.md)
  - [使用( E) `ros2doctor` 确定问题](Tutorials/Beginner-Client-Libraries/Getting-Started-With-Ros2doctor.md)
  - [创建与使用插件（C++）](Tutorials/Beginner-Client-Libraries/Pluginlib.md)
- [中级](Tutorials/Intermediate.md)
  - [使用 rosdep 管理依赖](Tutorials/Intermediate/Rosdep.md)
  - [创建动作](Tutorials/Intermediate/Creating-an-Action.md)
  - [编写动作服务端与客户端（C++）](Tutorials/Intermediate/Writing-an-Action-Server-Client/Cpp.md)
  - [编写动作服务端与客户端（Python）](Tutorials/Intermediate/Writing-an-Action-Server-Client/Py.md)
  - [编写可组合节点（C++）](Tutorials/Intermediate/Writing-a-Composable-Node.md)
  - [在单个进程中组合多个节点](Tutorials/Intermediate/Composition.md)
  - [使用节点接口模板类（C++）](Tutorials/Intermediate/Using-Node-Interfaces-Template-Class.md)
  - [监控参数变化（C++）](Tutorials/Intermediate/Monitoring-For-Parameter-Changes-CPP.md)
  - [启动](Tutorials/Intermediate/Launch/Launch-Main.md)
  - [`tf2`](Tutorials/Intermediate/Tf2/Tf2-Main.md)
  - [测试](Tutorials/Intermediate/Testing/Testing-Main.md)
  - [URDF](Tutorials/Intermediate/URDF/URDF-Main.md)
  - [RViz](Tutorials/Intermediate/RViz/RViz-Main.md)
- [高级](Tutorials/Advanced.md)
  - [补充自定义 rosdep 键](Tutorials/Advanced/Supplementing-Custom-Rosdep-Keys.md)
  - [启用话题统计（C++）](Tutorials/Advanced/Topic-Statistics-Tutorial/Topic-Statistics-Tutorial.md)
  - [使用 Fast DDS Discovery Server 发现协议（社区贡献）](Tutorials/Advanced/Discovery-Server/Discovery-Server.md)
  - [实现自定义内存分配器](Tutorials/Advanced/Allocator-Template-Tutorial.md)
  - [Ament Lint 命令行工具](Tutorials/Advanced/Ament-Lint-For-Clean-Code.md)
  - [发挥 Fast DDS 中间件的能力（社区贡献）](Tutorials/Advanced/FastDDS-Configuration.md)
  - [在节点中录制 bag（C++）](Tutorials/Advanced/Recording-A-Bag-From-Your-Own-Node-CPP.md)
  - [在节点中录制 bag（Python）](Tutorials/Advanced/Recording-A-Bag-From-Your-Own-Node-Py.md)
  - [读取 bag 文件（C++）](Tutorials/Advanced/Reading-From-A-Bag-File-CPP.md)
  - [创建 rqt_bag 插件](Tutorials/Advanced/Create-An-Rqtbag-Plugin.md)
  - [使用 ros2_tracing 追踪与分析应用](Tutorials/Advanced/ROS2-Tracing-Trace-and-Analyze.md)
  - [创建一个 `rmw` 执行](Tutorials/Advanced/Creating-An-RMW-Implementation.md)
  - [仿真器](Tutorials/Advanced/Simulators/Simulation-Main.md)
  - [安全](Tutorials/Advanced/Security/Security-Main.md)
- [演示](Tutorials/Demos.md)
  - [在丢包网络中配置服务质量](Tutorials/Demos/Quality-of-Service.md)
  - [管理节点生命周期：示例](Tutorials/Demos/Managed-Nodes.md)
  - [配置高效的进程内通信](Tutorials/Demos/Intra-Process-Communication.md)
  - [记录和播放回放数据 `rosbag` 使用 ROS 1 桥](Tutorials/Demos/Rosbag-with-ROS1-Bridge.md)
  - [理解实时编程](Tutorials/Demos/Real-Time-Programming.md)
  - [使用虚拟机器人进行实验](Tutorials/Demos/dummy-robot-demo.md)
  - [日志](Tutorials/Demos/Logging-and-logger-configuration.md)
  - [创建内容过滤订阅](Tutorials/Demos/Content-Filtering-Subscription.md)
  - [等待确认](Tutorials/Demos/Wait-for-Acknowledgment.md)
  - [外部资源](Tutorials/Demos.md#external-resources)
- [其他教程](Tutorials/Miscellaneous.md)
  - [部署到 IBM Cloud Kubernetes（社区贡献）](Tutorials/Miscellaneous/Deploying-ROS-2-on-IBM-Cloud.md)
  - [使用 Eclipse 氧气 `rviz2` \[社区贡献\]](Tutorials/Miscellaneous/Eclipse-Oxygen-with-ROS-2-and-rviz2.md)
  - [构建实时 Linux 内核（社区贡献）](Tutorials/Miscellaneous/Building-Realtime-rt_preempt-kernel-for-ROS-2.md)
  - [使用 Eclipse 2021-06 构建软件包](Tutorials/Miscellaneous/Building-ROS2-Package-with-eclipse-2021-06.md)

<span id="examples"></span>

## 实例

- [Python 和 C++ 最小示例](https://github.com/ros2/examples).
