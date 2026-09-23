---
translation_status: machine_translated
source: Installation/RMW-Implementations/DDS-Implementations/Working-with-GurumNetworks-GurumDDS.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="gurumnetworks-gurumdds"></span>

# GurumNetworks GurumDDS

`rmw_gurumdds` 使用 GurumNetworks GurumDDS 执行 ROS 中间软件界面。关于 GurumDDS 的更多信息,请访问 [古鲁姆网络网站](https://gurum.cc/index_eng).

<span id="prerequisites"></span>

## 前提条件

本指南假设您已完成ROS 2 环境设置进程, 由 [通过 Deb 软件包安装 ROS 2](../../Ubuntu-Install-Debs.md) 或 时 间 [从Ubuntu的源头建造ROS 2](../../Alternatives/Ubuntu-Development-Setup.md).

版本要求([详情请参见 README](https://github.com/ros2/rmw_gurumdds)):

| ROS 2 临时任务 | GurumDDS 版本( G) |
|----------------|-------------------|
| 滚动           | `>= 3.2.0`        |
| 格言词         | `>= 3.2.0`        |
| 轻轻的         | `>= 3.2.0`        |
| 爵士乐         | `>= 3.2.0`        |
| 谦卑           | `3.1.x`           |

在Ubuntu上的ROS 2 apt寄存器中提供了GurumDDS的Deb包,GurumDDS的Windows二进制安装器将很快提供.

您可以从芬兰获得免费的审理许可证。 [GurumDDS 自由试验页面](https://gurum.cc/free_trial_eng.html).

取得许可证后,将其置于下列地点: `/etc/gurumnet`

<span id="installation"></span>

## 安装

<span id="option-1-install-from-the-ros-2-apt-repository-recommended"></span>

### 备选案文1:从ROS 2 apt存储器安装(建议)

``` console
$ sudo apt install ros-rolling-rmw-gurumdds-cpp
```

此两个都安装 `rmw_gurumdds_cpp` 财务报告和财务报告 `gurumdds`.

<span id="option-2-build-from-source-code"></span>

### 备选案文2:从源代码构建

1.  清除存储器

``` console
$ cd ros2_ws/src
$ git clone https://github.com/ros2/rmw_gurumdds -b rolling ros2/rmw_gurumdds
```

2.  安装依赖性 :

``` console
$ cd ..
$ rosdep install --from src -i --rosdistro rolling
```

3.  使用 Colcon 构建工作空间 :

``` console
$ colcon build --symlink-install
```

<span id="switch-to-rmw-gurumdds"></span>

## 切换到 rmw\_ gurumds

通过设置环境变量从其他 RMW 执行切换到rmw\_ gurumdds :

``` console
$ export RMW_IMPLEMENTATION=rmw_gurumdds_cpp
```

关于与多个《保护移徒公约》实施工作合作的更多信息,请参见: [B. 与多项《保护移徙工人公约》的实施合作](../../../How-To-Guides/Working-with-multiple-RMW-implementations.md).

<span id="testing-the-installation"></span>

## 测试安装

运行 `talker` 财务报告和财务报告 `listener` 用于验证您的安装的节点 :

``` console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp talker
```

``` console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp listener
```

如果节点成功通信,您的安装工作正常 。

> **说明**
>
> 在运行这些命令前请记住源代码为 ROS 2 的设置脚本 。
