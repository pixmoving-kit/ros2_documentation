<span id="tf2"></span> <span id="tf2main"></span>

# tf2

许多 tf2 教程同时提供 C++ 和 Python 版本，内容按各自的学习路线组织。如果希望掌握两种语言，建议分别完整学习 C++ 路线和 Python 路线。

<span id="workspace-setup"></span>

## 配置工作空间

如果尚未创建用于学习的工作空间，请先完成[创建工作空间](../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md)教程。

<span id="learning-tf2"></span>

## 学习 tf2

1. [tf2 入门](Introduction-To-Tf2.md)：通过 turtlesim 多机器人示例了解 tf2 的用途，并学习 `tf2_echo`、`view_frames` 和 RViz。
2. 编写静态广播器：[Python](Writing-A-Tf2-Static-Broadcaster-Py.md)、[C++](Writing-A-Tf2-Static-Broadcaster-Cpp.md)。学习向 tf2 广播静态坐标系。
3. 编写广播器：[Python](Writing-A-Tf2-Broadcaster-Py.md)、[C++](Writing-A-Tf2-Broadcaster-Cpp.md)。学习向 tf2 广播机器人状态。
4. 编写监听器：[Python](Writing-A-Tf2-Listener-Py.md)、[C++](Writing-A-Tf2-Listener-Cpp.md)。学习通过 tf2 获取坐标系变换。
5. 添加坐标系：[Python](Adding-A-Frame-Py.md)、[C++](Adding-A-Frame-Cpp.md)。学习添加额外的固定坐标系。
6. [使用时间（C++）](Learning-About-Tf2-And-Time-Cpp.md)：学习使用 `lookup_transform` 的超时参数，等待变换在 tf2 树中可用。
7. [时间穿越（C++）](Time-Travel-With-Tf2-Cpp.md)：学习 tf2 的高级跨时间查询功能。

<span id="debugging-tf2"></span>

## 调试 tf2

1. [四元数基础](Quaternion-Fundamentals.md)：学习 ROS 2 中四元数的基本用法。
2. [调试 tf2 问题](Debugging-Tf2-Problems.md)：学习系统化的 tf2 问题排查方法。

<span id="using-sensor-messages-with-tf2"></span>

## 配合 tf2 使用传感器消息

1. [通过 tf2_ros::MessageFilter 使用带时间戳的数据类型](Using-Stamped-Datatypes-With-Tf2-Ros-MessageFilter.md)：学习如何处理带时间戳的数据类型。
