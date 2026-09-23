<span id="parameters"></span>
# 参数

<span id="overview"></span>
## 概述

ROS 2 中的参数属于各个节点。参数用于在节点启动时或运行过程中配置节点，无需修改代码。参数的生命周期与所属节点一致，不过节点可以自行实现持久化机制，在重启后重新加载参数值。

参数通过节点名称、节点命名空间、参数名称和参数命名空间来定位，其中参数命名空间是可选的。

每个参数由键、值和描述符组成。键是字符串，值可以是以下类型之一：`bool`、`int64`、`float64`、`string`、`byte[]`、`bool[]`、`int64[]`、`float64[]` 或 `string[]`。描述符默认为空，也可以包含参数说明、取值范围、类型信息和其他约束。

参数的动手实践教程见[理解 ROS 2 参数](../../Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md)。

<span id="parameters-background"></span>
## 参数基础

<span id="declaring-parameters"></span>
### 声明参数

默认情况下，节点需要**声明**其生命周期内可以接受的所有参数。这样，参数的类型和名称在节点启动时就已明确，有助于减少后续配置错误。有关在节点中声明和使用参数的教程，请参阅[在 C++ 类中使用参数](../../Tutorials/Beginner-Client-Libraries/Using-Parameters-In-A-Class-CPP.md)或[在 Python 类中使用参数](../../Tutorials/Beginner-Client-Libraries/Using-Parameters-In-A-Class-Python.md)。

某些节点无法预先确定所有参数。这种情况下，可以在创建节点实例时将 `allow_undeclared_parameters` 设为 `true`，允许获取和设置尚未声明的参数。

<span id="parameter-types"></span>
### 参数类型

ROS 2 节点中的每个参数都属于概述中列出的预定义类型。默认情况下，尝试在运行时改变已声明参数的类型会失败。这可以防止常见错误，例如把布尔值赋给整数参数。

如果某个参数需要支持多种类型，并且使用该参数的代码能够处理这些类型，就可以修改默认行为：声明参数时，使用一个将 `dynamic_typing` 成员变量设为 `true` 的 `ParameterDescriptor`。

<span id="parameter-callbacks"></span>
### 参数回调

ROS 2 节点可以注册两种回调，以便在参数发生变化时获得通知。这两种回调都是可选的。

第一种称为“设置参数”回调，通过节点 API 的 `add_on_set_parameters_callback` 注册。回调接收一个由不可变 `Parameter` 对象组成的列表，并返回 `rcl_interfaces/msg/SetParametersResult`。它的主要用途是让用户检查即将发生的参数变更，并能够明确拒绝这次变更。

!!! note "注意"
    “设置参数”回调不应产生副作用。多个此类回调可以串联执行，单个回调无法知道后续回调是否会拒绝更新。例如，如果某个回调修改了其所属类的状态，这个状态就可能与实际参数值不一致。若要在参数成功修改**之后**执行回调，请使用下面介绍的另一种回调。

第二种称为“参数事件”回调，通过参数客户端 API 的 `on_parameter_event` 注册。回调接收一个 `rcl_interfaces/msg/ParameterEvent` 对象，不返回任何值。输入事件中的所有参数完成声明、修改或删除后，才会调用此回调。它的主要用途是让用户对已经成功接受的参数变更作出响应。

<span id="interacting-with-parameters"></span>
## 与参数交互

ROS 2 节点可以通过节点 API 操作参数，具体见[在 C++ 类中使用参数](../../Tutorials/Beginner-Client-Libraries/Using-Parameters-In-A-Class-CPP.md)或[在 Python 类中使用参数](../../Tutorials/Beginner-Client-Libraries/Using-Parameters-In-A-Class-Python.md)。外部进程可以通过参数服务操作参数；这些服务默认在创建节点实例时创建，包括：

- `/node_name/describe_parameters`：服务类型为 `rcl_interfaces/srv/DescribeParameters`。给定参数名称列表，返回对应的参数描述符列表。
- `/node_name/get_parameter_types`：服务类型为 `rcl_interfaces/srv/GetParameterTypes`。给定参数名称列表，返回对应的参数类型列表。
- `/node_name/get_parameters`：服务类型为 `rcl_interfaces/srv/GetParameters`。给定参数名称列表，返回对应的参数值列表。
- `/node_name/list_parameters`：服务类型为 `rcl_interfaces/srv/ListParameters`。可选择提供参数前缀列表，返回具有这些前缀的可用参数列表；前缀为空时，返回所有参数。
- `/node_name/set_parameters`：服务类型为 `rcl_interfaces/srv/SetParameters`。给定参数名称和值的列表，尝试设置节点上的这些参数，并返回每个参数的设置结果。部分参数可能设置成功，其他参数可能失败。
- `/node_name/set_parameters_atomically`：服务类型为 `rcl_interfaces/srv/SetParametersAtomically`。给定参数名称和值的列表，尝试设置节点上的这些参数，并返回一个表示整体设置结果的值。只要有一个参数设置失败，所有参数的设置都会失败。

<span id="setting-initial-parameter-values-when-running-a-node"></span>
## 运行节点时设置参数初始值

运行节点时，可以通过单独的命令行参数或 YAML 文件设置参数初始值。示例见[从命令行直接设置参数](../../How-To-Guides/Node-arguments.md#nodeargsparameters)。

<span id="setting-initial-parameter-values-when-launching-nodes"></span>
## 通过 launch 启动节点时设置参数初始值

使用 ROS 2 的 launch 功能运行节点时，也可以设置参数初始值。如何通过 launch 指定参数，请参阅[在大型项目中使用 ROS 2 launch](../../Tutorials/Intermediate/Launch/Using-ROS2-Launch-For-Large-Projects.md)。

<span id="manipulating-parameter-values-at-runtime"></span>
## 在运行时操作参数值

`ros2 param` 命令是与运行中节点的参数交互的通用方式。它通过上述参数服务 API 执行各种操作。用法详见[使用 ros2 param](../../How-To-Guides/Using-ros2-param.md)。

<span id="migrating-from-ros-1"></span>
## 从 ROS 1 迁移

[Launch 文件迁移指南](../../How-To-Guides/Migrating-from-ROS1/Migrating-Launch-Files.md)介绍了如何将 ROS 1 launch 文件中的 `param` 和 `rosparam` 标签迁移到 ROS 2。

[参数迁移指南](../../How-To-Guides/Migrating-from-ROS1/Migrating-Parameters.md)介绍了如何将参数从 ROS 1 迁移到 ROS 2。
