---
translation_status: machine_translated
source: Installation/RMW-Implementations/Non-DDS-Implementations/Working-with-Zenoh.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="zenoh"></span>

# Zenoh

Zenoh是一种开源通信协议和中间软件,旨在便利不同系统之间的有效数据分布。它为高性能的pub/sub和分布式查询提供位置透明抽象。另见: <https://zenoh.io/docs/getting-started/first-app/>

<span id="prerequisites"></span>

## 前提条件

有过 [已安装 rosdep](../../../Tutorials/Intermediate/Rosdep.md).

<span id="installation-packages"></span>

## 安装软件包

rmw执行Zenoh可以通过二进制安装,推荐稳定发展.

支持的ROS 2 分布的二进制软件包( 参见 Distro 分支) 可以在相应的 Tier-1 平台上为分布提供。 首先, 请遵循这里的指示, 确保您的系统安装 ROS 2 的二进制 。

然后使用命令安装 rmw_zenoh 二进制

``` bash
sudo apt install ros-rolling-rmw-zenoh-cpp
```

<span id="build-from-source-code"></span>

## 从源代码构建

只有在需要最新特征的情况下,才建议从源头建造。

默认情况下,我们销售和编译 `zenoh-cpp` 带有 Zenoh 特性的子集。 `ZENOHC_CARGO_FLAGS` CMake 参数可能随需要包含的其他特性而覆盖。见 [zenoh_cpp_vendor/CMakeLists.txt](https://github.com/ros2/rmw_zenoh/blob/rolling/zenoh_cpp_vendor/CMakeLists.txt) 更多细节。

1.  清除存储器

``` bash
mkdir ~/ws_rmw_zenoh/src -p && cd ~/ws_rmw_zenoh/src
git clone https://github.com/ros2/rmw_zenoh.git -b rolling
```

1.  安装依赖性 :

``` bash
cd ~/ws_rmw_zenoh
rosdep install --from-paths src --ignore-src --rosdistro rolling -y
```

3.  使用 Colcon 构建工作空间 :

``` bash
source /opt/ros/rolling/setup.bash
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release
```

<span id="switch-to-rmw-zenoh-cpp"></span>

## 切换到 rmw_zenoh_cpp

通过指定环境变量从其他rmw切换到rmw_zenoh_cpp.

``` bash
export RMW_IMPLEMENTATION=rmw_zenoh_cpp
```

<span id="run-the-talker-and-listener"></span>

## 运行说话者和收听者

现在快跑 `talker` 财务报告和财务报告 `listener` 以试禅诺.

启动 Zenoh 路由器

``` bash
# terminal 1
source /opt/ros/rolling/setup.bash
ros2 run rmw_zenoh_cpp rmw_zenohd
```

> **说明**
>
> 没有Zenoh路由器,节点将无法互相发现,因为多播发现默认在节点的会话配置中被禁用。 相反,节点会通过Zenoh路由器的八卦功能接收关于其他同行的发现信息。

``` bash
# terminal 2
export RMW_IMPLEMENTATION=rmw_zenoh_cpp
source /opt/ros/rolling/setup.bash
ros2 run demo_nodes_cpp talker
```

``` bash
# terminal 3
export RMW_IMPLEMENTATION=rmw_zenoh_cpp
source /opt/ros/rolling/setup.bash
ros2 run demo_nodes_cpp listener
```

> **说明**
>
> 在运行这些命令前请记住源代码为 ROS 2 的设置脚本 。
