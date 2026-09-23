<span id="internal-ros-2-interfaces"></span>

# ROS 2 内部接口

ROS 内部接口是公开的 C API，供开发客户端库或接入新底层中间件的开发者使用，并非面向普通 ROS 用户。大多数 ROS 用户熟悉的用户层 API 由 ROS 客户端库提供，这些库可以使用多种编程语言实现。

<span id="internal-api-architecture-overview"></span>

## 内部 API 架构概述

主要有两类内部接口：

- ROS 中间件接口（`rmw` API）。
- ROS 客户端库接口（`rcl` API）。

`rmw` API 位于 ROS 2 软件栈与底层中间件实现之间。本文介绍的 ROS 2 底层中间件采用 DDS 或 RTPS 实现，负责发现、发布与订阅、服务的请求与响应，以及消息类型的序列化。

`rcl` API 的层次稍高，用于实现客户端库。它不直接访问中间件实现，而是通过 ROS 中间件接口（`rmw` API）这一抽象层访问。

![ROS 2 软件栈](../images/ros_client_library_api_stack.png)

如图所示，这些 API 逐层叠加：普通 ROS 用户通过 `rclcpp` 等客户端库 API 编写可执行程序或库。客户端库（例如 `rclcpp`）通过 `rcl` 接口访问 ROS 计算图和图事件；`rcl` 的实现再通过 `rmw` API 访问 ROS 计算图。`rcl` 的作用是为多个客户端库提供较复杂的 ROS 概念和实用功能的通用实现，同时不依赖特定底层中间件。`rmw` 接口则仅包含支持 ROS 客户端库所必需的最少中间件功能。最终，`rmw` API 由特定中间件的软件包（如 `rmw_fastrtps_cpp`）实现，其库使用对应供应商的 DDS 接口和类型进行编译。

上图中还有一个标为 `ros_to_dds` 的方框，代表一类可能的软件包：它们允许用户通过 ROS 对应对象访问 DDS 供应商专有的对象和设置。抽象接口的目标之一，是将 ROS 用户代码与所用的中间件完全隔离，使切换 DDS 供应商甚至中间件技术时，对用户代码的影响尽可能小。不过，有时确实需要深入实现内部手动调整设置，即使这样做可能带来一些影响。通过要求使用这类专用软件包才能访问底层 DDS 供应商对象，可以避免在常规接口中暴露供应商专用的符号和头文件。此外，只需检查软件包是否依赖某个 `ros_to_dds` 软件包，就能识别哪些代码可能影响跨供应商的可移植性。

<span id="type-specific-interfaces"></span>
<span id="id1"></span>

## 类型专用接口

在整个调用链中，某些 API 必须针对所交换的消息类型，例如发布消息或订阅话题。因此，需要为每种消息类型生成代码。下图展示了用户定义的 `rosidl` 文件（如 `.msg` 文件）如何转化为类型专用代码，供用户和系统执行与类型有关的操作。

<span id="id2"></span>

![ROS 2 IDL 静态类型支持栈](../images/ros_idl_api_stack_static.png)

*图：从 `rosidl` 文件到用户代码的“静态”类型支持生成流程。*

图的右侧展示了 `.msg` 文件如何直接传递给特定语言的代码生成器，例如 `rosidl_generator_cpp` 或 `rosidl_generator_py`。生成器创建相应代码，供用户包含或导入，并用作 `.msg` 文件所定义消息的内存表示。例如，使用 `std_msgs/String` 消息时，C++ 用户可以编写 `#include <std_msgs/msg/string.hpp>`，Python 用户可以编写 `from std_msgs.msg import String`。这些语句所引用的文件，正是由特定语言但与中间件无关的生成器软件包创建的。

另一方面，`.msg` 文件还用于为每种类型生成类型支持代码。这里的“类型支持”是指针对某种类型的元数据或函数，系统依靠它们完成该类型的特定任务。例如，某个消息的类型支持可能包含其所有字段的名称和类型列表，也可能包含指向执行特定操作（例如发布消息）的代码的引用。

<span id="static-type-support"></span>
<span id="internal-interfaces-static-type-support"></span>

### 静态类型支持

当类型支持引用代码来执行某种消息类型的特定操作时，这些代码有时需要执行中间件专有的操作。例如，类型专用的发布函数在使用“供应商 A”时需要调用 A 的 API，使用“供应商 B”时则需要调用 B 的 API。为支持这类代码，用户定义的 `.msg` 文件可能会用于生成供应商专用的代码。这些代码仍通过类型支持抽象层对用户隐藏，其方式类似于“私有实现”（Private Implementation，Pimpl）模式。

<span id="static-type-support-with-dds"></span>

### DDS 的静态类型支持

对于基于 DDS、尤其是根据 OMG IDL 文件（`.idl` 文件）生成代码的中间件，用户定义的 `rosidl` 文件（`.msg` 文件）会转换为等价的 OMG IDL 文件。随后根据这些文件生成供应商专用代码，并在类型支持所引用的类型专用函数中使用这些代码。上图左侧展示了这一过程：`rosidl_dds` 软件包读取 `.msg` 文件并生成 `.idl` 文件，再将 `.idl` 文件交给特定语言和 DDS 供应商的类型支持生成软件包。

以 Fast DDS 实现为例，`rosidl_typesupport_fastrtps_cpp` 软件包负责生成代码，将 C++ 消息对象转换为待通过网络发送的序列化字节缓冲区等。尽管这些代码专用于 Fast DDS，但由于类型支持代码提供了抽象层，它们仍不会暴露给用户。

<span id="dynamic-type-support"></span>
<span id="internal-interfaces-dynamic-type-support"></span>

### 动态类型支持

实现类型支持的另一种方式，是使用通用函数来执行向话题发布消息等操作，而不为每种消息类型分别生成函数。为此，通用函数需要消息类型的元信息，例如按消息中出现顺序排列的字段名称和类型列表。发布消息时，调用通用发布函数，同时传入消息和包含所需类型元数据的结构即可。这称为“动态”类型支持；与之相对，“静态”类型支持需要为每种类型生成专用函数。

<span id="id3"></span>

![ROS 2 IDL 动态类型支持栈](../images/ros_idl_api_stack_dynamic.png)

*图：从 `rosidl` 文件到用户代码的“动态”类型支持生成流程。*

上图展示了从用户定义的 `rosidl` 文件到生成用户代码的过程。它与静态类型支持的流程非常相似，区别仅在图左侧所示的类型支持生成方式。在动态类型支持中，`.msg` 文件直接转换为用户代码。

这些代码只包含消息的元信息，因此同样与中间件无关。实际执行操作（例如向话题发布消息）的函数适用于各种消息类型，并在必要时调用特定中间件的 API。静态类型支持由 DDS 供应商专用的软件包提供代码；动态类型支持则为每种语言提供与中间件无关的软件包，例如 `rosidl_typesupport_introspection_c` 和 `rosidl_typesupport_introspection_cpp`。软件包名称中的 `introspection` 指的是使用生成的消息类型元数据，对任意消息实例进行内省的能力。这正是以通用方式实现“向话题发布消息”等功能的基础。

这种方式的优点是：所有生成的代码都与中间件无关，只要其他中间件实现支持动态类型支持，就可以复用这些代码。此外，生成代码更少，也降低了编译时间和代码体积。

不过，动态类型支持要求底层中间件具备类似的能力。在 DDS 中，DDS-XTypes 标准允许使用元信息而非生成代码来发布消息。因此，底层中间件必须支持 DDS-XTypes 或类似机制。另外，动态类型支持通常比静态类型支持更慢。静态类型支持的类型专用生成代码，在序列化等操作中无需遍历类型元数据，因此可以实现更高的效率。

<span id="the-rcl-repository"></span>

## `rcl` 仓库

ROS 客户端库接口（`rcl` API）可供 `rclc`、`rclcpp`、`rclpy` 等客户端库使用，避免重复实现逻辑和功能。复用 `rcl` API 能使客户端库更精简，也使不同库之间更一致。某些功能有意不纳入 `rcl` API，因为这些部分应采用符合各语言习惯的方式实现。执行模型就是一个例子：`rcl` 完全不涉及它，而由客户端库提供符合语言习惯的方案，如 C 中的 `pthreads`、C++11 中的 `std::thread` 和 Python 中的 `threading.Thread`。总体而言，`rcl` 提供既不依赖特定语言模式、也不依赖特定消息类型的函数。

`rcl` API 位于 GitHub 上的 [ros2/rcl](https://github.com/ros2/rcl) 仓库，以 C 头文件定义接口。同一仓库中的 `rcl` 软件包提供其 C 实现。该实现通过 `rmw` 和 `rosidl` API 工作，避免直接接触中间件。

完整 API 定义参见 [rcl 文档](http://docs.ros.org/en/rolling/p/rcl/)。

<span id="the-rmw-repository"></span>

## `rmw` 仓库

ROS 中间件接口（`rmw` API）定义了在中间件之上构建 ROS 所需的最少基础能力。不同中间件的提供者必须实现此接口，才能支撑完整的 ROS 软件栈。目前，大多数中间件实现面向不同的 DDS 供应商。

`rmw` API 位于 [ros2/rmw](https://github.com/ros2/rmw) 仓库。`rmw` 软件包包含定义接口的 C 头文件；接口的实际实现由面向不同 DDS 供应商的各个 rmw 实现软件包提供。

API 定义参见 [rmw 文档](http://docs.ros.org/en/rolling/p/rmw/)。关于 ROS 2 与不同中间件集成的深入实践介绍，参见[中间件实现教程](../../Tutorials/Advanced/Creating-An-RMW-Implementation.md)。

<span id="the-rosidl-repository"></span>

## `rosidl` 仓库

`rosidl` API 包含一些与消息有关的静态函数和类型，并规定了针对不同语言应生成哪些消息代码。这些生成代码面向特定语言，可以复用其他语言的生成代码，也可以独立实现。API 规定的生成内容包括消息数据结构、构造和析构函数等，还提供获取消息类型支持结构的方法；发布或订阅该类型的话题时需要使用这一结构。

多个仓库共同参与 `rosidl` API 及其实现。

GitHub 上的 [ros2/rosidl](https://github.com/ros2/rosidl) 仓库定义消息 IDL 语法，即 `.msg`、`.srv` 等文件的语法，并包含用于解析文件、提供消息代码生成的 CMake 基础设施、生成与实现无关的头文件和源文件，以及确定默认生成器集合的软件包：

- `rosidl_cmake`：提供根据 `.msg`、`.srv` 等 `rosidl` 文件生成代码的 CMake 函数和模块。
- `rosidl_default_generators`：定义默认生成器列表，确保这些生成器作为依赖被安装；也可以使用额外注入的生成器。
- `rosidl_generator_c`：提供根据 `rosidl` 文件生成 C 头文件（`.h`）的工具。
- `rosidl_generator_cpp`：提供根据 `rosidl` 文件生成 C++ 头文件（`.hpp`）的工具。
- `rosidl_generator_py`：提供根据 `rosidl` 文件生成 Python 模块的工具。
- `rosidl_parser`：提供解析 `rosidl` 文件的 Python API。

其他语言的生成器（如 `rosidl_generator_java`）位于外部仓库，但会使用与上述生成器相同的机制，将自身注册为 `rosidl` 生成器。

除解析 `rosidl` 文件和生成头文件的软件包外，`rosidl` 仓库还包含为文件中定义的消息类型提供“类型支持”的软件包。类型支持是指解释和操作特定类型的 ROS 消息实例所表示的信息的能力，例如发布消息。它既可以由编译时生成的代码提供，也可以通过内省，根据 `.msg`、`.srv` 等 `rosidl` 文件内容和接收到的数据在运行时实现。在运行时解释消息的情况下，ROS 2 生成的消息代码可以与具体 rmw 实现无关。通过数据内省提供类型支持的软件包包括：

- `rosidl_typesupport_introspection_c`：提供生成 C 代码的工具，以支持 `rosidl` 消息数据类型。
- `rosidl_typesupport_introspection_cpp`：提供生成 C++ 代码的工具，以支持 `rosidl` 消息数据类型。

如果类型支持在编译时生成，而不是在运行时通过程序解释实现，就需要使用针对特定 rmw 实现的软件包。这是因为，特定 rmw 实现通常要求按照 DDS 供应商专用的方式存储和操作数据，以便 DDS 实现使用它。详情参见前文的[类型专用接口](#type-specific-interfaces)。

关于 `rosidl` API（静态部分和生成部分）的具体内容，原文在此提到了进一步阅读的页面，但未提供链接。

<span id="the-rcutils-repository"></span>

## `rcutils` 仓库

ROS 2 C 工具库（`rcutils`）是由宏、函数和数据结构组成的 C API，在 ROS 2 代码中广泛使用。它主要用于错误处理、命令行参数解析和日志记录等不专属于客户端层或中间件层的功能，因此可由两层共用。

`rcutils` API 及其实现位于 GitHub 上的 [ros2/rcutils](https://github.com/ros2/rcutils) 仓库，接口通过 C 头文件定义。

完整 API 定义参见 [rcutils 文档](https://docs.ros.org/en/rolling/p/rcutils/)。
