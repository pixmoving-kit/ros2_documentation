---
translation_status: machine_translated
source: How-To-Guides/Working-with-multiple-RMW-implementations.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="working-with-multiple-ros-2-middleware-implementations"></span>

# 使用多个 ROS 2 中间件实现

本页面解释默认的 RMW 执行方式以及如何指定一个选项.

<span id="prerequisites"></span>

## 前提条件

你应该已经读过 [DDS 和 ROS 中间软件执行页面](../Concepts/Intermediate/About-Different-Middleware-Vendors.md).

<span id="specifying-rmw-implementations"></span>

## B. 具体说明《公约》的实施情况

要具备多个 RMW 执行程序可供使用, 您必须安装了 ROS 2 二进制和任何额外的 RMW 执行的依赖, 或者从源头创建 ROS 2 , 并在工作空间中安装多个 RMW 执行程序( RMW 执行程序如果满足编译时间依赖性, 默认包含在构建中) 。 [安装 RMW 执行](../Installation/RMW-Implementations.md).

------------------------------------------------------------------------

C++ 和 Python 节点都支持环境变量 `RMW_IMPLEMENTATION` 允许用户在运行 ROS 2 应用程序时选择 RMW 执行。

用户可将该变量设定为特定的执行标识符,例如: `rmw_cyclonedds_cpp`, `rmw_fastrtps_cpp`, `rmw_connextdds`,或 `rmw_gurumdds_cpp`.

例如,要使用 C++ 聊天器和 Python 收听器运行演讲者演示, 并使用 Connext RTW 执行 :

##### Linux

在一个终端运行 :

``` console
$ RMW_IMPLEMENTATION=rmw_connextdds ros2 run demo_nodes_cpp talker
```

在另一个终端运行 :

``` console
$ RMW_IMPLEMENTATION=rmw_connextdds ros2 run demo_nodes_py listener
```

##### macOS

在一个终端运行 :

``` console
$ RMW_IMPLEMENTATION=rmw_connextdds ros2 run demo_nodes_cpp talker
```

在另一个终端运行 :

``` console
$ RMW_IMPLEMENTATION=rmw_connextdds ros2 run demo_nodes_py listener
```

##### Windows

在一个终端运行 :

``` console
$ set RMW_IMPLEMENTATION=rmw_connextdds
$ ros2 run demo_nodes_cpp talker
```

在另一个终端运行 :

``` console
$ set RMW_IMPLEMENTATION=rmw_connextdds
$ ros2 run demo_nodes_py listener
```

<span id="adding-rmw-implementations-to-your-workspace"></span>

## 将 RMW 执行添加到您的工作空间

通过安装必要的依赖关系和重建工作空间,可以在您的工作空间中添加额外的 DDS 和 RMW 执行 。 [RMW 实现](../Installation/RMW-Implementations.md) 关于安装可用的DDS选项的更多信息。

假设您只安装了快DDS, 并且只安装了快DDS RMW 执行程序。 您上次建造工作空间时, 任何其他 RMW 执行软件包, `rmw_connextdds` 例如, 可能无法找到相关的 DDS 执行的安装。 如果您再安装一个额外的 DDS 执行, 例如 Connext, 您需要重新触发检查, 检查在构建 Connext 的 RTW 执行时发生的 Connext 安装。 您可以通过指定 `--cmake-clean-cache` 在您下一个工作空间构建上标出旗号, 您应该看到, RMW 执行套件随后被构建为新安装的 DDS 执行 。

当“重建”工作空间,利用《公约》和《议定书》进一步实施《议定书》时,有可能遇到一个问题。 `--cmake-clean-cache` 选项。要解决这个问题,可以将默认执行设置设置到之前的状态。 `RMW_IMPLEMENTATION` CMake 参数,或者您可以删除用于投诉和继续构建的软件包的构建文件夹 `--packages-start <package name>`.

<span id="troubleshooting"></span>

## 麻烦的解决

<span id="checking-the-current-rmw"></span>

### 检查当前 RMW

要检查当前使用的 RMW , 您只需检查 `RMW_IMPLEMENTATION` 环境变量。在 Linux 系统中 `printenv` 打印环境变量的完整列表。其他操作系统将有其他程序来查看环境变量。如果 `RMW_IMPLEMENTATION` 不在环境中可以安全地假定您正在使用 ROS distro 的默认值, 否则当前的 RTW 是 列出的值。 每个 ROS diro 的默认 RTW 可以在 [REP-2000号报告](https://reps.openrobotics.org/rep-2000/#platforms-by-distribution).

<span id="ensuring-use-of-a-particular-rmw-implementation"></span>

### 确保利用某项《保护移栖物种公约》的实施

如果说 `RMW_IMPLEMENTATION` 环境变量被设定为没有安装支持的 RMW 执行, 如果您只安装了一个执行, 您将会看到一个类似以下的错误消息 :

``` bash
Expected RMW implementation identifier of 'rmw_connextdds' but instead found 'rmw_fastrtps_cpp', exiting with 102.
```

如果您对安装的多个 RMW 执行程序有支持,并且您请求使用未安装的,您将会看到类似的东西:

``` bash
Error getting RMW implementation identifier / RMW implementation not installed (expected identifier of 'rmw_connextdds'), exiting with 1.
```

如果发生这种情况, 请双次检查您的 ROS 2 安装是否包含您在定义中指定的 RMW 执行支持 `RMW_IMPLEMENTATION` 环境变量。

如果您想要在 RMW 执行之间切换, 请验证ROS 2 守护进程是否与之前的 RMW 执行一起运行, 以避免节点和命令行工具之间出现任何问题, 如 `ros2 node`。例如,如果运行:

``` bash
RMW_IMPLEMENTATION=rmw_connextdds ros2 run demo_nodes_cpp talker
```

财务报告和财务报告

``` console
$ ros2 node list
```

它将生成一个带有快速 DDS 执行的守护进程 :

``` bash
21318 22.0  0.6 535896 55044 pts/8    Sl   16:14   0:00 /usr/bin/python3 /opt/ros/rolling/bin/_ros2_daemon --rmw-implementation rmw_fastrtps_cpp --ros-domain-id 0
```

即使您再次以正确的 RTW 执行方式运行命令行工具,守护进程 RTW 的执行也不会改变, ROS 2 命令行工具也会失败.

要解决这个问题,只需停止守护进程:

``` console
$ ros2 daemon stop
```

并重新运行 ROS 2 命令行工具,并有正确的 RMW 执行.

<span id="rti-connext-on-osx-failure-due-to-insufficient-shared-memory-kernel-settings"></span>

### OSX上的 RTI Connext: 由于共享内存设置不足而失败

如果您在 OSX 上运行 RTI Connext 时收到类似下面的错误消息 :

``` console
[D0062|ENABLE]DDS_DomainParticipantPresentation_reserve_participant_index_entryports:!enable reserve participant index
[D0062|ENABLE]DDS_DomainParticipant_reserve_participant_index_entryports:Unusable shared memory transport. For a more in-   depth explanation of the possible problem and solution, please visit https://community.rti.com/kb/osx510.
```

此错误是由操作系统允许的共享内存片段数量或大小不足造成的。因此, `DomainParticipant` 无法分配足够资源并计算导致错误的参与者指数。

您可以暂时或永久地增加您的机器的共享内存资源 。

要暂时增加设置,您可以将以下命令作为用户root运行:

``` console
$ /usr/sbin/sysctl -w kern.sysv.shmmax=419430400
$ /usr/sbin/sysctl -w kern.sysv.shmmin=1
$ /usr/sbin/sysctl -w kern.sysv.shmmni=128
$ /usr/sbin/sysctl -w kern.sysv.shmseg=1024
$ /usr/sbin/sysctl -w kern.sysv.shmall=262144
```

要永久增加设置, 您需要编辑或创建文件 `/etc/sysctl.conf`。创建或编辑此文件需要 root 权限。或者添加到您现有的 `etc/sysctl.conf` 文件或创建 `/etc/sysctl.conf` 采用下列直线:

``` bash
kern.sysv.shmmax=419430400
kern.sysv.shmmin=1
kern.sysv.shmmni=128
kern.sysv.shmseg=1024
kern.sysv.shmall=262144
```

您需要修改此文件后重新启动机器, 以使更改生效 。

此解决方案由 RTI Connext 社区论坛编辑 。 [原员额](https://community.rti.com/kb/osx510) 更详细的解释。
