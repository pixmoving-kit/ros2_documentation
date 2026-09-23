---
translation_status: machine_translated
source: Tutorials/Intermediate/URDF/Adding-Physical-and-Collision-Properties-to-a-URDF-Model.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="adding-physical-and-collision-properties"></span> <span id="urdfproperties"></span>

# 添加物理与碰撞属性

**目标：** 学习如何将碰撞和惯性属性加到链接中,以及如何将联合动力学加到关节中.

**教程级别：** 中级

**用时：** 10分钟

如何在您的URDF模型中添加一些基本物理属性, 以及如何指定其碰撞属性。

<span id="collision"></span>

## 碰撞

目前为止,我们只指明了我们与单一子元素的联系, `visual`,它定义了(并不奇怪)机器人的外观。然而,为了让碰撞探测工作发挥作用或模拟机器人,我们需要定义一个 `collision` 元素也一样。 [这儿是新来的urdf](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/urdf/07-physics.urdf) 与碰撞和物理特性。

这是我们新的基础链接的代码。

``` xml
<link name="base_link">
    <visual>
      <geometry>
        <cylinder length="0.6" radius="0.2"/>
      </geometry>
      <material name="blue">
        <color rgba="0 0 .8 1"/>
      </material>
    </visual>
    <collision>
      <geometry>
        <cylinder length="0.6" radius="0.2"/>
      </geometry>
    </collision>
  </link>
```

- 碰撞元素是链接对象的直接子元素,与视觉标记处于同一水平.

- 碰撞元件定义其形状与视觉元件一样,带有几何标记,这里的几何标记格式与视觉完全相同.

- 也可以以与碰撞标记的子元件相同的方式(如视觉)指定一个源头.

在很多情况下,你会希望碰撞几何和源头与视觉几何和源头完全相同。 但是,有两种主要情况你不会:

> - **快速处理** 对两个网格进行碰撞探测比两个简单的几何数据要复杂得多。因此,你可能想用碰撞元素中更简单的几何数据来取代网格。
>
> - **安全区** 您可能想要限制接近敏感设备的移动。 比如,如果我们不想让R2D2头部发生碰撞,我们可能会将碰撞几何定义为一个缠住他头部的圆柱,以防止任何东西过于靠近他的头部。

<span id="physical-properties"></span>

## 物理属性

为了让你的模型能够正确模拟,你需要定义你的机器人的几个物理属性,即像Gazebo这样的物理引擎所需要的属性.

<span id="inertia"></span>

### 因诺蒂亚

每个被模拟的链接元素都需要一个惯性标记。 这里有一个简单的标记 。

``` xml
<link name="base_link">
  <visual>
    <geometry>
      <cylinder length="0.6" radius="0.2"/>
    </geometry>
    <material name="blue">
      <color rgba="0 0 .8 1"/>
    </material>
  </visual>
  <collision>
    <geometry>
      <cylinder length="0.6" radius="0.2"/>
    </geometry>
  </collision>
  <inertial>
    <mass value="10"/>
    <inertia ixx="1e-3" ixy="0.0" ixz="0.0" iyy="1e-3" iyz="0.0" izz="1e-3"/>
  </inertial>
</link>
```

- 此元素也是链接对象的子元素.

- 质量以公斤计算。

- 3x3 旋转惯性矩阵与惯性元素一起指定,由于它是对称的,因此它只能由6个元素来表示,因此如此.

  > |          |                |                |
  > |----------|----------------|----------------|
  > | **页:1** | **十六岁**     | **ưμ㼯A**      |
  > | 十六岁   | **iyy (法语)** | **iyz (英语)** |
  > | ưμ㼯A    | iyz (英语)     | **头晕**       |

- 可以通过MeshLab等模拟程序向您提供这种信息。 几何原始物的惯性(圆柱、框、球)可以使用维基百科计算。 [惯性分数列表](https://en.wikipedia.org/wiki/List_of_moments_of_inertia#List_of_3D_inertia_tensors) (并用于以上例中).

- 惯性拉伸度取决于物体的质量与质量的分布。好的第一近似值是在物体的体积中假定质量的均匀分布,并根据物体的形状计算惯性拉伸度,如上所述。

- 如果无法确定要放置什么, ixx/ iyy/ izz=1e-3 或更小的矩阵, 通常是中等尺寸链接的合理默认值( 它对应一个0. 1 m 的边长, 质量为 0. 6 kg) 。 身份矩阵是一个特别糟糕的选择, 因为它往往太高 。 ( 它对应一个 0. 1 m 的边长, 质量为 600  kg 的框 !)

- 也可以指定一个来源标记来指定重心和惯性参考框架(与链接的参考框架有关).

- 在使用实时控制器时,零(或几乎零)的惯性元素可以导致机器人模型无预警地崩溃,所有链接都会随其起源与世界起源同步出现.

<span id="contact-coefficients"></span>

### 联系系数

您也可以定义链接在相互接触时的表现方式。 使用名为 compact_coects 的碰撞标记的子元件进行此操作。 有三种属性需要指定 :

> - 吴... [滑动系数](https://simple.wikipedia.org/wiki/Coefficient_of_friction)
>
> - 键 - [恒温系数](https://en.wikipedia.org/wiki/Stiffness)
>
> - 克达 - [沉积系数](https://en.wikipedia.org/wiki/Damping_ratio#Damping_ratio_definition)

<span id="joint-dynamics"></span>

### 联合动态

关节动作如何由关节的动态标记定义。 这里有两个属性 :

> - `friction` 物理静电摩擦,对棱角关节来说,单位是牛顿,对旋转关节来说,单位是牛顿米.
>
> - `damping` 对棱柱关节来说,单位每米为牛顿秒;对旋转关节来说,每弧度为牛顿秒。

如果没有指定,这些系数默认为零。

<span id="other-tags"></span>

## 其他标记

在纯URDF(即不包括Gazebo特定标记)领域,还有两个剩余标记来帮助定义关节:校准和安全控制器. [光谱](https://wiki.ros.org/urdf/XML/joint),因为它们不包括在这个教程中。

<span id="next-steps"></span>

## 下一个步骤

减少代码数和烦人的数学数 [使用 xacro 键](Using-Xacro-to-Clean-Up-a-URDF-File.md).
