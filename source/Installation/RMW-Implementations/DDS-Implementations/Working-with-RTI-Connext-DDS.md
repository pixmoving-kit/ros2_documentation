---
translation_status: machine_translated
source: Installation/RMW-Implementations/DDS-Implementations/Working-with-RTI-Connext-DDS.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="rti-connext-dds"></span>

# RTI Connext DDS

2000年多以来,RTI Connext DDS是全世界最严格的系统设计中最信任的,它分发性能、可靠性和安全性最高的关键实时数据。 它免费用于原型、研究、非商业和学术用途。 [RTI网站 (中文(简体) ).](https://www.rti.com/ros) 了解有关支助和商业许可证的备选方案。

<span id="prerequisites"></span>

## 前提条件

<span id="install-rti-connext-dds"></span>

### 安装 RTI 连接 DDS

> 建造和使用 `rmw_connextdds` 需要符合 ROS 2 分发版本的 Connext DDS 。安装时包含 Connext DDS 。 `rmw_connextdds` 使用 apt 或可以手动安装用于从源头构建 。以下表格的细节是使用 Connext DDS 版本安装的 。 `apt`,并且从源头建楼需要哪些版本:
>
> | ROS 2 分发 | 使用 apt 安装 | 要从源创建 |
> |------------|---------------|------------|
> | 滚动       | n/a           | `7.7.0`    |
> | 格言词     | `7.7.0`       | `7.7.0`    |
> | 轻轻的     | `7.3.0`       | `7.3.0`    |
> | 爵士乐     | `6.0.1`       | `6.0.1`    |
> | 谦卑       | `6.0.1`       | `6.0.1`    |

RTI Connext Pro通过多种渠道提供:

**ROS 2号储油层**  
ROS 2用户可以使用以下命令从ROS pt寄存器中安装用于x86_64 Linux的RTI Connext DDS库的非商业使用版本:

##### v7.3.0

``` console
$ sudo apt update && sudo apt install -q -y rti-connext-dds-7.3.0-ros
```

##### v6.0.1

``` console
$ sudo apt update && sudo apt install -q -y rti-connext-dds-6.0.1
```

此软件包仅包含 RTI Connext 核心 DDS 库; 不包括完整的 Connex 专业工具套件和运行时间服务。 请注意这些 Connext 库在安装时是自动安装的 。 `rmw_connextdds` 使用Apt。 使用Apt。

**其他安装选项** 那个... [Connex 机器人工具包](https://www.rti.com/developers/connext-robotics-toolkit) 包括全套的Connext工具和基础设施服务。它为ROS和Connext提供单步安装,使用apt。它免费用于原型开发、研究、非商业和学术用途。

关于在各种平台建立和调整RMW和ROS 2应用程序的详细指示,以及启用DS安全的详细指示,可在以下平台上查阅: [RTI ROS社区](https://community.rti.com/ros) 页面。

<span id="install-rmw-connextdds-binary-packages"></span>

## 安装 rmw_connextds 二进制包

要安装二进制软件包 `rmw_connextdds` 和 ROS 2 pt 库中的 Connext 库,使用以下命令:

``` console
$ sudo apt update && sudo apt install -q -y ros-rolling-rmw-connextdds
```

<span id="building-rmw-connextdds-from-source-code"></span>

## 从源代码构建 rmw_connextends

从源代码构建可以确保 RMW 与您的系统匹配并正确安装。以下指令假设 Linux x86_64 构建主机和目标; [RTI ROS社区](https://community.rti.com/ros) 页面有为包括Arm,Windows,和macOS在内的其他平台和目标建设的指令.

清除存储器 `rmw_connextdds` 输入 ROS 2 工作空间,并选择与 ROS 2 使用中的分布匹配的分支 :

``` console
$ mkdir -p ros2_ws/src
$ cd ros2_ws
$ git clone -b rolling https://github.com/ros2/rmw_connextdds src/rmw_connextdds
```

设置环境来帮助 colcon 发现 RTI Connext 安装位置 。 可以通过手动设置环境变量来实现 。 `NDDSHOME` 到 RTI Connext 安装的位置,或者使用随 RTI Connext 安装而来的脚本:

``` console
$ source ${RTI_CONNEXT_INSTALL_LOCATION}/resource/scripts/rtisetenv_x64Linux4gcc7.3.0.bash
```

确保设置 ROS 2 环境:

``` console
$ source /opt/ros/rolling/setup.bash
```

使用 comcon 构建 RMW :

``` console
$ colcon build --symlink-install
```

在构建成功完成后, 请确定为工作空间提供设置文件 :

``` console
$ source install/setup.bash
```

<span id="use-the-resulting-rmw-connextdds"></span>

## 使用由此产生的 rmw_connexdds

设置环境变量 `RMW_IMPLEMENTATION` 告诉ROS 2 使用哪个 RMW :

``` console
$ export RMW_IMPLEMENTATION=rmw_connextdds
```

另见: [B. 与多项《保护移徙工人公约》的实施合作](../../../How-To-Guides/Working-with-multiple-RMW-implementations.md)

<span id="run-the-talker-and-listener"></span>

## 运行说话者和收听者

现在快跑 `talker` 财务报告和财务报告 `listener` 以测试 RTI 连接 DDS

``` console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp talker
```

``` console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp listener
```
