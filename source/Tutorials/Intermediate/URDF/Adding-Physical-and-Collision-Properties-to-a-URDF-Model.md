<span id="adding-physical-and-collision-properties"></span> <span id="urdfproperties"></span>

# 添加物理和碰撞属性

**目标：** 学习为连杆添加碰撞和惯性属性，并为关节添加动力学属性。

**教程级别：** 中级

**预计耗时：** 10 分钟

本教程介绍如何为 URDF 模型添加基本物理属性，以及如何指定碰撞属性。

<span id="collision"></span>

## 碰撞

目前，我们只在连杆中定义了一个 `visual` 子元素，用于描述机器人的外观。但要进行碰撞检测或机器人仿真，还必须定义 `collision` 元素。[新的 URDF](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/urdf/07-physics.urdf)包含了碰撞和物理属性。

下面是新的基座连杆代码：

```xml
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

- `collision` 是 `link` 的直接子元素，与 `visual` 同级。
- 与 `visual` 一样，`collision` 使用 `geometry` 标签定义形状，格式完全相同。
- 同样可以在 `collision` 中添加 `origin` 子元素来指定原点。

很多时候，碰撞几何体及其原点应与可视化几何体一致，但主要有两类例外：

- **加快计算。** 两个网格之间的碰撞检测，比两个简单几何体之间的检测复杂得多。因此，可以在 `collision` 中用较简单的几何体替代网格。
- **安全区域。** 有时需要限制其他物体靠近敏感设备。例如，不希望任何物体碰撞 R2D2 的头部时，可以用一个包住头部的圆柱作为碰撞几何体，避免物体过于靠近。

<span id="physical-properties"></span>

## 物理属性

为使模型正确参与仿真，需要定义机器人若干物理属性，也就是 Gazebo 等物理引擎需要的信息。

<span id="inertia"></span>

### 惯性

每个参与仿真的连杆都需要 `inertial` 标签。简单示例如下：

```xml
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

- `inertial` 也是 `link` 的子元素。
- 质量单位为千克。
- `inertia` 元素指定 3×3 转动惯量矩阵。由于矩阵对称，只需提供 6 个元素：

| | | |
| --- | --- | --- |
| **ixx** | **ixy** | **ixz** |
| ixy | **iyy** | **iyz** |
| ixz | iyz | **izz** |

- MeshLab 等建模软件可以提供这些信息。圆柱、长方体、球等基本几何体的惯量可根据维基百科的[转动惯量张量列表](https://en.wikipedia.org/wiki/List_of_moments_of_inertia#List_of_3D_inertia_tensors)计算，上面的示例也使用了这些公式。
- 惯量张量取决于物体的质量及其分布。一个合理的初步近似是假设质量在体积内均匀分布，再根据物体形状计算惯量张量。
- 如果不确定取值，对于中等大小的连杆，`ixx/iyy/izz=1e-3` 或更小的矩阵通常是合理的默认值，这对应边长 0.1 米、质量 0.6 千克的立方体。单位矩阵通常非常不合适，因为惯量过大，相当于边长 0.1 米、质量 600 千克的立方体！
- 还可以通过 `origin` 指定重心及惯性参考坐标系，均相对于连杆的参考坐标系。
- 使用实时控制器时，惯量为零或接近零可能使机器人模型毫无预警地坍缩，所有连杆原点看起来都与世界原点重合。

<span id="contact-coefficients"></span>

### 接触系数

可以通过 `collision` 的 `contact_coefficients` 子元素定义连杆相互接触时的行为，有三个属性：

- `mu`：[摩擦系数](https://simple.wikipedia.org/wiki/Coefficient_of_friction)。
- `kp`：[刚度系数](https://en.wikipedia.org/wiki/Stiffness)。
- `kd`：[阻尼系数](https://en.wikipedia.org/wiki/Damping_ratio#Damping_ratio_definition)。

<span id="joint-dynamics"></span>

### 关节动力学

关节的 `dynamics` 标签描述其运动特性，有两个属性：

- `friction`：静摩擦。平移关节的单位为牛顿，转动关节为牛顿米。
- `damping`：阻尼。平移关节的单位为牛顿秒/米，转动关节为牛顿米秒/弧度。

若不指定，两个系数都默认为零。

<span id="other-tags"></span>

## 其他标签

在纯 URDF 范围内，不计 Gazebo 专用标签，还有 `calibration` 和 `safety_controller` 两个用于定义关节的标签。本教程不作介绍，详见[规范](https://wiki.ros.org/urdf/XML/joint)。

<span id="next-steps"></span>

## 后续步骤

通过[使用 Xacro](Using-Xacro-to-Clean-Up-a-URDF-File.md)，减少代码量和繁琐的计算。
