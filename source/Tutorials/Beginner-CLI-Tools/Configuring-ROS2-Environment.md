---
translation_status: machine_translated
source: Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="configuring-environment"></span> <span id="configros2"></span>

# 配置环境

**目标：** 此教程将显示您如何准备 ROS 2 环境 。

**教程级别：** 入门

**用时：** 5分钟

<span id="background"></span>

## 背景

ROS 2 依赖于使用外壳环境合并工作空间的概念。 “ Workspace” 是您系统所在位置的 ROS 术语。 在典型的 ROS 2 设置中, 核心 ROS 2 安装为内衬。 安装后源的本地工作空间是外衬, 因为内衬上层。 同样的工作空间可以充当后期源的另一工作空间的内衬。 当使用 ROS 2 开发时, 您通常会同时有多个工作空间活动 。

结合工作空间可以比较容易地针对ROS 2的不同版本或不同的套件进行开发,还可以在同一台计算机上安装几个ROS 2 分布(或"Distros",如Dashing和Eloatent)并在它们之间切换.

这是通过每次打开新 shell 或将源指令添加到您的 shell 启动脚本中一次来获取设置文件来实现的。 如果不获取设置文件, 您将无法访问 ROS 2 命令, 也无法找到或使用 ROS 2 软件包。 换句话说, 您将无法使用 ROS 2 。

<span id="prerequisites"></span>

## 前提条件

在开始这些教程前, 遵循 ROS 2 上的指示安装 ROS 2 [安装](../../Installation.md) 页面。

此教程中使用的命令假设您遵循操作系统的二进制包安装指南( 用于 Linux 的 DEb 包) 。 如果您从源头构建的话, 您仍然可以遵循, 但是您的设置文件的路径可能不同 。 您也将无法使用 。 `sudo apt install ros-<distro>-<package>` 如果从源头安装,则命令(在初学者级教程中经常使用)。

如果你正在使用 Linux 或 macOS, 但还没有熟悉的外壳, [此教程](https://www.linux.com/training-tutorials/bash-101-working-cli/) 将会有所帮助。

<span id="tasks"></span>

## 操作步骤

<span id="source-the-setup-files"></span>

### 1 来源设置文件

您需要在您打开的每个新 shell 上运行此命令, 才能访问 ROS 2 命令, 例如 :

##### Linux

``` console
$ source /opt/ros/rolling/setup.bash
```

替换 `.bash` 如果您没有使用 bash , 请使用您的外壳 。 可能的值是 : `setup.bash`, `setup.sh`, `setup.zsh`.

##### macOS

``` console
$ . ~/ros2_install/ros2-osx/setup.bash
```

##### Windows

``` console
$ call C:\dev\ros2\local_setup.bat
```

> **说明**
>
> 准确的命令取决于您安装 ROS 2. 如果您有问题, 请确保文件路径通向您的安装 。

<span id="add-sourcing-to-your-shell-startup-script"></span>

### 2 在您的 shell 启动脚本中添加来源

如果您不想每次打开新 shell( skipping story 1) 时都要源代码设置文件, 那么您就可以将命令添加到您的 shell 启动脚本 :

##### Linux

``` console
$ echo "source /opt/ros/rolling/setup.bash" >> ~/.bashrc
```

要撤销此选项, 请找到您的系统 shell 启动脚本并删除附加的源命令 。

##### macOS

``` console
$ echo "source ~/ros2_install/ros2-osx/setup.bash" >> ~/.bash_profile
```

要撤销此选项, 请找到您的系统 shell 启动脚本并删除附加的源命令 。

##### Windows

仅针对 PowerShell 用户, 在“ My Documents” 中创建一个名为“ WindowsPowerShell ” 的文件夹。 在“ WindowsPowerShell ” 中, 创建文件“ 微软 PowerShell\_ profile.ps1 ” 。 文件内部粘贴 :

``` console
$ C:\dev\ros2_rolling\local_setup.ps1
```

PowerShell 每次打开新 shell 时都会请求允许运行此脚本。 为避免这个问题, 您可以运行 :

``` console
$ Unblock-File C:\dev\ros2_rolling\local_setup.ps1
```

要取消此选项, 请删除新的 \` Microsoft. PowerShell\_ profile. ps1 ' 文件 。

<span id="check-environment-variables"></span>

### 3 检查环境变量

搜索ROS 2 设置文件会设置操作ROS 2 所需的几个环境变量. 如果您在查找或使用ROS 2 软件包时遇到问题,请使用以下命令确保您的环境设置得当:

##### Linux

``` console
$ printenv | grep -i ROS
```

##### macOS

``` console
$ printenv | grep -i ROS
```

##### Windows

``` console
$ set | findstr -i ROS
```

检查变量像 `ROS_DISTRO` 财务报告和财务报告 `ROS_VERSION` 已经设定。

``` default
ROS_VERSION=2
ROS_PYTHON_VERSION=3
ROS_DISTRO=rolling
```

如果环境变量设置不正确, 请返回您所遵循的安装指南中的 ROS 2 包安装部分。 如果您需要更具体的帮助( 因为环境设置文件可以来自不同的地方) , 您可以 [获取答案](https://robotics.stackexchange.com/) 从社区。

<span id="the-ros-domain-id-variable"></span>

#### 3.1 请求: `ROS_DOMAIN_ID` 变量

见 [域名标识](../../Concepts/Intermediate/About-Domain-ID.md) 关于ROS域名标识的细节的文章。

一旦您为您的组ROS 2 节点确定了一个独特的整数,您就可以设置环境变量,其命令如下:

##### Linux

``` console
$ export ROS_DOMAIN_ID=<your_domain_id>
```

要在 shell 会话之间保持此设置, 您可以在您的 shell 启动脚本中添加命令 :

``` console
$ echo "export ROS_DOMAIN_ID=<your_domain_id>" >> ~/.bashrc
```

##### macOS

``` console
$ export ROS_DOMAIN_ID=<your_domain_id>
```

要在 shell 会话之间保持此设置, 您可以在您的 shell 启动脚本中添加命令 :

``` console
$ echo "export ROS_DOMAIN_ID=<your_domain_id>" >> ~/.bash_profile
```

##### Windows

``` console
$ set ROS_DOMAIN_ID=<your_domain_id>
```

如果您想在 shell 会话间使这个永久化, 也运行 :

``` console
$ setx ROS_DOMAIN_ID <your_domain_id>
```

<span id="the-ros-localhost-only-variable"></span>

#### 3.2 检查 `ROS_LOCALHOST_ONLY` 变量

默认情况下,ROS 2通信并不限于localhost. `ROS_LOCALHOST_ONLY` 环境变量允许您只将 ROS 2 通信限制到本地主机。 这意味着您的 ROS 2 系统, 以及它的主题、 服务和动作不会被本地网络上的其他计算机看到 。 使用 。 `ROS_LOCALHOST_ONLY` 在某些环境(如教室)中很有帮助,因为多个机器人可能会发布到同一话题上,引起奇怪的行为。您可以设置环境变量,并使用以下命令 :

##### Linux

``` console
export ROS_LOCALHOST_ONLY=1
```

要在 shell 会话之间保持此设置, 您可以在您的 shell 启动脚本中添加命令 :

``` console
echo "export ROS_LOCALHOST_ONLY=1" >> ~/.bashrc
```

##### macOS

``` console
export ROS_LOCALHOST_ONLY=1
```

要在 shell 会话之间保持此设置, 您可以在您的 shell 启动脚本中添加命令 :

``` console
echo "export ROS_LOCALHOST_ONLY=1" >> ~/.bash_profile
```

##### Windows

``` console
set ROS_LOCALHOST_ONLY=1
```

如果您想在 shell 会话间使这个永久化, 也运行 :

``` console
setx ROS_LOCALHOST_ONLY 1
```

<span id="summary"></span>

## 小结

ROS 2 开发环境在使用前需要正确配置。 这可以通过两种方式实现: 要么从您打开的每个新 shell 中获取设置文件, 要么将源指令添加到您的启动脚本中 。

如果您在定位或使用 ROS 2 软件包时遇到任何问题, 您应该做的第一件事就是检查您的环境变量, 并确保它们被设定到您想要的版本和演示 。

<span id="next-steps"></span>

## 后续步骤

现在,你有一个工作 ROS 2 的安装,你知道如何源 它的设置文件, 你可以开始学习 ROS 2 的 进出 [龟兹工具](Introducing-Turtlesim/Introducing-Turtlesim.md).
