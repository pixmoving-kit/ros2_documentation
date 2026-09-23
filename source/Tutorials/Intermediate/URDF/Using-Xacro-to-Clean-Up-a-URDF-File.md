---
translation_status: machine_translated
source: Tutorials/Intermediate/URDF/Using-Xacro-to-Clean-Up-a-URDF-File.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-xacro-to-clean-up-your-code"></span> <span id="urdfxacro"></span>

# 使用 Xacro 精简代码

**目标：** 学习一些技巧以减少使用 Xacro 的 URDF 文件的代码数量

**教程级别：** 中级

**用时：** 20分钟

现在,如果你在家里用自己的机器人设计来遵循所有这些步骤,你可能已经厌倦了做各种数学来得到非常简单的机器人描述来正确分析。 幸运的是,你可以使用机器人设计。 [亚克罗](https://index.ros.org/p/xacro) 让你的生活更加简单 它做三个非常有帮助的事情

> - 常数
>
> - 简单的数学
>
> - 宏

在这个教程中,我们查看所有这些快捷键,以帮助缩小URDF文件的整体大小,并方便阅读和维护.

<span id="using-xacro"></span>

## 使用 Xacro 软件

正如它的名字所暗示的那样, [亚克罗](https://index.ros.org/p/xacro) 是 XML 的宏语言。 xacro 程序运行所有宏并输出结果。 典型的用法看起来是这样的 :

``` console
$ xacro model.xacro > model.urdf
```

您也可以在发射文件中自动生成urdf。 这样做很方便, 因为它保持最新状态, 不使用硬盘空间。 然而, 生成需要时间, 所以请注意您的发射文件可能需要更长的时间才能启动 。

要运行xacro 在你的发射文件中, 你需要将 `Command` 替换为参数 `robot_state_publisher`.

``` python
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

一种更容易装入机器人模型的方法是使用 [urdf_launch](https://github.com/ros/urdf_launch) 要自动加载xacro/urdf的软件包。

``` python
from launch import LaunchDescription
from launch.actions import IncludeLaunchDescription
from launch.substitutions import PathJoinSubstitution
from launch_ros.substitutions import FindPackageShare


def generate_launch_description():
    return LaunchDescription([
        IncludeLaunchDescription(
            PathJoinSubstitution([FindPackageShare('urdf_launch'), 'launch', 'display.launch.py']),
            launch_arguments={
                'urdf_package': 'turtlebot3_description',
                'urdf_package_path': PathJoinSubstitution(['urdf', 'turtlebot3_burger.urdf'])
            }.items()
        )
    ])
```

在 URDF 文件的顶部, 您必须指定一个命名空间, 以便文件正确解析。 例如, 这是有效 xacro 文件的前两行 :

``` xml
<?xml version="1.0"?>
<robot xmlns:xacro="http://www.ros.org/wiki/xacro" name="firefighter">
```

<span id="constants"></span>

## 常数

让我们看看我们的R2D2基础。

``` xml
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

这里的信息有点多余。我们两次指定了圆柱的长度和半径。更糟糕的是,如果我们想要改变它,我们需要在两个不同的地方这样做。

幸运的是,xacro允许您指定作为常数的属性。 相反, 我们可以从上面的代码中写入 。

``` xml
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

- 这两个值在前两行中指定。它们可以在任何级别(假设是有效的XML)任意定义,在使用之前或之后。通常它们位于顶端。

- 我们不是在几何元素中指定实际半径,而是使用美元标志和圆括号来表示数值。

- 此代码将生成上面显示的相同代码 。

然后使用 \$QQ 构造的内容值来替换 \$Q。 这意味着您可以在属性中将其与其他文本合并 。

``` xml
<xacro:property name="robotname" value="marvin" />
<link name="${robotname}s_leg" />
```

这将生成

``` xml
<link name="marvins_leg" />
```

不过, \$Q 中的内容并不只是财产, 这让我们来到下一个要点:

<span id="math"></span>

## 数学

您可以使用四个基本操作( +, -, \*, /) 、 unary 减值和括号来建立 \$QQ 构造中的任意的复杂表达式。 例如 :

``` xml
<cylinder radius="${wheeldiam/2}" length="0.1"/>
<origin xyz="${reflect*(width+.02)} 0 0.25" />
```

您也可以使用比基本数学操作更多的功能,比如 `sin` 财务报告和财务报告 `cos`.

<span id="macros"></span>

## 宏

这是xacro软件包中最大和最有用的部件。

<span id="simple-macro"></span>

### 简单宏

让我们看看一个简单的无用的宏。

``` xml
<xacro:macro name="default_origin">
    <origin xyz="0 0 0" rpy="0 0 0"/>
</xacro:macro>
<xacro:default_origin />
```

(这是无用的,因为如果未指定来源,它与此值相同. ) 此代码将生成以下内容 。

``` xml
<origin rpy="0 0 0" xyz="0 0 0"/>
```

- 名称在技术上不是一个需要的元素,但您需要指定它才能使用.

- 每一个例子 `<xacro:$NAME />` 替换为 `xacro:macro` 标记 。

- 注意,尽管它不完全相同(两个属性有切换顺序),但生成的XML是等效的.

- 如果找不到带有指定名称的xacro,则不会扩展,也不会产生错误.

<span id="parameterized-macro"></span>

### 参数化宏

您也可以将宏参数化,这样它们就不会每次生成相同的准确文本。 如果结合数学功能,这甚至更加强大。

首先,让我们举一个在R2D2中使用的简单宏的例子.

``` xml
<xacro:macro name="default_inertial" params="mass">
    <inertial>
            <mass value="${mass}" />
            <inertia ixx="1e-3" ixy="0.0" ixz="0.0"
                 iyy="1e-3" iyz="0.0"
                 izz="1e-3" />
    </inertial>
</xacro:macro>
```

此代码可以使用

``` xml
<xacro:default_inertial mass="10"/>
```

参数的作用就像属性, 你可以在表达式中使用它们

您也可以使用整个块作为参数 。

``` xml
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

- 要指定块参数,请在参数名称前加上星号.

- 可用插入_块命令插入块

- 任意插入块数 。

<span id="practical-usage"></span>

## 实际使用

Xacro语言在允许您做的事情上相当灵活。 以下是一些有用的方法, 用于 xacro 。 [R2D2型号](https://github.com/ros/urdf_tutorial/blob/ros2/urdf/08-macroed.urdf.xacro),除以上显示的默认惯性宏外。

要看到 xacro 文件生成的模型, 运行与前一个教程相同的命令 :

``` console
$ ros2 launch urdf_tutorial display.launch.py model:=urdf/08-macroed.urdf.xacro
```

(发射文件一直运行xacro命令,但由于没有宏可以扩展,这不重要)

<span id="leg-macro"></span>

### 脚部宏

通常,您想要在不同的位置创建多个相似的外观对象。您可以使用宏和一些简单的数学来减少您必须写入的代码数量,就像我们对R2的两条腿所做的那样。

``` xml
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

- 常见的Trick 1: 使用名称前缀来获取两个类似命名对象.

- Community Trick 2: 使用数学来计算联合源。 如果您改变您的机器人大小, 用一些数学来计算联合偏移的属性会节省很多麻烦。

- 常见的Trick 3: 使用反射参数, 并设置为 1 或 -1. 查看我们如何使用反射参数将腿放在身体的两侧的基位\_ \${ prefix\_ leg 源位 。
