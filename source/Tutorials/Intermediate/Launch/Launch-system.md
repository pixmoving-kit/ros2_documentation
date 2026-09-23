<span id="integrating-launch-files-into-ros-2-packages"></span>

# 将启动文件集成到 ROS 2 软件包中

**目标：** 为 ROS 2 软件包添加启动文件。

**教程级别：** 中级

**预计耗时：** 10 分钟

<span id="prerequisites"></span>

## 前提条件

应先完成[创建 ROS 2 软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md)教程。

与往常一样，别忘记在[每个新打开的终端](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)中加载 ROS 2 环境。

<span id="background"></span>

## 背景

[上一篇教程](Creating-Launch-Files.md)介绍了如何编写独立的启动文件。本教程介绍如何将启动文件添加到现有软件包中，以及通常遵循的约定。

<span id="tasks"></span>

## 任务

<span id="create-a-package"></span>

### 1 创建软件包

为软件包创建工作空间。

Linux：

```console
$ mkdir -p launch_ws/src
$ cd launch_ws/src
```

macOS：

```console
$ mkdir -p launch_ws/src
$ cd launch_ws/src
```

Windows：

```console
$ md launch_ws\src
$ cd launch_ws\src
```

创建 Python 包：

```console
$ ros2 pkg create --build-type ament_python --license Apache-2.0 py_launch_example
```

或创建 C++ 包：

```console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 cpp_launch_example
```

<span id="creating-the-structure-to-hold-launch-files"></span>

### 2 创建存放启动文件的目录结构

按照惯例，软件包的所有启动文件都存放在包内的 `launch` 目录中。在刚创建的软件包顶层建立 `launch` 目录。

对于 Python 包，目录结构应为：

```console
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

要让 colcon 找到并使用启动文件，需要通知 Python 的安装工具。打开 `setup.py`，在顶部添加所需的 `import` 语句，并将启动文件加入 `setup` 的 `data_files` 参数：

```python
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

对于 C++ 包，只需在 `CMakeLists.txt` 末尾、`ament_package()` 之前添加：

```cmake
# Install launch files.
install(DIRECTORY
  launch
  DESTINATION share/${PROJECT_NAME}/
)
```

<span id="writing-the-launch-file"></span>

### 3 编写启动文件

**XML 启动文件：** 在 `launch` 目录创建 `my_script_launch.xml`，内容见 [XML 示例](launch/my_script_launch.xml)。推荐使用 `_launch.xml` 后缀，但并非强制要求。

**YAML 启动文件：** 在 `launch` 目录创建 `my_script_launch.yaml`，内容见 [YAML 示例](launch/my_script_launch.yaml)。推荐使用 `_launch.yaml` 后缀，但并非强制要求。

**Python 启动文件：** 在 `launch` 目录创建 `my_script_launch.py`，内容见 [Python 示例](launch/my_script_launch.py)。推荐使用 `_launch.py` 后缀，但并非强制要求。不过，为了让 `ros2 launch` 识别并自动补全文件名，名称必须以 `launch.py` 结尾。

Python 启动文件应定义 `generate_launch_description()` 函数，返回供 `ros2 launch` 子命令使用的 `launch.LaunchDescription()`。

<span id="building-and-running-the-launch-file"></span>

### 4 构建并运行启动文件

进入工作空间顶层目录并构建：

```console
$ colcon build
```

`colcon build` 成功后，加载工作空间环境，即可运行启动文件。

Python 包中的 XML、YAML、Python 启动文件分别使用：

```console
$ ros2 launch py_launch_example my_script_launch.xml
```

```console
$ ros2 launch py_launch_example my_script_launch.yaml
```

```console
$ ros2 launch py_launch_example my_script_launch.py
```

C++ 包中的 XML、YAML、Python 启动文件分别使用：

```console
$ ros2 launch cpp_launch_example my_script_launch.xml
```

```console
$ ros2 launch cpp_launch_example my_script_launch.yaml
```

```console
$ ros2 launch cpp_launch_example my_script_launch.py
```

<span id="documentation"></span>

## 文档

[launch 文档](https://github.com/ros2/launch/blob/rolling/launch/doc/source/architecture.rst)详细介绍了 `launch_ros` 同样使用的概念。

更多关于启动功能的文档和示例将陆续补充。目前可以参阅 [launch 源代码](https://github.com/ros2/launch)和 [launch_ros 源代码](https://github.com/ros2/launch_ros)。
