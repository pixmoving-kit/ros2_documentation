---
translation_status: machine_translated
source: Installation/RMW-Implementations/DDS-Implementations/Working-with-eProsima-Fast-DDS.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="eprosima-fast-dds"></span>

# eProsima Fast DDS

eProsima Fast DDS是实时嵌入式架构和操作系统的完整开源DDS执行. 另见: <https://www.eprosima.com/index.php/products-all/eprosima-fast-dds>

<span id="prerequisites"></span>

## 前提条件

有过 [已安装 rosdep](../../../Tutorials/Intermediate/Rosdep.md).

<span id="install-packages"></span>

## 安装软件包

最简单的方法是从 ROS 2 apt 仓库安装.

``` console
$ sudo apt install ros-rolling-rmw-fastrtps-cpp
```

<span id="build-from-source-code"></span>

## 从源代码构建

从源代码构建也是另一种安装方式.

首先,在ROS 2工作空间源目录中克隆快速DDS和rmw_fastrtps.

``` console
$ cd ros2_ws/src
$ git clone https://github.com/ros2/rmw_fastrtps ros2/rmw_fastrtps -b rolling
$ git clone https://github.com/eProsima/Fast-DDS eProsima/fastrtps
```

然后,为Fast DDS安装必要的软件包.

``` console
$ cd ..
$ rosdep install --from src -i
```

最后,运行colcon建设。

``` console
$ colcon build --symlink-install
```

<span id="switch-to-rmw-fastrtps"></span>

## 切换到 rmw\_ fastrps

eProsima Fast DDS RMW可以通过指定环境变量来选择:

``` console
$ export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
```

另见: [B. 与多项《保护移徙工人公约》的实施合作](../../../How-To-Guides/Working-with-multiple-RMW-implementations.md)

<span id="run-the-talker-and-listener"></span>

## 运行说话者和收听者

现在快跑 `talker` 财务报告和财务报告 `listener` 测试快速DS。

``` console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp talker
```

``` console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp listener
```
