---
translation_status: machine_translated
source: Installation/RMW-Implementations/DDS-Implementations/Working-with-Eclipse-CycloneDDS.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="eclipse-cyclone-dds"></span>

# Eclipse Cyclone DDS

Eclipse Circle DDS是一个非常有性能和强大的开源DDS执行. Circle DDS是作为一个Eclipse IoT项目在开放处完全开发的. See: <https://projects.eclipse.org/projects/iot.cyclonedds>

<span id="prerequisites"></span>

## 前提条件

有过 [已安装 rosdep](../../../Tutorials/Intermediate/Rosdep.md).

<span id="install-packages"></span>

## 安装软件包

最简单的方法是从 ROS 2 apt 仓库安装.

``` console
$ sudo apt install ros-rolling-rmw-cyclonedds-cpp
```

<span id="build-from-source-code"></span>

## 从源代码构建

从源代码构建也是另一种安装方式.

首先,在ROS 2 工作空间源目录中克隆 Circon DDS 和 rmw\_ cyclonedds。要确定要退出的正确分支,需要找到您指定的版本 [ROS 发行的 ros2. repos 文件](https://raw.githubusercontent.com/ros2/ros2/refs/heads/rolling/ros2.repos).

或者,您可以运行以下代码来获取气旋DDS所需的正确分支/tag :

``` console
$ CYCLONEDDS_BRANCH=$(curl -s https://raw.githubusercontent.com/ros2/ros2/refs/heads/rolling/ros2.repos | grep -A 3 "eclipse-cyclonedds/cyclonedds:" | grep "version:" | awk '{print $2}')
```

现在,复制和检查代码:

``` console
$ cd ros2_ws/src
$ git clone https://github.com/ros2/rmw_cyclonedds ros2/rmw_cyclonedds -b rolling
$ git clone https://github.com/eclipse-cyclonedds/cyclonedds eclipse-cyclonedds/cyclonedds -b ${CYCLONEDDS_BRANCH}
```

然后,为气旋DDS安装必要的软件包.

``` console
$ cd ..
$ rosdep install --from src -i
```

最后,运行colcon建设。

``` console
$ colcon build --symlink-install
```

<span id="switch-to-rmw-cyclonedds"></span>

## 切换到 rmw\_ cycloneds

通过指定环境变量从其他rmw切换到rmw_cycloneds.

``` console
$ export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
```

另见: [B. 与多项《保护移徙工人公约》的实施合作](../../../How-To-Guides/Working-with-multiple-RMW-implementations.md)

<span id="run-the-talker-and-listener"></span>

## 运行说话者和收听者

现在快跑 `talker` 财务报告和财务报告 `listener` 测试气旋DDS。

``` console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp talker
```

``` console
$ source /opt/ros/rolling/setup.bash
$ ros2 run demo_nodes_cpp listener
```
