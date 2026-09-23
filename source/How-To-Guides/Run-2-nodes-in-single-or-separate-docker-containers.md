<span id="running-ros-2-nodes-in-docker-community-contributed"></span>
# 在 Docker 中运行 ROS 2 节点（社区贡献）

<span id="run-two-nodes-in-a-single-docker-container"></span>
## 在同一个 Docker 容器中运行两个节点

拉取标签为 `rolling-desktop` 的 ROS Docker 镜像：

```console
$ docker pull osrf/ros:rolling-desktop
```

以交互模式使用该镜像启动容器：

```console
$ docker run -it osrf/ros:rolling-desktop
```

接下来，`ros2` 命令行帮助会很有用：

```console
$ ros2 --help
```

例如，列出所有已安装的软件包：

```console
$ ros2 pkg list
(you will see a list of packages)
```

列出所有可执行程序：

```console
$ ros2 pkg executables
(you will see a list of <package> <executable>)
```

在该容器中运行 `demo_nodes_cpp` 软件包提供的最小示例，其中有两个 C++ 节点：一个话题订阅者 `listener` 和一个话题发布者 `talker`：

```console
$ ros2 run demo_nodes_cpp listener &
$ ros2 run demo_nodes_cpp talker
```

<span id="run-two-nodes-in-two-separate-docker-containers"></span>
## 在两个独立的 Docker 容器中运行两个节点

打开一个终端，以交互模式使用该镜像启动容器，并通过 `ros2 run` 启动话题发布者（`demo_nodes_cpp` 软件包中的 `talker` 可执行程序）：

```console
$ docker run -it --rm osrf/ros:rolling-desktop ros2 run demo_nodes_cpp talker
```

打开第二个终端，以交互模式启动另一个容器，并通过 `ros2 run` 启动话题订阅者（`demo_nodes_cpp` 软件包中的 `listener` 可执行程序）：

```console
$ docker run -it --rm osrf/ros:rolling-desktop ros2 run demo_nodes_cpp listener
```

除了从命令行分别启动，也可以创建一个 `docker-compose.yml` 文件（这里使用版本 2），最小内容如下：

```yaml
version: '2'

services:
  talker:
    image: osrf/ros:rolling-desktop
    command: ros2 run demo_nodes_cpp talker
  listener:
    image: osrf/ros:rolling-desktop
    command: ros2 run demo_nodes_cpp listener
    depends_on:
      - talker
```

在同一目录中执行 `docker compose up` 即可运行这些容器。按 `Ctrl+C` 可以关闭它们。
