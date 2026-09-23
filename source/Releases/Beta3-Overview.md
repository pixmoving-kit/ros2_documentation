---
translation_status: machine_translated
source: Releases/Beta3-Overview.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="beta-3-r2b3"></span>

# Beta 3 (英语).`r2b3`)

<span id="supported-platforms"></span>

## 支持的平台

我们支持三个平台上的ROS 2 Beta 3:Ubuntu 16.04(Xenial)、macOS 10.12(Sierra)和Windows 10. 我们为所有3个平台提供二进制软件包和如何从源代码编译的指示(见 [安装指令](../Installation.md) (a) 与《公约》有关的其他事项; [文档](https://docs.ros2.org/beta3/)).

<span id="features"></span>

## 特征

<span id="improvements-since-beta-2-release"></span>

### Beta 2 发布以来的改进

- Python 的执行模式, Python C 扩展中的许多内存管理修正

- 实验重写 [ros_control](https://github.com/ros2/ros2_control)

- 向用户(快速RTPS和Connext)披露DDS执行特定符号(见 [实例](https://github.com/ros2/demos/blob/6363be2efe2fea799d92bc22a66e776b2ca9c5d0/demo_nodes_cpp_native/src/talker.cpp))

- 日志 [API](https://github.com/ros2/rclpy/blob/1ef2924ef8e154c0553edf0fdba4840b08b728f8/rclpy/rclpy/logging.py) 在 Python 语句中

- 在各种软件包中固定了数个内存泄漏和比赛条件

- 对 PrismTech 提供的 OpenSplices( 在 Linux 和 Windows atm 上) 添加支持

- 使用开花( 没有补丁) 使 ROS 2 发布

<span id="new-demo-application"></span>

### 新建演示应用程序

- [HSR 演示](https://github.com/ruffsl/hsr_demo)

  - 遥控 HSR 机器人,使用 ROS 2 乐杆控制器

  - 运行 `ros1_bridge` 在HSR上的Docker容器中(因为机器人在Ubuntu Trusty上运行ROS 1)

  - 运行 ROS 2 开发版本 [rviz 维兹](https://github.com/ros2/rviz) 将来自机器人等的传感器数据可视化(见 [视频](https://vimeo.com/237016358))

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

  - PrismTech OpenSplice: 参见以下限制

- 用于网络活动的图表 API

- 分布式发现

- 在兼容的 DDS 执行下发布和订阅的实时安全代码路径( 目前只有 Connext )

  - 支持自定义分配器

- ROS 1 \< \> ROS 2动态桥节点

- 执行器线程模式( C++ 和 Python)

- 在编译 / 链接 / 运行时间 时组成节点的组件模型

- 使用标准生命周期管理的组件

- 扩展 `.msg` 具有新特性的格式 :

  - 边界阵列

  - 默认值

<span id="known-issues"></span>

## 已知问题

- 在 Windows Python 启动文件上, 当尝试中止时可能会挂起 `Ctrl-C` (见 [问题](https://github.com/ros2/launch/issues/64)。为了继续使用被挂起命令屏蔽的 shell, 您可能想要使用进程显示器结束挂起的 Python 进程 。

- OpenSplices 支持目前无法为 MacOS 提供 。 [使用本地手柄](https://github.com/ros2/rmw_opensplice/issues/182) 尚未执行。

- 使用 Connext 目前不允许两个具有相同基名但不同命名空间的话题有不同的类型( 请参见 ) [问题](https://github.com/ros2/rmw_connext/issues/234)).
