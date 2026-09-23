---
translation_status: machine_translated
source: Concepts/Basic/About-Command-Line-Tools.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="introspection-with-command-line-tools"></span>

# 使用命令行工具进行内省

ROS 2包括一套指令线工具,用于回顾ROS 2系统.

<span id="usage"></span>

## 使用量

工具的主要切入点是命令 `ros2`,它本身有各种子命令,用于回顾和与节点,主题,服务等合作.

查看所有可用的子命令运行 :

``` console
$ ros2 --help
```

可用子命令的例子包括:

- `action`: 内视/与ROS动作互动

- `bag`: 记录/玩一个罗什包

- `component`: 管理组件容器

- `daemon`: 内视/配置 ROS 2 守护进程

- `doctor`:检查ROS的设置以了解潜在的问题

- `interface`: 显示关于ROS接口的信息

- `launch`: 运行/检查发射文件

- `lifecycle`: 有管理寿命周期的内视/管理节点

- `multicast`: 多播调试命令

- `node`: 内视ROS节点

- `param`: 节点上的内视/配置参数

- `pkg`: 内视ROS软件包

- `plugin`: 内视ROS插件

- `run`: 运行 ROS 节点

- `security`: 配置安全设置

- `service`:内视/呼叫ROS服务

- `test`: 进行ROS发射测试

- `topic`:回顾/介绍ROS专题

- `trace`: 追踪工具以获取关于ROS节点执行的信息(仅在Linux上可用)

- `wtf`: 一个别名 `doctor`

<span id="example"></span>

## 示例

要使用命令行工具生成典型的谈话者-听众示例, `topic` 子命令可以用于发布和回声某个主题上的信息。

在一个终端发布信件,地址是:

``` console
$ ros2 topic pub /chatter std_msgs/msg/String "data: Hello world"
publisher: beginning loop
publishing #1: std_msgs.msg.String(data='Hello world')

publishing #2: std_msgs.msg.String(data='Hello world')
```

在另一个终端收到的回声信息有:

``` console
$ ros2 topic echo /chatter
data: Hello world

data: Hello world
```

<span id="ros-2-daemon-background-discovery-service"></span>

## ROS 2 守护进程: 背景发现服务

ROS 2使用分布式的发现过程来连接节点,由于这个过程目的不使用集中的发现机制,ROS节点需要时间才能发现ROS图中所有其他参与者. 为了解决这个问题,ROS 2运行一个背景守护进程,保存关于ROS图的信息,以提供更快的查询响应,如节点名称列表.

ROS 2 守护进程在您首先使用命令行工具时自动启动, 如 `ros2 node list`, `ros2 topic list`,或者其他反省命令。如果没有守护进程正在运行,这些工具将在执行请求的命令之前,在背景中即时执行一个新的守护进程。

守护进程使用本地主机网络接口( 127. 0.0.1) 进行通信, 并使用 [ROS_DOMAIN_ID](../Intermediate/About-Domain-ID.md) 环境变量作为端口数的偏移。这意味着如果您想要控制特定的守护进程实例(例如,使用 `ros2 daemon stop`),您必须保证您的 [ROS_DOMAIN_ID](../Intermediate/About-Domain-ID.md) 匹配守护进程所用的域 ID。 不同 [ROS_DOMAIN_ID](../Intermediate/About-Domain-ID.md) 值将导致在不同端口运行单独的守护进程。

你跑得开 `ros2 daemon --help` 用于与守护进程互动的更多选项,包括启动、停止或检查守护进程状态的命令。

<span id="running-the-daemon-in-the-foreground"></span>

### 在前景中运行守护进程

为了调试目的, 在前台运行 ROS 2 守护进程可以有用, 以便其输出直接打印到 stdout 和 stderr 。 您可以使用 `_ros2_daemon` 命令,这是守护进程本身的切入点:

``` console
$ _ros2_daemon --ros-domain-id 0 --rmw-implementation rmw_fastrtps_cpp
```

这将启动守护进程而不执行守护进程, 允许您实时观察所有发现活动和 XML- RPC 请求 。 替换 `--ros-domain-id` 财务报告和财务报告 `--rmw-implementation` 与您的设置相适应的值。

> **说明**
>
> 确保停止任何已存在的守护进程实例( Q)`ros2 daemon stop`),在前缘开始一个以避免港口冲突.

<span id="implementation"></span>

## 执行情况

源代码 : `ros2` 命令在 <https://github.com/ros2/ros2cli>.

那个... `ros2` 工具已作为框架执行,可通过插件扩展。例如, [斜线2](https://github.com/ros2/sros2) 软件包提供 `security` 自动检测到的子命令 `ros2` 工具,如果 `sros2` 软件包已安装。
