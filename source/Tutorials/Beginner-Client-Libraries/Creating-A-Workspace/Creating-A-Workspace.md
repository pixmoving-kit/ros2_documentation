---
translation_status: machine_translated
source: Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="creating-a-workspace"></span> <span id="ros2workspace"></span>

# 创建工作空间

**目标：** 创建一个工作空间,并学习如何设置用于开发和测试的叠加.

**教程级别：** 入门

**用时：** 20分钟

<span id="background"></span>

## 背景

工作空间是一个包含 ROS 2 软件包的目录。 在使用 ROS 2 之前, 您必须在您计划工作的终端中源出 ROS 2 的安装工作空间。 这使得 ROS 2 的软件包可供您在终端中使用 。

您也可以选择获取一个“ 重叠” — — 一个次要的工作空间, 您可以在此添加新的软件包, 而不会干扰您正在扩展的 ROS 2 工作空间, 或“ 插入 ” 。 您的软件包必须包含其覆盖中所有软件包的依赖性 。 在您的覆盖中, 软件包会覆盖其覆盖中的软件包 。 您还可以拥有多个层次的软件包和软件包, 并且每个连续的软件包都使用其母软件包的覆盖 。

<span id="prerequisites"></span>

## 前提条件

- [ROS 2 安装](../../../Installation.md)

- [锥形安装](../Colcon-Tutorial.md)

- [git 安装](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

- [安装龟床](../../Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md)

- 有过 [已安装 rosdep](../../Intermediate/Rosdep.md)

- 了解基本的终端命令

  - [Linux/ Unix 的 SSU 指南](https://www2.cs.sfu.ca/~ggbaker/reference/unix/)

  - [初学者的 Ubuntu Linux 命令行指南](https://ubuntu.com/desktop/docs/en/latest/tutorial/the-linux-command-line-for-beginners/)

- 您选择的文本编辑器

<span id="tasks"></span>

## 操作步骤

<span id="source-ros-2-environment"></span>

### 1 资料来源 ROS 2 环境

您的主要 ROS 2 安装将会是您在此教程中的底板 。 (请注意, 底板并不一定是 ROS 2 的主要安装 。 )

取决于您如何安装ROS 2(来自源代码或二进制),以及您所在的哪个平台, 您的确切源指令会有所不同 :

##### Linux

``` console
$ source /opt/ros/rolling/setup.bash
```

##### macOS

``` console
$ . ~/ros2_install/ros2-osx/setup.bash
```

##### Windows

记住要用一个 `x64 Native Tools Command Prompt for VS 2019` 用于执行以下命令,因为我们将要建立一个工作空间。

``` console
$ call C:\dev\ros2\local_setup.bat
```

咨询 [安装指南](../../../Installation.md) 如果这些命令对你无效, 您会遵守 。

<span id="create-a-new-directory"></span> <span id="new-directory"></span>

### 2 创建新目录

最佳的做法是为每个新的工作空间创建新的目录。 名称并不重要, 但最好能显示工作空间的目的。 让我们选择目录名称 。 `ros2_ws`,用于“开发工作空间”:

##### Linux

``` console
$ mkdir -p ~/ros2_ws/src
$ cd ~/ros2_ws/src
```

##### macOS

``` console
$ mkdir -p ~/ros2_ws/src
$ cd ~/ros2_ws/src
```

##### Windows

``` console
$ md \ros2_ws\src
$ cd \ros2_ws\src
```

另一个最佳做法是将您工作空间中的任何软件包放入 `src` 目录。上面的代码创建了 `src` 内置目录 `ros2_ws` 然后导航进入它。

<span id="clone-a-sample-repo"></span>

### 3 Clone 样回波

确保您仍然在 `ros2_ws/src` 复制前的目录。

在其它的初学者开发器教程中,您会创建自己的软件包,但现在您会利用现有的软件包练习将工作空间组合在一起.

如果你经过 [初学者: CLI 工具](../../Beginner-CLI-Tools.md) 课程,您将熟悉 `turtlesim`,其中一个包在 [ros_tutorials](https://github.com/ros/ros_tutorials/).

重播可以有多个分支。 您需要检查一个针对您安装的ROS 2 distro 的分支。 当你复制此重播时, 请添加 `-b` 接下来是那个分支。

在那个 `ros2_ws/src` 目录,运行以下命令:

``` console
$ git clone https://github.com/ros/ros_tutorials.git -b rolling
```

现在 `ros_tutorials` 在工作空间中复制。 `ros_tutorials` 寄存器包含 `turtlesim` 软件包,我们将在此教程的其余部分使用。此寄存器中的其他软件包不会被构建,因为它们包含 `COLCON_IGNORE` 文档。

到目前为止,您已经用一个样本包将您的工作空间填充,但它不是一个功能完备的工作空间。您需要先解决依赖性,然后建立工作空间。

<span id="resolve-dependencies"></span>

### 4 解决依赖关系

在构建工作空间之前, 您需要解决软件包的依赖性。 您可能已经拥有所有依赖性, 但最佳做法是每次复制时都要检查依赖性。 您不希望一个构建在等待了很长时间后失败, 只意识到您缺少依赖性 。

从您工作空间的根部( R)`ros2_ws`),运行以下命令:

##### Linux

如果你还在, `src` 带有目录 `ros_tutorials` 克隆, 确保运行 `cd ..` 移动到工作空间( E)`ros2_ws`).

``` console
$ cd ..
$ rosdep install -i --from-path src --rosdistro rolling -y
```

##### macOS

rosdep 只运行在 Linux 上, 所以您可以跳过到“ 5 用 colcon 构建工作空间 ” 一节 。

##### Windows

rosdep 只运行在 Linux 上, 所以您可以跳过到“ 5 用 colcon 构建工作空间 ” 一节 。

如果您从源代码或二进制归档中在 Linux 上安装了 ROS 2, 您需要使用其安装指令中的 rosdep 命令。 以下是 [从源的 rosdep 区段](../../../Installation/Alternatives/Ubuntu-Development-Setup.md#linux-development-setup-install-dependencies-using-rosdep) 页:1 [二进制归档 rosdep 段](../../../Installation/Alternatives/Ubuntu-Install-Binary.md#linux-install-binary-install-missing-dependencies).

如果您已经拥有了所有的依赖关系, 控制台会返回 :

``` text
#All required rosdeps installed successfully
```

软件包在 package.xml 文件中声明其依赖性( 您将在下一个教程中更多地了解软件包) 。 此命令会浏览这些声明并安装缺失的。 您可以更多地了解 `rosdep` 以别的教诲盟誓,

<span id="build-the-workspace-with-colcon"></span>

### 5 用 colcon 构建工作空间

从您工作空间的根部( R)`ros2_ws`,您现在可以使用命令构建您的软件包 :

##### Linux

``` console
$ colcon build
Starting >>> turtlesim
Finished <<< turtlesim [5.49s]

Summary: 1 package finished [5.58s]
```

##### macOS

``` console
$ colcon build
Starting >>> turtlesim
Finished <<< turtlesim [5.49s]

Summary: 1 package finished [5.58s]
```

##### Windows

``` console
$ colcon build --merge-install
Starting >>> turtlesim
Finished <<< turtlesim [5.49s]

Summary: 1 package finished [5.58s]
```

Windows 不允许长路径, 所以 `merge-install` 将所有路径结合到 `install` 目录。

> **说明**
>
> 其他有用的论据 `colcon build`:
>
> - `--packages-up-to` 构建您想要的软件包, 加上其所有依赖性, 但不是整个工作空间( 保存时间)
>
> - `--symlink-install` 省得你每次修剪python脚本都要重建
>
> - `--event-handlers console_direct+` 显示构建时主控台输出( 可在 `log` 目录)
>
> - `--executor sequential` 逐一处理软件包,而不是使用并行性

一旦构建完成,请在工作空间根中输入命令(`~/ros2_ws`。您将看到 collcon 创建了新的目录 :

##### Linux

``` console
$ ls
build  install  log  src
```

##### macOS

``` console
$ ls
build  install  log  src
```

##### Windows

``` console
$ dir
build  install  log  src
```

那个... `install` 目录是您工作空间的设置文件所在的位置, 您可以用来源代码表 。

<span id="source-the-overlay"></span>

### 6 源代码叠加

在获取覆盖物之前,您必须打开一个新的终端,与您建造工作空间的终端分离。在您建造的同一终端或者同样建造一个覆盖物源的终端中,测试一个覆盖物可能会产生复杂的问题。

在新的终端中, 将您的主要ROS 2 环境作为“ 内置” 源头, 这样您就可以在“ 顶端” 上构建 :

##### Linux

``` console
$ source /opt/ros/rolling/setup.bash
```

##### macOS

``` console
$ . ~/ros2_install/ros2-osx/setup.bash
```

##### Windows

在这种情况下,您可以使用一个普通的命令提示,因为我们不会在这个终端中构建任何工作空间.

``` console
$ call C:\dev\ros2\local_setup.bat
```

进入工作空间的根 :

##### Linux

``` console
$ cd ~/ros2_ws
```

##### macOS

``` console
$ cd ~/ros2_ws
```

##### Windows

``` console
$ cd \ros2_ws
```

在根中, 源代码您的覆盖 :

##### Linux

``` console
$ source install/local_setup.bash
```

##### macOS

``` console
$ . install/local_setup.bash
```

##### Windows

``` console
$ call install\setup.bat
```

> **说明**
>
> 测试 `local_setup` 中,将只将上覆中可用的软件包添加到您的环境中。 `setup` 源代码中创建的上线和底线,允许您使用两个工作空间。
>
> 所以,寻找你的主要ROS 2设备 `setup` 接着是 `ros2_ws` 叠加 `local_setup`就像你刚刚做的一样 和寻找 `ros2_ws`’s `setup`因为它包括了它底部的环境。

现在你可以运行 `turtlesim` 复选框中的软件包 :

``` console
$ ros2 run turtlesim turtlesim_node
```

但是,你怎么能知道这是覆盖龟兹的运行,而不是你的主要装置的龟兹?

让我们在上下文中修改乌龟图案,以便你能看到效果:

- 您可以在覆盖符中与覆盖符分开修改和重建套件.

- 上盖先于下盖.

<span id="modify-the-overlay"></span>

### 7 修改覆盖

你可以修改 `turtlesim` 通过编辑龟兹窗口的标题栏,在您的上覆中。要做到这一点,请找到 `turtle_frame.cpp` 文件输入 `~/ros2_ws/src/ros_tutorials/turtlesim/src`打开 `turtle_frame.cpp` 与您首选的文本编辑器。

查找函数 `setWindowTitle("TurtleSim");`,修改值 `"TurtleSim"` 改为: `"MyTurtleSim"`,并保存文件。

回到你运行的第一个终点站 `colcon build` 早点再运行一次

返回第二航站楼(覆盖源头所在),再运行龟兹姆:

``` console
$ ros2 run turtlesim turtlesim_node
```

你们可以看到龟兹窗上的标题栏上写着“MyTurtleSim”。

![](images/overlay.png)

尽管你的主要ROS 2环境 来源于这个终端较早, `ros2_ws` 环境优先于主干的内容。

要看到你的底盘仍然完好无损, 请打开一个全新的终端, 只提供您的 ROS 2 安装 。 运行龟兹 :

``` console
$ ros2 run turtlesim turtlesim_node
```

![](images/underlay.png)

你可以看到,对顶部的修改实际上并没有影响底部中的任何部分.

<span id="summary"></span>

## 小结

在此教程中, 您将您的主要 ROS 2 脱机安装为您的底盘, 并在新的工作空间中通过克隆和构建套件创建了覆盘。 覆盘会预设路径, 并且比底盘优先, 正如您用修改过的图案所看到的 。

因此你不必把所有东西都放在同一工作空间上, 重建一个巨大的工作空间。

<span id="next-steps"></span>

## 后续步骤

既然你了解了创建、建造和寻找自己工作空间背后的细节,你就可以学会如何 [创建自己的软件包](../Creating-Your-First-ROS2-Package.md).
