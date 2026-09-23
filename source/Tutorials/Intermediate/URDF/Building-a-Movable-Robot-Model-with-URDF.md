<span id="building-a-movable-robot-model"></span> <span id="moveableurdf"></span>

# 构建可运动的机器人模型

**目标：** 学习在 URDF 中定义可动关节。

**教程级别：** 中级

**预计耗时：** 10 分钟

本教程将修改[上一篇教程](Building-a-Visual-Robot-Model-with-URDF-from-Scratch.md)中的 R2D2 模型，为它加入可动关节。此前所有关节都是固定的，现在将介绍另外三种重要关节：连续旋转关节（continuous）、转动关节（revolute）和平移关节（prismatic）。

继续之前，请确认已安装全部必需组件，具体要求见[上一篇教程](Building-a-Visual-Robot-Model-with-URDF-from-Scratch.md)。本教程涉及的所有模型同样位于 [urdf_tutorial](https://index.ros.org/p/urdf_tutorial) 包中。

[新的 URDF](https://github.com/ros/urdf_tutorial/blob/ros2/urdf/06-flexible.urdf)包含可动关节。可与上一版本对比以查看全部改动；这里重点介绍三个关节示例。

使用与上篇教程相同形式的命令来显示和控制模型：

```console
$ ros2 launch urdf_tutorial display.launch.py model:=urdf/06-flexible.urdf
```

这次还会弹出一个 GUI，用于控制所有非固定关节的数值。先操作模型观察其运动，再看看实现方法。

![可运动模型截图](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/flexible.png)

<span id="the-head"></span>

## 头部

```xml
<joint name="head_swivel" type="continuous">
  <parent link="base_link"/>
  <child link="head"/>
  <axis xyz="0 0 1"/>
  <origin xyz="0 0 0.3"/>
</joint>
```

身体与头部之间采用连续旋转关节，它可以取从负无穷到正无穷的任意角度。车轮也采用这种关节，因此能够向两个方向无限旋转。

唯一需要补充的信息是旋转轴，通过 xyz 三元组指定头部绕其旋转的向量。这里希望绕 z 轴旋转，因此指定 `0 0 1`。

<span id="the-gripper"></span>

## 夹爪

```xml
<joint name="left_gripper_joint" type="revolute">
  <axis xyz="0 0 1"/>
  <limit effort="1000.0" lower="0.0" upper="0.548" velocity="0.5"/>
  <origin rpy="0 0 0" xyz="0.2 0.01 0"/>
  <parent link="gripper_pole"/>
  <child link="left_gripper"/>
</joint>
```

左右夹爪都采用转动关节。它们像连续旋转关节一样旋转，但具有严格的范围限制。因此必须包含 `limit` 标签，指定关节角度的上下限，单位为弧度。还必须指定最大速度和力矩，不过本教程暂不关心这些数值的实际大小。

<span id="the-gripper-arm"></span>

## 夹爪臂

```xml
<joint name="gripper_extension" type="prismatic">
  <parent link="base_link"/>
  <child link="gripper_pole"/>
  <limit effort="1000.0" lower="-0.38" upper="0" velocity="0.5"/>
  <origin rpy="0 0 0" xyz="0.19 0 0.2"/>
</joint>
```

夹爪臂采用平移关节，沿着一条轴移动，而非绕轴旋转。这种平移运动使机器人可以伸出和收回夹爪臂。

平移关节的限制与转动关节的指定方式相同，只是单位为米而不是弧度。

<span id="other-types-of-joints"></span>

## 其他关节类型

还有两种能在空间中运动的关节。平移关节只能沿一个维度移动，平面关节（planar）则可以在二维平面内运动；浮动关节（floating）不受约束，可在三维空间中运动。这些关节无法仅用一个数值描述，因此不在本教程范围内。

<span id="specifying-the-pose"></span>

## 指定位姿

拖动 GUI 中的滑块时，RViz 中的模型会随之运动。首先，[GUI](https://index.ros.org/p/joint_state_publisher_gui) 解析 URDF，找出全部非固定关节及其限制，再根据滑块数值发布 [sensor_msgs/msg/JointState](https://github.com/ros2/common_interfaces/blob/eloquent/sensor_msgs/msg/JointState.msg) 消息。[robot_state_publisher](https://index.ros.org/p/robot_state_publisher) 使用这些消息计算各部件之间的变换，最终得到的变换树用于在 RViz 中显示所有形状。

<span id="next-steps"></span>

## 后续步骤

现在已有一个可以显示和运动的模型，接下来可以[添加物理属性](Adding-Physical-and-Collision-Properties-to-a-URDF-Model.md)，或[使用 Xacro 简化代码](Using-Xacro-to-Clean-Up-a-URDF-File.md)。
