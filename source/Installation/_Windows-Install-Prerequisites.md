<span id="installing-prerequisites"></span>

## 安装前置依赖

<span id="install-chocolatey"></span>

### 安装 Chocolatey

Chocolatey 是 Windows 的软件包管理器。请按照其安装说明安装：<https://chocolatey.org/install>。

后续将使用 Chocolatey 安装其他开发工具。

<span id="install-python"></span>

### 安装 Python

打开命令提示符，输入以下命令，通过 Chocolatey 安装 Python：

```bash
choco install -y python --version 3.8.3
```

!!! note "说明"

    Chocolatey 会将 Python 安装到 `C:\Python38`，后续安装步骤也假定它位于此处。如果 Python 安装在其他位置，必须将其复制或链接到这个位置。

<span id="install-visual-c-redistributables"></span>

### 安装 Visual C++ 可再发行组件

打开命令提示符，输入以下命令，通过 Chocolatey 安装：

```bash
choco install -y vcredist2013 vcredist140
```

<span id="install-openssl"></span>

### 安装 OpenSSL

打开命令提示符，输入以下命令，通过 Chocolatey 安装 OpenSSL：

```bash
choco install -y openssl --version 1.1.1.2100
```

以下命令设置的环境变量会跨会话保留：

```bash
setx /m OPENSSL_CONF "C:\Program Files\OpenSSL-Win64\bin\openssl.cfg"
```

需要将 OpenSSL-Win64 的 bin 文件夹添加到 `PATH`。点击 Windows 图标，搜索“环境变量”，然后点击“编辑系统环境变量”。在弹出的对话框中点击“环境变量”，选择下方面板中的“Path”，点击“编辑”，添加以下路径：

- `C:\Program Files\OpenSSL-Win64\bin\`

<span id="install-visual-studio"></span>

### 安装 Visual Studio

安装 Visual Studio 2019。如果已经安装了付费版本（Professional 或 Enterprise），可以跳过此步骤。

Microsoft 提供免费的 Visual Studio 2019 Community 版本，可用于构建使用 ROS 2 的应用。[通过此链接下载安装程序](https://aka.ms/vs/16/release/vs_community.exe)。

确保安装 Visual C++ 功能。最简单的方法是在安装时选择 `Desktop development with C++`（使用 C++ 的桌面开发）工作负载。

![Visual Studio 安装选项](images/windows-vs-studio-install.png)

在待安装的组件列表中取消勾选 C++ CMake 工具，确保没有安装这些工具。

<span id="install-opencv"></span>

### 安装 OpenCV

部分示例需要安装 OpenCV。

可以从[此处](https://github.com/ros2/ros2/releases/download/opencv-archives/opencv-3.4.6-vc16.VS2019.zip)下载预编译的 OpenCV `3.4.6`。

假设将其解压到 `C:\opencv`，请在具有管理员权限的命令提示符中运行：

```bash
setx /m OpenCV_DIR C:\opencv
```

由于使用的是预编译 ROS 版本，需要告诉它 OpenCV 库的位置。请将 `C:\opencv\x64\vc16\bin` 添加到 `PATH` 环境变量。

<span id="install-dependencies"></span>

### 安装依赖

一些依赖在 Chocolatey 软件包数据库中不可用。为简化手动安装流程，我们提供了所需的 Chocolatey 软件包。

由于部分 Chocolatey 软件包依赖 CMake，先安装它：

```bash
choco install -y cmake
```

需要将 CMake 的 bin 文件夹 `C:\Program Files\CMake\bin` 添加到 `PATH`。

从[此 GitHub 仓库](https://github.com/ros2/choco-packages/releases/latest)下载以下软件包：

- `asio.1.12.1.nupkg`
- `bullet.3.17.nupkg`
- `cunit.2.1.3.nupkg`
- `eigen.3.3.4.nupkg`
- `tinyxml-usestl.2.6.2.nupkg`
- `tinyxml2.6.0.0.nupkg`

下载后，打开管理员 shell，执行：

```bash
choco install -y -s <PATH\TO\DOWNLOADS\> asio cunit eigen tinyxml-usestl tinyxml2 bullet
```

请将 `<PATH\TO\DOWNLOADS>` 替换为保存下载软件包的文件夹。

先升级 pip 和 setuptools：

```bash
python -m pip install -U pip setuptools==59.6.0
```

然后安装其他 Python 依赖：

```bash
python -m pip install -U catkin_pkg cryptography empy==3.3.4 importlib-metadata lark==1.1.1 lxml matplotlib netifaces numpy opencv-python PyQt5 pillow psutil pycairo pydot pyparsing==2.4.7 pyyaml rosdistro
```

<span id="install-qt5"></span>

### 安装 Qt5

从 Qt 网站下载 [5.12.X 离线安装程序](https://www.qt.io/offline-installers)并运行。确保选中 `Qt` → `Qt 5.12.12` 下的 `MSVC 2017 64-bit` 组件。

最后，在管理员 `cmd.exe` 窗口中设置以下环境变量。下列命令假设安装位置为默认的 `C:\Qt`。

```bash
setx /m Qt5_DIR C:\Qt\Qt5.12.12\5.12.12\msvc2017_64
setx /m QT_QPA_PLATFORM_PLUGIN_PATH C:\Qt\Qt5.12.12\5.12.12\msvc2017_64\plugins\platforms
```

!!! note "说明"

    路径可能因安装的 MSVC 版本、Qt 安装目录以及 Qt 版本而变化。

<span id="rqt-dependencies"></span>

### rqt 依赖

要运行 rqt_graph，需要[下载](https://graphviz.gitlab.io/_pages/Download/Download_windows.html)并安装 [Graphviz](https://graphviz.gitlab.io/)。安装程序会询问是否将 Graphviz 添加到 `PATH`，请选择为当前用户或所有用户添加。
