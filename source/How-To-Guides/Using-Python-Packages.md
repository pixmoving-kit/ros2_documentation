---
translation_status: machine_translated
source: How-To-Guides/Using-Python-Packages.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-python-packages-with-ros-2"></span> <span id="pythonpackages"></span>

# 在 ROS 2 中使用 Python 软件包

**目标：** 解释如何与ROS 2生态系统中的其他Python包进行互操作.

> **说明**
>
> 警告说明,如果您打算使用预包装的二进制( 或 `deb` python 解释器必须匹配用于构建原始二进制的文件。如果您打算使用类似的东西 。 `virtualenv` 或 时 间 `pipenv`中,确保使用系统解释器。如果您使用类似 `conda`,极有可能解释器不会与系统解释器匹配,并且会与ROS 2二进制不兼容.

<span id="installing-via-rosdep"></span>

## 正在通过 `rosdep`

包括第三方python包的最快方式是使用相应的rosdep密钥,如果有的话. `rosdep` 可以通过下列方式检查密钥:

- <https://github.com/ros/rosdistro/blob/master/rosdep/base.yaml>

- <https://github.com/ros/rosdistro/blob/master/rosdep/python.yaml>

这些 `rosdep` 键可以添加到您的 `package.xml` 文件,它向构建系统显示您的软件包(和依赖的软件包)依赖于这些密钥。在新的工作空间中,您也可以快速安装所有 rosdep 密钥,其中:

``` console
$ rosdep install -yr --from-paths ./path/to/your/workspace
```

如果目前没有的话 `rosdep` 您感兴趣的软件包的密钥,可以通过跟随 [罗斯德密钥贡献指南](http://docs.ros.org/en/independent/api/rosdep/html/contributing_rules.html).

来了解更多关于 `rosdep` 工具及其运作方式 [罗斯德文档](http://docs.ros.org/en/independent/api/rosdep/html/).

<span id="installing-via-a-package-manager"></span>

## 通过软件包管理器安装

如果您不想制作 rosdep 密钥, 但软件包可以在您的系统包管理器中使用( 例如 : `apt`),可以这样安装和使用软件包:

``` console
$ sudo apt install python3-serial
```

如果软件包可用于 [Python 软件包索引( PyPI)](https://pypi.org/) 您想要在全球安装您的系统 :

``` console
$ python3 -m pip install -U pyserial
```

如果 PyPI 上可以使用软件包, 您想要在本地安装给您的用户 :

``` console
$ python3 -m pip install -U --user pyserial
```

<span id="installing-via-a-virtual-environment"></span>

## 通过虚拟环境安装

首先,创建 Colcon 工作空间 :

``` console
$ mkdir -p ~/colcon_venv/src
$ cd ~/colcon_venv/
```

然后设置虚拟环境 :

``` console
$ virtualenv -p python3 ./venv # Make a virtual env and activate it
$ source ./venv/bin/activate
$ touch ./venv/COLCON_IGNORE # Make sure that colcon does not try to build the venv
```

接下来,在虚拟环境中安装您想要的 Python 软件包 :

``` console
$ python3 -m pip install gtsam pyserial… etc
```

现在可以构建工作空间,运行取决于安装在虚拟环境中的软件包的蟒蛇节点.

``` console
$ source /opt/ros/rolling/setup.bash # Source Rolling and build
$ colcon build
```

> **说明**
>
> 如果您想要使用 Bloom 发布您的软件包, 您应该添加您需要的软件包 。 `rosdep`,参见 [罗斯德密钥贡献指南](http://docs.ros.org/en/independent/api/rosdep/html/contributing_rules.html).
