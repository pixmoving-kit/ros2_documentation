---
translation_status: machine_translated
source: Tutorials/Intermediate/URDF/Using-a-URDF-in-Gazebo.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-a-urdf-in-gazebo"></span>

# 在 Gazebo 中使用 URDF

**目标：** 在 Gazebo 模拟器中模拟您的 URDF

**教程级别：** 中级

**用时：** 30分钟

依据 [此 ROS 1 教程](http://wiki.ros.org/urdf/Tutorials/Using%20a%20URDF%20in%20Gazebo).

让我们从安装演示软件包及其依赖性开始。

##### Ubuntu 软件包

``` console
sudo apt install ros-rolling-urdf-sim-tutorial
```

##### RHEL 软件包

``` console
sudo dnf install ros-rolling-urdf-sim-tutorial
```

##### 从源

``` console
git clone https://github.com/ros/urdf_sim_tutorial.git -b ros2
```

<span id="nonfunctional-gazebo-interface"></span>

## 不起作用的 Gazebo 接口

我们可以利用我们已经创造的模型 将它产入加泽博 `gazebo.launch.py`

``` console
ros2 launch urdf_sim_tutorial gazebo.launch.py
```

这个发射文件

> - 装入urdf 从 [宏教程](Using-Xacro-to-Clean-Up-a-URDF-File.md) 并作为一个专题出版(`/robot_description`)
>
> - 发射一个空的加泽波世界
>
> - 运行脚本从话题读取urdf,并在Gazebo产卵.
>
> - 默认情况下, Gazebo GUI 也会被显示, 像这样:

![Gazebo 的无功能机器人](https://raw.githubusercontent.com/ros/urdf_sim_tutorial/ros2/doc/NonFunctional.png)

然而,它却无所作为,并且缺少许多ROS需要使用这个机器人的关键信息。 [其他项目](Building-a-Visual-Robot-Model-with-URDF-from-Scratch.md) [教程](Building-a-Movable-Robot-Model-with-URDF.md) 我们用过 [joint_state_publisher](https://index.ros.org/p/joint_state_publisher/github-ros-joint_state_publisher/) 但是,机器人本身应该在现实世界或在Gazebo中提供这种信息。然而,Gazebo没有具体说明这一点,他不知道会公布这种信息。

要让机器人互动(与你和ROS),我们需要指定两件事:插件和控制器.

<span id="side-note-configuring-meshes"></span>

### 副说明:配置梅舍斯

![机器人有缺失的梅谢](https://raw.githubusercontent.com/ros/urdf_sim_tutorial/ros2/doc/NoMesh.png)

如果你在家跟随自己的机器人,或者有别的东西不合适,那么在Gazebo GUI的模型(即抓手的mezes不存在)中,meshes可能就不见了。 这也可能导致Gazebo在喷洒屏幕出现后需要几秒钟才能启动,因为它正在检查互联网上丢失的模型。

这是因为你的URDF软件包需要明确告诉Gazebo从哪里装入元件。我们通过修改元件来做到这一点。 `package.xml` 我们的URDF meshes所生活的软件包 包括一个新的导出。

``` xml
<export>
  <build_type>ament_cmake</build_type>
  <gazebo_ros gazebo_model_path="${prefix}/.."/>
</export>
```

准确价值背后的推论 `gazebo_model_path` 属性是 [一个单独的问题](https://github.com/ros-simulation/gazebo_ros_pkgs/issues/1500),但仅此而已,将它设定为这一价值将产生以下效果:

> - 您的网格文件名在URDF中使用 `package://package_name/possible_folder/filename.ext` 语法.
>
> - 网格( 通过 CMake) 安装在合适的共享文件夹中 。

<span id="gazebo-plugin"></span>

## Gazebo 插件

要让ROS 2 与 Gazebo 互动,我们必须动态链接 ROS 库,该库将告诉 Gazebo 该怎么做。理论上,这允许其他机器人操作系统以通用方式与 Gazebo 互动。实际上,它只是ROS 。

具体来说,Gazebo / ROS 2的交互全部通过连接到一个ROS 2控制库而发生,并带有新的URDF标记.

在结束之前,我们在乌拉圭国防军中具体列出以下内容: `</robot>` 标签 :

``` xml
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

> **说明**
>
> - 那个... `<gazebo>` 财务报告和财务报告 `<plugin>` 在ROS 1中,标签的工作方式与他们一样.
>
> - 我们至少要指定一个联合点,

最小配置文件是 :

``` yaml
controller_manager:
  ros__parameters:
    update_rate: 100
```

你可以看到这一点 [09a-最小值.urdf.xacro](https://github.com/ros/urdf_sim_tutorial/blob/ros2/urdf/09a-minimal.urdf.xacro) 通过运行

``` console
ros2 launch urdf_sim_tutorial 09a-minimal.launch.py
```

这开始一个 `/controller_manager` 节点和 `load_controller` 服务,但不会立即添加任何与机器人的有用互动。为此,我们需要在控制器 yaml 中指定更多信息。

<span id="spawning-controllers"></span>

## 喷洒控制器

既然我们已经将ROS和Gazebo联系起来,我们需要在Gazebo内部指定一些我们想要运行的ROS代码的位点,我们一般都称之为控制器。 现在我们可以参考一个更大的例子。 [这个 Yaml 文件](https://github.com/ros/urdf_sim_tutorial/blob/ros2/config/joints.yaml) 指定了我们的第一个控制器。

``` yaml
controller_manager:
  ros__parameters:
    update_rate: 100
    use_sim_time: true

    joint_state_broadcaster:
      type: joint_state_broadcaster/JointStateBroadcaster
```

此控制器见于 `joint_state_broadcaster` 将机器人关节的状态直接从Gazebo发布到ROS。

内 [09-joints.launch.py](https://github.com/ros/urdf_sim_tutorial/blob/ros2/launch/09-joints.launch.py) 我们还增加了一个 `ros2_control` 命令通过 `ExecuteProcess` 以启动此特定控制器。

你可以发射这个,但它还没有完全发射出来。

``` console
ros2 launch urdf_sim_tutorial 09-joints.launch.py
```

这将运行控制器, 并且事实上发布于 `/joint_states` 话题,但没有任何内容。

``` yaml
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

你还想要什么 Gazebo? 嗯,它想知道更多的关节信息。

<span id="ros-2-control-joint-definitions"></span>

## ROS 2 控制联合定义

对于每一个非固定关节,我们需要添加有关关节的信息在 `ros2_control` 标签,显示支持什么接口。让我们从头关节开始。修改您的联合标签 [URDF](https://github.com/ros/urdf_sim_tutorial/blob/ros2/urdf/10-firsttransmission.urdf.xacro#L241) 改为:

``` xml
   <joint name="head_swivel">
     <command_interface name="position" />
     <command_interface name="velocity" />
     <state_interface name="position"/>
     <state_interface name="velocity"/>
   </joint>

* Note that the joint name here matches the joint name from the standard URDF ``<joint>`` tag.
* For the moment, let us focus on the ``state_interface``s, in which we specify that we want to publish both position and velocity of this joint.
```

你可以用我们之前的发射配置来运行这个URDF.

``` console
ros2 launch urdf_sim_tutorial 09-joints.launch.py urdf_package_path:=urdf/10-firsttransmission.urdf.xacro
```

现在,头部在RViz中被正确显示,因为头部关节列在 `joint_states` 留言。

``` yaml
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

我们可以继续增加所有非固定关节的共同定义(我们将这样做 ) , 让所有关节都能被正确公布。 但是,生命中不仅仅是看机器人。 我们希望控制机器人。 所以,让我们在这里找到另一个控制器。

<span id="joint-control"></span>

## 联合控制

[在这里,](https://github.com/ros/urdf_sim_tutorial/blob/ros2/config/head.yaml) 我们添加的下一个控制器配置 。

``` yaml
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

在英语中,这表示要增加一个新的 `JointGroupPositionController` 调用 `head_controller`,然后在新的参数命名空间中指定包含哪些关节,以及我们正在发布位置。我们可以这样做,因为我们指定了 `<command_interface name="position" />` 在联合标签。

现在,我们可以启动这个 与添加的配置和另一个 `ros2 control` 命令和以前一样

``` console
ros2 launch urdf_sim_tutorial 10-head.launch.py
```

现在Gazebo被订阅为新话题,然后可以通过在ROS中发布一个值来控制头的位置.

``` console
ros2 topic pub /head_controller/commands std_msgs/msg/Float64MultiArray "data: [-0.707]"
```

当此命令发布时, 位置会立即更改为指定的值 。

<span id="controlling-multiple-joints-and-mimicking"></span>

## 控制多个关节和模仿

我们可以以类似方式改变格利珀关节的URDF,但在这种情况下,我们将将多个关节与一个控制器联系起来。 [ROS 参数在这里](https://github.com/ros/urdf_sim_tutorial/blob/ros2/config/gripper.yaml)。我们还必须更新 [URDF将包含三个额外的联合接口](https://github.com/ros/urdf_sim_tutorial/blob/ros2/urdf/12-gripper.urdf.xacro).

为了启动这个,

``` console
ros2 launch urdf_sim_tutorial 12-gripper.launch.py
```

我们现在可以用三个浮点数组来移动控制器。打开并退出 :

``` console
ros2 topic pub /gripper_controller/commands std_msgs/msg/Float64MultiArray "data: [0.0, 0.5, 0.5]"
```

关闭并收回:

``` console
ros2 topic pub /gripper_controller/commands std_msgs/msg/Float64MultiArray "data: [-0.4, 0.0, 0.0]"
```

这个握手器的设置方式让我们ALWAYS希望左握手器关节具有与右握手器关节相同的值。我们可以用几个步骤将它编码到URDF和控制器中。

> - 插入 `<mimic joint="left_gripper_joint"/>` 纳入《关于土地利用、土地利用的变化和林业的 `right_gripper_joint` (这是做 有点黑客英寸 [这儿的xacro](https://github.com/ros/urdf_sim_tutorial/blob/ros2/urdf/12a-mimic-gripper.urdf.xacro)
>
> - 插入 `<param name="mimic">left_gripper_joint</param>` 输入 `ros2_control` 联合接口 `right_gripper_joint`.
>
> - 在我们的新生活里 [控制参数](https://github.com/ros/urdf_sim_tutorial/blob/ros2/config/mimic-gripper.urdf),我们只列出两个关节 用于抓手控制器, 省去 `right_gripper_joint`.

我們可以用這個發射

``` console
ros2 launch urdf_sim_tutorial 12-gripper.launch.py urdf_package_path:=urdf/12a-mimic-gripper.urdf.xacro
```

而现在我们只能用两个值来控制它,例如.

``` console
ros2 topic pub /gripper_controller/commands std_msgs/msg/Float64MultiArray "data: [0.0, 0.5]"
```

<span id="the-wheels-on-the-droid-go-round-and-round"></span>

## 机器人的轮子 转转转转

要驱动机器人,我们首先必须指定更多的界面 `ros2_control` 标记为 [四个轮子的URDF](https://github.com/ros/urdf_sim_tutorial/blob/ros2/urdf/13-diffdrive.urdf.xacro)然而,现在只需要速度指令接口.

我们可以为每个车轮指定控制器,但其中的乐趣在哪里? 相反,我们要一起控制所有车轮。为此,我们需要 [更多 ROS 参数](https://github.com/ros/urdf_sim_tutorial/blob/ros2/config/diffdrive.yaml) 利用《京都议定书》 `DiffDriveController` 用于订阅标准扭矩 `cmd_vel` 并相应移动机器人。

``` console
ros2 launch urdf_sim_tutorial 13-diffdrive.launch.py
```

除了加载上述配置外,这还打开了 `RobotSteering` 面板,允许您驱动 R2D2 机器人周围, 同时观察它的实际行为( 在 Gazebo) 和它可视化的行为( 在 RViz 中):

![Gazebo 有驱动接口](https://raw.githubusercontent.com/ros/urdf_sim_tutorial/ros2/doc/DrivingInterface.png)

恭喜你们,现在你们正在用URDF模拟机器人。
