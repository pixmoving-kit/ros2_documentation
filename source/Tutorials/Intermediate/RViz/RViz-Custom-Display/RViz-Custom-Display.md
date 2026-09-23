---
translation_status: machine_translated
source: Tutorials/Intermediate/RViz/RViz-Custom-Display/RViz-Custom-Display.rst
---

<span id="building-a-custom-rviz-display"></span>

# 构建自定义 RViz 显示插件

<span id="background"></span>

## 背景

有许多类型的数据在RViz中具有已有的可视化. 然而,如果有一个消息类型还没有插件来显示它,那么在RViz中可以看到它有两种选择.

> 1.  将信件转换为其它类型, 例如 `visualization_msgs/Marker`.
>
> 2.  写入自定义 RViz 显示 。

有了第一个选项, 网络流量和数据表达的局限性会更多。 数据也是快速和灵活的。 后一个选项在这个教程中被解释。 需要做一些工作, 但可以导致更丰富的可视化 。

此教程的全部代码可见于 [此仓库](https://github.com/MetroRobots/rviz_plugin_tutorial)。为了看到此教程中写入的插件的递增进度,寄存器有不同的分支(`step2`, `step3`每一个可以编译和运行,随你走。

<span id="point2d-message"></span>

## 点2D 信件

将会播放一个在游戏中定义的玩具信息 `rviz_plugin_tutorial_msgs` 软件包 : `Point2D.msg`:

``` default
std_msgs/Header header
float64 x
float64 y
```

<span id="boilerplate-for-basic-plugin"></span>

## Basic 插件的沸腾

绑在其中, 有很多代码。 您可以用分支名称查看该代码的完整版本 `step1`.

<span id="header-file"></span>

### 页眉文件

内容如下: `point_display.hpp`

``` c++
#ifndef RVIZ_PLUGIN_TUTORIAL__POINT_DISPLAY_HPP_
#define RVIZ_PLUGIN_TUTORIAL__POINT_DISPLAY_HPP_

#include <rviz_common/message_filter_display.hpp>
#include <rviz_plugin_tutorial_msgs/msg/point2_d.hpp>

namespace rviz_plugin_tutorial
{
class PointDisplay
  : public rviz_common::MessageFilterDisplay<rviz_plugin_tutorial_msgs::msg::Point2D>
{
  Q_OBJECT

protected:
  void processMessage(const rviz_plugin_tutorial_msgs::msg::Point2D::ConstSharedPtr msg) override;
};
}  // namespace rviz_plugin_tutorial

#endif  // RVIZ_PLUGIN_TUTORIAL__POINT_DISPLAY_HPP_
```

- 我们正在执行 [信件过滤播放](https://github.com/ros2/rviz/blob/0ef2b56373b98b5536f0f817c11dc2b5549f391d/rviz_common/include/rviz_common/message_filter_display.hpp#L43) 类,可以与带有一个的任意信件一起使用 `std_msgs/Header`.

- 班级是和我们一样的 `Point2D` 信件类型。

- [基于此教程范围之外的原因](https://doc.qt.io/qt-5/moc.html),你需要的是一个 `Q_OBJECT` 宏在里面,以使图形界面的QT部分发挥作用。

- `processMessage` 需要执行的唯一方法, 我们将在 cpp 文件中这样做。

<span id="source-file"></span>

### 源文件

`point_display.cpp`

``` c++
#include <rviz_plugin_tutorial/point_display.hpp>
#include <rviz_common/logging.hpp>

namespace rviz_plugin_tutorial
{
void PointDisplay::processMessage(const rviz_plugin_tutorial_msgs::msg::Point2D::ConstSharedPtr msg)
{
  RVIZ_COMMON_LOG_INFO_STREAM("We got a message with frame " << msg->header.frame_id);
}
}  // namespace rviz_plugin_tutorial

#include <pluginlib/class_list_macros.hpp>
PLUGINLIB_EXPORT_CLASS(rviz_plugin_tutorial::PointDisplay, rviz_common::Display)
```

- 伐木并非绝对必要,但有助于调试.

- 为了让RViz找到我们的插件,我们需要这个 `PLUGINLIB` 在我们的守则中援引(以及下文其他内容)。

<span id="package-xml"></span>

### package.xml

我们需要我们包中的以下三个依赖性. xml:

``` xml
<depend>pluginlib</depend>
<depend>rviz_common</depend>
<depend>rviz_plugin_tutorial_msgs</depend>
```

<span id="rviz-common-plugins-xml"></span>

### rviz_common_plugins.xml

``` xml
<library path="point_display">
  <class type="rviz_plugin_tutorial::PointDisplay" base_class_type="rviz_common::Display">
    <description></description>
  </class>
</library>
```

- 这是标准 `pluginlib` 代码。

  - 库 `path` 我们将在 CMake 中指定的库名称 。

  - 班级应该与 `PLUGINLIB` 引自上经.

- 我们稍后再讨论这个描述,

<span id="cmakelists-txt"></span>

### CMakeLists.txt (中文(简体) ).

在标准锅炉板的顶部增加以下线路.

``` cmake
find_package(ament_cmake_ros REQUIRED)
find_package(pluginlib REQUIRED)
find_package(rviz_common REQUIRED)
find_package(rviz_plugin_tutorial_msgs REQUIRED)

set(CMAKE_AUTOMOC ON)
qt5_wrap_cpp(MOC_FILES
  include/rviz_plugin_tutorial/point_display.hpp
)

add_library(point_display src/point_display.cpp ${MOC_FILES})
target_include_directories(point_display PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)
ament_target_dependencies(point_display
  pluginlib
  rviz_common
  rviz_plugin_tutorial_msgs
)
install(TARGETS point_display
        EXPORT export_rviz_plugin_tutorial
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        RUNTIME DESTINATION bin
)
install(DIRECTORY include/
        DESTINATION include
)
install(FILES rviz_common_plugins.xml
        DESTINATION share/${PROJECT_NAME}
)
ament_export_include_directories(include)
ament_export_targets(export_rviz_plugin_tutorial)
pluginlib_export_plugin_description_file(rviz_common rviz_common_plugins.xml)
```

- 要生成适当的 Qt 文件, 我们需要

  - 转弯 `CMAKE_AUTOMOC` 继续

  - 通过调用来环绕信头 `qt5_wrap_cpp` 每个标题都有 `Q_OBJECT` 在它里面。 进它。

  - 包含 `MOC_FILES` 在库 与我们的其他 Cpp 文件。

- 注意,如果您不包装您的页眉文件,您在运行时尝试加载插件时可能会收到错误消息,大致如下:

  ``` default
  [rviz2]: PluginlibFactory: The plugin for class 'rviz_plugin_tutorial::PointDisplay' failed to load. Error: Failed to load library /home/ros/ros2_ws/install/rviz_plugin_tutorial/lib/libpoint_display.so. Make sure that you are calling the PLUGINLIB_EXPORT_CLASS macro in the library code, and that names are consistent between this macro and your XML. Error string: Could not load library LoadLibrary error: /home/ros/ros2_ws/install/rviz_plugin_tutorial/lib/libpoint_display.so: undefined symbol: _ZTVN20rviz_plugin_tutorial12PointDisplayE, at /tmp/binarydeb/ros-foxy-rcutils-1.1.4/src/shared_library.c:84
  ```

- 许多其他代码确保插件部分工作。 即调用 `pluginlib_export_plugin_description_file` 要让 RViz 找到您的新插件, 关键是 。

<span id="testing-it-out"></span>

### 试试看

编译您的代码并运行 `rviz2`。您应该能够通过单击添加您的新插件 `Add` 在左下方,然后选择您的软件包/插件。

[![添加显示的截图](images/Step1A.png)](images/Step1A.png)

最初,该显示会处于错误状态,因为您尚未指定一个话题 。

[![错误状态的截图](images/Step1B.png)](images/Step1B.png)

如果我们把话题 `/point` 里面,它应该装满精细,但不能显示任何东西。

[![函数空显示的截图](images/Step1C.png)](images/Step1C.png)

您可以使用以下命令发布消息:

``` console
$ ros2 topic pub /point rviz_plugin_tutorial_msgs/msg/Point2D "{header: {frame_id: map}, x: 1, y: 2}" -r 0.5
```

这应导致“我们收到一个信息”的伐木作业出现在 `stdout` (原始内容存档于2018-10-21). Official website

<span id="actual-visualization"></span>

## 实际视觉

您可以使用分支名称查看此步骤的完整版本 `step2`.

首先,您需要添加一个依赖性在 `CMakeLists.txt` 财务报告和财务报告 `package.xml` 软件包上 `rviz_rendering`.

我们需要在标题文件中添加三行:

- `#include <rviz_rendering/objects/shape.hpp>` - 有一个 [在 rviz_landing 软件包中有许多选项](https://github.com/ros2/rviz/tree/ros2/rviz_rendering/include/rviz_rendering/objects) 用于构建您可视化的物体。我们在此使用一个简单的形状。

- 在课上,我们会增加一个新的 `protected` 虚拟方法 : `void onInitialize() override;`

- 我们还在形状对象上添加一个指针: `std::unique_ptr<rviz_rendering::Shape> point_shape_;`

然后在cpp文件中,我们定义 `onInitialize` 方法 :

``` c++
void PointDisplay::onInitialize()
{
  MFDClass::onInitialize();
  point_shape_ =
    std::make_unique<rviz_rendering::Shape>(rviz_rendering::Shape::Type::Cube, scene_manager_,
      scene_node_);
}
```

- `MFDClass` 是,这是 [别名](https://github.com/ros2/rviz/blob/0ef2b56373b98b5536f0f817c11dc2b5549f391d/rviz_common/include/rviz_common/message_filter_display.hpp#L57) 为方便起见,请输入模板的父类。

- 形状对象必须在此构造 `onInitialize` 方法而不是构造器,因为否则 `scene_manager_` 财务报告和财务报告 `scene_node_` 将是不准备。

我们还更新我们的 `processMessage` 方法 :

``` c++
void PointDisplay::processMessage(const rviz_plugin_tutorial_msgs::msg::Point2D::ConstSharedPtr msg)
{
  RVIZ_COMMON_LOG_INFO_STREAM("We got a message with frame " << msg->header.frame_id);

  Ogre::Vector3 position;
  Ogre::Quaternion orientation;
  if (!context_->getFrameManager()->getTransform(msg->header, position, orientation)) {
    RVIZ_COMMON_LOG_DEBUG_STREAM("Error transforming from frame '" << msg->header.frame_id <<
        "' to frame '" << qPrintable(fixed_frame_) << "'");
  }

  scene_node_->setPosition(position);
  scene_node_->setOrientation(orientation);

  Ogre::Vector3 point_pos;
  point_pos.x = msg->x;
  point_pos.y = msg->y;
  point_shape_->setPosition(point_pos);
}
```

- 我们需要找到正确的信息框架 并改变 `scene_node_` 因此,这确保了可视化并不总是相对于固定框架出现。

- 我们所建立的实际可视化在最后四行:我们设定了可视化的位置,以配合信息的位置。

结果应该是这样的:

[![函数显示的截图](images/Step2A.png)](images/Step2A.png)

如果该盒子没有出现在该位置,可能是因为:

- 您此时不发表这个话题

- 这则讯息在最近两秒钟内仍未发布。

- 您没有在 RViz 中正确设定话题 。

<span id="it-s-nice-to-have-options"></span>

## 拥有选择权真好。

如果您想要允许用户自定义可视化的不同属性, 您需要添加 [rviz_common:: 财产对象](https://github.com/ros2/rviz/tree/ros2/rviz_common/include/rviz_common/properties).

您可以使用分支名称查看此步骤的完整版本 `step3`.

<span id="header-updates"></span>

### 页眉更新

包含颜色属性头文件 : `#include <rviz_common/properties/color_property.hpp>`颜色只是您可以设置的许多属性之一。

在原型中添加为 `updateStyle`,每当通过 Qt 的 SIGNAL/ SLOT 框架更改图形用户界面时,它就称为:

``` c++
private Q_SLOTS:
  void updateStyle();
```

在新属性中添加以存储属性本身 : `std::unique_ptr<rviz_common::properties::ColorProperty> color_property_;`

<span id="cpp-updates"></span>

### Cpp 更新

- `#include <rviz_common/properties/parse_color.hpp>` - 包含将属性转换为 OGRE 颜色的辅助函数。

- 敬我们 `onInitialize` 我们添加

``` c++
color_property_ = std::make_unique<rviz_common::properties::ColorProperty>(
    "Point Color", QColor(36, 64, 142), "Color to draw the point.", this, SLOT(updateStyle()));
updateStyle();
```

- 以名称、默认值、描述和召回来构建对象。

- 我们叫 `updateStyle` 直接使颜色设置在初始,甚至在属性改变之前。

- 然后,我们定义回调。

``` c++
void PointDisplay::updateStyle()
{
  Ogre::ColourValue color = rviz_common::properties::qtToOgre(color_property_->getColor());
  point_shape_->setColor(color);
}
```

结果应该是这样的:

[![带有色彩属性的截图](images/Step3A.png)](images/Step3A.png)

哦,粉红色的!

[![颜色改变的截图](images/Step3B.png)](images/Step3B.png) <span id="status-report"></span>

## 情况报告

您可以使用分支名称查看此步骤的完整版本 `step4`.

您也可以设置显示的状态。 作为任意的例子, 让我们在 X 坐标为负时显示显示警告, 因为为什么不 ? in `processMessage`:

``` c++
if (msg->x < 0) {
  setStatus(StatusProperty::Warn, "Message",
      "I will complain about points with negative x values.");
} else {
  setStatus(StatusProperty::Ok, "Message", "OK");
}
```

- 我们假设是先前 `using rviz_common::properties::StatusProperty;` 声明。

- 将密钥/ 等价配对的状态考虑一下, 密钥是一些字符串( 我们在此使用 ) `"Message"`)和值是状态级别(error/warn/ok)和描述(一些其他字符串).

[![状态为 OK 的截图](images/Step4A.png)](images/Step4A.png) [![带有警告状态的截图](images/Step4B.png)](images/Step4B.png) <span id="cleanup"></span>

## 清理

现在, 是时候清理一下它了。 这让事情看起来更加美好, 并且更容易使用, 但并不严格要求。 您可以用分支名称查看此步骤的完整版本 。 `step5`.

首先,我们更新插件声明.

``` xml
<library path="point_display">
  <class name="Point2D" type="rviz_plugin_tutorial::PointDisplay" base_class_type="rviz_common::Display">
    <description>Tutorial to display a point</description>
    <message_type>rviz_plugin_tutorial_msgs/msg/Point2D</message_type>
  </class>
</library>
```

- 我们加了 `name` 字段改为 `class` 标记。这改变了在 RViz 中显示的名称。在代码中,称它为 `PointDisplay` 但是在RViz,我们想简化。

- 我们把文字写进描述中。 不要懒惰。

- 通过在此声明特定消息类型, 当您试图按主题添加一个显示时, 它会为该类型的主题推荐此插件 。

我们还为插件添加图标 。 `icons/classes/Point2D.png`。文件夹是硬编码的,文件名应该与插件声明的名称(或者没有指定类别的名称)相匹配。 [\[虹源\]](https://commons.wikimedia.org/wiki/File:Free_software_icon.svg)

我们需要在CMake中安装图像文件 。

``` cmake
install(FILES icons/classes/Point2D.png
        DESTINATION share/${PROJECT_NAME}/icons/classes
)
```

现在,在添加显示时,它应该以图标和描述出现.

[![带有添加图标和描述的截图](images/Step5A.png)](images/Step5A.png)

以下是试图按主题添加时的显示 :

[![按主题对话框添加的截图](images/Step5B.png)](images/Step5B.png)

最后,这里是标准界面中的图标:

[![在标准界面中带有图标的截图](images/Step5C.png)](images/Step5C.png)

注意,如果更改插件名称,以前的 RViz 配置将不再有效 。
