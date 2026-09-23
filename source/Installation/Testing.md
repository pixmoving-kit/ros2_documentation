---
translation_status: machine_translated
source: Installation/Testing.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="testing-with-pre-release-binaries"></span>

# 使用预发布二进制包测试

许多ROS软件包是作为预建的二进制提供的。通常,您在跟踪时会获得已发布的二进制版本 [安装](../Installation.md)。还有预释的二进制版本,在正式发布前可用于测试。如果您想要尝试预释的 ROS 二进制版本,此文章描述了多个选项 。

当包被放入ROS分布(使用开花)时,建材农场将包建成Deb包,暂时储存在 **大楼** apt寄存器。随着依赖软件包的重建,一个自动进程定期同步软件包 **大楼** 转到一个二级仓库 **横向测试**. **横向测试** 目的是在软件包被手动同步到公共的 ros 仓库之前,让开发者和血缘用户进行额外测试。

大约每隔两周, rodistro的发布管理器 手动同步的内容 **横向测试** 输入 **主体** ROS仓库.

<span id="deb-testing-repository"></span>

## deb 测试存储器

对于基于Debian的操作系统,您可以从中安装二进制软件包 **横向测试** 存储器。

1.  请确保您在 DEB 包中安装工作 ROS 2 (参见 [安装](../Installation.md)).

2.  安装 ros2- testing- apt- source 软件包。 这将自动解开 ros2- apt- source 软件包, 因为每次只能启用一个寄存器 。

    ``` console
    $ sudo apt install -y ros2-testing-apt-source
    ```

3.  更新合适的索引 :

    ``` console
    $ sudo apt update
    ```

4.  您现在可以从测试仓库安装单个软件包, 例如 :

    ``` console
    $ sudo apt install ros-rolling-my-just-released-package
    ```

5.  或者,您可以将整个ROS 2 安装移动到测试仓库 :

    ``` console
    $ sudo apt dist-upgrade
    ```

6.  完成测试后,您可以通过重排 ros-apt-source 包来切换回普通寄存器 :

    ``` console
    $ sudo apt install -y ros2-apt-source
    ```

    并进行更新和升级:

    ``` console
    $ sudo apt update
    $ sudo apt dist-upgrade
    ```

<span id="rhel-testing-repository"></span>

## RHEL 测试仓库

对于 RHEL ,您可以从中安装二进制软件包 **横向测试** 寄存器,通过启用源配置的测试寄存器:

1.  确定您有工作 ROS 2 的 rpm 软件包安装( 请参看 [RHEL 安装指令](RHEL-Install-RPMs.md)).

2.  启用测试并禁用主仓库 :

    ``` console
    $ sudo dnf config-manager --set-enabled ros2-testing
    $ sudo dnf config-manager --set-disabled ros2
    ```

3.  更新 dnf 索引 :

    ``` console
    $ sudo dnf update
    ```

4.  您现在可以从测试仓库安装单个软件包, 例如 :

> ``` console
> $ sudo dnf install ros-rolling-my-just-released-package
> ```

5.  完成测试后,可以通过重新启用主寄存器来切换回普通寄存器:

> ``` console
> $ sudo dnf config-manager --set-disabled ros2-testing
> $ sudo dnf config-manager --set-enabled ros2
> ```
>
> 并进行更新和升级:
>
> ``` console
> $ sudo dnf update
> $ sudo dnf system-upgrade
> ```

<span id="binary-archives"></span> <span id="prerelease-binaries"></span>

## 二进制档案

对于核心软件包, 我们运行 Ubuntu Linux, RHEL, 和 Windows 的夜间包装任务。 这些包装任务会生成预建的二进制文件, 可以下载并提取到您的文件系统 。

1.  确定您已按照 [最新开发设置](Alternatives/Latest-Development-Setup.md) 为您的平台。

2.  转到 <https://ci.ros2.org/view/packaging/> ,然后从列表中选择一个与您的平台相对应的包装工作。

3.  在“最后成功的艺术”标题下,您应该看到一个下载链接(例如,Windows, `ros2-package-windows-AMD64.zip`).

4.  下载并提取归档到您的文件系统 。

5.  要使用二进制归档安装, 请来源于 `setup.*` 可在归档根中找到的文件。

    ##### Ubuntu Linux 和 RHEL 软件

    ``` console
    $ source path/to/extracted/archive/setup.bash
    ```

    ##### Windows

    ``` console
    $ call path\to\extracted\archive\setup.bat
    ```

<span id="docker"></span>

## Docker

对于Ubuntu Linux,也有一个基于夜二进制存档的夜道克图像.

1.  拖动 Docker 图像 :

    ``` console
    $ docker pull osrf/ros2:nightly
    ```

2.  启动交互式容器 :

    ``` console
    $ docker run -it osrf/ros2:nightly
    ```

对于在 Docker 运行 GUI 应用程序时的支持, 请查看教程 [用户界面与 Docker](https://wiki.ros.org/docker/Tutorials/GUI) 或该工具 [摇摆](https://github.com/osrf/rocker).
