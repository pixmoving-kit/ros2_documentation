---
translation_status: machine_translated
source: Installation/Windows-Install-Binary.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="windows-binary"></span>

# Windows（二进制安装）

本页面解释如何从一个预建的二进制包在Windows上安装ROS 2.

> **说明**
>
> 预建的二进制不包含所有 ROS 2 套件。所有套件都在 [ROS 基变型](https://reps.openrobotics.org/rep-2001/#ros-base) 包含,并且只包含在 [ROS 桌面变体](https://reps.openrobotics.org/rep-2001/#desktop-variants) 包的确切清单由以下所列寄存器描述: [此 ros2. repos 文件](https://github.com/ros2/ros2/blob/rolling/ros2.repos).

<span id="system-requirements"></span>

## 系统要求

仅支持Windows 10.

<span id="installing-prerequisites"></span> <span id="windows-install-binary-installing-prerequisites"></span>

## 安装前置依赖

<span id="install-chocolatey"></span>

### 安装巧克力

巧克力是Windows的软件包管理器,通过遵循其安装指令来安装:

<https://chocolatey.org/install>

您将使用巧克力来安装其他开发工具。

<span id="install-python"></span>

### 安装 Python

打开命令提示并按以下键通过巧克力安装 Python :

``` bash
choco install -y python --version 3.8.3
```

> **说明**
>
> 巧克力会安装 Python 在 `C:\Python38`,而其余的安装则预计会在那里。如果你在别的地方安装了Python,就必须复制或链接到该位置。

<span id="install-visual-c-redistributables"></span>

### 安装视觉 C++ 可重排

打开命令提示并输入以下内容, 以便通过巧克力安装 :

``` bash
choco install -y vcredist2013 vcredist140
```

<span id="install-openssl"></span>

### 安装 OpenSSL

打开命令提示并按以下键通过巧克力安装 OpenSSL :

``` bash
choco install -y openssl --version 1.1.1.2100
```

此命令设置一个会话持续的环境变量:

``` bash
setx /m OPENSSL_CONF "C:\Program Files\OpenSSL-Win64\bin\openssl.cfg"
```

您需要将 OpenSSL- Win64 bin 文件夹附加到您的 PATH 上。 您可以点击 Windows 图标, 输入“ 环境变量” , 然后点击“ 编辑系统环境变量 ” 。 在相应的对话框中, 请点击“ 环境变量 ” , 然后点击“ Path” , 最后点击“ 编辑” , 然后在下面添加路径 。

- `C:\Program Files\OpenSSL-Win64\bin\`

<span id="install-visual-studio"></span>

### 安装视觉工作室

安装视觉工作室 2019.

如果您已经拥有Visual Studio 2019(专业,企业)的付费版本,请跳过这一步.

微软提供免费版本的Visual Studio 2019,命名为Community,可用于构建使用ROS 2. [您可以直接通过此链接下载安装器 。](https://aka.ms/vs/16/release/vs_community.exe)

确保安装Visual C++特性.

确定安装的简单方法是选择 `Desktop development with C++` 安装期间的工作流程 。

> ![](images/windows-vs-studio-install.png)

确保没有 C++ CMake 工具通过在要安装的组件列表中不选择它们来安装 。

<span id="install-opencv"></span>

### 安装 OpenCV

一些例子要求安装OpenCV.

您可以下载 OpenCV 的预编译版本 `3.4.6` 从 [这儿](https://github.com/ros2/ros2/releases/download/opencv-archives/opencv-3.4.6-vc16.VS2019.zip).

假设你把它拆开 `C:\opencv`,在命令提示(需要管理员权限)上键入以下内容:

``` bash
setx /m OpenCV_DIR C:\opencv
```

既然您正在使用一个预编译的ROS版本, 我们必须告诉它在哪里找到 OpenCV 库。 您必须扩展 `PATH` 变量为 `C:\opencv\x64\vc16\bin`.

<span id="install-dependencies"></span>

### 安装依赖关系

巧克力软件包数据库中缺少一些依赖性。为了方便手工安装过程,我们提供了必要的巧克力软件包。

由于有些巧克力包依赖它,我们从安装CMake开始

``` bash
choco install -y cmake
```

您需要附加 CMake bin 文件夹 `C:\Program Files\CMake\bin` 到你的路径。

请下载这些软件包 [这个](https://github.com/ros2/choco-packages/releases/latest) GitHub 仓库 。

- `asio.1.12.1.nupkg`

- `bullet.3.17.nupkg`

- `cunit.2.1.3.nupkg`

- `eigen.3.3.4.nupkg`

- `tinyxml-usestl.2.6.2.nupkg`

- `tinyxml2.6.0.0.nupkg`

这些软件包下载后,打开行政 shell 并执行以下命令:

``` bash
choco install -y -s <PATH\TO\DOWNLOADS\> asio cunit eigen tinyxml-usestl tinyxml2 bullet
```

请替换 `<PATH\TO\DOWNLOADS>` 与您下载的软件包的文件夹。

第一次升级 pip 和设置工具 :

``` bash
python -m pip install -U pip setuptools==59.6.0
```

现在安装一些额外的蟒蛇依赖:

``` bash
python -m pip install -U catkin_pkg cryptography empy==3.3.4 importlib-metadata lark==1.1.1 lxml matplotlib netifaces numpy opencv-python PyQt5 pillow psutil pycairo pydot pyparsing==2.4.7 pyyaml rosdistro
```

<span id="install-qt5"></span>

### 安装 Qt5

下载 [5.12.X 离线安装器](https://www.qt.io/offline-installers) 从 Qt 的网站上。 运行安装器。 请确定选择 `MSVC 2017 64-bit` 构成部分项下 `Qt` -\> `Qt 5.12.12` 树。

最后,在一位管理员中 `cmd.exe` 窗口设置这些环境变量。下面的命令假设您将其安装到默认位置: `C:\Qt`.

``` bash
setx /m Qt5_DIR C:\Qt\Qt5.12.12\5.12.12\msvc2017_64
setx /m QT_QPA_PLATFORM_PLUGIN_PATH C:\Qt\Qt5.12.12\5.12.12\msvc2017_64\plugins\platforms
```

> **说明**
>
> 此路径可能基于已安装的MSVC版本而改变,目录Qt被安装到,而Qt的版本被安装.

<span id="rqt-dependencies"></span>

### RQt 依赖关系

要运行 rqt_graph 您需要 [下载](https://graphviz.gitlab.io/_pages/Download/Download_windows.html) 并安装 [图维兹](https://graphviz.gitlab.io/)。安装者将询问是否要在 PATH 中添加图维兹,选择将其添加到当前用户或所有用户中。

<span id="downloading-ros-2"></span>

## 正在下载 ROS 2

- 转到发布页面 : <https://github.com/ros2/ros2/releases>

- 下载 Windows 的最新软件包,例如, `ros2-rolling-*-windows-release-amd64.zip`.

> **说明**
>
> 可能有多个二进制下载选项, 可能导致文件名称不同 。

> **说明**
>
> 为 ROS 2 安装调试库, 请参见 [调试的额外内容](#extra-stuff-for-debug)。然后继续下载 `ros2-package-windows-debug-AMD64.zip`.

- 将拉链文件放入某个地方( 我们假设) `C:\dev\ros2_rolling`).

> **说明**
>
> 这些二进制是使用释放构建配置构建的。在Windows(MSVC)上,这意味着下游二进制(如您自己的节点)也需要使用释放配置或RelWithDebInfo配置来构建ABI兼容性。要使用另一个构建配置构建下游软件包, [从源创建 ROS 2](Alternatives/Windows-Development-Setup.md)。例如,使用调试:
>
> ``` console
> $ colcon build --cmake-args -DCMAKE_BUILD_TYPE=Debug
> ```

<span id="install-additional-dds-implementations-optional"></span>

## 安装额外的 DDS 执行( 可选)

如果您想要使用默认以外的另一个 DDS 或 RTPS 供应商, 您可以找到指令 [这儿](RMW-Implementations.md).

<span id="environment-setup"></span>

## 环境设置

启动命令 shell 并源代码为 ROS 2 的设置文件来设置工作空间 :

``` console
$ call C:\dev\ros2_rolling\local_setup.bat
```

正常的情况是,如果前一个命令没有出错,则输出 `The system cannot find the path specified.` 一次就是这样了

<span id="try-some-examples"></span>

## 尝试一些例子

在命令外壳中, 设置上述 ROS 2 环境, 然后运行 C++ `talker`:

``` console
$ ros2 run demo_nodes_cpp talker
```

启动另一个命令 shell 并运行 Python `listener`:

``` console
$ ros2 run demo_nodes_py listener
```

你应该看看 `talker` 说,这是 `Publishing` 信件和资料 `listener` 说 `I heard` 这证明C++和Python API都正常工作。万岁!

<span id="next-steps-after-installing"></span>

## 安装后的下一步

继续 [教程和演示](../Tutorials.md) 来配置环境,创建自己的工作空间和软件包,并学习ROS 2核心概念。

<span id="additional-rmw-implementations-optional"></span>

## 更多落实RMW(可选)

ROS 2 使用的默认中间软件是 `Fast DDS`,但中间软件(RMW)可以在运行时替换。 [指南](../How-To-Guides/Working-with-multiple-RMW-implementations.md) 如何与多个RMW合作。

<span id="troubleshooting"></span>

## 麻烦的解决

可以找到解决问题的技巧 [这儿](../How-To-Guides/Installation-Troubleshooting.md#windows-troubleshooting).

<span id="uninstall"></span>

## 卸载

1.  如果您按照上述指示将工作空间安装在colcon上, " 完全取消 " 可能只是打开一个新的终端,而不是提供工作空间。 `setup` 文件。这样,您的环境将表现为没有在您的系统中安装滚动。

2.  如果您还试图腾出空间, 您可以删除整个工作空间目录 :

    ``` console
    $ rmdir /s /q \ros2_rolling
    ```

<span id="extra-stuff-for-debug"></span>

## 调试的额外内容

要下载 ROS 2 调试库, 您需要下载 `ros2-rolling-*-windows-debug-AMD64.zip`。请注意,调试库需要更多的配置/设置,以便进行下文所述的工作。

Python 安装可能需要修改以启用调试符号和调试二进制 :

- 在窗口中搜索 **搜索栏** 打开 **应用和特征**.

- 搜索已安装的 Python 版本 。

- 单击修改。

  > [![](images/python_installation_modify.png)](images/python_installation_modify.png)

- 单击下一个要跳转到 **高级选项**.

  > [![](images/python_installation_next.png)](images/python_installation_next.png)

- 记得 **下载调试符号** 财务报告和财务报告 **下载调试二进制** 选中此项。

  > [![](images/python_installation_enable_debug.png)](images/python_installation_enable_debug.png)

- 点击安装 。
