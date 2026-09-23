---
translation_status: machine_translated
source: Concepts/Intermediate/About-Composition.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="composition"></span>

# 组件组合

<span id="ros-1-nodes-vs-nodelets"></span>

## ROS 1 - 节点对节点

在 ROS 1 中,您也可以将您的代码写成 [ROS 节点](https://wiki.ros.org/Nodes) 或作为 [ROS 节点](https://wiki.ros.org/nodelet). ROS 1 节点编译为可执行文件. ROS 1 节点则编译为共享库,然后通过容器过程在运行时加载.

<span id="ros-2-unified-api"></span>

## ROS 2 - 统一的API

在ROS 2中,推荐的代码写法类似于节点,我们称之为: `Component`。这使得在现有代码中加入共同概念变得容易,例如 [生命周期](https://design.ros2.org/articles/node_lifecycle.html)在ROS 2中避免出现不同的API,这是ROS 1中最大的缺点,因为这两种方法都使用相同的API。

> **说明**
>
> 仍然可以使用节点式的“写自己的主”风格,但对常见的情况则不建议使用。

通过使程序布局成为部署时间的决定,用户可以在以下两种选择:

- 在不同的进程中运行多个节点,同时具有进程/断层隔离以及单个节点更容易调试的好处。

- 在一个单一进程中运行多个节点,其间接费用较低,可选效率更高的通信(见 [进程内部交流](../../Tutorials/Demos/Intra-Process-Communication.md)).

此外,还有 `ros2 launch` 可以用来通过专门的发射行动使这些行动自动化。

<span id="component-container"></span> <span id="componentcontainer"></span>

## 集装箱组件

一个组件容器是一个主机进程,允许您在同一进程空间内运行时加载和管理多个组件.

截至目前,共有下列通用组件集装箱类型:

- [component_container](https://github.com/ros2/rclcpp/blob/rolling/rclcpp_components/src/component_container.cpp)

  - 使用单一组件的最通用容器 `SingleThreadedExecutor` 以执行所有组件。

- [component_container_mt](https://github.com/ros2/rclcpp/blob/rolling/rclcpp_components/src/component_container_mt.cpp)

  - 使用单个组件容器 `MultiThreadedExecutor` 用于执行组件。

- [component_container_isolated](https://github.com/ros2/rclcpp/blob/rolling/rclcpp_components/src/component_container_isolated.cpp)

  - 使用每个组件专用执行器的组件容器: `SingleThreadedExecutor` (违约)或 `MultiThreadedExecutor`.

关于执行人类型的更多信息,请参见: [执行者的类型](About-Executors.md#typesofexecutors)。关于每个组件容器的选项的更多信息,见 [组件容器类型](../../Tutorials/Intermediate/Composition.md#componentcontainertypes) 在构成教程中。

<span id="writing-a-component"></span>

## 写入组件

因为一个组件只建在一个共享的库中,所以它没有 `main` 函数(参见 [谈话者源代码](https://github.com/ros2/demos/blob/rolling/composition/src/talker_component.cpp)。一个组件通常是一个子类: `rclcpp::Node`。由于它不能控制线程,它不应该在构建器中执行任何长期运行或阻断任务。相反,它可以使用定时器来获取定期通知。 此外,它还可以创建出版商、订阅者、服务器和客户端。

将这样的类作为组成部分的一个重要方面是,类本身使用软件包中的宏进行注册。 `rclcpp_components` (参见源代码中最后一行) , 这使得组件在被装入库到运行过程中时可以发现—— 它起到某种切入点的作用 。

此外,一个组件一旦创建,就必须在索引中注册,才能通过工具发现。

``` cmake
add_library(talker_component SHARED src/talker_component.cpp)
rclcpp_components_register_nodes(talker_component "composition::Talker")
# To register multiple components in the same shared library, use multiple calls
# rclcpp_components_register_nodes(talker_component "composition::Talker2")
```

举例来说, [检查此教程](../../Tutorials/Intermediate/Writing-a-Composable-Node.md)

> **说明**
>
> 为了使组件_容器能够找到想要的组件,它必须执行或从已经源代码到相应的工作空间的 shell 发射.

<span id="cmake-registration-macros"></span>

## CMake 注册宏

ROS 2提供了两个CMake宏用于注册组件,每个宏的目的不同:

<span id="rclcpp-components-register-node"></span>

### `rclcpp_components_register_node`

此宏会注册一个组件并生成一个独立的可执行文件。 如果您同时需要可调和性和将节点作为独立进程运行的能力, 请使用此程序 。

``` cmake
add_library(talker_component SHARED src/talker_component.cpp)
rclcpp_components_register_node(talker_component
  PLUGIN "composition::Talker"
  EXECUTABLE talker)
```

<span id="rclcpp-components-register-nodes"></span>

### `rclcpp_components_register_nodes`

此宏将一个或多个组件注册为运行时的构成 **不含** 创建独立的可执行文件。当您想要在运行时装入组件容器的纯组件库时使用此程序。

``` cmake
add_library(talker_component SHARED src/talker_component.cpp)
rclcpp_components_register_nodes(talker_component "composition::Talker")
```

<span id="using-components"></span>

## 使用组件

那个... [组成](https://github.com/ros2/demos/tree/rolling/composition) 软件包包含关于如何使用组件的几种不同方法。

1.  开始 a ([通用集装箱工艺](https://github.com/ros2/rclcpp/blob/rolling/rclcpp_components/src/component_container.cpp)并呼叫ROS服务 [load_node](https://github.com/ros2/rcl_interfaces/blob/rolling/composition_interfaces/srv/LoadNode.srv) 然后,ROS服务将加载通过软件包名称和库名称指定的组件,并在运行过程中开始执行。您也可以使用一个程序化的ROS服务,而不是使用程序化的ROS服务。 [命令行工具](https://github.com/ros2/ros2cli/tree/rolling/ros2component) 以命令行参数引用 ROS 服务

2.  创建一个 [自定义可执行文件](https://github.com/ros2/demos/blob/rolling/composition/src/manual_composition.cpp) 包含在编译时已知的多个节点。这种方法要求每个组件都有一个头文件(对于第一个案例来说严格来说并不需要)。

3.  创建发射文件并使用 `ros2 launch` 以创建包含多个组件的容器进程。

<span id="practical-application"></span>

## 实际应用

试试看 [组成演示](../../Tutorials/Intermediate/Composition.md).
