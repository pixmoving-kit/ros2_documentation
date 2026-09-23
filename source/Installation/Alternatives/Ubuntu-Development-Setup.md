<span id="ubuntu-source"></span>
<span id="linux-latest"></span>

# Ubuntu（源码安装）

<span id="system-requirements"></span>

## 系统要求

Rolling Ridley 当前支持的 Debian 系目标平台为：

- Tier 1：Ubuntu Linux — Jammy（22.04）64 位
- Tier 3：Ubuntu Linux — Focal（20.04）64 位
- Tier 3：Debian Linux — Bullseye（11）64 位

其他支持程度不同的 Linux 平台包括：

- Arch Linux，参阅[对应说明](https://wiki.archlinux.org/index.php/ROS#ROS_2)。
- Fedora Linux，参阅[对应说明](Fedora-Development-Setup.md)。
- OpenEmbedded / webOS OSE，参阅[对应说明](https://github.com/ros/meta-ros/wiki/OpenEmbedded-Build-Instructions)。

支持等级定义见 [REP 2000](https://reps.openrobotics.org/rep-2000/)。

<span id="system-setup"></span>

## 系统设置

<span id="set-locale"></span>

### 设置区域设置

按照 [Ubuntu 区域设置说明](../_Ubuntu-Set-Locale.md)配置支持 UTF-8 的区域设置。

<span id="add-the-ros-2-apt-repository"></span>

### 添加 ROS 2 apt 软件源

按照 [apt 软件源配置说明](../_Apt-Repositories.md)添加软件源。

<span id="install-development-tools-and-ros-tools"></span>

### 安装开发工具和 ROS 工具

安装通用软件包：

```console
$ sudo apt update && sudo apt install -y \
  python3-flake8-docstrings \
  python3-pip \
  python3-pytest-cov \
  ros-dev-tools
```

然后根据 Ubuntu 版本安装相应软件包。

**Ubuntu 22.04 LTS 及更高版本：**

```console
$ sudo apt install -y \
   python3-flake8-blind-except \
   python3-flake8-builtins \
   python3-flake8-class-newline \
   python3-flake8-comprehensions \
   python3-flake8-deprecated \
   python3-flake8-import-order \
   python3-flake8-quotes \
   python3-pytest-repeat \
   python3-pytest-rerunfailures
```

**Ubuntu 20.04 LTS：**

```console
$ python3 -m pip install -U \
   flake8-blind-except \
   flake8-builtins \
   flake8-class-newline \
   flake8-comprehensions \
   flake8-deprecated \
   flake8-import-order \
   flake8-quotes \
   "pytest>=5.3" \
   pytest-repeat \
   pytest-rerunfailures \
   empy==3.3.4
```

<span id="get-ros-2-code"></span>
<span id="linux-dev-get-ros2-code"></span>

## 获取 ROS 2 代码

创建工作空间，并克隆所有仓库：

```console
$ mkdir -p ~/ros2_rolling/src
$ cd ~/ros2_rolling
$ vcs import --input https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos src
```

<span id="install-dependencies-using-rosdep"></span>
<span id="linux-development-setup-install-dependencies-using-rosdep"></span>

## 使用 rosdep 安装依赖

先按照[系统更新说明](../_Apt-Upgrade-Admonition.md)更新系统，然后运行：

```console
$ sudo rosdep init
$ rosdep update
$ rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-6.0.1 urdfdom_headers"
```

使用 Linux Mint 等 Ubuntu 衍生发行版时，请参阅 [rosdep 注意事项](../_rosdep_Linux_Mint.md)。

<span id="install-additional-dds-implementations-optional"></span>

## 安装其他 DDS 实现（可选）

如果希望使用默认供应商以外的 DDS 或 RTPS 实现，请参阅 [RMW 实现](../RMW-Implementations.md)。

<span id="install-colcon-mixins"></span>

### 安装 colcon mixin 配置

```console
$ colcon mixin add default https://github.com/colcon/colcon-mixin-repository/raw/master/index.yaml
$ colcon mixin update default
```

<span id="build-the-code-in-the-workspace"></span>

## 构建工作空间中的代码

如果已经通过其他方式安装了 ROS 2（deb 软件包或二进制发行包），请在尚未加载这些安装环境的新环境中运行以下命令。同时确保 `.bashrc` 中没有 `source /opt/ros/${ROS_DISTRO}/setup.bash`。可以运行 `printenv | grep -i ROS`，确认尚未加载 ROS 2 环境；输出应为空。

有关使用 ROS 工作空间的更多信息，请参阅[此教程](../../Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.md)。

```console
$ cd ~/ros2_rolling/
$ colcon build --symlink-install --mixin release
```

注意：如果某些示例无法编译，导致整个构建无法成功，可以像使用 [CATKIN_IGNORE](https://github.com/ros-infrastructure/rep/blob/master/rep-0128.rst) 一样使用 `COLCON_IGNORE`，忽略对应子目录树，也可以将相应文件夹从工作空间中移除。例如，如果不想安装体积较大的 OpenCV 库，只需在 `cam2image` 演示目录中运行 `touch COLCON_IGNORE`，即可在构建过程中跳过它。

<span id="environment-setup"></span>

## 环境设置

<span id="source-the-setup-script"></span>

### 加载设置脚本

通过加载以下文件配置环境：

```console
$ . ~/ros2_rolling/install/local_setup.bash
```

!!! note "说明"

    如果使用的不是 Bash，请将 `.bash` 替换为对应 shell 的扩展名。可用形式包括 `setup.bash`、`setup.sh` 和 `setup.zsh`。

<span id="try-some-examples"></span>
<span id="talker-listener"></span>

## 运行示例

在一个终端中加载设置文件，然后运行 C++ `talker`：

```console
$ . ~/ros2_rolling/install/local_setup.bash
$ ros2 run demo_nodes_cpp talker
```

在另一个终端中加载设置文件，然后运行 Python `listener`：

```console
$ . ~/ros2_rolling/install/local_setup.bash
$ ros2 run demo_nodes_py listener
```

你应该看到 `talker` 输出 `Publishing`，表示正在发布消息；`listener` 输出 `I heard`，表示收到了这些消息。这说明 C++ 和 Python API 都在正常工作，恭喜！

<span id="next-steps-after-installing"></span>

## 安装后的后续步骤

继续学习[教程和演示](../../Tutorials.md)，配置环境，创建自己的工作空间和软件包，并了解 ROS 2 核心概念。

<span id="using-the-ros-1-bridge"></span>

## 使用 ROS 1 桥接

ROS 1 桥接可以连接 ROS 1 和 ROS 2 的话题，实现双向通信。有关构建和使用方法，请参阅[专门的文档](https://github.com/ros2/ros1_bridge/blob/master/README.md)。

<span id="additional-rmw-implementations-optional"></span>

## 其他 RMW 实现（可选）

ROS 2 默认使用 `Fast DDS` 中间件，但可以在运行时切换中间件（RMW）。有关使用多个 RMW 的方法，请参阅[指南](../../How-To-Guides/Working-with-multiple-RMW-implementations.md)。

<span id="alternate-compilers"></span>

## 使用其他编译器

使用 GCC 以外的编译器构建 ROS 2 很简单。将环境变量 `CC` 和 `CXX` 分别设为可用的 C 和 C++ 编译器可执行文件，再触发 CMake 重新配置（使用 `--cmake-force-configure`，或删除希望重新配置的软件包），CMake 就会重新配置并使用新的编译器。

<span id="clang"></span>

### Clang

要让 CMake 检测并使用 Clang，请运行：

```console
$ sudo apt install clang
$ export CC=clang
$ export CXX=clang++
$ colcon build --cmake-force-configure
```

<span id="stay-up-to-date"></span>

## 保持更新

参阅[维护源码工作副本](../Maintaining-a-Source-Checkout.md)，定期更新从源码安装的环境。

<span id="troubleshooting"></span>

## 问题排查

问题排查方法见 [Linux 问题排查](../../How-To-Guides/Installation-Troubleshooting.md#linux-troubleshooting)。

<span id="uninstall"></span>

## 卸载

1. 如果按照以上说明使用 colcon 安装了工作空间，只需打开一个新终端，不加载工作空间的 `setup` 文件，就可以视为“卸载”。这样，当前环境的行为就如同系统未安装 Rolling 一样。
2. 如果还希望释放空间，可以删除整个工作空间目录：

```console
$ rm -rf ~/ros2_rolling
```
