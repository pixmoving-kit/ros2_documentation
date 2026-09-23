---
translation_status: machine_translated
source: Tutorials/Beginner-Client-Libraries/Getting-Started-With-Ros2doctor.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-ros2doctor-to-identify-issues"></span> <span id="ros2doctor"></span>

# 使用( E) `ros2doctor` 确定问题

**目标：** 使用 `ros2doctor` 工具。

**教程级别：** 入门

**用时：** 10分钟

<span id="background"></span>

## 背景

当您的 ROS 2 设置未按预期运行时, 您可以使用 `ros2doctor` 工具。

`ros2doctor` 请检查ROS 2的方方面面,包括平台,版本,网络,环境,运行系统等等,并警告您可能的错误和问题的原因.

<span id="prerequisites"></span>

## 前提条件

`ros2doctor` 属于 `ros2cli` 包,只要你还有 `ros2cli` 安装(任何正常安装都应该安装),您将能够使用 `ros2doctor`.

此教程用途 [乌龟](../Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md) 以说明其中的一些例子。

<span id="tasks"></span>

## 操作步骤

<span id="check-your-setup"></span>

### 1 检查您的设置

让我们来检查一下你的常规ROS 2 整体设置 `ros2doctor`。首先,在新的终端中输入源ROS 2,然后输入命令:

``` console
$ ros2 doctor
All <n> checks passed
```

这将检查您的所有设置模块, 并返回警告和错误。 如果您的 ROS 2 设置处于完美的状态, 您将会看到类似上面的信息 。

但是,一些警告被退回并非不寻常。 A `UserWarning` 这并非意味着你的设置是无法使用的; 这更可能只是表明某些事物的配置方式并不理想。

如果你确实收到警告,它会看起来像这样:

``` console
<path>: <line>: UserWarning: <message>
```

举例来说, `ros2doctor` 如果您使用不稳定的ROS 2 分布, 将会找到此警告 :

``` console
UserWarning: Distribution <distro> is not fully supported or tested. To get more consistent features, download a stable version at https://index.ros.org/doc/ros2/Installation/
```

若为: `ros2doctor` 只要在系统里找到警告,你就会收到 `All <n> checks passed` 留言。

大多数检查被归类为警告而非错误。 主要由您, 用户决定反馈的重要性 `ros2doctor` 返回。如果它确实在您的设置中发现了一个罕见的错误,则以 `UserWarning: ERROR:`,该检查被视为失败。

您将看到类似于以下问题反馈列表的信息 :

``` console
1/3 checks failed

Failed modules:  network
```

错误显示系统缺少对ROS 2. 至关重要的重要设置或功能. 应处理错误以确保系统功能适当.

<span id="check-a-system"></span>

### 2 检查系统

您也可以检查运行中的 ROS 2 系统,以确定问题可能的原因。 `ros2doctor` 运行一个运行中的系统, 让我们运行龟兹姆, 它的节点相互积极沟通。

启动系统的方式是打开一个新的终端,提供ROS 2,并进入命令:

``` console
$ ros2 run turtlesim turtlesim_node
```

打开另一个终端和源 ROS 2 来运行远程控制 :

``` console
$ ros2 run turtlesim turtle_teleop_key
```

现在快跑 `ros2doctor` 您将看到上次运行时的警告和错误 `ros2doctor` 如果有的话,你们将用新的警告来警告你们。

``` console
$ ros2 doctor
UserWarning: Publisher without subscriber detected on /turtle1/color_sensor.
UserWarning: Publisher without subscriber detected on /turtle1/pose.
```

看来... `/turtlesim` 节点将数据发布到两个没有被订阅的话题,以及 `ros2doctor` 认为这可能导致问题。

如果你运行命令回声 `/color_sensor` 财务报告和财务报告 `/pose` 这些警告将会消失 因为出版商会有订户

您可以在龟兹姆仍在运行时打开两个新的终端, 获取 ROS 2, 并在自己的终端中运行以下命令 。

``` console
$ ros2 topic echo /turtle1/color_sensor
```

``` console
$ ros2 topic echo /turtle1/pose
```

那快跑开! `ros2doctor` 在它的终端上。 `publisher without subscriber` 警告将消失 。 (确保输入) `Ctrl+C` 在运行的终端中 `echo`).

现在尝试退出龟兹窗口 或退出Teleop并运行 `ros2doctor` 。您将看到更多警告 `publisher without subscriber` 或 时 间 `subscriber without publisher` 不同主题的节点,

在一个有很多节点的复杂系统中, `ros2doctor` 对于查明通信问题的可能原因而言,将十分宝贵。

<span id="get-a-full-report"></span>

### 3 获得完整报告

虽然 `ros2doctor` 将会让您知道关于您的网络、系统等的警告,并使用 `--report` 参数会给你更多细节 帮助你分析问题。

你也许想用 `--report` 如果您得到关于您的网络设置的警告, 并想知道您的配置中究竟哪个部分引起警告 。

当您需要开张支持票以获得二号卫星的帮助时, 您也可以将报告的相关部分复制并粘贴在票中, 以便帮助您更好地了解环境并提供更好的帮助。

要获得完整报告,请在终端中输入以下命令:

``` console
$ ros2 doctor --report
```

将返回分为五组的资料清单:

``` console
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

您可以对照运行时得到的警告核对这里的信息 `ros2 doctor`。例如,如果 `ros2doctor` 返回警告(前面提到的),即您的发行量“没有得到充分支持或测试”,您可以查看 `ROS 2 INFORMATION` 本报告各节:

``` console
distribution name      : <distro>
distribution type      : ros2
distribution status    : prerelease
release platforms      : {'<platform>': ['<version>']}
```

在这里,你可以看到 `distribution status` 是,这是 `prerelease`这解释了为什么它没有得到充分的支持。

<span id="summary"></span>

## 小结

`ros2doctor` 将通知您 ROS 2 设置和运行系统中的问题。您可以通过使用 `--report` 参数。

记住, `ros2doctor` 这不是调试工具; 它不会帮助处理您的代码或您的系统执行方面的错误 。

<span id="related-content"></span>

## 相关内容

[ROS2 博士的读取器](https://github.com/ros2/ros2cli/tree/rolling/ros2doctor) 将会告诉你更多关于不同论点。你也许想看看周围 `ros2doctor` 也重新投入, 因为它是相当的初学者友好的,

<span id="next-steps"></span>

## 后续步骤

你完成了初学者的辅导课程!
