---
translation_status: machine_translated
source: Installation/Alternatives/Windows-Development-Setup.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="windows-source"></span> <span id="windows-latest"></span>

# Windows（源码安装）

本指南涉及如何在Windows上为ROS 2设置开发环境.

<span id="system-requirements"></span>

## 系统要求

仅支持Windows 10.

<span id="language-support"></span>

### 语文支助

确定您有支持的地址 `UTF-8`。例如,对于中文 Windows 10 的安装,您可能需要安装一个 [英语包](https://support.microsoft.com/en-us/windows/language-packs-for-windows-a5094319-a92d-18de-5b53-1cfc697cfca8).

<span id="installing-prerequisites"></span>

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

> ![](../images/windows-vs-studio-install.png)

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

<span id="additional-prerequisites"></span>

## 其他先决条件

从源头建造时,您需要安装一些附加先决条件。

<span id="install-additional-prerequisites-from-chocolatey"></span>

### 安装巧克力的附加先决条件

``` console
$ choco install -y cppcheck curl git winflexbison3
```

您需要附加 Git cmd 文件夹 `C:\Program Files\Git\cmd` 转到 PATH( 您可以点击 Windows 图标, 点击“ 环境变量” , 然后点击“ 编辑系统环境变量 ” 。 在相应的对话框中, 请点击“ 环境变量 ” , 在底板上点击“ Path” , 然后点击“ 编辑” 并添加路径 )。

<span id="install-python-prerequisites"></span>

### 安装 Python 先决条件

安装额外的 Python 依赖 :

``` bash
$ pip install -U colcon-common-extensions coverage flake8 flake8-blind-except flake8-builtins flake8-class-newline flake8-comprehensions flake8-deprecated flake8-docstrings flake8-import-order flake8-quotes mock mypy==0.931 pep8 pydocstyle pytest pytest-mock vcstool
```

<span id="install-miscellaneous-prerequisites"></span>

### 安装杂项先决条件

下一次安装 xmllint :

- 下载 [64位二进制文件](https://www.zlatkovic.com/pub/libxml/64bit/) 页:1 `libxml2` (及其附属关系) `iconv` 财务报告和财务报告 `zlib`从。 <https://www.zlatkovic.com/projects/libxml/>

- 将所有档案解装为例如. `C:\xmllint`

- 添加 `C:\xmllint\bin` 页:1 `PATH`.

<span id="get-the-ros-2-code"></span>

## 获取 ROS 2 代码

现在有了开发工具,我们就可以得到ROS 2源代码.

首先设置一个开发文件夹, 例如 `C:\rolling`:

> **说明**
>
> 由于Windows路径限制较短(260个字符),选择的路径很短,这一点非常重要。要允许更长的路径,请参见 [最大文件路径限制](https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=registry).

``` bash
$ md \rolling\src
$ cd \rolling
```

拿来 `ros2.repos` 文件定义了要复制的寄存器:

``` console
$ vcs import --input https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos src
```

<span id="install-additional-dds-implementations-optional"></span>

### 安装额外的 DDS 执行( 可选)

Fast DDS 与 ROS 2 源捆绑在一起,除非您放置一个 `COLCON_IGNORE` 文档中 `src\eProsima` 文件夹。

如果您想要在默认之外使用另一个 DDS 或 RTPS 供应商, 您可以找到指令 [这儿](../RMW-Implementations.md).

<span id="build-the-ros-2-code"></span>

## 构建 ROS 2 代码

<span id="windows-dev-build-ros2"></span>

要建立ROS 2, 您需要 Visual Studio Command Express (“x64 土著工具 Command Express for VS 2019”) 作为管理员运行 。

来建造 `\rolling` 文件夹树 :

``` console
$ colcon build --merge-install
```

> **说明**
>
> 我们正使用 `--merge-install` 来这是为了避免 `PATH` 变量。如果您正在调整这些指令以构建一个更小的工作空间,那么您可能可以使用孤立安装的默认行为,即将每个软件包安装到不同的文件夹。

> **说明**
>
> 如果您正在做调试构建使用 `python_d path\to\colcon_executable` `colcon`。见 [调试模式的额外内容](#extra-stuff-for-debug-mode) 用于调试中运行 Python 代码的更多信息基于 Windows 。

> **说明**
>
> 鉴于大量软件包被拖入工作空间,源安装需要很长时间。

<span id="setup-environment"></span>

## 设置环境

启动命令 shell 并源代码为 ROS 2 的设置文件来设置工作空间 :

``` console
$ call C:\rolling\install\local_setup.bat
```

这将自动为任何已建立支助的DDS供应商建立环境。

正常的情况是,如果前一个命令没有出错,则输出 `The system cannot find the path specified.` 一次就是这样了

<span id="test-and-run"></span>

## 测试和运行

请注意,您第一次运行任何可执行文件时, 您必须允许通过 Windows 防火墙弹出访问网络 。

您可以使用此命令运行测试 :

``` console
$ colcon test --merge-install
```

> **说明**
>
> `--merge-install` 只有在建造步骤中也使用时才应使用。

之后您可以使用此命令获取测试摘要 :

``` console
$ colcon test-result
```

要运行实例,首先打开一个干净的新 `cmd.exe` 并设置工作空间 通过源代码 `local_setup.bat` 文件。然后运行 C++ `talker`:

``` console
$ call install\local_setup.bat
$ ros2 run demo_nodes_cpp talker
```

在一个单独的外壳中,你可以做同样的, 但运行一个Python `listener`:

``` console
$ call install\local_setup.bat
$ ros2 run demo_nodes_py listener
```

你应该看看 `talker` 说,这是 `Publishing` 信件和资料 `listener` 说 `I heard` 这证明C++和Python API都正常工作。万岁!

> **说明**
>
> 不建议在您所找到的相同 cmd 提示中构建 `local_setup.bat`.

<span id="next-steps-after-installing"></span>

## 安装后的下一步

继续 [教程和演示](../../Tutorials.md) 来配置环境,创建自己的工作空间和软件包,并学习ROS 2核心概念。

<span id="additional-rmw-implementations-optional"></span>

## 更多落实RMW(可选)

ROS 2 使用的默认中间软件是 `Fast DDS`,但中间软件(RMW)可以在运行时替换。 [指南](../../How-To-Guides/Working-with-multiple-RMW-implementations.md) 如何与多个RMW合作。

<span id="extra-stuff-for-debug-mode"></span>

## 调试模式的额外内容

如果您想在调试模式下运行所有测试, 您需要再安装几件东西 :

- 为了能够提取Python源柏油丸,可以使用PeaZip:

``` bash
choco install -y peazip
```

- 您还需要 SVN, 因为一些 Python 源建依赖通过 SVN 检查 :

``` bash
choco install -y svn hg
```

- 您需要退出, 并在安装上述命令后重新启动命令 。

- 获取并提取 Python 3.8.3 来源于 `tgz`:

  - [⁇ -3.8.3](https://www.python.org/ftp/python/3.8.3/Python-3.8.3.tgz)

  - 为使这些指示简明扼要,请将其摘录至 `C:\dev\Python-3.8.3`

- 现在, 从 Visual Studio 命令提示中以调试模式构建 Python 源 :

``` bash
cd C:\dev\Python-3.8.3\PCbuild
get_externals.bat
build.bat -p x64 -d
```

- 最后,将构建的产品复制到Python38安装目录中,放在发布-mode Python可执行器和DLL的旁边:

``` bash
cd C:\dev\Python-3.8.3\PCbuild\amd64
copy python_d.exe C:\Python38 /Y
copy python38_d.dll C:\Python38 /Y
copy python3_d.dll C:\Python38 /Y
copy python38_d.lib C:\Python38\libs /Y
copy python3_d.lib C:\Python38\libs /Y
copy sqlite3_d.dll C:\Python38\DLLs /Y
for %I in (*_d.pyd) do copy %I C:\Python38\DLLs /Y
```

- 现在,从一个新的命令提示, 确保 `python_d` 工作内容 :

``` bash
python_d -c "import _ctypes ; import coverage"
```

- 一旦你核实了... `python_d`,需要重新安装一些与调试启用的库的依赖性:

``` bash
python_d -m pip install --force-reinstall https://github.com/ros2/ros2/releases/download/numpy-archives/numpy-1.18.4-cp38-cp38d-win_amd64.whl
python_d -m pip install --force-reinstall https://github.com/ros2/ros2/releases/download/lxml-archives/lxml-4.5.1-cp38-cp38d-win_amd64.whl
```

- 为核实这些依赖关系的安装情况:

``` bash
python_d -c "from lxml import etree ; import numpy"
```

- 当您希望返回到构建释放二进制时,需要解禁调试变体并使用释放变体:

``` bash
python -m pip uninstall numpy lxml
python -m pip install numpy lxml
```

- 创建可执行文件 python 脚本( S)`.exe`), python_d 用于引用colcon

``` bash
python_d path\to\colcon_executable build
```

- 万岁,你完蛋了!

<span id="stay-up-to-date"></span>

## 保持最新进展

见 [维护源码工作副本](../Maintaining-a-Source-Checkout.md) 以定期刷新您的源安装。

<span id="troubleshooting"></span>

## 麻烦的解决

可以找到解决问题的技巧 [这儿](../../How-To-Guides/Installation-Troubleshooting.md#windows-troubleshooting).

<span id="uninstall"></span>

## 卸载

1.  如果您按照上述指示将工作空间安装在colcon上, " 完全取消 " 可能只是打开一个新的终端,而不是提供工作空间。 `setup` 文件。这样,您的环境将表现为没有在您的系统中安装滚动。

2.  如果您还试图腾出空间, 您可以删除整个工作空间目录 :

    ``` console
    $ rmdir /s /q \ros2_rolling
    ```
