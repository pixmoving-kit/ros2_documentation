---
translation_status: machine_translated
source: Releases/Beta2-Overview.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="beta-2-r2b2"></span>

# Beta 2 (中文(简体) ).`r2b2`)

<span id="supported-platforms"></span>

## 支持的平台

我们支持三个平台上的ROS 2 Beta 2:Ubuntu 16.04(Xenial)、macOS 10.12(Sierra)和Windows 10. 我们为所有3个平台提供从源代码编译的二进制软件包和指示(见 [安装指令](../Installation.md) (a) 与《公约》有关的其他事项; [文档](https://docs.ros2.org/beta2/)).

<span id="features"></span>

## 特征

<span id="improvements-since-beta-1-release"></span>

### Beta 1 发布以来的改进

- DDS\_ 安全支持( aka SROS2, 参见 [斜线2](https://github.com/ros2/sros2))

- Ubuntu Xenial 的 Debian 软件包

- Typesupport已经重新设计,以便您只构建一个单一的可执行文件,并且可以通过设置环境变量来选择可用的 RMW 执行程序之一(参见 [文档](../How-To-Guides/Working-with-multiple-RMW-implementations.md)).

- 节点和主题的命名空间支持(参见 [设计文章](https://design.ros2.org/articles/topic_and_service_names.html),见下文已知问题)。

- 一组使用可扩展的命令行工具 `ros2` 命令(参见 [概念性条款](../Concepts/Basic/About-Command-Line-Tools.md)).

- C / C++ 中用于记录信件的一组宏(见 API docs of [rcutils 维基月球](https://docs.ros2.org/beta2/api/rcutils/index.html)).

<span id="new-demo-application"></span>

### 新建演示应用程序

- [Turtlebot 2 演示](https://github.com/ros2/turtlebot2_demo) 使用以下已(部分)转换为ROS 2(仅使用Linux)的寄存器:

  - [ros_astra_camera](https://github.com/ros2/ros_astra_camera.git)

  - [depthimage_to_laserscan](https://github.com/ros2/depthimage_to_laserscan.git)

  - [pcl_conversions](https://github.com/ros2/pcl_conversions.git)

  - [制图员](https://github.com/ros2/cartographer.git)

  - [cartographer_ros](https://github.com/ros2/cartographer_ros.git)

  - [弧度解析器](https://github.com/ros2/ceres-solver.git)

  - [导航](https://github.com/ros2/navigation.git)

  - [teleop_twist_keyboard](https://github.com/ros2/teleop_twist_keyboard.git)

  - [joystick_drivers](https://github.com/ros2/joystick_drivers.git)

  - [teleop_twist_joy](https://github.com/ros2/teleop_twist_joy.git)

- [Dummy_robot 演示](../Tutorials/Demos/dummy-robot-demo.md):

  - [robot_model](https://github.com/ros2/robot_model)

  - [robot_state_publisher](https://github.com/ros2/robot_state_publisher)

<span id="selected-features-from-previous-alpha-beta-releases"></span>

### 先前 Alpha/Beta 发布中选取的特性

完整名单见 [较早的发布注释](../index.md).

- C++ 和 Python 执行 ROS 2 客户端库,包括 API 用于:

  - 出版和签署ROS专题

  - 请求和答复ROS服务(仅同步(C++)和同步)

  - 获取和设置ROS参数(仅C++,同步和同步)

  - 计时器回调

- 支持多个DDS/RTPS执行之间的互操作性

  - eProsima Fast RTPS 是默认执行, 并包含在二进制包中

  - 支持 RTI Connext : 从源创建以尝试它

  - 我们最初支持PrismTech OpenSplice,但目前对此的支持被搁置

- 用于网络活动的图表 API

- 分布式发现

- 在兼容的 DDS 执行下发布和订阅的实时安全代码路径( 目前只有 Connext )

  - 支持自定义分配器

- ROS 1 \< \> ROS 2动态桥节点

- 执行器线程模式( 仅C++)

- 在编译 / 链接 / 运行时间 时组成节点的组件模型

- 使用标准生命周期管理的组件

- 扩展 `.msg` 具有新特性的格式 :

  - 边界阵列

  - 默认值

<span id="known-issues"></span>

### 已知问题

- 我们追踪各种寄存器的问题, 但主要的切入点是 [ros2/ros2 问题跟踪器](https://github.com/ros2/ros2/issues)

- 我们想强调一个 [已知问题](https://github.com/ros2/rmw_connext/issues/234) 我们所研究的话题不允许两个具有相同基名但不同命名空间的话题在使用时具有不同类型 `rmw_connext_cpp`.

- 具有长响应的服务不是与 Fast-RTPS 合作。 修复虽然不是 beta2 的一部分, 但它是上游可用的, 因此您可以通过使用 Fast-RTPS 主分支从源头构建来围绕这个问题工作 。
