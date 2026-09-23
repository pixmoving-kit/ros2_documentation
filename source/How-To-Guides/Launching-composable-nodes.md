<span id="using-ros-2-launch-to-launch-composable-nodes"></span>

# 使用 ROS 2 launch 启动可组合节点

在[组件组合教程](../Tutorials/Intermediate/Composition.md)中，你已经了解了可组合节点，以及如何通过命令行使用它们。
在[启动教程](../Tutorials/Intermediate/Launch/Launch-Main.md)中，你学习了启动文件，以及如何使用启动文件管理多个节点。

本指南将结合这两部分内容，介绍如何为可组合节点编写启动文件。

<span id="setup"></span>

## 环境准备

有关安装 ROS 2 的详细信息，请参阅[安装说明](../Installation.md)。

如果通过软件包安装了 ROS 2，请确认已安装 `ros-rolling-image-tools`。
如果下载的是二进制归档，或从源码构建 ROS 2，则安装内容中已包含该软件包。

<span id="launch-file-examples"></span>

## 启动文件示例

下面分别给出使用 XML、YAML 和 Python 启动可组合节点的启动文件。这些文件都执行以下操作：

- 实例化一个 `cam2image` 可组合节点，并设置重映射、自定义参数和额外参数。
- 实例化一个 `showimage` 可组合节点，并设置重映射、自定义参数和额外参数。

对应的示例文件：

- [XML：composition_launch.xml](launch/composition_launch.xml)
- [YAML：composition_launch.yaml](launch/composition_launch.yaml)
- [Python：composition_launch.py](launch/composition_launch.py)

<span id="loading-composable-nodes-into-an-existing-container"></span>

## 将可组合节点加载到已有容器中

容器有时由其他启动文件或命令行启动。在这种情况下，需要将组件添加到已有容器。
可以使用 `LoadComposableNodes` 将组件加载到指定容器中。
以下示例启动的节点与前面的示例相同：

- [XML：composition_load_launch.xml](launch/composition_load_launch.xml)
- [YAML：composition_load_launch.yaml](launch/composition_load_launch.yaml)
- [Python：composition_load_launch.py](launch/composition_load_launch.py)

<span id="using-the-launch-files-from-the-command-line"></span>

## 从命令行运行启动文件

上述任一启动文件都可以通过 `ros2 launch` 运行。
将示例内容复制到本地文件，然后执行：

```console
$ ros2 launch <path_to_launch_file>
```

<span id="intra-process-communications"></span>

## 进程内通信

以上所有示例都通过额外参数配置节点之间的进程内通信。
有关进程内通信的详细介绍，请参阅[进程内通信教程](../Tutorials/Demos/Intra-Process-Communication.md)。

<span id="xml-yaml-or-python-which-should-i-use"></span>

## 应该使用 XML、YAML 还是 Python？

请参阅[不同格式启动文件](Launch-file-different-formats.md)中的[相关讨论](Launch-file-different-formats.md#launch-file-different-formats-which)。
