---
translation_status: machine_translated
source: How-To-Guides/Run-2-nodes-in-single-or-separate-docker-containers.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="running-ros-2-nodes-in-docker-community-contributed"></span>

# 在 Docker 中运行 ROS 2 节点（社区贡献）

<span id="run-two-nodes-in-a-single-docker-container"></span>

## 在单个嵌入器容器中运行两个节点

绘制带有“滚动桌面”标签的 ROS 插头图像。

``` console
$ docker pull osrf/ros:rolling-desktop
```

在交互式模式下在容器中运行图像 。

``` console
$ docker run -it osrf/ros:rolling-desktop
```

你最好的朋友是那个 `ros2` 现在命令行帮助。

``` console
$ ros2 --help
```

例如列出所有已安装的软件包.

``` console
$ ros2 pkg list
(you will see a list of packages)
```

例如,列出所有可执行文件:

``` console
$ ros2 pkg executables
(you will see a list of <package> <executable>)
```

运行一个 2 C++ 节点的最小示例(1 个主题订阅器) `listener`, 1个专题出版社 `talker`从包中 `demo_nodes_cpp` 在此容器中:

``` console
$ ros2 run demo_nodes_cpp listener &
$ ros2 run demo_nodes_cpp talker
```

<span id="run-two-nodes-in-two-separate-docker-containers"></span>

## 在两个独立的插头容器中运行两个节点

打开终端。 以交互模式在容器中运行图像并启动主题发布器( 可执行) `talker` 从软件包中 `demo_nodes_cpp`与 `ros2 run`:

``` console
$ docker run -it --rm osrf/ros:rolling-desktop ros2 run demo_nodes_cpp talker
```

打开第二个终端。 以交互模式在容器中运行图像并启动主题订阅器( 可执行) `listener` 从软件包中 `demo_nodes_cpp`与 `ros2 run`:

``` console
$ docker run -it --rm osrf/ros:rolling-desktop ros2 run demo_nodes_cpp listener
```

作为命令行引用的替代品,您可以创建 `docker-compose.yml` 文件(此处为第2版),包含以下(最小)内容:

``` yaml
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

要运行容器呼叫 `docker compose up` 您可以关闭容器。 `Ctrl+C`.
