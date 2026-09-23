<span id="using-a-urdf-in-gazebo"></span>

# 在 Gazebo 中使用 URDF

**目标：** 在 Gazebo 仿真器中仿真 URDF 模型。

**教程级别：** 中级

**预计耗时：** 30 分钟

本教程基于[这篇 ROS 1 教程](http://wiki.ros.org/urdf/Tutorials/Using%20a%20URDF%20in%20Gazebo)。

首先安装演示包及其依赖。

Ubuntu 软件包：

```console
sudo apt install ros-rolling-urdf-sim-tutorial
```

RHEL 软件包：

```console
sudo dnf install ros-rolling-urdf-sim-tutorial
```

从源码安装：

```console
git clone https://github.com/ros/urdf_sim_tutorial.git -b ros2
```

<span id="nonfunctional-gazebo-interface"></span>

## 尚不能交互的 Gazebo 模型

可使用 `gazebo.launch.py` 将已经创建的模型生成到 Gazebo 中：

```console
ros2 launch urdf_sim_tutorial gazebo.launch.py
```

这个启动文件会：

- 加载[宏教程](Using-Xacro-to-Clean-Up-a-URDF-File.md)中的 URDF，发布到 `/robot_description` 话题。
- 启动空白 Gazebo 世界。
- 运行脚本，从话题读取 URDF 并在 Gazebo 中生成模型。
- 默认显示 Gazebo GUI，效果如下：

![尚不能交互的机器人](https://raw.githubusercontent.com/ros/urdf_sim_tutorial/ros2/doc/NonFunctional.png)

不过，这个模型还不会做任何事，也缺少 ROS 使用它所需的许多关键信息。在[可视化模型](Building-a-Visual-Robot-Model-with-URDF-from-Scratch.md)和[可运动模型](Building-a-Movable-Robot-Model-with-URDF.md)教程中，我们通过 [joint_state_publisher](https://index.ros.org/p/joint_state_publisher/github-ros-joint_state_publisher/) 指定各关节位姿。但在真实环境或 Gazebo 中，这些信息应由机器人本身提供；若没有相应配置，Gazebo 并不知道需要发布它们。

要让机器人能与你和 ROS 交互，必须指定两类内容：插件和控制器。

<span id="side-note-configuring-meshes"></span>

### 补充：配置网格

![缺少网格的机器人](https://raw.githubusercontent.com/ros/urdf_sim_tutorial/ros2/doc/NoMesh.png)

使用自己的机器人跟随教程，或配置出现问题时，Gazebo GUI 中可能缺少模型网格，例如夹爪不见了。这也可能让 Gazebo 在启动画面出现后再等待数秒，因为它正在联网查找缺失的模型。

原因是 URDF 软件包必须明确告诉 Gazebo 从哪里加载网格。修改网格所在包的 `package.xml`，新增以下导出配置：

```xml
<export>
  <build_type>ament_cmake</build_type>
  <gazebo_ros gazebo_model_path="${prefix}/.."/>
</export>
```

`gazebo_model_path` 具体取值的原因可见[此问题](https://github.com/ros-simulation/gazebo_ros_pkgs/issues/1500)。只要满足下面两项条件，使用上述值就能工作：

- URDF 中的网格路径采用 `package://package_name/possible_folder/filename.ext` 格式。
- 网格通过 CMake 安装到正确的 share 目录。

<span id="gazebo-plugin"></span>

## Gazebo 插件

要让 ROS 2 与 Gazebo 交互，需要动态链接 ROS 库，由它告诉 Gazebo 应执行什么操作。理论上这种通用机制也允许其他机器人操作系统与 Gazebo 交互，实践中主要用于 ROS。

具体而言，通过新增 URDF 标签链接 ROS 2 Control 库来实现交互。在 URDF 的 `</robot>` 结束标签前添加：

```xml
<ros2_control name="GazeboSystem" type="system">
  <hardware>
    <plugin>gazebo_ros2_control/GazeboSystem</plugin>
  </hardware>
  <joint name="head_swivel" />
</ros2_control>

<gazebo>
  <plugin filename="libgazebo_ros2_control.so" name="gazebo_ros2_control">
    <parameters>$(find urdf_sim_tutorial)/config/09a-minimal.yaml</parameters>
  </plugin>
</gazebo>
```

> `<gazebo>` 和 `<plugin>` 的用法与 ROS 1 相同。最小示例至少需要指定一个关节，之后会继续添加其他关节。

最小配置文件为：

```yaml
controller_manager:
  ros__parameters:
    update_rate: 100
```

完整示例见 [09a-minimal.urdf.xacro](https://github.com/ros/urdf_sim_tutorial/blob/ros2/urdf/09a-minimal.urdf.xacro)，运行方式：

```console
ros2 launch urdf_sim_tutorial 09a-minimal.launch.py
```

这会启动 `/controller_manager` 节点及其 `load_controller` 服务，但暂时还无法与机器人进行有用的交互。为此还需要在控制器 YAML 中补充信息。

<span id="spawning-controllers"></span>

## 启动控制器

连接 ROS 和 Gazebo 后，需要指定在 Gazebo 中运行的 ROS 代码，通常称为控制器。下面基于[这个 YAML 文件](https://github.com/ros/urdf_sim_tutorial/blob/ros2/config/joints.yaml)，配置第一个控制器：

```yaml
controller_manager:
  ros__parameters:
    update_rate: 100
    use_sim_time: true

    joint_state_broadcaster:
      type: joint_state_broadcaster/JointStateBroadcaster
```

该控制器来自 `joint_state_broadcaster` 包，直接从 Gazebo 获取机器人的关节状态并发布到 ROS。

在 [09-joints.launch.py](https://github.com/ros/urdf_sim_tutorial/blob/ros2/launch/09-joints.launch.py) 中，还通过 `ExecuteProcess` 添加了一条 `ros2_control` 命令，启动这个控制器。

可以运行它，但功能仍不完整：

```console
ros2 launch urdf_sim_tutorial 09-joints.launch.py
```

控制器确实会向 `/joint_states` 发布消息，但内容为空：

```yaml
header:
  stamp:
    sec: 13
    nanosec: 331000000
  frame_id: ''
name: []
position: []
velocity: []
effort: []
```

Gazebo 还需要更多关节信息。

<span id="ros-2-control-joint-definitions"></span>

## ROS 2 Control 关节定义

对于每个非固定关节，都需要在 `ros2_control` 标签中添加信息，说明支持哪些接口。先从头部关节开始，将 [URDF](https://github.com/ros/urdf_sim_tutorial/blob/ros2/urdf/10-firsttransmission.urdf.xacro#L241) 中相应关节标签修改为：

```xml
   <joint name="head_swivel">
     <command_interface name="position" />
     <command_interface name="velocity" />
     <state_interface name="position"/>
     <state_interface name="velocity"/>
   </joint>

* Note that the joint name here matches the joint name from the standard URDF ``<joint>`` tag.
* For the moment, let us focus on the ``state_interface``s, in which we specify that we want to publish both position and velocity of this joint.
```

使用之前的启动配置运行该 URDF：

```console
ros2 launch urdf_sim_tutorial 09-joints.launch.py urdf_package_path:=urdf/10-firsttransmission.urdf.xacro
```

现在 `joint_states` 消息包含头部关节，因此 RViz 能正确显示头部：

```yaml
header:
  stamp:
    sec: 4
    nanosec: 707000000
  frame_id: ''
name:
- head_swivel
position:
- -2.9051283156888985e-08
velocity:
- 7.575990694887896e-06
effort:
- .nan
```

我们可以继续为所有非固定关节添加定义，后面也会这样做，让全部关节状态正确发布。不过，除了观察机器人，我们还希望控制它，因此接下来再添加一个控制器。

<span id="joint-control"></span>

## 关节控制

新增的控制器配置见[此文件](https://github.com/ros/urdf_sim_tutorial/blob/ros2/config/head.yaml)：

```yaml
controller_manager:
  ros__parameters:
    # ... snip ...

    head_controller:
      type: position_controllers/JointGroupPositionController

head_controller:
  ros__parameters:
    joints:
      - head_swivel
    interface_name: position
```

它添加名为 `head_controller` 的 `JointGroupPositionController`，并在新的参数命名空间中指定所包含的关节，以及发布的是位置命令。之所以能够控制位置，是因为关节标签中已经声明 `<command_interface name="position" />`。

和前面一样，加入配置及另一条 `ros2 control` 命令后启动：

```console
ros2 launch urdf_sim_tutorial 10-head.launch.py
```

Gazebo 现在订阅了一个新话题，可以通过 ROS 发布数值来控制头部位置：

```console
ros2 topic pub /head_controller/commands std_msgs/msg/Float64MultiArray "data: [-0.707]"
```

发布该命令后，位置会立即变为指定值。

<span id="controlling-multiple-joints-and-mimicking"></span>

## 控制多个关节与联动

可以同样修改夹爪关节的 URDF，但这次将多个关节关联到一个控制器。更新后的 [ROS 参数在这里](https://github.com/ros/urdf_sim_tutorial/blob/ros2/config/gripper.yaml)，还必须[更新 URDF，添加三个关节接口](https://github.com/ros/urdf_sim_tutorial/blob/ros2/urdf/12-gripper.urdf.xacro)。

启动：

```console
ros2 launch urdf_sim_tutorial 12-gripper.launch.py
```

现在可以用三个浮点数组成的数组控制夹爪。张开并伸出：

```console
ros2 topic pub /gripper_controller/commands std_msgs/msg/Float64MultiArray "data: [0.0, 0.5, 0.5]"
```

闭合并缩回：

```console
ros2 topic pub /gripper_controller/commands std_msgs/msg/Float64MultiArray "data: [-0.4, 0.0, 0.0]"
```

实际上，这个夹爪始终需要左右关节的数值相同。可通过以下步骤在 URDF 和控制器中实现联动：

- 在 URDF 的 `right_gripper_joint` 定义中加入 `<mimic joint="left_gripper_joint"/>`，见[此 xacro 中的实现](https://github.com/ros/urdf_sim_tutorial/blob/ros2/urdf/12a-mimic-gripper.urdf.xacro)，其中采用了一个变通方法。
- 在 `right_gripper_joint` 的 `ros2_control` 关节接口中加入 `<param name="mimic">left_gripper_joint</param>`。
- 在新的[控制参数](https://github.com/ros/urdf_sim_tutorial/blob/ros2/config/mimic-gripper.urdf)中，夹爪控制器只列出两个关节，去掉 `right_gripper_joint`。

启动：

```console
ros2 launch urdf_sim_tutorial 12-gripper.launch.py urdf_package_path:=urdf/12a-mimic-gripper.urdf.xacro
```

现在只用两个值就能控制，例如：

```console
ros2 topic pub /gripper_controller/commands std_msgs/msg/Float64MultiArray "data: [0.0, 0.5]"
```

<span id="the-wheels-on-the-droid-go-round-and-round"></span>

## 让机器人的车轮转起来

要驱动机器人移动，先在 [URDF 的 ros2_control 标签](https://github.com/ros/urdf_sim_tutorial/blob/ros2/urdf/13-diffdrive.urdf.xacro)中，为四个车轮分别添加接口；此时只需速度命令接口。

可以为每个车轮分别配置控制器，但我们希望同时控制所有车轮。为此需要配置[更多 ROS 参数](https://github.com/ros/urdf_sim_tutorial/blob/ros2/config/diffdrive.yaml)，使用 `DiffDriveController`。它订阅标准的 Twist 类型 `cmd_vel` 消息，并据此驱动机器人。

```console
ros2 launch urdf_sim_tutorial 13-diffdrive.launch.py
```

除了加载配置，这条命令还会打开 `RobotSteering` 面板，让你驾驶 R2D2，同时观察 Gazebo 中的实际仿真行为和 RViz 中的可视化效果：

![Gazebo 驾驶界面](https://raw.githubusercontent.com/ros/urdf_sim_tutorial/ros2/doc/DrivingInterface.png)

恭喜！现在你已经能使用 URDF 进行机器人仿真了。
