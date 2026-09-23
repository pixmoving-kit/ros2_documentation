---
translation_status: machine_translated
source: Tutorials/Intermediate/Launch/Launch-system.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="integrating-launch-files-into-ros-2-packages"></span>

# 将启动文件集成到 ROS 2 软件包

**目标：** 将发射文件添加到 ROS 2 软件包

**教程级别：** 中级

**用时：** 10分钟

<span id="prerequisites"></span>

## 前提条件

你本该去教书的 [创建 ROS 2 软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md).

与往常一样, [您打开的每个新终端](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md).

<span id="background"></span>

## 背景

在那个 [上一个教程](Creating-Launch-Files.md),我们看到了如何写一个独立的发射文件。这个教程将显示如何将发射文件添加到现有的软件包中,以及通常使用的常规。

<span id="tasks"></span>

## 操作步骤

<span id="create-a-package"></span>

### 1 创建软件包

创建工作空间供软件包在 :

##### Linux

``` console
$ mkdir -p launch_ws/src
$ cd launch_ws/src
```

##### macOS

``` console
$ mkdir -p launch_ws/src
$ cd launch_ws/src
```

##### Windows

``` console
$ md launch_ws\src
$ cd launch_ws\src
```

##### Python 软件包

``` console
$ ros2 pkg create --build-type ament_python --license Apache-2.0 py_launch_example
```

##### C++ 软件包

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 cpp_launch_example
```

<span id="creating-the-structure-to-hold-launch-files"></span>

### 2 创建保存发射文件的结构

根据惯例,一个软件包的所有发射文件都储存在 `launch` 软件包中的目录。确保创建 `launch` 在您在上面创建的软件包的顶层设置目录。

##### Python 软件包

对于 Python 软件包,包含您的软件包的目录应该像这样:

``` console
src/
  py_launch_example/
    launch/
    package.xml
    py_launch_example/
    resource/
    setup.cfg
    setup.py
    test/
```

为了让colcon找到和使用我们的发射文件,我们需要告知Python的安装工具是否存在。 `setup.py` 文件,请添加必要的内容 `import` 上方的语句,并将发射文件输入 `data_files` 参数 `setup`:

``` python
import os
from glob import glob
# Other imports ...

package_name = 'py_launch_example'

setup(
    # Other parameters ...
    data_files=[
        # ... Other data files
        # Include all launch files.
        (os.path.join('share', package_name, 'launch'), glob('launch/*'))
    ]
)
```

##### C++ 软件包

对于 C++ 软件包,我们将只调整 `CMakeLists.txt` 通过添加文件 :

``` cmake
# Install launch files.
install(DIRECTORY
  launch
  DESTINATION share/${PROJECT_NAME}/
)
```

到文件的结尾( 但在此之前) `ament_package()`).

<span id="writing-the-launch-file"></span>

### 3 写入发射文件

##### XML 发射文件

在你体内 `launch` 目录,创建名为 `my_script_launch.xml`. `_launch.xml` 作为 XML 发射文件的文件后缀。

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <node pkg="demo_nodes_cpp" exec="talker" name="talker"/>
</launch>
```

##### YAML 发射文件

在你体内 `launch` 目录,创建名为 `my_script_launch.yaml`. `_launch.yaml` 作为YAML发射文件的文件后缀。

``` yaml
%YAML 1.2
---
launch:
  - node:
      pkg: "demo_nodes_cpp"
      exec: "talker"
      name: "talker"
```

##### Python 发射文件

在你体内 `launch` 目录,创建名为 `my_script_launch.py`. `_launch.py` 由于 Python 发射文件的文件后缀,建议但不需要。然而,发射文件名称需要以 `launch.py` 待确认和自动完成 `ros2 launch`.

您的发射文件应该定义 `generate_launch_description()` 函数返回 a `launch.LaunchDescription()` 供《京都议定书》 `ros2 launch` 动词.

``` python
import launch
import launch_ros.actions


def generate_launch_description():
    return launch.LaunchDescription([
        launch_ros.actions.Node(
            package='demo_nodes_cpp',
            executable='talker',
            name='talker'),
    ])
```

<span id="building-and-running-the-launch-file"></span>

### 4 建立和运行发射文件

进入工作空间的顶层,并建造:

``` console
$ colcon build
```

之后 `colcon build` 已经成功, 您已经从工作空间中找到, 您应该能够运行发射文件如下 :

##### Python 软件包

##### XML 发射文件

``` console
$ ros2 launch py_launch_example my_script_launch.xml
```

##### YAML 发射文件

``` console
$ ros2 launch py_launch_example my_script_launch.yaml
```

##### Python 发射文件

``` console
$ ros2 launch py_launch_example my_script_launch.py
```

##### C++ 软件包

##### XML 发射文件

``` console
$ ros2 launch cpp_launch_example my_script_launch.xml
```

##### YAML 发射文件

``` console
$ ros2 launch cpp_launch_example my_script_launch.yaml
```

##### Python 发射文件

``` console
$ ros2 launch cpp_launch_example my_script_launch.py
```

<span id="documentation"></span>

## 文档

[发射文件](https://github.com/ros2/launch/blob/rolling/launch/doc/source/architecture.rst) 提供更详细的资料,说明在《公约》中也采用的概念。 `launch_ros`.

其他文件/发射能力实例即将提交。<https://github.com/ros2/launch> 财务报告和财务报告 <https://github.com/ros2/launch_ros>在此期间。
