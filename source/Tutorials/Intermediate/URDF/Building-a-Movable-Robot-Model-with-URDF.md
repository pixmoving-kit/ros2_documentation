---
translation_status: machine_translated
source: Tutorials/Intermediate/URDF/Building-a-Movable-Robot-Model-with-URDF.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="building-a-movable-robot-model"></span> <span id="moveableurdf"></span>

# 构建可运动的机器人模型

**目标：** 学习如何定义URDF中的可移动关节.

**教程级别：** 中级

**用时：** 10分钟

校对:Portnoy 校对:Soup [上一个教程](Building-a-Visual-Robot-Model-with-URDF-from-Scratch.md) 在前一种模式中,所有关节都已经固定。 现在我们将探索另外三种重要的关节类型:连续的、转动的和棱角的。

在继续之前,请确定您已经安装了所有先决条件。 [上一个教程](Building-a-Visual-Robot-Model-with-URDF-from-Scratch.md) (d) 提供所需信息。

同样,本教程中提到的所有机器人模型都可以在 [urdf_tutorial](https://index.ros.org/p/urdf_tutorial) 软件包。

[这儿是新来的urdf](https://github.com/ros/urdf_tutorial/blob/ros2/urdf/06-flexible.urdf) 与前一个版本相比,我们可以看到所有的变化,但我们只关注三个实例。

要可视化并控制此模型, 运行与上一个教程相同的命令 :

``` console
$ ros2 launch urdf_tutorial display.launch.py model:=urdf/06-flexible.urdf
```

然而,现在这也将出现一个图形界面,允许您控制所有非固定关节的值。 播放一些模型,看看它是如何移动的。 然后,我们可以看看我们是如何完成的 。

[![灵活模型的截图](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/flexible.png)](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/flexible.png) <span id="the-head"></span>

## 团长

``` xml
<joint name="head_swivel" type="continuous">
  <parent link="base_link"/>
  <child link="head"/>
  <axis xyz="0 0 1"/>
  <origin xyz="0 0 0.3"/>
</joint>
```

机身与头部的连接是一个连续的关节,意思是它可以从负无穷到正无穷的任意角度上进行,轮子也是像这样的模型,这样它们就可以永远地向两个方向滚动.

我们必须增加的唯一额外信息是旋转轴,这里由xyz三进制指定,它指定了头围绕旋转的向量。既然我们希望它绕到z轴上,我们就指定了向量“0 0 1”。

<span id="the-gripper"></span>

## 灰熊队

``` xml
<joint name="left_gripper_joint" type="revolute">
  <axis xyz="0 0 1"/>
  <limit effort="1000.0" lower="0.0" upper="0.548" velocity="0.5"/>
  <origin rpy="0 0 0" xyz="0.2 0.01 0"/>
  <parent link="gripper_pole"/>
  <child link="left_gripper"/>
</joint>
```

左右抓动关节的模型是转动关节。这意味着它们与连续关节的旋转方式相同,但有严格的限制。 因此,我们必须包括指定关节的上下限(弧度)的极限标记。 我们还必须为这个关节指定一个最大速度和努力,但实际值对我们这里的目的来说并不重要。

<span id="the-gripper-arm"></span>

## 猛兽臂

``` xml
<joint name="gripper_extension" type="prismatic">
  <parent link="base_link"/>
  <child link="gripper_pole"/>
  <limit effort="1000.0" lower="-0.38" upper="0" velocity="0.5"/>
  <origin rpy="0 0 0" xyz="0.19 0 0.2"/>
</joint>
```

握手臂是另一种关节,即棱柱关节。这意味着它沿着轴线移动,而不是绕着它移动。这种翻译运动使我们的机器人模型能够伸展和收回握手臂。

棱臂的限度与折叠关节相同,但单位为米,而非弧度.

<span id="other-types-of-joints"></span>

## 其他类型的联合企业

还有另外两种关节在空间中移动。 棱关节只能沿着一个维度移动,而一个平面或两个维度则可以移动。 此外,一个浮关节不受约束,可以在三个维度中任意移动。 这些关节不能只用一个数字来指定,因此不包含在这个教程中。

<span id="specifying-the-pose"></span>

## 指定 pose 中

当您在图形界面中移动滑动器时, 模型会在 Rviz 中移动。 如何完成 ? [图形界面](https://index.ros.org/p/joint_state_publisher_gui) 解析 URDF 并找到所有非固定关节及其限制。然后,它使用滑动器的值来发布 [sensor_msgs/msg/JointState](https://github.com/ros2/common_interfaces/blob/eloquent/sensor_msgs/msg/JointState.msg) 消息。然后这些信息被 [robot_state_publisher](https://index.ros.org/p/robot_state_publisher) 用于计算不同部分之间的所有变换。然后使用所产生的变换树来显示Rviz中的所有形状。

<span id="next-steps"></span>

## 后续步骤

现在你有了明显的功能模型,你可以 [在一些物理属性中添加](Adding-Physical-and-Collision-Properties-to-a-URDF-Model.md),或 [开始使用xacro来简化代码](Using-Xacro-to-Clean-Up-a-URDF-File.md).
