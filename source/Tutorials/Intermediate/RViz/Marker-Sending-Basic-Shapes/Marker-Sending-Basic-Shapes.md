---
translation_status: machine_translated
source: Tutorials/Intermediate/RViz/Marker-Sending-Basic-Shapes/Marker-Sending-Basic-Shapes.rst
---

<span id="marker-sending-basic-shapes-c"></span>

# Marker：发送基本形状（C++）

**目标：** 显示如何使用 `visualization_msgs/msg/Marker` 将基本形状发送到 RViz 的信息。

**教程级别：** 中级

**用时：** 15分钟

> **说明**
>
> 此教程假设您已经对写 ROS 2 C++ 节点和构建套件很满意 。 `colcon`.

<span id="intro"></span>

## 介绍

与许多其它 RViz 显示不同, `Marker` 显示可以使数据可视化,而不需要 RViz 提前了解数据的含义。相反,您的节点发送原始对象通过 `visualization_msgs/msg/Marker` 和 RViz 将它们变成箭头、盒子、球体、圆柱和其他标记类型。

这个教程显示如何发送四个基本形状:立方体、球体、圆柱和箭头。我们将创建一个程序,每秒发送一个新的标记,用不同的形状取代最后一个标记。

如果您想要在此行走后为标记字段和对象类型提供更广泛的引用,请参见 [Marker：显示类型](../Marker-Display-types/Marker-Display-types.md).

<span id="create-a-package"></span>

## 创建软件包

把包裹拿过来 [可视化\_ 图像存储器](https://github.com/ros-visualization/visualization_tutorials) 并在你的工作空间里建造它。

``` console
$ colcon build --packages-select visualization_marker_tutorials
```

<span id="sending-markers"></span>

## 发送标记

<span id="the-code"></span>

### 代码

此教程的代码生活在 `visualization_marker_tutorials` 软件包。您可以在 [basic_shapes.cpp](https://github.com/ros-visualization/visualization_tutorials/blob/ros2/visualization_marker_tutorials/src/basic_shapes.cpp).

<span id="the-code-explained"></span>

### 代码解释

好,让我们把代码块逐块拆开。我们首先要包括节点所用的头,包括: `rclcpp` 页:1 `visualization_msgs/msg/Marker` 消息定义。

``` c++
#include <memory>

#include "rclcpp/logging.hpp"
#include "rclcpp/rclcpp.hpp"
#include "visualization_msgs/msg/marker.hpp"
```

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

这看起来很眼熟。我们先初始化ROS 2, 创建一个节点, 并创建一个出版商。 `visualization_marker` 主题。

``` c++
rclcpp::init(argc, argv);
auto node = rclcpp::Node::make_shared("basic_shapes");
auto marker_pub = node->create_publisher<visualization_msgs::msg::Marker>(
  "visualization_marker", 1);
rclcpp::Rate loop_rate(1);
```

你应该看看ROS 2 包括和节点的设置。 出版商对RViz很重要,因为 `Marker` 显示对同一主题的订阅。

在这里,我们创建一个整数来跟踪我们将要公布的形状。我们将使用的四种类型都使用 `visualization_msgs/msg/Marker` 以同样的方式传递信息,这样我们就可以简单地切换形状类型,以显示四个不同的形状.

``` c++
uint32_t shape = visualization_msgs::msg::Marker::CUBE;
```

这开始了节目的肉,首先我们创造新的 `visualization_msgs/msg/Marker` 并开始填表。页眉为标记设置框架ID和时间戳。

``` c++
visualization_msgs::msg::Marker marker;
marker.header.frame_id = "my_frame";
marker.header.stamp = rclcpp::Clock().now();
```

我们准备好了 `frame_id` 改为: `my_frame` 。在一个运行的系统中,这应该是您想要解释标记所显示的边框。由于此教程不发布变换,RViz以后需要使用相同的固定边框。

命名空间和ID字段一起用于为标记创建一个独有的名称。如果另一个消息到达时带有相同的命名空间和ID,则新的标记将取代旧的标记。

``` c++
marker.ns = "basic_shapes";
marker.id = 0;
```

这个 `type` 字段指定我们发送的标记类型。可用的类型列于 `visualization_msgs/msg/Marker` 消息。我们在此设置类型到 `shape` 变量,它通过循环每次改变。

``` c++
marker.type = shape;
```

那个... `action` 字段指定要如何处理标记。ROS 2中使用的值是: `ADD`, `DELETE`,以及 `DELETEALL`. `ADD` 这是一种错误的表示,因为它真正意味着“创建或修改”。

``` c++
marker.action = visualization_msgs::msg::Marker::ADD;
```

我们在此设定标记的姿势。 这是一个完整的 6- DOF 姿势, 相对于标题中指定的框架和时间。 在此我们将其放置在源头并使用身份导向 。

``` c++
marker.pose.position.x = 0;
marker.pose.position.y = 0;
marker.pose.position.z = 0;
marker.pose.orientation.x = 0.0;
marker.pose.orientation.y = 0.0;
marker.pose.orientation.z = 0.0;
marker.pose.orientation.w = 1.0;
```

现在,我们指定标记的尺度。对于基本形状来说,尺度是: `1.0` 在所有方向上,都意味着一个表在一边。

``` c++
marker.scale.x = 1.0;
marker.scale.y = 1.0;
marker.scale.z = 1.0;
```

颜色指定为区域中的 RGBA 值 `[0, 1]`。在这里,我们使用不透明的绿色。α通道特别重要,因为如果默认,标记是透明的。 `a` 左侧为 `0`.

``` c++
marker.color.r = 0.0f;
marker.color.g = 1.0f;
marker.color.b = 0.0f;
marker.color.a = 1.0;
```

那个... `lifetime` 字段控制标记在被自动删除之前应该停留多长时间。零持续时间意味着它永远不应该被自动删除。

``` c++
marker.lifetime = rclcpp::Duration::from_nanoseconds(0);
```

现在我们发布标记信息。

``` c++
marker_pub->publish(marker);
```

这个代码可以让我们显示所有四个形状, 同时仅仅发布一个标记信息。 根据目前的形状,我们设定了要公布的下一个形状 。

``` c++
switch (shape) {
  case visualization_msgs::msg::Marker::CUBE:
    shape = visualization_msgs::msg::Marker::SPHERE;
    break;
  case visualization_msgs::msg::Marker::SPHERE:
    shape = visualization_msgs::msg::Marker::ARROW;
    break;
  case visualization_msgs::msg::Marker::ARROW:
    shape = visualization_msgs::msg::Marker::CYLINDER;
    break;
  case visualization_msgs::msg::Marker::CYLINDER:
    shape = visualization_msgs::msg::Marker::CUBE;
    break;
}
```

睡一秒钟,然后回顶部。

``` c++
loop_rate.sleep();
```

<span id="building-the-code"></span>

### 构建代码

构建 `visualization_marker_tutorials` 在工作空间中:

``` console
$ colcon build --packages-select visualization_marker_tutorials
```

<span id="running-the-code"></span>

### 运行代码

提供您的工作空间并运行节点。

``` console
$ source install/setup.bash
$ ros2 run visualization_marker_tutorials basic_shapes
```

<span id="viewing-the-markers"></span>

## 查看标记

现在节点正在发布标记,请开始RViz,这样你就可以查看它们.

``` console
$ source install/setup.bash
$ ros2 run rviz2 rviz2
```

如果您从未使用过 RViz,请从 [RViz 用户指南](../RViz-User-Guide/RViz-User-Guide.md).

因为我们没有设置任何变换, 第一件事就是设置 `Fixed Frame` 到标记信息中使用的框架, `my_frame`后添加一个 `Marker` 显示。注意默认主题, `visualization_marker`,是节点正在发布的同一节点。

现在您应该看到一个标记, 它的起源会改变每秒的形状。

![](images/basic_shapes_tutorial.png) <span id="more-information"></span>

## 更多信息

对于下一个标记教程,请继续 [标记: 点和线](../Marker-Points-and-Lines/Marker-Points-and-Lines.md)。如果需要更多关于标记信息字段和此处显示的四类之外的标记类型的信息,请继续 [Marker：显示类型](../Marker-Display-types/Marker-Display-types.md)。关于完整的源树,见 [可视化\_ 图像存储器](https://github.com/ros-visualization/visualization_tutorials).
