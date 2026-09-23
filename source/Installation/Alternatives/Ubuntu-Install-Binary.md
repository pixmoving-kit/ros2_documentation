<span id="ubuntu-binary"></span>

# Ubuntu（二进制安装）

本页介绍如何通过预先构建好的二进制软件包，在 Ubuntu Linux 上安装 ROS 2。

!!! note "说明"

    预构建二进制包并不包含全部 ROS 2 软件包。它包含 [ROS base 变体](https://reps.openrobotics.org/rep-2001/#ros-base)中的所有软件包，以及 [ROS desktop 变体](https://reps.openrobotics.org/rep-2001/#desktop-variants)中的部分软件包。具体包含哪些软件包，由 [ros2.repos 文件](https://github.com/ros2/ros2/blob/rolling/ros2.repos)中列出的仓库决定。

也可以使用 [deb 软件包](../Ubuntu-Install-Debs.md)安装。

<span id="system-requirements"></span>

## 系统要求

目前支持 Ubuntu Linux Jammy（22.04）的 64 位 x86 和 64 位 ARM 平台。

<span id="add-the-ros-2-apt-repository"></span>

## 添加 ROS 2 apt 软件源

按照 [apt 软件源配置说明](../_Apt-Repositories.md)添加软件源。

<span id="downloading-ros-2"></span>

## 下载 ROS 2

- 前往[发布页面](https://github.com/ros2/ros2/releases)。
- 下载适用于 Ubuntu 的最新软件包。此处假设下载后的文件位于 `~/Downloads/ros2-package-linux-x86_64.tar.bz2`。注意：可能有多个二进制下载选项，因此实际文件名可能不同。
- 解压软件包：

```console
$ mkdir -p ~/ros2_rolling
$ cd ~/ros2_rolling
$ tar xf ~/Downloads/ros2-package-linux-x86_64.tar.bz2
```

<span id="installing-and-initializing-rosdep"></span>

## 安装并初始化 rosdep

```console
$ sudo apt update
$ sudo apt install -y python3-rosdep
$ sudo rosdep init
$ rosdep update
```

<span id="installing-the-missing-dependencies"></span>
<span id="linux-install-binary-install-missing-dependencies"></span>

## 安装缺失的依赖

先按照[系统更新说明](../_Apt-Upgrade-Admonition.md)更新系统。

根据所下载的发行版设置 rosdistro。

```bash
rosdep install --from-paths ~/ros2_rolling/ros2-linux/share --ignore-src -y --skip-keys "cyclonedds fastcdr fastrtps rti-connext-dds-6.0.1 urdfdom_headers"
```

使用 Linux Mint 等 Ubuntu 衍生发行版时，请参阅 [rosdep 注意事项](../_rosdep_Linux_Mint.md)。

<span id="install-development-tools-optional"></span>

### 安装开发工具（可选）

如果准备构建 ROS 软件包或进行其他开发工作，还可以安装开发工具：

```bash
sudo apt install ros-dev-tools
```

<span id="install-additional-dds-implementations-optional"></span>

### 安装其他 DDS 实现（可选）

如果希望使用默认供应商以外的 DDS 或 RTPS 实现，请参阅 [RMW 实现](../RMW-Implementations.md)。

<span id="environment-setup"></span>

## 环境设置

<span id="source-the-setup-script"></span>

### 加载设置脚本

通过加载以下文件配置环境：

```console
$ . ~/ros2_rolling/ros2-linux/setup.bash
```

!!! note "说明"

    如果使用的不是 Bash，请将 `.bash` 替换为对应 shell 的扩展名。可用形式包括 `setup.bash`、`setup.sh` 和 `setup.zsh`。

<span id="try-some-examples"></span>

## 运行示例

在一个终端中加载设置文件，然后运行 C++ `talker`：

```console
$ . ~/ros2_rolling/ros2-linux/setup.bash
$ ros2 run demo_nodes_cpp talker
```

在另一个终端中加载设置文件，然后运行 Python `listener`：

```console
$ . ~/ros2_rolling/ros2-linux/setup.bash
$ ros2 run demo_nodes_py listener
```

你应该看到 `talker` 输出 `Publishing`，表示正在发布消息；`listener` 输出 `I heard`，表示收到了这些消息。这说明 C++ 和 Python API 都在正常工作，恭喜！

<span id="next-steps-after-installing"></span>

## 安装后的后续步骤

继续学习[教程和演示](../../Tutorials.md)，配置环境，创建自己的工作空间和软件包，并了解 ROS 2 核心概念。

<span id="using-the-ros-1-bridge"></span>

## 使用 ROS 1 桥接

ROS 1 桥接可以连接 ROS 1 和 ROS 2 的话题，实现双向通信。有关构建和使用方法，请参阅[专门的文档](https://github.com/ros2/ros1_bridge/blob/master/README.md)。

<span id="additional-rmw-implementations-optional"></span>

## 其他 RMW 实现（可选）

ROS 2 默认使用 `Fast DDS` 中间件，但可以在运行时切换中间件（RMW）。有关使用多个 RMW 的方法，请参阅[指南](../../How-To-Guides/Working-with-multiple-RMW-implementations.md)。

<span id="troubleshooting"></span>

## 问题排查

参阅[安装问题排查](../../How-To-Guides/Installation-Troubleshooting.md)。

<span id="uninstall"></span>

## 卸载

1. 如果按照以上说明使用 colcon 安装了工作空间，只需打开一个新终端，不加载工作空间的 `setup` 文件，就可以视为“卸载”。这样，当前环境的行为就如同系统未安装 Rolling 一样。
2. 如果还希望释放空间，可以删除整个工作空间目录：

```console
$ rm -rf ~/ros2_rolling
```
