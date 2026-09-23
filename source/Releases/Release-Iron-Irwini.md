---
translation_status: machine_translated
source: Releases/Release-Iron-Irwini.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="iron-irwini-iron"></span> <span id="iron-release"></span>

# Iron Irwini（`iron`)

*铁欧文尼* 以下是自上次发布以来铁Irwini的重要变化和特征的亮点。 [长窗体变化日志](Iron-Irwini-Complete-Changelog.md).

<span id="supported-platforms"></span>

## 支持的平台

Iron Irwini支持以下平台: [平台支持级别](../The-ROS2-Project/Platform-Support-Tiers.md):

第一级平台:

- 乌本图22.04 (詹姆斯): `amd64` 财务报告和财务报告 `arm64`

- Windows 10 (Visual Studio 2019): (英语). `amd64`

第二级平台:

- 莱尔9: `amd64`

第三级平台:

- 马科斯: `amd64`

- 德比安红眼: `amd64`

目标平台:

| 建筑 | 乌班图·贾米(22.04) | Windows 10 (VS2019) (英语). | 第9条 | macOS | 德比安公牛(11) | OpenEmbed / Yocto 项目 |
|----|----|----|----|----|----|----|
| amd64 (中文(简体) ). | 第1级 \[d\]\[a\]\[s\] | 第1级 \[s\] | 第二级\[d\]\[a\]\[s\] | 第3级 \[s\] | 第3级 \[s\] | 第3级 \[s\] |
| 军火64 | 第1级 \[d\]\[a\]\[s\] |  |  |  | 第3级 \[s\] | 第3级 \[s\] |
| 臂弹32 | 第3级 \[s\] |  |  |  | 第3级 \[s\] | 第3级 \[s\] |

以下指标显示每个平台都有何种交付机制。

"\[d\]" 发行专用(Debian,RPM等)包将提供给本平台,用于提交rosdistro的包.

" \[a\] " 二进制释放作为每个平台的单一档案提供,其中包含铁ROS 2 repos文件中的所有包\[^12\].

" \[s\] " 来源汇编。

中间软件执行支持 :

| 中间软件库 | 中件提供者 | 支助级别 | 平台 | 建筑 |
|----|----|----|----|----|
| rmw_fastrtps_cpp\* | eProsima Fast-DDS 软件 | 第1级 | 所有平台 | 所有建筑 |
| rmw_cyclonedds_cpp | Eclipse Cyclone DDS | 第1级 | 所有平台 | 所有建筑 |
| rmw_connextdds | RTI 连接 | 第1级 | Ubuntu, Windows, 和 macOS 软件 | 除arm64外的所有建筑 |
| rmw_fastrtps_dynamic_cpp | eProsima Fast-DDS 软件 | 第二级 | 所有平台 | 所有建筑 |
| rmw_gurumdds_cpp | GurumNetworks GurumDDS | 第3级 | Ubuntu 和 视窗 | 除arm32外的所有建筑 |

" \* " 是指未执行的《保护所有移徙工人及其家庭成员权利国际公约》。

Middleware 执行支持依赖于平台支持级别。例如,在 Tier 2 平台上执行的 Lier 1 中间软件只能得到 Tier 2 支持。

最低语文要求:

- C++17

- ⁇  3.8

依赖性要求:

<table class="docutils align-default">
<thead>
<tr class="row-odd">
<th class="head"></th>
<th colspan="2" class="head"><p>所需支助</p></th>
<th colspan="4" class="head"><p>建议的支助</p></th>
</tr>
<tr class="row-even">
<th class="head"><p>软件包</p></th>
<th class="head"><p>乌邦图查米</p></th>
<th class="head"><p>视窗10**</p></th>
<th class="head"><p>第9条</p></th>
<th class="head"><p>马科斯**</p></th>
<th class="head"><p>德比安红心</p></th>
<th class="head"><p>已打开*</p></th>
</tr>
</thead>
<tbody>
<tr class="row-odd">
<td><p>CMake</p></td>
<td><p>3.22.1</p></td>
<td><p>3.22.0</p></td>
<td><p>3.20.2</p></td>
<td><p>3.14.4</p></td>
<td><p>3.18.4</p></td>
<td><p>3.22.3 / 3.16.5***</p></td>
</tr>
<tr class="row-even">
<td><p>爱咪</p></td>
<td><p>3.3.4</p></td>
<td><p>3.3.2</p></td>
<td><p>3.3.4</p></td>
<td colspan="3"><p>3.3.2</p></td>
</tr>
<tr class="row-odd">
<td><p>Gazebo 经典</p></td>
<td><p>11.x.x*</p></td>
<td><p>N/A</p></td>
<td><p>N/A</p></td>
<td><p>11.x.x</p></td>
<td><p>11.x.x*</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>Gazebo( 点火) Name</p></td>
<td><p>堡垒*</p></td>
<td><p>N/A</p></td>
<td><p>N/A</p></td>
<td><p>堡垒*</p></td>
<td><p>堡垒*</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-odd">
<td><p>数字</p></td>
<td><p>1.21.5</p></td>
<td><p>1.18.4</p></td>
<td><p>1.20.1</p></td>
<td><p>1.18.4</p></td>
<td><p>1.19.5</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>食人鱼</p></td>
<td colspan="5"><p>1.12.1*</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-odd">
<td><p>打开CV</p></td>
<td><p>4.5.4</p></td>
<td><p>3.4.6*</p></td>
<td><p>4.6.0</p></td>
<td><p>4.2.0</p></td>
<td><p>4.5.1</p></td>
<td><p>4.1.0 / 3.2.0***</p></td>
</tr>
<tr class="row-even">
<td><p>打开SSL</p></td>
<td><p>3.0.2</p></td>
<td><p>1.1.1l</p></td>
<td><p>3.0.1</p></td>
<td><p>1.1.1f</p></td>
<td><p>1.1.1i</p></td>
<td><p>1.1.1d / 1.1.1b***</p></td>
</tr>
<tr class="row-odd">
<td><p>Python</p></td>
<td><p>3.10.6</p></td>
<td><p>3.8.3</p></td>
<td><p>3.9.14</p></td>
<td><p>3.10.8</p></td>
<td><p>3.9.1</p></td>
<td><p>3.8.2 / 3.7.5***</p></td>
</tr>
<tr class="row-even">
<td><p>Qt 键</p></td>
<td><p>5.15.3</p></td>
<td><p>5.12.12</p></td>
<td><p>5.15.3</p></td>
<td><p>5.12.3</p></td>
<td><p>5.15.2</p></td>
<td><p>5.14.1 / 5.12.5***</p></td>
</tr>
<tr class="row-odd">
<td colspan="2"></td>
<td colspan="5"><p><strong>仅限 Linux</strong></p></td>
</tr>
<tr class="row-even">
<td><p>个人计算机L</p></td>
<td><p>1.12.1</p></td>
<td><p>N/A</p></td>
<td><p>1.12.0</p></td>
<td><p>N/A</p></td>
<td><p>1.11.1</p></td>
<td><p>1.10.0</p></td>
</tr>
<tr class="row-odd">
<td colspan="7"><p><strong>RMW DDS 中间软件</strong></p></td>
</tr>
<tr class="row-even">
<td><p>Cyclone DDS</p></td>
<td colspan="6"><p>0.9</p></td>
</tr>
<tr class="row-odd">
<td><p>快速数据交换系统</p></td>
<td colspan="6"><p>2.8</p></td>
</tr>
<tr class="row-even">
<td><p>Connext DDS</p></td>
<td colspan="4"><p>6.0.1</p></td>
<td colspan="2"><p>N/A</p></td>
</tr>
<tr class="row-odd">
<td><p>Gurum 数据交换系统</p></td>
<td colspan="2"><p>2.8.x</p></td>
<td colspan="4"><p>N/A</p></td>
</tr>
</tbody>
</table>

" \* " 是指这不是上游版本(可在官方操作系统寄存器上查阅),而是OSRF或社区分发的包(自定义寄存器上建立和分发的包)。

" \*\*"是指依赖可能看到多个版本的改变,因为依赖使用一个包管理器,在没有稳定的API的情况下不断更新依赖.

" \*\*\* " WebOS OSE提供了这个不同的版本.

此文档只抓取 ROS 发行的首次发布时的版本, 并且不会随着依赖关系向前移动而更新, 因此这些版本是低水印 。

依赖软件包管理器的使用 :

- Ubuntu, Debian: apt (英语).

- 视窗:巧克力,pip

- 马科斯: 土生土长,皮普

- (原始内容存档于2019-09-31) (英语). RHEL: dnf

- 打开嵌入式: opkg

构建系统支持 :

- ament_cmake

- 制作( C)

- 设置工具

<span id="installation"></span>

## 安装

[安装 Irwini](https://docs.ros.org/en/iron/Installation.html)

<span id="new-features-in-this-ros-2-release"></span>

## 本 ROS 2 发布中的新功能

<span id="api-documentation-generation-for-python-packages"></span>

### Python 软件包的 API 文档生成

ROS 2已经为C++软件包拥有自动API文档,用于多个版本,例如. <https://docs.ros.org/en/rolling/p/rclcpp/generated/index.html>。铁为 Python 软件包也添加了自动 API 文档,例如: <https://docs.ros.org/en/rolling/p/rclpy/rclpy.html>.

见 <https://github.com/ros-infrastructure/rosdoc2/pull/28>, <https://github.com/ros-infrastructure/rosdoc2/pull/49>, <https://github.com/ros-infrastructure/rosdoc2/pull/51>,以及 <https://github.com/ros-infrastructure/rosdoc2/pull/52> 更多细节。

<span id="service-introspection"></span>

### 服务回顾

现在可以在每次服务的基础上启用服务回顾。 启用后, 用户可以查看与客户端请求某项服务相关的元数据, 服务器接受请求, 服务器发送回复, 客户端接受回复。 可选的是, 客户端/ 服务器请求/ 回复的内容也可以进行回顾。 所有信息都发布在服务名称生成的隐藏主题上 。 所以如果服务被调用 。 `/myservice`,然后将信息发布在 `/myservice/_service_event`.

注意此功能默认被禁用; 要启用, 用户必须呼叫 `configure_introspection` 在创建服务客户端或服务器之后。有实例显示如何在 <https://github.com/ros2/demos/tree/iron/demo_nodes_cpp/src/services> (C++)和 (中文(简体) ). <https://github.com/ros2/demos/blob/iron/demo_nodes_py/demo_nodes_py/services/introspection.py> (毕.

见 [REP 2012 (英语).](https://github.com/ros-infrastructure/rep/pull/360) 和跟踪错误 <https://github.com/ros2/ros2/issues/1285> 以获取更多信息。

<span id="pre-and-post-set-parameter-callback-support"></span>

### 前后设置参数调回支持

对于现在的许多版本,当一个节点上的参数被外部实体更改时,用户可以注册一个调用回调(如: `ros2 param set`。 这种回调可以检查已改变的参数类型和值,如果其中之一不符合某些标准,则拒绝全部数据。 但是,它不能修改参数列表,也不能修改状态(因为在设定之后可能还有其他回调会拒绝参数 ) 。

此版本在回调中添加。 回调按此顺序调用 :

- “预”设置参数回调,可以根据任意标准修改参数清单。

- “ 设置” 参数召回, 它不能修改列表, 只能根据其类型和值接受或拒绝这些参数( 这是现有的召回) 。

- “ post” 设置参数回调, 它可以根据参数进行状态变化, 只有在前两个回调成功时才被调用 。

这方面的例子有: <https://github.com/ros2/demos/blob/iron/demo_nodes_cpp/src/parameters/set_parameters_callback.cpp> (C++)和 (中文(简体) ). <https://github.com/ros2/demos/blob/iron/demo_nodes_py/demo_nodes_py/parameters/set_parameters_callback.py> (毕.

见 <https://github.com/ros2/rclcpp/pull/1947>, <https://github.com/ros2/rclpy/pull/966>,以及 <https://github.com/ros2/demos/pull/565> 以获取更多信息。

<span id="improved-discovery-options"></span>

### 改进的发现选项

以前的 ROS 2 版本提供了有限的发现选项。 基于 DDS 的 RMW 执行的默认行为是通过多播来发现任何可到达的节点 。 它可以通过设置环境变量来限制在同一个机器上 。 `ROS_LOCALHOST_ONLY`,但任何额外的配置都需要直接配置中间软件,通常通过中间软件特定的XML文件和环境变量进行配置. ROS Iron保留了相同的默认发现行为,但贬值 `ROS_LOCALHOST_ONLY` 支持更多的颗粒性选择。

- `ROS_AUTOMATIC_DISCOVERY_RANGE` 控制 ROS 节点将尝试发现彼此的距离。 有效的选项有 :

  - `SUBNET` - 默认,对于基于DDS的中间软件,它将通过多播发现任何可达到的节点.

  - `LOCALHOST` - 只会试着在同一台机器上发现其他节点

  - `OFF` - 不会试图自动发现任何其他节点, 即使在同一台机器上。

  - `SYSTEM_DEFAULT` - 将不会更改任何发现设置。 如果您已经为您的中间软件设置了自定义设置, 并且不希望ROS 更改这些设置, 这样做将很有用 。

- `ROS_STATIC_PEERS` - 分号(`;`) ROS 应尝试在其中发现节点的地址的分离列表。这允许用户在specifc机器上连接节点(只要其发现范围没有设定为 `OFF`).

例如,你可能拥有几个机器人 `ROS_AUTOMATIC_DISCOVERY_RANGE` 设置为 `LOCALHOST` 因此,它们不会互相沟通。当你想要将 RViz 连接到其中之一时,您会添加地址到 `ROS_STATIC_PEERS` 现在您可以使用 ROS 2 CLI 和可视化工具与机器人互动。

见 <https://github.com/ros2/ros2/issues/1359> 以获取关于此特性的更多信息。

<span id="matched-events"></span>

### 匹配事件

除了QoS事件之外,当任何出版商和订阅商建立或降低它们之间的连接时,也可以生成匹配的事件. 用户可以向每个出版商和订阅商提供匹配事件触发的回调功能,并以他们认为合适的方式处理它们,类似于处理一个话题上收到的消息的方式.

- 发布器 : 当它发现一个符合主题且兼容QoS或连接的订阅被断开时, 会发生此事件 。

- 订阅: 当发现一个与主题匹配且兼容的QoS或连接的出版商被断开时, 会发生此事件 。

见跟踪问题 <https://github.com/ros2/rmw/issues/330> 以获取更多信息。

- C++ 匹配事件的演示 : <https://github.com/ros2/demos/blob/iron/demo_nodes_cpp/src/events/matched_event_detect.cpp>

- 匹配事件的 Python Demo : <https://github.com/ros2/demos/blob/iron/demo_nodes_py/demo_nodes_py/events/matched_event_detect.py>

<span id="external-configuration-services-of-loggers"></span>

### 伐木机的外部配置服务

现在可以通过一个服务远程配置节点日志级别。 `enable_logger_service` 选项在节点创建期间启用, `set_logger_levels` 财务报告和财务报告 `get_logger_levels` 将提供服务。

请注意: `enable_logger_service` 选项默认被禁用,因此用户需要启用节点创建中的此选项。

见 <https://github.com/ros2/ros2/issues/1355> 以获取更多信息。

<span id="type-description-distribution"></span>

### 类型描述分布

现在可以将 ROS 2 消息的类型信息进行交流,这样,具有潜在不同类型同名信息的系统就可以更加透明地发现其兼容性。 由 REP-2011: Evolutioning Messages Types 的子集定义的这一系列能力在 Iron 中拥有许多部分。

首先,引入新的一揽子计划 [type_description_interfaces](https://index.ros.org/p/type_description_interfaces/github-ros2-rcl_interfaces/#iron) 提供了交流ROS 2通信接口类型(msg, srv, action)描述的通用方式.

接下来,决定了一种散列类型描述的方法,即ROS 接口哈兴标准(RIHS)——从第一个版本RIHS01开始. RIHS 散列在构建时对所有编译的ROS类型自动计算,并烤入生成的代码,以便检查它们。这些散列在发现时也会自动通信,并包含在其中. `rmw_topic_endpoint_info_t` 用于图形内向查询,例如: `get_publishers_info_by_topic`.

全数 `TypeDescription` 数据结构以及原始源文本(例如: `.msg` 文件),用来生成的,现在默认会烤到信件库中,这样它们就可以被 `typesupport` 虽然我们期望这些数据能为大多数用户提供价值,但有些用户试图在安装空间中最小化字节,可以通过定义 CMake 变量在构建 ROS 2 Core 时禁用特性 `ROSIDL_GENERATOR_C_DISABLE_TYPE_DESCRIPTION_CODEGEN`.

最后,新服务 `type_description_interfaces/GetTypeDescription.srv` 已定义可允许节点在遇到未知的RIHS 类型散列时从该类型的节点广告中请求完整定义。正在使用ROS 2 节点提供该特性,作为节点构造的可选切换。该特性尚未发运,但预计在2023年中某个时候会返回到Iron中。同时,用户节点可以使用稳定的服务接口,不小心执行这一服务。

见 [REP 2011 (英语).](https://github.com/ros-infrastructure/rep/pull/358) 关于设计建议,见 [类型描述分布](https://github.com/ros2/ros2/issues/1159) 用于跟踪功能集的开发。

<span id="dynamic-types-and-dynamic-messages"></span>

### 动态类型和动态信件

除了上述类型描述分布特征外,还有在运行时构建和访问动态创建类型(即动态类型)的能力。 `rcl`,带有新的 `rmw` 用于支持将消息作为动态消息(即由动态类型结构构建或遵循该结构生成的消息)的接口。

第一,将公用事业引入 [罗西德](https://index.ros.org/r/rosidl/github-ros2-rosidl/#iron) 帮助构建和操纵类型描述。

接下来, [rosidl_dynamic_typesupport](https://index.ros.org/r/rosidl_dynamic_typesupport/github-ros2-rosidl_dynamic_typesupport/#iron) 软件包被写入,并提供了一个中间软件不可知界面,用于在运行时构建动态类型和动态消息。类型可以在运行时通过程序构建,也可以通过解析a构建。 `type_description_interfaces/TypeDescription` 留言。

> **说明**
>
> 那个... `rosidl_dynamic_typesupport` 库需要序列化支持库来实施中间软件特定动态类型的行为。一个用于 Fast DDS 的序列化支持库已经在 [rosidl_dynamic_typesupport_fastrtps](https://index.ros.org/r/rosidl_dynamic_typesupport_fastrtps/github-ros2-rosidl_dynamic_typesupport_fastrtps/#iron). 理想的更多中间软件将执行支持库,增加支持此功能的中间软件的数量.

最后,为了支持使用动态类型和动态信息,增加了新方法。 [rmw (英语).](https://index.ros.org/r/rmw/github-ros2-rmw/#iron) 财务报告和财务报告 [rcl (中文(简体) ).](https://index.ros.org/r/rcl/github-ros2-rcl/#iron) 支持:

- 获得中度软件特定序列化支持的能力

- 在使用动态类型运行时构建消息类型支持的能力

- 使用动态类型接收动态消息的能力

正在开展工作,以便利用动态类型在客户端库中创建订阅功能(参见: `rclcpp` 用户可以使用新程序编写自己的订阅,订阅动态类型。 `rmw` 财务报告和财务报告 `rcl` 作为本功能集的一部分引入的特性。

见 [REP 2011 (英语).](https://github.com/ros-infrastructure/rep/pull/358) 关于设计建议,见 [动态订阅](https://github.com/ros2/ros2/issues/1374) 用于跟踪特性集的开发, [rclcpp](https://github.com/ros2/rclcpp/pull/2176) 需要大量的工作。 需要大量的工作。

<span id="launch"></span>

### `launch`

<span id="pythonexpression-now-supports-importing-modules"></span>

#### `PythonExpression` 现在支持导入模块

现在可以发射了 `PythonExpression` 输入模块后再进行评价。这可用于拉动用于评价表达式的其他功能。

见 <https://github.com/ros2/launch/pull/655> 以获取更多信息。

<span id="readytotest-can-be-called-from-an-event-handler"></span>

#### `ReadyToTest` 可以从事件处理器调用

现在可以注册一个事件处理器使用 `ReadyToTest` 。这在允许测试运行之前,可以用于进行下载资产等工作。

见 <https://github.com/ros2/launch/pull/665> 以获取更多信息。

<span id="addition-of-anysubstitution-and-allsubstitution"></span>

#### 增 编 `AnySubstitution` 财务报告和财务报告 `AllSubstitution`

现在可以指定在任何输入参数属实时的替换(`AnySubstitution`),或当所有输入参数都是真实的(`AllSubstitution`).

见 <https://github.com/ros2/launch/pull/649> 更多细节。

<span id="addition-of-a-new-substitution-to-get-the-launch-logging-directory"></span>

#### 为获取发射记录目录而添加新的替代

现在可以使用一种叫做 `LaunchLogDir` 以获取当前日志目录以启动。

见 <https://github.com/ros2/launch/pull/652> 更多细节。

<span id="launch-ros"></span>

### `launch_ros`

<span id="add-a-lifecycletransition-action"></span>

#### 添加一个 `LifecycleTransition` 动作

现在可以通过新的系统向生命周期节点发送过渡信号。 `LifeCycleTransition` 行动。

见 <https://github.com/ros2/launch_ros/pull/317> 以获取更多信息。

<span id="add-a-setroslogdir-action"></span>

#### 添加一个 `SetROSLogDir` 动作

现在可以配置用于通过 `SetROSLogDir` 行动。

见 <https://github.com/ros2/launch_ros/pull/325> 以获取更多信息。

<span id="ability-to-specify-a-condition-to-a-composablenode"></span>

#### 是否有能力对一个条件作出具体规定 `ComposableNode`

现在可以规定一个必须满足的条件,以便: `ComposableNode` 将插入其容器。

见 <https://github.com/ros2/launch_ros/pull/311> 以获取更多信息。

<span id="launch-testing"></span>

### `launch_testing`

<span id="timeout-for-process-startup-is-now-configurable"></span>

#### 进程启动的超时已可配置

在释放之前, `ReadyToTest` 动作会等待 15 秒 启动进程。 如果进程耗时超过 15 秒, 则会失败 。 现在有一个新的装饰器 。 `ready_to_test_action_timeout` 允许用户配置等待进程启动的时间。

见 <https://github.com/ros2/launch/pull/625> 以获取更多信息。

<span id="rclcpp"></span>

### `rclcpp`

<span id="addition-of-a-new-paradigm-for-handling-node-and-lifecyclenode"></span>

#### 增加一个新的处理模式 `Node` 财务报告和财务报告 `LifecycleNode`

那个... `Node` 财务报告和财务报告 `LifecycleNode` 分类是相关的,因为两者提供了相同的一套基本方法(尽管 `LifecycleNode` 由于执行方面的各种考虑,它们并非来自共同的基础类别。

这给下游密码带来了一些麻烦 因为它想接受任何一种 `Node` 或一个 `LifecycleNode`一种解决办法是有两个方法签名,一个接受一种方法签名。 `Node` 以及一个接受 `LifecycleNode`。另一个建议的解决办法是采用一种方法,接受可从两个类别中访问的“节点接口”指针,例如。

``` C++
void do_thing(rclcpp::node_interfaces::NodeGraphInterface graph)
{
  fprintf(stderr, "Doing a thing\n");
}

void do_thing(rclcpp::Node::SharedPtr node)
{
  do_thing(node->get_node_graph_interface());
}

void do_thing(rclcpp::LifecycleNode::SharedPtr node)
{
  do_thing(node->get_node_graph_interface());
}
```

这样做是可行的,但当需要许多节点接口时,它会变得有些不灵巧。要让这一点变得更好一点,现在有了一个新的 `NodeInterfaces` 类,可以构造以包含接口,然后被其他代码使用。

有一些实例说明如何将它用于 <https://github.com/ros2/rclcpp/pull/2041>.

<span id="introduction-of-a-new-executor-type-the-events-executor"></span>

#### 引入新的执行器类型:事件执行器

那个... `EventsExecutor` 从 iRobot 合并为主 `rclcpp` 代码库 。 此替代执行器执行程序使用事件驱动的调用符, 从中间软件执行中调用调用符到调用符 。 `rclcpp` 层。除了推力模型外, `EventsExecutor` 还把计时器管理移到一个单独的线程中,这可以实现更准确的结果和较低的管理费用,尤其是对于许多计时器来说.

那个... `EventsExecutor` 拥有大量文件和实际使用,因此成为列入《公约》的有力候选人。 `rclcpp` 关于初步执行提案和业绩基准,见 <https://discourse.ros.org/t/ros2-middleware-change-proposal/15863>。关于设计方面的更多信息,请参见设计PR: <https://github.com/ros2/design/pull/305>.

因为API是相同的,尝试 `EventsExecutor` 与替换您的当前执行程序一样简单( 例如 ) `SingleThreadedExecutor`):

``` C++
#include <rclcpp/experimental/executors/events_executor/events_executor.hpp>
using rclcpp::experimental::executors::EventsExecutor;

EventsExecutor executor;
executor.add_node(node);
executor.spin();
```

**说明** 那个... `EventsExecutor` 财务报告和财务报告 `TimersManager` 目前处于 `experimental` 名称空间。虽然它已经用作独立的执行一段时间了。 <https://github.com/irobot-ros/events-executor>,它被决定使用 `experimental` 命名空间,用于至少一次发布,在发布中给修改 API 的空间以自由度。请小心,因为它不会受到非实验代码的相同的 API/ABI 保证。

<span id="rclpy"></span>

### `rclpy`

<span id="ability-to-wait-for-another-node-to-join-the-graph"></span>

#### 等待另一个节点加入图表的能力

现在可以等待另一个节点加入网络图,其代码如下:

``` Python
node.wait_for_node('/fully_qualified_node_name')
```

见 <https://github.com/ros2/rclpy/pull/930> 以获取更多信息。

<span id="implementation-of-asyncparameterclient"></span>

#### 执行 `AsyncParameterClient`

`rclpy` 现在有一个 `AsyncParameterClient` ,使其与 `rclcpp`。此类用于在一个远程节点上执行参数动作,而不阻碍调用节点。

见 <https://github.com/ros2/rclpy/pull/959> 以获得更多信息和实例。

<span id="subscription-callbacks-can-now-optionally-get-the-message-info"></span>

#### 订阅回调可以选择获取消息信息

现在可以注册一个订阅回调,其功能签名既包括消息,也包括消息信息,例如:

``` Python
def msg_info_cb(msg, msg_info):
    print('Message info:', msg_info)

node.create_subscription(msg_type=std_msgs.msg.String, topic='/chatter', qos_profile=10, callback=msg_info_cb)
```

消息信息结构包含各种信息,如消息的序列号,来源和收到的时间戳,以及出版商的GID等.

见 <https://github.com/ros2/rclpy/pull/922> 以获取更多信息。

<span id="optional-argument-that-hides-assertions-for-messages-class"></span>

#### 为信件类隐藏断言的可选参数

所有信件类现在都包含一个新的可选参数,允许从信件中隐藏每个字段类型的断言。默认情况下,断言会被隐藏起来,这可以在运行时间中提供性能改进。为了让断言能够用于开发/调试目的,您有两种选择:

1.  定义环境变量 `ROS_PYTHON_CHECK_FIELDS` 改为: `'1'` (这将影响您项目中的所有信息):

``` Python
import os
from std_msgs.msg import String

os.environ['ROS_PYTHON_CHECK_FIELDS'] = '1'
new_message=String()
```

2.  通过在构建器中明确定义新参数来选择单个信件的具体行为 :

``` Python
from std_msgs.msg import String

new_message=String(check_fields=True)
```

见 <https://github.com/ros2/rosidl_python/pull/194> 以获取更多信息。

<span id="ros2param"></span>

### `ros2param`

<span id="option-to-timeout-when-waiting-for-a-node-with-ros2-param"></span>

#### 等待节点时的超时选项 `ros2 param`

现在可以拥有各种 `ros2 param` 命令超时通过 `--timeout` 给命令。

见 <https://github.com/ros2/ros2cli/pull/802> 以获取更多信息。

<span id="deprecated-options-were-removed"></span>

#### 已删除折旧选项

`--output-dir` 财务报告和财务报告 `--print` 选项 `dump` 命令已被删除。

见 <https://github.com/ros2/ros2cli/pull/824> 以获取更多信息。

<span id="ros2topic"></span>

### `ros2topic`

<span id="now-as-keyword-for-builtin-interfaces-msg-time-and-auto-for-std-msgs-msg-header"></span>

#### `now` 作为关键词 `builtin_interfaces.msg.Time` 财务报告和财务报告 `auto` (单位:千美元) `std_msgs.msg.Header`

`ros2 topic pub` 现在允许设置 `builtin_interfaces.msg.Time` 消息,通过 `now` 关键词。 `std_msg.msg.Header` 消息在传递关键字时会自动生成 `auto`。这一行为与ROS 1 的行为相匹配 `rostopic` (<http://wiki.ros.org/ROS/YAMLCommandLine#Headers.2Ftimestamps>)

相关 PR : [ros2/ros2cli#749](https://github.com/ros2/ros2cli/pull/749)

<span id="ros2-topic-pub-can-be-configured-to-wait-a-maximum-amount-of-time"></span>

#### `ros2 topic pub` 能够配置以等待最大时间

命令 `ros2 topic pub -w 1` 在发布信件之前, 将至少等待该用户数。 此发布会添加到 a `--max-wait-time` 选项,这样,如果看不到订阅者,命令将只等待最多的时间,然后退出。

见 <https://github.com/ros2/ros2cli/pull/800> 以获取更多信息。

<span id="ros2-topic-echo-can-be-configured-to-wait-a-maximum-amount-of-time"></span>

#### `ros2 topic echo` 能够配置以等待最大时间

命令 `ros2 topic echo` 现在接受一个 `--timeout` 选项,它控制命令等待出版物发生的最大时间。

见 <https://github.com/ros2/ros2cli/pull/792> 以获取更多信息。

<span id="deprecated-option-was-removed"></span>

#### 删除已折旧的选项

`--lost-messages` 选项 `echo` 命令已删除。

见 <https://github.com/ros2/ros2cli/pull/824> 以获取更多信息。

<span id="changes-since-the-humble-release"></span>

## 自Humble发行以来的变化

<span id="change-to-the-default-console-logging-file-flushing-behavior"></span>

### 更改为默认控制台日志文件冲洗行为

这特别适用于默认 `spdlog` ROS 2 中的基于日志后端 `rcl_logging_spdlog`。每次使用“ 错误” 日志消息时, 日志文件冲洗都被修改为冲洗, 例如每个 `RCLCPP_ERROR()` 并定期每5秒钟拨打一次电话。

此前, `spdlog` 在使用时,除了创建用于日志到文件的汇外,没有配置其他任何文件。

我们测试了修改结果,但没有发现CPU的俯仰率很高,甚至在有慢盘(如sd卡)的机器上也是如此。然而,如果这种修改给您带来问题,您可以通过设置旧的动作来获得。 `RCL_LOGGING_SPDLOG_EXPERIMENTAL_OLD_FLUSHING_BEHAVIOR=1` 环境变量。

稍后,我们希望获得对完整配置文件的支持(见: <https://github.com/ros2/rcl_logging/issues/92>)),在如何进行伐木方面给予你更大的灵活性,但这是目前只有规划的工作.

> 因此, **这一环境变量应被视为实验性因素,可在今后不作折旧而予以清除。**,当添加配置文件支持时, `rcl_logging_spdlog` 记录后端。

请参看此拉动请求, 了解更详细的变化 : <https://github.com/ros2/rcl_logging/pull/95>

<span id="ament-cmake-auto"></span>

### `ament_cmake_auto`

<span id="include-dependencies-are-now-marked-as-system"></span>

#### 包含依赖关系现在标记为系统

使用时 `ament_auto_add_executable` 或 时 间 `ament_auto_add_library`,现在自动添加为 `SYSTEM`。这意味着不报告依赖关系头文件中的警告。

见 <https://github.com/ament/ament_cmake/pull/385> 更多细节。

<span id="ament-cmake-nose"></span>

### `ament_cmake_nose`

<span id="package-has-been-deprecated-and-removed"></span>

#### 软件包已贬值并删除

蟒蛇 `nose` 软件包早已贬值。由于目前释放到Humble 或 Rolling 的开源软件包目前都不依赖于它,因此这种软件包会贬值并移除周围的Ament包装。

见 <https://github.com/ament/ament_cmake/pull/415> 以获取更多信息。

<span id="ament-lint"></span>

### `ament_lint`

<span id="files-can-be-excluded-from-linter-checks"></span>

#### 文件可以从linter 检查中排除

某些文件现在可以通过设置 `AMENT_LINT_AUTO_FILE_EXCLUDE` 调用前的 CMake 变量 `ament_lint_auto_find_test_dependencies`.

见 <https://github.com/ament/ament_lint/pull/386> 以获取更多信息。

<span id="camera-info-manager"></span>

### `camera_info_manager`

<span id="lifecycle-node-support"></span>

#### 生命周期节点支持

`camera_info_manager` 现在在常规ROS 2节点之外支持生命周期节点.

见 <https://github.com/ros-perception/image_common/pull/190> 以获取更多信息。

<span id="id1"></span>

### `launch`

<span id="launchconfigurationequals-and-launchconfigurationnotequals-are-deprecated"></span>

#### `LaunchConfigurationEquals` 财务报告和财务报告 `LaunchConfigurationNotEquals` 已贬值

那个... `LaunchConfigurationEquals` 财务报告和财务报告 `LaunchConfigurationNotEquals` 条件被贬低,并将在今后发行时删除。 `Equals` 财务报告和财务报告 `NotEquals` 应代之以使用替代。

见 <https://github.com/ros2/launch/pull/649> 更多细节。

<span id="id2"></span>

### `launch_ros`

<span id="renamed-classes-which-used-ros-in-the-name-to-use-ros-in-line-with-pep8"></span>

#### 已重命名的类 `Ros` 在要使用的名称 `ROS` 与项目EP8一致

更改的类别:

- `launch_ros.actions.RosTimer` -\> `launch_ros.actions.ROSTimer`

- `launch_ros.actions.PushRosNamespace` -\> `launch.actions.PushROSNamespace`

旧班名尚存,但将贬.

见 <https://github.com/ros2/launch_ros/pull/326> 以获取更多信息。

<span id="launch-xml"></span>

### `launch_xml`

<span id="expose-emulate-tty-to-xml-frontend"></span>

#### 曝光 `emulate_tty` 到 XML 前端

几度释放都有可能使 `launch` Python 代码使用伪刻度来模拟 TTY( 并因此进行诸如打印颜色之类的工作) 。 这个功能现在可以通过通过 XML 前端 `emulate_tty` 参数到可执行命令。

见 <https://github.com/ros2/launch/pull/669> 以获取更多信息。

<span id="expose-sigterm-timeout-and-sigkill-timeout-to-xml-frontend"></span>

#### 曝光 `sigterm_timeout` 财务报告和财务报告 `sigkill_timeout` 到 XML 前端

几期发布可以配置 SIGTERM 和 SIGKILLL 信号的最大超时值 `launch` Python 代码。 该功能通过通过 递递 `sigterm_timeout` 或 时 间 `sigkill_timeout` 参数到可执行命令。

见 <https://github.com/ros2/launch/pull/667> 以获取更多信息。

<span id="launch-yaml"></span>

### `launch_yaml`

<span id="expose-emulate-tty-to-yaml-frontend"></span>

#### 曝光 `emulate_tty` 转到 YAML 前端

几度释放都有可能使 `launch` Python 代码使用伪刻度来模拟 TTY( 并因此进行诸如打印颜色之类的工作 ) 。 该功能现在可以通过传递到 YAML 前端 。 `emulate_tty` 参数到可执行命令。

见 <https://github.com/ros2/launch/pull/669> 以获取更多信息。

<span id="expose-sigterm-timeout-and-sigkill-timeout-to-yaml-frontend"></span>

#### 曝光 `sigterm_timeout` 财务报告和财务报告 `sigkill_timeout` 转到 YAML 前端

几期发布可以配置 SIGTERM 和 SIGKILLL 信号的最大超时值 `launch` Python 代码。 该功能现在可以通过通过 YAML 前端 `sigterm_timeout` 或 时 间 `sigkill_timeout` 参数到可执行命令。

见 <https://github.com/ros2/launch/pull/667> 以获取更多信息。

<span id="message-filters"></span>

### `message_filters`

<span id="new-approximate-time-policy"></span>

#### 新的大致时间政策

在更简单的近似时间策略中添加调用 `ApproximateEpsilonTime`。这一次政策起作用了 `ExactTime`,但允许时间戳在 epsilon 容忍度之内。 <https://github.com/ros2/message_filters/pull/84> 以获取更多信息。

<span id="new-upsampling-time-policy"></span>

#### 新的抽样时间政策

在新时间策略中添加 `LatestTime`。它可以通过零位数进行检测,按其速率同步多达9条信息。见 <https://github.com/ros2/message_filters/pull/73> 以获取更多信息。

<span id="rcl-yaml-param-parser"></span>

### `rcl_yaml_param_parser`

<span id="support-for-yaml-str-syntax-in-parameter-files"></span>

#### 支持YAML `!!str` 参数文件中的语法

现在可以使用 YAML 强制 ROS 参数文件解析器将字段解释为字符串 `!!str` 语法。见 <https://github.com/ros2/rcl/pull/999> 以获取更多信息。

<span id="id3"></span>

### `rclcpp`

<span id="default-number-of-threads-for-multi-threaded-executor-has-been-changed"></span>

#### 多线程执行器的默认线索数已更改

如果用户不另外指定,多线程执行器的默认线程数将设定为机器上的CPU数。如果基础操作系统不支持获取此信息,则设定为2。

见 <https://github.com/ros2/rclcpp/pull/2032> 以获取更多信息。

<span id="a-warning-is-now-printed-when-qos-of-keep-last-is-specified-with-a-depth-of-0"></span>

#### 当 KEEP\_ LAST 的 QoS 指定深度为 0 时, 将打印警告

指定深度为 0 的 KEEP_LAST QOS 是一种非感知性安排, 因为实体将无法发送或接收任何数据 。 `rclcpp` 如果指定了此组合, 将会现在打印一个警告, 但会继续, 让内置的中间软件选择一个正常值( 一般是深度为 1) 。

见 <https://github.com/ros2/rclcpp/pull/2048> 以获取更多信息。

<span id="deprecated-rclcpp-scope-exit-macro-was-removed"></span>

#### 已折旧 `RCLCPP_SCOPE_EXIT` 宏已删除

在Humble,这个宏 `RCLCPP_SCOPE_EXIT` 已贬值,赞成 `RCPPUTILS_SCOPE_EXIT`。在铁, `RCLCPP_SCOPE_EXIT` 宏已完全删除。

<span id="id4"></span>

### `rclpy`

<span id="id5"></span>

#### 多线程执行器的默认线索数已更改

如果用户不另外指定,多线程执行器的默认线程数将设定为机器上的CPU数。如果基础操作系统不支持获取此信息,则设定为2。

见 <https://github.com/ros2/rclpy/pull/1031> 以获取更多信息。

<span id="id6"></span>

#### 当 KEEP\_ LAST 的 QoS 指定深度为 0 时, 将打印警告

指定深度为 0 的 KEEP_LAST QOS 是一种非感知性安排, 因为实体将无法发送或接收任何数据 。 `rclpy` 如果指定了此组合, 将会现在打印一个警告, 但会继续, 让内置的中间软件选择一个正常值( 一般是深度为 1) 。

见 <https://github.com/ros2/rclpy/pull/1048> 以获取更多信息。

<span id="time-and-duration-no-longer-raise-exception-when-compared-to-another-type"></span>

#### 时间和期限不再比其他类型引起例外

现在可以比较了 `rclpy.time.Time` 财务报告和财务报告 `rclpy.duration.Duration` 如果类型不具有可比性,则比较返回 `False`。请注意,这是与以前的发布相比,行为上的变化。

``` Python
print(None in [rclpy.time.Time(), rclpy.duration.Duration()])  # Prints "False" instead of raising TypeError
```

见 <https://github.com/ros2/rclpy/pull/1007> 以获取更多信息。

<span id="rcutils"></span>

### `rcutils`

<span id="improve-the-performance-of-message-logging"></span>

#### 提高信件记录的性能

当输出日志消息时所用的代码 `RCUTILS_LOG_*` 或 时 间 `RCLCPP_*` 这些日志信息现在应该更有效率,尽管不应该高调调。 <https://github.com/ros2/rcutils/pull/381>, <https://github.com/ros2/rcutils/pull/372>, <https://github.com/ros2/rcutils/pull/369>,以及 <https://github.com/ros2/rcutils/pull/367> 以获取更多信息。

<span id="deprecated-rcutils-get-env-h-header-was-removed"></span>

#### 已折旧 `rcutils/get_env.h` 页眉已删除

在Humble, 标题 `rcutils/get_env.h` 已贬值,赞成 `rcutils/env.h`。在铁, `rcutils/get_env.h` 标题被完全删除 。

<span id="rmw"></span>

### `rmw`

<span id="change-the-gid-storage-to-16-bytes"></span>

#### 将 GID 存储改为 16 字节

RAMW 层中的 GID 意在成为 ROS 图中全球独有的作者标识符。 此前, 它被错误地设定为基于旧的RAMW 执行中的一个错误的 24 字节。 但是 `rmw` 软件包应该对此进行定义,所有执行都应符合这一定义。因此,该版本将其定义为16字节(DDS标准),并修改所有执行以使用该定义。

见 <https://github.com/ros2/rmw/pull/345> 和(非公开,但相关) <https://github.com/ros2/rmw/pull/328> 以获取更多信息。

<span id="rmw-dds-common"></span>

### `rmw_dds_common`

<span id="id7"></span>

#### 将 GID 存储改为 16 字节

随同改变 `rmw` 层,将发送 GID 信息的消息更改为 16 字节。

见 <https://github.com/ros2/rmw_dds_common/pull/68> 以获取更多信息。

<span id="id8"></span>

### `ros2topic`

<span id="ros2-topic-hz-bw-pub-now-respect-use-sim-time"></span>

#### `ros2 topic hz/bw/pub` 现在尊重 `use_sim_time`

当模拟运行时, ROS 2 生态系统一般从 `/clock` 模拟器公布的话题(而不是使用系统时钟). ROS 2节点一般通过设置来得知这一变化. `use_sim_time` 参数。节点创建的节点 `ros2 topic` 命令 `hz`, `bw`,以及 `pub` 现在尊重该参数,并将酌情使用模拟时间。

见 <https://github.com/ros2/ros2cli/pull/754> 以获取更多信息。

<span id="rosbag2"></span>

### `rosbag2`

<span id="change-default-bag-file-type-to-mcap"></span>

#### 更改默认袋文件类型为 `mcap`

在这次发布之前,Rosbag2默认会将数据记录到sqlite3数据库中。在测试中发现,在许多情况下,这种记录不够有效,缺乏离线处理所需的某些特性。

为了满足这些需要,一个新的袋格式(受ROS 1 袋文件原始格式的影响)称为 `mcap` 已开发。此包文件格式有许多 sqlite3 文件格式中缺失的特性, 并且应该更能执行 。

此释放切换为使用 `mcap` 作为写入新包的默认文件格式。旧的 `sqlite3` 文件格式仍然可用,如果需要,用户可以选择用于写入。此发布还允许播放来自其中任一部分的数据。 `sqlite3` 文件格式或该格式 `mcap` 文件格式。

见 <https://github.com/ros2/rosbag2/pull/1160> 以获取更多信息。

<span id="store-message-definitions-in-bag-files-with-sqlite3-plugin"></span>

#### 用 SQLite3 插件存储信件定义到包文件

现在,我们支持将信息定义保存到 `sqlite3` 数据库文件,格式与我们正在保存的格式相同。 `mcap` 文件。这为第三方工具提供了一个机会,使其能够在没有正确版本的机器上所有原始的 .msg 文件的情况下解码记录的 rospage2 文件 `sqlite3` 插件 。

见 <https://github.com/ros2/rosbag2/issues/782> 财务报告和财务报告 <https://github.com/ros2/rosbag2/pull/1293> 以获取更多信息。

<span id="new-playback-and-recording-controls"></span>

#### 新建播放和录制控制

添加了若干拉动请求,以加强用户对弹出袋的控制。拉动请求 [960](https://github.com/ros2/rosbag2/pull/960) 添加播放 bag 指定秒数的能力。 并拉请求 [1005](https://github.com/ros2/rosbag2/pull/1005) 允许播放包直到指定的时间戳。另一个拉动请求 [1007](https://github.com/ros2/rosbag2/pull/1007) 添加通过服务调用远程停止播放的能力。 如果播放器处于暂停状态, 停止播放并强制退出播放方式 。

<span id="managing-recording-via-service-calls"></span>

#### 通过服务电话管理录音

从远程节点控制录制过程有新的选项。 拖曳请求 [1131](https://github.com/ros2/rosbag2/pull/1131) 通过服务电话添加暂停和恢复录音的能力。 另一份拉动请求 [1115](https://github.com/ros2/rosbag2/pull/1115) 通过发送服务呼叫,在录制过程中增加拆分袋的能力.

<span id="filtering-topics-via-regular-expression-during-playback"></span>

#### 播放时通过正则表达式过滤话题

用户有时只需要从录制的袋中重新播放一个子集的主题,接下来的两个拉请求会添加这种能力. Pull request [1034](https://github.com/ros2/rosbag2/pull/1034) 添加新选项 `--topics-regex` 通过正则表达式过滤话题。 `--topics-regex` 选项接受由空格分隔的多个正则表达式。并拖动请求 [1046](https://github.com/ros2/rosbag2/pull/1046) 通过在新版本中提供正则表达式,增加将某些主题排除在重播之外的能力。 `--exclude` (并) `-x`)选项。

<span id="allow-plugins-to-register-their-own-cli-verb-arguments"></span>

#### 允许插件注册自己的 CLI 动词参数

调用请求 [1209](https://github.com/ros2/rosbag2/pull/1209) 添加以下功能: `rosbag2` 用于注册可选 Python 条目的插件, 提供插件特定的 CLI 参数值。 因此, 命令行选项 `--storage-preset-profile` (单位:千美元) `ros2 bag record` 动词会根据基础存储插件而有不同的有效选项.

<span id="other-changes"></span>

#### 其他变动

牵引请求 [1038](https://github.com/ros2/rosbag2/pull/1038) 添加在元数据. yaml 文件的“ custom” 字段中记录任意密钥/ 值对的能力。 当用户需要保存某些硬件特定 id 或记录所在坐标时, 这样做很有用。 并拉动请求 [1180](https://github.com/ros2/rosbag2/pull/1180) 通过提供新命令行来更改记录器的基本节点名称的选项 `--node-name` 选项。此选项可用于创建远程分布式记录,其中包含多个Rosbag2记录器实例。它提供了向专用的Rosbag2记录器实例发送管理记录过程的服务呼叫的能力。

<span id="rosidl-python"></span>

### `rosidl_python`

<span id="modification-of-content-of-slots-attribute"></span>

#### 内容的修改 `__slots__` 属性

目前为止,这个属性 `__slots__` 从 python 信件类中, 已经被用作包含信件的字段名称的成员。 在 Iron 中, 此属性不再只包含信件结构中的字段名称, 而包含所有类成员的字段名称。 因此, 用户不应该依赖此属性来检索字段名称信息, 相反, 用户应该使用该方法检索它 。 `get_field_and_field_types()`.

见 <https://github.com/ros2/rosidl_python/pull/194> 以获取更多信息。

<span id="rviz"></span>

### `rviz`

<span id="map-display-can-now-be-shown-as-binary"></span>

#### 地图显示现在可以显示为二进制

RViz 映射图现在可以显示为二进制, 带有固定阈值。 在某些情况下, 这对于检查地图或与具有固定阈值的规划者结合使用是有用的 。

见 <https://github.com/ros2/rviz/pull/846> 以获取更多信息。

<span id="camera-display-plugin-respects-the-roi-in-the-camerainfo-message"></span>

#### 相机显示插件尊重相机Info消息中的 ROI

相机Display插件现在尊重相机Info消息中的利益区域设置。 这说明一个图像被相机驱动程序裁剪以减少带宽。

见 <https://github.com/ros2/rviz/pull/864> 以获取更多信息。

<span id="binary-stl-files-from-solidworks-work-without-error"></span>

#### 来自 SOLIDWORKS 的二进制 STL 文件没有出错

STL 加载器被修改为它接受来自 SOLIDWORKS 的二进制 STL 文件, 这些文件有“ 固” 一词。 这在技术上违反了STL 的规格, 但很常见的是, 增加了一个特殊的案例来处理这些文件 。

见 <https://github.com/ros2/rviz/pull/917> 以获取更多信息。

<span id="tracetools"></span>

### `tracetools`

<span id="tracing-instrumentation-is-now-included-by-default-on-linux"></span>

#### Linux 上默认包含追踪仪器

ROS 2 核心已经存在一段时间了。 但是, 它默认是编译出来的。 为了获得仪表, LTTng 跟踪器必须在从源头重建 ROS 2 之前手动安装。 在 Iron 中, 追踪仪表和跟踪点默认包含在内; 因此, LTTng 跟踪器现在是 ROS 2 的依赖性 。

注意这仅适用于Linux.

见 <https://github.com/ros2/ros2_tracing/pull/31> 财务报告和财务报告 <https://github.com/ros2/ros2/issues/1177> 更多信息。请参看 [此如何导引去除仪表( 或加入 Humble 和 older 的仪表) 。](../How-To-Guides/Building-ROS-2-with-Tracing-Instrumentation.md).

<span id="new-tracepoints-for-rclcpp-intra-process-are-added"></span>

#### 新建跟踪点 `rclcpp` 添加进程内

添加了新的跟踪点支持 `rclcpp` 进程内部通信。 这样可以评估消息发布和调回在进程内部通信中开始的时间。

见 <https://github.com/ros2/ros2_tracing/pull/30> 财务报告和财务报告 <https://github.com/ros2/rclcpp/pull/2091> 以获取更多信息。

<span id="known-issues"></span>

## 已知问题

- `rmw_connextdds` 不使用 Windows 二进制发布包。 RTI 已不长时间分发 `RTI ConnextDDS 6.0.1` 用于创建 Windows 的二进制。相反,它们现在正在分发 `RTI ConnextDDS 6.1.0` 的 ABI 与生成的二进制不兼容 。 解决方案是依赖 ROS 2 和 ROS 2 的源建构 `rmw_connextdds` 在视窗上。

- `sros2` 在 Windows 上,用户需要降级 `cryptography` Python 模块改为 `cryptography==38.0.4` 已讨论 [这儿](https://github.com/ros2/sros2/issues/285).

- `ros1_bridge` 不使用来自 NOEtic 包 [上游乌邦图](https://packages.ubuntu.com/jammy/ros-core-dev)建议的变通办法是从源头建造ROS Noetic,然后建造 `ros1_bridge` 用这个。

<span id="release-timeline"></span>

## 发布时间线

> 2022年11月 - 平台决定.  
> 《2000年区域环境方案》与目标平台和主要依赖性版本一起更新。
>
> 至2023年1月 - 滚动平台换乘.  
> Build farm与Iron Irwini的新平台版本和依赖性版本(如有必要)一起更新.
>
> Mon. 2023年4月10日 - Alpha + RMW 冻结  
> ROS基地的初步测试和稳定 <span id="id19"></span>[\[1\]](#id24) 软件包,以及RAMW供应商软件包的 API 和特性冻结。
>
> 2023年4月17日 - 冻结  
> ROS Base 的 API 和特性冻结 <span id="id20"></span>[\[1\]](#id24) 在 Rolling Ridley 中的软件包。在此点之后,只应该发布错误修正。新软件包可以独立发布。
>
> 2023年4月24日 - 分会  
> 罗林瑞德利的分店 `rosdistro` 重新开放用于 ROS 基地的 滚动PRs 。 <span id="id21"></span>[\[1\]](#id24) 软件包。 `ros-rolling-*` 软件包到 `ros-iron-*` 软件包。
>
> 2023年5月1日 - 贝塔  
> ROS 桌面更新版 <span id="id22"></span>[\[2\]](#id25) 可用软件包。请进行一般测试。
>
> 2023年5月15日 - 释放候选人.  
> 创建了候选软件包。 ROS 桌面的更新版 <span id="id23"></span>[\[2\]](#id25) 可用软件包。
>
> Thu. 2023年5月18日 - 冻结地段  
> 冻结Rodistro 没有铁的公关 `rosdistro` Repo将合并(发布公告后重新开放).
>
> Tue. 2023年5月23日 - 一般可用性  
> 发布公告. `rosdistro` 重新打开铁公关。

<span id="id24"></span>

\[1\] ([1](#id19),[2](#id20),[3](#id21))

那个... `ros_base` 变体描述于 [REP 2001(跨基)](https://reps.openrobotics.org/rep-2001/#ros-base).

<span id="id25"></span>

\[2\] ([1](#id22),[2](#id23))

那个... `desktop` 变体描述于 [REP 2001(桌面变量)](https://reps.openrobotics.org/rep-2001/#desktop-variants).

<span id="development-progress"></span>

## 发展进度

关于铁Irwini的开发和发布进展情况,参见: [跟踪 GitHub 问题](https://github.com/ros2/ros2/issues/1298).

关于Iron Irwini所遵循的广义过程,参见: [进程描述页面](Release-Process.md).
