<span id="client-libraries"></span>
# 客户端库

<span id="overview"></span>
## 概述

客户端库提供了用于编写 ROS 2 程序的 API。通过客户端库，用户可以使用节点、话题、服务等 ROS 2 概念。客户端库支持多种编程语言，用户可以根据应用需求选择最合适的语言。例如，Python 有利于快速进行原型迭代，适合编写可视化工具；对于系统中注重执行效率的部分，使用 C++ 实现节点可能更合适。

使用不同客户端库编写的节点能够相互交换消息，因为各客户端库都实现了代码生成器，使用户能够在相应的编程语言中使用 ROS 2 接口文件。

除了各语言的通信工具，客户端库还向用户提供了构成 ROS 核心特性的功能。通常可以通过客户端库使用以下功能：

- 名称与命名空间
- 时间（真实时间或仿真时间）
- 参数
- 控制台日志
- 线程模型
- 进程内通信

<span id="supported-client-libraries"></span>
## 支持的客户端库

C++ 客户端库 `rclcpp` 和 Python 客户端库 `rclpy` 都使用了 `rcl` 中的公共功能。

<span id="the-rclcpp-package"></span>
### rclcpp 软件包

ROS C++ 客户端库（`rclcpp`）面向用户，提供符合 C++ 使用习惯的接口，涵盖创建节点、发布者和订阅等全部 ROS 客户端功能。`rclcpp` 构建于 `rcl` 和 `rosidl` API 之上，与 `rosidl_generator_cpp` 生成的 C++ 消息配合使用。

`rclcpp` 充分利用 C++ 以及 C++17 的特性，尽可能让接口易于使用。同时，由于它复用了 `rcl` 的实现，因此能与其他使用 `rcl` API 的客户端库保持一致的行为。

`rclcpp` 仓库位于 GitHub 的 [ros2/rclcpp](https://github.com/ros2/rclcpp)，包含 `rclcpp` 软件包。自动生成的 API 文档见 [rclcpp API 文档](https://docs.ros.org/en/rolling/p/rclcpp/)。

<span id="the-rclpy-package"></span>
### rclpy 软件包

ROS Python 客户端库（`rclpy`）是与 C++ 客户端库对应的 Python 实现。与 C++ 客户端库一样，`rclpy` 也基于 `rcl` 的 C API 实现。它使用列表、上下文对象等 Python 原生类型和模式，提供符合 Python 使用习惯的接口。由于实现采用了 `rcl` API，其功能和行为与其他客户端库保持一致。除了为 `rcl` API 提供符合 Python 习惯的绑定、为每种消息提供 Python 类之外，Python 客户端库还负责执行模型，通过 `threading.Thread` 或类似机制运行 `rcl` API 中的函数。

与 C++ 一样，`rclpy` 会为用户使用的每种 ROS 消息生成专用的 Python 代码；不同之处在于，它最终会把原生 Python 消息对象转换为对应的 C 消息。所有操作都在 Python 消息对象上进行，直到消息需要传入 `rcl` 层时，才会转换为纯 C 表示，以便传给 `rcl` 的 C API。同一进程内的发布者和订阅之间通信时，会尽可能避免这种转换，减少在 Python 表示与 C 表示之间来回转换的开销。

`rclpy` 仓库位于 GitHub 的 [ros2/rclpy](https://github.com/ros2/rclpy)，包含 `rclpy` 软件包。自动生成的 API 文档见 [rclpy API 文档](https://docs.ros.org/en/rolling/p/rclpy/)。

<span id="community-maintained"></span>
### 社区维护的客户端库

C++ 和 Python 客户端库由 ROS 2 核心团队维护，ROS 2 社区成员还维护了其他客户端库：

- [Ada](https://github.com/ada-ros/ada4ros2)：一组用于编写 ROS 2 Ada 应用的软件包，包括 `rcl` 绑定、消息生成器、`tf2` 绑定、示例和教程。
- [C](https://github.com/ros2/rclc)：`rclc` 并不在 `rcl` 之上再封装一层，而是补充 `rcl`，使 `rcl` 与 `rclc` 一起构成功能完整的 C 客户端库。教程见 [micro.ros.org](https://micro.ros.org/)。
- [JVM 和 Android](https://github.com/ros2-java)：ROS 2 的 Java 和 Android 绑定。
- [.NET Core、UWP 和 C#](https://github.com/esteve/ros2_dotnet)：一组用于为 .NET Core 和 .NET Standard 编写 ROS 2 应用的项目，包括绑定、代码生成器、示例等。
- [Node.js](https://www.npmjs.com/package/rclnodejs)：`rclnodejs` 是 ROS 2 的 Node.js 客户端，为 ROS 2 编程提供简洁易用的 JavaScript API。
- [Rust](https://github.com/ros2-rust/ros2_rust)：一组让开发者能够用 Rust 编写 ROS 2 应用的项目，包括 `rclrs` 客户端库、代码生成器、示例等。
- [Flutter 和 Dart](https://github.com/rcldart)：ROS 2 的 Flutter 和 Dart 绑定。

以下是较早的、已不再维护的客户端库：

- [C#](https://github.com/firesurfer/rclcs)
- [Objective C 和 iOS](https://github.com/esteve/ros2_objc)
- [Zig](https://github.com/jacobperron/rclzig)

<span id="common-functionality-rcl"></span>
## 公共功能：rcl

客户端库中的大多数功能并不依赖其所用的编程语言。例如，参数的行为和命名空间的逻辑，理想情况下应在所有编程语言中保持一致。因此，客户端库会使用公共的核心 ROS 客户端库（RCL）接口，复用其中与语言无关的 ROS 概念逻辑和行为，无需从头实现这些公共功能。这样，客户端库只需通过外部函数接口封装 RCL 的公共功能，自身便能保持精简，也更易于开发。公共 RCL 功能以 C 接口形式提供，因为对于客户端库来说，C 通常是最容易封装的语言。

公共核心除了能使客户端库保持轻量，还能让不同语言的行为更加一致。如果核心 RCL 中某项功能的逻辑或行为发生变化，例如命名空间处理发生变化，所有使用 RCL 的客户端库都会随之获得这些变化。此外，公共核心也能减少为多个客户端库修复同一缺陷的维护工作。

`rcl` 的 API 文档见 [rcl API 文档](https://docs.ros.org/en/rolling/p/rcl/)。

<span id="language-specific-functionality"></span>
## 语言特有的功能

需要依赖语言特有功能或属性的客户端库概念，不在 RCL 中实现，而由各个客户端库分别实现。例如，`spin` 函数所使用的线程模型会根据客户端库的编程语言来实现。

<span id="demo"></span>
## 演示

若要了解使用 `rclpy` 的发布者与使用 `rclcpp` 的订阅之间如何交换消息，可以从 17:25 开始观看[这场 ROSCon 演讲](https://vimeo.com/187696091)，并参阅[演示幻灯片](https://roscon.ros.org/2016/presentations/ROSCon%202016%20-%20ROS%202%20Update.pdf)。

<span id="comparison-to-ros-1"></span>
## 与 ROS 1 的比较

ROS 1 的各个客户端库都是从头独立开发的。这使得 ROS 1 的 Python 客户端库可以完全用 Python 实现，从而获得无需编译代码等好处。不过，不同客户端库的命名约定和行为并不总是一致，缺陷需要在多个地方分别修复，而且许多功能只在某一个客户端库中实现，例如 UDPROS。

<span id="summary"></span>
## 小结

复用公共的核心 ROS 客户端库，可以降低以不同编程语言编写客户端库的难度，并使它们的行为更加一致。
