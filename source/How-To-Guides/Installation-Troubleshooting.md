---
translation_status: machine_translated
source: How-To-Guides/Installation-Troubleshooting.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="installation-troubleshooting"></span>

# 安装问题排查

安装的故障排除技术由它们所应用的平台进行排序.

<span id="general"></span>

## A. 概况

一般故障排除技术适用于所有平台.

<span id="enable-multicast"></span>

### 启用多播

为了通过DDS成功通信,所使用的网络接口必须启用多播。我们在过去的经验中看到,在使用回路适配器时(在Ubuntu 或 OSX 上),这不一定是默认启用的。 [原刊](https://github.com/ros2/ros2/issues/552) 或一个 [关于 ros- 答案的对话](https://answers.ros.org/question/300370/ros2-talker-cannot-communicate-with-listener/)。可以验证您的当前设置允许使用 ROS 2 工具进行多播:

1号航站楼:

``` console
$ ros2 multicast receive
```

在2号航站楼

``` console
$ ros2 multicast send
```

如果第一个命令没有返回类似 :

``` bash
Received from xx.xxx.xxx.xx:43751: 'Hello World!'
```

然后,您需要更新防火墙配置,以便允许使用多播 [乌乌( uw)](https://help.ubuntu.com/community/UFW).

``` console
$ sudo ufw allow in proto udp to 224.0.0.0/4
$ sudo ufw allow in proto udp from 224.0.0.0/4
```

您可以检查多播旗是否启用了您的网络界面 `ifconfig` 工具并查找 `MULTICAST` 在旗帜部分:

``` bash
eno1: flags=4163<...,MULTICAST>
   ...
```

<span id="import-failing-without-library-present-on-the-system"></span>

### 导入失败, 系统没有显示库

有时 `rclpy` 无法导入, 因为找不到预期的 C 扩展库。 如果是的话, 将目录中的库与错误消息中提及的库进行比较。 假设存在类似名称的文件( 类似前缀) 。 `_rclpy.` 和同样的后缀,像 `.so` 但是不同的 Python 版本 / 架构 ) 您使用的 Python 解释器不同于用于构建 C 扩展的 Python 解释器 。 请务必使用与用于构建二进制的 Python 解释器 。

例如,这样的错配会在更新OS后出现。然后,重建工作空间可能会解决这个问题。

<span id="linux"></span> <span id="linux-troubleshooting"></span>

## Linux

<span id="internal-compiler-error"></span>

### 内部编译器错误

如果您尝试在像Raspberry PI这样的内存受限平台上编译时体验到一个 ICE, 您可能想要构建单个线程( 前缀 building 引用使用 `MAKEFLAGS=-j1`).

<span id="out-of-memory"></span>

### 内存缺失

那个... `ros1_bridge` 当前形式的内存需要4Gb的免费内存来编译。 如果您没有那么多的内存可用, 建议使用 `COLCON_IGNORE` 在文件夹中,并跳过它的汇编。

<span id="multiple-host-interference"></span>

### 多重主机干扰

如果您在同一个网络上运行多个实例, 可能会受到干扰。 要避免这种情况, 您可以设置环境变量 。 `ROS_DOMAIN_ID` 到不同的整数,默认值为零。这将为您的系统定义 DDS 域 ID 。

<span id="exception-sourcing-setup-bash"></span>

### 例外来源设置. bash

如果您在试图从源头创建后获取环境时遇到例外, 请尝试升级 `colcon` 使用相关的软件包

``` console
$ colcon version-check  # check if newer versions available
$ sudo apt install python3-colcon* --only-upgrade  # upgrade installed colcon packages to latest version
```

<span id="mixing-conda-and-apt-python-conflict"></span>

### 混合 conda 和 python 冲突

使用 ROS 2 时, 安装了套件 。 `apt` 安装软件包 `conda` 如果您正在使用官方设备 `apt` ROS 2 的二进制, 请确保您 `PATH` 环境变量在其中没有任何康达路径。您可能需要检查您的 `.bashrc` 对于此行,并评论出来。

另一方面在Windows上,官方的ROS 2安装程序使用 `conda` 软件包通过 `pixi` 软件包管理器, 并且由于不同软件包管理器没有组合, 这样效果很好

`conda` ROS 2 的软件包可以建造(例如由社区管理的软件包)。 [机械斯图克Name](https://robostack.github.io/) 但是没有为ROS 2提供正式的conda包。

<span id="macos"></span> <span id="macos-troubleshooting"></span>

## macOS

<span id="segmentation-fault-when-using-pyenv"></span>

### 使用时的分割断层 `pyenv`

`pyenv` Python 似乎默认使用 `.a` 文档,但这会引起问题 `rclpy`,因此建议在 macOS 上使用允许的框架构建 Python `pyenv`:

<https://github.com/pyenv/pyenv/wiki#how-to-build-cpython-with-framework-support-on-os-x>

<span id="library-not-loaded-image-not-found"></span>

### 未装入库; 未找到图像

如果您在运行时看到库加载问题( 运行测试或运行节点), 例如 :

``` bash
ImportError: dlopen(.../ros2_<distro>/ros2-osx/lib/python3.7/site-packages/rclpy/_rclpy.cpython-37m-darwin.so, 2): Library not loaded: @rpath/librcl_interfaces__rosidl_typesupport_c.dylib
  Referenced from: .../ros2_<distro>/ros2-osx/lib/python3.7/site-packages/rclpy/_rclpy.cpython-37m-darwin.so
  Reason: image not found
```

那你可能已经启用了系统完整性保护。 [这些指示](https://developer.apple.com/library/content/documentation/Security/Conceptual/System_Integrity_Protection_Guide/ConfiguringSystemIntegrityProtection/ConfiguringSystemIntegrityProtection.html) 以禁用系统完整性保护(SIP)。

<span id="qt-build-error-unknown-type-name-q-enum"></span>

### Qt 构建错误 : `unknown type name 'Q_ENUM'`

如果您看到与 Qt 相关的构建错误, 例如 :

``` bash
In file included from /usr/local/opt/qt/lib/QtGui.framework/Headers/qguiapplication.h:46:
/usr/local/opt/qt/lib/QtGui.framework/Headers/qinputmethod.h:87:5: error:
      unknown type name 'Q_ENUM'
    Q_ENUM(Action)
    ^
```

您可能正在使用 qt4 而不是 qt5 : 见 <https://github.com/ros2/ros2/issues/441>

<span id="missing-symbol-when-opencv-and-therefore-libjpeg-libtiff-and-libpng-are-installed-with-homebrew"></span>

### 与 Homebrew 安装 opencv( 以及 libjpeg, libtiff, 和 libpng) 时缺少符号

如果您安装了 opencv , 您可能会得到这个 :

``` bash
dyld: Symbol not found: __cg_jpeg_resync_to_restart
  Referenced from: /System/Library/Frameworks/ImageIO.framework/Versions/A/ImageIO
  Expected in: /usr/local/lib/libJPEG.dylib
 in /System/Library/Frameworks/ImageIO.framework/Versions/A/ImageIO
/bin/sh: line 1: 25274 Trace/BPT trap: 5       /usr/local/bin/cmake
```

如果是这样,你必须这样做:

``` console
$ brew unlink libpng libtiff libjpeg
```

因此你还需要更新它才能继续工作:

``` console
$ sudo install_name_tool -change /usr/local/lib/libjpeg.8.dylib /usr/local/opt/jpeg/lib/libjpeg.8.dylib /usr/local/lib/libopencv_highgui.2.4.dylib
$ sudo install_name_tool -change /usr/local/lib/libpng16.16.dylib /usr/local/opt/libpng/lib/libpng16.16.dylib /usr/local/lib/libopencv_highgui.2.4.dylib
$ sudo install_name_tool -change /usr/local/lib/libtiff.5.dylib /usr/local/opt/libtiff/lib/libtiff.5.dylib /usr/local/lib/libopencv_highgui.2.4.dylib
$ sudo install_name_tool -change /usr/local/lib/libjpeg.8.dylib /usr/local/opt/jpeg/lib/libjpeg.8.dylib /usr/local/Cellar/libtiff/4.0.4/lib/libtiff.5.dylib
```

第一个命令是为了避免在/usr/local/lib中获取针对系统libjpeg(etc.)构建的东西。其他命令是更新Homebrew构建的东西,以便找到libjpeg(etc.)的版本,而无需在/usr/local/lib中。

<span id="xcode-select-error-tool-xcodebuild-requires-xcode-but-active-developer-directory-is-a-command-line-instance"></span>

### Xcode 选择错误: 工具 `xcodebuild` 需要 Xcode, 但是活动开发者目录是一个命令行实例

如果您最近安装了 Xcode, 您可能会遇到这个错误 :

``` bash
Xcode: xcode-select: error: tool 'xcodebuild' requires Xcode,
but active developer directory '/Library/Developer/CommandLineTools' is a command line tools instance
```

为解决这个错误,您需要:

1.  双倍检查您是否安装了命令行工具 :

``` console
$ xcode-select --install
```

2.  通过在终端中输入来接受 Xcode 的条款和条件 :

``` console
$ sudo xcodebuild -license accept
```

3.  确保 Xcode app 在 `/Applications` 目录( NOT) `/Users/{user}/Applications`)

4.  点 `xcode-select` 到 Xcode 应用程序开发者目录,使用以下命令:

``` console
$ sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```

<span id="rosdep-install-error-homebrew-failed-to-detect-successful-installation-of-qt5"></span>

### rosdep 安装错误 `homebrew: Failed to detect successful installation of [qt5]`

在跟踪时 [创建工作空间](../Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md) 教程中,您可能遇到以下错误: `rosdep` 无法安装 Qt5 。

``` console
$ rosdep install -i --from-path src --rosdistro rolling -y
executing command [brew install qt5]
Warning: qt 5.15.0 is already installed and up-to-date
To reinstall 5.15.0, run `brew reinstall qt`
ERROR: the following rosdeps failed to install
  homebrew: Failed to detect successful installation of [qt5]
```

这个错误似乎是源于 [将问题联系起来](https://github.com/ros-infrastructure/rosdep/issues/490#issuecomment-334959426) 并可以通过运行以下命令来解决.

``` console
$ cd /usr/local/Cellar
$ sudo ln -s qt qt5
```

运行 `rosdep` 命令现在应该正常执行 :

``` console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

命令应该返回 :

``` text
#All required rosdeps installed successfully
```

<span id="windows"></span> <span id="windows-troubleshooting"></span>

## Windows

<span id="import-failing-even-with-library-present-on-the-system"></span>

### 导入失败, 即使系统上有库

有时 `rclpy` 无法导入, 因为您系统中缺少一些 DLL 。 如果是的话, 请确保安装“ 包含先决条件” 段落中列出的所有依赖性 。 [安装指令](../Installation/Windows-Install-Binary.md#windows-install-binary-installing-prerequisites)).

如果您正在从二进制安装, 您可能需要更新您的依赖性 : 它们必须与用于构建二进制的版本相同 。

如果你还有问题,你可以使用 [依赖关系](https://github.com/lucasg/Dependencies) 工具来确定您系统中缺少哪些依赖性。使用工具加载相应的 `.pyd` 文件,它应该报告不可用 `DLL` 模块。在您执行工具之前,请确定当前工作空间是源代码的,否则会有未解决的ROS DLL文件。使用此信息安装额外的依赖性或根据需要调整路径。

<span id="cmake-error-setting-modification-time"></span>

### CMake 错误设置修改时间

如果您遇到 CMake 错误 `file INSTALL cannot set modification time on ...` 当安装文件时, 可能有一个抗病毒软件或Windows Defender正在干扰构建。 例如, 对于Windows Defender, 您可以列出排除的工作空间位置, 以防止它扫描这些文件 。

<span id="character-path-limit"></span>

### 260 字符路径限制

``` bash
The input line is too long.
The syntax of the command is incorrect.
```

根据您的目录等级,您在从源代码或自己的库中构建 ROS 2 时可能会看到路径长度限制错误 。

允许更深的路径长度 :

运行 `regedit.exe`导航到 `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem`设置 `LongPathsEnabled` 至 0x00000001 (1).

击打窗口密钥和类型 `Edit Group Policy`。导航到本地计算机政策 \> 计算机配置 \> 行政模板 \> 系统 \> 文件系统。右键单击 `Enable Win32 long paths`,单击“编辑”。在对话框中,选择“启用”和“单击”“确定”。

关闭并打开您的终端以重新设置环境,并再次尝试建设.

<span id="cmake-packages-unable-to-find-asio-tinyxml2-tinyxml-or-eigen"></span>

### CMake 软件包无法找到 asio、 tinixml2、 tinixml 或 eigen

我们曾经看到过, `asio`, `tinyxml2`等不添加重要的注册条目, 而CMake在建 ROS 2. 时将无法找到它们。 我们尚未能够识别根源, `-n` 如果首次未安装失败,然后重新安装它们将解决这个问题。

<span id="patch-exe-opens-a-new-command-window-and-asks-for-administrator"></span>

### 补丁.exe 打开新命令窗口并询问管理员

这也会导致需要使用补丁的软件包的构建失败,甚至允许它使用管理员权限.

- `choco uninstall patch; colcon build --cmake-clean-cache` - 这是虫子在里面 [GNU Windows 软件包的补丁](https://chocolatey.org/packages/patch)。如果不安装此软件包,则构建过程将使用用 git 分布的 Patch 版本。

<span id="failed-to-load-fast-rtps-shared-library"></span>

### 装入快速 RTPS 共享库失败

快速RTPS 需要 `msvcr20.dll`,属于 `Visual C++ Redistributable Packages for Visual Studio 2013`尽管它通常是默认安装在Windows 10中,但我们知道一些Windows 10类似版本并没有默认安装(例如:Windows Server 2019). 如果您没有安装它,你可以从中下载它. [这儿](https://www.microsoft.com/en-us/download/details.aspx?id=40784).

<span id="failed-to-create-process"></span>

### 创建进程失败

如果运行 ROS 二进制给出错误 :

``` default
| failed to create process.
```

可能找不到 Python 解译器。 对于每个可执行文件, 都会使用随附脚本的shebang( 第一行), 所以请确定 Python 在预期路径下可用( 默认 : `C:\Python38\`).

<span id="binary-installation-specific"></span>

### 二进制安装特异性

- 如果您的示例因 DLL 缺失而未启动, 请确认来自 OpenCV 等外部依赖的所有库都位于您的内部 。 `PATH` 变量。

- 如果你忘记打电话 `local_setup.bat` 从您的终端中获取的文件, 演示程序极有可能立即崩溃 。

<span id="running-rviz-with-wsl2"></span>

### 使用 WSL2 运行 RViz

如果你在用的话 [WSL2 维基月球](https://learn.microsoft.com/en-us/windows/wsl/install) 在Windows上运行ROS 2,您可能会遇到一个运行 RViz 的问题,它看起来像:

``` console
$ rviz2
[INFO] [1695823660.091830699] [rviz2]: Stereo is NOT SUPPORTED
[INFO] [1695823660.091943524] [rviz2]: OpenGl version: 4.1 (GLSL 4.1)
D3D12: Removing Device.
Segmentation fault
```

一种可能的解决办法是迫使RViz使用软件渲染:

``` console
$ export LIBGL_ALWAYS_SOFTWARE=true
$ rviz2
[INFO] [1695823660.091830699] [rviz2]: Stereo is NOT SUPPORTED
```
