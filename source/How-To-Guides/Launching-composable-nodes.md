---
translation_status: machine_translated
source: How-To-Guides/Launching-composable-nodes.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-ros-2-launch-to-launch-composable-nodes"></span>

# 使用 ROS 2 launch 启动可组合节点

在那个 [组成辅导](../Tutorials/Intermediate/Composition.md)中,您从命令行中得知了可混凝土节点以及如何使用这些节点。 [发射教程](../Tutorials/Intermediate/Launch/Launch-Main.md),你学到了发射文件 以及如何使用它们来管理多个节点。

此指南将综合上述两个话题,并教你如何为可堆叠的节点编写发射文件.

<span id="setup"></span>

## 设置

见 [安装指令](../Installation.md) 关于安装ROS 2的详情。

如果您已经从软件包中安装了 ROS 2, 请确保您已经安装过 `ros-rolling-image-tools` 已安装。如果从源头下载归档或建立ROS 2,它就已经是安装的一部分。

<span id="launch-file-examples"></span>

## 启动文件示例

下面是一个发射文件,在XML,YAML,和Python中发射可复合节点。发射文件都做如下工作:

- 以重映射、自定义参数和额外参数来证明一个相机2image compositionable节点

- 以重映射、自定义参数和附加参数来证明一个可显示的节点

##### XML 数据

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <node_container pkg="rclcpp_components" exec="component_container" name="image_container" namespace="">
    <composable_node pkg="image_tools" plugin="image_tools::Cam2Image" name="cam2image">
      <remap from="/image" to="/burgerimage" />
      <param name="width" value="320" />
      <param name="height" value="240" />
      <param name="burger_mode" value="true" />
      <param name="history" value="keep_last" />
      <extra_arg name="use_intra_process_comms" value="true" />
    </composable_node>
    <composable_node pkg="image_tools" plugin="image_tools::ShowImage" name="showimage">
      <remap from="/image" to="/burgerimage" />
      <param name="history" value="keep_last" />
      <extra_arg name="use_intra_process_comms" value="true" />
    </composable_node>
  </node_container>
</launch>
```

##### 也门

``` yaml
%YAML 1.2
---
launch:
  - node_container:
      pkg: rclcpp_components
      exec: component_container
      name: image_container
      composable_node:
        - pkg: image_tools
          plugin: image_tools::Cam2Image
          name: cam2image
          remap:
            - from: /image
              to: /burgerimage
          param:
            - name: width
              value: 320
            - name: height
              value: 240
            - name: burger_mode
              value: true
            - name: history
              value: keep_last
          extra_arg:
            - name: use_intra_process_comms
              value: true

        - pkg: image_tools
          plugin: image_tools::ShowImage
          name: showimage
          remap:
            - from: /image
              to: /burgerimage
          param:
            - name: history
              value: keep_last
          extra_arg:
            - name: use_intra_process_comms
              value: true
```

##### Python

``` python
import launch
from launch_ros.actions import ComposableNodeContainer
from launch_ros.descriptions import ComposableNode


def generate_launch_description():
    """Generate launch description with multiple components."""
    return launch.LaunchDescription([
        ComposableNodeContainer(
            name='image_container',
            namespace='',
            package='rclcpp_components',
            executable='component_container',
            composable_node_descriptions=[
                ComposableNode(
                    package='image_tools',
                    plugin='image_tools::Cam2Image',
                    name='cam2image',
                    remappings=[('/image', '/burgerimage')],
                    parameters=[{
                        'width': 320,
                        'height': 240,
                        'burger_mode': True,
                        'history': 'keep_last'}],
                    extra_arguments=[{'use_intra_process_comms': True}]),
                ComposableNode(
                    package='image_tools',
                    plugin='image_tools::ShowImage',
                    name='showimage',
                    remappings=[('/image', '/burgerimage')],
                    parameters=[{'history': 'keep_last'}],
                    extra_arguments=[{'use_intra_process_comms': True}])
            ],
            output='both',
        ),
    ])
```

<span id="loading-composable-nodes-into-an-existing-container"></span>

## 将可堆肥的节点装入现有容器

容器有时可以通过其他发射文件或命令线发射。在这种情况下,需要将组件添加到现有的容器中。为此,可以使用 `LoadComposableNodes` 将组件装入给定的容器。下面的例子发射的节点与上面的相同。

##### XML 数据

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <node pkg="rclcpp_components" exec="component_container" name="image_container" />
  <load_composable_node target="image_container">
    <composable_node pkg="image_tools" plugin="image_tools::Cam2Image" name="cam2image">
      <remap from="/image" to="/burgerimage" />
      <param name="width" value="320" />
      <param name="height" value="240" />
      <param name="burger_mode" value="true" />
      <param name="history" value="keep_last" />
      <extra_arg name="use_intra_process_comms" value="true" />
    </composable_node>
    <composable_node pkg="image_tools" plugin="image_tools::ShowImage" name="showimage" namespace="">
      <remap from="/image" to="/burgerimage" />
      <param name="history" value="keep_last" />
      <extra_arg name="use_intra_process_comms" value="true" />
    </composable_node>
  </load_composable_node>
</launch>
```

##### 也门

``` yaml
%YAML 1.2
---
launch:
  - node_container:
      pkg: rclcpp_components
      exec: component_container
      name: image_container
  - load_composable_node:
      target: "image_container"
      composable_node:
        - pkg: image_tools
          plugin: image_tools::Cam2Image
          name: cam2image
          remap:
            - from: /image
              to: /burgerimage
          param:
            - name: width
              value: 320
            - name: height
              value: 240
            - name: burger_mode
              value: true
            - name: history
              value: keep_last
          extra_arg:
            - name: use_intra_process_comms
              value: true

        - pkg: image_tools
          plugin: image_tools::ShowImage
          name: showimage
          remap:
            - from: /image
              to: /burgerimage
          param:
            - name: history
              value: keep_last
          extra_arg:
            - name: use_intra_process_comms
              value: true
```

##### Python

``` python
from launch import LaunchDescription
from launch_ros.actions import LoadComposableNodes, Node
from launch_ros.descriptions import ComposableNode


def generate_launch_description():
    return LaunchDescription([
        Node(
            name='image_container',
            package='rclcpp_components',
            executable='component_container',
            output='both',
        ),
        LoadComposableNodes(
            target_container='image_container',
            composable_node_descriptions=[
                ComposableNode(
                    package='image_tools',
                    plugin='image_tools::Cam2Image',
                    name='cam2image',
                    remappings=[('/image', '/burgerimage')],
                    parameters=[
                        {'width': 320, 'height': 240, 'burger_mode': True, 'history': 'keep_last'}
                    ],
                    extra_arguments=[{'use_intra_process_comms': True}],
                ),
                ComposableNode(
                    package='image_tools',
                    plugin='image_tools::ShowImage',
                    name='showimage',
                    remappings=[('/image', '/burgerimage')],
                    parameters=[{'history': 'keep_last'}],
                    extra_arguments=[{'use_intra_process_comms': True}]
                ),
            ],
        )
    ])
```

<span id="using-the-launch-files-from-the-command-line"></span>

## 使用命令行的启动文件

上面的任何发射文件都可以用 `ros2 launch`。将数据复制到本地文件,然后运行:

``` console
$ ros2 launch <path_to_launch_file>
```

<span id="intra-process-communications"></span>

## 流程内通信

上述所有例子都使用了额外的参数来设置节点之间的进程内通信。关于进程内通信的更多信息,请参见: [进程内通讯教程](../Tutorials/Demos/Intra-Process-Communication.md).

<span id="xml-yaml-or-python-which-should-i-use"></span>

## XML, YAML, 或 Python: 我应该用哪一种?

见 [讨论情况](Launch-file-different-formats.md#launch-file-different-formats-which) 输入 [使用 XML、YAML 和 Python 编写 ROS 2 启动文件](Launch-file-different-formats.md) 以获取更多信息。
