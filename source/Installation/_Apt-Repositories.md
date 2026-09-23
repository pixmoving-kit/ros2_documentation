---
translation_status: machine_translated
source: Installation/_Apt-Repositories.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

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
