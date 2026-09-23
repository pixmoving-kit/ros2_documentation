<span id="macos-source"></span>
<span id="macos-latest"></span>

# macOS（源码安装）

<span id="system-requirements"></span>

## 系统要求

目前支持 macOS Mojave（10.14）。

<span id="install-prerequisites"></span>

## 安装前置依赖

构建 ROS 2 需要安装以下组件。

### 1. Xcode

如果尚未安装，请安装 [Xcode](https://apps.apple.com/app/xcode/id497799835)。注意：Xcode 11.3.1 之后的版本无法安装在 macOS Mojave 上，因此需要手动安装旧版本，参阅 <https://stackoverflow.com/a/61046761>。

如果尚未安装命令行工具，也请安装：

```console
$ xcode-select --install
$ sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
```

如果手动安装了 Xcode.app，需要接受其许可协议。可以打开 Xcode.app，或运行以下命令：

```console
$ sudo xcodebuild -license
```

### 2. Homebrew

Homebrew 用于安装后续组件，你可能已经安装过它。按照 <http://brew.sh/> 的说明安装。

可选：运行以下命令，检查 `brew` 是否发现系统配置问题，并修复它报告的问题。

```console
$ brew doctor
```

### 3. 使用 brew 安装其他组件

```console
$ brew install asio assimp bison bullet cmake console_bridge cppcheck \
   cunit eigen freetype graphviz opencv openssl orocos-kdl pcre poco \
   pyqt@5 python qt@5 sip spdlog osrf/simulation/tinyxml1 tinyxml2
```

### 4. 设置环境变量

```console
~ Add the openssl dir for DDS-Security
~ if you are using BASH, then replace '.zshrc' with '.bashrc'
$ echo "export OPENSSL_ROOT_DIR=$(brew --prefix openssl)" >> ~/.zshrc

~ Add the Qt directory to the PATH and CMAKE_PREFIX_PATH
$ export CMAKE_PREFIX_PATH=$CMAKE_PREFIX_PATH:$(brew --prefix qt@5)
$ export PATH=$PATH:$(brew --prefix qt@5)/bin
```

### 5. 使用 python3 -m pip 安装其他组件

请使用 `python3 -m pip`；仅使用 `pip` 可能会为 Python 3 或 Python 2 安装软件包。

```console
$ python3 -m pip install --upgrade pip

$ python3 -m pip install -U \
  --config-settings="--global-option=build_ext" \
  --config-settings="--global-option=-I$(brew --prefix graphviz)/include/" \
  --config-settings="--global-option=-L$(brew --prefix graphviz)/lib/" \
  argcomplete catkin_pkg colcon-common-extensions coverage \
  cryptography empy flake8 flake8-blind-except==0.1.1 flake8-builtins \
  flake8-class-newline flake8-comprehensions flake8-deprecated \
  flake8-docstrings flake8-import-order flake8-quotes \
  importlib-metadata lark==1.1.1 lxml matplotlib mock mypy==0.931 netifaces \
  nose pep8 psutil pydocstyle pydot pygraphviz pyparsing==2.4.7 \
  pytest-mock rosdep rosdistro setuptools==59.6.0 vcstool
```

请确保 `$PATH` 环境变量中包含可执行文件的安装位置（`$(brew --prefix)/bin`）。

### 6. 安装 ROS 1（可选）

如果希望构建 ROS 1 与 ROS 2 之间的桥接，还必须安装 ROS 1。

先按照[常规安装说明](http://wiki.ros.org/kinetic/Installation/OSX/Homebrew/Source)操作。在使用 `rosinstall_generator` 获取源码的步骤，可以改用以下命令，只获取构建可用桥接所需的最少组件：

```console
$ rosinstall_generator catkin common_msgs roscpp rosmsg --rosdistro kinetic --deps --wet-only --tar > kinetic-ros2-bridge-deps.rosinstall
$ wstool init -j8 src kinetic-ros2-bridge-deps.rosinstall
```

其余步骤仍按照常规说明操作，然后加载生成的 `install_isolated/setup.bash`，再继续构建 ROS 2。

<span id="disable-system-integrity-protection-sip"></span>

## 禁用系统完整性保护（SIP）

macOS/OS X 10.11 及更高版本默认启用系统完整性保护。为了避免 SIP 阻止进程继承 `DYLD_LIBRARY_PATH` 等动态链接器环境变量，需要按照[这些说明](https://developer.apple.com/library/content/documentation/Security/Conceptual/System_Integrity_Protection_Guide/ConfiguringSystemIntegrityProtection/ConfiguringSystemIntegrityProtection.html)禁用它。

<span id="get-the-ros-2-code"></span>

## 获取 ROS 2 代码

创建工作空间，并克隆所有仓库：

```console
$ mkdir -p ~/ros2_rolling/src
$ cd ~/ros2_rolling
$ vcs import --input https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos src
```

<span id="install-additional-dds-vendors-optional"></span>

## 安装其他 DDS 供应商实现（可选）

如果希望使用默认供应商以外的 DDS 或 RTPS 实现，请参阅 [RMW 实现](../RMW-Implementations.md)。

<span id="build-the-ros-2-code"></span>

## 构建 ROS 2 代码

运行 `colcon` 构建所有组件。有关 `colcon` 的更多用法，请参阅[此教程](../../Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.md)。

```console
$ cd ~/ros2_rolling/
$ colcon build --symlink-install --packages-skip-by-dep python_qt_binding
```

注意：由于 SIP、Qt@5 和 PyQt5 之间尚未解决的问题，需要禁用 `python_qt_binding` 才能成功构建。问题解决后将移除此限制，参阅 <https://github.com/ros-visualization/python_qt_binding/issues/103>。

<span id="environment-setup"></span>

## 环境设置

加载 ROS 2 设置文件：

```console
$ . ~/ros2_rolling/install/setup.zsh
```

这会自动配置构建时已启用支持的各个 DDS 供应商实现所需的环境。

<span id="try-some-examples"></span>

## 运行示例

在一个终端中，按照上述说明设置 ROS 2 环境，然后运行 C++ `talker`：

```console
$ ros2 run demo_nodes_cpp talker
```

在另一个终端中加载设置文件，然后运行 Python `listener`：

```console
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

<span id="stay-up-to-date"></span>

## 保持更新

参阅[维护源码工作副本](../Maintaining-a-Source-Checkout.md)，定期更新从源码安装的环境。

<span id="troubleshooting"></span>

## 问题排查

参阅 [macOS 问题排查](../../How-To-Guides/Installation-Troubleshooting.md#macos-troubleshooting)。

<span id="uninstall"></span>

## 卸载

1. 如果按照以上说明使用 colcon 安装了工作空间，只需打开一个新终端，不加载工作空间的 `setup` 文件，就可以视为“卸载”。这样，当前环境的行为就如同系统未安装 Rolling 一样。
2. 如果还希望释放空间，可以删除整个工作空间目录：

```console
$ rm -rf ~/ros2_rolling
```
