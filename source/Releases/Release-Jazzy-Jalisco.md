---
translation_status: machine_translated
source: Releases/Release-Jazzy-Jalisco.rst
---

<span id="jazzy-jalisco-jazzy"></span> <span id="jazzy-release"></span>

# Jazzy Jalisco（`jazzy`)

*Jazzy Jalisco* 以下是自上次发布以来Jazzy Jalisco公司的重要变化和特征。 [长窗体变化日志](Jazzy-Jalisco-Complete-Changelog.md)

<span id="supported-platforms"></span>

## 支持的平台

Jazzy 哈利斯科支持以下平台: [平台支持级别](../The-ROS2-Project/Platform-Support-Tiers.md):

第一级平台:

- 乌班图24.04(诺布尔语: `amd64` 财务报告和财务报告 `arm64`

- Windows 10 (Visual Studio 2019): (英语). `amd64`

第二级平台:

- 莱尔9: `amd64`

第三级平台:

- 马科斯: `amd64`

- 底栖书虫 : `amd64`

目标平台:

| 建筑 | 乌本图·诺贝尔(24.04) | Windows 10 (VS2019) (英语). | 第9条 | 乌班图·贾米(22.04) | macOS | 德比亚书虫(12) | OpenEmbed / Yocto 项目 |
|----|----|----|----|----|----|----|----|
| amd64 (中文(简体) ). | 第1级 \[d\]\[a\]\[s\] | 第1级 \[s\] | 第二级\[d\]\[a\]\[s\] | 第3级 \[s\] | 第3级 \[s\] | 第3级 \[s\] | 第3级 \[s\] |
| 军火64 | 第1级 \[d\]\[a\]\[s\] |  |  |  |  | 第3级 \[s\] | 第3级 \[s\] |
| 臂弹32 | 第3级 \[s\] |  |  |  |  | 第3级 \[s\] | 第3级 \[s\] |

以下指标显示每个平台都有何种交付机制。

"\[d\]" 发行专用(Debian,RPM等)包将提供给本平台,用于提交rosdistro的包.

" \[a\] " 二进制版本作为每个平台的单一档案提供,其中包含Jazzy ROS 2 repos文件中的所有包\[^13\].

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
<th colspan="5" class="head"><p>建议的支助</p></th>
</tr>
<tr class="row-even">
<th class="head"><p>软件包</p></th>
<th class="head"><p>乌邦图诺布尔</p></th>
<th class="head"><p>视窗10**</p></th>
<th class="head"><p>第9条</p></th>
<th class="head"><p>乌邦图查米</p></th>
<th class="head"><p>马科斯**</p></th>
<th class="head"><p>底栖书虫</p></th>
<th class="head"><p>已打开*</p></th>
</tr>
</thead>
<tbody>
<tr class="row-odd">
<td><p>CMake</p></td>
<td><p>3.28.3</p></td>
<td><p>3.22.0</p></td>
<td><p>3.20.2</p></td>
<td><p>3.22.1</p></td>
<td><p>3.20.0</p></td>
<td><p>3.25.1</p></td>
<td><p>3.22.3</p></td>
</tr>
<tr class="row-even">
<td><p>爱咪</p></td>
<td><p>3.3.4</p></td>
<td><p>3.3.2</p></td>
<td colspan="5"><p>3.3.4</p></td>
</tr>
<tr class="row-odd">
<td><p>Gazebo</p></td>
<td><p>谐波器*</p></td>
<td><p>N/A</p></td>
<td><p>N/A</p></td>
<td><p>谐波器*</p></td>
<td><p>谐波器*</p></td>
<td><p>谐波器*</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>数字</p></td>
<td><p>1.26.4</p></td>
<td><p>1.18.4</p></td>
<td><p>1.20.1</p></td>
<td><p>1.21.5</p></td>
<td><p>1.18.4</p></td>
<td><p>1.24.2</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-odd">
<td><p>食人鱼</p></td>
<td colspan="6"><p>1.12.10</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>打开CV</p></td>
<td><p>4.6.0</p></td>
<td><p>3.4.6*</p></td>
<td><p>4.6.0</p></td>
<td><p>4.5.4</p></td>
<td><p>4.2.0</p></td>
<td><p>4.6.0</p></td>
<td><p>4.1.0 / 3.2.0***</p></td>
</tr>
<tr class="row-odd">
<td><p>打开SSL</p></td>
<td><p>3.0.13</p></td>
<td><p>1.1.1l</p></td>
<td><p>3.0.7</p></td>
<td><p>1.1.1l</p></td>
<td><p>1.1.1f</p></td>
<td><p>3.0.11</p></td>
<td><p>1.1.1d / 1.1.1b***</p></td>
</tr>
<tr class="row-even">
<td><p>Python</p></td>
<td><p>3.12.3</p></td>
<td><p>3.8.3</p></td>
<td><p>3.9.16</p></td>
<td><p>3.10.4</p></td>
<td><p>3.10.8</p></td>
<td><p>3.11.2</p></td>
<td><p>3.8.2 / 3.7.5***</p></td>
</tr>
<tr class="row-odd">
<td><p>Qt 键</p></td>
<td><p>5.15.10</p></td>
<td><p>5.12.12</p></td>
<td><p>5.15.3</p></td>
<td><p>5.15.3</p></td>
<td><p>5.12.3</p></td>
<td><p>5.15.8</p></td>
<td><p>5.14.1 / 5.12.5***</p></td>
</tr>
<tr class="row-even">
<td colspan="2"></td>
<td colspan="6"><p><strong>仅限 Linux</strong></p></td>
</tr>
<tr class="row-odd">
<td><p>个人计算机L</p></td>
<td><p>1.14.0</p></td>
<td><p>N/A</p></td>
<td><p>1.12.0</p></td>
<td><p>1.12.1</p></td>
<td><p>N/A</p></td>
<td><p>1.13.0</p></td>
<td><p>1.10.0</p></td>
</tr>
<tr class="row-even">
<td colspan="8"><p><strong>RMW DDS 中间软件</strong></p></td>
</tr>
<tr class="row-odd">
<td><p>Cyclone DDS</p></td>
<td colspan="7"><p>0.10.4</p></td>
</tr>
<tr class="row-even">
<td><p>快速数据交换系统</p></td>
<td colspan="7"><p>2.14.0</p></td>
</tr>
<tr class="row-odd">
<td><p>Connext DDS</p></td>
<td colspan="5"><p>6.0.1</p></td>
<td colspan="2"><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>Gurum 数据交换系统</p></td>
<td colspan="2"><p>4.2.0</p></td>
<td colspan="5"><p>N/A</p></td>
</tr>
</tbody>
</table>

" \* " 是指这不是上游版本(可在官方操作系统寄存器上查阅),而是OSRF或社区分发的包(自定义寄存器上建立和分发的包)。

" \*\*"是指依赖可能看到多个版本的改变,因为依赖使用一个包管理器,在没有稳定的API的情况下不断更新依赖.

" \*\*\* " WebOS OSE提供了这个不同的版本.

此文档只抓取 ROS 发行的首次发布时的版本, 并且不会随着依赖关系向前移动而更新, 因此这些版本是低水印 。

依赖软件包管理器的使用 :

- Ubuntu, Debian: apt, pip) 维基语录链接:名人名言 - 文学作品 - 谚语 - 谚语 - 谚语

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

[安装 Jazzy 哈利斯科](https://docs.ros.org/en/jazzy/Installation.html)

<span id="changes-to-how-ros-2-and-gazebo-integrate"></span>

## ROS 2 和 Gazebo 整合方式的改变

从Jazzy Jalisco开始 我们在简化ROS 2和 [Gazebo](https://gazebosim.org) 对于每个ROS 2 版本,都会有一个推荐的,支持的 Gazebo 版本与该版本同时进行。对于 Jazzy 哈利斯科来说,推荐的 Gazebo 版本将是和谐的。

为了让ROS 2 套件更容易消费 Gazebo 套件,现在有了 `gz_*_vendor` 软件包。这些软件包是:

- gz_common_vendor: <https://github.com/gazebo-release/gz_common_vendor>

- gz_cmake_vendor: <https://github.com/gazebo-release/gz_cmake_vendor>

- gz_math_vendor: <https://github.com/gazebo-release/gz_math_vendor>

- gz_transport_vendor: <https://github.com/gazebo-release/gz_transport_vendor>

- gz_sensor_vendor: <https://github.com/gazebo-release/gz_sensor_vendor>

- gz_sim_vendor: <https://github.com/gazebo-release/gz_sim_vendor>

- gz_tools_vendor: <https://github.com/gazebo-release/gz_tools_vendor>

- gz_utils_vendor: <https://github.com/gazebo-release/gz_utils_vendor>

- sdformat_vendor: <https://github.com/gazebo-release/sdformat_vendor>

ROS 2 软件包可以通过在其中添加依赖性来使用这些软件包中的功能 `package.xml`, e.g.:

``` default
<depend>gz_math_vendor</depend>
```

然后用它们进去 `CMakeLists.txt`, e.g.:

``` default
find_package(gz_math_vendor REQUIRED)
find_package(gz-math)

add_executable(my_executable src/exe.cpp)
target_link_libraries(my_executable gz-math::core)
```

> **说明**
>
> 与Jazzy Jalisco一起使用备选的 Gazebo 版本仍然是可能的。 但是,这些版本不会像ROS 2 那样得到很好的测试或集成 。 <https://gazebosim.org/docs/harmonic/ros_installation> 以获取更多信息。

<span id="new-features-in-this-ros-2-release"></span>

## 本 ROS 2 发布中的新功能

<span id="common-interfaces"></span>

### `common_interfaces`

<span id="new-velocitystamped-message"></span>

#### 新建速度标定的消息

添加了定义速度和转换速度所需的所有字段的新信息 。

见 <https://github.com/ros2/common_interfaces/pull/240> 更多细节。

<span id="adds-arrow-strip-to-marker-msg"></span>

#### 将 ARROW_STRIP 添加到 Marker.msg 中

添加了新类型的标记, `ARROW_STRIP`给Marker.msg。

见 <https://github.com/ros2/common_interfaces/pull/242> 更多细节。

<span id="image-transport"></span>

### `image_transport`

<span id="support-lazy-subscribers"></span>

#### 支持懒惰的订户

见 <https://github.com/ros-perception/image_common/issues/272> 更多细节。

<span id="expose-option-to-set-callback-groups"></span>

#### 曝光设置回调组的选项

见 <https://github.com/ros-perception/image_common/issues/274> 更多细节。

<span id="enable-allow-list"></span>

#### 启用允许列表

添加参数使用户可以有选择地禁用 `image_transport` 运行时的插件 。

见 <https://github.com/ros-perception/image_common/issues/264> 更多细节。

<span id="advertise-and-subscribe-with-custom-qos"></span>

#### 用自定义 QoS 进行广告和订阅

允许用户在创建时在自定义的服务质量中通过 `image_transport` 出版社和订户。

见 <https://github.com/ros-perception/image_common/issues/288> 更多脱衣舞者。

<span id="added-rclcpp-component-to-republish"></span>

#### 添加 Rclcpp 组件到 Republish

用户现在可以启动 `image_transport` 重排一个 rclpp\_ 组件 。

见 <https://github.com/ros-perception/image_common/issues/275> 更多细节。

<span id="message-filters"></span>

### `message_filters`

<span id="typeadapters-support"></span>

#### 类型绘图器支持

允许用户在消息_过滤器内使用类型适应.

见 <https://github.com/ros2/message_filters/pull/96> 以获取更多信息。

<span id="rcl"></span>

### `rcl`

<span id="add-get-type-description-service"></span>

#### 添加获取类型描述服务

执行 `~/get_type_description` 服务允许外部用户获得对节点提供的每一类型描述。此服务由每个节点根据 [REP 2016 (英语).](https://github.com/ros-infrastructure/rep/pull/381).

见 <https://github.com/ros2/rcl/pull/1052> 更多细节。

<span id="rclcpp"></span>

### `rclcpp`

<span id="type-support-helper-for-services"></span>

#### 服务类型的支持助手

服务的新类型的支持助手 `rclcpp::get_service_typesupport_handle` 用于提取服务类型的支持手柄。

见 <https://github.com/ros2/rclcpp/pull/2209> 更多细节。

<span id="rclpy"></span>

### `rclpy`

<span id="parametereventhandler"></span>

#### 参数EventHandler

新类 `ParameterEventHandler` 允许我们通过参数事件来监测和响应参数的变化.

见 <https://github.com/ros2/rclpy/pull/1135> 更多细节。

<span id="ros2cli"></span>

### `ros2cli`

<span id="added-a-log-file-name-command-line-argument"></span>

#### 添加a `--log-file-name` 命令行参数

现在可以使用 `--log-file-name` 命令行参数来指定日志文件名前缀。

``` console
$ ros2 run demo_nodes_cpp talker --ros-args --log-file-name filename
```

见 <https://github.com/ros2/ros2cli/issues/856> 以获取更多信息。

<span id="added-qos-to-subscription-options"></span>

#### 在订阅选项中添加QoS

用户设置的 QoS 参数已被添加到 `TopicStatisticsOptions`,它允许统计与订阅本身有不同的QoS。

见 <https://github.com/ros2/rclcpp/pull/2323> 更多细节。

<span id="add-clients-and-services-count"></span>

#### 添加客户端和服务数

现在可以获得服务所创造的客户数量.

<span id="ros2action"></span>

### `ros2action`

<span id="type-sub-command-supported"></span>

#### `type` 支持子命令

现在可以使用 `type` 用于检查动作类型的子命令。

``` console
$ ros2 action type /fibonacci
action_tutorials_interfaces/action/Fibonacci
```

见 <https://github.com/ros2/ros2cli/pull/894> 以获取更多信息。

<span id="rosbag2"></span>

### `rosbag2`

<span id="service-recording-and-playback"></span>

#### 服务记录和回放

现在,可以将服务数据录入和播放。 `ros2bag` 命令行界面。

这个特征建立在 [服务回顾](https://github.com/ros2/ros2/issues/1285),自铁伊维尼时代开始就已经存在. [服务记录和显示](https://github.com/ros2/rosbag2/pull/1480) 添加将服务数据记录到袋文件的能力 。 [服务回放](https://github.com/ros2/rosbag2/pull/1481) 可以从包文件中播放服务数据。

记录所有服务数据 :

``` console
$ ros2 bag record --all-services
```

记录所有服务和所有主题数据 :

``` console
$ ros2 bag record --all
```

播放包文件中的服务数据 :

``` console
$ ros2 bag play --publish-service-requests bag_path
```

见 [设计文件](https://github.com/ros2/rosbag2/blob/rolling/docs/design/rosbag2_record_replay_service.md) 以获取更多信息。

<span id="new-filter-modes"></span>

#### 新建过滤模式

现在可以按主题类型过滤。

``` console
$ ros2 bag record --topic_types sensor_msgs/msg/Image sensor_msgs/msg/CameraInfo
```

``` console
$ ros2 bag record --topic_types sensor_msgs/msg/Image
```

详情请参见 <https://github.com/ros2/rosbag2/pull/1577> 财务报告和财务报告 <https://github.com/ros2/rosbag2/pull/1582>.

<span id="player-and-recorder-are-now-exposed-as-rclcpp-components"></span>

#### 玩家和录音机现在被曝光为 rclcpp 组件

这允许在数据记录或回复时使用进程内部通信时使用“零复制件 ” 。 在处理高波段宽度数据流时, 这可以大大减少记录或回复时的CPU 负载, 并且有助于避免传输层中的数据丢失。 它还提供了使用 YAML 配置文件的能力 。 `rosbag2_transport::Player` 财务报告和财务报告 `rosbag2_transport::Recorder` 可堆积节点.

见 <https://github.com/ros2/rosbag2/tree/jazzy?tab=readme-ov-file#using-with-composition> 更多细节。

<span id="added-option-to-disable-recorder-keyboard-controls"></span>

#### 添加选项以禁用记录器键盘控件

见 <https://github.com/ros2/rosbag2/pull/1607> 更多细节。

<span id="use-middleware-send-and-receive-timestamps-from-message-info-during-recording"></span>

#### 使用中间软件发送并接收时间戳 `message_info` 记录期间

如果有的话, `rosbag2` 现在使用中间软件提供的发送和接收时间戳。这些时间戳更能说明数据是何时实际发送和接收的。请注意,将时间戳保存到一个包中,目前只为MCAP文件(默认)提供支持。

见 <https://github.com/ros2/rosbag2/pull/1531> 更多细节。

<span id="added-compression-threads-priority-to-record-options"></span>

#### 添加压缩线程优先级以记录选项

现在可以指定执行压缩的线程的优先级.

见 <https://github.com/ros2/rosbag2/pull/1457> 更多细节。

<span id="added-ability-to-split-already-existing-ros2-bags-by-time"></span>

#### 增加分拆已存在的 ros2 袋的能力

已添加 `start_time_ns` 财务报告和财务报告 `end_time_ns` 页:1 `StorageOptions` 以排除未在 `[start_time;end_time]` 期间 `ros2 bag convert` 操作。

见 <https://github.com/ros2/rosbag2/pull/1455> 更多细节。

<span id="store-serialized-metadata-in-bag-files-directly"></span>

#### 将序列元数据直接存储在包文件中

`rosbag2` 总是存储元数据到 `metadata.yaml` 与 bag 文件相关的文件。现在, 元数据也存储在每个 bag 文件中, 打开文件时一次, 关闭书面的 bag 文件时第二次。 这样可以使 bag 文件自成一体, 并且不用 。 `metadata.yaml` 文件在 rosbag2 播放器或第三方应用程序中。 `ros2 bag reindex` 仍然可以用来恢复 `metadata.yaml` 文档,如果需要的话。

<span id="store-ros-distro-name-in-the-metadata"></span>

#### 在元数据中存储 ROS_DISTRO 名称

见 <https://github.com/ros2/rosbag2/pull/1241> 更多细节。

<span id="added-introspection-qos-methods-to-python-bindings"></span>

#### 在 Python 绑定中添加了反演 QoS 方法

现在可以从 Python 绑定物中对 QoS 设置进行回顾。

见 <https://github.com/ros2/rosbag2/pull/1648> 更多细节。

<span id="rosidl"></span>

### `rosidl`

<span id="added-interfaces-to-support-key-annotation"></span>

#### 为支持密钥注释而添加的接口

那个... `key` 注释可以表示一个数据成员是密钥的一部分,该密钥可以有零或更多密钥字段,可以应用于各种类型的结构字段.

见 <https://github.com/ros2/rosidl/pull/796> 财务报告和财务报告 <https://github.com/ros2/rosidl_typesupport_fastrtps/pull/116> 更多细节。

<span id="rviz2"></span>

### `rviz2`

<span id="added-regex-filter-field-for-tf-display"></span>

#### 为 TF 显示添加了 regex 过滤字段

当有许多框架 `/tf` 在 RViz 中很难正确显示它们,特别是在帧重叠的情况下。通常的解决方案是在 TF 显示的帧字段中启用和禁用想要的帧。现在可以使用正则表达式过滤帧。

见 <https://github.com/ros2/rviz/pull/1032> 更多细节。

<span id="append-measured-subscription-frequency-to-topic-status"></span>

#### 将测量的订阅频率添加到专题状态

在主题状态部件中可以直观化 Hz 。

见 <https://github.com/ros2/rviz/issues/1113> 更多细节。

<span id="reset-functionality"></span>

#### 重置功能

可以使用新服务或键盘快捷键重置时间 `R`.

见 <https://github.com/ros2/rviz/issues/1109> 财务报告和财务报告 <https://github.com/ros2/rviz/issues/1088> 更多细节。

<span id="added-support-for-point-cloud-transport"></span>

#### 为点\_ cloud\_ transport 添加了支持

可以使用 `point_cloud_transport` 软件包。

见 <https://github.com/ros2/rviz/pull/1008> 更多细节。

<span id="feature-parity-with-rviz-for-ros"></span>

#### 与 RViz 对 ROS 的特征对等

可以使用ROS 1版本中可用的相同插件.

- 深层

- 标记

- 粘贴图

- 手腕贴板

- 任务

<span id="camera-info-display"></span>

#### 相机信息显示

可以在3D场景中可视化相机Info消息.

见 <https://github.com/ros2/rviz/pull/1166> 更多细节。

<span id="rcpputils"></span>

### `rcpputils`

<span id="added-tl-expected"></span>

#### 已添加 tl_预期

[std: 预想](https://en.cppreference.com/w/cpp/utility/expected) 是 C++23 特性,尚未在 ROS 2. 中支持,但是可以使用 `tl::expected` 从rcpputils 通过一个背传执行。

见 <https://github.com/ros2/rcpputils/pull/185> 更多细节。

<span id="rcutils"></span>

### `rcutils`

<span id="add-human-readable-date-to-logging-formats"></span>

#### 在日志格式中添加人类可读日期

在使用控制台记录时,现在可以以人可读格式输出日期。 `{date_time_with_ms}` 符号在 `RCUTILS_CONSOLE_OUTPUT_FORMAT` 环境变量。

见 <https://github.com/ros2/rcutils/pull/441> 更多细节。

<span id="changes-since-the-iron-release"></span>

## 自铁发行以来的变化

<span id="id1"></span>

### `common_interfaces`

<span id="added-ids-to-geometry-msgs-polygon-and-polygonstamped"></span>

#### 添加到几何\_ msgs/ Polygon 和 Polygon 的ID 标注

多边形常用于代表特定对象,但目前很难在没有任何特定标识的情况下纠正。这个特性增加了一个ID字段来分解多边形。

见 <https://github.com/ros2/common_interfaces/pull/232> 更多细节。

<span id="geometry2"></span>

### `geometry2`

<span id="removed-deprecated-headers"></span>

#### 已删除的已贬值信头

在Humble,标题: `tf2_bullet/tf2_bullet.h`, `tf2_eigen/tf2_eigen.h`, `tf2_geometry_msgs/tf2_geometry_msgs.h`, `tf2_kdl/tf2_kdl.h`, `tf2_sensor_msgs/tf2_sensor_msgs.h` 被贬低,赞成: `tf2_bullet/tf2_bullet.hpp`, `tf2_eigen/tf2_eigen.hpp`, `tf2_geometry_msgs/tf2_geometry_msgs.hpp`, `tf2_kdl/tf2_kdl.hpp`, `tf2_sensor_msgs/tf2_sensor_msgs.hpp` 在爵士乐里, `tf2_bullet/tf2_bullet.h`, `tf2_eigen/tf2_eigen.h`, `tf2_geometry_msgs/tf2_geometry_msgs.h`, `tf2_kdl/tf2_kdl.h`, `tf2_sensor_msgs/tf2_sensor_msgs.h` 信头已被完全删除 。

<span id="changed-return-types-of-wait-for-transform-async-and-wait-for-transform-full-async"></span>

#### 更改的返回类型 `wait_for_transform_async` 财务报告和财务报告 `wait_for_transform_full_async`

此前 `wait_for_transform_async` 财务报告和财务报告 `wait_for_transform_full_async` 会 议 日 程 和 议 程 `Buffer` 班级返回了一个包含真实或虚假的未来 在Jazzy,未来将包含等待的变换信息.

<span id="enabled-twist-interpolator"></span>

#### 启用了 Twist 插件

包含新的 API 以查看参考框中移动帧的速度 。

见 <https://github.com/ros2/geometry2/pull/646> 以获取更多信息。

<span id="id2"></span>

### `rcl`

<span id="actual-and-expected-call-time-when-timer-is-called"></span>

#### 调用计时器的实际和预期通话时间

新计时器 API `rcl_timer_call_with_info` 添加以收集调时器的实际和预期的调时。这允许用户在预计调时器和实际调时器时获得计时器信息。

见 <https://github.com/ros2/rcl/pull/1113> 更多细节。

<span id="improved-rcl-wait-in-the-area-of-timeout-computation-and-spurious-wakeups"></span>

#### 在超时计算和假醒领域改进 rcl_等待

添加了对计时器的特殊处理, 并启用了时间超载。 对于这些计时器, 我们不应该计算超时, 因为等待器被相关守护条件唤醒 。

见 <https://github.com/ros2/rcl/issues/1146> 更多细节。

<span id="id3"></span>

### `rclcpp`

<span id="fixed-data-race-conditions"></span>

#### 固定数据种族条件

执行器中的固定数据种族条件.

见 <https://github.com/ros2/rclcpp/issues/2500> 更多细节。

<span id="utilize-rclcpp-waitset-as-part-of-the-executors"></span>

#### 使用 `rclcpp::WaitSet` 作为执行者的一部分

增加数量 `rcl_wait_set` 通过使默认的单行/多行脚执行器在实体收藏重建方面像静态的单行线执行器一样发挥作用来创建和删除。

见 <https://github.com/ros2/rclcpp/pull/2142> 更多细节。

由于这一改变,执行器中的召回命令不再一致,即使在同一个实体内也是如此.

见 <https://github.com/ros2/rclcpp/issues/2532> 更多细节。

<span id="rclcpp-get-typesupport-handle-is-deprecated"></span>

#### `rclcpp::get_typesupport_handle` 已贬值

那个... `rclcpp::get_typesupport_handle` 用于提取消息类型支持控件的操作符被贬值,并将在未来的发布中删除。 `rclcpp::get_message_typesupport_handle` 应使用。

见 <https://github.com/ros2/rclcpp/pull/2209> 更多细节。

<span id="deprecated-rclcpp-qos-event-hpp-header-was-removed"></span>

#### 已折旧 `rclcpp/qos_event.hpp` 页眉已删除

在铁,头 `rclcpp/qos_event.hpp` 已贬值,赞成 `rclcpp/event_handler.hpp`在Jazzy,该 `rclcpp/qos_event.hpp` 头已完全删除 。

<span id="deprecated-subscription-callback-signatures-were-removed"></span>

#### 已删除已过期的订阅回调签名

回到Humble,表单的订阅签名 `void callback(std::shared_ptr<MessageT>)` 财务报告和财务报告 `void callback(std::shared_ptr<MessageT>, const rclcpp::MessageInfo &)` 被贬值。

在Jazzy,这些订阅签名已被删除。 用户应该切换到使用 `void callback(std::shared_ptr<const MessageT>)` 或 时 间 `void callback(std::shared_ptr<const MessageT>, const rclcpp MessageInfo &)`.

<span id="id4"></span>

#### 调用计时器的实际和预期通话时间

`rclcpp::TimerInfo` 参数被添加到计时器召回中,以在调用计时器时收集实际和预期的调用时间。这允许用户在预计调用计时器时获得计时器信息,以及调用计时器的实际时间。

见 <https://github.com/ros2/rclcpp/pull/2343> 更多细节。

<span id="rclcpp-action"></span>

### `rclcpp_action`

<span id="callback-after-cancel"></span>

#### 取消后召回

添加了一个功能,以便在目标手柄超出范围后停止召回。 此功能允许我们在锁定上下文中放下手柄 。

见 <https://github.com/ros2/rclcpp/pull/2281> 更多细节。

<span id="rclcpp-lifecycle"></span>

### `rclcpp_lifecycle`

<span id="add-new-node-interface-typedescriptionsinterface"></span>

#### 添加新节点接口类型Descriptions Interface

添加新节点接口 `TypeDescriptionsInterface` 提供《京都议定书》 `GetTypeDescription` 服务。

见 <https://github.com/ros2/rclcpp/pull/2224> 更多细节。

<span id="id5"></span>

### `rclpy`

<span id="rclpy-node-node-declare-parameter"></span>

#### `rclpy.node.Node.declare_parameter`

那个... `rclpy.node.Node.declare_parameter` 不允许静态输入没有默认值的参数。

见 <https://github.com/ros2/rclpy/pull/1216> 更多细节。

<span id="added-types-to-method-arguments"></span>

#### 添加到方法参数中的类型

添加类型检查,以改善任何人使用静态类型检查的经验.

见 <https://github.com/ros2/rclcpp/pull/2224>, <https://github.com/ros2/rclpy/issues/1240>, <https://github.com/ros2/rclpy/issues/1237>, <https://github.com/ros2/rclpy/issues/1231>, <https://github.com/ros2/rclpy/issues/1241>,以及 <https://github.com/ros2/rclpy/issues/1233>.

<span id="id6"></span>

### `rosbag2`

<span id="rename-of-the-exclude-cli-option"></span>

#### 重命名为 `--exclude` CLI 选项

那个... `--exclude` CLI 选项已更名为 `--exclude-regex` 以更好地反映它的作用。

见 <https://github.com/ros2/rosbag2/pull/1480> 以获取更多信息。

<span id="changes-in-representation-of-the-offered-qos-profiles"></span>

#### 联合国代表席位的变动 `offered_qos_profiles`

现在使用的是 enum 值 `offered_qos_profiles` 在代码中,在元数据中用于QoS设置的人类可读字符串值中,以及在压倒性的QoS profile YAML文件中。

见 <https://github.com/ros2/rosbag2/tree/jazzy?tab=readme-ov-file#overriding-qos-profiles> 举个例子。

<span id="added-node-name-to-the-read-and-write-bag-split-event-messages"></span>

#### 将节点名称添加到读写包分割事件消息中

见 <https://github.com/ros2/rosbag2/pull/1609> 更多细节。

<span id="added-bagsplitinfo-service-call-on-bag-close"></span>

#### 已添加 `BagSplitInfo` 关闭邮包服务呼叫

见 <https://github.com/ros2/rosbag2/pull/1422> 更多细节。

<span id="resolved-multiple-issues-related-to-the-handling-sigint-and-sigterm-signals-in-rosbag2"></span>

#### 解决与处理Rosbag2中的SIGINT和SIGTERM信号有关的多个问题

见 <https://github.com/ros2/rosbag2/pull/1557>, <https://github.com/ros2/rosbag2/pull/1301> 财务报告和财务报告 <https://github.com/ros2/rosbag2/pull/1464> 更多细节。

<span id="added-topic-id-returned-by-storage-to-the-topicmetadata"></span>

#### 已添加 `topic_id` 通过存储返回到 `TopicMetadata`

见 <https://github.com/ros2/rosbag2/pull/1538> 更多细节。

<span id="added-python-bindings-for-compressionoptions-and-compressionmode-structures"></span>

#### 为压缩选项和压缩模块结构添加的 Python 绑定

见 <https://github.com/ros2/rosbag2/pull/1425> 更多细节。

<span id="improve-performance-in-sqlitestorage-get-bagfile-size"></span>

#### 提高业绩 `SqliteStorage::get_bagfile_size()`

这将在用 SQLite3 存储插件录制时, 包分割操作中丢失消息的概率最小化 。

见 <https://github.com/ros2/rosbag2/pull/1516> 更多细节。

<span id="rqt-bag"></span>

### `rqt_bag`

<span id="improved-performance-and-updated-rosbag-api"></span>

#### 改进性能和更新Rosbag API

Rosbag2 API 和 Ubuntu Noble 库版本中有一些破碎的更改,需要修改到 `rqt_bag`.

见 <https://github.com/ros-visualization/rqt_bag/pull/156> 更多细节。

<span id="development-progress"></span>

## 发展进度

关于Jazzy Jalisco公司的发展进展,参见: [这个项目的董事会](https://github.com/orgs/ros2/projects/52).

关于Jazzy Jalisco所遵循的广泛进程,见 [进程描述页面](Release-Process.md).

<span id="known-issues"></span>

## 已知问题

来

<span id="release-timeline"></span>

## 发布时间线

> 2023年11月 - 平台决定.  
> 《2000年区域环境方案》与目标平台和主要依赖性版本一起更新。
>
> 至2024年1月 - 滚动平台换乘  
> Build farm更新了Jazzy Jalisco的新平台版本和依赖性版本.
>
> Mon. 2024年4月8日 - Alpha + RMW 冻结  
> ROS基地的初步测试和稳定 <span id="id7"></span>[\[1\]](#id12) 软件包,以及RAMW供应商软件包的 API 和特性冻结。
>
> 2024年4月15日 - 冻结  
> ROS Base 的 API 和特性冻结 <span id="id8"></span>[\[1\]](#id12) 在 Rolling Ridley 中的软件包。在此点之后,只应该发布错误修正。新软件包可以独立发布。
>
> 2024年4月22日 - 分行  
> 罗林瑞德利的分店 `rosdistro` 重新开放用于 ROS 基地的 滚动PRs 。 <span id="id9"></span>[\[1\]](#id12) 软件包。 `ros-rolling-*` 软件包到 `ros-jazzy-*` 软件包。
>
> Mon. 2024年4月29日 - 贝塔  
> ROS 桌面更新版 <span id="id10"></span>[\[2\]](#id13) 可用软件包。请进行一般测试。
>
> 韦德,2024年5月1日 - 踢走教习党.  
> 教师: <https://github.com/osrf/ros2_test_cases> 开放给社区测试。
>
> 2024年5月13日 - 释放候选人  
> 创建了候选软件包。 ROS 桌面的更新版 <span id="id11"></span>[\[2\]](#id13) 可用软件包。
>
> 2024年5月20日 - 冻结地铁  
> 全部冻结Jazzy的树枝 [ROS 2 桌面软件包](https://reps.openrobotics.org/rep-2001/#jazzy-jalisco-may-2024-may-2029) 财务报告和财务报告 `rosdistro`。没有拉动请求。 `jazzy` 分支或目标 `jazzy/distribution.yaml` 输入 `rosdistro` Repo将合并.
>
> Thu. 2024年5月23日 - 一般可用性  
> 发布公告. [ROS 2 桌面软件包](https://reps.openrobotics.org/rep-2001/#jazzy-jalisco-may-2024-may-2029) 源冻结解除, `rosdistro` 为 Jazzy 拖拉请求重新打开 。

<span id="id12"></span>

\[1\] ([1](#id7),[2](#id8),[3](#id9))

那个... `ros_base` 变体描述于 [REP 2001(跨基)](https://reps.openrobotics.org/rep-2001/#ros-base).

<span id="id13"></span>

\[2\] ([1](#id10),[2](#id11))

那个... `desktop` 变体描述于 [REP 2001(桌面变量)](https://reps.openrobotics.org/rep-2001/#desktop-variants).
