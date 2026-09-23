<span id="building-a-custom-deb-package"></span>
# 构建自定义 deb 软件包

许多 Ubuntu 用户通过安装 [deb 软件包](../Installation/Ubuntu-Install-Debs.md) 在系统中安装 ROS 2。本指南简要介绍如何在本地构建自定义 deb 软件包。

<span id="prerequisites"></span>
## 前提条件

待构建软件包的所有依赖项必须能在本地或通过 rosdep 获得。此外，应在该软件包的 `package.xml` 中正确声明全部依赖项。

<span id="install-dependencies"></span>
## 安装依赖项

运行以下命令，安装构建所需的工具：

```console
$ sudo apt install python3-bloom python3-rosdep fakeroot debhelper dh-python
```

<span id="initialize-rosdep"></span>
## 初始化 rosdep

执行以下命令，初始化 rosdep 数据库：

```console
$ sudo rosdep init
$ rosdep update
```

如果之前已经初始化过，`rosdep init` 可能会失败；这种情况可以忽略。

<span id="build-the-deb-from-the-package"></span>
## 从软件包源码构建 deb 包

运行以下命令构建 deb 包：

```console
$ cd /path/to/pkg_source  # this should be the directory that contains the package.xml
$ bloom-generate rosdebian
$ fakeroot debian/rules binary
```

如果所有必需的依赖项均已具备，且编译成功，新生成的软件包将位于当前目录的父目录中。
