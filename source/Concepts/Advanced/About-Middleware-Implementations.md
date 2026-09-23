---
translation_status: machine_translated
source: Concepts/Advanced/About-Middleware-Implementations.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="ros-2-middleware-implementations"></span>

# ROS 2 中间件实现

ROS 中间软件的安装是一组 [软件包](../../Glossary.md#term-package) 执行一些内部ROS接口,例如: `rmw`, `rcl`,以及 `rosidl` [APIs 辅助程序](../../Glossary.md#term-API).

关于ROS 2如何与不同的中间软件执行集成的更实际的深入概述,参见: [中间软件执行教程](../../Tutorials/Advanced/Creating-An-RMW-Implementation.md).

<span id="common-packages-for-dds-middleware-packages"></span>

## DDS 中间软件包的常见软件包

目前所有的ROS中间软件执行都是基于全部或部分的DDS执行。例如,有一个中间软件执行是使用 RTI 的 Connext DDS , 一个执行是使用 eProsima 的快速DDS 。由于这个原因,有一些共享 [软件包](../../Glossary.md#term-package) 在大多数基于DDS的中间软件执行中。

在那个 [ros2/rosidl_dds](https://github.com/ros2/rosidl_dds) 运行于 [GitHub](https://github.com/),有以下内容: [软件包](../../Glossary.md#term-package):

- `rosidl_generator_dds_idl`: 提供生成 DDS 的工具 `.idl` 文件来自 `rosidl` 文档,例如: `.msg` 文档, `.srv` 文档等。

那个... `rosidl_generator_dds_idl` [软件包](../../Glossary.md#term-package) 生成一个 DDS `.idl` 每个文件 `rosidl` 文档,例如: `.msg` 文件, 定义 [软件包](../../Glossary.md#term-package) 包含信件。当前基于 DDS 的 ROS 中间软件执行会利用此生成器的输出 `.idl` 生成预编译类型支持的文档。

<span id="structure-of-ros-middleware-implementations"></span> <span id="about-middleware-impls-struct-dds"></span>

## ROS 中件执行的结构

ROS 中间软件的执行通常由几个部分组成 [软件包](../../Glossary.md#term-package) 在一个单独的存储器中:

- `<implementation_name>_cmake_module`: 包含用于发现和暴露所需依赖关系的CMake模块

- `rmw_<implementation_name>_<language>`: 载有《公约》执行情况 `rmw` [API](../../Glossary.md#term-API) 在特定语言中,通常为 C++

- `rosidl_typesupport_<implementation_name>_<language>`: 包含生成静态类型支持代码的工具 `rosidl` 文档,以特定语言定制,通常为 C 或 C++

那个... `<implementation_name>_cmake_module` [软件包](../../Glossary.md#term-package) 包含任何需要的 CMake 模块和函数,以找到支持中软件执行的依赖性。例如, `rti_connext_dds_cmake_module` 提供用 RTI Connext DDS 运来的 CMake 模块周围的包装逻辑,以确保所有依赖它的软件包都会选择同样的 RTI Connext DDS 安装。同样, `fastrtps_cmake_module` 包括一个 CMake 模块,用于查找 eProsima 的快速 DDS 和 `gurumdds_cmake_module` 包含一个 CMake 模块来寻找 GurumNetworks GurumDDS 。 并非所有执行都会有这样的软件包: 例如, Eclipe 的 Cycrop DDS 已经提供了 CMake 模块, 由 CMake 模块直接使用, 而不需要额外的包装 。

那个... `rmw_<implementation_name>_<language>` [软件包](../../Glossary.md#term-package) 执行 `rmw` C [API](../../Glossary.md#term-API) 。执行本身可以是 C++,它只需要将信头的符号暴露为 `extern "C"` 这样C应用程序就可以链接到它上.

那个... `rosidl_typesupport_<implementation_name>_<language>` [软件包](../../Glossary.md#term-package) 提供生成特定语言的 DDS 代码的生成器。使用 `.idl` 生成的文件 `rosidl_generator_dds_idl` [软件包](../../Glossary.md#term-package) 和由 DDS 供应商提供的 DDS IDL 代码生成器。它也会生成将 ROS 信件结构转换为 DDS 信件结构的代码。这个生成器还负责为它正在使用的信息包创建一个共享库,该库是针对信件包中的信息和正在使用的 DDS 供应商的。

如上文所述, `rosidl_typesupport_introspection_<language>` 如果一个 Rmw 执行支持消息的运行时间解释,则可以使用供应商特定类型的一揽子支持。 [DDS X- Types 动态数据标准](https://www.omg.org/spec/DDS-XTypes/)因此,rmw 执行可能会为 X-Types 标准提供支持,和/或为在编译其DDS 执行所特有的时间生成的类型支持提供套件.

作为 Rmw 执行存储库的一个例子, `Eclipse Cyclone DDS` ROS 中间软件执行启动 [GitHub](https://github.com/) 现时 [ros2/rmw_cyclonedds](https://github.com/ros2/rmw_cyclonedds).

rmw 的实施工作 `Fast DDS` 开始 [GitHub](https://github.com/) 现时 [ros2/rmw_fastrtps_cpp](https://github.com/ros2/rmw_fastrtps_cpp).

rmw 的实施工作 `Connext DDS` 开始 [GitHub](https://github.com/) 现时 [ros2/rmw_connextdds](https://github.com/ros2/rmw_connextdds).

rmw 的实施工作 `GurumDDS` 开始 [GitHub](https://github.com/) 现时 [ros/rmw_gurumdds](https://github.com/ros2/rmw_gurumdds).
