需要将 ROS 2 的 apt 软件源添加到系统中。

首先，确保已启用 [Ubuntu Universe 软件源](https://help.ubuntu.com/community/Repositories/Ubuntu)。

```console
$ sudo apt install software-properties-common
$ sudo add-apt-repository universe
```

[ros-apt-source](https://github.com/ros-infrastructure/ros-apt-source/) 软件包为各个 ROS 软件源提供密钥和 apt 软件源配置。

安装 ros2-apt-source 软件包即可为系统配置 ROS 2 软件源。当 ROS 软件源发布该软件包的新版本时，软件源配置也会自动更新。

```console
$ sudo apt update && sudo apt install curl -y
$ export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F'"' '{print $4}')
$ curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
$ sudo dpkg -i /tmp/ros2-apt-source.deb
```
