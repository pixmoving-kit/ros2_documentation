---
translation_status: machine_translated
source: Concepts/Intermediate/About-Logging.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="logging-and-logger-configuration"></span>

# 日志与日志记录器配置

<span id="overview"></span>

## 概述

ROS 2中的伐木子系统旨在向各种目标发送伐木信息,包括:

- 给控制台(如果附着的话)

- 要登录磁盘上的文件( 如果本地存储可用)

- 给 `/rosout` ROS 2网络的专题

默认情况下,ROS 2 节点中的日志信息将输出到控制台(在stderr上),在磁盘上日志文件,并发送到 `/rosout` 在ROS 2 网络上的主题。所有目标都可以逐个启用或禁用。

这份文件的其余部分将讨论伐木子系统背后的一些想法。

<span id="severity-level"></span>

## 严重性水平

日志信息具有与之相关的严重性级别 : `DEBUG`, `INFO`, `WARN`, `ERROR` 或 时 间 `FATAL`,按上升顺序排列.

日志只处理严重性或高于为日志选择的指定关卡的日志信息。

每个节点都有一个与它相关的日志,自动包含节点的名称和命名空间。如果节点的名称被外部重绘到源代码定义以外的东西上,它将会在日志名称中反映出来。也可以创建使用特定名称的非节点日志。

logger 名称代表一个等级。 如果一个名为 “ abc. def” 的日志的级别未被设置, 则它会推迟到其母的级别, 并且如果该级别也被设置, 则会使用默认的日志级别。 当日志“ abc” 级别被更改时, 其所有子孙( 如 “ abc. def ” 、 “ abc. ghi.jkl ” ) 的级别都会受到影响, 除非其级别已被明确设定 。

<span id="apis"></span>

## APIs 辅助程序

这些是ROS 2 日志基础设施的终端用户应该使用的API,由客户端库分割.

##### C++

- `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}` - 每当此行被击中时, 都会输出给定的 printf 风格信件

- `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_ONCE` - 仅在此行第一次被击中时输出给定的 printf 风格信件

- `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_EXPRESSION` - 仅在给定的表达式属实时输出给定的 printf 风格信件

- `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_FUNCTION` - 仅在给定函数返回真实时输出给定的 printf 样式消息

- `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_SKIPFIRST` - 输出所有给定的 printf 样式信件, 但此行第一次被击中时除外

- `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_THROTTLE` - 以整数毫秒输出给定的 printf 风格信件

- `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_SKIPFIRST_THROTTLE` - 以整数毫秒输出给定的 printf 风格信件,但跳过第一个

- `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_STREAM` - 每次点击时输出给定的 C++ 流式消息

- `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_STREAM_ONCE` - 输出给定的 C++ 流式消息, 仅此行第一次被击中

- `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_STREAM_EXPRESSION` - 仅在给定的表达式属实的情况下输出给定的 C++ 流式消息

- `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_STREAM_FUNCTION` - 仅在给定函数返回真实时输出给定的 C++ 流式消息

- `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_STREAM_SKIPFIRST` - 输出给定的 C++ 流式消息, 但此行第一次被击中时除外

- `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_STREAM_THROTTLE` - 以整数毫秒输出给定的 C++ 流式消息

- `RCLCPP_{DEBUG,INFO,WARN,ERROR,FATAL}_STREAM_SKIPFIRST_THROTTLE` - 按整数毫秒输出给定的 C++ 流式消息,但跳过第一个

上面的每个API都有一个 `rclcpp::Logger` 对象作为第一个参数。可以通过调用来从节点 API 中调用此参数。 `node->get_logger()` (建议)或建造一个独立的 `rclcpp::Logger` 对象。

- `rcutils_logging_set_logger_level` - 将特定日志名称的日志级别设置到给定的严重程度级别

- `rcutils_logging_get_logger_effective_level` - 给定日志名称, 返回日志级别( 可能是未设置的)

##### Python

- `logger.{debug,info,warning,error,fatal}` - 将给定的 Python 字符串输出到日志基础设施。这些调用接受以下关键词参数来控制行为:

  - `throttle_duration_sec` - 如果不是,无,以浮动点秒表示节流阀间隔的长度

  - `skip_first` - 如果 True, 除了第一次被击中, 全部输出信件 。

  - `once` - 如果 True, 第一次点击此行时只输出消息

- `rclpy.logging.set_logger_level` - 将特定日志名称的日志级别设置到给定的严重程度级别

- `rclpy.logging.get_logger_effective_level` - 给定日志名称, 返回日志级别( 可能是未设置的)

<span id="configuration"></span>

## 配置

自兹 `rclcpp` 财务报告和财务报告 `rclpy` 使用相同的基础伐木基础设施,配置选项相同.

<span id="environment-variables"></span>

### 环境变量

以下环境变量控制 ROS 2 日志的一些方面。 对于每个环境设置,请注意这是一个全过程设置,因此适用于该过程中的所有节点。

- `ROS_LOG_DIR` - 控制用于将日志信件写入磁盘的日志目录( 如果启用的话) 。 如果非空, 请使用此变量中指定的精确目录 。 如果为空, 请使用其中的内容 。 `ROS_HOME` 构造窗体路径的环境变量 `$ROS_HOME/.log`。在所有情况下, `~` 字符扩展为用户的 Home 目录。

- `ROS_HOME` - 控制用于各种ROS文件的主目录,包括日志和配置文件。在日志的背景中,此变量用于构建日志文件目录的路径。如果不是空的,则使用该变量的内容进行ROS_HOME路径。在所有情况下, `~` 字符扩展为用户的家目录。

- `RCUTILS_LOGGING_USE_STDOUT` - 控制流输出消息。如果是未设置或0,请使用 stderr。如果是 1,请使用 stdout 。

- `RCUTILS_LOGGING_BUFFERED_STREAM` - 控制日志流(配置于 `RCUTILS_LOGGING_USE_STDOUT`)应该被线缓冲或未缓冲。如果未设置,请使用流的默认值(一般为 stdout 的线缓冲,而未缓冲为 stderr 的线缓冲)。如果是 0,则强迫流不被缓冲。如果是 1,则强迫流被线缓冲 。

- `RCUTILS_COLORIZED_OUTPUT` - 控制在输出消息时是否使用颜色。 如果未设定, 将自动确定基于平台的颜色以及控制台是否为 TTY 。 如果为 0, 强制禁用颜色来输出 。 如果 1, 强制禁用颜色来输出 。

- `RCUTILS_CONSOLE_OUTPUT_FORMAT` - 控制每个日志消息输出的字段。可用的字段有:

  - `{severity}` - 严重程度。

  - `{name}` - 日志的名称( 可能是空的) 。

  - `{message}` - 日志信息( 可能是空的) 。

  - `{function_name}` - 此函数名称来自( 可能是空的) 。

  - `{file_name}` - 这是从(可能是空的)调用的文件名 。

  - `{time}` - 从纪元开始的几秒钟。

  - `{time_as_nanoseconds}` - 纳米时间 从时代开始。

  - `{line_number}` - 这行号是从(可能是空的)调来的。

  如果没有给出格式,则默认为 `[{severity}] [{time}] [{name}]: {message}` 已使用。

<span id="node-creation"></span>

### 节点创建

初始化 ROS 2 节点时,可以通过节点选项控制行为的某些方面,因为这些是每个节点的选项,即使节点组成一个单一的过程,它们也可以被不同地设定为不同的节点.

- `log_levels` - 用于该特定节点内某个组件的日志级别。可设置如下: `ros2 run demo_nodes_cpp talker --ros-args --log-level talker:=DEBUG`

- `external_log_config_file` - 用于配置后端日志的外部文件。 如果它是 NULL, 则将使用默认配置。 请注意, 此文件的格式是后端特定格式( 且目前尚未执行 spdlog 的默认后端日志) 。 您可以设定如下 : `ros2 run demo_nodes_cpp talker --ros-args --log-config-file log-config.txt`

- `log_stdout_disabled` - 是否禁用向控制台写入日志消息。这可以通过下列方式实现: `ros2 run demo_nodes_cpp talker --ros-args --disable-stdout-logs`

- `log_rosout_disabled` - 是否禁用向外写入日志消息 `/rosout`。这可以大大节省网络带宽,但外部观察者将无法监视记录。这可以通过下列方法实现: `ros2 run demo_nodes_cpp talker --ros-args --disable-rosout-logs`

- `log_ext_lib_disabled` - 是否完全禁用外部记录器 。 在某些情况下, 这样做可能更快, 但意味着日志不会写入磁盘 。 可用下列方式实现 : `ros2 run demo_nodes_cpp talker --ros-args --disable-external-lib-logs`

<span id="logging-subsystem-design"></span>

## 日志子系统设计

下面的图像显示了伐木子系统中的五大块以及它们是如何相互作用的.

<figure class="align-center">
<a href="../images/ros2_logging_architecture.png"><img src="../images/ros2_logging_architecture.png" alt="ROS 2 伐木结构" /></a>
</figure>

<span id="rcutils"></span>

### rcutils 维基月球

`rcutils` 拥有一个可以按照特定格式格式格式化日志消息的日志执行(参见 `Configuration` 并把这些日志信息输出到控制台。 `rcutils` 执行完整的伐木解决方案,但允许更高层次的组件以依赖注入模式插入伐木基础设施。 `rcl` 层下。

注意,这是 *每个进程* 日志执行,所以在这个级别上配置的任何内容都会影响整个过程,而不仅仅是单个节点.

<span id="rcl-logging-spdlog"></span>

### rcl_logging_spdlog

`rcl_logging_spdlog` 执行 `rcl_logging_interface` API, 并因此提供外部记录服务。 `rcl` ,特别是, `rcl_logging_spdlog` 执行将格式化的日志信件写入磁盘上使用 `spdlog` 库,一般在 `~/.ros/log` (虽然这是可塑的,你看,) `Configuration` (见上文)。

<span id="rcl"></span>

### rcl (中文(简体) ).

登录子系统在 `rcl` 用途 `rcutils` 财务报告和财务报告 `rcl_logging_spdlog` 以提供 ROS 2 的大部分记录服务。当日志信息输入时, `rcl` 决定发送地点。 日志信息可发送到3个主要位置; 单个节点可能允许其组合 :

- 通过控制台 `rcutils` 层

- 通过磁盘 `rcl_logging_spdlog` 层

- 给 `/rosout` ROS 2 网络通过 RMW 层的专题

<span id="rclcpp"></span>

### rclcpp

这是主要ROS 2 C++ API ,它坐到 `rcl` API 在伐木方面, `rclcpp` 提供 `RCLCPP_` 日志宏; 参见 `APIs` 上方的完整列表。当其中之一 `RCLCPP_` 宏运行时,它会对照宏的重度水平检查节点当前的严重程度水平。如果宏的重度水平大于或等于节点严重程度水平,则消息将格式化,输出到当前配置的所有位置。请注意 `rclcpp` 使用全局的 mutex进行日志调用,所以同一过程中的所有日志调用最终都是单行的.

<span id="rclpy"></span>

### rclpy

这是主要ROS 2 Python API,它坐在顶端 `rcl` API 在伐木方面, `rclpy` 提供 `logger.debug`- 样式函数;见 `APIs` 上方的完整列表。当其中之一 `logger.debug` 函数运行时,它将检查节点当前的严重性水平与宏的严重程度水平。如果宏的严厉性水平大于或等于节点严重性水平,则消息将格式化,输出到当前配置的所有位置。

<span id="logging-usage"></span>

## 伐木使用量

##### C++

- 见 [rclcpp 日志演示](https://github.com/ros2/demos/tree/rolling/logging_demo) 对于一些简单的例子。

- 见 [日志演示](../../Tutorials/Demos/Logging-and-logger-configuration.md) 比如说用法。

- 见 [rclcpp 文档](https://docs.ros2.org/latest/api/rclcpp/logging_8hpp.html) 用于列出大量功能清单。

##### Python

- 见 [粗略实例](https://github.com/ros2/examples/blob/rolling/rclpy/services/minimal_client/examples_rclpy_minimal_client/client.py) 例如,使用节点的日志。

- 见 [rclpy 测试](https://github.com/ros2/rclpy/blob/rolling/rclpy/test/test_logging.py) 例如关键词参数的使用(例如: `skip_first`, `once`).
