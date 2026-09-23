<span id="visualizing-ros-2-data-with-foxglove-studio"></span>
# 使用 Foxglove Studio 可视化 ROS 2 数据

[Foxglove Studio](https://foxglove.dev/studio) 是一个用于机器人数据的开源可视化和调试工具。

为方便开发，它提供多种使用方式：独立桌面应用、浏览器访问，或在自己的域名上自行部署。

源码位于 [GitHub](https://www.github.com/foxglove/studio)。

<span id="installation"></span>
## 安装

要使用网页应用，只需在 Google Chrome 中打开 [studio.foxglove.dev](https://studio.foxglove.dev)。

要使用 Linux、macOS 或 Windows 桌面应用，可直接从 [Foxglove Studio 网站](https://foxglove.dev/download)下载。

<span id="connect-to-a-data-source"></span>
## 连接数据源

打开 Foxglove Studio 后，会显示一个列出[所有可用数据源](https://foxglove.dev/docs/studio/connection/data-sources)的对话框。

要连接 ROS 2 系统，点击 “Open connection”，选择 “Rosbridge (ROS 1 & 2)” 标签页，并配置 “WebSocket URL”。

也可以将本地 ROS 2 `.db3` 文件直接拖入应用，加载后进行回放。

!!! note "说明"
    要[加载 ROS 2 文件中的自定义消息定义](https://github.com/ros2/rosbag2/issues/782)，可以尝试将文件转换为 [MCAP 格式](https://mcap.dev)。

更详细的操作说明见 [Foxglove Studio 文档](https://foxglove.dev/docs/studio/connection/native)。

<span id="building-layouts-with-panels"></span>
## 使用面板构建布局

[面板](https://foxglove.dev/docs/studio/panels/introduction)是模块化的可视化界面，可以进行配置和排列，组成 Studio 的[布局](https://foxglove.dev/docs/studio/layouts)。布局可以保存，供自己或机器人团队以后复用。

侧边栏的 “Add panel” 标签页列出了全部可用面板。

下面介绍几个特别实用的面板。

<span id="d-display-visualization-markers-in-a-3d-scene"></span>
### 1 3D：在三维场景中显示可视化标记

发布标记消息，可在 3D 面板的场景中添加箭头、球体等基本形状，以及占据栅格、点云等更复杂的可视化内容。

通过左侧的话题选择器选择要显示的话题，再在 “Edit topic settings” 菜单中配置每个话题的可视化设置。

![Foxglove Studio 的 3D 面板](foxglove-studio/3d.png){ width="500" }

[文档](https://foxglove.dev/docs/studio/panels/3d)列出了全部[支持的消息类型](https://foxglove.dev/docs/studio/panels/3d#supported-messages)和实用的[交互操作](https://foxglove.dev/docs/studio/panels/3d#user-interactions)。

<span id="diagnostics-filter-and-sort-diagnostics-messages"></span>
### 2 Diagnostics：筛选和排序诊断消息

持续显示类型为 `diagnostic_msgs/msg/DiagnosticArray` 的话题所报告的节点状态（过期、错误、警告或正常），并查看指定 `diagnostic_name/hardware_id` 的诊断数据。

![Foxglove Studio 的 Diagnostics 面板](foxglove-studio/diagnostics.png){ width="500" }

详见[文档](https://foxglove.dev/docs/studio/panels/diagnostics)。

<span id="image-view-camera-feed-images"></span>
### 3 Image：查看相机图像

选择一个 `sensor_msgs/msg/Image` 或 `sensor_msgs/msg/CompressedImage` 话题进行显示。

![Foxglove Studio 的 Image 面板](foxglove-studio/image.png){ width="500" }

详见[文档](https://foxglove.dev/docs/studio/panels/image)。

<span id="log-view-log-messages"></span>
### 4 Log：查看日志消息

要实时查看 `rcl_interfaces/msg/Log` 消息，使用桌面应用[连接](https://foxglove.dev/docs/studio/connection/native)正在运行的 ROS 系统。要查看预先录制的数据文件中的日志消息，可以将文件拖入[网页应用](https://studio.foxglove.dev)或桌面应用。

然后向布局添加 [Log 面板](https://foxglove.dev/docs/studio/panels/log)。正确连接 ROS 系统后，应能看到日志消息列表，并可按节点名称或严重程度筛选。

详见[文档](https://foxglove.dev/docs/studio/panels/log)。

<span id="plot-plot-arbitrary-values-over-time"></span>
### 5 Plot：绘制任意数值随时间变化的曲线

根据话题消息路径中的任意数值，绘制其随回放时间变化的曲线。

为 y 轴指定希望绘制的话题数值。x 轴可选择该 y 轴数值的时间戳、元素索引，或另一条自定义话题消息路径。

![Foxglove Studio 的 Plot 面板](foxglove-studio/plot.png){ width="500" }

详见[文档](https://foxglove.dev/docs/studio/panels/plot)。

<span id="raw-messages-view-incoming-topic-messages"></span>
### 6 Raw Messages：查看收到的话题消息

以易于阅读、可折叠的 JSON 树显示收到的话题数据。

![Foxglove Studio 的 Raw Messages 面板](foxglove-studio/raw-messages.png){ width="500" }

详见[文档](https://foxglove.dev/docs/studio/panels/raw-messages)。

<span id="teleop-teleoperate-your-robot"></span>
### 7 Teleop：遥控机器人

向实时 ROS 系统中的指定话题发布 `geometry_msgs/msg/Twist` 消息，即可遥控实体机器人。

![Foxglove Studio 的 Teleop 面板](foxglove-studio/teleop.png){ width="300" }

详见[文档](https://foxglove.dev/docs/studio/panels/teleop)。

<span id="urdf-viewer-view-and-manipulate-your-urdf-model"></span>
### 8 URDF Viewer：查看和操作 URDF 模型

要在 Foxglove Studio 中可视化并控制机器人模型，打开网页或桌面应用，向布局添加 [URDF Viewer 面板](https://foxglove.dev/docs/studio/panels/urdf-viewer)，然后将 URDF 文件拖入该面板。

![Foxglove Studio 的 URDF Viewer 面板](foxglove-studio/urdf.png){ width="300" }

选择任意发布 `JointState` 消息的话题，即可根据发布的关节状态更新可视化模型；默认使用 `/joint_states`。

切换到 “Manual joint control”，即可通过界面控件设置关节位置。

![可编辑关节位置的 URDF Viewer 面板](foxglove-studio/urdf-joints.png){ width="500" }

详见[文档](https://foxglove.dev/docs/studio/panels/urdf-viewer)。

<span id="other-basic-actions"></span>
## 其他基本操作

<span id="view-your-ros-graph"></span>
### 1 查看 ROS 计算图

[使用桌面应用](https://foxglove.dev/download)[连接](https://foxglove.dev/docs/studio/connection/native)正在运行的 ROS 系统，然后向布局添加 [Topic Graph 面板](https://foxglove.dev/docs/studio/panels/topic-graph)。正确连接后，面板中会显示 ROS 节点、话题和服务组成的计算图。通过面板右侧的控件，可以选择显示哪些话题，或切换服务的显示。

<span id="view-and-edit-your-ros-params"></span>
### 2 查看和编辑 ROS 参数

[使用桌面应用](https://foxglove.dev/download)[连接](https://foxglove.dev/docs/studio/connection/native)正在运行的 ROS 系统，然后向布局添加 [Parameters 面板](https://foxglove.dev/docs/studio/panels/parameters)。正确连接后，即可实时查看当前 `rosparams`。编辑参数值，会将 `rosparam` 更新发布回 ROS 系统。

<span id="publish-messages-back-to-your-live-ros-stack"></span>
### 3 向实时 ROS 系统发布消息

[使用桌面应用](https://foxglove.dev/download)[连接](https://foxglove.dev/docs/studio/connection/native)正在运行的 ROS 系统，然后向布局添加 [Publish 面板](https://foxglove.dev/docs/studio/panels/publish)。

指定要发布到的话题，应用会推断其数据类型，并在文本框中填入 JSON 消息模板。

也可以在常用 ROS 数据类型下拉列表中选择类型，同样会生成 JSON 消息模板。

编辑模板以定制消息，然后点击 “Publish”。

![Foxglove Studio 的 Publish 面板](foxglove-studio/publish.png){ width="300" }
