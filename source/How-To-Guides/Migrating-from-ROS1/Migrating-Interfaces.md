<span id="migrating-interfaces"></span>
# 迁移接口

ROS 2 将消息、服务和动作统称为接口（interfaces）。

<span id="interface-definitions"></span>
## 接口定义

消息文件必须以 `.msg` 结尾，并放在 `msg` 子目录中。服务文件必须以 `.srv` 结尾，并放在 `srv` 子目录中。动作文件必须以 `.action` 结尾，并放在 `action` 子目录中。

可能需要更新这些文件，使其符合 [ROS 接口定义](http://design.ros2.org/articles/legacy_interface_definition.html)。某些基本类型已被移除；ROS 1 中内置的 `duration` 和 `time` 类型已改为普通消息定义，必须使用 [`builtin_interfaces`](https://github.com/ros2/rcl_interfaces/tree/rolling/builtin_interfaces) 包中的类型。此外，部分命名规范也比 ROS 1 更严格。更多信息见[接口概念文档](../../Concepts/Basic/About-Interfaces.md)。

<span id="building-interfaces"></span>
## 构建接口

ROS 2 构建接口的方式与 ROS 1 有很大不同。接口只能在包含 `CMakeLists.txt` 的软件包中构建。如果开发的是纯 Python 软件包，应将接口放在另一个专门存放接口的软件包中，这本身也是推荐做法。更多信息见[自定义接口教程](../../Tutorials/Beginner-Client-Libraries/Custom-ROS2-Interfaces.md)。

<span id="migrating-interface-package-to-ros-2"></span>
### 将接口软件包迁移到 ROS 2

在 `package.xml` 中：

- 添加 `<buildtool_depend>rosidl_default_generators</buildtool_depend>`。
- 添加 `<exec_depend>rosidl_default_runtime</exec_depend>`。
- 添加 `<member_of_group>rosidl_interface_packages</member_of_group>`。
- 为每个依赖的消息包添加 `<depend>message_package</depend>`。

在 `CMakeLists.txt` 中启用 C++17：

```cmake
set(CMAKE_CXX_STANDARD 17)
```

然后：

- 添加 `find_package(rosidl_default_generators REQUIRED)`。
- 为每个依赖的消息包添加 `find_package(message_package REQUIRED)`，并将 `generate_messages` 调用替换为 `rosidl_generate_interfaces`。

这样就可以替代原先通过 `add_message_files` 和 `add_service_files` 列出所有消息及服务文件的方式，并删除这两个调用。
