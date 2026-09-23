<span id="beta-3-r2b3"></span>

# Beta 3（`r2b3`）

<span id="supported-platforms"></span>

## 支持的平台

ROS 2 Beta 3 支持三类平台：Ubuntu 16.04（Xenial）、macOS 10.12（Sierra）和 Windows 10。三类平台都提供二进制包和源码编译说明。参见[安装说明](../Installation.md)和 [Beta 3 文档](https://docs.ros2.org/beta3/)。

<span id="features"></span>

## 功能



<span id="improvements-since-beta-2-release"></span>

### 相较 Beta 2 的改进

- Python 执行模型，以及 Python C 扩展中多项内存管理修复。
- 实验性重写的 [ros_control](https://github.com/ros2/ros2_control)。
- 向用户暴露 Fast RTPS 和 Connext 的实现专用符号，参见[示例](https://github.com/ros2/demos/blob/6363be2efe2fea799d92bc22a66e776b2ca9c5d0/demo_nodes_cpp_native/src/talker.cpp)。
- Python 日志 [API](https://github.com/ros2/rclpy/blob/1ef2924ef8e154c0553edf0fdba4840b08b728f8/rclpy/rclpy/logging.py)。
- 修复多个软件包中的内存泄漏和竞态条件。
- 恢复 PrismTech 提供的 OpenSplice 支持，目前适用于 Linux 和 Windows。
- 使用无需补丁的 bloom 发布 ROS 2。

<span id="new-demo-application"></span>

### 新的演示应用

[HSR 演示](https://github.com/ruffsl/hsr_demo)：

- 使用 ROS 2 游戏手柄控制器遥控 HSR 机器人。
- 在 HSR 上的 Docker 容器中运行 `ros1_bridge`，因为机器人使用 Ubuntu Trusty 和 ROS 1。
- 运行 ROS 2 开发版 [rviz](https://github.com/ros2/rviz)，可视化机器人的传感器数据等，参见[视频](https://vimeo.com/237016358)。

<span id="selected-features-from-previous-alpha-beta-releases"></span>

### 之前 Alpha/Beta 版本的部分功能

完整列表见[早期发行说明](../Releases.md)。

- C++ 和 Python 客户端库提供以下 API：发布和订阅 ROS 话题；请求和响应 ROS 服务（同步方式仅支持 C++，也支持异步方式）；获取和设置 ROS 参数（仅支持 C++，包括同步和异步方式）；定时器回调。
- 支持多个 DDS/RTPS 实现之间的互操作。eProsima Fast RTPS 是默认实现，包含在二进制包中；支持 RTI Connext，可从源码构建以试用。PrismTech OpenSplice 的限制见下文。
- 面向网络事件的计算图 API。
- 分布式发现。
- 使用兼容 DDS 实现时，发布和订阅具有实时安全的代码路径，目前仅适用于 Connext；支持自定义分配器。
- ROS 1 ↔ ROS 2 动态桥接节点。
- C++ 和 Python 执行器线程模型。
- 支持在编译、链接或运行时组合节点的组件模型。
- 使用标准生命周期的受管理组件。
- 扩展 `.msg` 格式，支持有界数组和默认值。

<span id="known-issues"></span>

## 已知问题

- Windows 上的 Python 启动文件在尝试用 `Ctrl-C` 中止时可能挂起，参见 [issue](https://github.com/ros2/launch/issues/64)。若要继续使用被该命令阻塞的 shell，可以通过进程监视器结束挂起的 Python 进程。
- macOS 暂不支持 OpenSplice；[访问原生句柄](https://github.com/ros2/rmw_opensplice/issues/182)也尚未实现。
- 使用 Connext 时，基础名称相同但命名空间不同的两个话题不能使用不同类型，参见 [issue](https://github.com/ros2/rmw_connext/issues/234)。
