<span id="using-substitutions"></span>

# 使用替换

**目标：** 了解 ROS 2 启动文件中的替换机制。

**教程级别：** 中级

**预计耗时：** 15 分钟

<span id="background"></span>

## 背景

启动文件用于启动节点、服务和执行进程。这些动作可以接收影响其行为的参数。在参数中使用替换，可更灵活地描述可复用的启动文件。替换是一种仅在执行启动描述时才求值的变量，可用于获取启动配置、环境变量等特定信息，或计算任意 Python 表达式。

本教程展示 ROS 2 启动文件中替换机制的使用示例。

<span id="prerequisites"></span>

## 前提条件

本教程使用 [turtlesim](../../Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md) 包，并假设你已熟悉[创建软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md)。

与往常一样，别忘记在[每个新打开的终端](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)中加载 ROS 2 环境。

<span id="id1"></span>

## 使用替换

<span id="create-and-setup-the-package"></span>

### 1 创建并配置软件包

首先创建名为 `launch_tutorial` 的包。

Python 包使用 `ament_python` 构建类型：

```console
$ ros2 pkg create --build-type ament_python --license Apache-2.0 launch_tutorial
```

C++ 包使用 `ament_cmake` 构建类型：

```console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 launch_tutorial
```

在包内创建 `launch` 目录。

Linux：

```console
$ mkdir launch_tutorial/launch
```

macOS：

```console
$ mkdir launch_tutorial/launch
```

Windows：

```console
$ md launch_tutorial/launch
```

最后，确保启动文件会被安装。对于 Python 包，修改 `setup.py`：

```python
import os
from glob import glob
from setuptools import find_packages, setup

package_name = 'launch_tutorial'

setup(
    # Other parameters ...
    data_files=[
        # ... Other data files
        # Include all launch files.
        (os.path.join('share', package_name, 'launch'), glob('launch/*'))
    ]
)
```

对于 C++ 包，在 `CMakeLists.txt` 的 `ament_package()` 之前添加：

```cmake
install(DIRECTORY
        launch
        DESTINATION share/${PROJECT_NAME}/
)
```

<span id="parent-launch-file"></span>

### 2 父启动文件

接下来创建一个调用另一启动文件并向其传参的启动文件，可采用 XML、YAML 或 Python。将文件放在 `launch_tutorial` 包的 `launch` 目录中。

**XML：** 将[完整示例](launch/example_main_launch.xml)复制到 `launch/example_main_launch.xml`。`$(find-pkg-share launch_tutorial)` 替换用于查找 `launch_tutorial` 包的路径，再将该路径与 `example_substitutions_launch.xml` 文件名拼接（第 4 行）。将 `background_r` 变量与 `turtlesim_ns`、`use_provided_red` 参数传给 `include` 动作；`$(var background_r)` 替换使用 `background_r` 的值定义 `new_background_r` 参数（第 5–7 行）。

**YAML：** 将[完整示例](launch/example_main_launch.yaml)复制到 `launch/example_main_launch.yaml`。同样使用 `$(find-pkg-share launch_tutorial)` 查找包路径，并与 `example_substitutions_launch.yaml` 拼接（第 8 行）；将 `background_r`、`turtlesim_ns` 和 `use_provided_red` 传给 `include`，通过 `$(var background_r)` 定义 `new_background_r`（第 9–15 行）。

**Python：** 将[完整示例](launch/example_main_launch.py)复制到 `launch/example_main.launch.py`。`FindPackageShare` 查找包路径，`PathJoinSubstitution` 将其与 `example_substitutions.launch.py` 文件名拼接（第 14–18 行）。包含 `turtlesim_ns` 和 `use_provided_red` 参数的 `launch_arguments` 字典传给 `IncludeLaunchDescription` 动作（第 19–23 行）。

<span id="substitutions-example-launch-file"></span>

### 3 替换示例启动文件

在同一目录中创建被调用的启动文件。

**XML：** 创建 `launch/example_substitutions_launch.xml`，内容使用 [XML 示例](launch/example_substitutions_launch.xml)。定义 `turtlesim_ns`、`use_provided_red`、`new_background_r` 启动配置，保存启动参数值并传给所需动作。在启动描述的任何位置，都可用 `$(var <name>)` 获取启动参数值。`arg` 标签声明可由父启动文件或命令行传入的参数（第 3–5 行）。

定义 `turtlesim_node`，用 `$(var <name>)` 将其 `namespace` 设为 `turtlesim_ns` 的值（第 7 行）。接着定义带有 `cmd` 标签的 `executable` 动作，调用 turtlesim 节点的 spawn 服务；构造命令字符串时也通过 `$(var <name>)` 获取 `turtlesim_ns`（第 8 行）。

同样的方法用于执行 `ros2 param` 的动作，修改背景色的红色分量。不同之处在于：计时器中的第二个动作仅当 `new_background_r` 等于 `200` 且 `use_provided_red` 为 `True` 时执行。`if` 条件使用 `$(eval <python-expression>)` 替换求值（第 9–13 行）。

**YAML：** 创建 `launch/example_substitutions_launch.yaml`，内容使用 [YAML 示例](launch/example_substitutions_launch.yaml)。同样定义三个启动配置，用于保存并向动作传递启动参数值，并通过 `$(var <name>)` 在描述中读取。`arg` 标签声明可由父启动文件或命令行传入的参数（第 4–12 行）。

使用 `$(var <name>)` 将 `turtlesim_node` 的命名空间设为 `turtlesim_ns`（第 14–18 行）。带有 `cmd` 的 `executable` 动作调用 spawn 服务，命令字符串中的命名空间也通过替换获取（第 19–20 行）。`ros2 param` 动作用同样的方法修改背景红色分量；计时器中的第二个动作仅在 `new_background_r` 为 `200`、`use_provided_red` 为 `True` 时执行，条件由 `$(eval <python-expression>)` 求值（第 21–28 行）。

**Python：** 创建 `launch/example_substitutions.launch.py`，内容使用 [Python 示例](launch/example_substitutions_launch.py)。定义 `turtlesim_ns`、`use_provided_red`、`new_background_r`，用来表示启动参数值并传给相应动作。`LaunchConfiguration` 替换允许在启动描述的任意位置取得参数值；`DeclareLaunchArgument` 声明可由父启动文件或命令行传入的参数（第 14–25 行）。

定义 `turtlesim_node`，将 `namespace` 设为 `turtlesim_ns` 这一 `LaunchConfiguration` 替换（第 26–31 行）。接着通过 `ExecuteProcess` 的 `cmd` 参数调用 spawn 服务，并用 `LaunchConfiguration` 在命令字符串中提供 `turtlesim_ns` 的值（第 32–41 行）。

`change_background_r` 和 `change_background_r_conditioned` 也采用这种方式修改背景红色分量。后一个动作仅在 `new_background_r` 为 `200` 且 `use_provided_red` 为 `True` 时执行，`IfCondition` 中的条件由 `PythonExpression` 替换求值（第 51–72 行）。

<span id="build-the-package"></span>

### 4 构建软件包

进入工作空间根目录并构建：

```console
$ colcon build
```

构建后记得加载工作空间环境。

<span id="launching-example"></span>

## 运行示例

使用 `ros2 launch` 启动。YAML、XML、Python 对应命令分别为：

```console
$ ros2 launch launch_tutorial example_main.launch.yaml
```

```console
$ ros2 launch launch_tutorial example_main_launch.xml
```

```console
$ ros2 launch launch_tutorial example_main.launch.py
```

启动后将执行以下操作：

1. 启动背景为蓝色的 turtlesim 节点。
2. 生成第二只海龟。
3. 将背景改为紫色。
4. 如果提供的 `background_r` 为 `200` 且 `use_provided_red` 为 `True`，在两秒后将背景改为粉色。

<span id="modifying-launch-arguments"></span>

## 修改启动参数

对于 YAML，可以修改 `example_main.launch.yaml` 中的 `background_r` 变量，也可以直接启动 `example_substitutions.launch.yaml` 并传入所需参数。查看可接受的参数：

```console
$ ros2 launch launch_tutorial example_substitutions.launch.yaml --show-args
```

对于 XML，可以修改 `example_main_launch.xml` 中的 `background_r`，或直接向 `example_substitutions_launch.xml` 传参。查看参数：

```console
$ ros2 launch launch_tutorial example_substitutions_launch.xml --show-args
```

对于 Python，可以修改 `example_main.launch.py` 的 `launch_arguments` 字典，或直接向 `example_substitutions.launch.py` 传参。查看参数：

```console
$ ros2 launch launch_tutorial example_substitutions.launch.py --show-args
```

命令将显示启动文件可接收的参数及其默认值：

```console
Arguments (pass arguments as '<name>:=<value>'):

    'turtlesim_ns':
        no description given
        (default: 'turtlesim1')

    'use_provided_red':
        no description given
        (default: 'False')

    'new_background_r':
        no description given
        (default: '200')
```

现在可以传入所需参数。YAML、XML、Python 对应命令分别为：

```console
$ ros2 launch launch_tutorial example_substitutions.launch.yaml turtlesim_ns:='turtlesim3' use_provided_red:='True' new_background_r:=200
```

```console
$ ros2 launch launch_tutorial example_substitutions_launch.xml turtlesim_ns:='turtlesim3' use_provided_red:='True' new_background_r:=200
```

```console
$ ros2 launch launch_tutorial example_substitutions.launch.py turtlesim_ns:='turtlesim3' use_provided_red:='True' new_background_r:=200
```

<span id="documentation"></span>

## 文档

[launch 文档](https://docs.ros.org/en/rolling/p/launch/doc/source/architecture.html)提供了可用替换的详细信息。

<span id="summary"></span>

## 小结

本教程介绍了启动文件中的替换机制，以及如何利用它的能力创建可复用的启动文件。

接下来可学习[使用事件处理器](Using-Event-Handlers.md)，通过复杂规则动态调整启动文件的行为。
