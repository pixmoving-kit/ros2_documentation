<span id="ros-2-middleware-implementations"></span>

# ROS 2 中间件实现

ROS 中间件实现由一组软件包组成，用于实现 ROS 的部分内部接口，例如 `rmw`、`rcl` 和 `rosidl` API。

关于 ROS 2 如何集成不同中间件实现的深入实践介绍，参见[中间件实现教程](../../Tutorials/Advanced/Creating-An-RMW-Implementation.md)。

<span id="common-packages-for-dds-middleware-packages"></span>

## DDS 中间件共用的软件包

本文所述的 ROS 中间件实现均基于完整或部分 DDS 实现，例如使用 RTI Connext DDS 的实现和使用 eProsima Fast DDS 的实现。因此，大多数基于 DDS 的中间件实现会共用一些软件包。

GitHub 上的 [ros2/rosidl_dds](https://github.com/ros2/rosidl_dds) 仓库包含以下软件包：

- `rosidl_generator_dds_idl`：提供工具，从 `.msg`、`.srv` 等 `rosidl` 文件生成 DDS `.idl` 文件。

对于消息软件包中定义的每个 `rosidl` 文件（例如 `.msg` 文件），`rosidl_generator_dds_idl` 都会生成一个 DDS `.idl` 文件。目前，基于 DDS 的 ROS 中间件实现使用这些 `.idl` 文件，生成针对特定供应商的预编译类型支持。

<span id="structure-of-ros-middleware-implementations"></span>
<span id="about-middleware-impls-struct-dds"></span>

## ROS 中间件实现的结构

一个 ROS 中间件实现通常由同一个仓库中的几个软件包组成：

- `<implementation_name>_cmake_module`：包含用于查找并提供所需依赖信息的 CMake 模块。
- `rmw_<implementation_name>_<language>`：使用特定语言（通常为 C++）实现 `rmw` API。
- `rosidl_typesupport_<implementation_name>_<language>`：提供工具，为 `rosidl` 文件生成针对该中间件实现、使用特定语言（通常为 C 或 C++）编写的静态类型支持代码。

`<implementation_name>_cmake_module` 包含查找中间件依赖所需的 CMake 模块和函数。例如，`rti_connext_dds_cmake_module` 对 RTI Connext DDS 自带的 CMake 模块进行封装，确保所有依赖它的软件包选择同一个 RTI Connext DDS 安装。类似地，`fastrtps_cmake_module` 包含查找 eProsima Fast DDS 的 CMake 模块，`gurumdds_cmake_module` 包含查找 GurumNetworks GurumDDS 的模块。并非所有实现都需要这样的软件包：Eclipse Cyclone DDS 自身提供的 CMake 模块可供其 RMW 实现直接使用，无需额外封装。

`rmw_<implementation_name>_<language>` 使用特定语言实现 `rmw` C API。实现本身可以用 C++ 编写，只需将头文件中的符号以 `extern "C"` 形式导出，使 C 应用程序能够链接它。

`rosidl_typesupport_<implementation_name>_<language>` 提供特定语言的 DDS 代码生成器。它使用 `rosidl_generator_dds_idl` 生成的 `.idl` 文件，以及 DDS 供应商提供的 DDS IDL 代码生成器，还会生成 ROS 消息结构与 DDS 消息结构之间的双向转换代码。此外，它负责为使用它的消息软件包创建共享库；该共享库针对软件包内的消息类型以及所用的 DDS 供应商。

如果某个 rmw 实现支持在运行时解释消息，就可以使用 `rosidl_typesupport_introspection_<language>`，替代供应商专用的类型支持软件包。通过支持 [DDS X-Types Dynamic Data 标准](https://www.omg.org/spec/DDS-XTypes/)，程序可以无需预先生成代码，就通过话题发送和接收相应类型的数据。因此，rmw 实现可以支持 X-Types 标准，也可以提供在编译时生成、针对其 DDS 实现的类型支持软件包，或同时支持两者。

以下是一些 rmw 实现仓库：

- Eclipse Cyclone DDS：[ros2/rmw_cyclonedds](https://github.com/ros2/rmw_cyclonedds)。
- Fast DDS：[ros2/rmw_fastrtps_cpp](https://github.com/ros2/rmw_fastrtps_cpp)。
- Connext DDS：[ros2/rmw_connextdds](https://github.com/ros2/rmw_connextdds)。
- GurumDDS：[ros/rmw_gurumdds](https://github.com/ros2/rmw_gurumdds)。
