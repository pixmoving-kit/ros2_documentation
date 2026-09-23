<span id="understanding-services"></span> <span id="ros2services"></span>
# 理解服务

**目标：** 使用命令行工具了解 ROS 2 服务。

**教程级别：** 初学者

**预计用时：** 10 分钟

<span id="background"></span>
## 背景

服务是 ROS 计算图中节点通信的另一种方式。话题采用发布/订阅模式，服务则采用调用/响应模式。通过话题，节点可以订阅数据流并持续获取更新；服务只在客户端明确发起调用时才提供数据。

![单个服务客户端](images/Service-SingleServiceClient.gif)

![多个服务客户端](images/Service-MultipleServiceClient.gif)

<span id="prerequisites"></span>
## 前提条件

本教程提到的[节点](../Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md)和[话题](../Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)等概念，已在本系列前面的教程中介绍。

需要安装 [turtlesim 软件包](../Introducing-Turtlesim/Introducing-Turtlesim.md)。

与往常一样，不要忘记在[每个新打开的终端](../Configuring-ROS2-Environment.md)中加载 ROS 2 环境。

<span id="tasks"></span>
## 操作步骤

<span id="setup"></span>
### 1 准备工作

启动 turtlesim 的两个节点：`/turtlesim` 和 `/teleop_turtle`。

打开新终端，运行：

```console
$ ros2 run turtlesim turtlesim_node
```

打开另一个终端，运行：

```console
$ ros2 run turtlesim turtle_teleop_key
```

<span id="ros2-service-list"></span>
### 2 ros2 service list

在新终端中运行 `ros2 service list`，会返回系统中当前所有活动服务的列表：

```console
$ ros2 service list
/clear
/kill
/reset
/spawn
/teleop_turtle/describe_parameters
/teleop_turtle/get_parameter_types
/teleop_turtle/get_parameters
/teleop_turtle/list_parameters
/teleop_turtle/set_parameters
/teleop_turtle/set_parameters_atomically
/turtle1/set_pen
/turtle1/teleport_absolute
/turtle1/teleport_relative
/turtlesim/describe_parameters
/turtlesim/get_parameter_types
/turtlesim/get_parameters
/turtlesim/list_parameters
/turtlesim/set_parameters
/turtlesim/set_parameters_atomically
```

可以看到，两个节点都有六个名称中包含 `parameters` 的同类服务。ROS 2 中几乎每个节点都提供这些基础服务，参数功能就建立在它们之上。下一篇教程会详细介绍参数，本教程暂不讨论这些参数服务。

现在重点关注 turtlesim 特有的服务：`/clear`、`/kill`、`/reset`、`/spawn`、`/turtle1/set_pen`、`/turtle1/teleport_absolute` 和 `/turtle1/teleport_relative`。在[使用 turtlesim、ros2 和 rqt](../Introducing-Turtlesim/Introducing-Turtlesim.md)教程中，你已经通过 rqt 使用过其中一些服务。

<span id="ros2-service-type"></span>
### 3 ros2 service type

服务类型描述了请求数据和响应数据的结构。它的定义方式与话题类型类似，但服务类型包含两部分：一条用于请求的消息，以及一条用于响应的消息。

用以下命令查看服务类型：

```console
$ ros2 service type <service_name>
```

以 turtlesim 的 `/clear` 服务为例，在新终端中输入：

```console
$ ros2 service type /clear
std_srvs/srv/Empty
```

`Empty` 类型表示调用服务时，请求不携带数据，响应也不携带数据。

<span id="ros2-service-list-t"></span>
#### 3.1 ros2 service list -t

要同时查看所有活动服务的类型，可以为 `list` 命令添加 `--show-types` 选项，缩写为 `-t`：

```console
$ ros2 service list -t
/clear [std_srvs/srv/Empty]
/kill [turtlesim/srv/Kill]
/reset [std_srvs/srv/Empty]
/spawn [turtlesim/srv/Spawn]
...
/turtle1/set_pen [turtlesim/srv/SetPen]
/turtle1/teleport_absolute [turtlesim/srv/TeleportAbsolute]
/turtle1/teleport_relative [turtlesim/srv/TeleportRelative]
...
```

<span id="ros2-service-find"></span>
### 4 ros2 service find

使用以下命令查找指定类型的所有服务：

```console
$ ros2 service find <type_name>
```

例如，查找所有 `Empty` 类型的服务：

```console
$ ros2 service find std_srvs/srv/Empty
/clear
/reset
```

<span id="ros2-interface-show"></span>
### 5 ros2 interface show

可以通过命令行调用服务，不过首先需要了解输入参数的结构：

```console
$ ros2 interface show <type_name>
```

对 `/clear` 服务的 `Empty` 类型试一下：

```console
$ ros2 interface show std_srvs/srv/Empty
---
```

`---` 将上方的请求结构和下方的响应结构分开。前面已经知道，`Empty` 类型不发送或接收任何数据，所以两部分都是空的。

接下来查看一个请求和响应都包含数据的服务，例如 `/spawn`。根据 `ros2 service list -t` 的输出，它的类型是 `turtlesim/srv/Spawn`。

运行以下命令，查看 `/spawn` 服务的请求和响应参数：

```console
$ ros2 interface show turtlesim/srv/Spawn
float32 x
float32 y
float32 theta
string name # Optional.  A unique name will be created and returned if this is empty
---
string name
```

`---` 上方列出了调用 `/spawn` 所需的参数。`x`、`y` 和 `theta` 决定新生成海龟的二维位姿，`name` 则是可选项。

当前调用不需要使用分隔线下方的信息，但这些信息有助于理解调用返回的响应数据类型。

<span id="ros2-service-call"></span>
### 6 ros2 service call

现在已经了解服务类型、查找服务类型的方法，以及查看类型参数结构的方法，可以用以下命令调用服务：

```console
$ ros2 service call <service_name> <service_type> <arguments>
```

`<arguments>` 是可选部分。例如，`Empty` 类型的服务不需要任何参数：

```console
$ ros2 service call /clear std_srvs/srv/Empty
```

该命令会清除 turtlesim 窗口中海龟画出的所有线条。

![清除绘制的轨迹](images/clear.png)

现在通过调用 `/spawn` 并指定参数，生成一只新海龟。通过命令行调用服务时，`<arguments>` 必须使用 YAML 语法。

输入以下命令：

```console
$ ros2 service call /spawn turtlesim/srv/Spawn "{x: 2, y: 2, theta: 0.2, name: ''}"
requester: making request: turtlesim.srv.Spawn_Request(x=2.0, y=2.0, theta=0.2, name='')

response:
turtlesim.srv.Spawn_Response(name='turtle2')
```

终端会先以类似方法调用的形式显示请求，再显示服务响应。

turtlesim 窗口会立即更新，显示新生成的海龟：

![通过服务生成新海龟](images/spawn.png)

<span id="summary"></span>
## 小结

ROS 2 节点可以通过服务通信。话题是单向通信模式，一个节点发布的信息可以由一个或多个订阅者接收；服务则是请求/响应模式，客户端向提供服务的节点发送请求，服务端处理请求并生成响应。

通常不应使用服务进行连续调用；话题，或某些情况下的动作，会更加合适。

本教程使用命令行工具查找、查看和调用了服务。

<span id="next-steps"></span>
## 后续步骤

下一篇教程[理解参数](../Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md)将介绍如何配置节点。

<span id="related-content"></span>
## 相关内容

[这篇教程](https://discourse.ubuntu.com/t/call-services-in-ros-2/15261)使用 Robotis 机械臂，展示了 ROS 服务在真实场景中的应用。
