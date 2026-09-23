<span id="ros-2-on-raspberry-pi"></span>
# 在 Raspberry Pi 上使用 ROS 2

ROS 2 同时支持 32 位（arm32）和 64 位（arm64）ARM 处理器。不过，[支持平台说明](https://reps.openrobotics.org/rep-2000/)中列出：arm64 属于 Tier 1 支持级别，arm32 属于 Tier 3。Tier 1 提供适用于发行版的软件包和二进制归档；Tier 3 则需要用户从源码编译 ROS 2。

使用 ROS 2 最快捷、最简单的方式是选择 Tier 1 支持的配置。

也就是说，可以在 Raspberry Pi 上安装 64 位 Ubuntu，或使用 64 位 Raspberry Pi OS 并在 Docker 中运行 ROS 2。

<span id="ubuntu-linux-on-raspberry-pi-with-binary-ros-2-install"></span>
## 在 Raspberry Pi 的 Ubuntu Linux 上安装 ROS 2 二进制包

可从[这里](https://ubuntu.com/download/raspberry-pi)下载适用于 Raspberry Pi 的 Ubuntu。

请按照 [REP-2000](https://reps.openrobotics.org/rep-2000/) 确认所选版本正确。

之后即可按照 Ubuntu Linux 的常规二进制安装说明安装 ROS 2。

<span id="raspberry-pi-os-with-ros-2-in-docker"></span>
## 在 Raspberry Pi OS 上通过 Docker 使用 ROS 2

可从[这里](https://www.raspberrypi.com/software/operating-systems/)下载 64 位 Raspberry Pi OS。

Raspberry Pi OS 基于 Debian，属于 Tier 3 支持范围；不过，它可以运行 Ubuntu Docker 容器，从而使用 Tier 1 支持的环境。

刷入操作系统后，[安装 Docker](https://docs.docker.com/engine/install/debian/#install-using-the-convenience-script)。

ROS 2 官方 Docker 镜像见[这里](https://hub.docker.com/_/ros/tags)。

可以选择 ros-core、ros-base 或 perception。有关这些变体的更多信息，请参阅 [REP-2001](https://reps.openrobotics.org/rep-2001/)。

拉取并运行镜像：

```console
$ docker pull ros:rolling-ros-core
$ docker run -it --rm ros:rolling-ros-core
```

也可以自行构建镜像。

将 [docker_images Git 仓库](https://github.com/osrf/docker_images)克隆到 Raspberry Pi，进入上述链接所指的目录，再进入所需变体的目录。

在该目录中构建容器镜像：

```console
$ docker build -t ros_docker .
```

在受支持的系统上，构建这些 Docker 容器只需一两分钟，因为源码已经编译成二进制文件。
