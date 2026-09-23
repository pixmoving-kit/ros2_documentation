---
translation_status: machine_translated
source: Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Services/Understanding-ROS2-Services.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="understanding-services"></span> <span id="ros2services"></span>

# 理解服务

**目标：** 使用命令行工具学习ROS 2中的服务.

**教程级别：** 入门

**用时：** 10分钟

<span id="background"></span>

## 背景

服务是ROS图中节点的另一种通信方法. 服务基于调用和响应模式,而不是主题的发布者-订阅者模式. 虽然主题允许节点订阅数据流并获得持续更新,但服务只有在客户端特别调用时才会提供数据.

![](images/Service-SingleServiceClient.gif) ![](images/Service-MultipleServiceClient.gif) <span id="prerequisites"></span>

## 前提条件

本教程中提及的一些概念,例如 [节点](../Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md) 财务报告和财务报告 [话题](../Understanding-ROS2-Topics/Understanding-ROS2-Topics.md),在系列的以往教程中覆盖。

你需要那个... [龟兹包](../Introducing-Turtlesim/Introducing-Turtlesim.md).

与往常一样, [您打开的每个新终端](../Configuring-ROS2-Environment.md).

<span id="tasks"></span>

## 操作步骤

<span id="setup"></span>

### 1 设置

启动两个乌龟结点, `/turtlesim` 财务报告和财务报告 `/teleop_turtle`.

打开新的终端并运行 :

``` console
$ ros2 run turtlesim turtlesim_node
```

打开另一个终端并运行 :

``` console
$ ros2 run turtlesim turtle_teleop_key
```

<span id="ros2-service-list"></span>

### 2 ros2 服务列表

运行 `ros2 service list` 命令在新终端中将返回当前系统中所有正在运行的服务列表 :

``` console
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

你会看到两个节点都有同样的六个服务 `parameters` 。几乎所有 ROS 2 中的节点都有参数所构建的这些基础设施服务。下一个教程中将有更多关于参数的内容。在此教程中,讨论时将省略参数服务。

现在,让我们集中关注针对龟类的服务, `/clear`, `/kill`, `/reset`, `/spawn`, `/turtle1/set_pen`, `/turtle1/teleport_absolute`,以及 `/turtle1/teleport_relative`。您可能记得使用 rqt 在 [使用龟形、 ros2 和 rqt](../Introducing-Turtlesim/Introducing-Turtlesim.md) 教学。

<span id="ros2-service-type"></span>

### 3 ros2 服务类型

服务有类型来描述一个服务的请求和响应数据的结构. 服务类型的定义与主题类型相似,但服务类型有两个部分:一个是请求的信息,另一个是回应信息.

要找到服务的类型, 请使用命令 :

``` console
$ ros2 service type <service_name>
```

让我们看看龟兹的作品 `/clear` 服务。在新的终端中,输入命令:

``` console
$ ros2 service type /clear
std_srvs/srv/Empty
```

那个... `Empty` type 表示服务调用在请求时不发送数据,在收到回复时不接收数据.

<span id="ros2-service-list-t"></span>

#### 3.1 ros2 服务列表 - t

要同时看到所有活动服务的类型,您可以附加 `--show-types` 选项,缩写为 `-t`,改为: `list` 命令 :

``` console
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

### 4个 ros2 服务查找器

如果您想要找到特定类型的所有服务, 您可以使用命令 :

``` console
$ ros2 service find <type_name>
```

例如,你可以找到所有 `Empty` 像这样的输入服务 :

``` console
$ ros2 service find std_srvs/srv/Empty
/clear
/reset
```

<span id="ros2-interface-show"></span>

### 5 ros2 接口显示

您可以从命令行调用服务,但首先需要了解输入参数的结构.

``` console
$ ros2 interface show <type_name>
```

试试这个 `/clear` 服务类型, `Empty`:

``` console
$ ros2 interface show std_srvs/srv/Empty
---
```

那个... `---` 将请求结构(以上)与响应结构(以下)分开。但是,正如你先前所得知的那样, `Empty` 类型不会发送或接收任何数据。所以自然,其结构是空白的。

让我们回顾一下一个服务,它的类型是发送和接收数据,比如: `/spawn`。从结果来看 `ros2 service list -t`我们知道 `/spawn`其类型是: `turtlesim/srv/Spawn`.

以了解缔约国的请求和答复论点 `/spawn` 服务,运行命令 :

``` console
$ ros2 interface show turtlesim/srv/Spawn
float32 x
float32 y
float32 theta
string name # Optional.  A unique name will be created and returned if this is empty
---
string name
```

以上资料 `---` 线条告诉我们需要调用哪些参数 `/spawn`. `x`, `y` 财务报告和财务报告 `theta` 确定产卵海龟的2D姿势, `name` 很明显是可选的。

线下的信息并不是你需要了解的,

<span id="ros2-service-call"></span>

### 6 ros2 服务呼叫

既然您知道服务类型是什么,如何找到服务类型,以及如何找到该类型参数的结构,您可以使用:

``` console
$ ros2 service call <service_name> <service_type> <arguments>
```

那个... `<arguments>` 部分是可选的。例如,你知道, `Empty` 输入服务没有任何论据:

``` console
$ ros2 service call /clear std_srvs/srv/Empty
```

此命令会清除您所绘制的任何线条的龟图窗口 。

![](images/clear.png)

现在,让我们通过呼叫来产出一只新乌龟 `/spawn` 和设置参数。输入 `<arguments>` 在命令行发出的服务呼叫中,需要用YAML语法。

输入命令 :

``` console
$ ros2 service call /spawn turtlesim/srv/Spawn "{x: 2, y: 2, theta: 0.2, name: ''}"
requester: making request: turtlesim.srv.Spawn_Request(x=2.0, y=2.0, theta=0.2, name='')

response:
turtlesim.srv.Spawn_Response(name='turtle2')
```

您会得到这个方法式的视角, 了解正在发生的事情, 然后得到服务响应。

你的新产海龟的窗口会马上更新:

![](images/spawn.png) <span id="summary"></span>

## 小结

节点可以使用ROS 2. 不同的是,一个主题 - 一个节点发布信息,可以被一个或多个订阅者消费的一种方式的通信模式 - 一个服务是一个请求/响应模式,客户端向一个提供该服务的节点提出请求,服务处理请求并生成响应.

您通常不想使用服务进行连续通话; 话题甚至动作更合适 。

在此教程中, 您使用命令行工具来识别、 透视和调用服务 。

<span id="next-steps"></span>

## 后续步骤

在接下来的辅导中, [理解参数](../Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md),您将学习配置节点设置。

<span id="related-content"></span>

## 相关内容

检查出来 [此教程](https://discourse.ubuntu.com/t/call-services-in-ros-2/15261)使用机器人臂的ROS服务,
