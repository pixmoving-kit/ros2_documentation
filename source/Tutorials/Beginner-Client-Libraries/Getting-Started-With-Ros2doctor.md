<span id="using-ros2doctor-to-identify-issues"></span> <span id="ros2doctor"></span>
# 使用 ros2doctor 发现问题

**目标：** 使用 `ros2doctor` 工具发现 ROS 2 配置中的问题。

**教程级别：** 初学者

**预计用时：** 10 分钟

<span id="background"></span>
## 背景

ROS 2 未按预期运行时，可以使用 `ros2doctor` 检查配置。

`ros2doctor` 会检查 ROS 2 的各个方面，包括平台、版本、网络、环境、正在运行的系统等，提示潜在错误和问题原因。

<span id="prerequisites"></span>
## 前提条件

`ros2doctor` 属于 `ros2cli` 软件包。只要已经安装 `ros2cli`，就可以使用它；正常安装 ROS 2 时都会包含该软件包。

本教程使用 [turtlesim](../Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md)演示部分示例。

<span id="tasks"></span>
## 操作步骤

<span id="check-your-setup"></span>
### 1 检查配置

先使用 `ros2doctor` 整体检查 ROS 2 配置。在新终端加载 ROS 2 环境，然后输入：

```console
$ ros2 doctor
All <n> checks passed
```

它会检查各个配置模块，返回警告和错误。如果配置没有问题，就会看到类似上面的消息。

出现几条警告并不罕见。`UserWarning` 不代表配置不可用，更可能只是说明某些设置不够理想。

警告形式如下：

```console
<path>: <line>: UserWarning: <message>
```

例如，使用不稳定的 ROS 2 发行版时，`ros2doctor` 会发出以下警告：

```console
UserWarning: Distribution <distro> is not fully supported or tested. To get more consistent features, download a stable version at https://index.ros.org/doc/ros2/Installation/
```

如果只发现警告，仍会显示 `All <n> checks passed`。

大多数检查结果归类为警告，而非错误。用户需要自行判断这些反馈的重要程度。如果发现较少见的配置错误，消息以 `UserWarning: ERROR:` 标识，对应检查就会判定失败。

此时会看到类似的结果：

```console
1/3 checks failed

Failed modules:  network
```

错误表示系统缺少对 ROS 2 至关重要的设置或功能，需要解决这些错误，以确保系统正常运行。

<span id="check-a-system"></span>
### 2 检查运行中的系统

也可以检查正在运行的 ROS 2 系统，寻找问题的潜在原因。运行 turtlesim，让节点开始通信，以观察 `ros2doctor` 如何检查运行中的系统。

打开新终端，加载 ROS 2 环境，并运行：

```console
$ ros2 run turtlesim turtlesim_node
```

打开另一个终端，加载 ROS 2 环境，运行遥控节点：

```console
$ ros2 run turtlesim turtle_teleop_key
```

现在回到专门运行 `ros2doctor` 的终端，再执行一次。如果上次有配置警告或错误，它们仍会显示；随后还会看到与当前系统有关的新警告：

```console
$ ros2 doctor
UserWarning: Publisher without subscriber detected on /turtle1/color_sensor.
UserWarning: Publisher without subscriber detected on /turtle1/pose.
```

这表明 `/turtlesim` 向两个无人订阅的话题发布数据，`ros2doctor` 认为这可能带来问题。

如果对 `/color_sensor` 和 `/pose` 话题运行 echo 命令，发布者就会有订阅者，相应警告也会消失。

保持 turtlesim 运行，再打开两个新终端，各自加载 ROS 2 环境，然后分别运行：

```console
$ ros2 topic echo /turtle1/color_sensor
```

```console
$ ros2 topic echo /turtle1/pose
```

再次运行 `ros2doctor`，`publisher without subscriber` 警告便会消失。试验后，记得在运行 `echo` 的终端中按 `Ctrl+C`。

现在尝试关闭 turtlesim 窗口，或退出遥控节点，再运行 `ros2doctor`。由于系统中的一个节点已不可用，会看到针对不同话题的 `publisher without subscriber` 或 `subscriber without publisher` 警告。

对于包含许多节点的复杂系统，`ros2doctor` 很有助于查找通信问题的潜在原因。

<span id="get-a-full-report"></span>
### 3 获取完整报告

`ros2doctor` 会提示网络、系统等方面的问题；添加 `--report` 参数后，还可以获取更多细节，帮助分析问题。

例如，收到网络配置警告后，可以使用 `--report` 确定究竟哪部分配置触发了警告。

向他人提交 ROS 2 支持请求时，这份报告也很有用。将相关部分复制到请求中，可以帮助他人更好地了解你的环境，并提供更准确的帮助。

在终端输入以下命令，获取完整报告：

```console
$ ros2 doctor --report
```

输出信息分为五组：

```console
NETWORK CONFIGURATION
...

PLATFORM INFORMATION
...

RMW MIDDLEWARE
...

ROS 2 INFORMATION
...

TOPIC LIST
...
```

可以将报告内容与 `ros2 doctor` 的警告对应起来。例如，若出现前文所述的发行版“未得到完整支持或测试”警告，可以查看报告的 `ROS 2 INFORMATION` 部分：

```console
distribution name      : <distro>
distribution type      : ros2
distribution status    : prerelease
release platforms      : {'<platform>': ['<version>']}
```

其中 `distribution status` 为 `prerelease`，说明它是预发布版本，这就解释了为何尚未得到完整支持。

<span id="summary"></span>
## 小结

`ros2doctor` 可以提示 ROS 2 配置和运行中系统的问题。通过 `--report` 参数，可以进一步查看警告背后的详细信息。

请记住，`ros2doctor` 不是代码调试工具，无法诊断代码错误或系统实现层面的问题。

<span id="related-content"></span>
## 相关内容

[ros2doctor 的 README](https://github.com/ros2/ros2cli/tree/rolling/ros2doctor)介绍了更多参数。也可以浏览 `ros2doctor` 的代码：它对初学者比较友好，是开始参与贡献的不错选择。

<span id="next-steps"></span>
## 后续步骤

你已经完成初学者级别的教程！
