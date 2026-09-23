---
translation_status: machine_translated
source: Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-colcon-to-build-packages"></span> <span id="colcon"></span>

# 使用( E) `colcon` 创建软件包

**目标：** 构建 ROS 2 工作空间 `colcon`.

**教程级别：** 入门

**用时：** 20分钟

这是一个关于如何创建和构建 ROS 2 工作空间的简要教程 `colcon`它是一种实用的教程,不是用来取代核心文件的。

<span id="background"></span>

## 背景

`colcon` 是ROS构建工具上的迭代 `catkin_make`, `catkin_make_isolated`, `catkin_tools` 财务报告和财务报告 `ament_tools`。关于设计科隆的更多信息,请参见 [本文](https://design.ros2.org/articles/build_tool.html).

源代码可见于 [Colcon GitHub 组织](https://github.com/colcon).

<span id="prerequisites"></span>

## 前提条件

<span id="install-colcon"></span>

### 安装 colcon

##### Ubuntu

``` console
$ sudo apt install python3-colcon-common-extensions
```

##### RHEL

``` console
$ sudo dnf install python3-colcon-common-extensions
```

##### macOS

``` console
$ python3 -m pip install colcon-common-extensions
```

##### Windows

``` console
$ pip install -U colcon-common-extensions
```

<span id="install-ros-2"></span>

### 安装 ROS 2

要建立样本,需要安装ROS 2.

跟着 [安装指令](../../Installation.md).

> **注意**
>
> 如果从 deb 包安装, 此教程需要 [桌面安装](../../Installation/Ubuntu-Install-Debs.md#linux-install-debs-install-ros-2-packages).

<span id="basics"></span>

## 基本情况

ROS 工作空间是一个具有特定结构的目录。 通常有一个 `src` 子目录。 子目录内部是ROS 软件包的源代码所在。 通常, 目录会从空开始 。

colcon 执行外源构建。 默认情况下, 它会创建以下目录作为同级元素 `src` 目录 :

- 那个... `build` 目录将是存储中间文件的地方。对于每个软件包,将创建一个子文件夹,例如,正在引用CMake。

- 那个... `install` 目录是每个软件包将被安装到的地方。默认情况下,每个软件包将被安装在一个单独的子目录中。

- 那个... `log` 目录中包含各种关于每个相机引用的日志信息。

> **说明**
>
> 和猫皮相比,没有 `devel` 目录。

<span id="create-a-workspace"></span>

### 创建工作空间

第一,创建目录( E)`ros2_ws`控制我们的工作空间:

##### Linux

``` console
$ mkdir -p ~/ros2_ws/src
$ cd ~/ros2_ws
```

##### macOS

``` console
$ mkdir -p ~/ros2_ws/src
$ cd ~/ros2_ws
```

##### Windows

``` console
$ md \dev\ros2_ws\src
$ cd \dev\ros2_ws
```

此时工作空间包含一个单独的空目录 `src`:

``` bash
.
└── src

1 directory, 0 files
```

<span id="add-some-sources"></span>

### 添加一些来源

让我们复制 [实例](https://github.com/ros2/examples) 存储到 `src` 工作空间目录 :

``` console
$ git clone https://github.com/ros2/examples src/examples -b rolling
```

现在工作区应该有ROS 2的源代码:

``` bash
.
└── src
    └── examples
        ├── CONTRIBUTING.md
        ├── LICENSE
        ├── rclcpp
        ├── rclpy
        └── README.md

4 directories, 3 files
```

<span id="source-an-underlay"></span>

### 来源为底线

重要的是,我们已经为现有的ROS 2 安装提供了环境,这将为我们的工作空间提供实例包所需的构建依赖。这是通过提供由二进制安装或源安装提供的设置脚本实现的,即另一个colcon工作空间(参见 [安装](../../Installation.md)我们称这环境为 **支线**.

我们的工作空间, `ros2_ws`,将是一个 **覆盖** 在现有的ROS 2安装之上。一般来说,建议在计划安装少量软件包时使用一个覆盖符,而不是将所有软件包放入同一个工作空间。

<span id="build-the-workspace"></span>

### 构建工作空间

> **注意**
>
> 要在 Windows 上构建软件包, 您需要在一个 Visual Studio 环境中, 查看 [构建 ROS 2 代码](../../Installation/Alternatives/Windows-Development-Setup.md#windows-dev-build-ros2) 更多细节。

在工作空间的根部,运行 `colcon build`。由于建筑类型,例如 `ament_cmake` 并不支持 `devel` 空格并需要安装软件包, colcon 支持选项 `--symlink-install`。这允许通过修改已安装的文件来更改已安装的文件。 `source` 空间(例如Python文件或其他非编译资源),用于更快的迭代。

##### Linux

``` console
$ colcon build --symlink-install
```

##### macOS

``` console
$ colcon build --symlink-install
```

##### Windows

``` console
$ colcon build --merge-install
```

Windows 不允许长路径, 所以 `merge-install` 将所有路径结合到 `install` 目录。在Windows上,创建符号链接需要特殊的权限,所以 `--symlink-install` 。要使用它,就需要在系统设置中以管理员身份运行命令或启用开发者模式。

> **提示**
>
> 运行 `colcon build` 可能冻结CPU、RAM和I/O有限系统的屏幕和鼠标(例如Raspberry Pi),因此使用该程序可能是有益的。 `--executor sequential` 参数来逐个构建软件包,而不是使用并行性。请参见 [折叠文档](https://colcon.readthedocs.io/en/released/reference/executor-arguments.html) 需要更多的论据。

建筑完工后,我们再看看 `build`, `install`,以及 `log` 目录 :

``` bash
.
├── build
├── install
├── log
└── src

4 directories, 0 files
```

<span id="run-tests"></span> <span id="colcon-run-the-tests"></span>

### 运行测试

为了测试我们刚刚建造的软件包, 运行如下:

##### Linux

``` console
$ colcon test
```

##### macOS

``` console
$ colcon test
```

##### Windows

记住要用一个 `x64 Native Tools Command Prompt for VS 2019` 用于执行以下命令,因为我们将要建立一个工作空间。

``` console
$ colcon test --merge-install
```

您还需要指定 `--merge-install` 从我们用它来建造上层建筑起

<span id="source-the-environment"></span> <span id="colcon-tutorial-source-the-environment"></span>

### 环境来源

Colcon 成功建成后, 输出将会在 `install` 目录。在您使用任何已安装的可执行文件或库之前,您需要将它们添加到您的路径和库路径中。 colcon 将生成 bash/bat 文件于 `install` 帮助设置环境的目录 。 这些文件将把所有所需的元素添加到您的路径和库路径中, 并提供由软件包导出的任何shash或 shell命令 。

##### Linux

``` console
$ source install/setup.bash
```

##### macOS

``` console
$ . install/setup.bash
```

##### Windows

在 Windows 命令行界面中 :

``` console
$ call install\setup.bat
```

或与宝珠曰:

``` console
$ install\setup.ps1
```

<span id="try-a-demo"></span>

### 尝试演示

使用环境源头,我们可以运行colcon构建的可执行文件。让我们从实例中运行一个用户节点:

``` console
$ ros2 run examples_rclcpp_minimal_subscriber subscriber_member_function
```

在另一个终端,让我们运行一个出版商节点(不要忘记源码设置脚本):

``` console
$ ros2 run examples_rclcpp_minimal_publisher publisher_member_function
```

您应该看到来自出版商和订阅者的信息,并加码。

<span id="create-your-own-package"></span>

## 创建自己的软件包

Colcon 使用该 `package.xml` 定义的规格 [REP 149号文件](https://reps.openrobotics.org/rep-0149/) ([格式 2](https://reps.openrobotics.org/rep-0140/) 也得到支持)。

colcon 支持多个构建类型。 推荐的构建类型为 `ament_cmake` 财务报告和财务报告 `ament_python`。还支持纯度 `cmake` 软件包。

一个实例: `ament_python` 建设是 [ament_index_python 软件包](https://github.com/ament/ament_index/tree/rolling/ament_index_python) ,其中设置.py是建筑的主要切入点.

类似软件包 [demo_nodes_cpp](https://github.com/ros2/demos/tree/rolling/demo_nodes_cpp) 使用该 `ament_cmake` 构建类型,并使用 CMake 作为构建工具。

为了方便,你可以使用工具 `ros2 pkg create` 基于模板创建新软件包。关于创建软件包和如何使用软件包的全部描述 `ros2 pkg create` 正在即将到来的教程中 [创建软件包](Creating-Your-First-ROS2-Package.md).

> **说明**
>
> 为: `catkin` 用户,此值相当于 `catkin_create_package`.

<span id="setup-colcon-cd"></span>

## 设置 `colcon_cd`

命令 `colcon_cd` 允许您将当前 shell 的工作目录快速更改为软件包目录。例如, `colcon_cd some_ros_package` 会很快把你带到目录中 `~/ros2_ws/src/some_ros_package`. 设置 `colcon_cd` 您需要运行以下命令来修改您的 shell 启动脚本 :

##### Linux

``` console
$ echo "source /usr/share/colcon_cd/function/colcon_cd.sh" >> ~/.bashrc
$ echo "export _colcon_cd_root=/opt/ros/rolling/" >> ~/.bashrc
```

##### macOS

``` console
$ echo "source /usr/local/share/colcon_cd/function/colcon_cd.sh" >> ~/.bashrc
$ echo "export _colcon_cd_root=~/ros2_install" >> ~/.bashrc
```

##### Windows

尚未提供

取决于你安装的方式 `colcon_cd` 且您的工作空间所在处,上述指示可能有所不同,请参见: [文件](https://colcon.readthedocs.io/en/released/user/installation.html#quick-directory-changes) 要在 Linux 和 macOS 中撤销此选项,请找到您的系统 shell 启动脚本并删除所附的源和导出命令 。

<span id="setup-colcon-tab-completion"></span>

## 设置 `colcon` 选项卡补全

那个... `colcon` 命令支持针对 bash 和 shash 类 shells 的命令补全。 `colcon-argcomplete` 软件包必须安装,并且 [可能需要一些设置](https://colcon.readthedocs.io/en/released/user/installation.html#enable-completion) 让它发挥作用。

<span id="tips"></span>

## 提示

- 如果您不想构建一个特定软件包, 请放置一个名为空文件 `COLCON_IGNORE` 在目录中,它将不会被索引。

- 如果您想要避免在 CMake 包中配置和构建测试, 您可以通过 : `--cmake-args -DBUILD_TESTING=0`.

- 如果您想要从一个软件包中运行一个特定的测试 :

  ``` console
  $ colcon test --packages-select YOUR_PKG_NAME --ctest-args -R YOUR_TEST_IN_PKG
  ```

<span id="setup-colcon-mixins"></span>

## 设置 `colcon` 混音器

各种命令行选项是写作的乏味和/或难以记住的.

例如,要更改 CMake 构建类型以调试,通常会使用 :

``` console
$ colcon build --cmake-args -DCMAKE_BUILD_TYPE=Debug
```

为使通用命令行选项更容易引用此寄存器,这些“短线”是可以使用的。

要安装默认的 colcon 混音器, 请运行以下操作 :

``` console
$ colcon mixin add default https://raw.githubusercontent.com/colcon/colcon-mixin-repository/master/index.yaml
$ colcon mixin update default
```

那就试试看 `debug` 混合 :

``` console
$ colcon build --mixin debug
```

详情请参见: [Colcon 混音库](https://github.com/colcon/colcon-mixin-repository).
