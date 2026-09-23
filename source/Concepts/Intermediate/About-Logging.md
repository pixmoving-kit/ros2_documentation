<span id="logging-and-logger-configuration"></span>

# 日志与日志记录器配置

<span id="overview"></span>

## 概述

ROS 2 日志子系统可以将日志消息发送到多个位置，包括：

- 控制台（如果已连接）。
- 磁盘上的日志文件（如果有可用的本地存储）。
- ROS 2 网络上的 `/rosout` 话题。

默认情况下，ROS 2 节点的日志同时输出到控制台（stderr）、磁盘日志文件和 `/rosout` 话题。每个节点都可以单独启用或禁用其中任意一种输出。

下文介绍日志子系统的相关概念。

<span id="severity-level"></span>

## 严重程度级别

日志消息按严重程度从低到高分为 `DEBUG`、`INFO`、`WARN`、`ERROR` 和 `FATAL`。

日志记录器只处理严重程度大于或等于其指定级别的消息。

每个节点都关联一个日志记录器，其名称自动包含节点名称和命名空间。如果节点名称被外部重映射为不同于源代码定义的名称，这一变化也会反映在日志记录器名称中。也可以创建不属于节点、使用指定名称的日志记录器。

日志记录器名称具有层级关系。如果名为 `abc.def` 的记录器没有设置级别，它会使用父级 `abc` 的级别；如果父级也没有设置，就使用默认日志级别。当 `abc` 的级别发生变化时，其所有后代记录器（如 `abc.def`、`abc.ghi.jkl`）都会受到影响，除非它们已经显式设置了自己的级别。

<span id="apis"></span>

## API

以下是 ROS 2 日志基础设施面向用户的 API，按客户端库分类。

### C++

| 宏 | 行为 |
| --- | --- |
| `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}` | 每次执行到该行时，输出给定的 printf 风格消息。 |
| `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_ONCE` | 仅第一次执行到该行时输出 printf 风格消息。 |
| `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_EXPRESSION` | 仅给定表达式为真时输出 printf 风格消息。 |
| `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_FUNCTION` | 仅给定函数返回真时输出 printf 风格消息。 |
| `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_SKIPFIRST` | 跳过第一次执行，之后每次执行到该行时输出 printf 风格消息。 |
| `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_THROTTLE` | 按给定的整数毫秒间隔限制 printf 风格消息的输出频率。 |
| `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_SKIPFIRST_THROTTLE` | 跳过第一次输出，并按给定的整数毫秒间隔限制 printf 风格消息的输出频率。 |
| `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_STREAM` | 每次执行到该行时，输出给定的 C++ 流风格消息。 |
| `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_STREAM_ONCE` | 仅第一次执行到该行时输出 C++ 流风格消息。 |
| `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_STREAM_EXPRESSION` | 仅给定表达式为真时输出 C++ 流风格消息。 |
| `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_STREAM_FUNCTION` | 仅给定函数返回真时输出 C++ 流风格消息。 |
| `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_STREAM_SKIPFIRST` | 跳过第一次执行，之后每次执行到该行时输出 C++ 流风格消息。 |
| `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_STREAM_THROTTLE` | 按给定的整数毫秒间隔限制 C++ 流风格消息的输出频率。 |
| `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_STREAM_SKIPFIRST_THROTTLE` | 跳过第一次输出，并按给定的整数毫秒间隔限制 C++ 流风格消息的输出频率。 |

上述 API 的第一个参数都是 `rclcpp::Logger` 对象。推荐调用 `node->get_logger()` 从节点 API 获取，也可以单独构造一个 `rclcpp::Logger` 对象。

- `rcutils_logging_set_logger_level`：将指定名称的日志记录器设置为给定的严重程度级别。
- `rcutils_logging_get_logger_effective_level`：根据日志记录器名称返回其日志级别（该级别可能未被设置）。

### Python

`logger.{debug,info,warning,error,fatal}` 将给定的 Python 字符串输出到日志基础设施。可通过以下关键字参数控制行为：

- `throttle_duration_sec`：若不为 `None`，则以浮点秒数指定限频间隔。
- `skip_first`：若为 `True`，则跳过第一次执行，之后每次执行到该行时输出消息。
- `once`：若为 `True`，则只在第一次执行到该行时输出消息。

另外还提供：

- `rclpy.logging.set_logger_level`：将指定名称的日志记录器设置为给定的严重程度级别。
- `rclpy.logging.get_logger_effective_level`：根据日志记录器名称返回其日志级别（该级别可能未被设置）。

<span id="configuration"></span>

## 配置

`rclcpp` 和 `rclpy` 使用相同的底层日志基础设施，因此配置选项也相同。

<span id="environment-variables"></span>

### 环境变量

以下环境变量用于控制 ROS 2 日志记录器的行为。这些设置均作用于整个进程，会影响进程中的所有节点。

- `ROS_LOG_DIR`：指定将日志写入磁盘时使用的目录。若非空，直接使用指定目录；若为空，使用 `ROS_HOME` 环境变量构造形如 `$ROS_HOME/.log` 的路径。无论哪种情况，`~` 都会展开为用户的主目录。
- `ROS_HOME`：指定日志、配置文件等 ROS 文件所使用的主目录。对日志而言，它用于构造日志文件目录的路径。若非空，就使用该变量的值作为 ROS_HOME 路径。`~` 会展开为用户的主目录。
- `RCUTILS_LOGGING_USE_STDOUT`：控制日志输出流。未设置或为 `0` 时使用 stderr；为 `1` 时使用 stdout。
- `RCUTILS_LOGGING_BUFFERED_STREAM`：控制 `RCUTILS_LOGGING_USE_STDOUT` 指定的日志流是否按行缓冲。未设置时使用流的默认行为（stdout 通常按行缓冲，stderr 通常不缓冲）；为 `0` 时强制不缓冲；为 `1` 时强制按行缓冲。
- `RCUTILS_COLORIZED_OUTPUT`：控制输出是否使用颜色。未设置时，根据平台和控制台是否为 TTY 自动决定；为 `0` 时强制关闭颜色；为 `1` 时强制开启颜色。
- `RCUTILS_CONSOLE_OUTPUT_FORMAT`：控制每条日志消息输出的字段，可用字段见下表。

| 字段 | 含义 |
| --- | --- |
| `{severity}` | 严重程度级别。 |
| `{name}` | 日志记录器名称，可能为空。 |
| `{message}` | 日志消息，可能为空。 |
| `{function_name}` | 发起日志调用的函数名称，可能为空。 |
| `{file_name}` | 发起日志调用的文件名称，可能为空。 |
| `{time}` | 自纪元起经过的秒数。 |
| `{time_as_nanoseconds}` | 自纪元起经过的纳秒数。 |
| `{line_number}` | 发起日志调用的行号，可能为空。 |

未指定格式时，默认使用 `[{severity}] [{time}] [{name}]: {message}`。

<span id="node-creation"></span>

### 创建节点

初始化 ROS 2 节点时，可以通过节点选项控制部分日志行为。这些选项按节点生效，因此即使多个节点组合在同一进程中，也可以采用不同设置。

- `log_levels`：指定该节点中特定组件使用的日志级别。例如：`ros2 run demo_nodes_cpp talker --ros-args --log-level talker:=DEBUG`。
- `external_log_config_file`：指定配置后端日志记录器的外部文件。若为 NULL，使用默认配置。文件格式由后端决定；默认的 spdlog 后端目前尚未实现此功能。例如：`ros2 run demo_nodes_cpp talker --ros-args --log-config-file log-config.txt`。
- `log_stdout_disabled`：是否禁止将日志写入控制台。例如：`ros2 run demo_nodes_cpp talker --ros-args --disable-stdout-logs`。
- `log_rosout_disabled`：是否禁止将日志发送到 `/rosout`。禁用可显著节省网络带宽，但外部观察者将无法监控日志。例如：`ros2 run demo_nodes_cpp talker --ros-args --disable-rosout-logs`。
- `log_ext_lib_disabled`：是否完全禁用外部日志库。在某些情况下这样会更快，但日志将不会写入磁盘。例如：`ros2 run demo_nodes_cpp talker --ros-args --disable-external-lib-logs`。

<span id="logging-subsystem-design"></span>

## 日志子系统设计

下图展示日志子系统的五个主要部分及其交互方式。

![ROS 2 日志架构](../images/ros2_logging_architecture.png)

<span id="rcutils"></span>

### rcutils

`rcutils` 提供日志实现，可按照指定格式（参见前文[配置](#configuration)）格式化日志消息，并将其输出到控制台。它本身提供完整的日志方案，同时允许更高层组件以依赖注入方式接入日志基础设施。下文介绍 `rcl` 层时会进一步说明。

这是一个*进程级*日志实现，因此在这一层进行的任何配置都会影响整个进程，而不只是某个节点。

<span id="rcl-logging-spdlog"></span>

### rcl_logging_spdlog

`rcl_logging_spdlog` 实现 `rcl_logging_interface` API，为 `rcl` 层提供外部日志服务。它使用 `spdlog` 库，将格式化后的日志消息写入磁盘文件，通常位于 `~/.ros/log`，也可以通过前文的配置修改。

<span id="rcl"></span>

### rcl

`rcl` 中的日志子系统通过 `rcutils` 和 `rcl_logging_spdlog` 提供 ROS 2 的主要日志服务。当日志消息到达时，由 `rcl` 决定将其发送到哪里。主要有三个输出位置，每个节点都可以任意组合启用：

- 通过 `rcutils` 层输出到控制台。
- 通过 `rcl_logging_spdlog` 层写入磁盘。
- 通过 RMW 层发送到 ROS 2 网络上的 `/rosout` 话题。

<span id="rclcpp"></span>

### rclcpp

`rclcpp` 是位于 `rcl` API 之上的主要 ROS 2 C++ API。在日志方面，它提供 `RCLCPP_` 系列宏，完整列表见前文 [API](#apis)。调用这些宏时，会比较宏的严重程度级别和节点当前的级别。若宏的级别大于或等于节点级别，就格式化消息，并输出到当前配置的全部位置。`rclcpp` 对日志调用使用全局互斥锁，因此同一进程中的所有日志调用最终都会串行执行。

<span id="rclpy"></span>

### rclpy

`rclpy` 是位于 `rcl` API 之上的主要 ROS 2 Python API。在日志方面，它提供 `logger.debug` 等函数，完整列表见前文 [API](#apis)。调用这些函数时，会比较该日志调用的严重程度级别和节点当前的级别。若调用级别大于或等于节点级别，就格式化消息，并输出到当前配置的全部位置。

<span id="logging-usage"></span>

## 日志使用示例

### C++

- [rclcpp 日志演示](https://github.com/ros2/demos/tree/rolling/logging_demo)提供了一些简单示例。
- [日志演示教程](../../Tutorials/Demos/Logging-and-logger-configuration.md)介绍具体用法。
- [rclcpp 文档](https://docs.ros2.org/latest/api/rclcpp/logging_8hpp.html)列出了更完整的功能。

### Python

- [rclpy 示例](https://github.com/ros2/examples/blob/rolling/rclpy/services/minimal_client/examples_rclpy_minimal_client/client.py)展示节点日志记录器的用法。
- [rclpy 测试](https://github.com/ros2/rclpy/blob/rolling/rclpy/test/test_logging.py)展示 `skip_first`、`once` 等关键字参数的用法。
