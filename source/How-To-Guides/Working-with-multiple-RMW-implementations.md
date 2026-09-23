<span id="working-with-multiple-ros-2-middleware-implementations"></span>
# 使用多种 ROS 2 中间件实现

本页介绍默认的 RMW 实现，以及如何指定其他实现。

<span id="prerequisites"></span>
## 前提条件

应先阅读 [DDS 和 ROS 中间件实现](../Concepts/Intermediate/About-Different-Middleware-Vendors.md)。

<span id="specifying-rmw-implementations"></span>
## 指定 RMW 实现

要使用多种 RMW 实现，需要安装 ROS 2 二进制包以及各 RMW 实现所需的额外依赖，或在包含多种 RMW 实现的工作空间中从源码构建 ROS 2。只要满足编译时依赖，相应的 RMW 实现默认就会参与构建。参见[安装 RMW 实现](../Installation/RMW-Implementations.md)。

C++ 和 Python 节点都支持 `RMW_IMPLEMENTATION` 环境变量，用户可通过它选择运行 ROS 2 应用程序时使用的 RMW 实现。

可以将该变量设为具体实现的标识符，例如 `rmw_cyclonedds_cpp`、`rmw_fastrtps_cpp`、`rmw_connextdds` 或 `rmw_gurumdds_cpp`。

例如，使用 Connext RMW 实现运行 C++ talker 和 Python listener：

**Linux：** 在一个终端中运行：

```console
$ RMW_IMPLEMENTATION=rmw_connextdds ros2 run demo_nodes_cpp talker
```

在另一个终端中运行：

```console
$ RMW_IMPLEMENTATION=rmw_connextdds ros2 run demo_nodes_py listener
```

**macOS：** 在一个终端中运行：

```console
$ RMW_IMPLEMENTATION=rmw_connextdds ros2 run demo_nodes_cpp talker
```

在另一个终端中运行：

```console
$ RMW_IMPLEMENTATION=rmw_connextdds ros2 run demo_nodes_py listener
```

**Windows：** 在一个终端中运行：

```console
$ set RMW_IMPLEMENTATION=rmw_connextdds
$ ros2 run demo_nodes_cpp talker
```

在另一个终端中运行：

```console
$ set RMW_IMPLEMENTATION=rmw_connextdds
$ ros2 run demo_nodes_py listener
```

<span id="adding-rmw-implementations-to-your-workspace"></span>
## 向工作空间添加 RMW 实现

安装必要依赖并重新构建工作空间，即可添加其他 DDS 和 RMW 实现。有关安装可用 DDS 实现的更多信息，请参阅 [RMW 实现](../Installation/RMW-Implementations.md)。

假设构建 ROS 2 工作空间时只安装了 Fast DDS，因此只构建了 Fast DDS 对应的 RMW 实现。上次构建时，其他 RMW 软件包（例如 `rmw_connextdds`）很可能没有找到对应的 DDS 安装。如果之后安装了 Connext 等其他 DDS 实现，就需要重新触发构建 Connext RMW 时进行的安装检查。下一次构建工作空间时指定 `--cmake-clean-cache`，即可看到新安装的 DDS 实现所对应的 RMW 软件包开始构建。

使用 `--cmake-clean-cache` 重新构建并加入其他 RMW 实现时，可能会遇到构建过程报告默认 RMW 实现已发生变化的问题。可以通过 CMake 参数 `RMW_IMPLEMENTATION` 将默认实现设回原值；也可以删除报错软件包的构建目录，再通过 `--packages-start <package name>` 继续构建。

<span id="troubleshooting"></span>
## 故障排查

<span id="checking-the-current-rmw"></span>
### 检查当前 RMW

查看 `RMW_IMPLEMENTATION` 环境变量即可判断当前使用的 RMW。在 Linux 中，`printenv` 会打印全部环境变量；其他操作系统有各自的查看方式。如果环境中没有 `RMW_IMPLEMENTATION`，可以认为正在使用该 ROS 发行版的默认实现，否则当前 RMW 就是该变量的值。各发行版的默认 RMW 见 [REP-2000](https://reps.openrobotics.org/rep-2000/#platforms-by-distribution)。

<span id="ensuring-use-of-a-particular-rmw-implementation"></span>
### 确保使用指定的 RMW 实现

如果 `RMW_IMPLEMENTATION` 指定的实现未安装，且系统只安装了一个实现，会看到类似错误：

```bash
Expected RMW implementation identifier of 'rmw_connextdds' but instead found 'rmw_fastrtps_cpp', exiting with 102.
```

如果已安装多种 RMW 实现，但请求使用另一种未安装的实现，则会看到：

```bash
Error getting RMW implementation identifier / RMW implementation not installed (expected identifier of 'rmw_connextdds'), exiting with 1.
```

遇到这些错误时，请确认 ROS 2 安装中包含 `RMW_IMPLEMENTATION` 指定的实现。

切换 RMW 实现时，还应确认 ROS 2 守护进程没有继续使用之前的实现，以免节点与 `ros2 node` 等命令行工具之间出现问题。例如，运行：

```bash
RMW_IMPLEMENTATION=rmw_connextdds ros2 run demo_nodes_cpp talker
```

然后运行：

```console
$ ros2 node list
```

会启动一个使用 Fast DDS 实现的守护进程：

```bash
21318 22.0  0.6 535896 55044 pts/8    Sl   16:14   0:00 /usr/bin/python3 /opt/ros/rolling/bin/_ros2_daemon --rmw-implementation rmw_fastrtps_cpp --ros-domain-id 0
```

即使随后用正确的 RMW 实现重新运行命令行工具，守护进程的实现也不会改变，ROS 2 命令行工具仍会失败。

要解决此问题，只需停止守护进程：

```console
$ ros2 daemon stop
```

然后使用正确的 RMW 实现重新运行 ROS 2 命令行工具。

<span id="rti-connext-on-osx-failure-due-to-insufficient-shared-memory-kernel-settings"></span>
### OSX 上的 RTI Connext：共享内存内核设置不足导致失败

在 OSX 上运行 RTI Connext 时，可能出现如下错误：

```console
[D0062|ENABLE]DDS_DomainParticipantPresentation_reserve_participant_index_entryports:!enable reserve participant index
[D0062|ENABLE]DDS_DomainParticipant_reserve_participant_index_entryports:Unusable shared memory transport. For a more in-   depth explanation of the possible problem and solution, please visit https://community.rti.com/kb/osx510.
```

原因是操作系统允许的共享内存段数量或大小不足，导致 `DomainParticipant` 无法分配足够资源并计算参与者索引。

可以临时或永久增加机器的共享内存资源。

要临时调整设置，以 root 用户运行：

```console
$ /usr/sbin/sysctl -w kern.sysv.shmmax=419430400
$ /usr/sbin/sysctl -w kern.sysv.shmmin=1
$ /usr/sbin/sysctl -w kern.sysv.shmmni=128
$ /usr/sbin/sysctl -w kern.sysv.shmseg=1024
$ /usr/sbin/sysctl -w kern.sysv.shmall=262144
```

要永久调整，需要编辑或创建 `/etc/sysctl.conf`，此操作需要 root 权限。向现有文件添加以下内容，或用这些内容创建该文件：

```bash
kern.sysv.shmmax=419430400
kern.sysv.shmmin=1
kern.sysv.shmmni=128
kern.sysv.shmseg=1024
kern.sysv.shmall=262144
```

修改后需要重启计算机才能生效。

本解决方案改编自 RTI Connext 社区论坛。更详细的解释见[原帖](https://community.rti.com/kb/osx510)。
