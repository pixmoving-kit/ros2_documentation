<span id="composing-multiple-nodes-in-a-single-process"></span>

# 将多个节点组合到同一进程

**目标：** 将多个节点组合到一个进程中运行。

**教程级别：** 中级

**预计耗时：** 20 分钟

<span id="background"></span>

## 背景

参阅[组合概念](../../Concepts/Intermediate/About-Composition.md)。关于如何编写可组合节点，参阅[这篇教程](Writing-a-Composable-Node.md)。

<span id="prerequisites"></span>

## 前提条件

本教程使用 [rclcpp_components](https://github.com/ros2/rclcpp/tree/rolling/rclcpp_components)、[ros2component](https://github.com/ros2/ros2cli/tree/rolling/ros2component)、[composition](https://github.com/ros2/demos/tree/rolling/composition) 和 [image_tools](https://github.com/ros2/demos/tree/rolling/image_tools) 中的可执行程序。如果已经按照对应平台的[安装说明](../../Installation.md)安装，它们应已可用。

<span id="run-the-demos"></span>

## 运行示例

<span id="discover-available-components"></span>

### 查找可用组件

在终端执行以下命令，查看工作空间中已注册、可用的组件：

```console
$ ros2 component types
(... components of other packages here)
composition
  composition::Talker
  composition::Listener
  composition::NodeLikeListener
  composition::Server
  composition::Client
(... components of other packages here)
```

<span id="run-time-composition-using-ros-services-with-a-publisher-and-subscriber"></span>

### 通过 ROS 服务在运行时组合发布者和订阅者

在第一个终端启动组件容器：

```console
$ ros2 run rclcpp_components component_container
```

打开第二个终端，通过 `ros2` 命令确认容器正在运行，应看到容器名称：

```console
$ ros2 component list
/ComponentManager
```

在第二个终端加载 talker 组件，参见 [talker 源代码](https://github.com/ros2/demos/blob/rolling/composition/src/talker_component.cpp)。命令返回组件的唯一 ID 和节点名：

```console
$ ros2 component load /ComponentManager composition composition::Talker
Loaded component 1 into '/ComponentManager' container node as '/talker'
```

第一个终端应显示组件已加载的消息，并持续输出发布消息的日志。

在第二个终端加载 listener 组件，参见 [listener 源代码](https://github.com/ros2/demos/blob/rolling/composition/src/listener_component.cpp)：

```console
$ ros2 component load /ComponentManager composition composition::Listener
Loaded component 2 into '/ComponentManager' container node as '/listener'
```

现在可用 `ros2` 查看容器状态：

```console
$ ros2 component list
/ComponentManager
   1  /talker
   2  /listener
```

第一个终端应持续输出每次收到消息的日志。

<span id="run-time-composition-using-ros-services-with-a-server-and-client"></span>

### 通过 ROS 服务在运行时组合服务端和客户端

服务端与客户端的示例非常相似。在第一个终端执行：

```console
$ ros2 run rclcpp_components component_container
```

在第二个终端执行以下命令，源代码见 [server](https://github.com/ros2/demos/blob/rolling/composition/src/server_component.cpp) 和 [client](https://github.com/ros2/demos/blob/rolling/composition/src/client_component.cpp)：

```console
$ ros2 component load /ComponentManager composition composition::Server
$ ros2 component load /ComponentManager composition composition::Client
```

客户端向服务端发送请求，服务端处理并返回响应，客户端打印收到的响应。

<span id="compile-time-composition-with-hardcoded-nodes"></span>

### 在编译时组合硬编码的节点

相同的共享库也可用于编译一个运行多个组件的可执行程序，无须使用 ROS 接口。此示例在主函数中硬编码包含上述四个组件：talker、listener、server 和 client。

运行以下命令，源代码见 [manual_composition.cpp](https://github.com/ros2/demos/blob/rolling/composition/src/manual_composition.cpp)：

```console
$ ros2 run composition manual_composition
```

终端应持续显示两组组件的消息。

> 手动组合的组件不会出现在 `ros2 component list` 输出中。

<span id="run-time-composition-using-dlopen"></span>

### 通过 dlopen 在运行时组合

另一种运行时组合方式是创建通用容器进程，显式传入需要加载的库，不使用 ROS 接口。进程打开每个库，并为其中每个 `rclcpp::Node` 类创建一个实例，参见[源代码](https://github.com/ros2/demos/blob/rolling/composition/src/dlopen_composition.cpp)。

Linux：

```console
$ ros2 run composition dlopen_composition `ros2 pkg prefix composition`/lib/libtalker_component.so `ros2 pkg prefix composition`/lib/liblistener_component.so
```

macOS：

```console
$ ros2 run composition dlopen_composition `ros2 pkg prefix composition`/lib/libtalker_component.dylib `ros2 pkg prefix composition`/lib/liblistener_component.dylib
```

Windows 先查询 composition 的安装路径：

```console
$ ros2 pkg prefix composition
```

然后运行：

```console
$ ros2 run composition dlopen_composition <path_to_composition_install>\bin\talker_component.dll <path_to_composition_install>\bin\listener_component.dll
```

终端应持续显示每次发送和接收消息的输出。

> 通过 dlopen 组合的组件不会出现在 `ros2 component list` 输出中。

<span id="composition-using-launch-actions"></span>

### 使用启动动作进行组合

命令行工具便于调试、诊断组件配置，但通常一次启动一组组件更方便。可以使用[启动文件](https://github.com/ros2/demos/blob/rolling/composition/launch/composition_demo.launch.py)自动完成：

```console
$ ros2 launch composition composition_demo.launch.py
```

<span id="advanced-topics"></span>

## 进阶内容

掌握组件基本操作后，下面介绍更深入的用法。

<span id="component-container-types"></span> <span id="componentcontainertypes"></span>

### 组件容器类型

[组件容器概念](../../Concepts/Intermediate/About-Composition.md#componentcontainer)介绍了若干具有不同选项的容器类型，可按需求选择。

`component_container`：没有可用选项或参数。

```console
$ ros2 run rclcpp_components component_container
```

`component_container_mt`：使用由 4 个线程组成的 `MultiThreadedExecutor`，可通过 `thread_num` 参数指定线程数。

```console
$ ros2 run rclcpp_components component_container_mt --ros-args -p thread_num:=4
```

`component_container_isolated`：为每个组件单独使用执行器。`--use_multi_threaded_executor` 将各组件使用的执行器指定为 `MultiThreadedExecutor`。

```console
$ ros2 run rclcpp_components component_container_isolated --use_multi_threaded_executor
```

<span id="unloading-components"></span>

### 卸载组件

在第一个终端启动容器：

```console
$ ros2 run rclcpp_components component_container
```

通过命令确认容器正在运行：

```console
$ ros2 component list
/ComponentManager
```

在第二个终端加载 talker 和 listener：

```console
$ ros2 component load /ComponentManager composition composition::Talker
Loaded component 1 into '/ComponentManager' container node as '/talker'
$ ros2 component load /ComponentManager composition composition::Listener
Loaded component 2 into '/ComponentManager' container node as '/listener'
```

加载时会打印组件唯一 ID，也可通过列出组件查看所有 ID：

```console
$ ros2 component list
/ComponentManager
  1  /talker
  2  /listener
```

使用唯一 ID 卸载组件：

```console
$ ros2 component unload /ComponentManager 1 2
Unloaded component 1 from '/ComponentManager' container
Unloaded component 2 from '/ComponentManager' container
```

确认第一个终端中 talker 和 listener 的重复输出已停止。

<span id="remapping-container-name-and-namespace"></span>

### 重映射容器名称和命名空间

组件管理器的名称和命名空间可以通过标准命令行参数重映射：

```console
$ ros2 run rclcpp_components component_container --ros-args -r __node:=MyContainer -r __ns:=/ns
```

在第二个终端使用新的容器名加载组件：

```console
$ ros2 component load /ns/MyContainer composition composition::Listener
```

> 容器命名空间的重映射不会影响已加载组件的命名空间。

<span id="remap-component-names-and-namespaces"></span>

### 重映射组件名称和命名空间

可通过加载命令的参数修改组件名和命名空间。先在第一个终端启动容器：

```console
$ ros2 run rclcpp_components component_container
```

重映射节点名：

```console
$ ros2 component load /ComponentManager composition composition::Talker --node-name talker2
```

重映射命名空间：

```console
$ ros2 component load /ComponentManager composition composition::Talker --node-namespace /ns
```

同时重映射两者：

```console
$ ros2 component load /ComponentManager composition composition::Talker --node-name talker3 --node-namespace /ns2
```

再使用 `ros2` 查看：

```console
$ ros2 component list
/ComponentManager
   1  /talker2
   2  /ns/talker
   3  /ns2/talker3
```

> 容器命名空间的重映射不会影响已加载组件的命名空间。

<span id="passing-parameter-values-into-components"></span>

### 向组件传递参数值

`ros2 component load` 支持在构造节点时传入任意参数，例如：

```console
$ ros2 component load /ComponentManager image_tools image_tools::Cam2Image -p burger_mode:=true
$ ros2 run rqt_image_view rqt_image_view  # Shows burgers bouncing, instead of image from camera
```

<span id="passing-additional-arguments-into-components"></span>

### 向组件传递额外选项

`ros2 component load` 也支持向组件管理器传递特定选项，供构造节点时使用。下面展示 `use_intra_process_comms` 和 `forward_global_arguments`：

```console
$ ros2 component load /ComponentManager composition composition::Talker -e use_intra_process_comms:=true -e forward_global_arguments:=false
```

<span id="id1"></span>

支持的额外选项如下：

| 选项 | 类型 | 默认值 | 说明 |
| --- | --- | --- | --- |
| `forward_global_arguments` | 布尔值 | True | 加载时，将全局参数应用于组件节点。 |
| `use_intra_process_comms` | 布尔值 | False | 在组件节点中启用进程内通信。 |

<span id="composable-nodes-as-shared-libraries"></span>

## 将可组合节点作为共享库

如果希望从软件包导出可组合节点共享库，并在另一个采用链接时组合的软件包中使用，应在 CMake 文件中添加代码，让下游包能够导入实际目标，再安装并导出生成的文件。

实际示例见 [ROS Discourse：Ament 共享库最佳实践](https://discourse.openrobotics.org/t/ament-best-practice-for-sharing-libraries/3602)。

<span id="composing-non-node-derived-components"></span>

## 组合未继承 Node 的组件

ROS 2 组件可以提高系统资源利用率，并支持创建不绑定特定节点的可复用功能。未继承 Node 的功能也可以作为独立可执行程序或共享库，按需加载到 ROS 系统中。

创建此类组件时，应遵循：

1. 实现接收 `const rclcpp::NodeOptions&` 的构造函数。
2. 实现返回 `NodeBaseInterface::SharedPtr` 的 `get_node_base_interface()`。可以在构造函数中创建节点，并使用该节点的同名方法提供接口。

示例 [node_like_listener_component](https://github.com/ros2/demos/blob/rolling/composition/src/node_like_listener_component.cpp) 展示了不继承 Node、但监听 ROS 话题的组件。

更多信息参阅[相关讨论](https://github.com/ros2/rclcpp/issues/2110#issuecomment-1454228192)。
