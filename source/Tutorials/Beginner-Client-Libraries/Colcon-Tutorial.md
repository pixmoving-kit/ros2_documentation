<span id="using-colcon-to-build-packages"></span> <span id="colcon"></span>
# 使用 colcon 构建软件包

**目标：** 使用 `colcon` 构建 ROS 2 工作空间。

**教程级别：** 初学者

**预计用时：** 20 分钟

本教程简要介绍如何使用 `colcon` 创建并构建 ROS 2 工作空间，侧重实际操作，不替代其核心文档。

<span id="background"></span>
## 背景

`colcon` 是在 ROS 构建工具 `catkin_make`、`catkin_make_isolated`、`catkin_tools` 和 `ament_tools` 的基础上发展而来的。设计细节见[构建工具设计文档](https://design.ros2.org/articles/build_tool.html)。

源代码位于 [colcon GitHub 组织](https://github.com/colcon)。

<span id="prerequisites"></span>
## 前提条件

<span id="install-colcon"></span>
### 安装 colcon

**Ubuntu**

```console
$ sudo apt install python3-colcon-common-extensions
```

**RHEL**

```console
$ sudo dnf install python3-colcon-common-extensions
```

**macOS**

```console
$ python3 -m pip install colcon-common-extensions
```

**Windows**

```console
$ pip install -U colcon-common-extensions
```

<span id="install-ros-2"></span>
### 安装 ROS 2

构建示例前，需要按照[安装说明](../../Installation.md)安装 ROS 2。

!!! attention "注意"
    如果使用 deb 包安装，本教程需要[桌面版安装](../../Installation/Ubuntu-Install-Debs.md#linux-install-debs-install-ros-2-packages)。

<span id="basics"></span>
## 基础知识

ROS 工作空间是具有特定结构的目录，通常包含一个 `src` 子目录，用于存放 ROS 软件包的源代码。刚创建时，工作空间的其他部分通常为空。

colcon 在源码目录之外进行构建，默认会在与 `src` 同级的位置创建以下目录：

- `build`：存放中间文件，每个软件包都有单独的子目录，例如 CMake 就在相应子目录中运行。
- `install`：存放安装结果，默认每个软件包安装到单独的子目录。
- `log`：保存每次调用 colcon 时产生的各种日志。

!!! note "注意"
    与 catkin 不同，这里没有 `devel` 目录。

<span id="create-a-workspace"></span>
### 创建工作空间

首先创建 `ros2_ws` 目录作为工作空间：

**Linux**

```console
$ mkdir -p ~/ros2_ws/src
$ cd ~/ros2_ws
```

**macOS**

```console
$ mkdir -p ~/ros2_ws/src
$ cd ~/ros2_ws
```

**Windows**

```console
$ md \dev\ros2_ws\src
$ cd \dev\ros2_ws
```

此时工作空间只包含一个空的 `src` 目录：

```bash
.
└── src

1 directory, 0 files
```

<span id="add-some-sources"></span>
### 添加源码

将 [examples 仓库](https://github.com/ros2/examples)克隆到工作空间的 `src` 目录：

```console
$ git clone https://github.com/ros2/examples src/examples -b rolling
```

现在工作空间中应该已经包含 ROS 2 示例的源代码：

```bash
.
└── src
    └── examples
        ├── CONTRIBUTING.md
        ├── LICENSE
        ├── rclcpp
        ├── rclpy
        └── README.md

4 directories, 3 files
```

<span id="source-an-underlay"></span>
### 加载底层工作空间

必须先加载现有 ROS 2 安装的环境，为示例软件包提供构建所需的依赖。为此，需要加载二进制安装或源码安装提供的环境设置脚本；源码安装本身也是另一个 colcon 工作空间，详见[安装文档](../../Installation.md)。这个环境称为**底层工作空间（underlay）**。

我们的 `ros2_ws` 则是叠加在现有 ROS 2 安装之上的**上层工作空间（overlay）**。如果只打算对少量软件包反复开发，通常建议使用上层工作空间，无需把所有软件包放进同一个工作空间。

<span id="build-the-workspace"></span>
### 构建工作空间

!!! attention "注意"
    在 Windows 上构建软件包时，需要使用 Visual Studio 环境。详见[构建 ROS 2 代码](../../Installation/Alternatives/Windows-Development-Setup.md#windows-dev-build-ros2)。

在工作空间根目录运行 `colcon build`。`ament_cmake` 等构建类型没有 `devel` 空间的概念，必须安装软件包，因此 colcon 提供了 `--symlink-install` 选项。使用该选项后，修改源码空间中的文件（例如 Python 文件或其他无需编译的资源），就能直接改变对应的已安装文件，加快迭代。

**Linux**

```console
$ colcon build --symlink-install
```

**macOS**

```console
$ colcon build --symlink-install
```

**Windows**

```console
$ colcon build --merge-install
```

Windows 存在路径长度限制，因此 `merge-install` 将各软件包合并安装到 `install` 目录。Windows 创建符号链接需要特殊权限，所以默认不使用 `--symlink-install`。若要使用它，需要以管理员身份运行命令，或在系统设置中启用开发者模式。

!!! tip "提示"
    在 CPU、内存和 I/O 资源有限的系统上，例如树莓派，运行 `colcon build` 可能导致屏幕和鼠标无响应。可以使用 `--executor sequential` 参数，逐个构建软件包，避免并行构建。更多参数见 [colcon 文档](https://colcon.readthedocs.io/en/released/reference/executor-arguments.html)。

构建完成后，应能看到 `build`、`install` 和 `log` 目录：

```bash
.
├── build
├── install
├── log
└── src

4 directories, 0 files
```

<span id="run-tests"></span> <span id="colcon-run-the-tests"></span>
### 运行测试

对刚构建的软件包运行测试：

**Linux**

```console
$ colcon test
```

**macOS**

```console
$ colcon test
```

**Windows**

由于需要构建工作空间，请使用 `x64 Native Tools Command Prompt for VS 2019` 执行：

```console
$ colcon test --merge-install
```

之前构建时使用了 `--merge-install`，因此测试时也需要指定它。

<span id="source-the-environment"></span> <span id="colcon-tutorial-source-the-environment"></span>
### 加载环境

colcon 成功完成构建后，输出位于 `install` 目录。使用其中安装的可执行程序或库之前，需要将它们加入可执行文件和库的搜索路径。colcon 会在 `install` 中生成用于配置环境的 bash/bat 文件，添加所需路径，并提供软件包导出的 bash 或 shell 命令。

**Linux**

```console
$ source install/setup.bash
```

**macOS**

```console
$ . install/setup.bash
```

**Windows 命令提示符**

```console
$ call install\setup.bat
```

**Windows PowerShell**

```console
$ install\setup.ps1
```

<span id="try-a-demo"></span>
### 运行演示

加载环境后，就可以运行 colcon 构建的可执行程序。先运行示例订阅者节点：

```console
$ ros2 run examples_rclcpp_minimal_subscriber subscriber_member_function
```

在另一个终端运行发布者节点，别忘了先加载环境设置脚本：

```console
$ ros2 run examples_rclcpp_minimal_publisher publisher_member_function
```

你应该能看到发布者和订阅者输出包含递增数字的消息。

<span id="create-your-own-package"></span>
## 创建自己的软件包

colcon 使用 [REP 149](https://reps.openrobotics.org/rep-0149/) 定义的 `package.xml` 规范，也支持[格式 2](https://reps.openrobotics.org/rep-0140/)。

colcon 支持多种构建类型，推荐使用 `ament_cmake` 和 `ament_python`，也支持纯 `cmake` 软件包。

[ament_index_python 软件包](https://github.com/ament/ament_index/tree/rolling/ament_index_python)是 `ament_python` 构建类型的示例，以 `setup.py` 作为构建的主要入口。

[demo_nodes_cpp](https://github.com/ros2/demos/tree/rolling/demo_nodes_cpp) 等软件包使用 `ament_cmake` 构建类型，并以 CMake 作为构建工具。

可以使用 `ros2 pkg create` 根据模板方便地创建新软件包。后续的[创建软件包](Creating-Your-First-ROS2-Package.md)教程会详细说明软件包创建过程及命令用法。

!!! note "注意"
    对于 `catkin` 用户，这相当于 `catkin_create_package`。

<span id="setup-colcon-cd"></span>
## 配置 colcon_cd

`colcon_cd` 可以将 shell 当前工作目录快速切换到某个软件包目录。例如，`colcon_cd some_ros_package` 可以快速进入 `~/ros2_ws/src/some_ros_package`。运行以下命令修改 shell 启动脚本，配置 `colcon_cd`：

**Linux**

```console
$ echo "source /usr/share/colcon_cd/function/colcon_cd.sh" >> ~/.bashrc
$ echo "export _colcon_cd_root=/opt/ros/rolling/" >> ~/.bashrc
```

**macOS**

```console
$ echo "source /usr/local/share/colcon_cd/function/colcon_cd.sh" >> ~/.bashrc
$ echo "export _colcon_cd_root=~/ros2_install" >> ~/.bashrc
```

**Windows**

尚不支持。

具体操作可能因 `colcon_cd` 的安装方式和工作空间位置而异，详见[文档](https://colcon.readthedocs.io/en/released/user/installation.html#quick-directory-changes)。在 Linux 或 macOS 上撤销配置时，请找到 shell 启动脚本，删除追加的 source 和 export 命令。

<span id="setup-colcon-tab-completion"></span>
## 配置 colcon 命令补全

`colcon` 支持 bash 和类似 shell 的命令补全。需要安装 `colcon-argcomplete` 软件包，并可能需要[额外配置](https://colcon.readthedocs.io/en/released/user/installation.html#enable-completion)。

<span id="tips"></span>
## 提示

- 如果不想构建某个软件包，在其目录中放置一个名为 `COLCON_IGNORE` 的空文件，它就不会被索引。
- 如果不希望为 CMake 软件包配置和构建测试，可以传入 `--cmake-args -DBUILD_TESTING=0`。
- 要运行某个软件包中的单个指定测试，可以使用：

```console
$ colcon test --packages-select YOUR_PKG_NAME --ctest-args -R YOUR_TEST_IN_PKG
```

<span id="setup-colcon-mixins"></span>
## 配置 colcon mixin

有些命令行选项写起来繁琐，也不容易记住。例如，将 CMake 构建类型设为调试模式，通常需要：

```console
$ colcon build --cmake-args -DCMAKE_BUILD_TYPE=Debug
```

mixin 仓库为常见命令行选项提供了“快捷方式”。运行以下命令安装默认的 colcon mixin：

```console
$ colcon mixin add default https://raw.githubusercontent.com/colcon/colcon-mixin-repository/master/index.yaml
$ colcon mixin update default
```

然后尝试使用 `debug` mixin：

```console
$ colcon build --mixin debug
```

更多信息见 [colcon mixin 仓库](https://github.com/colcon/colcon-mixin-repository)。
