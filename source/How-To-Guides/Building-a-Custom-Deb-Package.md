---
translation_status: machine_translated
source: How-To-Guides/Building-a-Custom-Deb-Package.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="building-a-custom-deb-package"></span>

# 构建自定义 deb 软件包

许多Ubuntu用户通过安装在系统中安装ROS 2 [deb 软件包](../Installation/Ubuntu-Install-Debs.md),本指南给出了构建本地,定制的deb软件包的一套短指令.

<span id="prerequisites"></span>

## 前提条件

要成功构建自定义软件包,所要构建的软件包的所有依赖性都必须在当地或罗斯德普中提供。此外,软件包的所有依赖性都应该在 `package.xml` 软件包的文件。

<span id="install-dependencies"></span>

## 安装依赖关系

运行以下命令以安装构建所需的公用设施 :

``` console
$ sudo apt install python3-bloom python3-rosdep fakeroot debhelper dh-python
```

<span id="initialize-rosdep"></span>

## 初始化 rosdep

初始化 rosdep 数据库,使用调用 :

``` console
$ sudo rosdep init
$ rosdep update
```

请注意, `rosdep init` 命令如果在过去已经初始化,则可能失败;这可以安全地忽略。

<span id="build-the-deb-from-the-package"></span>

## 从软件包中构建 DEB

运行以下命令来构建 dib :

``` console
$ cd /path/to/pkg_source  # this should be the directory that contains the package.xml
$ bloom-generate rosdebian
$ fakeroot debian/rules binary
```

假设所有所需的依赖性都可用,并且汇编成功,新软件包将会在本目录的母目录中提供.
