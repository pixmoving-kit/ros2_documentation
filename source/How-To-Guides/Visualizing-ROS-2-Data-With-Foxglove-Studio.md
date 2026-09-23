---
translation_status: machine_translated
source: How-To-Guides/Visualizing-ROS-2-Data-With-Foxglove-Studio.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="visualizing-ros-2-data-with-foxglove-studio"></span>

# 使用 Foxglove Studio 可视化 ROS 2 数据

[Foxglove 工作室( Foxglove 工作室)](https://foxglove.dev/studio) 是您机器人数据的开源可视化和调试工具。

它以多种方式提供,以使开发尽可能方便——它可以作为一个独立的桌面应用程序运行,通过您的浏览器访问,甚至可以自行托管在自己的域名上.

查看源代码于 [GitHub](https://www.github.com/foxglove/studio).

<span id="installation"></span>

## 安装

要使用网络应用程序,只需打开 Google Chrome 并导航到 [工作室. foxglove.dev (英语).](https://studio.foxglove.dev).

要使用 Linux 、 macOS 或 Windows 的桌面应用程序, 请直接从 [福克斯格洛夫工作室网站](https://foxglove.dev/download).

<span id="connect-to-a-data-source"></span>

## 连接到数据源

打开 Foxglove 工作室时,您会看到一个包含列表的对话框 。 [所有可能的数据源](https://foxglove.dev/docs/studio/connection/data-sources).

要连接到您的 ROS 2 堆栈, 请单击“ 打开连接 ” , 选择“ Rosbridge (ROS 1 & 2) ” 标签, 并配置您的“ WebSocket URL ”。

也可以拖动本地 ROS 2 。 `.db3` 文件直接输入应用程序,以装入它们进行回放。

> **说明**
>
> 以图 [在您的 ROS 2 文件内装入自定义信件定义](https://github.com/ros2/rosbag2/issues/782)中,尝试将它们转换为 [MCAP 文件格式](https://mcap.dev).

检查一下 [Foxglove 工作室文件](https://foxglove.dev/docs/studio/connection/native) 更详细的指示。

<span id="building-layouts-with-panels"></span>

## 带有面板的建筑物布局

[小组讨论会](https://foxglove.dev/docs/studio/panels/introduction) 是模块化可视化接口,可以配置和安排到 Studio [版式](https://foxglove.dev/docs/studio/layouts)。您也可以保存您的布局, 供将来使用, 供您个人参考或与您更大的机器人团队使用 。

在边栏的“ 添加面板” 标签中查找可用的面板的完整列表 。

我们强调以下一些特别有益的内容:

<span id="d-display-visualization-markers-in-a-3d-scene"></span>

### 1 3D: 在3D 场景中显示可视化标记

发布标记信息, 将原始形状( 窄形、 球体等) 和更加复杂的可视化( 占用网格、 点云等) 添加到您的 3D 面板的场景中 。

选择您要通过左边的选题程序显示的主题, 并在“ 编辑主题设置” 菜单中配置每个主题的可视化设置 。

[![Foxglove 工作室的 3D 面板](foxglove-studio/3d.png)](foxglove-studio/3d.png)

参考: [文档](https://foxglove.dev/docs/studio/panels/3d) 完整清单 [已支持的消息类型](https://foxglove.dev/docs/studio/panels/3d#supported-messages) 和一些有用的东西 [用户交互](https://foxglove.dev/docs/studio/panels/3d#user-interactions).

<span id="diagnostics-filter-and-sort-diagnostics-messages"></span>

### 2 诊断:过滤和排序诊断信息

显示从主题中看到节点( 即 stale、 错误、 警告或 OK) 的状态 `diagnostic_msgs/msg/DiagnosticArray` 在运行中的种子中数据类型,并显示给定的诊断数据 `diagnostic_name/hardware_id`.

[![Foxglove Studio的诊断面板](foxglove-studio/diagnostics.png)](foxglove-studio/diagnostics.png)

参考: [文档](https://foxglove.dev/docs/studio/panels/diagnostics) 更多细节。

<span id="image-view-camera-feed-images"></span>

### 3 图像: 查看相机种子图像

选择一个 `sensor_msgs/msg/Image` 或 时 间 `sensor_msgs/msg/CompressedImage` 要显示的话题 。

[![Foxglove 工作室的图像面板](foxglove-studio/image.png)](foxglove-studio/image.png)

参考: [文档](https://foxglove.dev/docs/studio/panels/image) 更多细节。

<span id="log-view-log-messages"></span>

### 4 日志: 查看日志消息

查看 `rcl_interfaces/msg/Log` 信件实时, 使用桌面应用程序 [连接](https://foxglove.dev/docs/studio/connection/native) 到您运行的 ROS 堆栈。 要查看 `rcl_interfaces/msg/Log` 预录数据文件中的消息,您可以将文件拖放到任意一个 [网页](https://studio.foxglove.dev) 或桌面应用程序。

下一个,添加一个 [日志](https://foxglove.dev/docs/studio/panels/log) 面板到您的布局。 如果您已经正确连接到您的 ROS 堆栈, 您现在应该看到您的日志信件列表, 并能够通过节点名称或重度级别过滤它们 。

参考: [文档](https://foxglove.dev/docs/studio/panels/log) 更多细节。

<span id="plot-plot-arbitrary-values-over-time"></span>

### 5 绘图: 绘图任意值随时间推移

从您主题的信息路径中绘制任意的值, 并重播时间 。

指定您要沿 Y 轴绘制的主题值。 对于 x 轴, 请在绘制 Y 轴值的时间戳、 元素索引或其它自定义主题消息路径之间选择 。

[![Foxglove 工作室的绘图面板](foxglove-studio/plot.png)](foxglove-studio/plot.png)

参考: [文档](https://foxglove.dev/docs/studio/panels/plot) 更多细节。

<span id="raw-messages-view-incoming-topic-messages"></span>

### 6 Raw 信件: 查看收到的专题信件

以易于读取的 JSON 树格式显示收到的主题数据 。

[![Foxglove 工作室的原始信件面板](foxglove-studio/raw-messages.png)](foxglove-studio/raw-messages.png)

参考: [文档](https://foxglove.dev/docs/studio/panels/raw-messages) 更多细节。

<span id="teleop-teleoperate-your-robot"></span>

### 7 Teleop: 操作您的机器人

通过发布操作物理机器人 `geometry_msgs/msg/Twist` 给定主题的留言返回到您的 ROS 实时堆栈 。

[![Foxglove Studio的URDF查看器面板](foxglove-studio/teleop.png)](foxglove-studio/teleop.png)

参考: [文档](https://foxglove.dev/docs/studio/panels/teleop) 更多细节。

<span id="urdf-viewer-view-and-manipulate-your-urdf-model"></span>

### 8 URDF 查看器: 查看和操纵您的URDF模型

要在 Foxglove Studio 中可视化并控制您的机器人模型,请打开网页或桌面应用程序并添加一个 [URDF 查看器](https://foxglove.dev/docs/studio/panels/urdf-viewer) 将您的 URDF 文件拖入面板, 以可视化您的机器人模型 。

[![Foxglove Studio的URDF查看器面板](foxglove-studio/urdf.png)](foxglove-studio/urdf.png)

选择任何正在发布的主题 a `JointState` 根据已公布的联合状态更新可视化的信息(默认为 `/joint_states`).

切换到“人工联合控制”,以便利用所提供的控制建立联合阵地。

[![Foxglove Studio的URDF查看器面板,具有可编辑的联合位置](foxglove-studio/urdf-joints.png)](foxglove-studio/urdf-joints.png)

参考: [文档](https://foxglove.dev/docs/studio/panels/urdf-viewer) 更多细节。

<span id="other-basic-actions"></span>

## 其他基本行动

<span id="view-your-ros-graph"></span>

### 1 查看您的 ROS 图表

[使用桌面应用程序](https://foxglove.dev/download), [连接](https://foxglove.dev/docs/studio/connection/native) 在运行中的 ROS 堆栈中。下一步添加一个 [专题图](https://foxglove.dev/docs/studio/panels/topic-graph) 面板到您的布局。 如果您正确连接了 ROS 堆栈, 您现在应该在面板上看到一个 ROS 节点、 话题和服务 的计算图。 请使用面板右侧的控件来选择要显示或切换哪些主题的服务 。

<span id="view-and-edit-your-ros-params"></span>

### 2 查看和编辑您的 ROS 参数

[使用桌面应用程序](https://foxglove.dev/download), [连接](https://foxglove.dev/docs/studio/connection/native) 在运行中的 ROS 堆栈中。下一步添加一个 [参数](https://foxglove.dev/docs/studio/panels/parameters) 面板到您的布局。 如果您已经正确连接到您的 ROS 堆栈, 您现在应该看到当前状态的实时视图 。 `rosparams`。您可以编辑这些参数值以发布 `rosparam` 更新到您的 ROS 堆栈 。

<span id="publish-messages-back-to-your-live-ros-stack"></span>

### 3 将信件发布到您的 ROS 实时堆栈

[使用桌面应用程序](https://foxglove.dev/download), [连接](https://foxglove.dev/docs/studio/connection/native) 在运行中的 ROS 堆栈中。下一步添加一个 [发布](https://foxglove.dev/docs/studio/panels/publish) 面板到您的布局。

指定您要发布的主题以推断其数据类型,并用 JSON 消息模板填充文本字段。

在普通ROS数据类型的下拉中选择一个数据类型,也会以JSON消息模板填充文本字段.

编辑模板,以便在点击“ Publish” 之前自定义您的消息 。

[![Foxglove 工作室的出版面板](foxglove-studio/publish.png)](foxglove-studio/publish.png)
