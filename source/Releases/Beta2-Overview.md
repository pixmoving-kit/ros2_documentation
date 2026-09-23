<span id="beta-2-r2b2"></span>

# Beta 2（`r2b2`）

<span id="supported-platforms"></span>

## 支持的平台

ROS 2 Beta 2 支持三类平台：Ubuntu 16.04（Xenial）、macOS 10.12（Sierra）和 Windows 10。三类平台都提供二进制包和源码编译说明。参见[安装说明](../Installation.md)和 [Beta 2 文档](https://docs.ros2.org/beta2/)。

<span id="features"></span>

## 功能



<span id="improvements-since-beta-1-release"></span>

### 相较 Beta 1 的改进

- 支持 DDS_Security，即 SROS2，参见 [sros2](https://github.com/ros2/sros2)。
- 为 Ubuntu Xenial 提供 Debian 软件包。
- 重新设计类型支持：只需构建一个可执行程序，即可通过环境变量选择可用的 RMW 实现，参见[文档](../How-To-Guides/Working-with-multiple-RMW-implementations.md)。
- 支持节点和话题的命名空间，参见[设计文章](https://design.ros2.org/articles/topic_and_service_names.html)及下文已知问题。
- 提供基于可扩展 `ros2` 命令的命令行工具，参见[概念说明](../Concepts/Basic/About-Command-Line-Tools.md)。
- 提供 C/C++ 日志宏，参见 [rcutils API 文档](https://docs.ros2.org/beta2/api/rcutils/index.html)。

<span id="new-demo-application"></span>

### 新的演示应用

[Turtlebot 2 演示](https://github.com/ros2/turtlebot2_demo)使用以下已全部或部分迁移至 ROS 2 的仓库，仅支持 Linux：

- [ros_astra_camera](https://github.com/ros2/ros_astra_camera.git)
- [depthimage_to_laserscan](https://github.com/ros2/depthimage_to_laserscan.git)
- [pcl_conversions](https://github.com/ros2/pcl_conversions.git)
- [cartographer](https://github.com/ros2/cartographer.git)
- [cartographer_ros](https://github.com/ros2/cartographer_ros.git)
- [ceres-solver](https://github.com/ros2/ceres-solver.git)
- [navigation](https://github.com/ros2/navigation.git)
- [teleop_twist_keyboard](https://github.com/ros2/teleop_twist_keyboard.git)
- [joystick_drivers](https://github.com/ros2/joystick_drivers.git)
- [teleop_twist_joy](https://github.com/ros2/teleop_twist_joy.git)

[Dummy_robot 演示](../Tutorials/Demos/dummy-robot-demo.md)使用 [robot_model](https://github.com/ros2/robot_model) 和 [robot_state_publisher](https://github.com/ros2/robot_state_publisher)。

<span id="selected-features-from-previous-alpha-beta-releases"></span>

### 之前 Alpha/Beta 版本的部分功能

完整列表见[早期发行说明](../Releases.md)。

- C++ 和 Python 客户端库提供以下 API：发布和订阅 ROS 话题；请求和响应 ROS 服务（同步方式仅支持 C++，也支持异步方式）；获取和设置 ROS 参数（仅支持 C++，包括同步和异步方式）；定时器回调。
- 支持多个 DDS/RTPS 实现之间的互操作。eProsima Fast RTPS 是默认实现，包含在二进制包中；支持 RTI Connext，可从源码构建以试用。最初支持 PrismTech OpenSplice，但目前暂停支持。
- 面向网络事件的计算图 API。
- 分布式发现。
- 使用兼容 DDS 实现时，发布和订阅具有实时安全的代码路径，目前仅适用于 Connext；支持自定义分配器。
- ROS 1 ↔ ROS 2 动态桥接节点。
- 执行器线程模型，仅支持 C++。
- 支持在编译、链接或运行时组合节点的组件模型。
- 使用标准生命周期的受管理组件。
- 扩展 `.msg` 格式，支持有界数组和默认值。

<span id="known-issues"></span>

### 已知问题

- 问题分散在各仓库跟踪，主要入口是 [ros2/ros2 问题跟踪器](https://github.com/ros2/ros2/issues)。
- 正在调查一个[已知问题](https://github.com/ros2/rmw_connext/issues/234)：使用 `rmw_connext_cpp` 时，基础名称相同但命名空间不同的两个话题不能使用不同类型。
- Fast-RTPS 中响应较长的服务无法正常工作。修复已进入上游，但未包含在 Beta 2 中；可使用 Fast-RTPS 的 master 分支从源码构建来规避。
