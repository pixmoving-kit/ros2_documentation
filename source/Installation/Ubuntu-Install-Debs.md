<span id="ubuntu-deb-packages"></span>

# Ubuntu（deb 软件包）

目前为 Ubuntu Jammy（22.04）提供 ROS 2 Rolling Ridley 的 deb 软件包。目标平台定义见 [REP 2000](https://reps.openrobotics.org/rep-2000/)。

<span id="resources"></span>

## 资源

- ROS 2 Rolling（Ubuntu Jammy）状态页面：[amd64](http://repo.ros2.org/status_page/ros_rolling_default.html)、[arm64](http://repo.ros2.org/status_page/ros_rolling_ujv8.html)。
- [Jenkins 实例](http://build.ros2.org/)
- [软件源](http://repo.ros2.org)

<span id="set-locale"></span>

## 设置区域设置

按照 [Ubuntu 区域设置说明](_Ubuntu-Set-Locale.md)配置支持 UTF-8 的区域设置。

<span id="setup-sources"></span>
<span id="linux-install-debians-setup-sources"></span>

## 配置软件源

按照 [apt 软件源配置说明](_Apt-Repositories.md)添加软件源。

<span id="install-ros-2-packages"></span>
<span id="linux-install-debs-install-ros-2-packages"></span>

## 安装 ROS 2 软件包

配置软件源后，更新 apt 软件源缓存：

```console
$ sudo apt update
```

按照[系统更新说明](_Apt-Upgrade-Admonition.md)更新系统。

!!! warning "警告"

    由于 Ubuntu 22.04 早期更新中的问题，安装 ROS 2 前务必更新 `systemd` 和 `udev` 相关软件包。在刚安装、尚未升级的系统上安装 ROS 2 的依赖，可能导致**关键系统软件包被移除**。

    详情请参阅 [ros2/ros2#1272](https://github.com/ros2/ros2/issues/1272) 和 [Launchpad #1974196](https://bugs.launchpad.net/ubuntu/+source/systemd/+bug/1974196)。

桌面版安装（推荐）：包含 ROS、RViz、演示和教程。

```console
$ sudo apt install ros-rolling-desktop
```

ROS-Base 安装（基础组件）：包含通信库、消息软件包和命令行工具，不含图形界面工具。

```console
$ sudo apt install ros-rolling-ros-base
```

开发工具：用于构建 ROS 软件包的编译器及其他工具。

```console
$ sudo apt install ros-dev-tools
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

    如果使用的不是 Bash，请将 `.bash` 替换为对应 shell 的扩展名。可用形式包括 `setup.bash`、`setup.sh` 和 `setup.zsh`。

<span id="try-some-examples"></span>

## 运行示例

<span id="talker-listener"></span>

### Talker 与 Listener

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

<span id="using-the-ros-1-bridge"></span>

## 使用 ROS 1 桥接

ROS 1 桥接可以连接 ROS 1 和 ROS 2 的话题，实现双向通信。有关构建和使用方法，请参阅[专门的文档](https://github.com/ros2/ros1_bridge/blob/master/README.md)。

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
$ sudo apt remove '~nros-rolling-*' && sudo apt autoremove
```

也可以移除软件源：

```console
$ sudo apt remove ros2-apt-source
$ sudo apt update
$ sudo apt autoremove
$ sudo apt upgrade # Consider upgrading for packages previously shadowed.
```
