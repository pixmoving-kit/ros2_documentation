---
translation_status: machine_translated
source: How-To-Guides/Installing-on-Raspberry-Pi.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="ros-2-on-raspberry-pi"></span>

# 在 Raspberry Pi 上使用 ROS 2

在32位(arm32)和64位(arm64)ARM处理器上都支持ROS 2。但是,你可以看到 [这儿](https://reps.openrobotics.org/rep-2000/) arm64得到第一级支持,而arm32是第三级支持,而第一级支持意味着分配特定软件包和二进制档案可用,而第三级则要求用户从源头编译ROS 2.

使用ROS 2的最快和最简单的方式是使用第一级支持的配置.

这意味着要要么安装64位Ubuntu到Raspberry Pi,要么使用64位版本的Raspberry Pi OS,并在Docker运行ROS 2.

<span id="ubuntu-linux-on-raspberry-pi-with-binary-ros-2-install"></span>

## Ubuntu Linux 在Raspberry Pi上安装二进制 ROS 2

Raspberry Pi 的 Ubuntu 可用工具 [这儿](https://ubuntu.com/download/raspberry-pi).

请确认您选择了以下描述的正确版本: [REP-2000号报告](https://reps.openrobotics.org/rep-2000/).

现在可以使用普通的Ubuntu Linux二进制安装指令安装ROS 2.

<span id="raspberry-pi-os-with-ros-2-in-docker"></span>

## 树莓 Pi OS, 涂装为 ROS 2

Raspberry Pi OS 64 比特版本为 [此处可用](https://www.raspberrypi.com/software/operating-systems/).

Raspberry Pi OS是基于Debian,它得到第三级支持,但它可以为第一级支持运行Ubuntu docker容器.

闪烁了OS后, [安装嵌入器](https://docs.docker.com/engine/install/debian/#install-using-the-convenience-script).

官方 ROS 2 Docker 图像可以找到 [这儿](https://hub.docker.com/_/ros/tags).

您可以从 ros- core 、 ros- base 或 感知中选择 。 [这儿](https://reps.openrobotics.org/rep-2001/) 关于这些变种的更多信息。

获取并运行图像 :

``` console
$ docker pull ros:rolling-ros-core
$ docker run -it --rm ros:rolling-ros-core
```

您也可以自己构建图像 :

切开 [docker_images git repo 图像重播](https://github.com/osrf/docker_images) 在 Raspberry Pi 上,修改为上面链接的目录,然后修改为您喜欢的变体目录。

在目录内,以下列方式构建容器:

``` console
$ docker build -t ros_docker .
```

在被支持的系统上,只需要一两分钟就可以建造插头容器,因为源代码已经建在二进制中.
