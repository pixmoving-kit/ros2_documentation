<span id="rti-connext-dds"></span>
# RTI Connext DDS

RTI Connext DDS 已用于全球 2000 多个要求极为严格的系统设计，以高性能、高可靠性和高安全性分发关键实时数据。它可免费用于原型开发、研究、非商业用途和学术用途。有关更多信息、支持选项及商业许可证，请访问 [RTI 网站](https://www.rti.com/ros)。

<span id="prerequisites"></span>
## 前提条件

<span id="install-rti-connext-dds"></span>
### 安装 RTI Connext DDS

构建和使用 `rmw_connextdds`，需要安装与当前 ROS 2 发行版兼容的 Connext DDS 版本。通过 apt 安装 `rmw_connextdds` 时会一并安装 Connext DDS；从源码构建时，也可以手动安装。下表列出了 apt 安装的版本和源码构建所需的版本：

| ROS 2 发行版 | apt 安装的版本 | 源码构建所需版本 |
| --- | --- | --- |
| rolling | 不适用 | `7.7.0` |
| lyrical | `7.7.0` | `7.7.0` |
| kilted | `7.3.0` | `7.3.0` |
| jazzy | `6.0.1` | `6.0.1` |
| humble | `6.0.1` | `6.0.1` |

RTI Connext Pro 可以通过多种渠道获取。

**ROS 2 apt 仓库**

ROS 2 用户可以从 ROS apt 仓库，为 x86_64 Linux 安装限非商业用途的 RTI Connext DDS 库。

v7.3.0：

```console
$ sudo apt update && sudo apt install -q -y rti-connext-dds-7.3.0-ros
```

v6.0.1：

```console
$ sudo apt update && sudo apt install -q -y rti-connext-dds-6.0.1
```

这些软件包只包含 RTI Connext 核心 DDS 库，不包含完整的 Connext Professional 工具套件和运行时服务。通过 apt 安装 `rmw_connextdds` 时，会自动安装这些 Connext 库。

**其他安装方式**

[Connext Robotics Toolkit](https://www.rti.com/developers/connext-robotics-toolkit) 包含完整的 Connext 工具和基础设施服务，可通过 apt 一步安装 ROS 和 Connext。它可免费用于原型开发、研究、非商业用途和学术用途。

[RTI ROS 社区页面](https://community.rti.com/ros)提供了在多种平台上构建和调优 RMW、ROS 2 应用，以及启用 DDS 安全功能的详细说明。

<span id="install-rmw-connextdds-binary-packages"></span>
## 安装 rmw_connextdds 二进制软件包

运行以下命令，从 ROS 2 apt 仓库安装 `rmw_connextdds` 和 Connext 库的二进制软件包：

```console
$ sudo apt update && sudo apt install -q -y ros-rolling-rmw-connextdds
```

<span id="building-rmw-connextdds-from-source-code"></span>
## 从源码构建 rmw_connextdds

从源码构建，可以确保 RMW 与系统匹配并正确安装。以下说明假设构建主机和目标平台均为 Linux x86_64。[RTI ROS 社区页面](https://community.rti.com/ros)提供了面向 Arm、Windows、macOS 等其他平台和目标的构建说明。

将 `rmw_connextdds` 仓库克隆到 ROS 2 工作空间，并选择与所用 ROS 2 发行版匹配的分支：

```console
$ mkdir -p ros2_ws/src
$ cd ros2_ws
$ git clone -b rolling https://github.com/ros2/rmw_connextdds src/rmw_connextdds
```

配置环境，让 colcon 能够找到 RTI Connext 的安装位置。可以手动将环境变量 `NDDSHOME` 设为安装路径，也可以使用 RTI Connext 安装时附带的脚本：

```console
$ source ${RTI_CONNEXT_INSTALL_LOCATION}/resource/scripts/rtisetenv_x64Linux4gcc7.3.0.bash
```

确保已设置 ROS 2 环境：

```console
$ source /opt/ros/rolling/setup.bash
```

使用 colcon 构建 RMW：

```console
$ colcon build --symlink-install
```

构建成功后，务必加载工作空间的环境设置文件：

```console
$ source install/setup.bash
```

<span id="use-the-resulting-rmw-connextdds"></span>
## 使用构建得到的 rmw_connextdds

设置环境变量 `RMW_IMPLEMENTATION`，告诉 ROS 2 使用哪个 RMW 实现：

```console
$ export RMW_IMPLEMENTATION=rmw_connextdds
```

另请参见[使用多种 RMW 实现](../../../How-To-Guides/Working-with-multiple-RMW-implementations.md)。

<span id="run-the-talker-and-listener"></span>
## 运行 talker 和 listener

运行 `talker` 和 `listener`，测试 RTI Connext DDS：

```console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp talker
```

```console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp listener
```
