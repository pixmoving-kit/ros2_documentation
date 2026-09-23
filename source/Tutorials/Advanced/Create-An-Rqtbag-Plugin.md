---
translation_status: machine_translated
source: Tutorials/Advanced/Create-An-Rqtbag-Plugin.rst
---

<span id="create-an-rqt-bag-plugin"></span>

# 创建 rqt_bag 插件

假设您有包文件, 您想要创建一些数据的自定义可视化 。 `rqt_bag` 赋予您通过录制的信息滚动和可视化原始信息值的能力。

``` console
$ ros2 run rqt_bag rqt_bag ~/path/to/BagFile
$ rqt_bag ~/path/to/BagFile                     # alternative
```

这提供了一个标准的统一可视化:

![标准 rqt_bag 视图的截图](images/rqtbag_plugin_base.png)

然而,您有时可能需要一个更视觉的演示文稿,或者需要在原始信件上做一些后处理。为此,您可以写一个 `rqt_bag` 插件 , 使用 Python 插件系统。 这样您就可以对信件进行定制的可视化 :

![右侧带有彩色时间表和额外面板的 rqt_bag 截图](images/rqtbag_plugin_full.png) <span id="some-test-data"></span>

## 一些测试数据

在这个教程中,我们将使用 `level` 区域 [diagnostic_msgs/msg/DiagnosticStatus](https://docs.ros.org/en/rolling/p/diagnostic_msgs/msg/DiagnosticStatus.html) 消息。下面是一个简单的脚本,用于随机生成诊断状态。您可以从此脚本中记录自己的包,或者使用 [此样本数据](https://github.com/MetroRobots/rqt_bag_diagnostics_demo/raw/refs/heads/main/SomeDiagnostics.zip) 一旦你打开它。

``` python
from diagnostic_msgs.msg import DiagnosticStatus
import random
import rclpy
from rclpy.node import Node

MODES = ['OK', 'WARN', 'ERROR']

class DiagnosticPub(Node):
    def __init__(self):
        super().__init__('diagnostic_pub')
        self.last_status = None
        self.publisher = self.create_publisher(DiagnosticStatus, '/diagnostics', 10)
        self.timer = self.create_timer(1, self.callback)

    def callback(self):
        if self.last_status is None:
            # Random initial status
            status = random.randint(0, len(MODES))
        elif random.randint(0, 5) != 0:
            # Do not publish a msg every cycle
            return
        else:
            # Random new (different) status
            delta = random.randint(1, 2)
            status = (self.last_status + delta) % len(MODES)

        self.get_logger().info(f'Publishing {MODES[status]} status')
        self.publisher.publish(DiagnosticStatus(level=bytes(status)))
        self.last_status = status


def main(args=None):
    rclpy.init(args=args)
    node = DiagnosticPub()
    rclpy.spin(node)


if __name__ == '__main__':
    main()
```

<span id="package-setup"></span>

## 软件包设置

我们准备制作一个名为“我们”的软件包 `rqt_bag_diagnostics_demo`创建基础 `ament_python` 软件包,例如通过调用:

``` console
$ ros2 pkg create --build-type ament_python --dependencies diagnostic_msgs python_qt_binding rqt_bag \
  --description "rqt_bag plugin for diagnostics_msgs" --license Apache-2.0 \
  --maintainer-name "My Name" --maintainer-email "my@name.robots" \
  rqt_bag_diagnostics_demo
```

编辑生成部分的相关部分 `package.xml` 看起来像这样:

``` xml
<exec_depend>diagnostic_msgs</exec_depend>
<exec_depend>python_qt_binding</exec_depend>
<exec_depend>rqt_bag</exec_depend>
<export>
  <build_type>ament_python</build_type>
  <rqt_bag plugin="${prefix}/plugins.xml"/>
</export>
```

我们在这里所做的是让我们的软件包依赖于rqt_bag, python_qt_绑定和诊断_msgs软件包,然后导出一个定义我们的rqt_bag插件的XML文件。 in `setup.py`,添加以下一行

``` python
('share/' + package_name, ['plugins.xml']),
```

页:1 `data_files`.

接着,我们将在名为 XML 的文件内定义插件 `plugins.xml` (参见 `package.xml`。此文件描述了此套件提供的所有插件( 每个套件可以有多个插件) 。

``` xml
<library path=".">
  <class name="DiagnosticBagPlugin"
         type="rqt_bag_diagnostics_demo.the_plugin.DiagnosticBagPlugin"
         base_class_type="rqt_bag::Plugin">
    <description>Awesome Diagnostic</description>
  </class>
</library>
```

那个... `name` 属性是我们创建的插件的名称。它必须是在所有插件中独有的,但您不会以任何其他方式使用它。 `type` 属性 是我们在 Python 中导入插件类的方式, 即 。 `package_name.module_name.class_name`

<span id="defining-the-plugin"></span>

## 定义插件

现在我们需要实际执行 `the_plugin.py` Python 模块(参考于 `plugins.xml`。首先,确保有一个空文件 `__init__.py` 输入 `rqt_bag_diagnostics_demo` 子文件夹,将其变成 Python 包。

> **说明**
>
> 请注意,根据ROS中目前的Python标准,带有ROS包的文件夹(Python).`rqt_bag_diagnostics_demo`) 包含一个同名的子文件夹。 因此, 完整路径是 。 `WORKSPACE/src/rqt_bag_diagnostics_demo/rqt_bag_diagnostics_demo/__init__.py`.

现在创建 `the_plugin.py` 旁边 `__init__.py`。此文件将包含插件的全部代码。

首先,核心插件类.

``` python
from rqt_bag.plugins.plugin import Plugin
from python_qt_binding.QtCore import Qt
from diagnostic_msgs.msg import DiagnosticStatus


def get_color(diagnostic):
    if diagnostic.level == DiagnosticStatus.OK:
        return Qt.green
    elif diagnostic.level == DiagnosticStatus.WARN:
        return Qt.yellow
    else:  # ERROR or STALE
        return Qt.red


class DiagnosticBagPlugin(Plugin):
    def __init__(self):
        pass

    def get_view_class(self):
        # This method is required; we will implement it later
        return None

    def get_renderer_class(self):
        return None

    def get_message_types(self):
        return ['diagnostic_msgs/msg/DiagnosticStatus']
```

在这里,我们有一些基本进口, 和帮助器功能,我们以后会使用, 和一个类别,定义一个的三部分 `rqt_bag` 插件 。

> 1.  `view_class` - a.k.a. `TopicMessageView` - 一个单独的面板,可用于查看单个消息。
>
> 2.  `renderer_class` - a.k.a. `TimelineView` - 一个工具,用于绘制袋数据的时间表视图。
>
> 3.  `message_types` - 定义该插件可以使用何种消息的字符串。您可以返回 `['*']` 以应用到所有信件中。

自从我们返回前两种方法“无”之后,这个插件不会做任何事情。我们将分别处理每个插件。

<span id="topicmessageview"></span>

## 专题信箱

<span id="version-1"></span>

### 第1版 语 文

我们将创造出一个能扩展 `TopicMessageView` 类( 仍在) `the_plugin.py`。首先,添加导入:

``` python
from rqt_bag import TopicMessageView
```

然后定义此新类 :

``` python
class DiagnosticPanel(TopicMessageView):
    name = 'Awesome Diagnostic'

    def message_viewed(self, bag, entry, ros_message, msg_type_name, topic):
        super(DiagnosticPanel, self).message_viewed(bag=bag, entry=entry, ros_message=ros_message, msg_type_name=msg_type_name, topic=topic)
        print(f'{topic}: {ros_message}')
```

在这里,我们定义了两件事。 `name` 类变量定义右键单击时 rqt_bag 显示的内容 `DiagnosticStatus` 主题在时间线中。 `message_viewed` 方法定义了选择信件时要做什么。所以,我们在此只打印信件到终端。

我们需要将我们创建的这一类连接到插件基础设施中,为此,我们返回该类对象本身在 `get_view_class` 方法。

``` python
def get_view_class(self):
    return DiagnosticPanel
```

> **说明**
>
> 不键入 `return DiagnosticPanel()` (与 `()`。 。 。 。 。 。 `return DiagnosticPanel` 说对了

要看到这个行动,运行 `rqt_bag` 用您的包文件,然后右键点击诊断音轨。它将在“视频”下给出两个选项:Raw和我们的“出色诊断 ” 。单击此选项将打开一个面板,您可以滚动信件并观看其打印。

![带空白额外面板的 rqt_bag 截图](images/rqtbag_plugin_panel.png) <span id="version-2"></span>

### 第2版 维基文库中相关的原始文献

`TopicMessageView` 本身是 a 的扩展 `QObject`利用Qt的所有力量和力量,你可以做很多事情。不幸的是,这不是一个蟒蛇Qt教程, [虽然网上有许多](https://doc.qt.io/qtforpython-6/examples/example_widgets_painting_basicdrawing.html)。所以我们只需要添加一个简单的 QWidget 并借鉴它。首先,添加以下进口:

``` python
from python_qt_binding.QtWidgets import QWidget
from python_qt_binding.QtGui import QBrush, QPainter
```

然后更新 `DiagnosticPanel` 分类为:

``` python
class DiagnosticPanel(TopicMessageView):
    name = 'Awesome Diagnostic'

    def __init__(self, timeline, parent, topic):
        super(DiagnosticPanel, self).__init__(timeline, parent, topic)
        self.widget = QWidget()
        parent.layout().addWidget(self.widget)
        self.msg = None
        self.widget.paintEvent = self.paintEvent

    def message_viewed(self, bag, entry, ros_message, msg_type_name, topic):
        super(DiagnosticPanel, self).message_viewed(bag=bag, entry=entry,
                                                    ros_message=ros_message, msg_type_name=msg_type_name, topic=topic)
        self.msg = ros_message
        self.widget.update()

    def paintEvent(self, event):
        qp = QPainter()
        qp.begin(self.widget)

        rect = event.rect()

        if self.msg is None:
            qp.fillRect(0, 0, rect.width(), rect.height(), Qt.white)
        else:
            color = get_color(self.msg)
            qp.setBrush(QBrush(color))
            qp.drawEllipse(0, 0, rect.width(), rect.height())
        qp.end()
```

在建筑师中,我们创造了一个 `QWidget` 并覆盖其 `paintEvent` 方法。现在,当我们收到一个消息 `message_viewed`,我们保存它,并更新部件, 它反过来会呼唤我们 `paintEvent`不调用 `paintEvent` 由 Qt 手动完成。 在选择一个消息之前, 我们只需要画一个白色的矩形。 否则, 我们就会画一个圆圈, 使用手动帮助方法将颜色与诊断的级别联系起来 。

![在额外面板上绘制圆形的 rqt_bag 截图](images/rqtbag_plugin_circle.png) <span id="timelinerenderer"></span>

## 时间线渲染器

<span id="version-1-1"></span> <span id="id1"></span>

### 第1版 语 文

为了利用时间表,我们延长了 `TimelineRenderer` 类( 仍在) `the_plugin.py`。添加导入 :

``` python
from rqt_bag import TimelineRenderer
```

后加新类.

``` python
class DiagnosticTimeline(TimelineRenderer):
    def __init__(self, timeline, height=80):
        TimelineRenderer.__init__(self, timeline, msg_combine_px=height)

    def draw_timeline_segment(self, painter: QPainter, topic, start: float, end: float, x: float, y: int, width: float, height: int):
        painter.setBrush(QBrush(Qt.blue))
        painter.drawRect(int(x), y, int(width), height)
```

您可以自定义消息的长度与时间间隔有多高 。 `msg_combine_px` 参数。要覆盖的关键方法是 `draw_timeline_segment()` 绘制时间线。现在,我们将在每一段画蓝色的矩形。

和信件视图一样,您还需要编辑插件以返回您的类 。

``` python
def get_renderer_class(self):
    return DiagnosticTimeline
```

要看到这一点,您必须在rqt_bag gui中启用“Thumbnails”(一个误导性名称)。

![在时间线上绘制带蓝色条的 rqt_bag 截图](images/rqtbag_plugin_blue.png) <span id="version-2-1"></span> <span id="id2"></span>

### 第2版 维基文库中相关的原始文献

现在,我们实际上想要根据信件本身来定制时间线中信件的绘制方式。为此,你需要读取和去除包文件中信件的序列。以下是新的导入:

``` python
from python_qt_binding.QtGui import QPen
from rclpy.time import Time
from rclpy.serialization import deserialize_message
from rqt_bag.bag_helper import to_sec
```

然后更新 `draw_timeline_segment()`:

``` python
def draw_timeline_segment(self, painter: QPainter, topic, start: float, end: float, x: float, y: int, width: float, height: int):
    bag_timeline = self.timeline.scene()
    start_t = Time(seconds=start)
    end_t = Time(seconds=end)

    for bag, entry in bag_timeline.get_entries_with_bags([topic], start_t, end_t):
        topic, raw_data, t = bag_timeline.read_message(bag, entry.timestamp, topic)
        msg = deserialize_message(raw_data, DiagnosticStatus)
        color = get_color(msg)
        painter.setBrush(QBrush(color))
        painter.setPen(QPen(color, 5))

        t_float = to_sec(Time(nanoseconds=t))
        p_x = int(self.timeline.map_stamp_to_x(t_float))
        painter.drawLine(p_x, y, p_x, y + height - 1)
```

使用 `topic`, `start` 财务报告和财务报告 `end` 参数,我们可以得到与时间段对应的包项。然后,我们可以得到实际消息,然后用它来绘制。这里我们根据诊断消息的级别绘制一条线。我们可以自动地在水平上使用此线绘制消息。 `map_stamp_to_x()` 将浮动秒转换为部件像素的方法。

![显示时间线上不同颜色条的 rqt_bag 截图](images/rqtbag_plugin_timeline.png)

如果在时间线上计算信件表示值要求更高,则您应该使用 [时间线缓存](https://github.com/ros-visualization/rqt_bag/blob/rolling/rqt_bag/src/rqt_bag/timeline_cache.py) 喜欢 [图像时间线查看器](https://github.com/ros-visualization/rqt_bag/blob/rolling/rqt_bag_plugins/src/rqt_bag_plugins/image_timeline_renderer.py) 做,但知道这一点 留作为练习 给读者。
