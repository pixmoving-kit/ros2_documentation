---
translation_status: machine_translated
source: How-To-Guides/Migrating-from-ROS1/Migrating-Parameters.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="migrating-parameters"></span>

# 迁移参数

在ROS 1中,参数与中央服务器相关联,通过使用网络API在运行时允许检索参数. 在ROS 2中,参数每个节点关联,在运行时可配置ROS服务.

- 见 [ROS 2 参数设计文件](https://design.ros2.org/articles/ros_parameters.html) 关于系统模型的更多细节。

- 见 [ROS 2 CLI 使用量](../../Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md) 以便更好地了解国家综合倡议工具如何运作及其与ROS 1工具的区别。

<span id="global-parameter-server"></span>

## 全球参数服务器

在ROS 1中, `roscore` 动作像一个全局参数黑板,所有节点都可以得到并设置参数。因为没有中心 `roscore` 在ROS 2 中,该功能已经不存在。ROS 2中推荐的方法是使用与使用它们的节点紧密相连的每个节点参数。如果仍然需要一个全局黑板,则有可能为此创建一个专用节点。 `ros-rolling-demo-nodes-cpp` 调用软件包 `parameter_blackboard`; 可用下列方式运行:

``` console
$ ros2 run demo_nodes_cpp parameter_blackboard
```

密码 `parameter_blackboard` 是,这是 [这儿](https://github.com/ros2/demos/blob/rolling/demo_nodes_cpp/src/parameters/parameter_blackboard.cpp).

<span id="migrating-yaml-parameter-files"></span>

## 正在移动 YAML 参数文件

本指南描述了ROS 1参数文件如何适用于ROS 2.

<span id="yaml-file-example"></span>

### YAML 文件示例

YAML 用于在 ROS 1 和 ROS 2. 中写入参数文件,ROS 2 的主要区别在于必须使用节点名称来处理参数。除了完全合格的节点名称外,我们还使用密钥“ ros_参数” 来表示节点参数的开始。

例如,这里是一个 ROS 1: 中的参数文件 。

``` yaml
lidar_name: foo
lidar_id: 10
ports: [11312, 11311, 21311]
debug: true
```

让我们假设前两个参数是用来命名节点的 `/lidar_ns/lidar_node_name`,下一个参数是命名为“节点”的节点 `/imu`,以及我们要在两个节点上设置的最后一个参数。

我们将构建我们的ROS 2参数文件如下:

``` yaml
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

注意使用通配符(`/**`)以表示参数 `debug` 应当设置在任意命名空间中的任何节点上。

<span id="feature-parity"></span>

### 特征均等

ROS 1 参数文件的一些特性在ROS 2:中不存在.

- 列表中的混合类型尚未支持( N)[相关议题](https://github.com/ros2/rcl/issues/463))

- `deg` 财务报告和财务报告 `rad` 不支持替换

<span id="parameter-atomic-operation"></span>

## 参数原子操作

当将 ROS 1 的参数组迁移到 ROS 2 时,需要考虑重要的差异. In ROS 1, `dynamic_reconfigure` 从原子角度处理参数组,意思是重组请求中的所有参数都在同一回调中一起处理。 `set_parameters` 服务单个处理每个参数,这可能导致多次召回引用。 `dynamic_reconfigure`时,使用 `set_parameters_atomically` 服务,它将所有参数作为单一的操作进行验证和应用。如果任何参数不能进行验证,则不会更新参数。
