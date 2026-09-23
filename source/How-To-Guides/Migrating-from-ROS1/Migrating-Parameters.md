<span id="migrating-parameters"></span>
# 迁移参数

ROS 1 的参数关联到一个中央服务器，可以通过网络 API 在运行时获取。ROS 2 的参数分别属于各个节点，并通过 ROS 服务在运行时配置。

- 系统模型的更多详情见 [ROS 2 参数设计文档](https://design.ros2.org/articles/ros_parameters.html)。
- [ROS 2 命令行工具用法](../../Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md)介绍了 CLI 工具的工作方式，以及它们与 ROS 1 工具的区别。

<span id="global-parameter-server"></span>
## 全局参数服务器

ROS 1 的 `roscore` 类似一个全局参数黑板，所有节点都可以从中读取和设置参数。ROS 2 没有中央 `roscore`，因此也没有这项功能。推荐在 ROS 2 中使用节点级参数，让参数与使用它们的节点紧密关联。

如果仍然需要全局黑板，可以创建一个专用节点。ROS 2 的 `ros-rolling-demo-nodes-cpp` 包提供了名为 `parameter_blackboard` 的节点，可通过以下命令运行：

```console
$ ros2 run demo_nodes_cpp parameter_blackboard
```

`parameter_blackboard` 的源码见[此文件](https://github.com/ros2/demos/blob/rolling/demo_nodes_cpp/src/parameters/parameter_blackboard.cpp)。

<span id="migrating-yaml-parameter-files"></span>
## 迁移 YAML 参数文件

下面介绍如何将 ROS 1 参数文件调整为 ROS 2 格式。

<span id="yaml-file-example"></span>
### YAML 文件示例

ROS 1 和 ROS 2 都使用 YAML 编写参数文件。主要区别在于，ROS 2 必须通过节点名称指定参数所属的节点。除了节点的完全限定名称，还使用 `ros__parameters` 键标记该节点参数的起始位置。

例如，以下是一个 ROS 1 参数文件：

```yaml
lidar_name: foo
lidar_id: 10
ports: [11312, 11311, 21311]
debug: true
```

假设前两个参数属于 `/lidar_ns/lidar_node_name` 节点，第三个参数属于 `/imu` 节点，最后一个参数需要同时设置到两个节点。ROS 2 参数文件可以写成：

```yaml
/lidar_ns:
  lidar_node_name:
    ros__parameters:
      lidar_name: foo
      id: 10
imu:
  ros__parameters:
    ports: [2438, 2439, 2440]
/**:
  ros__parameters:
    debug: true
```

注意，通配符 `/**` 表示为任意命名空间中的任意节点设置 `debug` 参数。

<span id="feature-parity"></span>
### 功能差异

ROS 1 参数文件中的某些功能在 ROS 2 中尚不可用：

- 尚不支持同一列表中混合多种类型，参见[相关问题](https://github.com/ros2/rcl/issues/463)。
- 不支持 `deg` 和 `rad` 替换表达式。

<span id="parameter-atomic-operation"></span>
## 参数原子操作

从 ROS 1 迁移参数组到 ROS 2 时，需要注意一个重要差异。ROS 1 的 `dynamic_reconfigure` 以原子方式处理参数组，即一次重配置请求中的所有参数都在同一个回调中一起处理。ROS 2 的 `set_parameters` 服务逐个处理参数，因此可能多次调用回调。

从 `dynamic_reconfigure` 迁移时，要保持原子行为，应使用 `set_parameters_atomically` 服务。它将所有参数的验证和应用作为一次操作处理；只要任意参数验证失败，就不会更新任何参数。
