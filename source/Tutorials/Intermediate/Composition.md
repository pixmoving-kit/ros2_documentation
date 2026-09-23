---
translation_status: machine_translated
source: Tutorials/Intermediate/Composition.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="composing-multiple-nodes-in-a-single-process"></span>

# 在单个进程中组合多个节点

**目标：** 将多个节点组成一个单一的过程.

**教程级别：** 中级

**用时：** 20分钟

<span id="background"></span>

## 背景

见 [概念性条款](../../Concepts/Intermediate/About-Composition.md).

关于如何写一个可堆肥的节点的信息, [检查此教程](Writing-a-Composable-Node.md).

<span id="prerequisites"></span>

## 前提条件

此教程使用可执行文件 [rclcpp_components](https://github.com/ros2/rclcpp/tree/rolling/rclcpp_components), [ros2 组件](https://github.com/ros2/ros2cli/tree/rolling/ros2component), [组成](https://github.com/ros2/demos/tree/rolling/composition),以及 [image_tools](https://github.com/ros2/demos/tree/rolling/image_tools) 软件包。如果您遵循了 [安装指令](../../Installation.md) 对于您的平台, 这些应该已经安装了 。

<span id="run-the-demos"></span>

## 运行演示

<span id="discover-available-components"></span>

### 发现可用的组件

查看工作空间中注册和可用的组件,在 shell 中执行以下内容:

``` console
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

### 使用 ROS 服务与出版商和订阅商的运行时间构成

在第一个外壳中,启动组件容器:

``` console
$ ros2 run rclcpp_components component_container
```

打开第二个外壳并核实容器是否通过 `ros2` 命令行工具。您应该看到组件的名称 :

``` console
$ ros2 component list
/ComponentManager
```

在第二个外壳中, 加载说话者组件( 见 [说话者](https://github.com/ros2/demos/blob/rolling/composition/src/talker_component.cpp) 源代码 。 命令将返回装入组件的独有ID以及节点名称 :

``` console
$ ros2 component load /ComponentManager composition composition::Talker
Loaded component 1 into '/ComponentManager' container node as '/talker'
```

现在第一个 shell 应该显示一个消息,即组件是加载的,以及用于发布一个消息的重复消息.

在第二个 shell 中运行另一个命令来装入听众组件( 见 [监听器](https://github.com/ros2/demos/blob/rolling/composition/src/listener_component.cpp) 源代码 :

``` console
$ ros2 component load /ComponentManager composition composition::Listener
Loaded component 2 into '/ComponentManager' container node as '/listener'
```

那个... `ros2` 命令行工具现在可用于检查容器状态 :

``` console
$ ros2 component list
/ComponentManager
   1  /talker
   2  /listener
```

现在第一个 shell 应该显示每个收到消息的重复输出 。

<span id="run-time-composition-using-ros-services-with-a-server-and-client"></span>

### 使用 ROS 服务器和客户端服务的运行时间构成

服务器和客户端的示例非常相似.

在第一弹壳中:

``` console
$ ros2 run rclcpp_components component_container
```

在第二个壳中(参见: [服务器](https://github.com/ros2/demos/blob/rolling/composition/src/server_component.cpp) 财务报告和财务报告 [客户端](https://github.com/ros2/demos/blob/rolling/composition/src/client_component.cpp) 源代码 :

``` console
$ ros2 component load /ComponentManager composition composition::Server
$ ros2 component load /ComponentManager composition composition::Client
```

在这种情况下,客户端会向服务器发送请求,服务器会以回复处理请求和回复,客户端会打印收到的回复.

<span id="compile-time-composition-with-hardcoded-nodes"></span>

### 以硬码节点编译时间构成

此演示显示,同样的共享库可以被重用来编译单个可执行程序运行多个组件而不使用ROS接口. 可执行程序包含上面所有四个组件: 谈话器和听众以及服务器和客户端,这在主函数中是硬编码的.

在外壳呼叫中( 见 [源代码](https://github.com/ros2/demos/blob/rolling/composition/src/manual_composition.cpp)):

``` console
$ ros2 run composition manual_composition
```

这应该显示来自双对、谈话者和听众以及服务器和客户端的重复信息。

> **说明**
>
> 手工编组的组件将不反映在 `ros2 component list` 命令行工具输出。

<span id="run-time-composition-using-dlopen"></span>

### 使用 dlopen 的运行时构成

此演示通过创建通用的容器过程, 并明确通过库来加载而不使用 ROS 接口, 呈现运行时间构成的替代方案。 进程将打开每个库, 在库中创建一个“ rclpp: Node” 类实例( ) 。[源代码](https://github.com/ros2/demos/blob/rolling/composition/src/dlopen_composition.cpp)).

##### Linux

``` console
$ ros2 run composition dlopen_composition `ros2 pkg prefix composition`/lib/libtalker_component.so `ros2 pkg prefix composition`/lib/liblistener_component.so
```

##### macOS

``` console
$ ros2 run composition dlopen_composition `ros2 pkg prefix composition`/lib/libtalker_component.dylib `ros2 pkg prefix composition`/lib/liblistener_component.dylib
```

##### Windows

``` console
$ ros2 pkg prefix composition
```

以获得配置设置的路径。然后调用

``` console
$ ros2 run composition dlopen_composition <path_to_composition_install>\bin\talker_component.dll <path_to_composition_install>\bin\listener_component.dll
```

现在, shell 应该显示每个发送和接收信件的重复输出 。

> **说明**
>
> dlopen-composed 组件将不会在 `ros2 component list` 命令行工具输出。

<span id="composition-using-launch-actions"></span>

### 使用发射行动的构成

虽然命令行工具对于调试和诊断组件配置很有用,但同时启动一组组件往往更方便。要实现此动作的自动化,我们可以使用一个 [发射文件](https://github.com/ros2/demos/blob/rolling/composition/launch/composition_demo.launch.py):

``` console
$ ros2 launch composition composition_demo.launch.py
```

<span id="advanced-topics"></span>

## 高级主题

现在,我们已经看到各组成部分的基本运作,我们可以讨论几个更先进的议题。

<span id="component-container-types"></span> <span id="componentcontainertypes"></span>

### 组件容器类型

一、导 言 [集装箱组件](../../Concepts/Intermediate/About-Composition.md#componentcontainer)中,可以选择最合适的组件容器类型。

- `component_container` (无选项/参数)

  > ``` console
  > $ ros2 run rclcpp_components component_container
  > ```

- `component_container_mt` 与 `MultiThreadedExecutor` 由4条线组成.  
  - `thread_num` 参数选项可以指定线程数 `MultiThreadedExecutor`.

  ``` console
  $ ros2 run rclcpp_components component_container_mt --ros-args -p thread_num:=4
  ```

- `component_container_isolated` 与 `MultiThreadedExecutor` 用于每个组件。  
  - `--use_multi_threaded_executor` 参数指定每个组件所用的执行器类型 `MultiThreadedExecutor`.

  ``` console
  $ ros2 run rclcpp_components component_container_isolated --use_multi_threaded_executor
  ```

<span id="unloading-components"></span>

### 卸载组件

在第一个外壳中,启动组件容器:

``` console
$ ros2 run rclcpp_components component_container
```

验证容器是否通过 `ros2` 命令行工具 :

``` console
$ ros2 component list
/ComponentManager
```

在第二枚炮弹中,我们像以前一样,把说话者和听众都装满:

``` console
$ ros2 component load /ComponentManager composition composition::Talker
Loaded component 1 into '/ComponentManager' container node as '/talker'
$ ros2 component load /ComponentManager composition composition::Listener
Loaded component 2 into '/ComponentManager' container node as '/listener'
```

组件的唯一ID在加载时会打印出来。 您也可以通过列出所有组件的唯一ID来获取它们, 因为这些组件已经加载 :

``` console
$ ros2 component list
/ComponentManager
  1  /talker
  2  /listener
```

使用独有的ID从组件容器中卸下组件.

``` console
$ ros2 component unload /ComponentManager 1 2
Unloaded component 1 from '/ComponentManager' container
Unloaded component 2 from '/ComponentManager' container
```

在第一个外壳中,验证来自说话者和听众的重复消息已经停止.

<span id="remapping-container-name-and-namespace"></span>

### 重映射容器名称和命名空间

组件管理器名称和命名空间可以通过标准命令行参数重映射 :

``` console
$ ros2 run rclcpp_components component_container --ros-args -r __node:=MyContainer -r __ns:=/ns
```

在第二个外壳中,组件可以通过使用更新的容器名称来加载:

``` console
$ ros2 component load /ns/MyContainer composition composition::Listener
```

> **说明**
>
> 容器的命名空间重新绘图不影响装入的组件。

<span id="remap-component-names-and-namespaces"></span>

### 重新绘制组件名称和命名空间

组件名称和命名空间可以通过参数调整到负载命令.

在第一个外壳中,启动组件容器:

``` console
$ ros2 run rclcpp_components component_container
```

如何重新绘制名称和命名空间的一些例子.

重新绘制节点名称 :

``` console
$ ros2 component load /ComponentManager composition composition::Talker --node-name talker2
```

重新绘制命名空间 :

``` console
$ ros2 component load /ComponentManager composition composition::Talker --node-namespace /ns
```

重新绘制两个:

``` console
$ ros2 component load /ComponentManager composition composition::Talker --node-name talker3 --node-namespace /ns2
```

现在使用 `ros2` 命令行工具栏 :

``` console
$ ros2 component list
/ComponentManager
   1  /talker2
   2  /ns/talker
   3  /ns2/talker3
```

> **说明**
>
> 容器的命名空间重新绘图不影响装入的组件。

<span id="passing-parameter-values-into-components"></span>

### 将参数值传递到组件中

那个... `ros2 component load` 命令行支持在构建时将任意参数传递到节点。此功能可使用如下:

``` console
$ ros2 component load /ComponentManager image_tools image_tools::Cam2Image -p burger_mode:=true
$ ros2 run rqt_image_view rqt_image_view  # Shows burgers bouncing, instead of image from camera
```

<span id="passing-additional-arguments-into-components"></span>

### 将更多论据传递到组件中

那个... `ros2 component load` 命令行支持将特定选项传递给组件管理器,以便在构建节点时使用。

以下示例显示额外参数的使用 `use_intra_process_comms` 财务报告和财务报告 `forward_global_arguments`:

``` console
$ ros2 component load /ComponentManager composition composition::Talker -e use_intra_process_comms:=true -e forward_global_arguments:=false
```

以下的额外参数得到支持.

<span id="id1"></span>

| 参数                       | 类型 | 默认 | 说明                              |
|----------------------------|------|------|-----------------------------------|
| `forward_global_arguments` | 布尔 | 真   | 在加载时对组件节点应用全局参数 。 |
| `use_intra_process_comms`  | 布尔 | 假   | 在组件节点中启用进程内部通信 。   |

组件管理器的额外参数 {.docutils .align-default}

<span id="composable-nodes-as-shared-libraries"></span>

## 作为共享库的可编译节点

如果您想从一个软件包中导出一个可共建的节点作为共享库,并在另一个进行链接时构成的软件包中使用该节点,请在导入下游软件包中实际目标的CMake文件中添加代码.

然后安装生成的文件并导出生成的文件.

从这里可以看到一个实际的例子: [ROS 论文 -- -- 共享库的最佳做法](https://discourse.openrobotics.org/t/ament-best-practice-for-sharing-libraries/3602)

<span id="composing-non-node-derived-components"></span>

## 编组非节点衍生组件

在ROS 2中,组件可以更有效地使用系统资源,并提供一个强大的功能,使您能够创建不与特定节点绑定的可重复使用的功能.

使用组件的一个优点是,它们允许您创建非节点衍生功能,作为独立的可执行文件或共享库,可以根据需要加载到ROS系统.

为了创建一个不是从节点中衍生出来的组件,遵循本准则: 1.

1.  执行一个需要的构造器 `const rclcpp::NodeOptions&` 作为它的论点。

2.  执行 `get_node_base_interface()` 方法,该方法应返回 a `NodeBaseInterface::SharedPtr`。可以使用 `get_node_base_interface()` 您在构建器中创建的节点提供此接口的方法。

以下是一个不来自节点的组件的例子, [node_like_listener_component](https://github.com/ros2/demos/blob/rolling/composition/src/node_like_listener_component.cpp).

关于此议题的更多信息,请参考: [讨论情况](https://github.com/ros2/rclcpp/issues/2110#issuecomment-1454228192).
