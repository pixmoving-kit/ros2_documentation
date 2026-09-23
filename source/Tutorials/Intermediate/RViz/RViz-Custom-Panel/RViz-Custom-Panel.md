---
translation_status: machine_translated
source: Tutorials/Intermediate/RViz/RViz-Custom-Panel/RViz-Custom-Panel.rst
---

<span id="building-a-custom-rviz-panel"></span>

# 构建自定义 RViz 面板

这个教程是给那些想在RViz环境中工作的人的,要么在二维环境中显示,要么与一些数据互动.

在这个教程中,您将学会如何在 RViz 内做三件事:

- 在 RViz 内创建一个新的 QT 面板 。

- 在 RViz 内创建一个主题订阅器,可以监视该主题上发布的消息,并在 RViz 面板内显示它们.

- 在 RViz 发布范围内创建这样的主题发布器, 将按钮按到 ROS 中的输出主题 。

此教程的全部代码可见于 [此仓库](https://github.com/MetroRobots/rviz_panel_tutorial).

<span id="boilerplate-code"></span>

## Boilerplate 代码

<span id="header-file"></span>

### 页眉文件

内容如下: `demo_panel.hpp`

``` c++
#ifndef RVIZ_PANEL_TUTORIAL__DEMO_PANEL_HPP_
#define RVIZ_PANEL_TUTORIAL__DEMO_PANEL_HPP_

#include <rviz_common/panel.hpp>

namespace rviz_panel_tutorial
{
class DemoPanel
  : public rviz_common::Panel
{
  Q_OBJECT
public:
  explicit DemoPanel(QWidget * parent = 0);
  ~DemoPanel() override;
};
}  // namespace rviz_panel_tutorial

#endif  // RVIZ_PANEL_TUTORIAL__DEMO_PANEL_HPP_
```

- 我们正在扩展 [rviz_common::Panel](https://github.com/ros2/rviz/blob/9a94bdf2f5f92ccdac4037c9268b95940845d609/rviz_common/include/rviz_common/panel.hpp#L46) 班级。

- [基于此教程范围之外的原因](https://doc.qt.io/qt-5/moc.html),你需要的是一个 `Q_OBJECT` 宏在里面,以使图形界面的QT部分发挥作用。

- 我们首先宣布一个建筑和破坏器 由Cpp文件执行

<span id="source-file"></span>

### 源文件

`demo_panel.cpp`

``` c++
#include <rviz_panel_tutorial/demo_panel.hpp>

namespace rviz_panel_tutorial
{
DemoPanel::DemoPanel(QWidget* parent) : Panel(parent)
{
}

DemoPanel::~DemoPanel() = default;
}  // namespace rviz_panel_tutorial

#include <pluginlib/class_list_macros.hpp>
PLUGINLIB_EXPORT_CLASS(rviz_panel_tutorial::DemoPanel, rviz_common::Panel)
```

- 超越建筑师和解构师并非绝对必要,但我们以后可以做更多的事情。

- 为了让RViz找到我们的插件,我们需要这个 `PLUGINLIB` 在我们的守则中援引(以及下文其他内容)。

<span id="package-xml"></span>

### package.xml

我们需要我们的软件包中的以下依赖性. xml:

``` xml
<depend>pluginlib</depend>
<depend>rviz_common</depend>
```

<span id="rviz-common-plugins-xml"></span>

### rviz_common_plugins.xml

``` xml
<library path="demo_panel">
  <class type="rviz_panel_tutorial::DemoPanel" base_class_type="rviz_common::Panel">
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

set(CMAKE_AUTOMOC ON)
qt5_wrap_cpp(MOC_FILES
  include/rviz_panel_tutorial/demo_panel.hpp
)

add_library(demo_panel src/demo_panel.cpp ${MOC_FILES})
target_include_directories(demo_panel PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)
ament_target_dependencies(demo_panel
  pluginlib
  rviz_common
)
install(TARGETS demo_panel
        EXPORT export_rviz_panel_tutorial
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
ament_export_targets(export_rviz_panel_tutorial)
pluginlib_export_plugin_description_file(rviz_common rviz_common_plugins.xml)
```

- 要生成适当的 Qt 文件, 我们需要

  - 转弯 `CMAKE_AUTOMOC` 继续

  - 通过调用来环绕信头 `qt5_wrap_cpp` 每个标题都有 `Q_OBJECT` 在它里面。 进它。

  - 包含 `MOC_FILES` 在库 与我们的其他 Cpp 文件。

- 许多其他代码确保插件部分工作。 即调用 `pluginlib_export_plugin_description_file` 要让 RViz 找到您的新插件, 关键是 。

<span id="testing-it-out"></span>

### 试试看

编译您的代码, 源代码您的工作空间并运行 `rviz2`.

在顶端菜单栏中,应该有一个“小组”菜单。从该菜单中选择“添加新面板”。

[![添加新面板对话框的截图](images/Select0.png)](images/Select0.png)

一个对话框将弹出显示所有在 ROS 环境中可以访问的面板, 根据 ROS 软件包分组到文件夹中。 通过双击其名称或选择其并单击“ 确定” 来创建您面板的新实例 。

这将在您的 RViz 窗口中创建一个新面板, 尽管它只包含一个标题栏, 上面有您面板的名称 。

[![显示新的简单面板的整个 RViz 窗口的截图](images/RViz0.png)](images/RViz0.png) <span id="filling-in-the-panel"></span>

## 填充小组成员

我们准备用一些非常基本的ROS/QT交互来更新我们的面板。我们要做的大致是访问RViz内部的ROS节点,这些节点既可以订阅,也可以发布ROS主题。我们将利用我们的订阅者来监视一个 RViz 中的 ROS 节点。 `/input` 显示已出版的 ROS 中的主题 `String` 元件中的值。 我们使用我们的出版商将 RViz 内部的按钮映射到在 ROS 主题上发布的消息 。 `/output` .

<span id="updated-header-file"></span>

### 更新了信头文件

更新 `demo_panel.hpp` 包括以下内容和类体。

``` c++
#include <rviz_common/panel.hpp>
#include <rviz_common/ros_integration/ros_node_abstraction_iface.hpp>
#include <std_msgs/msg/string.hpp>
#include <QLabel>
#include <QPushButton>

namespace rviz_panel_tutorial
{
class DemoPanel : public rviz_common::Panel
{
  Q_OBJECT
public:
  explicit DemoPanel(QWidget * parent = 0);
  ~DemoPanel() override;

  void onInitialize() override;

protected:
  std::shared_ptr<rviz_common::ros_integration::RosNodeAbstractionIface> node_ptr_;
  rclcpp::Publisher<std_msgs::msg::String>::SharedPtr publisher_;
  rclcpp::Subscription<std_msgs::msg::String>::SharedPtr subscription_;

  void topicCallback(const std_msgs::msg::String & msg);

  QLabel* label_;
  QPushButton* button_;

private Q_SLOTS:
  void buttonActivated();
};
}  // namespace rviz_panel_tutorial
```

- 在ROS方面,我们宣布了一个抽象的节点指针,我们将用它来创建更广泛的ROS生态系统的接口。我们有一个用户,它将使我们能够从ROS中获取信息并在RViz中使用。出版商允许我们从RViz内部发布信息/活动,并在ROS中提供这些信息/活动。我们还拥有一种初始化方法来建立ROS组件(ROS) 。`onInitialize`)和订户的回调(`topicCallback`).

- 在QT方面,我们声明一个标签和一个按钮,以及一个按钮的回调(`buttonActivated`).

<span id="updated-source-file"></span>

### 更新源文件

更新 `demo_panel.cpp` 具有下列内容:

``` c++
#include <rviz_panel_tutorial/demo_panel.hpp>
#include <QVBoxLayout>
#include <rviz_common/display_context.hpp>

namespace rviz_panel_tutorial
{

DemoPanel::DemoPanel(QWidget* parent) : Panel(parent)
{
  // Create a label and a button, displayed vertically (the V in VBox means vertical)
  const auto layout = new QVBoxLayout(this);
  // Create a button and a label for the button
  label_ = new QLabel("[no data]");
  button_ = new QPushButton("GO!");
  // Add those elements to the GUI layout
  layout->addWidget(label_);
  layout->addWidget(button_);

  // Connect the event of when the button is released to our callback,
  // so pressing the button results in the buttonActivated callback being called.
  QObject::connect(button_, &QPushButton::released, this, &DemoPanel::buttonActivated);
}

DemoPanel::~DemoPanel() = default;

void DemoPanel::onInitialize()
{
  // Access the abstract ROS Node and
  // in the process lock it for exclusive use until the method is done.
  node_ptr_ = getDisplayContext()->getRosNodeAbstraction().lock();

  // Get a pointer to the familiar rclcpp::Node for making subscriptions/publishers
  // (as per normal rclcpp code)
  rclcpp::Node::SharedPtr node = node_ptr_->get_raw_node();

  // Create a String publisher for the output
  publisher_ = node->create_publisher<std_msgs::msg::String>("/output", 10);

  // Create a String subscription and bind it to the topicCallback inside this class.
  subscription_ = node->create_subscription<std_msgs::msg::String>("/input", 10, std::bind(&DemoPanel::topicCallback, this, std::placeholders::_1));
}

// When the subscriber gets a message, this callback is triggered,
// and then we copy its data into the widget's label
void DemoPanel::topicCallback(const std_msgs::msg::String & msg)
{
  label_->setText(QString(msg.data.c_str()));
}

// When the widget's button is pressed, this callback is triggered,
// and then we publish a new message on our topic.
void DemoPanel::buttonActivated()
{
  auto message = std_msgs::msg::String();
  message.data = "Button clicked!";
  publisher_->publish(message);
}

}  // namespace rviz_panel_tutorial

#include <pluginlib/class_list_macros.hpp>

PLUGINLIB_EXPORT_CLASS(rviz_panel_tutorial::DemoPanel, rviz_common::Panel)
```

<span id="testing-with-ros"></span>

### 使用ROS进行测试

用您的面板重新编译并启动 RViz2。 您现在应该在面板上看到您的标签和按钮 。

[![RViz 面板默认状态的截图](images/RViz1.png)](images/RViz1.png)

为了改变标签,我们只需要在 `/input` 主题,您可以使用此命令 :

``` console
$ ros2 topic pub /input std_msgs/msg/String "{data: 'Please be kind.'}"
```

由于此部件被订阅为此话题, 它将触发回调并更改标签的文本 。

[![显示自定义字符串消息的 RViz 面板截图](images/RViz2.png)](images/RViz2.png)

按下按钮会发布一个消息,您可以通过回声看到 `/output` 主题,就像这个命令。

``` console
$ ros2 topic echo /output
```

<span id="cleanup"></span>

## 清理

现在是时候清理一下了。这让事情看起来更加美好,也更容易使用,但并没有严格的要求。

首先,您应该更新您的插件在 `rviz_common_plugins.xml`

我们还为插件添加图标 。 `icons/classes/DemoPanel.png`。文件夹是硬编码的,文件名应该与插件声明的名称(或者没有指定类别的名称)相匹配。

我们需要在CMake中安装图像文件 。

``` cmake
install(FILES icons/classes/DemoPanel.png
        DESTINATION share/${PROJECT_NAME}/icons/classes
)
```

现在,当您添加面板时,它应该显示一个图标和描述.

[![添加自定义图标和描述的新面板对话框的截图](images/Select1.png)](images/Select1.png)

面板还将有一个更新的图标.

[![带有自定义图标的 RViz 面板截图](images/RViz3.png)](images/RViz3.png)
