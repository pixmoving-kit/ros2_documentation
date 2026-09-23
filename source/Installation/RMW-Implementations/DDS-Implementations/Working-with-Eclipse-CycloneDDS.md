<span id="eclipse-cyclone-dds"></span>
# Eclipse Cyclone DDS

Eclipse Cyclone DDS 是一个性能出色、健壮的开源 DDS 实现。它作为 Eclipse IoT 项目，采用完全开放的开发方式。更多信息见 [Eclipse 项目页面](https://projects.eclipse.org/projects/iot.cyclonedds)。

<span id="prerequisites"></span>
## 前提条件

已[安装 rosdep](../../../Tutorials/Intermediate/Rosdep.md)。

<span id="install-packages"></span>
## 安装软件包

最简单的方式是从 ROS 2 apt 仓库安装：

```console
$ sudo apt install ros-rolling-rmw-cyclonedds-cpp
```

<span id="build-from-source-code"></span>
## 从源码构建

也可以通过构建源码来安装。

首先，将 Cyclone DDS 和 rmw_cyclonedds 克隆到 ROS 2 工作空间的源码目录中。要确定应检出的分支，请查看[所用 ROS 发行版的 ros2.repos 文件](https://raw.githubusercontent.com/ros2/ros2/refs/heads/rolling/ros2.repos)中指定的版本。

也可以运行以下命令，获取 Cyclone DDS 所需的分支或标签：

```console
$ CYCLONEDDS_BRANCH=$(curl -s https://raw.githubusercontent.com/ros2/ros2/refs/heads/rolling/ros2.repos | grep -A 3 "eclipse-cyclonedds/cyclonedds:" | grep "version:" | awk '{print $2}')
```

然后克隆代码并检出对应版本：

```console
$ cd ros2_ws/src
$ git clone https://github.com/ros2/rmw_cyclonedds ros2/rmw_cyclonedds -b rolling
$ git clone https://github.com/eclipse-cyclonedds/cyclonedds eclipse-cyclonedds/cyclonedds -b ${CYCLONEDDS_BRANCH}
```

接着，安装 Cyclone DDS 所需的软件包：

```console
$ cd ..
$ rosdep install --from src -i
```

最后，运行 colcon 构建：

```console
$ colcon build --symlink-install
```

<span id="switch-to-rmw-cyclonedds"></span>
## 切换到 rmw_cyclonedds

设置以下环境变量，即可从其他 RMW 实现切换到 rmw_cyclonedds：

```console
$ export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
```

另请参见[使用多种 RMW 实现](../../../How-To-Guides/Working-with-multiple-RMW-implementations.md)。

<span id="run-the-talker-and-listener"></span>
## 运行 talker 和 listener

运行 `talker` 和 `listener`，测试 Cyclone DDS：

```console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp talker
```

```console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp listener
```
