<span id="zenoh"></span>
# Zenoh

Zenoh 是一种开源通信协议和中间件，旨在实现异构系统之间的高效数据分发。它为高性能发布／订阅和分布式查询提供了位置透明的抽象。另请参见 [Zenoh 入门文档](https://zenoh.io/docs/getting-started/first-app/)。

<span id="prerequisites"></span>
## 前提条件

已[安装 rosdep](../../../Tutorials/Intermediate/Rosdep.md)。

<span id="installation-packages"></span>
## 安装软件包

Zenoh 的 RMW 实现可以通过二进制软件包安装，稳定开发环境推荐使用这种方式。

受支持的 ROS 2 发行版（见对应发行版分支）在各自的 Tier 1 平台上提供二进制软件包。首先，确保系统已经完成安装 ROS 2 二进制软件包所需的配置。

然后使用以下命令安装 rmw_zenoh 二进制软件包：

```bash
sudo apt install ros-rolling-rmw-zenoh-cpp
```

<span id="build-from-source-code"></span>
## 从源码构建

只有需要最新功能时，才推荐从源码构建。

默认情况下，会将 `zenoh-cpp` 作为随附的第三方依赖进行构建，并仅启用 Zenoh 的一部分功能。如需其他功能，可以覆盖 CMake 参数 `ZENOHC_CARGO_FLAGS`。详情见 [`zenoh_cpp_vendor/CMakeLists.txt`](https://github.com/ros2/rmw_zenoh/blob/rolling/zenoh_cpp_vendor/CMakeLists.txt)。

克隆仓库：

```bash
mkdir ~/ws_rmw_zenoh/src -p && cd ~/ws_rmw_zenoh/src
git clone https://github.com/ros2/rmw_zenoh.git -b rolling
```

安装依赖项：

```bash
cd ~/ws_rmw_zenoh
rosdep install --from-paths src --ignore-src --rosdistro rolling -y
```

使用 Colcon 构建工作空间：

```bash
source /opt/ros/rolling/setup.bash
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release
```

<span id="switch-to-rmw-zenoh-cpp"></span>
## 切换到 rmw_zenoh_cpp

设置以下环境变量，即可从其他 RMW 实现切换到 rmw_zenoh_cpp：

```bash
export RMW_IMPLEMENTATION=rmw_zenoh_cpp
```

<span id="run-the-talker-and-listener"></span>
## 运行 talker 和 listener

运行 `talker` 和 `listener`，测试 Zenoh。

先启动 Zenoh 路由器：

```bash
# terminal 1
source /opt/ros/rolling/setup.bash
ros2 run rmw_zenoh_cpp rmw_zenohd
```

!!! note "说明"
    如果没有 Zenoh 路由器，节点将无法相互发现，因为节点的会话配置默认禁用了组播发现。节点通过 Zenoh 路由器的 gossip 功能接收其他对等节点的发现信息。

```bash
# terminal 2
export RMW_IMPLEMENTATION=rmw_zenoh_cpp
source /opt/ros/rolling/setup.bash
ros2 run demo_nodes_cpp talker
```

```bash
# terminal 3
export RMW_IMPLEMENTATION=rmw_zenoh_cpp
source /opt/ros/rolling/setup.bash
ros2 run demo_nodes_cpp listener
```

!!! note "说明"
    运行这些命令前，记得加载（source）ROS 2 环境设置脚本。
