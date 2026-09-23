---
translation_status: machine_translated
source: Concepts/Basic/About-Parameters.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="parameters"></span>

# 参数

<span id="overview"></span>

## 概述

ROS 2 中的参数与单个节点相关。 参数用于在启动时( 运行期间) 配置节点, 而不会改变代码。 参数的寿命与节点的寿命挂钩( 尽管节点可以执行某种持久性, 以便在重新启动后重新加载值 ) 。

参数通过节点名称、节点名称空间、参数名称和参数名称空间处理。提供参数名称空间是可选的。

每个参数由一个键、一个值和一个描述符组成。键是字符串,值是以下类型之一: `bool`, `int64`, `float64`, `string`, `byte[]`, `bool[]`, `int64[]`, `float64[]` 或 时 间 `string[]`。默认情况下,所有描述符都是空的,但可以包含参数描述,值范围,类型信息,以及额外的限制.

包含 ROS 参数的实践教程请参见 [理解参数](../../Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md).

<span id="parameters-background"></span>

## 参数背景

<span id="declaring-parameters"></span>

### 宣告参数

默认情况下,节点需要 *声明* 它在生命期内会接受的所有参数。 这使得参数的类型和名称在节点启动时间得到很好的定义, 从而减少以后错误配置的可能性 。 见 : [在类中使用参数（C++）](../../Tutorials/Beginner-Client-Libraries/Using-Parameters-In-A-Class-CPP.md) 或 时 间 [在类中使用参数（Python）](../../Tutorials/Beginner-Client-Libraries/Using-Parameters-In-A-Class-Python.md) 用于关于从节点声明和使用参数的教程。

对于某些类型的节点,并非所有的参数都会被提前知道。在这种情况下,节点可以被即时的 `allow_undeclared_parameters` 设置为 `true`,这样可以让参数在节点上得到和设定,即使它们还没有被宣布。

<span id="parameter-types"></span>

### 参数类型

ROS 2 节点上的每个参数都有一个在 Overview 中提及的预定义的参数类型。默认情况下,在运行时更改已声明参数类型的尝试将失败。这可以防止常见的错误,比如将布尔值放入整数参数中。

如果一个参数需要多个不同类型,而使用该参数的代码可以处理它,则这种默认行为可以改变。当参数被宣布时,应当使用一个参数来宣布它。 `ParameterDescriptor` 与 `dynamic_typing` 成员变量设置为 `true`.

<span id="parameter-callbacks"></span>

### 参数召回

一个ROS 2节点可以注册两种不同类型的回调,以便在参数发生变化时被告知. 两个回调都是可选的.

第一种称为“设定参数”召回,可以通过调用设置 `add_on_set_parameters_callback` 从节点 API 中。 调用通过不可更改的列表 `Parameter` 对象,并返回 `rcl_interfaces/msg/SetParametersResult`。这种回调的主要目的是让用户能够检查即将到来的参数更改,并明确拒绝更改。

> **说明**
>
> 重要的是“设置参数”召回没有副作用。 由于多个“设置参数”召回可以连锁, 单个召回者无法知道后一位召回者是否会拒绝更新。 如果单个召回者要修改它所在的类别, 例如, 它可能会与实际的参数同步。 要获得召回, 将无法调回 。 *之后* a 参数已成功更改,请见下文下一类调用。

第二类回调被称为“在参数上的事件”回调,可以通过调用设置 `on_parameter_event` 从参数客户端 APIs 中调用。 `rcl_interfaces/msg/ParameterEvent` 对象,则不返回任何内容。在输入事件的所有参数被宣布、更改或删除后,将调用此调用。此调用的主要目的是使用户能够对已成功接受的参数的更改作出反应。

<span id="interacting-with-parameters"></span>

## 与参数的交互

ROS 2节点可以通过节点API来进行参数操作,如上所述. [在类中使用参数（C++）](../../Tutorials/Beginner-Client-Libraries/Using-Parameters-In-A-Class-CPP.md) 或 时 间 [在类中使用参数（Python）](../../Tutorials/Beginner-Client-Libraries/Using-Parameters-In-A-Class-Python.md)。外部进程可以通过一个节点被即时设定时默认创建的参数服务执行参数操作。默认创建的服务是:

- `/node_name/describe_parameters`: 使用服务类型: `rcl_interfaces/srv/DescribeParameters`。在参数名称列表中,返回与参数相关的描述符列表。

- `/node_name/get_parameter_types`: 使用服务类型: `rcl_interfaces/srv/GetParameterTypes`。如果给出参数名称列表,则返回与参数相关的参数类型列表。

- `/node_name/get_parameters`: 使用服务类型: `rcl_interfaces/srv/GetParameters`。在参数名称列表中,返回与参数相关的参数值列表。

- `/node_name/list_parameters`: 使用服务类型: `rcl_interfaces/srv/ListParameters`。考虑到参数前缀的可选列表,返回带有该前缀的现有参数列表。如果前缀是空的,则返回所有参数。

- `/node_name/set_parameters`: 使用服务类型: `rcl_interfaces/srv/SetParameters`。鉴于参数名称和值的列表,试图在节点上设置参数。返回一个试图设置每个参数的结果列表;其中一些可能已经成功,有些可能已经失败。

- `/node_name/set_parameters_atomically`: 使用服务类型: `rcl_interfaces/srv/SetParametersAtomically`。鉴于参数名称和值的列表,试图在节点上设置参数。尝试设置所有参数后返回一个单一结果,因此如果一个参数失败,所有参数都失败。

<span id="setting-initial-parameter-values-when-running-a-node"></span>

## 运行节点时设置初始参数值

在运行节点时,可以通过单个命令行参数或YAML文件设定初始参数值。见 [直接从命令行设置参数](../../How-To-Guides/Node-arguments.md#nodeargsparameters) 用于示例,说明如何设置初始参数值。

<span id="setting-initial-parameter-values-when-launching-nodes"></span>

## 启动节点时设置初始参数值

在通过ROS 2发射设施运行节点时也可以设定初始参数值. See. [本文](../../Tutorials/Intermediate/Launch/Using-ROS2-Launch-For-Large-Projects.md) 关于如何通过发射指定参数的信息。

<span id="manipulating-parameter-values-at-runtime"></span>

## 运行时操纵参数值

那个... `ros2 param` 命令是和已经运行的节点参数进行交互的一般方式。 `ros2 param` 使用上述参数服务 API 来进行各种操作。参见 [此向导](../../How-To-Guides/Using-ros2-param.md) 关于如何使用的详细信息 `ros2 param`.

<span id="migrating-from-ros-1"></span>

## 从ROS 1 移走

那个... [启动文件迁移指南](../../How-To-Guides/Migrating-from-ROS1/Migrating-Launch-Files.md) 解释如何迁移 `param` 财务报告和财务报告 `rosparam` 从ROS 1到ROS 2的发射标记.

那个... [移徙指南](../../How-To-Guides/Migrating-from-ROS1/Migrating-Parameters.md) 解释如何将参数从ROS 1迁移到ROS 2.
