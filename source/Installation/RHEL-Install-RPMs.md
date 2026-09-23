---
translation_status: machine_translated
source: Installation/RHEL-Install-RPMs.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="rhel-rpm-packages"></span>

# RHEL（RPM 软件包）

ROS 2 Rolling Ridley的RPM软件包目前可供 RHEL 8. 目标平台定义于 [REP 2000 环境方案](https://reps.openrobotics.org/rep-2000/).

<span id="resources"></span>

## 资源

- 状态页面 :

  - ROS 2滚(RHEL 8): [amd64 (中文(简体) ).](http://repo.ros2.org/status_page/ros_rolling_rhel.html)

- [詹金斯实例](http://build.ros2.org/)

- [仓库](http://repo.ros2.org)

<span id="set-locale"></span>

## 设置区域

确定您有支持的地址 `UTF-8`。如果您处于一个最小的环境(例如一个插座容器),那么当地可能就是最小的环境,比如: `C`。我们用以下设置进行测试。但是,如果您使用不同的UTF-8支持的语境,则应该没问题。

``` console
$ locale  # check for UTF-8

$ sudo dnf install langpacks-en glibc-langpack-en
$ export LANG=en_US.UTF-8

$ locale  # verify settings
```

<span id="setup-sources"></span> <span id="rhel-install-rpms-setup-sources"></span>

## 设置来源

您需要启用 EPEL 寄存器和 PowerTools 寄存器 :

``` console
$ sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-$(rpm -E %rhel).noarch.rpm
$ sudo env FORCE_DNF=1 crb enable
```

> **说明**
>
> 这个步骤可能因您使用的分布而略有不同 。 [检查 EPEL 文档](https://docs.fedoraproject.org/en-US/epel/getting-started/)

下一个,下载 `ros2-release` 包并安装它 :

``` console
$ sudo dnf install curl
$ export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F'"' '{print $4}')
$ sudo dnf install "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-release-${ROS_APT_SOURCE_VERSION}-1.noarch.rpm"
```

那个... [rOS2 释放](https://github.com/ros-infrastructure/ros-apt-source/) 软件包为各种ROS寄存器提供密钥和重传配置。当此软件包的新版本发布到ROS寄存器时,更新到寄存器配置会自动发生。

<span id="install-ros-2-packages"></span> <span id="rhel-install-rpms-install-ros-2-packages"></span>

## 安装 ROS 2 套件

ROS 2 软件包建立在经常更新的 RHEL 系统上。总是建议您在安装新软件包之前确保您的系统更新。

``` console
$ sudo dnf update
```

桌面安装(建议):ROS,RViz,演示,教程.

``` console
$ sudo dnf install ros-rolling-desktop
```

ROS-Base安装(Bare Bones):通信库,消息包,命令行工具。没有GUI工具。

``` console
$ sudo dnf install ros-rolling-ros-base
```

<span id="environment-setup"></span>

## 环境设置

<span id="sourcing-the-setup-script"></span>

### 测试设置脚本

通过获取以下文件来设置您的环境 。

``` console
$ source /opt/ros/rolling/setup.bash
```

> **说明**
>
> 替换 `.bash` 如果您不使用控制台, 则使用您的外壳。 可能的值是 : `setup.bash`, `setup.sh`, `setup.zsh`.

<span id="try-some-examples"></span>

## 尝试一些例子

如果您安装了 `ros-rolling-desktop` 上面可以举几个例子。

在一个终端中, 源代码设置文件, 然后运行 C++ `talker`:

``` console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp talker
```

在另一个终端源代码中, 设置文件然后运行 Python `listener`:

``` console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_py listener
```

你应该看看 `talker` 说,这是 `Publishing` 信件和资料 `listener` 说 `I heard` 这证明C++和Python API都正常工作。万岁!

如果你想使用其他 RMW 执行,你可以检查 [指南](RMW-Implementations.md).

<span id="next-steps-after-installing"></span>

## 安装后的下一步

继续 [教程和演示](../Tutorials.md) 来配置环境,创建自己的工作空间和软件包,并学习ROS 2核心概念。

<span id="additional-rmw-implementations-optional"></span>

## 更多落实RMW(可选)

ROS 2 使用的默认中间软件是 `Fast DDS`,但中间软件(RMW)可以在运行时替换。 [指南](../How-To-Guides/Working-with-multiple-RMW-implementations.md) 如何与多个RMW合作。

<span id="troubleshooting"></span>

## 麻烦的解决

可以找到解决问题的技巧 [这儿](../How-To-Guides/Installation-Troubleshooting.md).

<span id="uninstall"></span>

## 卸载

如果您需要卸载ROS 2, 或一旦从二进制安装完毕, 就切换到基于源的安装, 请运行以下命令 :

``` console
$ sudo dnf remove ros-rolling-*
```

删除仓库配置运行

``` console
$ sudo dnf remove ros2-release
```
