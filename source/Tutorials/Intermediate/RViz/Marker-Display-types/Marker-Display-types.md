---
translation_status: machine_translated
source: Tutorials/Intermediate/RViz/Marker-Display-types/Marker-Display-types.rst
---

<span id="marker-display-types"></span>

# Marker：显示类型

**目标：** 此教程解释基本的标记类型和如何使用它们.

**教程级别：** 中级

**用时：** 15分钟

<span id="background"></span>

## 背景

标记显示允许通过发送一个程序将各种原始形状添加到3D视图中 [visualization_msgs/msg/Marker](https://github.com/ros2/common_interfaces/blob/rolling/visualization_msgs/msg/Marker.msg) 或 时 间 [visualization_msgs/msg/MarkerArray](https://github.com/ros2/common_interfaces/blob/rolling/visualization_msgs/msg/MarkerArray.msg) 留言。

![](images/marker_overview.png)

开始 [标记: 发送基本形状](../Marker-Sending-Basic-Shapes/Marker-Sending-Basic-Shapes.md) 对于一个最小的发布者示例,该实例将引入整个页面使用的标记信息。

<span id="the-marker-message"></span>

## 标记信件

<span id="example-usage-c"></span>

### 1 示例使用 (C++)

首先我们要建立一个简单的出版商节点 来出版 `Marker` 发来的信件 `visualization_messages` 软件包到 `visualization_marker` 主题 :

``` C++
auto marker_pub = node->create_publisher<visualization_msgs::msg::Marker>("visualization_marker", 1);
```

之后就跟填字一样简单了 [visualization_msgs/msg/Marker](https://github.com/ros2/common_interfaces/blob/rolling/visualization_msgs/msg/Marker.msg) 电文并发表:

``` C++
visualization_msgs::msg::Marker marker;

marker.header.frame_id = "/my_frame";
marker.header.stamp = rclcpp::Clock().now();

marker.ns = "basic_shapes";
marker.id = 0;

marker.type = visualization_msgs::msg::Marker::SPHERE;

marker.action = visualization_msgs::msg::Marker::ADD;

marker.pose.position.x = 0;
marker.pose.position.y = 0;
marker.pose.position.z = 0;
marker.pose.orientation.x = 0.0;
marker.pose.orientation.y = 0.0;
marker.pose.orientation.z = 0.0;
marker.pose.orientation.w = 1.0;

marker.scale.x = 1.0;
marker.scale.y = 1.0;
marker.scale.z = 1.0;

marker.color.r = 0.0f;
marker.color.g = 1.0f;
marker.color.b = 0.0f;
marker.color.a = 1.0;   // Don't forget to set the alpha!

// only if using a MESH_RESOURCE marker type:
marker.mesh_resource = "package://pr2_description/meshes/base_v0/base.dae";

marker.lifetime = rclcpp::Duration::from_nanoseconds(0);

marker_pub->publish(marker);
```

还有一个 [visualization_msgs/msg/MarkerArray](https://github.com/ros2/common_interfaces/blob/rolling/visualization_msgs/msg/MarkerArray.msg) 信息,可以同时发布许多标记。

<span id="message-parameters"></span>

### 2 信件参数

标记信息类型定义于 [ROS 2 共同接口](https://github.com/ros2/common_interfaces/tree/rolling/visualization_msgs/msg) 软件包。本套件中的信息包括有助于理解信息中每个字段的评论。

- `ns`:

  > 这些标记的命名空间。 这加上 ID 构成一个独特的标识符 。

- `id`:

  > 指定给此标记的独特 id 。 您有责任将这些独有 的 ID 保存在您的命名空间 。

- `type`:

  > 标记类型( Arrow, Sphere,...) 。 可用类型在信件定义中指定。

- `action`:

  > 0 = 添加/修改,1 = (折旧),2 = 删除,3 = 删除全部

- `pose`:

  > Pose标记,指定为x/y/z位置和x/y/z/w之四方向.

- `scale`:

  > 标记的缩放。 在位置/方向之前应用。 比例 \[1, 1, 1\] 表示对象为 1 m乘 1 m 乘 1 m 。

- `color`:

  > 对象的颜色,指定为r/g/b/a,值为\[0, 1\]。 `a` 或 alpha 值,表示标记的不透明,1表示不透明,0表示完全透明。默认值为 0,或完全透明。 **您必须将标记的值设置为非零值, 否则默认会透明 !**

- `points`:

  > 仅用于类型标记 `Points`, `Line strips`,以及 `Line` / `Cube` / `Sphere` -lists。如果要指定箭头起始点和终点点,它也用于箭头类型。本条目代表列表 `geometry_msgs/Point` 中键或您想要渲染的每个标记对象。

- `colors`:

  > 此字段仅用于使用积分成员的标记。 此字段为每个条目指定每个顶点的颜色 r/g/ b/ 颜色( 尚未有 α) 。 `points`.

- `lifetime`:

  > A [持续时间信息值](https://docs.ros.org/en/rolling/p/builtin_interfaces/msg/Duration.html) 用于在此时间段后自动删除标记。 如果同一时间段的另一标记, 倒计时会重排 。 `namespace` / `id` 已收讫。

- `frame_locked`:

  > 没有了 `frame_locked` 参数将基于当前变换放置标记,即使给定的变换在以后有所改变,也会留在那里。设置此参数会告诉 RViz 在每一个更新周期上将标记重新转换到指定框架的新当前位置。

- `text`:

  > 用于 `TEXT_VIEW_FACING` 标记类型

- `mesh_resource`:

  > 资源位置 `MESH_RESOURCE` 标记类型。可以是 RViz 支持的任何网格类型(`.stl` 或食人鱼 `.mesh` 在1.0中,在1.1中增加了COLLADA,格式是: [resource_retriever](https://github.com/ros/resource_retriever/tree/rolling),包括软件包://语法.

<span id="object-types"></span>

### 3 对象类型

<span id="arrow-arrow-0"></span> <span id="rvizmarkerobjecttypes"></span>

#### 3.1 箭头(ARROW=0)

![](images/ArrowMarker.png)

箭头类型提供了两种不同的方式来指定箭头的起始/结束位置:

- `Position/Orientation`:

  > 枢轴点在尾端周围,身份导向沿QQ轴点指向它. `scale.x` 是箭头的长度, `scale.y` 是箭头宽度,并且 `scale.z` 是箭头高度。

- `Start/End Points`:

  > 也可以使用积分成员来指定箭头的起始/结束点。如果将积分输入积分成员,它会假设你想这样做。
  >
  > - 指数0的点被假定为起点,指数1的点被假定为终点.
  >
  > - `scale.x` 是竖轴的直径, `scale.y` 是头直径。如果 `scale.z` 不是零,而是指定了头长。

<span id="cube-cube-1"></span>

#### 3.2 立方体(CUBE=1)

![](images/CubeMarker.png)

枢轴点位于立方体的中心.

<span id="sphere-sphere-2"></span>

#### 3.3 球形(SPHERE=2)

![](images/SphereMarker.png)

支点位于球体的中心.

`scale.x` 直径为x方向, `scale.y` 在Y方向, `scale.z` 在 z 方向。通过设置这些值到不同的值,您可以得到椭圆形而不是球体。

<span id="cylinder-cylinder-3"></span>

#### 3.4 圆柱体(CYLINDER=3)

![](images/CylinderMarker.png)

支点位于圆柱的中心.

`scale.x` 直径为x方向, `scale.y` 在 Y 方向上,通过设置这些值到不同的值,您可以得到椭圆而不是圆。使用 `scale.z` 以指定高度。

<span id="line-strip-line-strip-4"></span>

#### 3.5 线条(LINE_STRIP=4)

![](images/LineStripMarker.png)

线条使用 [visualization_msgs/msg/Marker](https://github.com/ros2/common_interfaces/blob/rolling/visualization_msgs/msg/Marker.msg) 信息,它会在每连续两个点之间划一条线,所以0-1,1-2,2-3,3-4,4-5...

线条也有一些特殊处理的缩放: `scale.x` 用于控制线段的宽度。

请注意: `pose` 仍然使用(线条中的点数会由它们转化),线条相对于 `frame id` 在页眉中指定。

<span id="line-list-line-list-5"></span>

#### 3.6 线路列表(LINE_LIST=5).

![](images/LineListMarker.png)

线条列表使用 [visualization_msgs/msg/Marker](https://github.com/ros2/common_interfaces/blob/rolling/visualization_msgs/msg/Marker.msg) 信息。它将在每对点之间划出一条线,所以0 -1,2 -3,4 -5...

线条列表中也有一些关于比例表的特殊处理: `scale.x` 用于控制线段的宽度。

请注意: `pose` 仍然使用(线条中的点数会由它们转化),线条相对于 `frame id` 在页眉中指定。

<span id="cube-list-cube-list-6"></span>

#### 3.7 立方体列表(CUBE_LIST=6)

![](images/CubeListMarker.png)

立方体列表是除其位置外所有属性相同的立方体的列表。使用此对象类型而不是一个 [visualization_msgs/msg/MarkerArray](https://github.com/ros2/common_interfaces/blob/rolling/visualization_msgs/msg/MarkerArray.msg) 允许 RViz 进行批量渲染, 这使得它们更快地渲染。 警告是它们都必须有相同的比例 。

那个... `points` 成员 [visualization_msgs/msg/Marker](https://github.com/ros2/common_interfaces/blob/rolling/visualization_msgs/msg/Marker.msg) 信件用于每个立方体的位置。

<span id="sphere-list-sphere-list-7"></span>

#### 3.8 球形列表(SPHERE_LIST=7)

![](images/SphereListMarker.png)

球体列表是除其位置外所有属性相同的球体的列表。使用此对象类型而不是一个 [visualization_msgs/msg/MarkerArray](https://github.com/ros2/common_interfaces/blob/rolling/visualization_msgs/msg/MarkerArray.msg) 允许 RViz 进行批量渲染, 这使得它们更快地渲染。 警告是它们都必须有相同的比例 。

那个... `points` 成员 [visualization_msgs/msg/Marker](https://github.com/ros2/common_interfaces/blob/rolling/visualization_msgs/msg/Marker.msg) 信件用于每个球体的位置。

请注意: `pose` 仍然被使用( `points` 线会由它们改变 线会正确相对于 `frame id` 在页眉中指定。

<span id="points-points-8"></span>

#### 3.9 点数(POINTS=8)

![](images/PointsMarker.png)

使用 `points` 成员 [visualization_msgs/msg/Marker](https://github.com/ros2/common_interfaces/blob/rolling/visualization_msgs/msg/Marker.msg) 留言。

`Points` 具有一些特殊处理大小 : `scale.x` 是点宽度, `scale.y` 点高

请注意: `pose` 仍然被使用( `points` 线会由它们改变 线会正确相对于 `frame id` 在页眉中指定。

<span id="view-oriented-text-text-view-facing-9"></span>

#### 3.10 面向视图的文本(TEXT_VIEW_FACING=9)

![](images/text_view_facing_marker.png)

此标记在世界3D 点上显示文本。 文本总是向正确的方向显示, RViZ 用户可以看到包含的文本。 使用 `text` 字段在标记中。

仅 `scale.z` 已使用。 `scale.z` 指定大写“A”的高度。

<span id="mesh-resource-mesh-resource-10"></span>

#### 3.11 网格资源(MESH_resource=10)

![](images/mesh_resource_marker.png)

使用 `mesh_resource` 字段。可以是RViz(二进制)支持的任何网格类型。 `.stl` 或食人鱼 `.mesh` 在1.0中,加上COLLADA(`.dae`)在1.1中,该格式是URI格式。 [resource_retriever](https://github.com/ros/resource_retriever/tree/rolling),包括 `package://` 语法.

网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网状网

``` C++
marker.type = visualization_msgs::Marker::MESH_RESOURCE;
marker.mesh_resource = "package://pr2_description/meshes/base_v0/base.dae";
```

网格上的缩放是相对的,比例尺(1.0,1.0,1.0)表示网格将显示为网格文件中指定的确切大小,比例尺(1.0,1.0,2.0)表示网格将显示两倍高,但宽度/深度相同.

如果说 `mesh_use_embedded_materials` 旗帜被设定为真实,网格属于支持嵌入材料(如COLLADA)的类型,该文件中定义的材料将被使用,而不是标记中定义的颜色.

自\[1.8\]版本起,即使 `mesh_use_embedded_materials` 如果标记是真实的 `color` 被设定为除 `r=0`, `g=0`, `b=0`, `a=0` 标记 `color` 财务报告和财务报告 `alpha` 将用来用嵌入材料将网状涂抹。

<span id="triangle-list-triangle-list-11"></span>

#### 3.12 三角线列表(TRINANGLE_LIST=11)

![](images/triangle_list_marker.png)

使用点数和可选的颜色成员. 每组3个点数被作为三角形处理,因此指数0-1-2,3-4-5等.

请注意: `pose` 财务报告和财务报告 `scale` 目前仍在使用(行中的点数会由它们转换),行数相对于 `frame id` 在页眉中指定。

<span id="rendering-complexity-notes"></span>

### 4 渲染复杂性说明

单一的标记总是比许多标记要便宜。例如,单一的立方体列表可以处理数千个立方体,我们无法制造数千个单独的立方体标记。
