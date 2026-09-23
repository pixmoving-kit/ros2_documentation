---
translation_status: machine_translated
source: Tutorials/Demos/Logging-and-logger-configuration.rst
---

<span id="logging"></span>

# 日志

见 [日志页面](../../Concepts/Intermediate/About-Logging.md) 关于现有功能的详情。

<span id="using-log-statements-in-code"></span>

## 使用代码中的日志语句

<span id="basic-logging"></span>

### 基本伐木

以下代码将输出来自 ROS 2 节点的日志消息 。 `DEBUG` 严重性 :

##### C++

``` C++
// printf style
RCLCPP_DEBUG(node->get_logger(), "My log message %d", 4);

// C++ stream style
RCLCPP_DEBUG_STREAM(node->get_logger(), "My log message " << 4);
```

##### Python

``` python
node.get_logger().debug('My log message %d' % (4))
```

请注意,在这两种情况下,不增加一条后排新线,因为伐木基础设施将自动增加一条。

<span id="logging-only-the-first-time"></span>

### 只有第一次登录

以下代码将输出来自 ROS 2 节点的日志消息 。 `INFO` 严重性,但只是第一次被击中:

##### C++

``` C++
// printf style
RCLCPP_INFO_ONCE(node->get_logger(), "My log message %d", 4);

// C++ stream style
RCLCPP_INFO_STREAM_ONCE(node->get_logger(), "My log message " << 4);
```

##### Python

``` python
num = 4
node.get_logger().info(f'My log message {num}', once=True)
```

<span id="logging-all-but-the-first-time"></span>

### 除第一次外, 全部登录

以下代码将输出来自 ROS 2 节点的日志消息 。 `WARN` 重度,但不是第一次被击中:

##### C++

``` C++
// printf style
RCLCPP_WARN_SKIPFIRST(node->get_logger(), "My log message %d", 4);

// C++ stream style
RCLCPP_WARN_STREAM_SKIPFIRST(node->get_logger(), "My log message " << 4);
```

##### Python

``` python
num = 4
node.get_logger().warning('My log message {0}'.format(num), skip_first=True)
```

<span id="logging-throttled"></span>

### 日志被拖动

以下代码将输出来自 ROS 2 节点的日志消息 。 `ERROR` 严重性,但每秒不超过一次。

指定信件间毫秒的间隔参数应具有整数数据类型,以便转换为 `rcutils_duration_value_t` (单位:千美元) `int64_t`):

##### C++

``` C++
// printf style
RCLCPP_ERROR_THROTTLE(node->get_logger(), *node->get_clock(), 1000, "My log message %d", 4);

// C++ stream style
RCLCPP_ERROR_STREAM_THROTTLE(node->get_logger(), *node->get_clock(), 1000, "My log message " << 4);

// For now, use the nanoseconds() method to use an existing rclcpp::Duration value, see https://github.com/ros2/rclcpp/issues/1929
RCLCPP_ERROR_STREAM_THROTTLE(node->get_logger(), *node->get_clock(), msg_interval.nanoseconds()/1000000, "My log message " << 4);
```

##### Python

``` python
num = 4
node.get_logger().error(f'My log message {num}', throttle_duration_sec=1)
```

<span id="logging-throttled-all-but-the-first-time"></span>

### 除了第一次外, 记录都减速了

以下代码将输出来自 ROS 2 节点的日志消息 。 `DEBUG` 重度,每秒不超过一次,跳过它第一次被击中:

##### C++

``` C++
// printf style
RCLCPP_DEBUG_SKIPFIRST_THROTTLE(node->get_logger(), *node->get_clock(), 1000, "My log message %d", 4);

RCLCPP_DEBUG_SKIPFIRST_THROTTLE(node->get_logger(), *node->get_clock(), 1000, "My log message " << 4);
```

##### Python

``` python
num = 4
node.get_logger().debug(f'My log message {num}', skip_first=True, throttle_duration_sec=1.0)
```

<span id="logging-demo"></span>

## 日志演示

在这个里面 [演示](https://github.com/ros2/demos/tree/rolling/logging_demo),显示了不同类型的日志调用,并在当地和外部配置了不同日志的重度级别。

以 :

``` console
$ ros2 run logging_demo logging_demo_main
```

随着时间的推移,您将看到不同属性的日志调用输出。 要从头开始, 您将只看到日志调用输出的严重性 。 `INFO` 及以上(续)`WARN`, `ERROR`, `FATAL`。请注意,第一个信件将只记录一次,尽管每个迭代上都通了行,因为这是用于该信件的日志调用属性。

<span id="logging-directory-configuration"></span>

## 日志目录配置

日志目录可以通过两个环境变量进行配置: `ROS_LOG_DIR` 财务报告和财务报告 `ROS_HOME`。逻辑如下:

- 使用 `$ROS_LOG_DIR` 若为 `ROS_LOG_DIR` 已设定,而非空。

- 否则,使用 `$ROS_HOME/log`,使用 `~/.ros` (单位:千美元) `ROS_HOME` 如果未设置,或为空。

例如,将日志目录设置为 `~/my_logs`:

##### Linux

``` console
$ export ROS_LOG_DIR=~/my_logs
$ ros2 run logging_demo logging_demo_main
```

##### macOS

``` console
$ export ROS_LOG_DIR=~/my_logs
$ ros2 run logging_demo logging_demo_main
```

##### Windows

``` console
$ set "ROS_LOG_DIR=~/my_logs"
$ ros2 run logging_demo logging_demo_main
```

然后在下面找到日志 `~/my_logs/`.

或者,你可以设置 `ROS_HOME` 并且记录目录将与其相对(`$ROS_HOME/log`). `ROS_HOME` 用于需要基准目录的任何东西。请注意 `ROS_LOG_DIR` 必须是未设置或空的。例如, `ROS_HOME` 设置为 `~/my_ros_home`:

##### Linux

``` console
$ export ROS_HOME=~/my_ros_home
$ ros2 run logging_demo logging_demo_main
```

##### macOS

``` console
$ export ROS_HOME=~/my_ros_home
$ ros2 run logging_demo logging_demo_main
```

##### Windows

``` console
$ set "ROS_HOME=~/my_ros_home"
$ ros2 run logging_demo logging_demo_main
```

然后在下面找到日志 `~/my_ros_home/log/`.

<span id="logger-level-configuration-programmatically"></span>

## Logger 级别配置: 程序

10 次重复之后, 日志的级别将设定为 `DEBUG`,将会导致额外消息被登录。

其中一些调试消息会导致额外的函数/表达式被评价,这些函数/表达式之前被跳过为 `DEBUG` 日志调用没有被启用 。 见 [源代码](https://github.com/ros2/demos/blob/rolling/logging_demo/src/logger_usage_component.cpp) 演示文稿,以进一步解释所使用的电话,并查看支持的登录电话的完整列表的 rclcpp 记录文件。

<span id="logger-level-configuration-externally"></span>

## Logger 级别配置: 外部

今后将对运行时伐木机的外部配置采取通用办法(类似于如何进行) [rqt_logger_level](https://wiki.ros.org/rqt_logger_level) 在ROS 1中允许通过远程程序调用进行日志配置). **这一概念在ROS 2中尚未得到官方支持.** 在此期间,这个演示提供了 **实例** 可以对外调用的服务,用于请求对进程中已知的日志名称进行日志级别配置。

之前启动的演示已经运行此示例服务。 要将演示的日志级别设置为 `INFO`,拨打服务电话:

``` console
$ ros2 service call /config_logger logging_demo/srv/ConfigLogger "{logger_name: 'logger_usage_demo', level: INFO}"
```

此服务调用将针对在此过程中运行的日志器工作, 只要您知道它的名称。 这包括ROS 2 核心中的日志器, 例如 `rcl` (普通客户端库软件包)。要启用调试日志 `rcl`,拨打:

``` console
$ ros2 service call /config_logger logging_demo/srv/ConfigLogger "{logger_name: 'rcl', level: DEBUG}"
```

您应该看到调试输出 `rcl` 开始显示。

<span id="using-the-logger-config-component"></span>

### 使用日志配置组件

响应日志配置请求的服务器已被开发为组件, 以便添加到基于配置的现有系统中。 例如, 如果您正在使用 [运行节点的容器](../Intermediate/Composition.md),为了能够配置您的日志,您只需要请求它额外加载 `logging_demo::LoggerConfig` 组件进入容器。

例如,如果您想要调试 `composition::Talker` 演示,您可以正常地启动演讲者:

贝壳1:

``` console
$ ros2 run rclcpp_components component_container
```

贝壳2:

``` console
$ ros2 component load /ComponentManager composition composition::Talker
```

然后,当你想要启用调试记录时,加载 `LoggerConfig` 组件:

贝壳 2

``` console
$ ros2 component load /ComponentManager logging_demo logging_demo::LoggerConfig
```

最后,通过处理空名的日志,将所有未设置的日志刻录者配置到调试的重度。请注意,已专门配置用于使用特定重度的日志不会受到此调用的影响。

贝壳2:

``` console
$ ros2 service call /config_logger logging_demo/srv/ConfigLogger "{logger_name: '', level: DEBUG}"
```

您应该看到进程里任何先前未设置的日志的调试输出开始出现, 包括来自 ROS 2 核心的调试输出 。

<span id="logger-level-configuration-command-line"></span>

## 锁定级别配置:命令行

截至 Bouncy ROS 2 发布时,未设置严重性设置的日志者,其严重程度等级可以从命令行中明确配置。重新启动演示,包括以下命令行参数:

``` console
$ ros2 run logging_demo logging_demo_main --ros-args --log-level debug
```

此选项为任何未设置的日志重置到调试重置级的默认重置。 您应该从演示本身和ROS 2 核心的日志中看到调试输出 。

截至银河系ROS 2 发布时,可以从命令行中配置单个日志的重度级别。重新启动演示,包括以下命令行参数:

##### 银河系和新星系

``` console
$ ros2 run logging_demo logging_demo_main --ros-args --log-level logger_usage_demo:=debug
```

<span id="console-output-formatting"></span>

### 主控台输出格式化

如果您想要多多少少的动词格式化, 您可以使用 RUPATLS\_ CONSOLE\_ OUTPUT\_ FORMAT 环境变量。 例如, 为了额外获得日志调用的时间戳和位置, 请停止演示, 并用环境变量集重启它 :

##### Linux

``` console
$ export RCUTILS_CONSOLE_OUTPUT_FORMAT="[{severity} {time}] [{name}]: {message} ({function_name}() at {file_name}:{line_number})"
$ ros2 run logging_demo logging_demo_main
```

##### macOS

``` console
$ export RCUTILS_CONSOLE_OUTPUT_FORMAT="[{severity} {time}] [{name}]: {message} ({function_name}() at {file_name}:{line_number})"
$ ros2 run logging_demo logging_demo_main
```

##### Windows

``` console
$ set "RCUTILS_CONSOLE_OUTPUT_FORMAT=[{severity} {time}] [{name}]: {message} ({function_name}() at {file_name}:{line_number})"
$ ros2 run logging_demo logging_demo_main
```

您应该查看时间戳, 以秒计, 以及每个信件附加打印的函数名称、 文件名和行号 。 *“时间”选项仅在ROS 2 Bouncy发布时才得到支持。*

<span id="console-output-colorizing"></span>

### 主控台输出颜色

默认情况下,输出在瞄准终端时会变色。如果您想要强制启用或使其失效,您可以使用 `RCUTILS_COLORIZED_OUTPUT` 环境变量。例如:

##### Linux

``` console
$ export RCUTILS_COLORIZED_OUTPUT=0  # 1 for forcing it
$ ros2 run logging_demo logging_demo_main
```

##### macOS

``` console
$ export RCUTILS_COLORIZED_OUTPUT=0  # 1 for forcing it
$ ros2 run logging_demo logging_demo_main
```

##### Windows

``` console
$ set "RCUTILS_COLORIZED_OUTPUT=0" :: 1 for forcing it
$ ros2 run logging_demo logging_demo_main
```

你应该看到调试、警告、错误和致命的日志现在还没有变色。

> **说明**
>
> 在 Linux 和 MacOS 中,强制色化输出意味着如果输出重定向到文件, ansi 逃脱的颜色代码会出现在它上。 在窗口中, 色彩化方法依赖于控制台 API。 如果强制, 您将会得到新的警告, 表示色彩化失败 。 默认行为已经检查输出是否为控制台, 因此不建议强制色化 。

> **说明**
>
> 如果您通过下列方式启动几个节点: `ros2 launch`,节点上有一个活动终端(除非您设置) `emulate_tty=True`。这意味着要获得彩色输出 `ros2 launch`,需要设置 `RCUTILS_COLORIZED_OUTPUT=1` 明确无误。

<span id="default-stream-for-console-output"></span>

### 控制台输出的默认流

在Foxy和以后,所有调试级的输出默认会转到 stderr 。通过设置,可以强制所有输出到 stdout 。 `RCUTILS_LOGGING_USE_STDOUT` 环境变量为 `1`。例如:

##### Linux

``` console
$ export RCUTILS_LOGGING_USE_STDOUT=1
```

##### macOS

``` console
$ export RCUTILS_LOGGING_USE_STDOUT=1
```

##### Windows

``` console
$ set "RCUTILS_LOGGING_USE_STDOUT=1"
```

<span id="line-buffered-console-output"></span>

### 线条缓冲控制台输出

默认情况下,所有的日志输出都是未缓冲的。您可以通过设置 `RCUTILS_LOGGING_BUFFERED_STREAM` 环境变量为 1. 例如:

##### Linux

``` console
$ export RCUTILS_LOGGING_BUFFERED_STREAM=1
```

##### macOS

``` console
$ export RCUTILS_LOGGING_BUFFERED_STREAM=1
```

##### Windows

``` console
$ set "RCUTILS_LOGGING_BUFFERED_STREAM=1"
```

然后运行 :

``` console
$ ros2 run logging_demo logging_demo_main
```
