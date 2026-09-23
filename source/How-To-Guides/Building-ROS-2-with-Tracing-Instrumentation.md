<span id="building-ros-2-with-tracing-instrumentation"></span>
# 构建带跟踪插桩的 ROS 2

本指南介绍如何构建包含 `ros2_tracing` 跟踪插桩的 ROS 2。更多信息请参阅[项目仓库](https://github.com/ros2/ros2_tracing)。

ROS 2 源码中已包含插桩代码。但无论使用二进制包还是从源码构建，默认情况下这些插桩都不会真正触发跟踪点。要启用跟踪点，需先安装 LTTng 跟踪器，再从源码构建或重新构建 ROS 2 的部分组件。

!!! note "说明"
    本指南仅适用于 Linux 系统，并假定使用 Ubuntu。

<span id="prerequisites"></span>
## 前提条件

配置好从源码构建 ROS 2 所需的系统环境，详见[源码安装页面](../Installation/Alternatives/Ubuntu-Development-Setup.md)。

<span id="installing-the-tracer"></span>
## 安装跟踪器

安装 [LTTng 跟踪器](https://lttng.org/docs) 及相关工具和依赖项：

```bash
sudo apt-get update
sudo apt-get install -y lttng-tools liblttng-ust-dev python3-lttng python3-babeltrace babeltrace
```

这里只安装 LTTng 用户空间跟踪器，不安装内核跟踪器，因为跟踪 ROS 2 应用程序不需要后者。

<span id="building"></span>
## 构建

这一步取决于你使用的是 ROS 2 源码安装还是二进制安装。

<span id="with-source-installation"></span>
### 源码安装

如果在安装 LTTng 之前已经[从源码构建过 ROS 2](../Installation/Alternatives/Ubuntu-Development-Setup.md)，需要至少重新构建到 `tracetools` 软件包：

```bash
cd ~/ws
colcon build --packages-up-to tracetools --cmake-force-configure
```

<span id="with-binary-installation"></span>
### 二进制安装

如果使用 ROS 2 二进制文件（[deb 软件包](../Installation/Ubuntu-Install-Debs.md)或[完整二进制归档](../Installation/Alternatives/Ubuntu-Install-Binary.md)），需要将 `ros2_tracing` 仓库克隆到工作空间中，并至少构建到 `tracetools` 软件包：

```bash
cd ~/ws/src
git clone https://github.com/ros2/ros2_tracing.git
cd ../
colcon build --packages-up-to tracetools
```

<span id="validating"></span>
## 验证

加载环境并确认跟踪功能已启用：

```bash
cd ~/ws
source install/setup.bash
ros2 run tracetools status
```

应输出：

```bash
Tracing enabled
```

如果输出其他内容，说明某个步骤出了问题。

<span id="disabling-tracing"></span>
## 禁用跟踪

如果构建 `tracetools` 时找到了已安装的 LTTng 用户空间跟踪器，跟踪功能会自动启用。若希望构建时从 ROS 2 中彻底移除跟踪点和跟踪插桩代码，可将 CMake 选项 `TRACETOOLS_DISABLED` 设为 `ON`：

```bash
colcon build --cmake-args -DTRACETOOLS_DISABLED=ON --no-warn-unused-cli
```
