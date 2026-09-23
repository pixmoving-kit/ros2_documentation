---
translation_status: machine_translated
source: How-To-Guides/ROS-2-IDEs.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="ides-and-debugging-community-contributed"></span>

# IDE 与调试（社区贡献）

ROS 2不是围绕特定的开发环境制作的,主要重点是构建/运行于命令行. 尽管如此,集成开发环境(IDE)可用于开发,运行和/或调试ROS 2节点.

下面列出一些IDE和关于如何使用ROS 2的指令.

<span id="general"></span>

## A. 概况

<span id="installed-python-code"></span> <span id="installedpythoncode"></span>

### 已安装 Python 代码

默认情况下,在构建工作空间时:

``` console
$ colcon build
```

Python 代码将会被处理到 `build`/`install` 目录。 因此, 当将调试器附加到 `ros2 run` 命令,从 IDE 内部运行代码(从 `build`/`install`)与IDE项目中打开的文件不同.

处理该问题有两种选择:

- 打开源文件从 `build`/`install` 将断点放在那里。

- 创建工作空间与 [- 符号链接-内置](https://colcon.readthedocs.io/en/released/reference/verb/build.html#command-line-arguments) 标记为 colcon, 这将将源文件链接到 `build`/`install` 取而代之的是目录。

<span id="visual-studio-code"></span>

## 视觉工作室代码

[甚高频](https://code.visualstudio.com/) 是一个多才多艺的自由发展环境。

VSCode与ROS 2. 相对容易使用,只需在命令行中激活您的环境,从同一个终端启动VSCode应用程序并正常使用。所以:

1.  创建正常的 ROS 工作空间 。

2.  在终端中, 源代码为ROS 2 和您的安装( 如果已经建成) 。

3.  从同一命令行启动 VSCode。 终端将被屏蔽, 直至应用程序再次关闭 。

##### Linux

``` console
$ source /opt/ros/rolling/setup.bash
$ cd ~/dev_ws
$ source ./install/setup.bash
$ /usr/bin/code ./src/my_node/
```

##### macOS

``` console
$ . ~/ros2_install/ros2-osx/setup.bash
$ cd ~/dev_ws
$ . ./install/setup.bash
$ /Applications/Visual Studio Code.app/Contents/Resources/app/bin/code ./src/my_node/
```

##### Windows

在 Windows 命令行界面中 :

``` console
$ call C:\dev\ros2\local_setup.bat
$ cd C:\dev_ws
$ call .\install\local_setup.bat
$ "C:\Program Files\Microsoft VS Code\Code.exe" .\src\my_node\
```

或于权壳中:

``` console
$ C:\dev\ros2\local_setup.ps1
$ cd C:\dev_ws
$ .\install\local_setup.ps1
$ & "C:\Program Files\Microsoft VS Code\Code.exe" .\src\my_node\
```

VSCode和VSCode内部创建的任何终端都会从母环境正确继承,并且应该有ROS和安装的软件包可用.

> **说明**
>
> 在添加软件包或进行重大修改之后, 您可能需要再次源代码安装。 最简单的方式就是关闭 VSCode 并像上面一样重新启动它 。

<span id="python"></span>

### Python

在您的工作空间中, 校验正确的解释器。 通过获取基本命令 `python` 应该正确,但 VSCode 喜欢为 Python 使用绝对路径。在右下角点击“ S选定 Python 解释器 ” 来修改它。

如果您的 ROS 2 Python 版本来自虚拟环境, VSCode 将尝试在每个运行命令中源代码。 但我们已经从源环境启动 VSCode, 因此这个额外步骤没有必要 。 您可以为当前工作空间禁用此功能, 方法是找到“ 设置” \> “ 扩展” \> “ Python” \> “ 激活环境” 并取消检查功能 。

现在只需运行一个文件或者在其中创建配置 `launch.json`。调试节点最容易通过创建类似一个配置 `python ...` 命令,而不是 `ros2 run/launch ...`。示例 `launch.json` 可以是:

``` default
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: File",
            "type": "python",
            "request": "launch",
            "program": "my_node.py"
        },
    ]
}
```

相反,你也可以在“使用进程一”下创建一个附加在运行中进程的配置。

见 [设置 ROS 2 并带有 VSCode 和 Docker](Setup-ROS-2-with-VSCode-and-Docker-Container.md) 用于完整说明如何使用VSCode,与Docker结合使用.

<span id="pycharm"></span>

## PyCharm 药效

[PyCharm 药效](https://www.jetbrains.com/pycharm/) 是 Python 专用的 IDE 。

当然它只能被有意义地用于在Python制造的节点.

使用 PyCharm 可以附加到已有进程( 可能由您通过 `ros2 run ...` 或 时 间 `ros2 launch ...`)或直接从 Python 运行节点(相当于 `python [file.py]`.

<span id="integrate-for-code-inspection"></span>

### 编码检查整合

您可以设置您的 PyCharm 项目, 这样它完全意识到 ROS 2 代码, 允许代码完成和建议 。

<span id="linux"></span>

#### Linux

打开终端, 源 ROS 并启动 PyCharm :

``` console
$ source /opt/ros/humble/setup.bash
$ cd path/to/dev_ws
$ /opt/pycharm/bin/pycharm.sh
```

在选择正确的翻译后,一切都应该奏效.

> **说明**
>
> 这是未经检验的。

<span id="windows"></span>

#### Windows

首先从命令行中获取ROS,然后启动 PyCharm 似乎对 Windows 没有任何效果。 相反,一些设置需要调整 。

1.  创建正常的 ROS 工作空间 。

2.  正常启动 PyCharm 。

3.  打开一个工程。 这应该是您正在开发的 ROS 节点的根目录, 例如 。 `C:\dev_ws\src\my_node`.

4.  点击“ 添加新解释器” \> “ 添加本地解释器... ” 。 请选择一个系统解释器( 如果您正在使用虚拟环境) , 并选择 ROS Python 版本的可执行文件( 通常情况下) `C:\Python38\python.exe`).

    > - 如果您现在打开一个代码文件, 您将会看到关于丢失导入的警告 。 试图运行文件将会确认这些问题 。

5.  在“ Python 解释器” 窗口下, 查找并选择 ROS 解释器。 将名称编辑为可以识别的东西。 更重要的是, 现在单击“ 显示解释器路径 ” 按钮 。

6.  在新窗口中, 您可以看到已经与此解释器相关的路径 。 单击“ +” 按钮, 并添加两个路径( 根据您安装的 ROS ):

    > - `C:\dev\ros2_humble\bin`
    >
    > - `C:\dev\ros2_humble\Lib\site-packages`

PyCharm 将重新索引, 完成后它应该正确解释您的项目, 识别 ROS 2 系统包。 您可以按预期浏览代码、 完成并读取 doc 模糊度 。

如果在您的软件包旁建有依赖性, 则它们可能尚未被识别, 并导致无效的IDE警告和运行时间错误 。

决议如下:

- 保证 `PATH` 运行/调试配置中的覆盖既包括ROS 2安装,也包括您的工作空间,例如:

  ``` console
  $ C:\dev\ros2_humble\local_setup.ps1
  $ C:\dev_ws\install\local_setup.ps1
  $ echo $ENV:Path
  ```

- 从“%s”中添加相关文件夹 `install/` 您的项目源目录 。

  转到“......安排”,在“项目”栏下,点击“添加内容根”。 `site-packages` 文件夹在 `install/Lib/*`.

  最后, 请确保您的运行/ 调试配置具有“ 包含 PYTHONPATH 中的内容根” 的选项 。

> **提示**
>
> 使用 [- 即兴上演](https://colcon.readthedocs.io/en/released/user/isolated-vs-merged-workspaces.html) 选项将限制目录的数量,从而更容易配置 PyCharm 。

<span id="attach-to-process"></span>

### 附着于进程

即使没有任何配置到 PyCharm, 你也可以一直附加在运行中的 Python 节点上。 打开您的工程源并像往常一样运行您的节点 :

``` console
$ ros2 run my_node main
```

然后在 PyCharm 中选择“ 运行” \> “ 启动到进程... ” 。 可能需要一秒钟, 但一个小窗口应该显示当前运行的 Python 实例, 包括您的节点。 可能有多个 Python 进程, 因此可能有一些尝试和错误来找到正确的进程 。

在选择实例后, 通常的调试工具是可用的。 您可以在代码中暂停或创建断点并踩过它 。

> **说明**
>
> 您工程中的代码可能不是正在执行的文件 。 [这个](#installedpythoncode).

<span id="run-debug"></span>

### Run/Debug

紧随整合步骤先行.

从 PyCharm 运行您的 PyThon 文件可能会导致导入错误。 这是因为 PyCharm 扩展了 `PYTHONPATH` 环境变异,但它离开 `PATH` 未触及。 必要的库文件在 `ros/bin` 无法找到。

编辑您文件的运行/ 调试配置, 并在“ 环境变量 : ” 下添加一个新的变量。 目前不支持扩展已有的变量 `PATH`,所以我们需要覆盖它。从源代码ROS终端输出内容。 `PATH` 改为: `echo $Env:PATH`。复制结果。

回到PyCharm,把它贴上 `PATH`,应用修改并运行或调试您的节点。它应该像现在的任何 Python 项目一样工作,允许容易地添加断点和其他调试方法。

> **说明**
>
> 在Windows上,它似乎是资本化的 `PATH` “环境变量:”下的变量必须是“路径”(所有小写),才能工作。
