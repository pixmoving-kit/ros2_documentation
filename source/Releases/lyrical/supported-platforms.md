<span id="lyrical-luth-supported-platforms"></span>

# Lyrical Luth 支持的平台

ROS Lyrical 按照[平台支持级别](../../The-ROS2-Project/Platform-Support-Tiers.md)支持以下平台：

| 架构 | Ubuntu Resolute（26.04） | Ubuntu Noble*（24.04） | Windows 11（VS2022） | RHEL 10 | macOS | Debian Trixie*（13） | OpenEmbedded / Yocto Project |
| --- | --- | --- | --- | --- | --- | --- | --- |
| amd64 | 一级 [d][a] | 三级 | 一级 [a] | 二级 [d][a] | 三级 | 三级 | 三级 |
| arm64 | 一级 [d][a] | 三级 | | | | 三级 | 三级 |
| arm32 | 三级 | 三级 | | | | 三级 | 三级 |

- `*`：根据[平台支持终止政策](../../The-ROS2-Project/Platform-EOL-Policy.md)，提前结束支持。Ubuntu Noble 支持至 `2029-06-01`，Debian Trixie 支持至 `2028-08-09`。
- `[d]`：可通过发行版专用软件包（Debian、RPM 等）安装 ROS Lyrical。
- `[a]`：可下载预构建归档包，包含 [ROS Lyrical ros2.repos 文件](https://github.com/ros2/ros2/blob/lyrical/ros2.repos)列出的全部软件包。

在三级支持平台上使用 ROS Lyrical，必须从源码构建。

<span id="minimum-language-requirements"></span>

## 编程语言最低要求

- [C++20](https://discourse.openrobotics.org/t/ros-2-lyrical-c-version/52551)
- C17
- Python 3.12～3.14

<span id="dependency-requirements"></span>

## 依赖要求

Ubuntu Resolute 和 Windows 11 为必须支持的平台，其余列为建议支持的平台。

| 软件包 | Ubuntu Resolute | Windows 11 | Ubuntu Noble | RHEL 10 | macOS*** | Debian Trixie | OpenEmbedded |
| --- | --- | --- | --- | --- | --- | --- | --- |
| CMake | 4.2.3 | 3.28.3 | 3.28.3 | 3.30.5 | 4.3.2 | 3.31.6 | 4.3.2 |
| EmPY | 4.2.1 | 3.3.4 | 3.3.4 | 4.2.1 | | 3.3.4 | 3.3.2 |
| Gazebo | Jetty* | 不适用 | 不适用 | 不适用 | Jetty* | Jetty* | 不适用 |
| NumPy | 2.3.4 | 1.26.4 | 1.26.4 | 1.26.4 | 2.4.5 | 2.2.4 | 不适用 |
| Ogre | 1.12.10 | 1.12.10 | 1.12.10 | 不适用 | | 1.12.10 | 不适用 |
| OpenCV | 4.10.0 | 4.10.0 | 4.6.0 | 4.10.0 | 4.13.10 | 4.10.0 | 4.13.10 |
| OpenSSL | 3.5.5 | `>=3.4` | 3.0.13 | 3.5.1 | 3.6.2 | 3.5.6 | 3.5.6 |
| Python | 3.14.3 | 3.12.3 | 3.12.3 | 3.12.12 | 3.14.5 | 3.13.5 | 3.14.4 |
| Qt | 6.10.2 | `>=6` | 5.15.13 | 6.10.1 | 6.11.1 | 6.8.2 | 不适用 |
| PCL | 1.15.1 | 不适用 | 1.14.0 | 1.15.0* | 1.15.1 | 1.15.0 | 6.12.0 |

`*` 表示并非操作系统官方仓库中的上游版本，而是 OSRF 或社区在自定义仓库构建和分发的软件包。

`**` 表示该依赖可能多次变更版本，因为其包管理器会持续更新依赖，且不提供稳定 API。

本文仅记录 ROS 发行版首次发布时的依赖版本，之后不会随依赖升级更新，因此这些版本代表最低基准。

<span id="middleware-implementation-support"></span>

## 中间件实现支持

ROS Lyrical 默认使用 **rmw_fastrtps_cpp**。

| 中间件 | Ubuntu Resolute | Windows 11 | Ubuntu Noble | RHEL 10 | macOS | Debian Trixie | OpenEmbedded |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Connext DDS | 7.7.0 | 7.7.0 | 不适用 | 不适用 | 7.7.0 | 不适用 | 不适用 |
| Cyclone DDS | 11.0.x | 11.0.x | 11.0.x | 11.0.x | 11.0.x | 11.0.x | 11.0.x |
| Fast-DDS | 3.6.x | 3.6.x | 3.6.x | 3.6.x | 3.6.x | 3.6.x | 3.6.x |
| Zenoh | 1.8.0 | 1.8.0 | 1.8.0 | 1.8.0 | 1.8.0 | 1.8.0 | 1.8.0 |

| 中间件库 | 提供者 | 支持级别 | 架构 |
| --- | --- | --- | --- |
| `rmw_fastrtps_cpp` | eProsima Fast-DDS | 一级 | 所有架构 |
| `rmw_connextdds` | RTI Connext | 一级 | 除 arm64 外的所有架构 |
| `rmw_cyclonedds_cpp` | Eclipse Cyclone DDS | 一级 | 所有架构 |
| `rmw_zenoh_cpp` | Eclipse Zenoh | 一级 | 所有架构 |
| `rmw_fastrtps_dynamic_cpp` | eProsima Fast-DDS | 二级 | 所有架构 |

中间件实现的支持级别受平台支持级别限制。例如，一级支持的中间件实现运行在二级平台上时，只能获得二级支持。
