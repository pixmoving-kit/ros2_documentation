<span id="rhel-rpm-packages"></span>

# RHEL（RPM 软件包）

目前为 RHEL 8 提供 ROS 2 Rolling Ridley 的 RPM 软件包。目标平台定义见 [REP 2000](https://reps.openrobotics.org/rep-2000/)。

<span id="resources"></span>

## 资源

- ROS 2 Rolling（RHEL 8）状态页面：[amd64](http://repo.ros2.org/status_page/ros_rolling_rhel.html)。
- [Jenkins 实例](http://build.ros2.org/)
- [软件源](http://repo.ros2.org)

<span id="set-locale"></span>

## 设置区域设置

按照 [RHEL 区域设置说明](_RHEL-Set-Locale.md)配置支持 UTF-8 的区域设置。

<span id="setup-sources"></span>
<span id="rhel-install-rpms-setup-sources"></span>

## 配置软件源

需要启用 EPEL 和 PowerTools 软件源：

```console
$ sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-$(rpm -E %rhel).noarch.rpm
$ sudo env FORCE_DNF=1 crb enable
```

!!! note "说明"

    此步骤可能因所用发行版而略有不同，请查阅 [EPEL 文档](https://docs.fedoraproject.org/en-US/epel/getting-started/)。

接下来，下载并安装 `ros2-release` 软件包：

```console
$ sudo dnf install curl
$ export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F'"' '{print $4}')
$ sudo dnf install "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-release-${ROS_APT_SOURCE_VERSION}-1.noarch.rpm"
```

[ros2-release](https://github.com/ros-infrastructure/ros-apt-source/) 软件包为各个 ROS 软件源提供密钥和软件源配置。当 ROS 软件源发布该软件包的新版本时，软件源配置也会自动更新。

<span id="install-ros-2-packages"></span>
<span id="rhel-install-rpms-install-ros-2-packages"></span>

## 安装 ROS 2 软件包

先按照[系统更新说明](_Dnf-Update-Admonition.md)更新系统。

桌面版安装（推荐）：包含 ROS、RViz、演示和教程。

```console
$ sudo dnf install ros-rolling-desktop
```

ROS-Base 安装（基础组件）：包含通信库、消息软件包和命令行工具，不含图形界面工具。

```console
$ sudo dnf install ros-rolling-ros-base
```

<span id="environment-setup"></span>

## 环境设置

<span id="sourcing-the-setup-script"></span>

### 加载设置脚本

通过加载以下文件配置环境：

```console
$ source /opt/ros/rolling/setup.bash
```

!!! note "说明"

    请根据所用 shell 选择相应的设置脚本扩展名。可用形式包括 `setup.bash`、`setup.sh` 和 `setup.zsh`。

<span id="try-some-examples"></span>

## 运行示例

如果前面安装了 `ros-rolling-desktop`，就可以运行一些示例。

在一个终端中加载设置文件，然后运行 C++ `talker`：

```console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp talker
```

在另一个终端中加载设置文件，然后运行 Python `listener`：

```console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_py listener
```

你应该看到 `talker` 输出 `Publishing`，表示正在发布消息；`listener` 输出 `I heard`，表示收到了这些消息。这说明 C++ 和 Python API 都在正常工作，恭喜！

如果希望使用其他 RMW 实现，请参阅[指南](RMW-Implementations.md)。

<span id="next-steps-after-installing"></span>

## 安装后的后续步骤

继续学习[教程和演示](../Tutorials.md)，配置环境，创建自己的工作空间和软件包，并了解 ROS 2 核心概念。

<span id="additional-rmw-implementations-optional"></span>

## 其他 RMW 实现（可选）

ROS 2 默认使用 `Fast DDS` 中间件，但可以在运行时切换中间件（RMW）。有关使用多个 RMW 的方法，请参阅[指南](../How-To-Guides/Working-with-multiple-RMW-implementations.md)。

<span id="troubleshooting"></span>

## 问题排查

参阅[安装问题排查](../How-To-Guides/Installation-Troubleshooting.md)。

<span id="uninstall"></span>

## 卸载

如果已经通过二进制软件包安装 ROS 2，现在需要卸载，或切换为源码安装，请运行：

```console
$ sudo dnf remove ros-rolling-*
```

要移除软件源配置，请运行：

```console
$ sudo dnf remove ros2-release
```
