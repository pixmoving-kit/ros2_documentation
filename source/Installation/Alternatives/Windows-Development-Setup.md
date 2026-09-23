<span id="windows-source"></span>
<span id="windows-latest"></span>

# Windows（源码安装）

本指南介绍如何在 Windows 上设置 ROS 2 开发环境。

<span id="system-requirements"></span>

## 系统要求

仅支持 Windows 10。

<span id="language-support"></span>

### 语言支持

确保使用支持 `UTF-8` 的区域设置。例如，中文版 Windows 10 可能需要安装[英语语言包](https://support.microsoft.com/en-us/windows/language-packs-for-windows-a5094319-a92d-18de-5b53-1cfc697cfca8)。

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

按照 [Windows 前置依赖安装说明](../_Windows-Install-Prerequisites.md)，安装 Chocolatey、Python、Visual C++ 可再发行组件、OpenSSL、Visual Studio、OpenCV、其他依赖、Qt5 和 rqt 依赖。

<span id="additional-prerequisites"></span>

## 其他前置依赖

从源码构建还需要安装一些额外依赖。

<span id="install-additional-prerequisites-from-chocolatey"></span>

### 通过 Chocolatey 安装额外依赖

```console
$ choco install -y cppcheck curl git winflexbison3
```

需要将 Git 的 cmd 文件夹 `C:\Program Files\Git\cmd` 添加到 `PATH`。点击 Windows 图标，搜索“环境变量”，点击“编辑系统环境变量”。在弹出的对话框中点击“环境变量”，选择下方面板中的“Path”，点击“编辑”，然后添加该路径。

<span id="install-python-prerequisites"></span>

### 安装 Python 依赖

安装额外的 Python 依赖：

```bash
$ pip install -U colcon-common-extensions coverage flake8 flake8-blind-except flake8-builtins flake8-class-newline flake8-comprehensions flake8-deprecated flake8-docstrings flake8-import-order flake8-quotes mock mypy==0.931 pep8 pydocstyle pytest pytest-mock vcstool
```

<span id="install-miscellaneous-prerequisites"></span>

### 安装其他依赖

接下来安装 xmllint：

- 从 <https://www.zlatkovic.com/projects/libxml/> 下载 `libxml2` 及其依赖 `iconv`、`zlib` 的 [64 位二进制归档包](https://www.zlatkovic.com/pub/libxml/64bit/)。
- 将所有归档包解压到同一个位置，例如 `C:\xmllint`。
- 将 `C:\xmllint\bin` 添加到 `PATH`。

<span id="get-the-ros-2-code"></span>

## 获取 ROS 2 代码

开发工具准备好后，就可以获取 ROS 2 源码。

首先创建开发文件夹，例如 `C:\rolling`。

!!! note "说明"

    Windows 默认路径长度上限较短（260 个字符），因此务必选择较短的路径。若要启用更长的路径，参阅[最大文件路径长度限制](https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=registry)。

```bash
$ md \rolling\src
$ cd \rolling
```

使用定义了待克隆仓库的 `ros2.repos` 文件获取代码：

```console
$ vcs import --input https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos src
```

<span id="install-additional-dds-implementations-optional"></span>

### 安装其他 DDS 实现（可选）

Fast DDS 随 ROS 2 源码提供，除非在 `src\eProsima` 文件夹中放置 `COLCON_IGNORE` 文件，否则始终会构建它。

如果希望使用默认供应商以外的 DDS 或 RTPS 实现，请参阅 [RMW 实现](../RMW-Implementations.md)。

<span id="build-the-ros-2-code"></span>
<span id="windows-dev-build-ros2"></span>

## 构建 ROS 2 代码

构建 ROS 2 需要以管理员身份运行 Visual Studio 命令提示符（“x64 Native Tools Command Prompt for VS 2019”）。

构建 `\rolling` 文件夹树：

```console
$ colcon build --merge-install
```

!!! note "说明"

    这里使用 `--merge-install`，以避免构建完成后的 `PATH` 环境变量过长。如果按这些说明构建较小的工作空间，也许可以采用默认的隔离安装方式，即每个软件包安装到不同文件夹。

!!! note "说明"

    进行 Debug 构建时，请使用 `python_d path\to\colcon_executable` 调用 `colcon`。有关在 Windows 的 Debug 构建中运行 Python 代码的说明，参阅[Debug 模式所需的额外设置](#extra-stuff-for-debug-mode)。

!!! note "说明"

    工作空间中会拉取大量软件包，因此源码安装可能耗时较长。

<span id="setup-environment"></span>

## 设置环境

打开命令行 shell，加载 ROS 2 设置文件以配置工作空间：

```console
$ call C:\rolling\install\local_setup.bat
```

这会自动配置构建时已启用支持的各个 DDS 供应商实现所需的环境。

如果没有其他错误，上述命令恰好输出一次 `The system cannot find the path specified.` 属于正常现象。

<span id="test-and-run"></span>

## 测试与运行

首次运行任何可执行文件时，需要在 Windows 防火墙弹窗中允许其访问网络。

运行以下命令执行测试：

```console
$ colcon test --merge-install
```

!!! note "说明"

    只有在构建步骤中使用了 `--merge-install`，测试时才应使用它。

随后，运行以下命令查看测试摘要：

```console
$ colcon test-result
```

运行示例时，先打开一个干净的新 `cmd.exe` 窗口，加载 `local_setup.bat` 以配置工作空间，然后运行 C++ `talker`：

```console
$ call install\local_setup.bat
$ ros2 run demo_nodes_cpp talker
```

在另一个 shell 中同样配置环境，但改为运行 Python `listener`：

```console
$ call install\local_setup.bat
$ ros2 run demo_nodes_py listener
```

你应该看到 `talker` 输出 `Publishing`，表示正在发布消息；`listener` 输出 `I heard`，表示收到了这些消息。这说明 C++ 和 Python API 都在正常工作，恭喜！

!!! note "说明"

    不建议在已经加载 `local_setup.bat` 的同一个命令提示符窗口中执行构建。

<span id="next-steps-after-installing"></span>

## 安装后的后续步骤

继续学习[教程和演示](../../Tutorials.md)，配置环境，创建自己的工作空间和软件包，并了解 ROS 2 核心概念。

<span id="additional-rmw-implementations-optional"></span>

## 其他 RMW 实现（可选）

ROS 2 默认使用 `Fast DDS` 中间件，但可以在运行时切换中间件（RMW）。有关使用多个 RMW 的方法，请参阅[指南](../../How-To-Guides/Working-with-multiple-RMW-implementations.md)。

<span id="extra-stuff-for-debug-mode"></span>

## Debug 模式所需的额外设置

如果希望在 Debug 模式下运行全部测试，还需要安装一些组件。

可以使用 PeaZip 解压 Python 源码 tar 包：

```bash
choco install -y peazip
```

部分 Python 源码构建依赖通过 SVN 检出，因此还需要安装 SVN：

```bash
choco install -y svn hg
```

安装完成后，退出并重新打开命令提示符。

下载 [Python 3.8.3 源码 tgz 包](https://www.python.org/ftp/python/3.8.3/Python-3.8.3.tgz)并解压。为简化下文说明，请将其解压到 `C:\dev\Python-3.8.3`。

在 Visual Studio 命令提示符中，以 Debug 模式构建 Python 源码：

```bash
cd C:\dev\Python-3.8.3\PCbuild
get_externals.bat
build.bat -p x64 -d
```

将构建产物复制到 Python38 安装目录，与 Release 模式的 Python 可执行文件和 DLL 放在一起：

```bash
cd C:\dev\Python-3.8.3\PCbuild\amd64
copy python_d.exe C:\Python38 /Y
copy python38_d.dll C:\Python38 /Y
copy python3_d.dll C:\Python38 /Y
copy python38_d.lib C:\Python38\libs /Y
copy python3_d.lib C:\Python38\libs /Y
copy sqlite3_d.dll C:\Python38\DLLs /Y
for %I in (*_d.pyd) do copy %I C:\Python38\DLLs /Y
```

在新的命令提示符中确认 `python_d` 能够正常运行：

```bash
python_d -c "import _ctypes ; import coverage"
```

确认 `python_d` 正常后，需要重新安装几个依赖，使用启用调试的库：

```bash
python_d -m pip install --force-reinstall https://github.com/ros2/ros2/releases/download/numpy-archives/numpy-1.18.4-cp38-cp38d-win_amd64.whl
python_d -m pip install --force-reinstall https://github.com/ros2/ros2/releases/download/lxml-archives/lxml-4.5.1-cp38-cp38d-win_amd64.whl
```

验证这些依赖的安装：

```bash
python_d -c "from lxml import etree ; import numpy"
```

如果要恢复构建 Release 二进制文件，需要卸载 Debug 变体，改用 Release 变体：

```bash
python -m pip uninstall numpy lxml
python -m pip install numpy lxml
```

要为 Python 脚本创建可执行文件（`.exe`），应使用 `python_d` 调用 colcon：

```bash
python_d path\to\colcon_executable build
```

恭喜，设置完成！

<span id="stay-up-to-date"></span>

## 保持更新

参阅[维护源码工作副本](../Maintaining-a-Source-Checkout.md)，定期更新从源码安装的环境。

<span id="troubleshooting"></span>

## 问题排查

参阅 [Windows 问题排查](../../How-To-Guides/Installation-Troubleshooting.md#windows-troubleshooting)。

<span id="uninstall"></span>

## 卸载

1. 如果按照以上说明使用 colcon 安装了工作空间，只需打开一个新终端，不加载工作空间的 `setup` 文件，就可以视为“卸载”。这样，当前环境的行为就如同系统未安装 Rolling 一样。
2. 如果还希望释放空间，可以删除整个工作空间目录：

```console
$ rmdir /s /q \ros2_rolling
```
