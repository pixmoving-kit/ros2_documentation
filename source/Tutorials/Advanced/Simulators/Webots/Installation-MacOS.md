---
translation_status: machine_translated
source: Tutorials/Advanced/Simulators/Webots/Installation-MacOS.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="installation-macos"></span>

# 安装（macOS）

**目标：** 安装 `webots_ros2` 软件包和运行macOS上的模拟实例。

**教程级别：** 高级

**用时：** 10分钟

<span id="background"></span>

## 背景

那个... `webots_ros2` 软件包提供了ROS 2 和 Webots 之间的接口。它包括几个子软件包,包括 `webots_ros2_driver`,它允许您启动 Webots 并与之通信。其他子软件包主要是使用界面显示多种可能执行的例子。在此教程中,您要安装软件包并学习如何运行其中的一个例子。

<span id="prerequisites"></span>

## 前提条件

建议理解初学者所包括的基本ROS原则。 [教程](../../../../Tutorials.md)特别是, [创建工作空间](../../../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md) 财务报告和财务报告 [创建软件包](../../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md) 是有用的先决条件。

有必要将 Webots 本地安装在 mac 上,以便使用 `webots_ros2` 软件包。您可以在虚拟机中遵循以下解释。 [安装程序](https://cyberbotics.com/doc/guide/installation-procedure) 或 时 间 [从源头构建它](https://github.com/cyberbotics/webots/wiki/macOS-installation/).

<span id="tasks"></span>

## 操作步骤

在macOS上,基于UTM虚拟机的解决方案提供了与本地的macOS安装相比ROS 2的改进用户体验,因为它在Linux环境中运行ROS,然而,Webots应该本土安装在macOS上,并且能够与虚拟机运行的ROS节点(VM)进行通信. 这个解决方案允许本地的3D硬件加速Webots. VM运行所有ROS部分(包括RViz),并通过TCP连接到主机开始Webots. 一个共享文件夹允许脚本将世界和其它资源文件从VM传输到Webots运行的macOS.

以下步骤解释了如何通过安装 VM 图像来创建 VM 图像 。 `webots_ros2` 。也可以从源头安装。在 [预配置图像](#preconfigured-images) 区域中,您可以找到Webots(从R2023a开始)每次发布下载时已经配置的图像。

<span id="create-the-vm-image"></span>

### 1 创建 VM 图像

在您的 macOS 机器上安装 UTM。 链接可以在 [UTM官方网站 (中文(简体) ).](https://mac.getutm.app/).

下载 `.iso` 图像 [乌邦图22.04](https://cdimage.ubuntu.com/jammy/daily-live/current/) 给Humble和Rolling的,或者 [乌邦图 20.04](https://cdimage.ubuntu.com/focal/daily-live/pending/) 为 Foxy 服务。请确定下载与您的 CPU 架构相对应的图像。

在UTM软件中:

- 创建新图像并选择 `Virtualize` 选项。

- 选择您已下载的 ISO 图像 `Boot ISO Image` 字段键入。

- 默认时保留所有硬件设置(包括硬件加速被禁用).

- 在那个 `Shared Directory` 窗口中,选择要使用的文件夹。 `webots_ros2` 以将所有 Webots 资产转移到主机。在此示例中,选中的文件夹是 `/Users/username/shared`.

- 将所有剩余参数作为默认值。

- 启动 VM 。 注意, 每次启动 VM 时您可以选择另一个共享文件夹 。

- 在首次启动 VM 时, 安装 Ubuntu 并选择您的账户的用户名。 在此示例中, 用户名是 `ubuntu`.

- Ubuntu安装后,关闭VM,从CD/DVD字段中移除异构图像,重启VM.

<span id="configure-the-vm"></span>

### 2 配置 VM

本节中,ROS 2安装在VM中,并配置了共享文件夹,以下指令和命令全部运行在VM内部.

- 在开始的 VM 中打开一个终端, 并安装 ROS 2 分配器, 您需要遵循其中的指令 [Ubuntu（deb 软件包）](../../../../Installation/Ubuntu-Install-Debs.md):

- 在 VM 中创建一个文件夹用作共享文件夹。 在此示例中, VM 中的共享文件夹是 `/home/ubuntu/shared`.

  ``` console
  $ mkdir /home/ubuntu/shared
  ```

- 要将此文件夹挂载到主机, 请执行以下命令 。 如果您的情况不同, 请不要忘记修改共享文件夹的路径 。

  ``` console
  $ sudo mount -t 9p -o trans=virtio share /home/ubuntu/shared -oversion=9p2000.L
  ```

- 要在启动 VM 时将此文件夹自动挂载到主机, 请添加以下一行到 `/etc/fstab`不要忘记修改共享文件夹的路径, 如果您的情况不同 。

  ``` console
  share     /home/ubuntu/shared     9p      trans=virtio,version=9p2000.L,rw,_netdev,nofail 0       0
  ```

- 环境变量 `WEBOTS_SHARED_FOLDER` 必须始终设定软件包才能在 VM 中正常工作。此变量指定用于在主机和虚拟机( VM)之间交换数据的共享文件夹的位置 。 `webots_ros2` 软件包。用于此变量的值应当为: `<host shared folder>:<VM shared folder>`时, `<host shared folder>` 是主机上共享文件夹的路径,并且 `<VM shared folder>` 是 VM 上同一共享文件夹的路径。

  举例来说:

  ``` console
  $ export WEBOTS_SHARED_FOLDER=/Users/username/shared:/home/ubuntu/shared
  ```

  您可以将此命令行添加到 `~/.bashrc` 在启用新终端时自动设置此环境变量。

<span id="install-webots-ros2"></span>

### 3 安装 `webots_ros2`

您可以安装 `webots_ros2` 从官方发布的软件包中安装,或从最新来源安装。 [吉图布](https://github.com/cyberbotics/webots_ros2).

##### 安装 Webots\_ ros2 分布式软件包

在 VM 终端中运行以下命令 。

``` console
$ sudo apt-get install ros-rolling-webots-ros2
```

##### 从源头安装webots_ros2

安装 git 。

``` console
$ sudo apt-get install git
```

创建 ROS 2 工作空间 `src` 目录。

``` console
$ mkdir -p ~/ros2_ws/src
```

来源 ROS 2 环境.

``` console
$ source /opt/ros/rolling/setup.bash
```

从Github那里获取情报

``` console
$ cd ~/ros2_ws
$ git clone --recurse-submodules https://github.com/cyberbotics/webots_ros2.git src/webots_ros2
```

安装软件包的依赖性 。

``` console
$ sudo apt install python3-pip python3-rosdep python3-colcon-common-extensions
$ sudo rosdep init && rosdep update
$ rosdep install --from-paths src --ignore-src --rosdistro rolling
```

使用 `colcon`.

``` console
$ colcon build
```

源此工作空间 。

``` console
$ source install/local_setup.bash
```

<span id="launch-the-webots-ros2-universal-robot-example"></span>

### 发射 `webots_ros2_universal_robot` 实例

如前几节所述,软件包使用共享文件夹从 VM 与主机Webots 通信。为了使 Webots 从 VM 的 ROS 软件包开始在主机上运行,必须运行一个本地的 TCP 模拟服务器 。

服务器可在此下载 : [local_simulation_server.py](https://github.com/cyberbotics/webots-server/blob/main/local_simulation_server.py)中指定 Webots 安装文件夹。 `WEBOTS_HOME` 环境变量(例如: `/Applications/Webots.app`),并使用下列命令运行服务器在主机上的新终端中运行(不在VM中) :

``` console
$ export WEBOTS_HOME=/Applications/Webots.app
$ python3 local_simulation_server.py
```

在 VM 中,打开一个终端并执行以下命令以启动一个软件包:

第一个源 ROS 2 环境,如果还没有完成的话。

``` console
$ source /opt/ros/rolling/setup.bash
```

如果安装了源头, 请源代码为 ROS 2 工作空间, 如果还没有完成的话 。

``` console
$ cd ~/ros2_ws
$ source install/local_setup.bash
```

如果尚未设置在内 `~/.bashrc`设置 `WEBOTS_SHARED_FOLDER` (详情见前几节)请确定根据您各自目录的位置更改路径。

``` console
$ export WEBOTS_SHARED_FOLDER=/Users/username/shared:/home/ubuntu/shared
```

使用ROS 2发射命令开始演示包(例如. `webots_ros2_universal_robot`).

``` console
$ ros2 launch webots_ros2_universal_robot multirobot_launch.py
```

如果Webots关闭或ROS 2进程中断,本地服务器将自动等待新软件包的启动,共享文件夹将被清理用于下一次运行.

<span id="pre-configured-images"></span> <span id="preconfigured-images"></span>

## 预配置图像

如果您不想从零开始设置 VM, 以下链接会为您提供Webots 的每个版本预配置的 UTM 图像 。 `webots_ros2` 版本是从官方寄存器中安装的(不是来源),通常是第一个与对应的Webots版本兼容的版本。欢迎您下载一个图像并升级软件包,或者在必要时从来源安装。

- [Webots R2023a版本 2023.0.2](https://cyberbotics.com/files/ros2/webots_ros2_2023_0_2.utm.zip) \[6.6 GB\]

- [Webots R2023b的2023.1版本](https://cyberbotics.com/files/ros2/webots_ros2_2023_1_1.utm.zip) \[8.0GB\] (英语).

在将下载的图像添加到UTM软件时,在下拉菜单(如. `/Users/username/shared`。一旦开始核查机制, `WEBOTS_SHARED_FOLDER` 环境变量必须总是被设定,以使软件包在虚拟机(VM)中正常工作。 `webots_ros2` 包装用于在主机和 VM 之间交换数据的共享文件夹的位置。该变量的值应当以下列格式显示: `<host shared folder>:<VM shared folder>`时, `<host shared folder>` 是主机上共享文件夹的路径,并且 `<VM shared folder>` 是 VM 上同一共享文件夹的路径。

在预配置的图像中, `WEBOTS_SHARED_FOLDER` 已经设置在 `~/.bashrc`。您需要更新,以便使用正确的主机文件夹路径 :

``` console
export WEBOTS_SHARED_FOLDER=/Users/username/shared:/home/ubuntu/shared
```
