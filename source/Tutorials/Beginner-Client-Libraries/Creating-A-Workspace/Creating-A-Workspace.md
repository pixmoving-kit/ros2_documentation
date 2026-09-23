<span id="creating-a-workspace"></span> <span id="ros2workspace"></span>
# 创建工作空间

**目标：** 创建工作空间，学习设置用于开发和测试的上层工作空间。

**教程级别：** 初学者

**预计用时：** 20 分钟

<span id="background"></span>
## 背景

工作空间是包含 ROS 2 软件包的目录。使用 ROS 2 之前，必须在准备工作的终端中加载 ROS 2 安装工作空间的环境，这样才能在该终端中使用 ROS 2 软件包。

也可以加载另一个“上层工作空间”（overlay），在其中添加新软件包，同时不影响被扩展的现有 ROS 2 工作空间，也就是“底层工作空间”（underlay）。底层必须包含上层所有软件包所需的依赖。上层的软件包会覆盖底层的同名软件包。还可以叠加多个层次，后续的上层工作空间使用其下各层工作空间中的软件包。

<span id="prerequisites"></span>
## 前提条件

- [安装 ROS 2](../../../Installation.md)。
- [安装 colcon](../Colcon-Tutorial.md)。
- [安装 git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)。
- [安装 turtlesim](../../Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md)。
- [安装 rosdep](../../Intermediate/Rosdep.md)。
- 了解基本终端命令，可参考 [SFU 的 Linux/Unix 指南](https://www2.cs.sfu.ca/~ggbaker/reference/unix/)或 [Ubuntu Linux 命令行入门](https://ubuntu.com/desktop/docs/en/latest/tutorial/the-linux-command-line-for-beginners/)。
- 准备自己熟悉的文本编辑器。

<span id="tasks"></span>
## 操作步骤

<span id="source-ros-2-environment"></span>
### 1 加载 ROS 2 环境

本教程以主 ROS 2 安装作为底层工作空间。不过，底层工作空间不一定必须是主 ROS 2 安装。

具体命令取决于安装方式（源码或二进制）和操作系统。

**Linux**

```console
$ source /opt/ros/rolling/setup.bash
```

**macOS**

```console
$ . ~/ros2_install/ros2-osx/setup.bash
```

**Windows**

接下来需要构建工作空间，请使用 `x64 Native Tools Command Prompt for VS 2019`：

```console
$ call C:\dev\ros2\local_setup.bat
```

如果这些命令不适用，请参考你使用的[安装指南](../../../Installation.md)。

<span id="create-a-new-directory"></span> <span id="new-directory"></span>
### 2 创建新目录

推荐为每个新工作空间创建独立目录。名称没有限制，但最好能体现用途。这里用 `ros2_ws` 表示开发工作空间。

**Linux**

```console
$ mkdir -p ~/ros2_ws/src
$ cd ~/ros2_ws/src
```

**macOS**

```console
$ mkdir -p ~/ros2_ws/src
$ cd ~/ros2_ws/src
```

**Windows**

```console
$ md \ros2_ws\src
$ cd \ros2_ws\src
```

另一个推荐做法是将工作空间中的软件包都放在 `src` 目录。上述命令在 `ros2_ws` 内创建 `src`，然后进入该目录。

<span id="clone-a-sample-repo"></span>
### 3 克隆示例仓库

克隆前，确认仍位于 `ros2_ws/src` 目录。

后续初学者开发教程会创建你自己的软件包；现在先使用现有软件包练习搭建工作空间。

如果学习过[初学者命令行工具教程](../../Beginner-CLI-Tools.md)，你应该已经熟悉 [ros_tutorials](https://github.com/ros/ros_tutorials/) 中的 `turtlesim` 软件包。

仓库可能有多个分支，需要检出与已安装 ROS 2 发行版对应的分支。克隆时添加 `-b` 参数，后面指定分支名。

在 `ros2_ws/src` 中运行：

```console
$ git clone https://github.com/ros/ros_tutorials.git -b rolling
```

现在 `ros_tutorials` 已克隆到工作空间。它包含本教程接下来使用的 `turtlesim` 软件包。仓库中的其他软件包带有 `COLCON_IGNORE` 文件，因此不会构建。

虽然工作空间已经包含示例软件包，但尚不能直接使用。还需要先安装依赖，再构建工作空间。

<span id="resolve-dependencies"></span>
### 4 安装依赖

构建前需要解决软件包的依赖。即使可能已经安装了全部依赖，也推荐每次克隆后检查一次，避免等待很久后才因缺少依赖而构建失败。

从工作空间根目录 `ros2_ws` 运行以下命令。

**Linux**

如果仍位于存放 `ros_tutorials` 的 `src` 目录，先用 `cd ..` 返回工作空间根目录：

```console
$ cd ..
$ rosdep install -i --from-path src --rosdistro rolling -y
```

**macOS 和 Windows**

本教程的 rosdep 步骤仅适用于 Linux，可直接跳到“5 使用 colcon 构建工作空间”。

如果在 Linux 上通过源码或二进制归档包安装 ROS 2，请使用相应安装说明中的 rosdep 命令：见[源码安装的 rosdep 步骤](../../../Installation/Alternatives/Ubuntu-Development-Setup.md#linux-development-setup-install-dependencies-using-rosdep)和[二进制归档包安装的依赖步骤](../../../Installation/Alternatives/Ubuntu-Install-Binary.md#linux-install-binary-install-missing-dependencies)。

全部依赖安装好后，终端会显示：

```text
#All required rosdeps installed successfully
```

软件包在 `package.xml` 中声明依赖，下一篇教程会进一步介绍软件包。该命令遍历这些声明并安装缺失的依赖。其他教程还会进一步介绍 `rosdep`。

<span id="build-the-workspace-with-colcon"></span>
### 5 使用 colcon 构建工作空间

现在可以在工作空间根目录 `ros2_ws` 中构建软件包。

**Linux**

```console
$ colcon build
Starting >>> turtlesim
Finished <<< turtlesim [5.49s]

Summary: 1 package finished [5.58s]
```

**macOS**

```console
$ colcon build
Starting >>> turtlesim
Finished <<< turtlesim [5.49s]

Summary: 1 package finished [5.58s]
```

**Windows**

```console
$ colcon build --merge-install
Starting >>> turtlesim
Finished <<< turtlesim [5.49s]

Summary: 1 package finished [5.58s]
```

Windows 存在路径长度限制，因此 `merge-install` 将各软件包合并安装到 `install` 目录。

!!! note "注意"
    `colcon build` 还有一些常用参数：

    - `--packages-up-to`：只构建指定软件包及其全部依赖，避免构建整个工作空间，节省时间。
    - `--symlink-install`：修改 Python 脚本后无需每次都重新构建。
    - `--event-handlers console_direct+`：构建时直接显示控制台输出，否则可在 `log` 目录中查看。
    - `--executor sequential`：逐个处理软件包，不进行并行构建。

构建完成后，在工作空间根目录 `~/ros2_ws` 中运行以下命令，查看 colcon 创建的新目录。

**Linux**

```console
$ ls
build  install  log  src
```

**macOS**

```console
$ ls
build  install  log  src
```

**Windows**

```console
$ dir
build  install  log  src
```

`install` 中包含工作空间的环境设置文件，可用于加载上层工作空间。

<span id="source-the-overlay"></span>
### 6 加载上层工作空间

加载上层环境前，务必打开一个新终端，不要使用刚才构建工作空间的终端。在构建所用终端中加载上层环境，或者在已加载上层环境的终端中构建，都可能产生复杂问题。

在新终端中，先加载主 ROS 2 环境作为底层，以便在其上叠加新工作空间。

**Linux**

```console
$ source /opt/ros/rolling/setup.bash
```

**macOS**

```console
$ . ~/ros2_install/ros2-osx/setup.bash
```

**Windows**

该终端不用于构建工作空间，因此可以使用普通命令提示符：

```console
$ call C:\dev\ros2\local_setup.bat
```

进入工作空间根目录。

**Linux**

```console
$ cd ~/ros2_ws
```

**macOS**

```console
$ cd ~/ros2_ws
```

**Windows**

```console
$ cd \ros2_ws
```

在根目录中加载上层环境。

**Linux**

```console
$ source install/local_setup.bash
```

**macOS**

```console
$ . install/local_setup.bash
```

**Windows**

```console
$ call install\setup.bat
```

!!! note "注意"
    加载上层的 `local_setup`，只会将该上层工作空间中的软件包添加到环境。`setup` 则会同时加载上层和创建它时所用的底层环境，让两个工作空间都可用。

    因此，先加载主 ROS 2 安装的 `setup`，再加载 `ros2_ws` 的 `local_setup`，与直接加载 `ros2_ws` 的 `setup` 等效，因为后者包含底层环境。

现在可以运行上层工作空间中的 `turtlesim`：

```console
$ ros2 run turtlesim turtlesim_node
```

如何确定运行的是上层的 turtlesim，而非主安装中的版本？接下来修改上层中的 turtlesim，观察以下效果：

- 可以独立修改和重新构建上层的软件包，不影响底层。
- 上层的优先级高于底层。

<span id="modify-the-overlay"></span>
### 7 修改上层工作空间

通过修改窗口标题来改变上层中的 `turtlesim`。找到 `~/ros2_ws/src/ros_tutorials/turtlesim/src/turtle_frame.cpp`，用文本编辑器打开。

找到 `setWindowTitle("TurtleSim");`，将 `"TurtleSim"` 改为 `"MyTurtleSim"`，并保存。

回到之前运行 `colcon build` 的第一个终端，再次执行构建。

回到已加载上层环境的第二个终端，再次运行 turtlesim：

```console
$ ros2 run turtlesim turtlesim_node
```

现在窗口标题会显示为“MyTurtleSim”。

![修改后的上层 turtlesim](images/overlay.png)

虽然这个终端之前加载了主 ROS 2 环境，但 `ros2_ws` 上层环境中的内容优先于底层。

要确认底层仍然保持原样，打开一个全新的终端，只加载主 ROS 2 安装环境，然后运行 turtlesim：

```console
$ ros2 run turtlesim turtlesim_node
```

![保持原样的底层 turtlesim](images/underlay.png)

可以看到，上层中的修改没有影响底层。

<span id="summary"></span>
## 小结

本教程将主 ROS 2 发行版安装作为底层环境，通过在新工作空间中克隆和构建软件包创建上层。上层路径会添加到搜索路径的前面，因此优先于底层，修改 turtlesim 的实验展示了这一点。

开发少量软件包时，推荐使用上层工作空间，无需将全部软件包放在一起，也无需在每次迭代时重新构建庞大的工作空间。

<span id="next-steps"></span>
## 后续步骤

现在你已经了解如何创建、构建和加载自己的工作空间，接下来学习[创建自己的软件包](../Creating-Your-First-ROS2-Package.md)。
