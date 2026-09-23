---
translation_status: machine_translated
source: Tutorials/Intermediate/URDF/Building-a-Visual-Robot-Model-with-URDF-from-Scratch.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="building-a-visual-robot-model-from-scratch"></span> <span id="buildingurdf"></span>

# 从零构建机器人可视模型

**目标：** 学习如何构建一个在Rviz可以查看的机器人的视觉模型

**教程级别：** 中级

**用时：** 20分钟

> **说明**
>
> 此教程假设您知道如何写出格式化好的 XML 代码

在这个教程中,我们将建立一个机器人的视觉模型,这个模型看起来模糊的像R2D2. 在后来的教程中,你会学会如何 [表达模式](Building-a-Movable-Robot-Model-with-URDF.md), [在一些物理属性中添加](Adding-Physical-and-Collision-Properties-to-a-URDF-Model.md),以及 [用 xacro 生成更整洁的代码](Using-Xacro-to-Clean-Up-a-URDF-File.md)但现在,我们将专注于 使视觉几何学正确。

在继续之前,确保你拥有 [joint_state_publisher](https://index.ros.org/p/joint_state_publisher) 软件包已安装。如果安装的话 [urdf_tutorial](https://index.ros.org/p/urdf_tutorial) 二进制,应该已经是这样了。如果没有,请更新您的安装以包含该软件包(使用) `rosdep` 以检查).

此教程( 和源文件) 中提及的所有机器人模型都可见于 [urdf_tutorial](https://index.ros.org/p/urdf_tutorial) 软件包。

<span id="one-shape"></span>

## 一个形状

首先,我们只是探索一个简单的形状。 这里的简单程度与你所能做到的一样。 [\[來源請求:01-myfirst.urdf\] (中文(简体) ).](https://github.com/ros/urdf_tutorial/blob/ros2/urdf/01-myfirst.urdf)

``` xml
<?xml version="1.0"?>
<robot name="myfirst">
  <link name="base_link">
    <visual>
      <geometry>
        <cylinder length="0.6" radius="0.2"/>
      </geometry>
    </visual>
  </link>
</robot>
```

要将 XML 翻译为英文, 这是一个有这个名字的机器人 `myfirst`,它只包含一个链接(a.k.a. part),其视觉组件只是一个长0.6米,半径为0.2米的圆柱。对于一个简单的“Hello World”类型例子来说,这似乎是许多附加标记。

为了检查模型,发射 `display.launch.py` 文件 :

``` console
$ ros2 launch urdf_tutorial display.launch.py model:=urdf/01-myfirst.urdf
```

这有三件事:

> - 装入指定的模型并将其保存为参数 `robot_state_publisher` 节点。
>
> - 运行要公布的节点 [sensor_msgs/msg/JointState](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/JointState.msg) 并变换(稍后更多关于这些)
>
> - 以配置文件启动 Rviz

发射后 `display.launch.py`,最后你应该通过 RViz 显示如下:

[![我的第一个形象](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/myfirst.png)](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/myfirst.png)

需要指出的是:  
- 固定框架是电网中心所在的变换框架。 这里,它是一个由我们唯一的链接(base_link)定义的框架。

- 视觉元件(圆柱)作为默认的起源于其几何学的中心,因此,半圆柱低于网格。

<span id="multiple-shapes"></span>

## 多个形状

现在让我们看看如何添加多个形状/链接。 如果我们只是给urdf添加更多的链接元素,解析器就不会知道把它们放在哪里。 因此,我们必须添加关节。 联合元素既可以指弹性关节,也可以指不弹性关节。 我们从不弹性或固定关节开始。 [\[來源:02-multipleshapes.urdf\] (中文(中国大陆) ).](https://github.com/ros/urdf_tutorial/blob/ros2/urdf/02-multipleshapes.urdf)

``` xml
<?xml version="1.0"?>
<robot name="multipleshapes">
  <link name="base_link">
    <visual>
      <geometry>
        <cylinder length="0.6" radius="0.2"/>
      </geometry>
    </visual>
  </link>

  <link name="right_leg">
    <visual>
      <geometry>
        <box size="0.6 0.1 0.2"/>
      </geometry>
    </visual>
  </link>

  <joint name="base_to_right_leg" type="fixed">
    <parent link="base_link"/>
    <child link="right_leg"/>
  </joint>

</robot>
```

- 注意我们如何定义一个0.6米x0.1米x0.2米的盒子

- 关节是用父母和孩子来定义的。 UNDF 最终是一个树结构,有一个根链。 这意味着腿的位置取决于基位_link的位置。

``` console
$ ros2 launch urdf_tutorial display.launch.py model:=urdf/02-multipleshapes.urdf
```

[![多个形状](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/multipleshapes.png)](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/multipleshapes.png)

两者的形状相互重叠,因为它们有着相同的起源。如果我们想要它们不重叠,我们必须界定更多的起源。

<span id="origins"></span>

## 来源

R2D2的腿部紧贴着他的躯干上半部。 因此,我们指定了JOINT的起源。 此外,它不紧贴在腿部中部,它紧贴在上部,因此我们也必须抵消腿部的起源。 我们还在旋转腿部,使其直立。 [\[來源:03发源地.urdf\].](https://github.com/ros/urdf_tutorial/blob/ros2/urdf/03-origins.urdf)

``` xml
<?xml version="1.0"?>
<robot name="origins">
  <link name="base_link">
    <visual>
      <geometry>
        <cylinder length="0.6" radius="0.2"/>
      </geometry>
    </visual>
  </link>

  <link name="right_leg">
    <visual>
      <geometry>
        <box size="0.6 0.1 0.2"/>
      </geometry>
      <origin rpy="0 1.57075 0" xyz="0 0 -0.3"/>
    </visual>
  </link>

  <joint name="base_to_right_leg" type="fixed">
    <parent link="base_link"/>
    <child link="right_leg"/>
    <origin xyz="0 -0.22 0.25"/>
  </joint>

</robot>
```

- 首先,让我们检查关节的起源。它是由父母的参考框架定义的。 因此,我们在y方向(左侧0.22米,但右侧相对轴)和Z方向(上方)0.25米。 这意味着无论儿童链接的视觉来源标记如何,儿童链接的起源都将是上向的。 由于我们没有指定rpy(roll pitch raw)属性,因此儿童帧默认方向与父母框架相同。

- 现在,看腿的视觉源头,它有xyz和rpy的偏移。这决定了视觉元素的中心位置,相对于其起源。既然我们希望腿部在顶部紧贴,那么我们通过将z的偏移设为-0.3米来抵消源头。既然我们希望腿部的长部与z轴平行,我们就将PI/2的视觉部分围绕Y轴旋转。

``` console
$ ros2 launch urdf_tutorial display.launch.py model:=urdf/03-origins.urdf
```

[![源代码截图](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/origins.png)](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/origins.png)

- 发射文件运行的软件包将基于您的 URDF 为您的模型中的每个链接创建 TF 框架。 Rviz 使用此信息来找出显示每个形状的地方 。

- 如果给定的URDF链接不存在一个TF框架,那么它将被放置在原产地白色([有关问题](http://answers.ros.org/question/207947/how-do-you-use-externally-defined-materials-in-a-urdfxacro-file/)).

<span id="material-girl"></span>

## 物质女孩

“好吧,”我听到你说 : “ 这非常可爱,但并不是每个人都拥有B21。 我的机器人和R2D2不是红色的! ” 。 说得好。 让我们来看看材料标签。 [\[來源:04-materials.urdf\] (中文(简体) ).](https://github.com/ros/urdf_tutorial/blob/ros2/urdf/04-materials.urdf)

``` xml
<?xml version="1.0"?>
<robot name="materials">

  <material name="blue">
    <color rgba="0 0 0.8 1"/>
  </material>

  <material name="white">
    <color rgba="1 1 1 1"/>
  </material>

  <link name="base_link">
    <visual>
      <geometry>
        <cylinder length="0.6" radius="0.2"/>
      </geometry>
      <material name="blue"/>
    </visual>
  </link>

  <link name="right_leg">
    <visual>
      <geometry>
        <box size="0.6 0.1 0.2"/>
      </geometry>
      <origin rpy="0 1.57075 0" xyz="0 0 -0.3"/>
      <material name="white"/>
    </visual>
  </link>

  <joint name="base_to_right_leg" type="fixed">
    <parent link="base_link"/>
    <child link="right_leg"/>
    <origin xyz="0 -0.22 0.25"/>
  </joint>

  <link name="left_leg">
    <visual>
      <geometry>
        <box size="0.6 0.1 0.2"/>
      </geometry>
      <origin rpy="0 1.57075 0" xyz="0 0 -0.3"/>
      <material name="white"/>
    </visual>
  </link>

  <joint name="base_to_left_leg" type="fixed">
    <parent link="base_link"/>
    <child link="left_leg"/>
    <origin xyz="0 0.22 0.25"/>
  </joint>

</robot>
```

- 身体现在是蓝色的。我们已经定义了一种名为“蓝色”的新材料,红、绿、蓝和α通道分别被定义为0,0,0.8和1。所有值都可以在\[0,1\] 范围内。然后,这种材料被碱基_link的视觉元素所引用。白色材料的定义类似。

- 您也可以从视觉元素内部定义材料标记, 甚至可以在其他链接中引用它。 如果您重新定义的话, 甚至没有人会抱怨 。

- 您也可以使用纹理指定用于给对象配色的图像文件

``` console
$ ros2 launch urdf_tutorial display.launch.py model:=urdf/04-materials.urdf
```

[![材料截图](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/materials.png)](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/materials.png) <span id="finishing-the-model"></span>

## 完成模式

现在,我们用更多的形状来完成模型:脚、轮子和头部。最显著的是,我们增加了一个球体和一些网点。 我们还会增加几个其它的碎片,我们稍后会使用这些碎片。 [\[來源請求:05-visual.urdf\] (中文(简体) ).](https://github.com/ros/urdf_tutorial/blob/ros2/urdf/05-visual.urdf)

``` xml
<?xml version="1.0"?>
<robot name="visual">

  <material name="blue">
    <color rgba="0 0 0.8 1"/>
  </material>
  <material name="black">
    <color rgba="0 0 0 1"/>
  </material>
  <material name="white">
    <color rgba="1 1 1 1"/>
  </material>

  <link name="base_link">
    <visual>
      <geometry>
        <cylinder length="0.6" radius="0.2"/>
      </geometry>
      <material name="blue"/>
    </visual>
  </link>

  <link name="right_leg">
    <visual>
      <geometry>
        <box size="0.6 0.1 0.2"/>
      </geometry>
      <origin rpy="0 1.57075 0" xyz="0 0 -0.3"/>
      <material name="white"/>
    </visual>
  </link>

  <joint name="base_to_right_leg" type="fixed">
    <parent link="base_link"/>
    <child link="right_leg"/>
    <origin xyz="0 -0.22 0.25"/>
  </joint>

  <link name="right_base">
    <visual>
      <geometry>
        <box size="0.4 0.1 0.1"/>
      </geometry>
      <material name="white"/>
    </visual>
  </link>

  <joint name="right_base_joint" type="fixed">
    <parent link="right_leg"/>
    <child link="right_base"/>
    <origin xyz="0 0 -0.6"/>
  </joint>

  <link name="right_front_wheel">
    <visual>
      <origin rpy="1.57075 0 0" xyz="0 0 0"/>
      <geometry>
        <cylinder length="0.1" radius="0.035"/>
      </geometry>
      <material name="black"/>
    </visual>
  </link>
  <joint name="right_front_wheel_joint" type="fixed">
    <parent link="right_base"/>
    <child link="right_front_wheel"/>
    <origin rpy="0 0 0" xyz="0.133333333333 0 -0.085"/>
  </joint>

  <link name="right_back_wheel">
    <visual>
      <origin rpy="1.57075 0 0" xyz="0 0 0"/>
      <geometry>
        <cylinder length="0.1" radius="0.035"/>
      </geometry>
      <material name="black"/>
    </visual>
  </link>
  <joint name="right_back_wheel_joint" type="fixed">
    <parent link="right_base"/>
    <child link="right_back_wheel"/>
    <origin rpy="0 0 0" xyz="-0.133333333333 0 -0.085"/>
  </joint>

  <link name="left_leg">
    <visual>
      <geometry>
        <box size="0.6 0.1 0.2"/>
      </geometry>
      <origin rpy="0 1.57075 0" xyz="0 0 -0.3"/>
      <material name="white"/>
    </visual>
  </link>

  <joint name="base_to_left_leg" type="fixed">
    <parent link="base_link"/>
    <child link="left_leg"/>
    <origin xyz="0 0.22 0.25"/>
  </joint>

  <link name="left_base">
    <visual>
      <geometry>
        <box size="0.4 0.1 0.1"/>
      </geometry>
      <material name="white"/>
    </visual>
  </link>

  <joint name="left_base_joint" type="fixed">
    <parent link="left_leg"/>
    <child link="left_base"/>
    <origin xyz="0 0 -0.6"/>
  </joint>

  <link name="left_front_wheel">
    <visual>
      <origin rpy="1.57075 0 0" xyz="0 0 0"/>
      <geometry>
        <cylinder length="0.1" radius="0.035"/>
      </geometry>
      <material name="black"/>
    </visual>
  </link>
  <joint name="left_front_wheel_joint" type="fixed">
    <parent link="left_base"/>
    <child link="left_front_wheel"/>
    <origin rpy="0 0 0" xyz="0.133333333333 0 -0.085"/>
  </joint>

  <link name="left_back_wheel">
    <visual>
      <origin rpy="1.57075 0 0" xyz="0 0 0"/>
      <geometry>
        <cylinder length="0.1" radius="0.035"/>
      </geometry>
      <material name="black"/>
    </visual>
  </link>
  <joint name="left_back_wheel_joint" type="fixed">
    <parent link="left_base"/>
    <child link="left_back_wheel"/>
    <origin rpy="0 0 0" xyz="-0.133333333333 0 -0.085"/>
  </joint>

  <joint name="gripper_extension" type="fixed">
    <parent link="base_link"/>
    <child link="gripper_pole"/>
    <origin rpy="0 0 0" xyz="0.19 0 0.2"/>
  </joint>

  <link name="gripper_pole">
    <visual>
      <geometry>
        <cylinder length="0.2" radius="0.01"/>
      </geometry>
      <origin rpy="0 1.57075 0 " xyz="0.1 0 0"/>
    </visual>
  </link>

  <joint name="left_gripper_joint" type="fixed">
    <origin rpy="0 0 0" xyz="0.2 0.01 0"/>
    <parent link="gripper_pole"/>
    <child link="left_gripper"/>
  </joint>

  <link name="left_gripper">
    <visual>
      <origin rpy="0.0 0 0" xyz="0 0 0"/>
      <geometry>
        <mesh filename="package://urdf_tutorial/meshes/l_finger.dae"/>
      </geometry>
    </visual>
  </link>

  <joint name="left_tip_joint" type="fixed">
    <parent link="left_gripper"/>
    <child link="left_tip"/>
  </joint>

  <link name="left_tip">
    <visual>
      <origin rpy="0.0 0 0" xyz="0.09137 0.00495 0"/>
      <geometry>
        <mesh filename="package://urdf_tutorial/meshes/l_finger_tip.dae"/>
      </geometry>
    </visual>
  </link>
  <joint name="right_gripper_joint" type="fixed">
    <origin rpy="0 0 0" xyz="0.2 -0.01 0"/>
    <parent link="gripper_pole"/>
    <child link="right_gripper"/>
  </joint>

  <link name="right_gripper">
    <visual>
      <origin rpy="-3.1415 0 0" xyz="0 0 0"/>
      <geometry>
        <mesh filename="package://urdf_tutorial/meshes/l_finger.dae"/>
      </geometry>
    </visual>
  </link>

  <joint name="right_tip_joint" type="fixed">
    <parent link="right_gripper"/>
    <child link="right_tip"/>
  </joint>

  <link name="right_tip">
    <visual>
      <origin rpy="-3.1415 0 0" xyz="0.09137 0.00495 0"/>
      <geometry>
        <mesh filename="package://urdf_tutorial/meshes/l_finger_tip.dae"/>
      </geometry>
    </visual>
  </link>

  <link name="head">
    <visual>
      <geometry>
        <sphere radius="0.2"/>
      </geometry>
      <material name="white"/>
    </visual>
  </link>
  <joint name="head_swivel" type="fixed">
    <parent link="base_link"/>
    <child link="head"/>
    <origin xyz="0 0 0.3"/>
  </joint>

  <link name="box">
    <visual>
      <geometry>
        <box size="0.08 0.08 0.08"/>
      </geometry>
      <material name="blue"/>
    </visual>
  </link>

  <joint name="tobox" type="fixed">
    <parent link="head"/>
    <child link="box"/>
    <origin xyz="0.1814 0 0.1414"/>
  </joint>
</robot>
```

``` console
$ ros2 launch urdf_tutorial display.launch.py model:=urdf/05-visual.urdf
```

[![视觉截图](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/visual.png)](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/visual.png)

如何增加这个领域应相当不言自明:

``` xml
<link name="head">
  <visual>
    <geometry>
      <sphere radius="0.2"/>
    </geometry>
    <material name="white"/>
  </visual>
</link>
```

这里的元件是从 PR2 中借来的, 是需要指定路径的单独文件 。 您应该使用 `package://NAME_OF_PACKAGE/path` 标记。此教程的网格位于 `urdf_tutorial` 包,在一个名为 meshes 的文件夹中。

``` xml
<link name="left_gripper">
  <visual>
    <origin rpy="0.0 0 0" xyz="0 0 0"/>
    <geometry>
      <mesh filename="package://urdf_tutorial/meshes/l_finger.dae"/>
    </geometry>
  </visual>
</link>
```

- 网格可以以若干不同格式导入。 STL 相当常见, 但引擎也支持 DAE, 它可以有自己的颜色数据, 意思是您不必指定颜色/ 材料。 通常这些是单独的文件。 这些网格引用了 。 `.tif` 文件夹中的文件 。

- Meshes也可以使用相对的缩放参数或边框大小来进行尺寸.

- 我们还可以在完全不同的一揽子方案中提及混血儿。

有了 类似R2D2的URDF模型 现在你可以继续下一步了 [让它动起来](Building-a-Movable-Robot-Model-with-URDF.md).
