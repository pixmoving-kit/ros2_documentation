<span id="building-a-visual-robot-model-from-scratch"></span> <span id="buildingurdf"></span>

# 从零构建机器人的可视化模型

**目标：** 学习构建可在 RViz 中查看的机器人可视化模型。

**教程级别：** 中级

**预计耗时：** 20 分钟

> 本教程假设你会编写格式正确的 XML。

本教程将构建一个外形大致类似 R2D2 的机器人模型。后续教程会介绍如何[让模型运动](Building-a-Movable-Robot-Model-with-URDF.md)、[添加物理属性](Adding-Physical-and-Collision-Properties-to-a-URDF-Model.md)，以及[使用 Xacro 生成更简洁的代码](Using-Xacro-to-Clean-Up-a-URDF-File.md)。现在先专注于正确描述外观几何形状。

继续前，请确认已经安装 [joint_state_publisher](https://index.ros.org/p/joint_state_publisher) 包。如果安装了 [urdf_tutorial](https://index.ros.org/p/urdf_tutorial) 二进制包，通常已经包含此依赖；否则请补充安装，可使用 `rosdep` 检查。

本教程提到的所有模型及源文件都位于 [urdf_tutorial](https://index.ros.org/p/urdf_tutorial) 包中。

<span id="one-shape"></span>

## 一个形状

先从一个简单形状开始。下面是几乎最简单的 URDF，源文件为 [01-myfirst.urdf](https://github.com/ros/urdf_tutorial/blob/ros2/urdf/01-myfirst.urdf)。

```xml
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

这段 XML 描述一个名为 `myfirst` 的机器人，它只有一个连杆，也就是一个部件。其可视化部分是长 0.6 米、半径 0.2 米的圆柱。对于这样一个“Hello world”级别的例子，所需的嵌套标签看起来或许不少。

运行 `display.launch.py` 查看模型：

```console
$ ros2 launch urdf_tutorial display.launch.py model:=urdf/01-myfirst.urdf
```

它会完成三件事：

- 加载指定模型，作为 `robot_state_publisher` 节点的参数保存。
- 运行发布 [sensor_msgs/msg/JointState](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/JointState.msg) 和变换的节点，后面会进一步介绍。
- 使用配置文件启动 RViz。

启动后，RViz 应显示如下：

![第一个模型](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/myfirst.png)

需要注意：

- 固定坐标系是网格中心所在的变换坐标系。本例使用唯一连杆定义的 `base_link`。
- 可视化元素（圆柱）的原点默认位于几何中心，因此一半圆柱位于网格下方。

<span id="multiple-shapes"></span>

## 多个形状

接下来添加多个形状或连杆。若只在 URDF 中增加 `link` 元素，解析器不知道应将它们放在哪里，因此还需要添加关节。关节可以是可动的，也可以是不可动的；先从固定关节开始。源文件为 [02-multipleshapes.urdf](https://github.com/ros/urdf_tutorial/blob/ros2/urdf/02-multipleshapes.urdf)。

```xml
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

- 这里定义了尺寸为 0.6 × 0.1 × 0.2 米的长方体。
- 关节通过父连杆和子连杆定义。URDF 本质上是只有一个根连杆的树结构，因此腿部的位置取决于 `base_link` 的位置。

```console
$ ros2 launch urdf_tutorial display.launch.py model:=urdf/02-multipleshapes.urdf
```

![多个形状](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/multipleshapes.png)

两个形状使用相同原点，因此相互重叠。要避免重叠，必须进一步定义原点。

<span id="origins"></span>

## 原点

R2D2 的腿连接在躯干上半部的侧面，因此应将**关节**原点设在那里。连接点也不是腿的中部，而是上部，所以还需要偏移腿部的可视化原点，并旋转腿部使其直立。源文件为 [03-origins.urdf](https://github.com/ros/urdf_tutorial/blob/ros2/urdf/03-origins.urdf)。

```xml
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

- 先看关节原点，它相对于父坐标系定义：y 方向为 -0.22 米（画面中的左侧，但相对于坐标轴是右侧），z 方向为 0.25 米（向上）。无论子连杆的 `visual` 原点如何设置，子连杆坐标系原点都会位于上方偏右处。由于未指定 `rpy`（横滚、俯仰、偏航），子坐标系默认与父坐标系朝向相同。
- 腿部的可视化原点同时具有 `xyz` 和 `rpy` 偏移，用于定义可视化元素中心相对于连杆原点的位置和方向。为了让腿从顶部连接，将 z 偏移设为 -0.3 米；为了让长边平行于 z 轴，将可视化部分绕 Y 轴旋转 π/2。

```console
$ ros2 launch urdf_tutorial display.launch.py model:=urdf/03-origins.urdf
```

![原点设置](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/origins.png)

- 启动文件运行的软件包会根据 URDF 为模型中的每个连杆创建 TF 坐标系。RViz 利用这些信息确定各形状的位置。
- 如果某个 URDF 连杆不存在对应 TF 坐标系，它会以白色显示在原点处，参见[相关问题](http://answers.ros.org/question/207947/how-do-you-use-externally-defined-materials-in-a-urdfxacro-file/)。

<span id="material-girl"></span>

## 材质与颜色

你可能会说：“模型是挺可爱，但不是每个人都有 B21。我的机器人和 R2D2 都不是红色的！”确实如此。下面看看 `material` 标签，源文件为 [04-materials.urdf](https://github.com/ros/urdf_tutorial/blob/ros2/urdf/04-materials.urdf)。

```xml
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

- 身体现在是蓝色。这里定义了名为 `blue` 的材质，其红、绿、蓝、透明度通道分别为 0、0、0.8、1；各值范围均为 `[0,1]`。`base_link` 的可视化元素引用该材质。白色材质的定义方式相同。
- 也可以直接在 `visual` 内定义 `material`，并在其他连杆中引用，甚至重新定义也不会报错。
- 还可以通过纹理指定用于为物体着色的图像文件。

```console
$ ros2 launch urdf_tutorial display.launch.py model:=urdf/04-materials.urdf
```

![材质效果](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/materials.png)

<span id="finishing-the-model"></span>

## 完成模型

再加入脚、轮子和头部等形状，即可完成模型。其中主要新增了球体和网格，也添加了一些后续教程会用到的部件。源文件为 [05-visual.urdf](https://github.com/ros/urdf_tutorial/blob/ros2/urdf/05-visual.urdf)。

```xml
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

```console
$ ros2 launch urdf_tutorial display.launch.py model:=urdf/05-visual.urdf
```

![完整可视化模型](https://raw.githubusercontent.com/ros/urdf_tutorial/ros2/images/visual.png)

添加球体的方式很直观：

```xml
<link name="head">
  <visual>
    <geometry>
      <sphere radius="0.2"/>
    </geometry>
    <material name="white"/>
  </visual>
</link>
```

这里的网格借用了 PR2 的模型。它们是独立文件，必须指定路径，建议采用 `package://NAME_OF_PACKAGE/path` 格式。本教程的网格位于 `urdf_tutorial` 包的 `meshes` 目录中。

```xml
<link name="left_gripper">
  <visual>
    <origin rpy="0.0 0 0" xyz="0 0 0"/>
    <geometry>
      <mesh filename="package://urdf_tutorial/meshes/l_finger.dae"/>
    </geometry>
  </visual>
</link>
```

- 可以导入多种格式的网格。STL 很常见，引擎也支持可自带颜色数据的 DAE，因此无须额外指定颜色或材质。相关数据通常位于独立文件中；这些网格引用的 `.tif` 文件也在 `meshes` 目录中。
- 可以用相对缩放参数或包围盒尺寸调整网格大小。
- 也可以引用完全不同的软件包中的网格。

现在已经有了一个类似 R2D2 的 URDF 模型，接下来可以[让它运动起来](Building-a-Movable-Robot-Model-with-URDF.md)。
