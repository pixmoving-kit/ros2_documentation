<span id="windows-binary"></span>

# Windows（二进制安装）

本页介绍如何通过预先构建好的二进制软件包，在 Windows 上安装 ROS 2。

!!! note "说明"

    预构建二进制包并不包含全部 ROS 2 软件包。它包含 [ROS base 变体](https://reps.openrobotics.org/rep-2001/#ros-base)中的所有软件包，以及 [ROS desktop 变体](https://reps.openrobotics.org/rep-2001/#desktop-variants)中的部分软件包。具体包含哪些软件包，由 [ros2.repos 文件](https://github.com/ros2/ros2/blob/rolling/ros2.repos)中列出的仓库决定。

<span id="system-requirements"></span>

## 系统要求

仅支持 Windows 10。

<span id="windows-install-binary-installing-prerequisites"></span>
<span id="installing-prerequisites"></span>
<span id="install-chocolatey"></span>
<span id="install-python"></span>
<span id="install-visual-c-redistributables"></span>
<span id="install-openssl"></span>
<span id="install-visual-studio"></span>
<span id="install-opencv"></span>
<span id="install-dependencies"></span>
<span id="install-qt5"></span>
<span id="rqt-dependencies"></span>

## 安装前置依赖

按照 [Windows 前置依赖安装说明](_Windows-Install-Prerequisites.md)，安装 Chocolatey、Python、Visual C++ 可再发行组件、OpenSSL、Visual Studio、OpenCV、其他依赖、Qt5 和 rqt 依赖。

<span id="downloading-ros-2"></span>

## 下载 ROS 2

- 前往[发布页面](https://github.com/ros2/ros2/releases)。
- 下载适用于 Windows 的最新软件包，例如 `ros2-rolling-*-windows-release-amd64.zip`。

!!! note "说明"

    可能有多个二进制下载选项，因此实际文件名可能不同。

!!! note "说明"

    要安装 ROS 2 的调试库，请先参阅[调试所需的额外设置](#extra-stuff-for-debug)，然后继续下载 `ros2-package-windows-debug-AMD64.zip`。

将 zip 文件解压到某个位置；本文假设使用 `C:\dev\ros2_rolling`。

!!! note "说明"

    这些二进制文件使用 Release 构建配置生成。在 Windows（MSVC）上，为保证 ABI 兼容，下游二进制文件（例如你自己的节点）也必须使用 Release 或 RelWithDebInfo 配置构建。如果希望使用其他配置构建下游软件包，需要[从源码构建 ROS 2](Alternatives/Windows-Development-Setup.md)。例如，使用 Debug 配置：

```console
$ colcon build --cmake-args -DCMAKE_BUILD_TYPE=Debug
```

<span id="install-additional-dds-implementations-optional"></span>

## 安装其他 DDS 实现（可选）

如果希望使用默认 Fast DDS 以外的 DDS 或 RTPS 实现，请参阅 [RMW 实现](RMW-Implementations.md)。

<span id="environment-setup"></span>

## 环境设置

打开命令行 shell，加载 ROS 2 设置文件以配置工作空间：

```console
$ call C:\dev\ros2_rolling\local_setup.bat
```

如果没有其他错误，上述命令恰好输出一次 `The system cannot find the path specified.` 属于正常现象。

<span id="try-some-examples"></span>

## 运行示例

在一个命令行 shell 中，按照上述说明设置 ROS 2 环境，然后运行 C++ `talker`：

```console
$ ros2 run demo_nodes_cpp talker
```

打开另一个命令行 shell，运行 Python `listener`：

```console
$ ros2 run demo_nodes_py listener
```

你应该看到 `talker` 输出 `Publishing`，表示正在发布消息；`listener` 输出 `I heard`，表示收到了这些消息。这说明 C++ 和 Python API 都在正常工作，恭喜！

<span id="next-steps-after-installing"></span>

## 安装后的后续步骤

继续学习[教程和演示](../Tutorials.md)，配置环境，创建自己的工作空间和软件包，并了解 ROS 2 核心概念。

<span id="additional-rmw-implementations-optional"></span>

## 其他 RMW 实现（可选）

ROS 2 默认使用 `Fast DDS` 中间件，但可以在运行时切换中间件（RMW）。有关使用多个 RMW 的方法，请参阅[指南](../How-To-Guides/Working-with-multiple-RMW-implementations.md)。

<span id="troubleshooting"></span>

## 问题排查

参阅 [Windows 问题排查](../How-To-Guides/Installation-Troubleshooting.md#windows-troubleshooting)。

<span id="uninstall"></span>

## 卸载

1. 如果按照以上说明使用 colcon 安装了工作空间，只需打开一个新终端，不加载工作空间的 `setup` 文件，就可以视为“卸载”。这样，当前环境的行为就如同系统未安装 Rolling 一样。
2. 如果还希望释放空间，可以删除整个工作空间目录：

```console
$ rmdir /s /q \ros2_rolling
```

<span id="extra-stuff-for-debug"></span>

## 调试所需的额外设置

要获取 ROS 2 调试库，需要下载 `ros2-rolling-*-windows-debug-AMD64.zip`。请注意，调试库需要以下额外配置才能正常工作。

可能需要修改 Python 安装，以启用调试符号和调试二进制文件：

1. 在 Windows 搜索栏中搜索并打开“应用和功能”（Apps and Features）。
2. 搜索已安装的 Python 版本。
3. 点击“修改”（Modify）。

![修改 Python 安装](images/python_installation_modify.png){ width="500" }

4. 点击“下一步”（Next），进入“高级选项”（Advanced Options）。

![进入 Python 高级安装选项](images/python_installation_next.png){ width="500" }

5. 确保选中“Download debugging symbols”和“Download debug binaries”。

![启用调试符号和调试二进制文件](images/python_installation_enable_debug.png){ width="500" }

6. 点击“安装”（Install）。
