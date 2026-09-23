<span id="using-xml-yaml-and-python-for-ros-2-launch-files"></span>

# 使用 XML、YAML 和 Python 编写 ROS 2 启动文件

ROS 2 启动文件可以使用 XML、YAML 或 Python 编写。
本指南演示如何使用这些格式完成相同的任务，并讨论各格式的适用情况。

<span id="launch-file-examples"></span>

## 启动文件示例

下面分别给出 XML、YAML 和 Python 格式的启动文件。每个文件都执行以下操作：

- 设置命令行参数及其默认值。
- 包含另一个启动文件。
- 在另一个命名空间中包含启动文件。
- 启动一个节点，并设置其命名空间。
- 启动一个节点，设置其命名空间，并使用传入的参数设置节点参数。
- 创建一个节点，将消息从一个话题重映射到另一个话题。

对应的示例文件：

- [XML：different_formats_launch.xml](launch/different_formats_launch.xml)
- [YAML：different_formats_launch.yaml](launch/different_formats_launch.yaml)
- [Python：different_formats_launch.py](launch/different_formats_launch.py)

<span id="using-the-launch-files-from-the-command-line"></span>

## 从命令行运行启动文件

<span id="launching"></span>

### 启动

上述任一启动文件都可以通过 `ros2 launch` 运行。
要在本地尝试这些文件，可以创建一个新软件包，然后执行：

```console
$ ros2 launch <package_name> <launch_file_name>
```

也可以指定启动文件的路径，直接运行该文件：

```console
$ ros2 launch <path_to_launch_file>
```

<span id="setting-arguments"></span>

### 设置启动参数

向启动文件传递参数时，应使用 `key:=value` 语法。
例如，可以这样设置 `background_r` 的值：

```console
$ ros2 launch <package_name> <launch_file_name> background_r:=255
```

或者：

```console
$ ros2 launch <path_to_launch_file> background_r:=255
```

<span id="controlling-the-turtles"></span>

### 控制海龟

为了验证重映射是否生效，可以在另一个终端中运行以下命令来控制海龟：

```console
$ ros2 run turtlesim turtle_teleop_key --ros-args --remap __ns:=/turtlesim1
```

<span id="xml-yaml-or-python-which-should-i-use"></span><span id="launch-file-different-formats-which"></span>

## 应该使用 XML、YAML 还是 Python？

!!! note "说明"

    ROS 1 的启动文件使用 XML 编写，因此对于从 ROS 1 迁移的用户，XML 可能最为熟悉。
    要了解其中的变化，请参阅[迁移启动文件](Migrating-from-ROS1/Migrating-Launch-Files.md)。

对于大多数应用，选择哪一种 ROS 2 启动文件格式主要取决于开发者的偏好。
不过，如果启动文件需要 XML 或 YAML 无法提供的灵活性，就可以使用 Python。
Python 更灵活，主要有以下两个原因：

- Python 是脚本语言，因此可以在启动文件中使用其语言特性和各种库。
- 提供通用启动功能的 [ros2/launch](https://github.com/ros2/launch) 和提供 ROS 2 专用启动功能的 [ros2/launch_ros](https://github.com/ros2/launch_ros) 都使用 Python 编写，因此可以访问 XML 和 YAML 未必提供的底层启动功能。

不过，使用 Python 编写的启动文件也可能比 XML 或 YAML 格式更复杂、篇幅更长。
