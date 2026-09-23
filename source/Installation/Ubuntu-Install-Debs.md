---
translation_status: machine_translated
source: Installation/Ubuntu-Install-Debs.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="ubuntu-deb-packages"></span>

# Ubuntu（deb 软件包）

ROS 2 Rolling Ridley的Deb软件包目前可供Ubuntu Jammy使用(22.04)。 [REP 2000 环境方案](https://reps.openrobotics.org/rep-2000/).

<span id="resources"></span>

## 资源

- 状态页面 :

  - ROS 2 滚滚(Ubuntu Jammy): [amd64 (中文(简体) ).](http://repo.ros2.org/status_page/ros_rolling_default.html), [军火64](http://repo.ros2.org/status_page/ros_rolling_ujv8.html)

- [詹金斯实例](http://build.ros2.org/)

- [仓库](http://repo.ros2.org)

<span id="set-locale"></span>

## 设置区域

确定您有支持的地址 `UTF-8`。如果您处于一个最小的环境(例如一个插座容器),那么当地可能就是最小的环境,比如: `POSIX`。我们用以下设置进行测试。但是,如果您使用不同的UTF-8支持的语境,则应该没问题。

``` console
$ locale  # check for UTF-8

$ sudo apt update && sudo apt install locales
$ sudo locale-gen en_US en_US.UTF-8
$ sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
$ export LANG=en_US.UTF-8

$ locale  # verify settings
```

<span id="setup-sources"></span> <span id="linux-install-debians-setup-sources"></span>

## 设置来源

您需要将 ROS 2 apt 存储器添加到您的系统中 。

首先是确保 [Ubuntu 宇宙存储器](https://help.ubuntu.com/community/Repositories/Ubuntu) 已启用。

``` console
$ sudo apt install software-properties-common
$ sudo add-apt-repository universe
```

那个... [ros- apt 源码](https://github.com/ros-infrastructure/ros-apt-source/) 软件包为各种ROS寄存器提供密钥和适当的源配置。

安装 ros2- apt 源软件包将为您的系统配置 ROS 2 寄存器。 当此软件包的新版本发布到 ROS 寄存器时, 将自动更新到寄存器配置 。

``` console
$ sudo apt update && sudo apt install curl -y
$ export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F'"' '{print $4}')
$ curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
$ sudo dpkg -i /tmp/ros2-apt-source.deb
```

<span id="install-ros-2-packages"></span> <span id="linux-install-debs-install-ros-2-packages"></span>

## 安装 ROS 2 套件

在设置仓库后更新您的 apt 仓库缓存 。

``` console
$ sudo apt update
```

ROS 2 软件包建立在经常更新的 Ubuntu 系统上。总是建议您在安装新软件包之前确保您的系统是最新的。

``` console
$ sudo apt upgrade
```

> **警告**
>
> 由于Ubuntu22.04的早期更新,重要的是 `systemd` 财务报告和财务报告 `udev`- 相关包在安装ROS 2. 安装ROS 2依赖新安装的系统而不升级可触发 **关键系统包的删除**.
>
> 请参见: [ros2/ros2#1272](https://github.com/ros2/ros2/issues/1272) 财务报告和财务报告 [发射板 #174196](https://bugs.launchpad.net/ubuntu/+source/systemd/+bug/1974196) 以获取更多信息。

桌面安装(建议):ROS,RViz,演示,教程.

``` console
$ sudo apt install ros-rolling-desktop
```

ROS-Base安装(Bare Bones):通信库,消息包,命令行工具。没有GUI工具。

``` console
$ sudo apt install ros-rolling-ros-base
```

开发工具:构建ROS软件包的编译器和其他工具

``` console
$ sudo apt install ros-dev-tools
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
> 替换 `.bash` 如果您没有使用 bash , 请使用您的外壳 。 可能的值是 : `setup.bash`, `setup.sh`, `setup.zsh`.

<span id="try-some-examples"></span>

## 尝试一些例子

<span id="talker-listener"></span>

### 谈话者-听众

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

<span id="using-the-ros-1-bridge"></span>

## 使用 ROS 1 桥

ROS 1 桥可以连接 ROS 1 和 ROS 2 的主题,反之亦然。 [文档](https://github.com/ros2/ros1_bridge/blob/master/README.md) 如何建造和使用 ROS 1 桥。

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
$ sudo apt remove '~nros-rolling-*' && sudo apt autoremove
```

您也可能想要删除仓库 :

``` console
$ sudo apt remove ros2-apt-source
$ sudo apt update
$ sudo apt autoremove
$ sudo apt upgrade # Consider upgrading for packages previously shadowed.
```
