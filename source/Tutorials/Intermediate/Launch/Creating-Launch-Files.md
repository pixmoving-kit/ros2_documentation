<span id="creating-a-launch-file"></span>

# 创建启动文件

**目标：** 创建启动文件，运行复杂的 ROS 2 系统。

**教程级别：** 中级

**预计耗时：** 10 分钟

<span id="prerequisites"></span>

## 前提条件

本教程使用 [rqt_graph 和 turtlesim](../../Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md) 软件包，还需要一个你喜欢的文本编辑器。

与往常一样，别忘记在[每个新打开的终端](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)中加载 ROS 2 环境。

<span id="background"></span>

## 背景

ROS 2 启动系统帮助用户描述系统配置，并按照描述执行。配置包括运行哪些程序、在哪里运行、传递哪些参数，以及 ROS 特有的约定。通过为组件赋予不同配置，可以方便地在整个系统中复用组件。启动系统还负责监控已启动进程的状态，并报告或响应状态变化。

XML、YAML 或 Python 启动文件可以启动和停止不同节点，也可以触发和响应各种事件。各种格式的介绍见[启动文件格式](../../../How-To-Guides/Launch-file-different-formats.md)。提供这一框架的软件包是 `launch_ros`，其底层使用不依赖 ROS 的 `launch` 框架。

[设计文档](https://design.ros2.org/articles/roslaunch.html)详细介绍了 ROS 2 启动系统的设计目标，其中部分功能尚未实现。

<span id="tasks"></span>

## 任务

<span id="setup"></span>

### 1 准备

创建存放启动文件的目录：

```console
$ mkdir launch
```

<span id="write-the-launch-file"></span>

### 2 编写启动文件

使用 `turtlesim` 包及其中的可执行程序编写 ROS 2 启动文件，可以选择 XML、YAML 或 Python：

- XML：将[完整示例](launch/turtlesim_mimic_launch.xml)复制到 `launch/turtlesim_mimic_launch.xml`。
- YAML：将[完整示例](launch/turtlesim_mimic_launch.yaml)复制到 `launch/turtlesim_mimic_launch.yaml`。
- Python：将[完整示例](launch/turtlesim_mimic_launch.py)复制到 `launch/turtlesim_mimic_launch.py`。

<span id="examine-the-launch-file"></span>

#### 2.1 分析启动文件

以上启动文件都会启动一个由三个节点组成的系统，节点均来自 `turtlesim` 包。系统会打开两个 turtlesim 窗口，让一只海龟模仿另一只的运动。

两个 turtlesim 节点的主要区别是命名空间。不同的命名空间使系统能够启动两个节点而不发生节点名或话题名冲突。两个海龟都通过同名话题接收命令、发布位姿，命名空间则用于区分发给不同海龟的消息。

两个节点也演示了不同的参数传递方式。第一个使用 `args` 直接向可执行程序传递参数，其中 ROS 专用参数需要加上 `--ros-args`。第二个使用专门传递 ROS 参数的 `ros_args`（Python 中为 `ros_arguments`）。混合 ROS 与非 ROS 参数时可使用 `args`，例如 `my_custom_arg --ros-args --log-level info`；只传递重映射、参数或日志级别等 ROS 参数时，使用 `ros_args` 更简洁。

最后一个节点同样来自 `turtlesim`，但使用的是 `mimic` 可执行程序。它通过重映射增加了配置：将 `/input/pose` 重映射到 `/turtlesim1/turtle1/pose`，将 `/output/cmd_vel` 重映射到 `/turtlesim2/turtle1/cmd_vel`。因此，`mimic` 订阅 `/turtlesim1/sim` 的位姿话题，再向 `/turtlesim2/sim` 所订阅的速度命令话题发布数据。换句话说，`turtlesim2` 将模仿 `turtlesim1` 的运动。

在 [XML 示例](launch/turtlesim_mimic_launch.xml)中，第 3–4 行的前两个动作通过不同参数传递方式启动两个窗口，第 5–8 行的最后一个动作启动带重映射的 mimic 节点。

在 [YAML 示例](launch/turtlesim_mimic_launch.yaml)中，对应位置分别为第 4–16 行和第 18–26 行。

在 [Python 示例](launch/turtlesim_mimic_launch.py)中，第 1–2 行导入所需的 Python `launch` 模块；第 5–6 行及第 30 行定义启动描述；第 7–20 行的前两个动作启动两个窗口；第 21–29 行的最后一个动作启动带重映射的 mimic 节点。

<span id="ros2-launch"></span>

### 3 ros2 launch

进入先前创建的目录，运行相应命令：

XML：

```console
$ cd launch
$ ros2 launch turtlesim_mimic_launch.xml
```

YAML：

```console
$ cd launch
$ ros2 launch turtlesim_mimic_launch.yaml
```

Python：

```console
$ cd launch
$ ros2 launch turtlesim_mimic_launch.py
```

可以像上面一样直接启动文件，也可以启动软件包提供的启动文件，后者语法为：

```console
$ ros2 launch <package_name> <launch_file_name>
```

创建软件包的方法见[创建第一个 ROS 2 软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md)。

对于包含启动文件的软件包，建议在 `package.xml` 中将 `ros2launch` 声明为 `exec_depend`：

```xml
<exec_depend>ros2launch</exec_depend>
```

这有助于确保软件包构建后 `ros2 launch` 命令可用，并能识别所有[启动文件格式](../../../How-To-Guides/Launch-file-different-formats.md)。

随后会打开两个 turtlesim 窗口，并显示以下 `[INFO]` 消息，说明启动文件启动了哪些节点：

```console
[INFO] [launch]: Default logging verbosity is set to INFO
[INFO] [turtlesim_node-1]: process started with pid [11714]
[INFO] [turtlesim_node-2]: process started with pid [11715]
[INFO] [mimic-3]: process started with pid [11716]
```

打开新终端，用 `ros2 topic pub` 向 `/turtlesim1/turtle1/cmd_vel` 发布消息，让第一只海龟运动：

```console
$ ros2 topic pub -r 1 /turtlesim1/turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: -1.8}}"
```

可以看到两只海龟沿相同路径运动。

![](images/mimic.png)

<span id="introspect-the-system-with-rqt-graph"></span>

### 4 使用 rqt_graph 查看系统

保持系统运行，打开新终端并运行 `rqt_graph`，进一步了解启动文件中各节点之间的关系：

```console
$ ros2 run rqt_graph rqt_graph
```

![](images/mimic_graph.png)

左侧有一个隐藏节点，即刚才运行的 `ros2 topic pub` 命令，向 `/turtlesim1/turtle1/cmd_vel` 发布数据，而 `/turtlesim1/sim` 订阅该话题。图中其余部分与前面的描述一致：`mimic` 订阅 `/turtlesim1/sim` 的位姿话题，再向 `/turtlesim2/sim` 的速度命令话题发布数据。

<span id="summary"></span>

## 小结

启动文件简化了包含多个节点和详细配置的复杂系统的运行过程。可以使用 XML、YAML 或 Python 创建启动文件，并通过 `ros2 launch` 运行。
