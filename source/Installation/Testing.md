<span id="testing-with-pre-release-binaries"></span>

# 使用预发布二进制软件包进行测试

许多 ROS 软件包都提供预先构建好的二进制软件包。按照[安装指南](../Installation.md)操作时，通常会安装已正式发布的二进制版本。此外，也有预发布版本，可用于在正式发布之前进行测试。本文介绍几种试用 ROS 预发布二进制版本的方法。

使用 bloom 将软件包发布到某个 ROS 发行版后，构建农场会将它们构建为 deb 软件包，并临时存放在 **building** apt 软件源中。随着依赖这些软件包的其他软件包完成重新构建，自动化流程会定期将 **building** 中的软件包同步到名为 **ros-testing** 的第二个软件源。**ros-testing** 用于在正式发布前进行持续测试，开发者和希望体验最新软件的用户可以在这里进一步测试软件包，然后这些软件包才会手动同步到用户通常使用的公共 ROS 软件源。

大约每两周，rosdistro 的发布管理员会手动将 **ros-testing** 的内容同步到 **main** ROS 软件源。

<span id="deb-testing-repository"></span>

## deb 测试软件源

在基于 Debian 的操作系统上，可以从 **ros-testing** 软件源安装二进制软件包。

1. 确保已通过 deb 软件包安装了可正常使用的 ROS 2，参阅[安装指南](../Installation.md)。

2. 安装 ros2-testing-apt-source 软件包。由于一次只能启用一个软件源，这会自动卸载 ros2-apt-source 软件包。

    ```console
    $ sudo apt install -y ros2-testing-apt-source
    ```

3. 更新 apt 索引：

    ```console
    $ sudo apt update
    ```

4. 现在可以从测试软件源安装单个软件包，例如：

    ```console
    $ sudo apt install ros-rolling-my-just-released-package
    ```

5. 也可以将整个 ROS 2 安装切换为测试软件源中的版本：

    ```console
    $ sudo apt dist-upgrade
    ```

6. 测试完成后，可以重新安装 ros2-apt-source 软件包，切回常规软件源：

    ```console
    $ sudo apt install -y ros2-apt-source
    ```

    然后更新软件包索引并升级：

    ```console
    $ sudo apt update
    $ sudo apt dist-upgrade
    ```

<span id="rhel-testing-repository"></span>

## RHEL 测试软件源

在 RHEL 上，可以在软件源配置中启用测试软件源，从 **ros-testing** 安装二进制软件包。

1. 确保已通过 RPM 软件包安装了可正常使用的 ROS 2，参阅 [RHEL 安装说明](RHEL-Install-RPMs.md)。

2. 启用测试软件源，并禁用主软件源：

    ```console
    $ sudo dnf config-manager --set-enabled ros2-testing
    $ sudo dnf config-manager --set-disabled ros2
    ```

3. 更新 dnf 索引：

    ```console
    $ sudo dnf update
    ```

4. 现在可以从测试软件源安装单个软件包，例如：

    ```console
    $ sudo dnf install ros-rolling-my-just-released-package
    ```

5. 测试完成后，可以重新启用主软件源，切回常规软件源：

    ```console
    $ sudo dnf config-manager --set-disabled ros2-testing
    $ sudo dnf config-manager --set-enabled ros2
    ```

    然后更新并升级：

    ```console
    $ sudo dnf update
    $ sudo dnf system-upgrade
    ```

<span id="binary-archives"></span>
<span id="prerelease-binaries"></span>

## 二进制归档包

对于核心软件包，我们每晚都会在 Ubuntu Linux、RHEL 和 Windows 上运行打包任务。这些任务生成包含预构建二进制文件的归档包，可以下载并解压到文件系统中。

1. 按照对应平台的[最新开发版本安装说明](Alternatives/Latest-Development-Setup.md)，确保已安装全部依赖。
2. 访问 <https://ci.ros2.org/view/packaging/>，从列表中选择对应平台的打包任务。
3. 在“Last Successful Artifacts”标题下找到下载链接，例如 Windows 的 `ros2-package-windows-AMD64.zip`。
4. 下载归档包，并将其解压到文件系统中。
5. 加载归档包根目录中的 `setup.*` 文件，使用这个二进制安装环境。

    **Ubuntu Linux 和 RHEL：**

    ```console
    $ source path/to/extracted/archive/setup.bash
    ```

    **Windows：**

    ```console
    $ call path\to\extracted\archive\setup.bat
    ```

<span id="docker"></span>

## Docker

对于 Ubuntu Linux，还提供基于每晚生成的二进制归档包构建的 Docker 镜像。

1. 拉取 Docker 镜像：

    ```console
    $ docker pull osrf/ros2:nightly
    ```

2. 启动交互式容器：

    ```console
    $ docker run -it osrf/ros2:nightly
    ```

如果需要在 Docker 中运行图形界面应用，请参阅[在 Docker 中使用图形界面应用](https://wiki.ros.org/docker/Tutorials/GUI)教程或 [rocker 工具](https://github.com/osrf/rocker)。
