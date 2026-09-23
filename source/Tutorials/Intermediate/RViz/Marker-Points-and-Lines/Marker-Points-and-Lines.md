---
translation_status: machine_translated
source: Tutorials/Intermediate/RViz/Marker-Points-and-Lines/Marker-Points-and-Lines.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="marker-points-and-lines-c"></span>

# Marker：点与线（C++）

**目标：** 显示如何使用 `visualization_msgs/msg/Marker` 给 RViz 发送点和线条的消息。

**教程级别：** 中级

**用时：** 15分钟

> **说明**
>
> 此教程假设您已完成 [标记: 发送基本形状](../Marker-Sending-Basic-Shapes/Marker-Sending-Basic-Shapes.md).

<span id="intro"></span>

## 介绍

内 [标记: 发送基本形状](../Marker-Sending-Basic-Shapes/Marker-Sending-Basic-Shapes.md) 您学会了如何使用可视化标记向 RViz 发送简单形状。您不仅可以发送简单的形状,而且此教程引入了 `POINTS`, `LINE_STRIP`,以及 `LINE_LIST` 标记类型。关于完整的类型列表,参见 [Marker：显示类型](../Marker-Display-types/Marker-Display-types.md).

<span id="using-points-line-strips-and-line-lists"></span>

## 使用点、线条条和线条列表

那个... `POINTS`, `LINE_STRIP`,以及 `LINE_LIST` 标记全部使用 `points` 成员 `visualization_msgs/msg/Marker` 信息。该信息 `POINTS` 类型在添加的每个点上设置点。 `LINE_STRIP` 类型在连接的一组行中将每个点作为顶点,其中0点与1、1至2、2至3点相连。 `LINE_LIST` 类型创建出每对点的无连接线,如点0到点1,点2到点3等.

<span id="the-code"></span>

### 代码

把包裹拿过来 [可视化\_ 图像存储器](https://github.com/ros-visualization/visualization_tutorials)。此教程的代码生活在 `visualization_marker_tutorials` 软件包。您可以在 [points_and_lines.cpp](https://github.com/ros-visualization/visualization_tutorials/blob/ros2/visualization_marker_tutorials/src/points_and_lines.cpp).

<span id="the-code-explained"></span>

### 代码解释

现在让我们拆解代码,跳过前一个教程中解释的东西。 创造的总体效果是旋转螺旋,每个顶点的线向上竖立。

我们从节点所用的信头开始,包括: `cmath` 用于螺旋和用于标记和点数的信息。

``` c++
#define _USE_MATH_DEFINES

#include <chrono>
#include <cmath>
#include <memory>

#include "rclcpp/rclcpp.hpp"
#include "geometry_msgs/msg/point.hpp"
#include "visualization_msgs/msg/marker.hpp"
```

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

这看起来应该很熟悉。我们先初始化ROS 2, 创建一个节点, 创建一个出版商。 `visualization_marker` ,并设定循环率。

``` c++
rclcpp::init(argc, argv);
auto node = rclcpp::Node::make_shared("points_and_lines");
auto marker_pub = node->create_publisher<visualization_msgs::msg::Marker>(
  "visualization_marker", 10);
rclcpp::Rate loop_rate(30);
```

我们还创建一个浮动点变量,用于随时间推移使螺旋动.

``` c++
float f = 0.0f;
```

在主循环中,我们创造三个 `visualization_msgs/msg/Marker` 信件并初始化所有共享数据。默认情况下,一个标记信息包含一个其四角已初始化为身份导向的姿势,所以我们只需要为这个教程设置重要的字段。

``` c++
visualization_msgs::msg::Marker points, line_strip, line_list;
points.header.frame_id = line_strip.header.frame_id = line_list.header.frame_id = "my_frame";
points.header.stamp = line_strip.header.stamp = line_list.header.stamp = rclcpp::Clock().now();
points.ns = line_strip.ns = line_list.ns = "points_and_lines";
points.action = line_strip.action = line_list.action = visualization_msgs::msg::Marker::ADD;
```

在这里,我们为三个标记分配了三个不同的ID。 `points_and_lines` 命名空间确保它们不会与其他标记出版商相撞。

``` c++
points.id = 0;
line_strip.id = 1;
line_list.id = 2;
```

在这里,我们设置标记类型到 `POINTS`, `LINE_STRIP`,以及 `LINE_LIST`.

``` c++
points.type = visualization_msgs::msg::Marker::POINTS;
line_strip.type = visualization_msgs::msg::Marker::LINE_STRIP;
line_list.type = visualization_msgs::msg::Marker::LINE_LIST;
```

那个... `scale` 成员指这些标记类型不同的事物. `POINTS` 标记使用 `x` 财务报告和财务报告 `y` 分别显示宽度和高度,同时 `LINE_STRIP` 财务报告和财务报告 `LINE_LIST` 标记只使用 `x` 组件,用于定义行宽。缩放值以米计。

``` c++
points.scale.x = 0.2;
points.scale.y = 0.2;

line_strip.scale.x = 0.1;
line_list.scale.x = 0.1;
```

我们在这里设定了绿点, 线条为蓝色, 线条列表为红色。 和其他标记一样, Alpha 通道必须是非零 。

``` c++
points.color.g = 1.0f;
points.color.a = 1.0;

line_strip.color.b = 1.0;
line_strip.color.a = 1.0;

line_list.color.r = 1.0;
line_list.color.a = 1.0;
```

现在我们为点和线创建顶点。我们使用正弦和余弦来生成螺旋。 `POINTS` 财务报告和财务报告 `LINE_STRIP` 标记每个顶点只需要一个点,而 `LINE_LIST` 标记要求每个线段有两个点。

``` c++
for (uint32_t i = 0; i < 100; ++i) {
  float y = 5 * sin(f + i / 100.0f * 2 * M_PI);
  float z = 5 * cos(f + i / 100.0f * 2 * M_PI);

  geometry_msgs::msg::Point p;
  p.x = static_cast<int32_t>(i) - 50;
  p.y = y;
  p.z = z;

  points.points.push_back(p);
  line_strip.points.push_back(p);

  // The line list needs two points for each line
  line_list.points.push_back(p);
  p.z += 1.0;
  line_list.points.push_back(p);
}
```

一旦标记信息被填写出来,我们公布全部三个信息.

``` c++
marker_pub->publish(points);
marker_pub->publish(line_strip);
marker_pub->publish(line_list);
```

然后我们睡觉,推进动画阶段,然后循环回顶部.

``` c++
loop_rate.sleep();
f += 0.04f;
```

<span id="viewing-the-markers"></span>

### 查看标记

在工作空间构建软件包 :

``` console
$ colcon build --packages-select visualization_marker_tutorials
```

然后源代码到工作空间并运行节点:

``` console
$ source install/setup.bash
$ ros2 run visualization_marker_tutorials points_and_lines
```

现在运行 RViz :

``` console
$ source install/setup.bash
$ ros2 run rviz2 rviz2
```

如果您从未使用过 RViz,请从 [RViz 用户指南](../RViz-User-Guide/RViz-User-Guide.md).

设置 RViz , 和上次的教程一样。 因为我们没有设置任何变换, 请设置 。 `Fixed Frame` 改为: `my_frame`后添加一个 `Marker` 显示。默认主题, `visualization_marker`,是节点正在发布的同一节点。

你应该看到一个旋转螺旋 看起来像这样的东西:

![](images/points_and_lines_marker_tutorial.png) <span id="next-steps"></span>

## 后续步骤

关于RViz所支持的标记和选项的更多信息,请继续 [Marker：显示类型](../Marker-Display-types/Marker-Display-types.md)。尝试一些其他的标记类型。
