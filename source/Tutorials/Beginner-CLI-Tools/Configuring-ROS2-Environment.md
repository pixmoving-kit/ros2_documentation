<span id="configuring-environment"></span> <span id="configros2"></span>
# 配置环境

**目标：** 学习如何准备 ROS 2 环境。

**教程级别：** 初学者

**预计用时：** 5 分钟

<span id="background"></span>
## 背景

ROS 2 通过 shell 环境将多个工作空间组合起来。在 ROS 中，“工作空间”是指系统中用于开展 ROS 2 开发的目录。在典型的 ROS 2 配置中，核心 ROS 2 安装是底层工作空间（underlay）。在加载这套安装的环境之后，再加载本地工作空间的环境，本地工作空间就成为叠加在底层之上的上层工作空间（overlay）。同一个工作空间也可以作为后续加载的其他工作空间的底层。开发 ROS 2 程序时，通常会同时启用多个工作空间。

组合工作空间可以方便地针对不同版本的 ROS 2 或不同的软件包集合进行开发，还允许在同一台计算机上安装多个 ROS 2 发行版（简称 distro，例如 Dashing 和 Eloquent），并在它们之间切换。

实现方法是每次打开新的 shell 时加载（source）环境设置文件，或者将相应的 source 命令添加到 shell 启动脚本中。没有加载这些文件，就无法使用 ROS 2 命令，也无法查找或使用 ROS 2 软件包。也就是说，无法使用 ROS 2。

<span id="prerequisites"></span>
## 前提条件

开始这些教程之前，请按照 [ROS 2 安装页面](../../Installation.md)中的说明安装 ROS 2。

本教程中的命令假定你已按照操作系统对应的二进制软件包安装指南完成安装（Linux 使用 deb 包）。如果你是从源码构建 ROS 2，也可以学习本教程，但环境设置文件的路径可能不同。此外，源码安装方式无法使用初学者教程中经常出现的 `sudo apt install ros-<distro>-<package>` 命令。

如果你使用 Linux 或 macOS，但还不熟悉 shell，可以先阅读[这篇教程](https://www.linux.com/training-tutorials/bash-101-working-cli/)。

<span id="tasks"></span>
## 操作步骤

<span id="source-the-setup-files"></span>
### 1 加载环境设置文件

每打开一个新的 shell，都需要运行相应命令，才能使用 ROS 2 命令：

**Linux**

```console
$ source /opt/ros/rolling/setup.bash
```

如果使用的不是 bash，请将 `.bash` 替换为所用 shell 对应的后缀。可用文件为 `setup.bash`、`setup.sh` 和 `setup.zsh`。

**macOS**

```console
$ . ~/ros2_install/ros2-osx/setup.bash
```

**Windows**

```console
$ call C:\dev\ros2\local_setup.bat
```

!!! note "注意"
    具体命令取决于 ROS 2 的安装位置。如果出现问题，请确认文件路径指向实际的安装目录。

<span id="add-sourcing-to-your-shell-startup-script"></span>
### 2 将加载命令添加到 shell 启动脚本

如果不希望每次打开新的 shell 都手动加载环境设置文件，可以将命令添加到 shell 启动脚本中，这样就可以省去步骤 1：

**Linux**

```console
$ echo "source /opt/ros/rolling/setup.bash" >> ~/.bashrc
```

要撤销此设置，请找到系统的 shell 启动脚本，删除追加的 source 命令。

**macOS**

```console
$ echo "source ~/ros2_install/ros2-osx/setup.bash" >> ~/.bash_profile
```

要撤销此设置，请找到系统的 shell 启动脚本，删除追加的 source 命令。

**Windows**

以下操作仅适用于 PowerShell 用户。在“我的文档”中创建 `WindowsPowerShell` 文件夹，再在其中创建 `Microsoft.PowerShell_profile.ps1` 文件，将以下内容粘贴到文件中：

```console
$ C:\dev\ros2_rolling\local_setup.ps1
```

每次打开新的 shell 时，PowerShell 都会请求运行此脚本的权限。可以运行以下命令，避免反复询问：

```console
$ Unblock-File C:\dev\ros2_rolling\local_setup.ps1
```

要撤销此设置，请删除新建的 `Microsoft.PowerShell_profile.ps1` 文件。

<span id="check-environment-variables"></span>
### 3 检查环境变量

加载 ROS 2 环境设置文件后，会设置运行 ROS 2 所需的若干环境变量。如果查找或使用 ROS 2 软件包时遇到问题，请使用以下命令检查环境是否配置正确：

**Linux**

```console
$ printenv | grep -i ROS
```

**macOS**

```console
$ printenv | grep -i ROS
```

**Windows**

```console
$ set | findstr -i ROS
```

确认 `ROS_DISTRO`、`ROS_VERSION` 等变量已经设置：

```text
ROS_VERSION=2
ROS_PYTHON_VERSION=3
ROS_DISTRO=rolling
```

如果环境变量设置不正确，请返回你使用的安装指南中的 ROS 2 软件包安装部分。由于环境设置文件可能来自不同位置，如果需要更具体的帮助，可以[向社区提问](https://robotics.stackexchange.com/)。

<span id="the-ros-domain-id-variable"></span>
#### 3.1 ROS_DOMAIN_ID 变量

有关 ROS 域 ID 的详细说明，请参阅[域 ID](../../Concepts/Intermediate/About-Domain-ID.md)。

为这一组 ROS 2 节点选定一个唯一的整数后，可以用以下命令设置环境变量：

**Linux**

```console
$ export ROS_DOMAIN_ID=<your_domain_id>
```

要让设置在后续 shell 会话中继续生效，可以将命令添加到 shell 启动脚本：

```console
$ echo "export ROS_DOMAIN_ID=<your_domain_id>" >> ~/.bashrc
```

**macOS**

```console
$ export ROS_DOMAIN_ID=<your_domain_id>
```

要让设置在后续 shell 会话中继续生效，可以将命令添加到 shell 启动脚本：

```console
$ echo "export ROS_DOMAIN_ID=<your_domain_id>" >> ~/.bash_profile
```

**Windows**

```console
$ set ROS_DOMAIN_ID=<your_domain_id>
```

如果希望该设置在后续 shell 会话中持续生效，还需运行：

```console
$ setx ROS_DOMAIN_ID <your_domain_id>
```

<span id="the-ros-localhost-only-variable"></span>
#### 3.2 ROS_LOCALHOST_ONLY 变量

默认情况下，ROS 2 通信并不限于本机。`ROS_LOCALHOST_ONLY` 环境变量可以将 ROS 2 通信限制在 localhost，也就是本机范围内。这样，局域网内的其他计算机就无法看到你的 ROS 2 系统及其话题、服务和动作。在课堂等场景中，多个机器人可能向同一话题发布数据并引发异常行为，此时 `ROS_LOCALHOST_ONLY` 就很有用。可以用以下命令设置该环境变量：

**Linux**

```console
export ROS_LOCALHOST_ONLY=1
```

要让设置在后续 shell 会话中继续生效，可以将命令添加到 shell 启动脚本：

```console
echo "export ROS_LOCALHOST_ONLY=1" >> ~/.bashrc
```

**macOS**

```console
export ROS_LOCALHOST_ONLY=1
```

要让设置在后续 shell 会话中继续生效，可以将命令添加到 shell 启动脚本：

```console
echo "export ROS_LOCALHOST_ONLY=1" >> ~/.bash_profile
```

**Windows**

```console
set ROS_LOCALHOST_ONLY=1
```

如果希望该设置在后续 shell 会话中持续生效，还需运行：

```console
setx ROS_LOCALHOST_ONLY 1
```

<span id="summary"></span>
## 小结

使用 ROS 2 开发环境前，需要正确配置环境。有两种方式：在每个新打开的 shell 中加载环境设置文件，或者将 source 命令添加到启动脚本中。

如果在 ROS 2 中查找或使用软件包时遇到问题，首先应检查环境变量，确认它们指向你想使用的版本和发行版。

<span id="next-steps"></span>
## 后续步骤

现在 ROS 2 已经安装好，你也掌握了加载环境设置文件的方法，接下来可以使用 [turtlesim 工具](Introducing-Turtlesim/Introducing-Turtlesim.md)进一步学习 ROS 2。
