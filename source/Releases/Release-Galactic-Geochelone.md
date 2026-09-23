---
translation_status: machine_translated
source: Releases/Release-Galactic-Geochelone.rst
---

<span id="galactic-geochelone-galactic"></span> <span id="galactic-release"></span>

# Galactic Geochelone（`galactic`)

*银河系地心酮* 以下是自上次发布以来银河系地理切龙的重要变化和特征的亮点。关于自Foxy以来的所有变化,请参见 [长窗体变化日志](Galactic-Geochelone-Complete-Changelog.md).

<span id="supported-platforms"></span>

## 支持的平台

Galactic Geocherone 支持以下平台: [平台支持级别](../The-ROS2-Project/Platform-Support-Tiers.md):

第一级平台:

- 乌本图20.04(福尔): `amd64` 财务报告和财务报告 `arm64`

- Windows 10 (Visual Studio 2019): (英语). `amd64`

第二级平台:

- 莱尔8: `amd64`

第三级平台:

- 乌本图20.04(福尔): `arm32`

- 德比安公牛(11): `amd64`, `arm64` 财务报告和财务报告 `arm32`

- OpenEmbed Thud (2.6) / webOS OSE : (中文(简体) ). `arm32` 财务报告和财务报告 `arm64`

- Mac macOS 10.14 (移动): `amd64`

目标平台:

| 建筑 | 乌邦图联络人(20.04) | Windows 10 (VS2019) (英语). | 第8条 | macOS | 德比安公牛(11) | OpenEmbed / webOS OSE 操作系统 |
|----|----|----|----|----|----|----|
| amd64 (中文(简体) ). | 第1级 \[d\]\[a\]\[s\] | 第1级 \[s\] | 第二级\[d\]\[a\]\[s\] | 第3级 \[s\] | 第3级 \[s\] |  |
| 军火64 | 第1级 \[d\]\[a\]\[s\] |  |  |  | 第3级 \[s\] | 第3级 \[s\] |
| 臂弹32 | 第3级 \[s\] |  |  |  | 第3级 \[s\] | 第3级 \[s\] |

以下指标显示每个平台都有何种交付机制。

"\[d\]" 发行专用(Debian,RPM等)包将提供给本平台,用于提交rosdistro的包.

" \[a\] " 二进制发布器作为每个平台的单一档案提供,包含银河系ROS 2 repos文件中的所有包\[^10\].

" \[s\] " 来源汇编。

中间软件执行支持 :

| 中间软件库 | 中件提供者 | 支助级别 | 平台 | 建筑 |
|----|----|----|----|----|
| rmw_cyclonedds_cpp\* | Eclipse Cyclone DDS | 第1级 | 所有平台 | 所有建筑 |
| rmw_fastrtps_cpp | eProsima Fast-DDS 软件 | 第1级 | 所有平台 | 所有建筑 |
| rmw_connextdds | RTI 连接 | 第1级 | Ubuntu, Windows, 和 macOS 软件 | 除arm64外的所有建筑 |
| rmw_fastrtps_dynamic_cpp | eProsima Fast-DDS 软件 | 第二级 | 所有平台 | 所有建筑 |
| rmw_gurumdds_cpp | GurumNetworks GurumDDS | 第3级 | Ubuntu 和 视窗 | 除arm32外的所有建筑 |

" \* " 是指未执行的《保护所有移徙工人及其家庭成员权利国际公约》。

Middleware 执行支持依赖于平台支持级别。例如,在 Tier 2 平台上执行的 Lier 1 中间软件只能得到 Tier 2 支持。

最低语文要求:

- C++17

- ⁇  3.6

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
<th class="head"><p>Ubuntu 焦点</p></th>
<th class="head"><p>视窗10**</p></th>
<th class="head"><p>第8条</p></th>
<th class="head"><p>马科斯**</p></th>
<th class="head"><p>德比安红心</p></th>
<th class="head"><p>已打开*</p></th>
</tr>
</thead>
<tbody>
<tr class="row-odd">
<td><p>CMake</p></td>
<td><p>3.16.3</p></td>
<td><p>3.19.1</p></td>
<td><p>3.18.2</p></td>
<td><p>3.14.4</p></td>
<td><p>3.18.4</p></td>
<td><p>3.16.1 / 3.12.2****</p></td>
</tr>
<tr class="row-even">
<td><p>爱咪</p></td>
<td colspan="6"><p>3.3.2</p></td>
</tr>
<tr class="row-odd">
<td><p>Gazebo</p></td>
<td><p>11.0.0*</p></td>
<td><p>N/A</p></td>
<td><p>N/A</p></td>
<td><p>11.0.0</p></td>
<td><p>11.0.0*</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>点火</p></td>
<td><p>楼 楼 *</p></td>
<td><p>N/A</p></td>
<td><p>N/A</p></td>
<td><p>楼 楼 *</p></td>
<td><p>楼 楼 *</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-odd">
<td><p>食人鱼</p></td>
<td colspan="5"><p>1.10*</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>打开CV</p></td>
<td><p>4.2.0</p></td>
<td><p>3.4.6*</p></td>
<td><p>3.4.6</p></td>
<td><p>4.2.0</p></td>
<td><p>4.5.1</p></td>
<td><p>4.1.0 / 3.2.0****</p></td>
</tr>
<tr class="row-odd">
<td><p>打开SSL</p></td>
<td><p>1.1.1d</p></td>
<td><p>1.1.1i</p></td>
<td><p>1.1.1g</p></td>
<td><p>1.1.1f</p></td>
<td><p>1.1.1i</p></td>
<td><p>1.1.1d / 1.1.1b****</p></td>
</tr>
<tr class="row-even">
<td><p>Python</p></td>
<td><p>3.8.0</p></td>
<td><p>3.8.3</p></td>
<td><p>3.6.8</p></td>
<td><p>3.8.2</p></td>
<td><p>3.9.1</p></td>
<td><p>3.8.2 / 3.7.5****</p></td>
</tr>
<tr class="row-odd">
<td><p>Qt 键</p></td>
<td><p>5.12.5</p></td>
<td><p>5.12.10</p></td>
<td><p>5.12.5</p></td>
<td><p>5.12.3</p></td>
<td><p>5.15.2</p></td>
<td><p>5.14.1 / 5.12.5****</p></td>
</tr>
<tr class="row-even">
<td colspan="3"></td>
<td colspan="4"><p><strong>仅限 Linux</strong></p></td>
</tr>
<tr class="row-odd">
<td><p>个人计算机L</p></td>
<td><p>1.10.0</p></td>
<td><p>N/A</p></td>
<td><p>1.11.1</p></td>
<td><p>N/A</p></td>
<td><p>1.11.1</p></td>
<td><p>1.10.0</p></td>
</tr>
<tr class="row-even">
<td colspan="7"><p><strong>ROMW DDS 中件供应商</strong></p></td>
</tr>
<tr class="row-odd">
<td><p>Cyclone DDS</p></td>
<td colspan="6"><p>0.8.x (法语).</p></td>
</tr>
<tr class="row-even">
<td><p>快速数据交换系统</p></td>
<td colspan="6"><p>2.3.x</p></td>
</tr>
<tr class="row-odd">
<td><p>Connext DDS</p></td>
<td colspan="2"><p>5.3.1</p></td>
<td><p>N/A</p></td>
<td><p>5.3.1</p></td>
<td colspan="2"><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>Gurum 数据交换系统</p></td>
<td colspan="2"><p>2.7.x</p></td>
<td colspan="4"><p>N/A</p></td>
</tr>
</tbody>
</table>

" \* " 是指这不是上游版本(可在官方操作系统寄存器上查阅),而是OSRF或社区分发的包(自定义寄存器上建立和分发的包)。

" \*\* " 滚动发行会看到这些依赖性在其存在期间的多个版本变化。为OpenEmberded显示的版本是3.1 Dunfell发行系列提供的版本;其他支持发行系列提供的版本在此列出: \<<https://github.com/ros/meta-ros/wiki/Package-Version-Differences>\>. 注意,根据此处显示的 OpenEmbed 支持策略,ROS distro 支持的 OpenEmbed 发布系列将在支持时间范围内改变: \<<https://github.com/ros/meta-ros/wiki/Policies#openembedded-release-series-support>\>. 然而,它将始终得到至少一个稳定的OpenEmbed发行系列的支持.

" \*\*\*\* " webOS OSE提供了这个不同的版本.

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

[安装银河系地理切龙](https://docs.ros.org/en/galactic/Installation.html)

<span id="new-features-in-this-ros-2-release"></span>

## 本 ROS 2 发布中的新功能

<span id="ability-to-specify-per-logger-log-levels"></span>

### 指定每个开发者日志级的能力

现在可以在命令行上为不同的伐木者指定不同的日志级别:

``` console
$ ros2 run demo_nodes_cpp talker --ros-args --log-level WARN --log-level talker:=DEBUG
```

以上命令设置了 WARN 的全局日志级别, 但将聊天者节点消息的日志级别设置为 DEBUG。 Name `--log-level` 命令行选项可以被通过任意次数,为每个日志设置不同的日志级别。

<span id="ability-to-configure-logging-directory-through-environment-variables"></span>

### 通过环境变量配置日志的能力

现在可以通过两个环境变量来配置日志目录: `ROS_LOG_DIR` 财务报告和财务报告 `ROS_HOME`。逻辑如下:

- 使用 `$ROS_LOG_DIR` 若为 `ROS_LOG_DIR` 已设定,而非空。

- 否则,使用 `$ROS_HOME/log`,使用 `~/.ros` (单位:千美元) `ROS_HOME` 如果未设置,或为空。

因此,默认值保持不变: `~/.ros/log`.

相关公关: [ros2/rcl_logging#53](https://github.com/ros2/rcl_logging/pull/53) 财务报告和财务报告 [ros2/launch#460](https://github.com/ros2/launch/pull/460).

例如:

``` bash
ROS_LOG_DIR=/tmp/foo ros2 run demo_nodes_cpp talker
```

将所有日志放入 `/tmp/foo`.

``` bash
ROS_HOME=/path/to/home ros2 run demo_nodes_cpp talker
```

将所有日志放入 `/path/to/home/log`.

<span id="ability-to-invoke-rosidl-pipeline-outside-cmake"></span>

### 援引能力 `rosidl` CMake外输油管

现在可以直接援引 `rosidl` CMake以外的接口生成管道。源代码生成器和接口定义翻译器可以通过统一的命令行接口访问。

例如,给予 `Demo` 给一些消息 `demo` 软件包类似 :

``` console
$ mkdir -p demo/msg
$ cd demo
$ cat << EOF > msg/Demo.msg
std_msgs/Header header
geometry_msgs/Twist twist
geometry_msgs/Accel accel
EOF
```

它很容易生成 C, C++,和 Python 支持源代码 :

``` console
$ rosidl generate -o gen -t c -t cpp -t py -I$(ros2 pkg prefix --share std_msgs)/.. \
  -I$(ros2 pkg prefix --share geometry_msgs)/.. demo msg/Demo.msg
```

生成的源代码将放入 `gen` 目录。

也可以将信件定义翻译为另一种格式,用于第三方代码生成工具的消费:

``` console
$ rosidl translate -o gen --to idl -I$(ros2 pkg prefix --share std_msgs)/.. \
  -I$(ros2 pkg prefix --share geometry_msgs)/.. demo msg/Demo.msg
```

翻译信件定义将放入 `gen` 目录。

请注意,这些工具产生源头,但不会建立源头 — — 责任仍然在呼叫者身上。 这是朝着使用户有能力的第一步。 `rosidl` 在 CMake 以外的构建系统中生成接口。参见 [设计文件](https://github.com/ros2/design/pull/310) 供进一步参考和采取下一步措施。

<span id="externally-configure-qos-at-start-up"></span>

### 启动时外部配置 QoS

现在可以在启动时为节点外部配置 QoS 设置。 QoS 设置是 **没有** 运行时可配置; 它们只在启动时可配置 。 节点作者必须在启动时选择允许更改 QoS 设置。 如果该特性在节点上被启用, 那么当节点开始时, QoS 设置可以使用 ROS 参数设定 。

[C++和Python中的Demos可以在这里找到.](https://github.com/ros2/demos/tree/a66f0e894841a5d751bce6ded4983acb780448cf/quality_of_service_demo#qos-overrides)

见 [更详细的设计文件](http://design.ros2.org/articles/qos_configurability.html).

注意,用户代码处理带有注册回调的参数更改时,应当避免拒绝未知参数的更新。在银河系之前,它被认为是不良做法,但如果外部可配置的QoS允许的话,它将导致硬性失败。

相关公关: [ros2/rclcpp#1408](https://github.com/ros2/rclcpp/pull/1408) 财务报告和财务报告 [ros2/rclpy#635](https://github.com/ros2/rclpy/pull/635)

<span id="python-point-cloud2-utilities-available"></span>

### 可用 Python 点\_ cloud2 工具

与下列机构进行互动的若干公用事业 [点云2 消息](https://github.com/ros2/common_interfaces/blob/galactic/sensor_msgs/msg/PointCloud2.msg) 在 Python 时 [移植到ROS 2](https://github.com/ros2/common_interfaces/pull/128)。这些公用设施允许从 PointCloud2 消息中获取一个点列表(`read_points` 财务报告和财务报告 `read_points_list`),并从点列表创建 PointCloud2 消息(`create_cloud` 财务报告和财务报告 `create_cloud_xyz32`).

一个创建 PointCloud 2 消息的例子, 然后读回:

``` python
import sensor_msgs_py.point_cloud2
from std_msgs.msg import Header

pointlist = [[0.0, 0.1, 0.2]]

pointcloud = sensor_msgs_py.point_cloud2.create_cloud_xyz32(Header(frame_id='frame'), pointlist)

for point in sensor_msgs_py.point_cloud2.read_points(pointcloud):
    print(point)
```

<span id="rviz2-time-panel"></span>

### RViz2 时间面板

Rviz2时间面板显示目前的墙壁和ROS时间以及经过的墙壁和ROS时间。 [移植到 RViz2](https://github.com/ros2/rviz/pull/599)。为了使时间面板能够使用,单击面板 - \> 添加新的面板,并选择“时间”。

![](rviz2-time-panel-2021-05-17.png) <span id="ros2-topic-echo-can-print-serialized-data"></span>

### ros2 主题回声可以打印序列化数据

当调试中间软件问题时,可以查看 RMW 发送的原始序列数据。 Name [- 绘制命令行旗](https://github.com/ros2/ros2cli/pull/470) 已添加到 `ros2 topic echo` 以显示此数据。要在操作中看到此数据,请运行以下命令。

第1航站楼:

``` console
$ ros2 topic pub /chatter std_msgs/msg/String "data: 'hello'"
```

第2航站楼:

``` console
$ ros2 topic echo --raw /chatter
b'\x00\x01\x00\x00\x06\x00\x00\x00hello\x00\x00\x00'
---
```

<span id="get-the-yaml-representation-of-messages"></span>

### 获取信件的 YAML 表示

现在可以使用 C++ 来获取所有信件的 YAML 表示 [to_yaml](https://github.com/ros2/rosidl/issues/523) 函数。打印 YAML 表示式的代码示例 :

``` c++
#include <cstdio>

#include <std_msgs/msg/string.hpp>

int main()
{
  std_msgs::msg::String msg;
  msg.data = "hello world";
  printf("%s", rosidl_generator_traits::to_yaml(msg).c_str());
  return 0;
}
```

<span id="ability-to-load-parameter-files-at-runtime-through-the-ros2-command"></span>

### 在运行时通过 ros2 命令加载参数文件的能力

ROS 2 早就有能力在启动时指定参数值( 通过命令行参数或 YAML 文件) , 并将当前参数向文件倾斜( 通过 `ros2 param dump`。银河加载能力到 [运行时装入参数值](https://github.com/ros2/ros2cli/pull/590) 从使用 YAML 文件 `ros2 param load` 动词。例如:

第1航站楼:

``` console
$ ros2 run demo_nodes_cpp parameter_blackboard
```

第2航站楼:

``` console
$ ros2 param set /parameter_blackboard foo bar  # sets 'foo' parameter to value 'bar'
$ ros2 param dump /parameter_blackboard  # dumps current value of parameters to ./parameter_blackboard.yaml
$ ros2 param set /parameter_blackboard foo different  # sets 'foo' parameter to value 'different'
$ ros2 param load /parameter_blackboard ./parameter_blackboard.yaml  # reloads previous state of parameters, 'foo' is back to 'bar'
```

<span id="tools-to-check-for-qos-incompatibilities"></span>

### 用于检查 QoS 兼容性的工具

建在新的 QoS 兼容性检查 API 之上, `ros2doctor` 财务报告和财务报告 `rqt_graph` 现在可以发现并报告QoS出版商与订阅商之间的不兼容性.

出版商和订户 [不兼容的QoS 设置](../Concepts/Intermediate/About-Quality-of-Service-Settings.md):

第1航站楼:

``` console
$ ros2 run demo_nodes_py talker_qos -n 1000  # i.e. best_effort publisher
```

第2航站楼:

``` console
$ ros2 run demo_nodes_py listener_qos --reliable -n 1000  # i.e. reliable subscription
```

`ros2doctor` 报告:

``` console
$ ros2 doctor --report
~ ...
   QOS COMPATIBILITY LIST
topic [type]            : /chatter [std_msgs/msg/String]
publisher node          : talker_qos
subscriber node         : listener_qos
compatibility status    : ERROR: Best effort publisher and reliable subscription;
~ ...
```

时 `rqt_graph` 显示 :

![](images/rqt_graph-qos-incompatibility-2021-05-17.png)

相关公关: [ros2/ros2cli#621](https://github.com/ros2/ros2cli/pull/621), [ros-visualization/rqt_graph#61](https://github.com/ros-visualization/rqt_graph/pull/61)

<span id="use-launch-substitutions-in-parameter-files"></span>

### 在参数文件中使用启动替换

碞钩 `rosparam` ROS 1 中的标记 `roslaunch`, `launch_ros` 现在可以评价参数文件中的替换。

例如,给一些 `parameter_file_with_substitutions.yaml` 类似如下:

``` yaml
/**:
  ros__parameters:
    launch_date: $(command date)
```

设定 `allow_substs` 改为: `True` 以获得对替换的评价 `Node` 发射:

``` python
import launch
import launch_ros.parameter_descriptions
import launch_ros.actions

def generate_launch_description():
    return launch.LaunchDescription([
        launch_ros.actions.Node(
            package='demo_nodes_cpp',
            executable='parameter_blackboard',
            parameters=[
                launch_ros.parameter_descriptions.ParameterFile(
                    param_file='parameter_file_with_substitutions.yaml',
                    allow_substs=True)
            ]
        )
    ])
```

XML发射文件也支持这个.

``` xml
<launch>
  <node pkg="demo_nodes_cpp" exec="parameter_blackboard">
    <param from="parameter_file_with_substitutions.yaml" allow_substs="true"/>
  </node>
</launch>
```

相关 PR : [ros2/launch_ros#168](https://github.com/ros2/launch_ros/pull/168)

<span id="support-for-unique-network-flows"></span>

### 支持独特的网络流动

应用程序现在可能要求UDP/TCP和基于IP的RMW执行提供独特的 *网络流量* (即独一无二) [差异服务代码点](https://tools.ietf.org/html/rfc2474) 和(或)独特性 [IPv6 流标签](https://tools.ietf.org/html/rfc6437) 和/或IP包头中的独特端口),用于出版商和订阅,使得支持这种功能的网络架构中的这些IP流的QoS规格成为可能,如5G网络.

要在操作中看到这一点, 您可以运行这些 C++ 示例( 将在 [ros2/examples](https://github.com/ros2/examples) 仓库 :

第1航站楼:

``` console
$ ros2 run examples_rclcpp_minimal_publisher publisher_member_function_with_unique_network_flow_endpoints
```

第2航站楼:

``` console
$ ros2 run examples_rclcpp_minimal_subscriber subscriber_member_function_with_unique_network_flow_endpoints
```

见 [独特的网络流设计文件](https://github.com/ros2/design/pull/304) 供进一步参考。

<span id="rosbag2-new-features"></span>

### Rosbag2 新特性

<span id="split-recording-by-time"></span>

#### 按时间划分记录

在 Foxy 中, 只能按包大小分拆包包, 现在也可以按时间分拆。 以下命令会将包文件分割成 100 秒块 。

``` console
$ ros2 bag record --all --max-bag-duration 100
```

<span id="ros2-bag-list"></span>

#### ROS2 袋列表

这个新命令列表已安装了 Rosbag2 使用的各类插件 。

``` console
$ ros2 bag list storage
rosbag2_v2
sqlite3

$ ros2 bag list converter
rosbag_v2_converter
```

<span id="compression-implementation-is-a-plugin"></span>

#### 压缩执行是一个插件

在 Foxy 中, Rosbag2 压缩用 Zstd 库执行的硬编码。 它已经重新配置, 以便压缩执行成为插件, 并且可以互换, 不修改核心 Rosbag2 编码库 。 `ros-galactic-rosbag2` 仍然是 Zstd 插件 - 但现在可以释放和使用更多的,通过选择性安装软件包 Zstd 可以排除在安装之外。

<span id="compress-per-message"></span>

#### 压缩每个消息

在Foxy中,您可以自动压缩每个rosbag文件的拆分(每个文件压缩),但现在您也可以指定每个消息压缩.

``` console
$ ros2 bag record --all --compression-format zstd --compression-mode message
```

<span id="rosbag2-python-api"></span>

#### 罗斯巴格2 Python API

新软件包 `rosbag2_py` 已经在 Galactic 中发布,它提供了 Python API。此软件包是 `pybind11` 绑定在 C++ API 周围。截至银河系统初始发布时,它尚未披露所有通过该程序提供的功能。 `rosbag2_cpp` API,但它是唯一的连接 `ros2 bag` CLI工具,所以有大量的功能可用.

<span id="performance-testing-package-and-performance-improvements"></span>

#### 性能测试包和性能改进

自Foxy发布以来,对Rosbag2进行了彻底的性能分析项目。 <https://github.com/ros2/rosbag2/blob/galactic/rosbag2_performance/rosbag2_performance_benchmarking/docs/rosbag2_performance_improvements.pdf> 软件包 `rosbag2_performance_benchmarking` 提供进行性能分析的工具,特别是在记录方面,这有助于我们维护和改进Rosbag2的性能。

在这份报告之后,关键的工作确实提高了性能,使实际机器人工作流程的性能更加可用。要突出一个关键度量标准——在高带宽压力测试(200Mbps)中,Foxy的发布率下降到70%,而Galactic版本的保存率约为100%。请参见链接报告的细节。

<span id="regex-and-exclude-options-for-topic-selection"></span>

#### `--regex` 财务报告和财务报告 `--exclude` 选择主题的选项

新的录音选项 `--regex` 财务报告和财务报告 `--exclude` 允许对包中记录的主题进行微调,而无需明确列出所有主题。这些选项可以一起使用,也可以单独使用,并结合 `--all`

以下命令将只记录名称中的“扫描”主题 。

``` console
$ ros2 bag record --regex "*scan*"
```

以下命令将记录除其中主题以外的所有主题 `/my_namespace/`

``` console
$ ros2 bag record --all --exclude "/my_namespace/*"
```

<span id="ros2-bag-reindex"></span>

#### `ros2 bag reindex`

ROS 2 袋由目录来代表,而不是一个文件。此目录包含一个 `metadata.yaml` 文件,以及一个或多个包文件。当 `metadata.yaml` 文件丢失或缺失, `ros2 bag reindex $bag_dir` 将尝试通过读取目录中的所有包文件来重建它 。

<span id="playback-time-control"></span>

#### 播放时间控制

已经为 rosbag2 播放添加了新的控制 - 暂停( R) 、 更改速率和播放后缀。 截至银河发布时, 这些控制只作为 rosbag2 播放器节点上的服务被曝光。 正在开发中, 以将其曝光于键盘控制中 。 `ros2 bag play`,但在此之前,带有按钮或键盘控件的用户应用程序可能会被轻而易举地执行来调用这些服务.

在一个壳中:

``` console
$ ros2 bag play my_bag
```

在另一个外壳中 :

``` console
$ ros2 service list -t
/rosbag2_player/get_rate [rosbag2_interfaces/srv/GetRate]
/rosbag2_player/is_paused [rosbag2_interfaces/srv/IsPaused]
/rosbag2_player/pause [rosbag2_interfaces/srv/Pause]
/rosbag2_player/play_next [rosbag2_interfaces/srv/PlayNext]
/rosbag2_player/resume [rosbag2_interfaces/srv/Resume]
/rosbag2_player/set_rate [rosbag2_interfaces/srv/SetRate]
/rosbag2_player/toggle_paused [rosbag2_interfaces/srv/TogglePaused]

$ ros2 service call /rosbag2_player/is_paused rosbag2_interfaces/IsPaused
```

暂停播放 :

``` console
$ ros2 service call /rosbag2_player/pause rosbag2_interfaces/Pause
```

要恢复播放 :

``` console
$ ros2 service call /rosbag2_player/resume rosbag2_interfaces/Resume
```

要将暂停的播放状态改为相反。 如果播放, 则暂停。 如果暂停, 则恢复 。

``` console
$ ros2 service call /rosbag2_player/toggle_paused rosbag2_interfaces/TogglePaused
```

要获得当前播放率 :

``` console
$ ros2 service call /rosbag2_player/get_rate
```

要设置当前播放率( 必须 \> 0) :

``` console
$ ros2 service call /rosbag2_player/set_rate rosbag2_interfaces/SetRate "rate: 0.1"
```

要播放一个单独的下一条消息( 只在暂停时工作) :

``` console
$ ros2 service call /rosbag2_player/play_next rosbag2_interfaces/PlayNext
```

<span id="playback-publishes-clock"></span>

#### 回放发布 / 小时

Rosbag2公司还可以通过向下列机构出版《模拟时间》来规定“模拟时间”。 `/clock` 重播时的话题。以下命令将在正常间隔时发布时钟消息 。

要以40Hz的默认速度发布 :

``` console
$ ros2 bag play my_bag --clock
```

以特定速率发布,例如100赫兹:

``` console
$ ros2 bag play my_bag --clock 100
```

<span id="changes-since-the-foxy-release"></span>

## Foxy 发布后的变化

<span id="default-rmw-changed-to-eclipse-cyclone-dds"></span>

### 默认 RMW 更改为 Eclipse 气旋 DDS

在银河开发过程中,ROS 2技术指导委员会 [表决](https://discourse.ros.org/t/ros-2-galactic-default-middleware-announced/18064) 将默认的 ROS 中间软件( RMW) 更改为 [Eclipse Cyclone DDS](https://github.com/eclipse-cyclonedds/cyclonedds) 项目 [Eclipse基金会](https://www.eclipse.org)。如果没有任何配置更改,用户将默认获得 Eclipse 旋风 DDS。快速 DDS 和 Connext 仍然是支持 RTW 的 Tier-1 销售商,用户可以通过使用 RTW 来选择使用其中一种 RTW 。 `RMW_IMPLEMENTATION` 环境变量。 [与多个《房管协定》实施指南合作](../How-To-Guides/Working-with-multiple-RMW-implementations.md) 以获取更多信息。

<span id="connext-rmw-changed-to-rmw-connextdds"></span>

### 连接 ROW 改为 rmw\_ connendds

Connext 的新 RMW 呼叫 [rmw_connextdds](https://github.com/ros2/rmw_connextdds) 银河系的RMW有更好的性能 并且解决了许多与更老的RMW有关的问题 `rmw_connext_cpp`.

<span id="large-improvements-in-testing-and-overall-quality"></span>

### 测试和整体质量的大幅提高.

银河系包含许多改变,可以固定赛车条件,插补内存泄漏,并解决用户报告的问题。 除了这些改变外,银河系开发期间还做出了协调一致的努力,通过实施提高系统的整体质量. [REP 2004 (英语).](https://reps.openrobotics.org/rep-2004/)。该词 `rclcpp` 软件包及其所有依赖性(包括大多数ROS 2非Python核心软件包)被提高到 [质量一级](https://reps.openrobotics.org/rep-2004/#quality-level-1) 通过:

- 具有版本政策(QL1要求1)

- 具有文件记录的改变控制程序(QL1要求2)

- 记录所有功能和公共API(QL1要求3)

- 增加许多额外测试(QL1要求4):

  - 对所有特性的系统测试

  - 所有公共API的单元测试

  - 夜间性能测试

  - 密码覆盖率为95%

- 包的所有运行时间依赖性至少与包一样高(QL1要求 5)

- 支持所有REP-2000平台(QL1要求6)

- 制定脆弱性披露政策(QL1要求7)

<span id="rmw"></span>

### rmw (英语).

<span id="new-api-for-checking-qos-profile-compatibility"></span>

#### 用于检查 QoS 配置文件兼容性的新 API

`rmw_qos_profile_check_compatible` 是检查两个 QoS 配置文件兼容性的新函数。

RAMW销售商应采用这一API,用于QoS调试和在工具中的反演功能,如: `rqt_graph` 以正确的方式工作。

相关 PR : [ros2/rmw#299](https://github.com/ros2/rmw/pull/299)

<span id="ament-cmake"></span>

### ament_cmake

<span id="ament-install-python-package-now-installs-a-python-egg"></span>

#### `ament_install_python_package()` 现在安装 Python 彩蛋

通过安装一个平面的 Python 蛋, Python 套件使用 `ament_install_python_package()` 可以通过模块,例如 `pkg_resources` 财务报告和财务报告 `` `importlib.metadata ``此外,还可以以下列方式提供额外元数据: `setup.cfg` 文档(包括条目)。

相关 PR : [ament/ament_cmake#326](https://github.com/ament/ament_cmake/pull/326)

<span id="ament-target-dependencies-handles-system-dependencies"></span>

#### `ament_target_dependencies()` 处理系统依赖性

一些软件包的依赖性现在可以被标记为系统依赖性,帮助应对外部代码中的警告。 通常,系统依赖性也被排除在依赖性计算之外 — — 谨慎地使用它们。

相关 PR : [ament/ament_cmake#297](https://github.com/ament/ament_cmake/pull/297)

<span id="nav2"></span>

### 导航2

更改包括但不限于一些稳定性的改进,新的插件,界面的更改,成本映射过滤器。见 [移徙指南](https://navigation.ros.org/migration/Foxy.html) 用于完整列表

<span id="tf2-ros-python-split-out-of-tf2-ros"></span>

### tf2_ros Python 从 tf2_ros 分裂出来

曾经生活在 tf2_ros 的 Python 代码已被移入它自己的名为 tf2_ros_py 的软件包。 任何现有的依赖于 tf2_ros 的 Python 代码都会继续工作, 但是这些软件包的软件包. xml 应该修改为 `exec_depend` 在 tf2_ros_py 上输入。

<span id="tf2-ros-python-transformlistener-uses-global-namespace"></span>

### tf2_ros Python 变形听器使用全局命名空间

蟒蛇 `TransformListener` 现在订阅 `/tf` 财务报告和财务报告 `/tf_static` 在全域命名空间中。以前,它是在节点的命名空间中悬浮。这意味着节点的命名空间将不再对节点的命名空间产生影响。 `/tf` 财务报告和财务报告 `/tf_static` 订阅.

例如:

``` console
$ ros2 run tf2_ros tf2_echo --ros-args -r __ns:=/test -- odom base_link
```

将订阅 `/tf` 财务报告和财务报告 `/tf_static`,作为 `ros2 topic list` 将显示。

相关 PR : [ros2/geometry2#390](https://github.com/ros2/geometry2/pull/390)

<span id="rclcpp"></span>

### rclcpp

<span id="change-in-spin-until-future-complete-template-parameters"></span>

#### 旋转到\_ 未来\_ 完整模板参数的变化

第一个模板参数: `Executor::spin_until_future_complete` 是未来的结果类型 `ResultT`,而该方法只接受a `std::shared_future<ResultT>`. 为了接受其他类型的期货(如: `std::future`),该参数被修改为未来类型本身。

地点: `spin_until_future_complete` 调用时依赖于模板参数的扣减,不需要更改 。 如果没有, 这是一个 diff 的例子 :

``` dpatch
std::shared_future<MyResultT> future;
...
-executor.spin_until_future_complete<MyResultT>(future);
+executor.spin_until_future_complete<std::shared_future<MyResultT>>(future);
```

详情见 [ros2/rclcpp#1160](https://github.com/ros2/rclcpp/pull/1160)。关于需要修改用户代码的例子,请参见 [ros-visualization/interactive_markers#72](https://github.com/ros-visualization/interactive_markers/pull/72).

<span id="change-in-default-clock-subscription-qos-profile"></span>

#### 默认更改 `/clock` 订阅 QoS 配置文件

默认从具有历史深度10的可靠通信改为具有历史深度1的最好努力通信. [ros2/rclcpp#1312](https://github.com/ros2/rclcpp/pull/1312).

<span id="waitable-api"></span>

#### 等待 API

等待的 API 被修改,以避免与 `MultiThreadedExecutor`。这只影响用户执行可等待的自定义。见 [ros2/rclcpp#1241](https://github.com/ros2/rclcpp/pull/1241) 更多细节。

<span id="change-in-rclcpp-s-logging-macros"></span>

#### 变动 `rclcpp`日志宏

之前,伐木的宏 很容易受到 [格式字符串攻击](https://owasp.org/www-community/attacks/Format_string_attack),如果格式字符串被评估,并且有可能执行代码,读取堆栈,或者在运行程序中造成分区错误。为了解决这个安全问题,日志宏现在只接受其格式字符串参数的字符串字符串。

如果你以前有代码,比如:

``` default
const char *my_const_char_string format = "Foo";
RCLCPP_DEBUG(get_logger(), my_const_char_string);
```

您现在应该将其替换为:

``` default
const char *my_const_char_string format = "Foo";
RCLCPP_DEBUG(get_logger(), "%s", my_const_char_string);
```

或 :

``` default
RCLCPP_DEBUG(get_logger(), "Foo");
```

此更改会从日志宏中删除一些方便, 因为 `std::string`s不再被接受为格式参数。

如果您以前有代码, 没有格式参数, 如 :

``` default
std::string my_std_string = "Foo";
RCLCPP_DEBUG(get_logger(), my_std_string);
```

您现在应该将其替换为:

``` default
std::string my_std_string = "Foo";
RCLCPP_DEBUG(get_logger(), "%s", my_std_string.c_str());
```

> **说明**
>
> 如果你在使用一个 `std::string` 作为带有格式参数的格式字符串,将该字符串转换为 `char *` 并使用它作为格式字符串将生成格式安全警告。这是因为编译器无法编译到内向 `std::string` 来验证参数。为避免安全警告,我们建议您手动构建字符串,并在没有像前一个例子那样的格式参数的情况下传递。

`std::stringstream` 类型仍然被接受为流日志宏的参数。见 [ros2/rclcpp#1442](https://github.com/ros2/rclcpp/pull/1442) 更多细节。

<span id="parameter-types-are-now-static-by-default"></span>

#### 参数类型现在默认为静态

以前,参数的类型在设定参数时可以更改。例如,如果一个参数被宣布为整数,那么以后将参数设置为字符串的调用可能会导致错误,而且很少是用户想要的。至于银河参数类型默认是静态的,试图改变该类型将失败。如果想要之前的动态行为,则有选择它的机制(见下文代码) 。

``` cpp
// declare integer parameter with default value, trying to set it to a different type will fail.
node->declare_parameter("my_int", 5);
// declare string parameter with no default and mandatory user provided override.
// i.e. the user must pass a parameter file setting it or a command line rule -p <param_name>:=<value>
node->declare_parameter("string_mandatory_override", rclcpp::PARAMETER_STRING);
// Conditionally declare a floating point parameter with a mandatory override.
// Useful when the parameter is only needed depending on other conditions and no default is reasonable.
if (mode == "modeA") {
    node->declare_parameter("conditionally_declare_double_parameter", rclcpp::PARAMETER_DOUBLE);
}
// You can also get the old dynamic typing behavior if you want:
rcl_interfaces::msg::ParameterDescriptor descriptor;
descriptor.dynamic_typing = true;
node->declare_parameter("dynamically_typed_param", rclcpp::ParameterValue{}, descriptor);
```

详情见 <https://github.com/ros2/rclcpp/blob/galactic/rclcpp/doc/notes_on_statically_typed_parameters.md>.

<span id="id1"></span>

#### 用于检查 QoS 配置文件兼容性的新 API

`qos_check_compatible` 是检查两个 QoS 配置文件兼容性的新函数。

相关 PR : [ros2/rclcpp#1554](https://github.com/ros2/rclcpp/pull/1554)

<span id="rclpy"></span>

### rclpy

<span id="removal-of-deprecated-node-set-parameters-callback"></span>

#### 删除已贬值的节点. set_参数_召回

方法 `Node.set_parameters_callback` 当时是 [在 ROS Foxy 中贬值](https://github.com/ros2/rclpy/pull/504) 并且一直以来 [在 ROS 银河系中删除](https://github.com/ros2/rclpy/pull/633)使用时 `Node.add_on_set_parameters_callback()` 换句话说,这里有一些使用它的例子代码。

``` python
import rclpy
import rclpy.node
from rcl_interfaces.msg import ParameterType
from rcl_interfaces.msg import SetParametersResult


rclpy.init()
node = rclpy.node.Node('callback_example')
node.declare_parameter('my_param', 'initial value')


def on_parameter_event(parameter_list):
    for parameter in parameter_list:
        node.get_logger().info(f'Got {parameter.name}={parameter.value}')
    return SetParametersResult(successful=True)


node.add_on_set_parameters_callback(on_parameter_event)
rclpy.spin(node)
```

运行此命令以查看参数的召回动作 。

``` default
ros2 param set /callback_example my_param "Hello World"
```

<span id="id2"></span>

#### 参数类型现在默认为静态

在Foxy和早先的调用中,设置一个参数可以改变其类型。由于银河参数类型是静态的,不能默认更改。如果需要先前的行为,则设置 `dynamic_typing` 在参数描述符中为真。这里有一个示例。

``` python
import rclpy
import rclpy.node
from rcl_interfaces.msg import ParameterDescriptor

rclpy.init()
node = rclpy.node.Node('static_param_example')
node.declare_parameter('static_param', 'initial value')
node.declare_parameter('dynamic_param', 'initial value', descriptor=ParameterDescriptor(dynamic_typing=True))
rclpy.spin(node)
```

运行这些命令以查看静态和动态键入的参数是如何不同的.

``` console
$ ros2 param set /static_param_example dynamic_param 42
Set parameter successful
$ ros2 param set /static_param_example static_param 42
Setting parameter failed: Wrong parameter type, expected 'Type.STRING' got 'Type.INTEGER'
```

详情见 <https://github.com/ros2/rclcpp/blob/galactic/rclcpp/doc/notes_on_statically_typed_parameters.md>.

<span id="id3"></span>

#### 用于检查 QoS 配置文件兼容性的新 API

`rclpy.qos.qos_check_compatible` 是,这是 [一个新的函数](https://github.com/ros2/rclpy/pull/708) 用于检查两个 QoS 配置文件的兼容性。如果配置文件兼容,那么使用它们的出版商和订阅商将能够相互交谈。

``` python
import rclpy.qos

publisher_profile = rclpy.qos.qos_profile_sensor_data
subscription_profile = rclpy.qos.qos_profile_parameter_events

print(rclpy.qos.qos_check_compatible(publisher_profile, subscription_profile))
```

``` console
$ python3 qos_check_compatible_example.py
(QoSCompatibility.ERROR, 'ERROR: Best effort publisher and reliable subscription;')
```

<span id="rclcpp-action"></span>

### rclcpp_action

<span id="action-client-goal-response-callback-signature-changed"></span>

#### 行动客户端目标响应回调签名更改

目标响应回调现在应该用一个共同的指针指向一个目标手柄,而不是未来.

为: [实例](https://github.com/ros2/examples/pull/291),旧签名 :

``` c++
void goal_response_callback(std::shared_future<GoalHandleFibonacci::SharedPtr> future)
```

新签名 :

``` c++
void goal_response_callback(GoalHandleFibonacci::SharedPtr goal_handle)
```

相关 PR : [ros2/rclcpp#1311](https://github.com/ros2/rclcpp/pull/1311)

<span id="rosidl-typesupport-introspection-c"></span>

### rosidl_typesupport_introspection_c

<span id="api-break-in-function-that-gets-an-element-from-an-array"></span>

#### API 函数断裂,从数组中获取元素

函数的签名之所以被更改,是因为它与用于从数组或序列中获取元素的所有其他函数都有着明显的不同。这只影响到使用内切类型支持的rmw执行的作者。

详细情况见 [ros2/rosidl#531](https://github.com/ros2/rosidl/pull/531).

<span id="rcl-lifecycle-and-rclcpp-lifecycle"></span>

### rcl_生命周期和 rclpp_生命周期

<span id="rcl-s-lifecycle-state-machine-gets-new-init-api"></span>

#### RCL 的生命周期状态机器获得新的 API

rcl_lifecycle中的生命周期状态机器被修改,以期望新引入的选项 struct,结合状态机器的一般配置. option struct可以表示状态机器是否应该以默认值初始化,其附属服务是否活跃,以及使用哪个分配器.

``` c
rcl_ret_t
rcl_lifecycle_state_machine_init(
  rcl_lifecycle_state_machine_t * state_machine,
  rcl_node_t * node_handle,
  const rosidl_message_type_support_t * ts_pub_notify,
  const rosidl_service_type_support_t * ts_srv_change_state,
  const rosidl_service_type_support_t * ts_srv_get_state,
  const rosidl_service_type_support_t * ts_srv_get_available_states,
  const rosidl_service_type_support_t * ts_srv_get_available_transitions,
  const rosidl_service_type_support_t * ts_srv_get_transition_graph,
  const rcl_lifecycle_state_machine_options_t * state_machine_options);
```

<span id="rcl-s-lifecycle-state-machine-stores-allocator-instance"></span>

#### RCL 的生命周期状态机器仓库分配器实例

结构选项( 上文讨论过) 需要使用分配器初始化状态机器的示例。 此选项将结构化, 并在那里将包含的分配器存储在生命周期状态机器中。 作为直接后果, `rcl_lifecycle_fini function` 不再期望一个分配器的Fini函数,而是使用结构选项中的分配器进行内部数据结构的处理。

``` c
rcl_ret_t
rcl_lifecycle_state_machine_fini(
  rcl_lifecycle_state_machine_t * state_machine,
  rcl_node_t * node_handle);
```

<span id="rclcpp-s-lifecycle-node-exposes-option-to-not-instantiate-services"></span>

#### RCLCPP 的生命周期节点会显示不即时服务选项

为了使用 Rclcpp 的生命周期节点而不暴露其内部服务,例如 `change_state`, `get_state` 等, 寿命周期节点的构建者有一个新引入的参数, 表示服务是否应该可用。 此布尔标记默认为真实, 如果不想, 不需要对现有 API 做任何修改 。

``` c++
explicit LifecycleNode(
  const std::string & node_name,
  const rclcpp::NodeOptions & options = rclcpp::NodeOptions(),
  bool enable_communication_interface = true);
```

相关公关: [ros2/rcl#882](https://github.com/ros2/rcl/pull/882) 财务报告和财务报告 [ros2/rclcpp#1507](https://github.com/ros2/rclcpp/pull/1507)

<span id="id4"></span>

### rcl_生命周期和 rclpp_生命周期

<span id="recording-split-by-time"></span>

#### 记录 - 按时间划分

<span id="known-issues"></span>

## 已知问题

<span id="ros2cli"></span>

### 罗斯2cli

<span id="daemon-slows-down-cli-on-windows"></span>

#### 守护进程在 Windows 上慢化 CLI

作为工作变通,CLI命令可以不用守护进程使用,例如:

``` console
$ ros2 topic list --no-daemon
```

问题由 [ros2/ros2cli#637](https://github.com/ros2/ros2cli/issues/637).

<span id="rqt"></span>

### rqt

<span id="some-rqt-bag-icons-are-missing"></span>

#### 缺少一些 rqt_bag 图标

“Zoom In”、“Zoom Out”、“Zoom Home”和“Togle Thumbnails”的图标在下列地点缺失: `rqt_bag`。这个问题将在 [ros-visualization/rqt_bag#102](https://github.com/ros-visualization/rqt_bag/issues/102)

<span id="most-rqt-utilities-don-t-work-standalone-on-windows"></span>

#### 大部分rqt 公用设施并不单独在 Windows 上工作

在Windows上“独立”启动rqt公用事业(如 `ros2 run rqt_graph rqt_graph`通常没有效果。 工作方向是启动rqt容器工艺(`rqt`),然后插入要使用的插件。

<span id="rviz2"></span>

### rviz2 (中文(简体) ).

<span id="rviz2-panel-close-buttons-are-blank"></span>

#### RViz2 面板关闭按钮为空白

每个 RViz2 面板的右上角应包含一个“ X ” , 以便关闭面板。 这些按钮已经存在, 但其中的“ X ” 在所有平台上都缺失。 这个问题正在被跟踪到 [ros2/rviz2#692](https://github.com/ros2/rviz/issues/692).

<span id="timeline-before-the-release"></span>

## 发布前的时间线

> Mon. 2021年3月22日 - Alpha (英语).  
> ROS Core的初步测试和稳定 <span id="id5"></span>[\[1\]](#id10) 软件包。
>
> 2021年4月5日-冻结  
> ROS Core 的 API 和特性冻结 <span id="id6"></span>[\[1\]](#id10) 软件包在滚筒里。请注意,这包括 `rmw`,这是递归依赖 `ros_core`。在此点之后,只发布错误修正。新软件包可以独立发布。
>
> Mon. 2021年4月19日 - 分会  
> 罗林瑞德利的分店 `rosdistro` 重新打开用于 ROS Core 的滚动PRs <span id="id7"></span>[\[1\]](#id10) 软件包。银河开发从 `ros-rolling-*` 软件包到 `ros-galactic-*` 软件包。
>
> Mon. 2021年4月26日 - 贝塔.  
> ROS 桌面更新版 <span id="id8"></span>[\[2\]](#id11) 可用软件包。请进行一般测试。
>
> Mon. 2021年5月17日 - RC (中文(简体) ).  
> 发布候选软件包已构建.  
> ROS 桌面更新版 <span id="id9"></span>[\[2\]](#id11) 可用软件包。
>
> Thu. 2021年5月20日 - Distro冻结  
> 冻结Rodistro 银河系没有公关 `rosdistro` Repo将合并(发布公告后重新开放).
>
> Sun. 2021年5月23日 - 一般可用性  
> 发布公告.  
> `rosdistro` 重新打开银河系PR。

<span id="id10"></span>

\[1\] ([1](#id5),[2](#id6),[3](#id7))

那个... `ros_core` 变体描述于 [REP 2001(对数)](https://reps.openrobotics.org/rep-2001/#ros-core).

<span id="id11"></span>

\[2\] ([1](#id8),[2](#id9))

那个... `desktop` 变体描述于 [REP 2001(桌面变量)](https://reps.openrobotics.org/rep-2001/#desktop-variants).
