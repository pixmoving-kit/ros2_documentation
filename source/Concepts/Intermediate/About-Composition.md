<span id="composition"></span>
# 组件组合

<span id="ros-1-nodes-vs-nodelets"></span>
## ROS 1：节点与 Nodelet

在 ROS 1 中，可以将代码编写为 [ROS 节点](https://wiki.ros.org/Nodes)或 [ROS Nodelet](https://wiki.ros.org/nodelet)。ROS 1 节点会编译为可执行文件，而 Nodelet 会编译为共享库，再由容器进程在运行时加载。

<span id="ros-2-unified-api"></span>
## ROS 2：统一的 API

在 ROS 2 中，推荐采用类似 Nodelet 的方式编写代码，我们将其称为组件（`Component`）。这种方式便于将[生命周期](https://design.ros2.org/articles/node_lifecycle.html)等通用概念引入现有代码。ROS 1 的一个主要缺点是两种方式使用不同的 API；ROS 2 中两者使用相同的 API，避免了这一问题。

!!! note "说明"
    仍然可以采用类似独立节点的方式，自行编写 `main` 函数，但在常见场景中不推荐这样做。

将进程布局的选择留到部署阶段，用户便可以在以下方案之间选择：

- 在独立进程中运行多个节点，以获得进程隔离和故障隔离，并方便单独调试各节点。
- 在同一进程中运行多个节点，以降低开销，并可选择更高效的通信方式，参见[进程内通信](../../Tutorials/Demos/Intra-Process-Communication.md)。

此外，还可以通过 `ros2 launch` 中专门的启动动作自动执行这些操作。

<span id="component-container"></span> <span id="componentcontainer"></span>
## 组件容器

组件容器是一个宿主进程，允许在运行时将多个组件加载到同一进程空间中并加以管理。

目前提供以下通用组件容器：

- [`component_container`](https://github.com/ros2/rclcpp/blob/rolling/rclcpp_components/src/component_container.cpp)：使用一个 `SingleThreadedExecutor` 执行所有组件，是最通用的组件容器。
- [`component_container_mt`](https://github.com/ros2/rclcpp/blob/rolling/rclcpp_components/src/component_container_mt.cpp)：使用一个 `MultiThreadedExecutor` 执行各组件。
- [`component_container_isolated`](https://github.com/ros2/rclcpp/blob/rolling/rclcpp_components/src/component_container_isolated.cpp)：为每个组件提供专用的执行器，可选 `SingleThreadedExecutor`（默认）或 `MultiThreadedExecutor`。

有关执行器类型的更多信息，请参见[执行器类型](About-Executors.md#typesofexecutors)。有关各组件容器的选项，请参见组合教程中的[组件容器类型](../../Tutorials/Intermediate/Composition.md#componentcontainertypes)。

<span id="writing-a-component"></span>
## 编写组件

组件只会构建为共享库，因此没有 `main` 函数，参见 [Talker 源代码](https://github.com/ros2/demos/blob/rolling/composition/src/talker_component.cpp)。组件通常是 `rclcpp::Node` 的子类。由于组件并不掌控线程，因此不应在构造函数中执行长时间运行或阻塞的任务。可以改用定时器来接收周期性通知。此外，组件也可以创建发布者、订阅、服务端和客户端。

要使这样的类成为组件，一个重要步骤是使用 `rclcpp_components` 包提供的宏注册该类，参见源代码的最后一行。这样，在将组件库加载到运行中的进程时，就能发现该组件；这个注册机制起到类似入口点的作用。

此外，创建组件后，还必须将其注册到索引中，工具才能发现它。

```cmake
add_library(talker_component SHARED src/talker_component.cpp)
rclcpp_components_register_nodes(talker_component "composition::Talker")
# To register multiple components in the same shared library, use multiple calls
# rclcpp_components_register_nodes(talker_component "composition::Talker2")
```

示例参见[编写可组合节点教程](../../Tutorials/Intermediate/Writing-a-Composable-Node.md)。

!!! note "说明"
    为使 `component_container` 能够找到所需组件，必须在已加载（source）相应工作空间环境的 shell 中运行或启动它。

<span id="cmake-registration-macros"></span>
## CMake 注册宏

ROS 2 提供了两个用于注册组件的 CMake 宏，分别适用于不同用途。

<span id="rclcpp-components-register-node"></span>
### `rclcpp_components_register_node`

此宏会注册一个组件，并生成一个独立的可执行文件。如果既希望支持组件组合，又希望能够将节点作为独立进程运行，可以使用此宏。

```cmake
add_library(talker_component SHARED src/talker_component.cpp)
rclcpp_components_register_node(talker_component
  PLUGIN "composition::Talker"
  EXECUTABLE talker)
```

<span id="rclcpp-components-register-nodes"></span>
### `rclcpp_components_register_nodes`

此宏会注册一个或多个用于运行时组合的组件，**不会**创建独立的可执行文件。如果只需要在运行时加载到组件容器中的组件库，可以使用此宏。

```cmake
add_library(talker_component SHARED src/talker_component.cpp)
rclcpp_components_register_nodes(talker_component "composition::Talker")
```

<span id="using-components"></span>
## 使用组件

[`composition`](https://github.com/ros2/demos/tree/rolling/composition) 包展示了几种使用组件的方法，其中最常见的三种是：

1. 启动[通用容器进程](https://github.com/ros2/rclcpp/blob/rolling/rclcpp_components/src/component_container.cpp)，调用容器提供的 ROS 服务 [`load_node`](https://github.com/ros2/rcl_interfaces/blob/rolling/composition_interfaces/srv/LoadNode.srv)。该服务会根据传入的包名和库名加载指定组件，并在运行中的进程内开始执行该组件。除了通过程序调用该服务，也可以使用[命令行工具](https://github.com/ros2/ros2cli/tree/rolling/ros2component)，通过命令行参数调用服务。
2. 创建一个[自定义可执行文件](https://github.com/ros2/demos/blob/rolling/composition/src/manual_composition.cpp)，其中包含编译时已知的多个节点。这种方式要求每个组件都有头文件，而第一种方式并没有这一硬性要求。
3. 创建启动文件，通过 `ros2 launch` 创建容器进程，并加载多个组件。

<span id="practical-application"></span>
## 实践应用

尝试运行[组件组合示例](../../Tutorials/Intermediate/Composition.md)。
