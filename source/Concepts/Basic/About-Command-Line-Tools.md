<span id="introspection-with-command-line-tools"></span>

# 使用命令行工具进行内省

ROS 2 提供了一组命令行工具，用于查看和检查 ROS 2 系统的运行状态。

<span id="usage"></span>

## 用法

这些工具的主入口是 `ros2` 命令。它包含多个子命令，用于检查和操作节点、话题、服务等实体。

查看所有可用子命令：

```console
$ ros2 --help
```

可用的子命令包括：

- `action`：检查 ROS 动作并与之交互。
- `bag`：录制或回放 rosbag。
- `component`：管理组件容器。
- `daemon`：检查或配置 ROS 2 守护进程。
- `doctor`：检查 ROS 配置中的潜在问题。
- `interface`：显示 ROS 接口信息。
- `launch`：运行或检查启动文件。
- `lifecycle`：检查或管理具有受控生命周期的节点。
- `multicast`：多播调试命令。
- `node`：检查 ROS 节点。
- `param`：检查或配置节点参数。
- `pkg`：检查 ROS 软件包。
- `plugin`：检查 ROS 插件。
- `run`：运行 ROS 节点。
- `security`：配置安全设置。
- `service`：检查或调用 ROS 服务。
- `test`：运行 ROS 启动测试。
- `topic`：检查 ROS 话题或发布消息。
- `trace`：追踪节点执行情况，仅在 Linux 上可用。
- `wtf`：`doctor` 的别名。

<span id="example"></span>

## 示例

可以使用 `topic` 子命令向话题发布消息并显示收到的消息，从而通过命令行工具实现典型的 talker-listener 示例。

在一个终端中发布消息：

```console
$ ros2 topic pub /chatter std_msgs/msg/String "data: Hello world"
publisher: beginning loop
publishing #1: std_msgs.msg.String(data='Hello world')

publishing #2: std_msgs.msg.String(data='Hello world')
```

在另一个终端中显示收到的消息：

```console
$ ros2 topic echo /chatter
data: Hello world

data: Hello world
```

<span id="ros-2-daemon-background-discovery-service"></span>

## ROS 2 守护进程：后台发现服务

ROS 2 通过分布式发现机制让节点相互连接。
这一机制有意避免使用集中式发现，因此节点可能需要一段时间才能发现 ROS 图中的所有其他参与者。
为此，ROS 2 运行一个后台守护进程来维护 ROS 图信息，从而更快地响应节点名称列表等查询。

首次使用 `ros2 node list`、`ros2 topic list` 等内省命令时，ROS 2 守护进程会自动启动。
如果当前没有运行中的守护进程，工具会先在后台创建一个，再执行请求的命令。

守护进程通过本地主机网络接口（127.0.0.1）通信，并使用 [ROS_DOMAIN_ID](../Intermediate/About-Domain-ID.md) 环境变量的值作为端口号偏移量。
因此，要控制某个特定守护进程实例，例如运行 `ros2 daemon stop`，必须确保当前的 `ROS_DOMAIN_ID` 与该实例使用的域 ID 一致。
不同的 `ROS_DOMAIN_ID` 值对应不同端口上的独立守护进程实例。

运行 `ros2 daemon --help`，可以查看启动、停止、查询状态等更多操作。

<span id="running-the-daemon-in-the-foreground"></span>

### 在前台运行守护进程

调试时，可以让 ROS 2 守护进程在前台运行，将输出直接打印到标准输出和标准错误。
使用守护进程自身的入口命令 `_ros2_daemon` 即可实现：

```console
$ _ros2_daemon --ros-domain-id 0 --rmw-implementation rmw_fastrtps_cpp
```

这样启动的进程不会转入后台，因此可以实时观察发现活动和 XML-RPC 请求。
请根据实际配置调整 `--ros-domain-id` 和 `--rmw-implementation` 的值。

!!! note "说明"

    在前台启动守护进程之前，先用 `ros2 daemon stop` 停止已有实例，避免端口冲突。

<span id="implementation"></span>

## 实现

`ros2` 命令的源码位于 <https://github.com/ros2/ros2cli>。

`ros2` 工具采用可通过插件扩展的框架。
例如，[sros2](https://github.com/ros2/sros2) 软件包提供了 `security` 子命令。安装该软件包后，`ros2` 会自动发现这个子命令。
