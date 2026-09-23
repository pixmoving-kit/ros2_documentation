---
translation_status: machine_translated
source: Tutorials/Intermediate/RViz/RViz-User-Guide/RViz-User-Guide.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="rviz-user-guide"></span>

# RViz 用户指南

**目标：** 理解 RViz

**教程级别：** 中级

**用时：** 25分钟

<span id="background"></span>

## 背景

RViz是机器人操作系统(ROS)框架的3D可视化器.

<span id="install-or-build-rviz"></span>

## 安装或构建 rviz

跟着 [安装指令](../../../../Installation.md) 用于操作系统安装 RViz。

<span id="startup"></span>

## 启动

别忘了提供设置文件。

``` console
$ source /opt/ros/rolling/setup.bash
```

然后启动视觉器

``` console
$ ros2 run rviz2 rviz2
```

当 RViz 第一次启动时, 您将会看到此窗口 :

![](images/initial_startup.png)

中间的大黑窗口是3D视图(空的,因为没有东西可以看 ) 。 左边是“ 显示” 列表,它将显示您装入的任何显示。现在它只包含全局选项和一个网格,我们稍后会看到。右边是下面描述的一些其他面板。

<span id="displays"></span>

## 显示

显示是一个在3D世界中画某种东西的东西,很可能在显示列表中有一些选项,一个例子是点云,机器人状态等.

<span id="adding-a-new-display"></span>

### 添加新显示

要添加显示,请单击底部的添加按钮:

![](images/add-button.png)

这将弹出新的显示对话框 :

![](images/add-display-dialog.png)

最上面的列表包含显示类型。 类型详细显示此显示将可视化何种数据。 中间的文本框会描述选定的显示类型。 最后, 您必须给显示一个独有的名称。 如果您在机器人上有两个激光扫描仪, 您可能会创建两个 `Laser Scan` 显示“激光基地”和“激光头”。

<span id="display-properties"></span>

### 显示属性

每个显示会获得自己的属性列表。 例如 :

![](images/display-properties.png) <span id="display-status"></span>

### 显示状态

每个显示都获得自己的状态,以帮助您知道是否一切都还好。状态可以是: `OK`, `Warning`, `Error`,或 `Disabled`。在显示标题中,以背景颜色表示状态,也可以在显示是否扩展的状态类别中表示状态:

![](images/display-status.png)

那个... `Status` 分类还扩展以显示特定状态信息。对于不同的显示,这种信息是不同的,消息应当自解。

<span id="built-in-display-types"></span>

### 内建显示类型

| 名称 | 说明 | 已使用的信件 |
|----|----|----|
| 轴 | 显示一组轴 |  |
| 任务 | 显示在机器人的每个转动关节中投入的努力 | [sensor_msgs/msg/JointStates](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/JointState.msg) |
| 摄像头 | 从相机的角度创建新的渲染窗口,并将图像覆盖在其上方. | [sensor_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/Image.msg), [sensor_msgs/msg/CameraInfo](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/CameraInfo.msg) |
| 网格 | 沿平面显示 2D 或 3D 网格 |  |
| 网格单元格 | 从网格中绘制单元格,通常从成本图中绘制障碍 [导航](https://github.com/ros-planning/navigation2) 堆栈。 | [nav_msgs/msg/GridCells](https://github.com/ros2/common_interfaces/blob/rolling/nav_msgs/msg/GridCells.msg) |
| 图像 | 创建一个带有图像的新渲染窗口。 与相机显示不同, 此显示不使用相机Info | [sensor_msgs/msg/Image](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/Image.msg) |
| 交互式马克 | 从一个或多个交互式标记服务器显示 3D 对象,并允许鼠标与它们交互 | [visualization_msgs/msg/InteractiveMarker](https://github.com/ros2/common_interfaces/blob/rolling/visualization_msgs/msg/InteractiveMarker.msg) |
| 激光扫描 | 显示来自激光扫描的数据,在渲染模式,积累等方面有不同的选项. | [sensor_msgs/msg/LaserScan](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/LaserScan.msg) |
| 地图 | 在地面平面上显示地图。 | [nav_msgs/msg/OccupancyGrid](https://github.com/ros2/common_interfaces/blob/rolling/nav_msgs/msg/OccupancyGrid.msg) |
| 标记 | 允许程序员通过主题显示任意原始形状 | [visualization_msgs/msg/Marker](https://github.com/ros2/common_interfaces/blob/rolling/visualization_msgs/msg/Marker.msg), [visualization_msgs/msg/MarkerArray](https://github.com/ros2/common_interfaces/blob/rolling/visualization_msgs/msg/MarkerArray.msg) |
| 路径 | 显示从 [导航](https://github.com/ros-planning/navigation2) 堆栈。 | [nav_msgs/msg/Path](https://github.com/ros2/common_interfaces/blob/rolling/nav_msgs/msg/Path.msg) |
| 点 | 画点为小球. | [geometry_msgs/msg/PointStamped](https://github.com/ros2/common_interfaces/blob/rolling/geometry_msgs/msg/PointStamped.msg) |
| 宝斯 | 绘制一个姿势,作为箭头或斧头。 | [geometry_msgs/msg/PoseStamped](https://github.com/ros2/common_interfaces/blob/rolling/geometry_msgs/msg/PoseStamped.msg) |
| Pose 阵列 | 绘制箭头的“ 云” , 在姿势阵列中为每个姿势绘制一个箭头 | [geometry_msgs/msg/PoseArray](https://github.com/ros2/common_interfaces/blob/rolling/geometry_msgs/msg/PoseArray.msg) |
| 点云(2) | 从点云显示数据,在渲染模式、积累等方面有不同的选项。 | [sensor_msgs/msg/PointCloud](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/PointCloud.msg), [sensor_msgs/msg/PointCloud2](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/PointCloud2.msg) |
| 多边形 | 绘制多边形的轮廓为线条。 | [geometry_msgs/msg/Polygon](https://github.com/ros2/common_interfaces/blob/rolling/geometry_msgs/msg/Polygon.msg) |
| 测量 | 积聚自久而久之的卵形. | [nav_msgs/msg/Odometry](https://github.com/ros2/common_interfaces/blob/rolling/nav_msgs/msg/Odometry.msg) |
| 范围 | 显示来自声纳或IR射程传感器的射程测量的圆锥。 版本: 电子+ | [sensor_msgs/msg/Range](https://github.com/ros2/common_interfaces/blob/rolling/sensor_msgs/msg/Range.msg) |
| 机器人模型 | 显示一个机器人在正确姿势中的视觉表现(由当前TF变换定义). |  |
| TF 主题 | 显示 [tf2](https://github.com/ros2/geometry2) 转变等级. |  |
| 扳手 | 绘制扳手为箭头( 力) 和箭头 + 圆( 曲折) | [geometry_msgs/msg/WrenchStamped](https://github.com/ros2/common_interfaces/blob/rolling/geometry_msgs/msg/WrenchStamped.msg) |
| 转动 | 绘制一个扭矩作为箭头(线性)和箭头+圆(角) | [geometry_msgs/msg/TwistStamped](https://github.com/ros2/common_interfaces/blob/rolling/geometry_msgs/msg/TwistStamped.msg) |

<span id="configurations"></span>

## 配置

显示器的不同配置通常对可视化器的不同用途有用。用于完整 PR2 的配置不一定对测试拖车有用。为此,可视化器允许您加载并保存不同的配置。

配置包含:

- 显示 + 属性

- 工具属性

- 三维可视化的视角和设置

<span id="views-panel"></span>

## 意见小组

可视化器中存在一些不同的相机类型.

![](images/camera-types.png)

相机类型既包括控制相机的不同方式,也包括不同的投影类型(正交对视角).

<span id="orbital-camera-default"></span>

### 轨道相机( 默认)

轨道摄像头只是围绕一个焦点旋转,而总是看着那个焦点。焦点在移动相机时被视像成一个小圆盘:

![](images/focal-point.png)

控件 :

- **鼠标左键**: 单击并拖动以围绕焦点旋转。

- **鼠标中键**: 单击并拖动相机上方和右方向量形成的平面中的焦点。 移动的距离取决于焦点 — 如果焦点上有对象, 点击顶部, 它会停留在鼠标下 。

- **鼠标右键**: 单击并拖动以缩放在/出焦点中。拖动缩放在/出焦点中,拖动缩放在/出焦点中。

- **滚轮**: 缩小/缩小联络点

<span id="fps-first-person-camera"></span>

### FPS(第一人称)相机

FPS摄像头是第一人称摄像头,

控件 :

- **鼠标左键**: 单击并拖动旋转。单击控制来选择鼠标下的对象并直接查看它。

- **鼠标中键**: 点击并拖动以沿着相机的上向和右向量形成的平面移动.

- **鼠标右键**: 单击并拖动相机的前进向量。向上拖动,向下拖动。

- **滚轮**:向前/向后移动。

<span id="top-down-orthographic"></span>

### 自上而下直径

自上而下的正反相机总是沿Z轴向下看(在机器人框中),是正反相机的视角,表示事情不会越远越小。

控件 :

- **鼠标左键**:单击并拖动以绕 Z 轴旋转。

- **鼠标中键**:单击并拖动相机沿XY平面移动.

- **鼠标右键**:单击并拖动以缩放图像。

- **滚轮**:放大图像.

<span id="xy-orbit"></span>

### XY 轨道

与轨道相机相同,焦点仅限于XY平面.

控件 :

见轨道相机.

<span id="third-person-follower"></span>

### 第三人称跟踪者

相机对目标框架保持一个恒定的查看角度。 相对于 XY 轨道, 如果目标框架显示, 相机会转动。 如果您正在用角对走廊进行三维映射, 这样做会很方便 。

控件 :

见轨道相机.

<span id="custom-views"></span>

### 自定义视图

视图面板还允许您创建不同的命名视图, 这些视图被保存并可在两者之间切换。 视图包含一个目标框架、 相机类型和相机姿势。 您可以点击视图面板的保存按钮来保存视图 。

![](images/views.png)

观点包括:

- 查看控制器类型

- 视图配置( 位置、 方向等; 每个视图控制器类型可能有所不同 ) 。

- 目标框架

视图是每个用户保存的,而不是配置文件中的 。

<span id="coordinate-frames"></span>

## 坐标框架

RViz 使用 tf 变换系统将它到达的坐标框中的数据转换成全局参考框,有两个坐标框在可视化器,目标帧和固定帧中很重要.

<span id="the-fixed-frame"></span>

### 固定框架

两个框架的更重要之处是固定框架。固定框架是用来表示 `world` 框架。通常这是 `map`,或 `world`,或者类似的东西,但也可以是,例如,你的odometric框架。

如果固定框被错误地设定为,比如机器人的底部,那么机器人所见过的所有物体都会出现在机器人前面,相对于被检测到的机器人的位置上,对于正确的结果来说,固定框不应该是相对于世界移动的.

如果您更改了固定框架,当前显示的所有数据都会被清除,而不是重新转换.

<span id="the-target-frame"></span>

### 目标框架

目标框架是相机视图的参考框架。例如,如果你的目标框架是地图,你会看到机器人在地图周围行驶。如果你的目标框架是机器人的底部,机器人会留在同一个地方,而其他一切则相对它移动。

<span id="tools"></span>

## 工具

可视化器有许多工具可以在工具栏上使用。以下章节将对这些工具作简短的介绍。您可以在“帮助 - \> 显示帮助面板”下找到一些更多信息。

![](images/tool.png) <span id="interact"></span>

### 交互

此工具可以让您与可视化的环境交互。 您可以点击对象, 并视其属性而定 。

键盘快捷键 : `i`

<span id="move-camera"></span>

### 移动相机

移动相机工具是默认工具。当选中此工具并在三维视图内单击时,视图会根据您在视图中选择的选项和相机类型而变化 `Views` 面板。见上一节。 `Views Panel` 以获取更多信息。

键盘快捷键 : `m`

<span id="select"></span>

### 选择

选择工具允许您选择在 3D 视图中显示的项目。 它支持单点选择以及单击/ 拖网框选择。 您可以用 Shift 键添加到选择中, 用 Ctrl 键从选择中移除。 如果您想要在选择时移动相机, 而无需切换到移动相机工具, 您可以按住 Alt 键 。 `f` 键将聚焦于当前选择的相机。

![](images/selection_highlight.png) ![](images/selection_selected.png)

键盘快捷键 : `s`

<span id="focus-camera"></span>

### 焦点相机

聚焦相机允许您在可视化器中选择一个位置。然后,相机会改变方向而不是位置,从而聚焦该点。

键盘快捷键 : `c`

<span id="measure"></span>

### 措施

通过测量工具,您可以测量可视化器中点之间的距离。在激活工具后,第一次点击将设定起始点,第二次点击将设定测量的终点。所产生的距离将显示在 RViz 窗口的底部。但请注意,该测量工具只在可视化器中实际渲染的对象中工作,因此不能在空格中使用。

![](images/measure.png)

键盘快捷键 : `n`

<span id="d-pose-estimate"></span>

### 2D Pose 估计

此工具允许您设置初始的设置来种子本地化系统( sent on the location) 。 `initialpose` ROS 主题 。 点击地面平面上的位置并拖动以选择方向。 输出主题可以在 `Tool Properties` 面板 。 (笑声)

![](images/set_pose.png)

该工具与 [导航](https://github.com/ros-planning/navigation2) 堆栈。

键盘快捷键 : `p`

<span id="d-nav-goal"></span>

### 2D 导航目标

这个工具可以让您设定一个目标 。 `goal_pose` ROS 主题。 点击地面平面上的位置并拖动以选择方向。 输出主题可以在 `Tool Properties` 面板 。 (笑声)

该工具与 [导航](https://github.com/ros-planning/navigation2) 堆栈。

键盘快捷键 : `g`

<span id="publish-point"></span>

### 发布点

发布点工具允许您在可视化器中选择一个对象, 工具将基于框架发布该点的坐标。 其结果像测量工具一样在底部显示, 但也发布在 `clicked_point` 主题。

键盘快捷键 : `u`

<span id="time"></span>

## 时间

时间面板在模拟器中运行时大部分是有用的,因为它可以让你看到ROS时间已经过去了多少,对称了多少. `Wall Clock` 时间已经过去。 时间面板还让你重新设置可视化器的内部时间状态,它重新显示所有显示以及 tf 的内部缓存数据。

![](images/time.png)

如果您没有在模拟中运行, 时间面板大多是无用的。 在大多数情况下, 它可以关闭, 而且你可能甚至不会注意到( 除了在 rviz 的其余部分拥有更多屏幕房地产 ) 。
