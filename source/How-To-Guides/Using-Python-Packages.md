<span id="using-python-packages-with-ros-2"></span>
<span id="pythonpackages"></span>
# 在 ROS 2 中使用 Python 软件包

**目标：** 介绍如何在 ROS 2 生态中与其他 Python 软件包配合使用。

!!! note "说明"
    如果打算使用预先打包的二进制文件（`deb` 文件或二进制归档发行版），Python 解释器必须与构建这些二进制文件时使用的解释器一致。如果使用 `virtualenv` 或 `pipenv`，请确保采用系统解释器。如果使用 `conda`，其解释器很可能与系统解释器不同，从而与 ROS 2 二进制文件不兼容。

<span id="installing-via-rosdep"></span>
## 通过 rosdep 安装

引入第三方 Python 软件包最快的方法，是使用它们对应的 rosdep 键（如果已有）。可以在以下文件中查找 `rosdep` 键：

- <https://github.com/ros/rosdistro/blob/master/rosdep/base.yaml>
- <https://github.com/ros/rosdistro/blob/master/rosdep/python.yaml>

将这些键加入 `package.xml`，即可向构建系统声明你的软件包及依赖它的软件包需要这些依赖项。在新工作空间中，也可以快速安装所有 rosdep 键对应的依赖：

```console
$ rosdep install -yr --from-paths ./path/to/your/workspace
```

如果所需软件包还没有对应的 `rosdep` 键，可以按照 [rosdep 键贡献指南](http://docs.ros.org/en/independent/api/rosdep/html/contributing_rules.html)添加。

有关 `rosdep` 工具及其工作原理的更多信息，请参阅 [rosdep 文档](http://docs.ros.org/en/independent/api/rosdep/html/)。

<span id="installing-via-a-package-manager"></span>
## 通过包管理器安装

如果不想创建 rosdep 键，但系统包管理器（如 `apt`）提供了该软件包，也可以直接安装使用：

```console
$ sudo apt install python3-serial
```

如果软件包位于 [Python Package Index（PyPI）](https://pypi.org/)，并且希望在系统中全局安装：

```console
$ python3 -m pip install -U pyserial
```

如果软件包位于 PyPI，且只想为当前用户安装：

```console
$ python3 -m pip install -U --user pyserial
```

<span id="installing-via-a-virtual-environment"></span>
## 通过虚拟环境安装

首先创建一个 Colcon 工作空间：

```console
$ mkdir -p ~/colcon_venv/src
$ cd ~/colcon_venv/
```

然后设置虚拟环境：

```console
$ virtualenv -p python3 ./venv # Make a virtual env and activate it
$ source ./venv/bin/activate
$ touch ./venv/COLCON_IGNORE # Make sure that colcon does not try to build the venv
```

接着，在虚拟环境中安装需要的 Python 软件包：

```console
$ python3 -m pip install gtsam pyserial… etc
```

现在可以构建工作空间，并运行依赖这些虚拟环境软件包的 Python 节点。

```console
$ source /opt/ros/rolling/setup.bash # Source {DISTRO_TITLE} and build
$ colcon build
```

!!! note "说明"
    如果希望通过 Bloom 发布软件包，应将所需的依赖包加入 `rosdep`。参见 [rosdep 键贡献指南](http://docs.ros.org/en/independent/api/rosdep/html/contributing_rules.html)。
