---
translation_status: machine_translated
source: How-To-Guides/Migrating-from-ROS1/Migrating-Interfaces.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="migrating-interfaces"></span>

# 迁移接口

信息、服务和行动被统称为 `interfaces` 在罗斯2号线上

<span id="interface-definitions"></span>

## 界面定义

信件文件必须结束于 `.msg` 并必须在子文件夹中找到 `msg`。服务文件必须结束于 `.srv` 并必须在子文件夹中找到 `srv`。动作文件必须结束于 `.action` 并必须在子文件夹中找到 `action`.

这些文件可能需要更新,以遵守 [ROS 界面定义](http://design.ros2.org/articles/legacy_interface_definition.html)。一些原始类型和类型已被删除。 `duration` 财务报告和财务报告 `time` 在ROS 1中内建的类别已被普通信件定义所取代,必须从 [builtin_interfaces](https://github.com/ros2/rcl_interfaces/tree/rolling/builtin_interfaces) 此外,有些命名惯例比ROS 1更为严格。 [概念性条款](../../Concepts/Basic/About-Interfaces.md).

<span id="building-interfaces"></span>

## 大楼接口

ROS 2中接口的构建方式与ROS 1. 接口的构建方式大不相同. `CMakeLists.txt`。如果您正在开发一个纯 Python 软件包,那么接口应该放在一个只包含接口的另类软件包中(不管怎样,这是最佳做法)。请参见 [自定义接口教程](../../Tutorials/Beginner-Client-Libraries/Custom-ROS2-Interfaces.md) 以获取更多信息。

<span id="migrating-interface-package-to-ros-2"></span>

### 移动接口包到 ROS 2

在你身边 `package.xml`:

- 添加 `<buildtool_depend>rosidl_default_generators</buildtool_depend>`.

- 添加 `<exec_depend>rosidl_default_runtime</exec_depend>`.

- 添加 `<member_of_group>rosidl_interface_packages</member_of_group>`

- 对于每个依赖的信息包,添加 `<depend>message_package</depend>`.

在你身边 `CMakeLists.txt`:

- 启用 C++17

``` cmake
set(CMAKE_CXX_STANDARD 17)
```

- 添加 `find_package(rosidl_default_generators REQUIRED)`

- 对于每个依赖的信息包,添加 `find_package(message_package REQUIRED)` 并替换 CMake 函数调用到 `generate_messages` 与 `rosidl_generate_interfaces`.

这将取代 `add_message_files` 财务报告和财务报告 `add_service_files` 列出可以删除的所有信件和服务文件。
