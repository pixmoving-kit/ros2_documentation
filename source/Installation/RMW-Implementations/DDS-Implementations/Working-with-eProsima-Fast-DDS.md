<span id="eprosima-fast-dds"></span>
# eProsima Fast DDS

eProsima Fast DDS 是一个完整的开源 DDS 实现，面向实时嵌入式架构和操作系统。更多信息见 [eProsima Fast DDS 产品页面](https://www.eprosima.com/index.php/products-all/eprosima-fast-dds)。

<span id="prerequisites"></span>
## 前提条件

已[安装 rosdep](../../../Tutorials/Intermediate/Rosdep.md)。

<span id="install-packages"></span>
## 安装软件包

最简单的方式是从 ROS 2 apt 仓库安装：

```console
$ sudo apt install ros-rolling-rmw-fastrtps-cpp
```

<span id="build-from-source-code"></span>
## 从源码构建

也可以通过构建源码来安装。

首先，将 Fast DDS 和 rmw_fastrtps 克隆到 ROS 2 工作空间的源码目录中：

```console
$ cd ros2_ws/src
$ git clone https://github.com/ros2/rmw_fastrtps ros2/rmw_fastrtps -b rolling
$ git clone https://github.com/eProsima/Fast-DDS eProsima/fastrtps
```

接着，安装 Fast DDS 所需的软件包：

```console
$ cd ..
$ rosdep install --from src -i
```

最后，运行 colcon 构建：

```console
$ colcon build --symlink-install
```

<span id="switch-to-rmw-fastrtps"></span>
## 切换到 rmw_fastrtps

可以通过设置以下环境变量选择 eProsima Fast DDS RMW：

```console
$ export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
```

另请参见[使用多种 RMW 实现](../../../How-To-Guides/Working-with-multiple-RMW-implementations.md)。

<span id="run-the-talker-and-listener"></span>
## 运行 talker 和 listener

运行 `talker` 和 `listener`，测试 Fast DDS：

```console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp talker
```

```console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp listener
```
