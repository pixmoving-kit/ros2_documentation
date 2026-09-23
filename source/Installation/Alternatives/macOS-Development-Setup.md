---
translation_status: machine_translated
source: Installation/Alternatives/macOS-Development-Setup.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="macos-source"></span> <span id="macos-latest"></span>

# macOS（源码安装）

<span id="system-requirements"></span>

## 系统要求

我们目前支持macOS Mojave(10.14)。

<span id="install-prerequisites"></span>

## 安装先决条件

您需要安装以下设备来构建 ROS 2:

1.  **Xcode 代码**

    - 如果您还没有安装, 请安装 [Xcode 代码](https://apps.apple.com/app/xcode/id497799835).

    - 注:Xcode的版本晚于11.3.1,无法再安装在macOS Mojave上,所以需要手动安装旧版本,参见: <https://stackoverflow.com/a/61046761>

    - 如果您还没有安装, 请安装命令行工具 :

      ``` console
      $ xcode-select --install
      $ sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
      ```

    > **说明**
    >
    > 如果您手动安装了 Xcode. app, 您需要接受 Xcode. app 许可证。 您可以打开 Xcode. app 或 运行 :
    >
    > ``` console
    > $ sudo xcodebuild -license
    > ```

2.  **酿酒** *(需要安装更多东西; 您可能已经拥有了)*:

    - 执行安装指令 : <http://brew.sh/>

    - *可选*: 请检查 `brew` 运行时对您的系统配置感到满意 :

      ``` console
      $ brew doctor
      ```

      解决它发现的任何问题。

3.  使用 `brew` 以安装更多内容 :

    ``` console
    $ brew install asio assimp bison bullet cmake console_bridge cppcheck \
       cunit eigen freetype graphviz opencv openssl orocos-kdl pcre poco \
       pyqt@5 python qt@5 sip spdlog osrf/simulation/tinyxml1 tinyxml2
    ```

4.  设置一些环境变量 :

    ``` console
    ~ Add the openssl dir for DDS-Security
    ~ if you are using BASH, then replace '.zshrc' with '.bashrc'
    $ echo "export OPENSSL_ROOT_DIR=$(brew --prefix openssl)" >> ~/.zshrc

    ~ Add the Qt directory to the PATH and CMAKE_PREFIX_PATH
    $ export CMAKE_PREFIX_PATH=$CMAKE_PREFIX_PATH:$(brew --prefix qt@5)
    $ export PATH=$PATH:$(brew --prefix qt@5)/bin
    ```

5.  使用 `python3 -m pip` (刚刚) `pip` 可能安装 Python3 或 Python2 以安装更多内容 :

    ``` console
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

    请确保 `$PATH` 环境变量包含二进制的安装位置( E)`$(brew --prefix)/bin`)

6.  *可选*:如果想要建造 ROS 1 \< \> 2 桥,你还必须安装 ROS 1:

    - 从正常安装指令开始 : <http://wiki.ros.org/kinetic/Installation/OSX/Homebrew/Source>

    - 当你到达你呼唤的台阶 `rosinstall_generator` 获取源代码,这里有一个替代的引用, 带入只需要最小的 产生有用的桥:

      ``` console
      $ rosinstall_generator catkin common_msgs roscpp rosmsg --rosdistro kinetic --deps --wet-only --tar > kinetic-ros2-bridge-deps.rosinstall
      $ wstool init -j8 src kinetic-ros2-bridge-deps.rosinstall
      ```

      否则,只是遵循正常指示,然后源 产生的结果 `install_isolated/setup.bash` 之前在这里开始建造ROS 2。

<span id="disable-system-integrity-protection-sip"></span>

## 禁用系统完整性保护( SIP)

macOS/OS X版本 QQ10.11 默认启用了系统完整性保护 。 这样 SIP 就不会阻止进程继承动态链接器环境变量, 例如 `DYLD_LIBRARY_PATH`,您需要禁用它 [遵照这些指示](https://developer.apple.com/library/content/documentation/Security/Conceptual/System_Integrity_Protection_Guide/ConfiguringSystemIntegrityProtection/ConfiguringSystemIntegrityProtection.html).

<span id="get-the-ros-2-code"></span>

## 获取 ROS 2 代码

创建工作空间并复制全部重置 :

``` console
$ mkdir -p ~/ros2_rolling/src
$ cd ~/ros2_rolling
$ vcs import --input https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos src
```

<span id="install-additional-dds-vendors-optional"></span>

## 安装更多DDS供应商(可选)

如果您想要在默认之外使用另一个 DDS 或 RTPS 供应商, 您可以找到指令 [这儿](../RMW-Implementations.md).

<span id="build-the-ros-2-code"></span>

## 构建 ROS 2 代码

运行 `colcon` 工具来构建一切(更多关于使用 `colcon` 输入 [此教程](../../Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.md)):

``` console
$ cd ~/ros2_rolling/
$ colcon build --symlink-install --packages-skip-by-dep python_qt_binding
```

注:由于SIP, Qt@5和PyQt5的未决问题, 我们需要禁用 `python_qt_binding` 当问题得到解决时,将删除此内容,请参见: <https://github.com/ros-visualization/python_qt_binding/issues/103>

<span id="environment-setup"></span>

## 环境设置

来源 ROS 2 设置文件 :

``` console
$ . ~/ros2_rolling/install/setup.zsh
```

这将自动为任何已建立支助的DDS供应商建立环境。

<span id="try-some-examples"></span>

## 尝试一些例子

在一个终端中,设置上述 ROS 2 环境,然后运行一个 C++ `talker`:

``` console
$ ros2 run demo_nodes_cpp talker
```

在另一个终端源代码中, 设置文件然后运行 Python `listener`:

``` console
$ ros2 run demo_nodes_py listener
```

你应该看看 `talker` 说,这是 `Publishing` 信件和资料 `listener` 说 `I heard` 这证明C++和Python API都正常工作。万岁!

<span id="next-steps-after-installing"></span>

## 安装后的下一步

继续 [教程和演示](../../Tutorials.md) 来配置环境,创建自己的工作空间和软件包,并学习ROS 2核心概念。

<span id="using-the-ros-1-bridge"></span>

## 使用 ROS 1 桥

ROS 1 桥可以连接 ROS 1 和 ROS 2 的主题,反之亦然。 [文档](https://github.com/ros2/ros1_bridge/blob/master/README.md) 如何建造和使用 ROS 1 桥。

<span id="additional-rmw-implementations-optional"></span>

## 更多落实RMW(可选)

ROS 2 使用的默认中间软件是 `Fast DDS`,但中间软件(RMW)可以在运行时替换。 [指南](../../How-To-Guides/Working-with-multiple-RMW-implementations.md) 如何与多个RMW合作。

<span id="stay-up-to-date"></span>

## 保持最新进展

见 [维护源码工作副本](../Maintaining-a-Source-Checkout.md) 以定期刷新您的源安装。

<span id="troubleshooting"></span>

## 麻烦的解决

可以找到解决问题的技巧 [这儿](../../How-To-Guides/Installation-Troubleshooting.md#macos-troubleshooting).

<span id="uninstall"></span>

## 卸载

1.  如果您按照上述指示将工作空间安装在colcon上, " 完全取消 " 可能只是打开一个新的终端,而不是提供工作空间。 `setup` 文件。这样,您的环境将表现为没有在您的系统中安装滚动。

2.  如果您还试图腾出空间, 您可以删除整个工作空间目录 :

    ``` console
    $ rm -rf ~/ros2_rolling
    ```
