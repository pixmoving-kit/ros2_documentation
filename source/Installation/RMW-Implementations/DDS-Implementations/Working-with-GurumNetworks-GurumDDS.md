<span id="gurumnetworks-gurumdds"></span>
# GurumNetworks GurumDDS

`rmw_gurumdds` 使用 GurumNetworks GurumDDS 实现 ROS 中间件接口。有关 GurumDDS 的更多信息，请访问 [GurumNetworks 网站](https://gurum.cc/index_eng)。

<span id="prerequisites"></span>
## 前提条件

本指南假设你已通过 [deb 软件包安装 ROS 2](../../Ubuntu-Install-Debs.md)，或[在 Ubuntu 上从源码构建 ROS 2](../../Alternatives/Ubuntu-Development-Setup.md)，完成了 ROS 2 环境配置。

版本要求如下，详情见 [README](https://github.com/ros2/rmw_gurumdds)：

| ROS 2 发行版 | GurumDDS 版本 |
| --- | --- |
| rolling | `>= 3.2.0` |
| lyrical | `>= 3.2.0` |
| kilted | `>= 3.2.0` |
| jazzy | `>= 3.2.0` |
| humble | `3.1.x` |

Ubuntu 上的 ROS 2 apt 仓库提供 GurumDDS 的 deb 软件包。GurumDDS 的 Windows 二进制安装程序即将提供。

可以从 [GurumDDS 免费试用页面](https://gurum.cc/free_trial_eng.html)获取免费试用许可证。获得许可证后，请将其放在 `/etc/gurumnet` 中。

<span id="installation"></span>
## 安装

<span id="option-1-install-from-the-ros-2-apt-repository-recommended"></span>
### 方式 1：从 ROS 2 apt 仓库安装（推荐）

```console
$ sudo apt install ros-rolling-rmw-gurumdds-cpp
```

该命令会同时安装 `rmw_gurumdds_cpp` 和 `gurumdds`。

<span id="option-2-build-from-source-code"></span>
### 方式 2：从源码构建

1. 克隆仓库：

```console
$ cd ros2_ws/src
$ git clone https://github.com/ros2/rmw_gurumdds -b rolling ros2/rmw_gurumdds
```

2. 安装依赖项：

```console
$ cd ..
$ rosdep install --from src -i --rosdistro rolling
```

3. 使用 Colcon 构建工作空间：

```console
$ colcon build --symlink-install
```

<span id="switch-to-rmw-gurumdds"></span>
## 切换到 rmw_gurumdds

设置以下环境变量，即可从其他 RMW 实现切换到 rmw_gurumdds：

```console
$ export RMW_IMPLEMENTATION=rmw_gurumdds_cpp
```

更多信息见[使用多种 RMW 实现](../../../How-To-Guides/Working-with-multiple-RMW-implementations.md)。

<span id="testing-the-installation"></span>
## 测试安装结果

运行 `talker` 和 `listener` 节点，验证安装是否成功：

```console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp talker
```

```console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp listener
```

如果两个节点能够成功通信，就说明安装正常。

!!! note "说明"
    运行这些命令前，记得加载（source）ROS 2 环境设置脚本。
