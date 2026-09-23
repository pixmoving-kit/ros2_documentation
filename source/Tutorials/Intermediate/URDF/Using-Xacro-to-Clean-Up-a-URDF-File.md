<span id="using-xacro-to-clean-up-your-code"></span> <span id="urdfxacro"></span>

# 使用 Xacro 简化代码

**目标：** 学习利用 Xacro 减少 URDF 文件代码量的技巧。

**教程级别：** 中级

**预计耗时：** 20 分钟

如果你一直用自己的机器人设计跟随本系列教程练习，可能已经厌倦了为了正确描述一个简单机器人而进行各种计算。[xacro](https://index.ros.org/p/xacro) 包可以让这些工作更轻松，它提供了三种很实用的功能：常量、简单数学运算和宏。

本教程将介绍这些简化手段，减少 URDF 文件的整体规模，使它更易阅读和维护。

<span id="using-xacro"></span>

## 使用 Xacro

顾名思义，[xacro](https://index.ros.org/p/xacro) 是一种 XML 宏语言。xacro 程序会展开所有宏并输出结果，典型用法为：

```console
$ xacro model.xacro > model.urdf
```

也可以在启动文件中自动生成 URDF。这样既能保证结果随源文件更新，也不占用额外硬盘空间。不过，生成过程需要时间，因此启动文件的启动过程可能变慢。

要在启动文件中运行 xacro，可将 `Command` 替换作为 `robot_state_publisher` 的参数：

```python
path_to_urdf = get_package_share_path('turtlebot3_description') / 'urdf' / 'turtlebot3_burger.urdf'
robot_state_publisher_node = launch_ros.actions.Node(
    package='robot_state_publisher',
    executable='robot_state_publisher',
    parameters=[{
        'robot_description': ParameterValue(
            Command(['xacro ', str(path_to_urdf)]), value_type=str
        )
    }]
)
```

更简单的模型加载方式是使用 [urdf_launch](https://github.com/ros/urdf_launch) 包自动加载 xacro/URDF，参见[启动文件示例](launch/urdf_display_launch.py)。

必须在 URDF 文件顶部声明命名空间，才能正确解析。例如，一个有效 xacro 文件的前两行如下：

```xml
<?xml version="1.0"?>
<robot xmlns:xacro="http://www.ros.org/wiki/xacro" name="firefighter">
```

<span id="constants"></span>

## 常量

先看看 R2D2 的 `base_link`：

```xml
<link name="base_link">
  <visual>
    <geometry>
      <cylinder length="0.6" radius="0.2"/>
    </geometry>
    <material name="blue"/>
  </visual>
  <collision>
    <geometry>
      <cylinder length="0.6" radius="0.2"/>
    </geometry>
  </collision>
</link>
```

这里存在信息重复：圆柱的长度和半径各指定了两次。如果要修改，就必须同时更改两处。

xacro 允许定义充当常量的属性，因此可以改写为：

```xml
<xacro:property name="width" value="0.2" />
<xacro:property name="bodylen" value="0.6" />
<link name="base_link">
    <visual>
        <geometry>
            <cylinder radius="${width}" length="${bodylen}"/>
        </geometry>
        <material name="blue"/>
    </visual>
    <collision>
        <geometry>
            <cylinder radius="${width}" length="${bodylen}"/>
        </geometry>
    </collision>
</link>
```

- 前两行指定这两个数值。只要符合 XML 语法，可以在几乎任何位置和层级定义属性，也可以放在使用它们之前或之后；通常放在文件顶部。
- 在几何元素中，不再直接写半径数值，而是用美元符号和花括号引用属性值。
- 这段代码会生成与前一个示例相同的结果。

`${}` 的内容求值后会替换整个 `${}`，因此可以在属性中将它与其他文本组合：

```xml
<xacro:property name="robotname" value="marvin" />
<link name="${robotname}s_leg" />
```

这将生成：

```xml
<link name="marvins_leg" />
```

不过，`${}` 中的内容并不局限于属性，还可以进行下面介绍的运算。

<span id="math"></span>

## 数学运算

在 `${}` 中，可以使用四则运算（`+`、`-`、`*`、`/`）、一元负号和括号，构造任意复杂的表达式。例如：

```xml
<cylinder radius="${wheeldiam/2}" length="0.1"/>
<origin xyz="${reflect*(width+.02)} 0 0.25" />
```

除基本运算外，还可以使用 `sin`、`cos` 等函数。

<span id="macros"></span>

## 宏

宏是 xacro 包中最重要、最有用的功能。

<span id="simple-macro"></span>

### 简单宏

先看一个简单但没有实际必要的宏：

```xml
<xacro:macro name="default_origin">
    <origin xyz="0 0 0" rpy="0 0 0"/>
</xacro:macro>
<xacro:default_origin />
```

之所以没有必要，是因为不指定原点时，默认值本来就与这里相同。这段代码会生成：

```xml
<origin rpy="0 0 0" xyz="0 0 0"/>
```

- 从语法上说名称不是必填项，但若要使用这个宏，就需要指定名称。
- 每个 `<xacro:$NAME />` 都会被相应 `xacro:macro` 标签的内容替换。
- 生成的两个属性顺序虽然交换了，但 XML 含义相同。
- 如果找不到指定名称的 xacro 宏，它不会展开，也不会产生错误。

<span id="parameterized-macro"></span>

### 带参数的宏

可以为宏添加参数，使其每次生成的文本有所不同。结合数学运算后，功能更强大。

下面是 R2D2 中一个简单宏：

```xml
<xacro:macro name="default_inertial" params="mass">
    <inertial>
            <mass value="${mass}" />
            <inertia ixx="1e-3" ixy="0.0" ixz="0.0"
                 iyy="1e-3" iyz="0.0"
                 izz="1e-3" />
    </inertial>
</xacro:macro>
```

调用方式为：

```xml
<xacro:default_inertial mass="10"/>
```

参数的用法与属性相同，也可以用于表达式。还可以将整个 XML 块作为参数传入：

```xml
<xacro:macro name="blue_shape" params="name *shape">
    <link name="${name}">
        <visual>
            <geometry>
                <xacro:insert_block name="shape" />
            </geometry>
            <material name="blue"/>
        </visual>
        <collision>
            <geometry>
                <xacro:insert_block name="shape" />
            </geometry>
        </collision>
    </link>
</xacro:macro>

<xacro:blue_shape name="base_link">
    <cylinder radius=".42" length=".01" />
</xacro:blue_shape>
```

- 在参数名前加星号，表示块参数。
- 使用 `insert_block` 插入该块。
- 可以根据需要多次插入同一个块。

<span id="practical-usage"></span>

## 实际应用

xacro 语言相当灵活。除上面的默认惯性宏外，[R2D2 模型](https://github.com/ros/urdf_tutorial/blob/ros2/urdf/08-macroed.urdf.xacro)还采用了下面这些实用方法。

要查看 xacro 文件生成的模型，仍使用前面教程中的命令形式：

```console
$ ros2 launch urdf_tutorial display.launch.py model:=urdf/08-macroed.urdf.xacro
```

实际上，启动文件一直在运行 xacro 命令，只是此前没有需要展开的宏，因此没有体现出区别。

<span id="leg-macro"></span>

### 腿部宏

经常需要在不同位置创建外形相似的多个对象。通过宏和简单计算，可以减少代码量，R2 的两条腿就是如此：

```xml
<xacro:macro name="leg" params="prefix reflect">
    <link name="${prefix}_leg">
        <visual>
            <geometry>
                <box size="${leglen} 0.1 0.2"/>
            </geometry>
            <origin xyz="0 0 -${leglen/2}" rpy="0 ${pi/2} 0"/>
            <material name="white"/>
        </visual>
        <collision>
            <geometry>
                <box size="${leglen} 0.1 0.2"/>
            </geometry>
            <origin xyz="0 0 -${leglen/2}" rpy="0 ${pi/2} 0"/>
        </collision>
        <xacro:default_inertial mass="10"/>
    </link>

    <joint name="base_to_${prefix}_leg" type="fixed">
        <parent link="base_link"/>
        <child link="${prefix}_leg"/>
        <origin xyz="0 ${reflect*(width+.02)} 0.25" />
    </joint>
    <!-- A bunch of stuff cut -->
</xacro:macro>
<xacro:leg prefix="right" reflect="1" />
<xacro:leg prefix="left" reflect="-1" />
```

- 常用技巧 1：使用名称前缀，得到名称相似的两个对象。
- 常用技巧 2：通过计算确定关节原点。机器人尺寸变化时，只需修改属性，并通过公式计算关节偏移量，可省去许多麻烦。
- 常用技巧 3：使用取值为 `1` 或 `-1` 的 `reflect` 参数。示例在 `base_to_${prefix}_leg` 的原点中使用它，将两条腿分别放在身体两侧。
