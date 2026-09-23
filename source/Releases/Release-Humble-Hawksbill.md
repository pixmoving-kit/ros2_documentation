---
translation_status: machine_translated
source: Releases/Release-Humble-Hawksbill.rst
---

<span id="humble-hawksbill-humble"></span> <span id="humble-release"></span>

# Humble Hawksbill（`humble`)

*Humble Hawksbill* 以下是Humble Hawksbill自上次发布以来重要变化和特征的亮点。 [长窗体变化日志](Humble-Hawksbill-Complete-Changelog.md).

<span id="supported-platforms"></span>

## 支持的平台

Humble Hawksbill 支持以下平台: 根据 [平台支持级别](../The-ROS2-Project/Platform-Support-Tiers.md):

第一级平台:

- 乌本图22.04 (詹姆斯): `amd64` 财务报告和财务报告 `arm64`

- Windows 10 (Visual Studio 2019): (英语). `amd64`

第二级平台:

- 莱尔8: `amd64`

第三级平台:

- 乌本图20.04(福尔): `amd64`

- 马科斯: `amd64`

- 德比安红眼: `amd64`

目标平台:

| 建筑 | 乌班图·贾米(22.04) | Windows 10 (VS2019) (英语). | 第8条 | 乌邦图联络人(20.04) | macOS | 德比安公牛(11) | OpenEmbed / Yocto 项目 |
|----|----|----|----|----|----|----|----|
| amd64 (中文(简体) ). | 第1级 \[d\]\[a\]\[s\] | 第1级 \[s\] | 第二级\[d\]\[a\]\[s\] | 第3级 \[s\] | 第3级 \[s\] | 第3级 \[s\] | 第3级 \[s\] |
| 军火64 | 第1级 \[d\]\[a\]\[s\] |  |  | 第3级 \[s\] |  | 第3级 \[s\] | 第3级 \[s\] |
| 臂弹32 | 第3级 \[s\] |  |  | 第3级 \[s\] |  | 第3级 \[s\] | 第3级 \[s\] |

以下指标显示每个平台都有何种交付机制。

"\[d\]" 发行专用(Debian,RPM等)包将提供给本平台,用于提交rosdistro的包.

" \[a\] " 二进制版本作为每个平台的单一档案提供,包含Humble ROS 2 repos文件中的所有软件包\[^11\].

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

- ⁇  3.6

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
<th class="head"><p>乌邦图查米</p></th>
<th class="head"><p>视窗10**</p></th>
<th class="head"><p>第8条</p></th>
<th class="head"><p>Ubuntu 焦点</p></th>
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
<td><p>3.16.3</p></td>
<td><p>3.14.4</p></td>
<td><p>3.18.4</p></td>
<td><p>3.22.3 / 3.16.5***</p></td>
</tr>
<tr class="row-even">
<td><p>爱咪</p></td>
<td><p>3.3.4</p></td>
<td colspan="6"><p>3.3.2</p></td>
</tr>
<tr class="row-odd">
<td><p>Gazebo 经典</p></td>
<td><p>11.x.x*</p></td>
<td><p>N/A</p></td>
<td><p>N/A</p></td>
<td><p>11.0.0*</p></td>
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
<td><p>堡垒*</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-odd">
<td><p>数字</p></td>
<td><p>1.21.5</p></td>
<td><p>1.18.4</p></td>
<td><p>1.14.3</p></td>
<td><p>1.17.4</p></td>
<td><p>1.18.4</p></td>
<td><p>1.19.5</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>食人鱼</p></td>
<td colspan="6"><p>1.12.1*</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-odd">
<td><p>打开CV</p></td>
<td><p>4.5.4</p></td>
<td><p>3.4.6*</p></td>
<td><p>3.4.6</p></td>
<td><p>4.2.0</p></td>
<td><p>4.2.0</p></td>
<td><p>4.5.1</p></td>
<td><p>4.1.0 / 3.2.0***</p></td>
</tr>
<tr class="row-even">
<td><p>打开SSL</p></td>
<td><p>1.1.1l</p></td>
<td><p>1.1.1l</p></td>
<td><p>1.1.1k</p></td>
<td><p>1.1.1d</p></td>
<td><p>1.1.1f</p></td>
<td><p>1.1.1i</p></td>
<td><p>1.1.1d / 1.1.1b***</p></td>
</tr>
<tr class="row-odd">
<td><p>Python</p></td>
<td><p>3.10.4</p></td>
<td><p>3.8.3</p></td>
<td><p>3.6.8</p></td>
<td><p>3.8.0</p></td>
<td><p>3.8.2</p></td>
<td><p>3.9.1</p></td>
<td><p>3.8.2 / 3.7.5***</p></td>
</tr>
<tr class="row-even">
<td><p>Qt 键</p></td>
<td><p>5.15.3</p></td>
<td><p>5.12.12</p></td>
<td><p>5.15.2</p></td>
<td><p>5.12.5</p></td>
<td><p>5.12.3</p></td>
<td><p>5.15.2</p></td>
<td><p>5.14.1 / 5.12.5***</p></td>
</tr>
<tr class="row-odd">
<td colspan="2"></td>
<td colspan="6"><p><strong>仅限 Linux</strong></p></td>
</tr>
<tr class="row-even">
<td><p>个人计算机L</p></td>
<td><p>1.12.1</p></td>
<td><p>N/A</p></td>
<td><p>1.11.1</p></td>
<td><p>1.10.0</p></td>
<td><p>N/A</p></td>
<td><p>1.11.1</p></td>
<td><p>1.10.0</p></td>
</tr>
<tr class="row-odd">
<td colspan="8"><p><strong>ROMW DDS 中件供应商</strong></p></td>
</tr>
<tr class="row-even">
<td><p>Cyclone DDS</p></td>
<td colspan="7"><p>0.9.x (帕皮隆斯语)</p></td>
</tr>
<tr class="row-odd">
<td><p>快速数据交换系统</p></td>
<td colspan="7"><p>2.6.x</p></td>
</tr>
<tr class="row-even">
<td><p>Connext DDS</p></td>
<td colspan="2"><p>6.0.1</p></td>
<td><p>N/A</p></td>
<td colspan="2"><p>6.0.1</p></td>
<td colspan="2"><p>N/A</p></td>
</tr>
<tr class="row-odd">
<td><p>Gurum 数据交换系统</p></td>
<td colspan="2"><p>2.7.x</p></td>
<td><p>N/A</p></td>
<td><p>2.7.x</p></td>
<td colspan="3"><p>N/A</p></td>
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

[安装 Humble Hawksbill( Hawmbooksbill) 工具](https://docs.ros.org/en/humble/Installation.html)

<span id="changes-in-patch-release-1-2022-11-23"></span>

## 补丁版1的修改(2022-11-23).

<span id="ros2topic"></span>

### ros2 专题

<span id="now-as-keyword-for-builtin-interfaces-msg-time-and-auto-for-std-msgs-msg-header"></span>

#### `now` 作为关键词 `builtin_interfaces.msg.Time` 财务报告和财务报告 `auto` (单位:千美元) `std_msgs.msg.Header`

`ros2 topic pub` 现在允许设置 `builtin_interfaces.msg.Time` 消息,通过 `now` 关键词。 `std_msg.msg.Header` 消息在传递关键字时会自动生成 `auto`。这一行为与ROS 1 的行为相匹配 `rostopic` (<http://wiki.ros.org/ROS/YAMLCommandLine#Headers.2Ftimestamps>)

相关 PR : [ros2/ros2cli#751](https://github.com/ros2/ros2cli/pull/751)

<span id="new-features-in-this-ros-2-release"></span>

## 本 ROS 2 发布中的新功能

<span id="ament-cmake-gen-version-h"></span>

### ament_cmake_gen_version_h

<span id="generating-a-c-c-header-with-version-info"></span>

#### 用版本信息生成 C/C++ 标题

将一个新的 CMake 函数添加到其中, 以生成一个带有软件包版本信息的标题 `ament_cmake_gen_version_h` 输入 [ament/ament_cmake#377](https://github.com/ament/ament_cmake/pull/377)以下是最简单的使用案例:

``` CMake
project(my_project)
add_library(my_lib ...)
ament_generate_version_header(my_lib)
```

它将生成一个带有来自 `package.xml` 并提供给那些与《公约》相抵触的目标。 `my_lib` 库。

如何包含标题 :

``` C
#include <my_project/version.h>
```

如果安装了信头,则:

``` cmake
set(VERSION_HEADER ${CMAKE_INSTALL_PREFIX}/include/my_project/my_project/version.h)
```

<span id="launch"></span>

### 发射

<span id="scoping-environment-variables-in-group-actions"></span>

#### 集团行动中的环境变量范围

与发射配置类似,现在默认情况下,环境变量的状态被范围化为分组动作.

例如,在以下发射文件中,已执行的进程将响应该值 `1` (Humble之前 它会回响 `2`):

##### XML 数据

``` xml
<launch>
  <set_env name="FOO" value="1" />
  <group>
    <set_env name="FOO" value="2" />
  </group>
  <executable cmd="echo $FOO" output="screen" shell="true" />
</launch>
```

##### Python

``` python
import launch
import launch.actions

def generate_launch_description():
    return launch.LaunchDescription([
        launch.actions.SetEnvironmentVariable(name='FOO', value='1'),
        launch.actions.GroupAction([
            launch.actions.SetEnvironmentVariable(name='FOO', value='2'),
        ]),
        launch.actions.ExecuteProcess(cmd=['echo', '$FOO'], output='screen', shell=True),
    ])
```

如果您想要禁用发射配置和环境变量的范围, 您可以设置 `scoped` 参数(或属性)为假。

相关 PR : [ros2/launch#601](https://github.com/ros2/launch/pull/601)

<span id="launch-pytest"></span>

#### launch_pytest

我们增加了一个新的计划, `launch_pytest`,作为替代 `launch_testing`. `launch_pytest` 是一个简单的 pytest 插件,它提供 pytest 固定装置来管理发射服务的寿命.

检查一下 [用于细节和示例的 README 软件包。](https://github.com/ros2/launch/tree/humble/launch_pytest)

相关 PR : [ros2/launch#528](https://github.com/ros2/launch/pull/528)

<span id="allow-matching-target-actions-with-a-callable"></span>

#### 允许将目标动作与可调用动作匹配

事件处理器如果将目标动作对象进行匹配,现在也可以使用调用器进行匹配.

相关 PR : [ros2/launch#540](https://github.com/ros2/launch/pull/540)

<span id="access-to-math-module-when-evaluating-python-expressions"></span>

#### 在评价 Python 表达式时访问数学模块

内部 `PythonExpression` 替换(`eval`) 我们现在可以使用 Python 的数学模块中的符号。例如,

``` xml
<launch>
  <log message="$(eval 'ceil(pi)')" />
</launch>
```

相关 PR : [ros2/launch#557](https://github.com/ros2/launch/pull/557)

<span id="boolean-substitutions"></span>

#### 布尔替换

新的替换 `NotSubstitution`, `AndSubstitution`,以及 `OrSubstitution` 提供进行逻辑操作的方便方式, 例如

``` xml
<launch>
  <let name="p" value="true" />
  <let name="q" value="false" />
  <group if="$(or $(var p) $(var q))">
    <log message="The first condition is true" />
  </group>
  <group unless="$(and $(var p) $(var q))">
    <log message="The second condition is false" />
  </group>
  <group if="$(not $(var q))">
    <log message="The third condition is true" />
  </group>
</launch>
```

相关 PR : [ros2/launch#598](https://github.com/ros2/launch/pull/598)

<span id="new-actions"></span>

#### 新行动

- `AppendEnvironmentVariable` 将一个值附加到一个已有的环境变量中。

  - 相关 PR : [ros2/launch#543](https://github.com/ros2/launch/pull/543)

- `ResetLaunchConfigurations` 重置任何用于发射配置的配置 。

  - 相关 PR : [ros2/launch#515](https://github.com/ros2/launch/pull/515)

<span id="launch-ros"></span>

### launch_ros

<span id="passing-ros-arguments-to-node-actions"></span>

#### 将 ROS 参数传递到节点动作

现在可以提供: [ROS 特定节点参数](../How-To-Guides/Node-arguments.md) 直接使用,无需使用 `args` 带线索的 `--ros-args` 旗帜 :

##### XML 数据

``` xml
<launch>
  <node pkg="demo_nodes_cpp" exec="talker" ros_args="--log-level debug" />
</launch>
```

##### 也门

``` yaml
launch:
- node:
    pkg: demo_nodes_cpp
    exec: talker
    ros_args: '--log-level debug'
```

对应参数 `Node` Python 发射文件中的动作是 `ros_arguments`:

``` python
from launch import LaunchDescription
import launch_ros.actions

def generate_launch_description():
    return LaunchDescription([
        launch_ros.actions.Node(
            package='demo_nodes_cpp',
            executable='talker',
            ros_arguments=['--log-level', 'debug'],
        ),
    ])
```

相关公关: [ros2/launch_ros#249](https://github.com/ros2/launch_ros/pull/249) 财务报告和财务报告 [ros2/launch_ros#253](https://github.com/ros2/launch_ros/pull/253).

<span id="frontend-support-for-composable-nodes"></span>

#### 可混凝土节点的前端支持

我们现在可以启动节点容器,并从前端发射文件装入组件,例如:

##### XML 数据

``` xml
<launch>
  <node_container pkg="rclcpp_components" exec="component_container" name="my_container" namespace="">
    <composable_node pkg="composition" plugin="composition::Talker" name="talker" />
  </node_container>
  <load_composable_node target="my_container">
    <composable_node pkg="composition" plugin="composition::Listener" name="listener" />
  </load_composable_node>
</launch>
```

##### 也门

``` yaml
launch:
  - node_container:
      pkg: rclcpp_components
      exec: component_container
      name: my_container
      namespace: ''
      composable_node:
        - pkg: composition
          plugin: composition::Talker
          name: talker
  - load_composable_node:
      target: my_container
      composable_node:
        - pkg: composition
          plugin: composition::Listener
          name: listener
```

相关 PR : [ros2/launch_ros#235](https://github.com/ros2/launch_ros/pull/235)

<span id="parameter-substitution"></span>

#### 参数替换

新的计划 `ParameterSubstitution` 让您替换先前在发射时设定的参数的值 。 `SetParameter` 例如,

``` xml
<launch>
  <set_parameter name="foo" value="bar" />
  <log message="Parameter foo has value $(param foo)" />
</launch>
```

相关 PR : [ros2/launch_ros#297](https://github.com/ros2/launch_ros/pull/297)

<span id="id1"></span>

#### 新行动

- `RosTimer` 就像发射一样 `TimerAction`,但使用ROS时钟(因此可以使用模拟时间,例如).

  - 相关公关: [ros2/launch_ros#244](https://github.com/ros2/launch_ros/pull/244) 财务报告和财务报告 [ros2/launch_ros#264](https://github.com/ros2/launch_ros/pull/264)

- `SetParametersFromFile` 将一个ROS参数文件传递给发射文件中的所有节点(包括节点组件).

  - 相关公关: [ros2/launch_ros#260](https://github.com/ros2/launch_ros/pull/260) 财务报告和财务报告 [ros2/launch_ros#281](https://github.com/ros2/launch_ros/pull/281)

<span id="sros2-security-enclaves-support-certificate-revocation-lists"></span>

### SROS2 安全飞地支持证书撤销列表

证书撤销列表( CRLs) 是一个在证书到期前可以撤销证书的概念。 至于 Humble , 现在可以将 CRL 放到 SROS2 安全飞地, 并获得荣誉 。 [SROS2 教程](https://github.com/ros2/sros2/blob/humble/SROS2_Linux.md#certificate-revocation-lists) 以作为如何使用它的例子。

<span id="content-filtered-topics"></span>

### 内容过滤主题

内容过滤主题支持更复杂的订阅,表明订阅者并不想要看到主题下发布的每个实例的所有值. 内容过滤主题可以在基础的RAMW执行支持此功能时用于请求内容订阅.

<span id="id27"></span>

|                |            |
|----------------|------------|
| rmw_fastrtps   | 已支持     |
| rmw_connextdds | 已支持     |
| rmw_cyclonedds | 不支持( N) |

RTW 内容过滤主题支持 {.docutils .align-default}

要学得更多,看 [content_filtering](https://github.com/ros2/examples/blob/humble/rclcpp/topics/minimal_subscriber/content_filtering.cpp) 实例。

相关设计 PR: [ros2/design#282](https://github.com/ros2/design/pull/282).

<span id="ros2cli"></span>

### 罗斯2cli

<span id="ros2-launch-has-a-launch-prefix-argument"></span>

#### `ros2 launch` 拥有 `--launch-prefix` 参数

这样可以将一个前缀传递给发射文件中的所有可执行文件,在许多调试情况下是有用的。请参见关联 [拉动请求](https://github.com/ros2/launch_ros/pull/254),以及 [教程](../How-To-Guides/Migrating-from-ROS1/Migrating-Launch-Files.md#launch-prefix-example) 以获取更多信息。

与此相关的是, `--launch-prefix-filter` 命令行选项被添加到有选择地添加从 `--launch-prefix` 可执行文件。见 [拉动请求](https://github.com/ros2/launch_ros/pull/261) 以获取更多信息。

<span id="ros2-topic-echo-has-a-flow-style-argument"></span>

#### `ros2 topic echo` 拥有 `--flow-style` 参数

允许用户强制 `flow style` 。没有此选项,则输出来自 `ros2 topic echo /tf_static` 可能看起来像:

``` default
transforms:
- header:
    stamp:
      sec: 1651172841
      nanosec: 433705575
    frame_id: single_rrbot_link3
  child_frame_id: single_rrbot_camera_link
  transform:
    translation:
      x: 0.05
      y: 0.0
      z: 0.9
    rotation:
      x: 0.0
      y: 0.0
      z: 0.0
      w: 1.0
```

有了这个选项,输出会看起来像:

``` default
transforms: [{header: {stamp: {sec: 1651172841, nanosec: 433705575}, frame_id: single_rrbot_link3}, child_frame_id: single_rrbot_camera_link, transform: {translation: {x: 0.05, y: 0.0, z: 0.9}, rotation: {x: 0.0, y: 0.0, z: 0.0, w: 1.0}}}]
```

见 [PYAML 文档](https://pyyaml.docsforge.com/master/documentation/#dictionaries-without-nested-collections-are-not-dumped-correctly) 以获取更多信息。

<span id="ros2-topic-echo-can-filter-data-based-on-message-contents"></span>

#### `ros2 topic echo` 能够根据信件内容过滤数据

这只允许用户打印符合 Python 表达式的话题数据。 例如, 使用以下参数只会打印以“ foo” 开头的字符串消息 :

``` default
ros2 topic echo --filter 'm.data.startswith("foo")` /chatter
```

见 [拉动请求](https://github.com/ros2/ros2cli/pull/654) 以获取更多信息。

<span id="rviz2"></span>

### rviz2 (中文(简体) ).

<span id="apply-textures-to-arbitrary-triangle-lists"></span>

#### 对任意三角形列表应用纹理

我们补充道: [通过 URI 定义的纹理对使用 UV 坐标的任意三角形列表应用的能力](https://github.com/ros2/rviz/pull/719)。现在我们可以从纹理图中创建梯度拉动,而不是默认的灰度。这样可以使标记的颜色变得复杂。要使用它,请使用 `visualization_msgs/Marker.msg` 并填写 `texture_resource`, `texture`, `uv_coordinates` 财务报告和财务报告 `mesh_file` 字段。您可以找到更多信息 [这儿](https://github.com/ros2/common_interfaces/pull/153).

![](images/triangle_marker_with_gradient.png) <span id="visualization-of-mass-properties-including-inertia"></span>

#### 质量特性的可视化(包括惯性)

我们还加入了可视化惯性的能力。为此,您在机器人模型下的“ 质量属性” 中选择“ Inertia ” :

![](images/rviz_mass_inertia.png)

可以看到下面的惯性图象.

![](images/tb4_inertia.png) <span id="visualize-yuv-images-in-rviz"></span>

#### 在 RViz 中可视化 YUV 图像

现在可以直接在 RViz 内对 YUV 图像进行可视化, 而不是先转换为 RGB 。 见 。 [ros2/rviz#701](https://github.com/ros2/rviz/pull/701) 详细情况。

<span id="allow-rendering-of-objects-100-meters"></span>

#### 允许渲染 \> 100米的物体

默认情况下, RViz 只能使距离相机100米以内的对象生效 。 在 rviz 相机插件中, 一个新的配置属性叫做“ Far Plane district ” , 允许配置渲染距离 。

![](images/rviz2-far-plane-distance.png)

见 [ros2/rviz#849](https://github.com/ros2/rviz/pull/849) 以获取更多信息。

<span id="changes-since-the-galactic-release"></span>

## 银河系发布以来的变化

<span id="c-headers-are-installed-in-a-subdirectory"></span>

### C++ 信头安装在子目录中

在Humble之前的ROS 2发布中,所有软件包的C++头被安装到单一的包含目录中。例如,在银河系中,目录结构看起来是这样的(为了简洁性而减少):

``` default
/opt/ros/galactic/include/
├── rcl
│   ├── node.h
├── rclcpp
│   ├── node.hpp
```

此结构在尝试使用覆盖时会产生严重的问题。 也就是说, 由于包含目录顺序, 很容易得到错误的一组标题文件 。 See <https://colcon.readthedocs.io/en/released/user/overriding-packages.html> 详细解释问题。

为了帮助对付这种情况,在Humble(以及所有ROS 2 发布中),目录结构已经改变:

``` default
/opt/ros/humble/include
├── rcl
│   └── rcl
│       ├── node.h
├── rclcpp
│   └── rclcpp
│       ├── node.hpp
```

请注意,使用这些信头的下游软件包确实 *没有* 必须更改; 使用 `#include <rclcpp/node.hpp>` 但是,在使用正在寻找的包含目录的IDE时,可能有必要将个人包含目录添加到搜索路径中。

见 <https://github.com/ros2/ros2/issues/1150> 需要更多信息,包括这一变化背后的理由。

<span id="common-interfaces"></span>

### common_interfaces

<span id="support-textures-and-embedded-meshes-for-marker-messages"></span>

#### 支持标记信件的纹理和嵌入式元件

这两项添加将提高以新方式将数据与标准消息进行可视化的能力,并同时使以Rosbag跟踪这些数据的能力得以实现.

**纹理** 将三个新字段添加到标记中:

``` bash
# Texture resource is a special URI that can either reference a texture file in
# a format acceptable to (resource retriever)[https://index.ros.org/p/resource_retriever/]
# or an embedded texture via a string matching the format:
#   "embedded://texture_name"
string texture_resource
# An image to be loaded into the rendering engine as the texture for this marker.
# This will be used iff texture_resource is set to embedded.
sensor_msgs/CompressedImage texture
# Location of each vertex within the texture; in the range: [0.0-1.0]
UVCoordinate[] uv_coordinates
```

RViz将通过嵌入式支持纹理渲染.

对那些熟悉的人来说 `mesh_resource`, `resource_retriever` 。这可以让程序员从本地文件或网络文件中选择要装入数据的地方。为了能够将所有数据记录在一个Rosbag中,包含嵌入纹理图像的能力。

**梅谢斯** 被以类似方式修改,以添加为录制目的嵌入原始 Mesh 文件的能力,并以类似方式修改。 Meshfile 消息有两个字段:

``` bash
# The filename is used for both debug purposes and to provide a file extension
# for whatever parser is used.
string filename

# This stores the raw text of the mesh file.
uint8[] data
```

内置 `Meshfile` 执行中尚不支持消息。

相关公关: [ros2/common_interfaces#153](https://github.com/ros2/common_interfaces/pull/153) [ros2/rviz#719](https://github.com/ros2/rviz/pull/719)

<span id="added-prism-type-to-solidprimitive"></span>

#### 已添加 `PRISM` 类型到 SolidPrimitive

那个... `SolidPrimitive` 消息有一个新的 `PRISM` 键,并附带适当的元数据。见 [ros2/common_interfaces#167](https://github.com/ros2/common_interfaces/pull/167) 以获取更多信息。

<span id="rmw"></span>

### rmw (英语).

<span id="struct-type-name-suffix-changed-from-t-to-s"></span>

#### `struct` 类型名称后缀更改自 `_t` 改为: `_s`

避免名称重复类型错误 `struct` 类型名称及其 `typedef`- 生成代码文档时的别名, 所有文件的后缀 `struct` 类型名称已经从 `_t` 改为: `_s`异名为 `_t` 后缀仍然保留。因此,这一修改只是对使用完整编码的破解更改。 `struct` 类型描述符,即: `struct type_name_t`.

见 [ros2/rmw#313](https://github.com/ros2/rmw/pull/313) 更多细节。

<span id="rmw-connextdds"></span>

### rmw_connextdds

<span id="use-connext-6-by-default"></span>

#### 默认使用 Connext 6

默认情况下,Humble Hawksbill 使用 Connext 6.0.1 作为 DDS 的实现 `rmw_connextdds`。仍然可以使用 Connext 5.3.1 与 `rmw_connextdds`,但它必须从源头重建。

<span id="rcl"></span>

### rcl (中文(简体) ).

<span id="id2"></span>

#### `struct` 类型名称后缀更改自 `_t` 改为: `_s`

避免名称重复类型错误 `struct` 类型名称及其 `typedef`- 生成代码文档时的别名, 所有文件的后缀 `struct` 类型名称已经从 `_t` 改为: `_s`异名为 `_t` 后缀仍然保留。因此,这一修改只是对使用完整编码的破解更改。 `struct` 类型描述符,即: `struct type_name_t`.

见 [ros2/rcl#932](https://github.com/ros2/rcl/pull/932) 更多细节。

<span id="ros-disable-loaned-messages-environment-variable-added"></span>

#### ROS_DISBEL_LOANED_MESAGES 环境变量已添加

此环境变量可用于禁用借出的信息支持, 如果 rmw 支持与否, 则独立禁用。 更多详情请参见指南 [配置零拷贝借用消息](../How-To-Guides/Configure-ZeroCopy-loaned-messages.md).

<span id="rclcpp"></span>

### rclcpp

<span id="support-type-adaption-for-publishers-and-subscriptions"></span>

#### 支持出版商和订阅商的适应类型

在定义类型适配器后,自定义的数据结构可以被出版商和订阅者直接使用,这有助于避免程序员的额外工作和潜在的错误来源。这在与复杂的数据类型合作时特别有用,比如在转换OpenCV时。 `cv::Mat` 给俄罗斯联邦的 `sensor_msgs/msg/Image` 类型。

以下是转换的适配器类型的例子 `std_msgs::msg::String` 改为: `std::string`:

``` cpp
template<>
struct rclcpp::TypeAdapter<
   std::string,
   std_msgs::msg::String
>
{
  using is_specialized = std::true_type;
  using custom_type = std::string;
  using ros_message_type = std_msgs::msg::String;

  static
  void
  convert_to_ros_message(
    const custom_type & source,
    ros_message_type & destination)
  {
    destination.data = source;
  }

  static
  void
  convert_to_custom(
    const ros_message_type & source,
    custom_type & destination)
  {
    destination = source.data;
  }
};
```

以及如何使用类型适配器的例子:

``` cpp
using MyAdaptedType = TypeAdapter<std::string, std_msgs::msg::String>;

// Publish a std::string
auto pub = node->create_publisher<MyAdaptedType>(...);
std::string custom_msg = "My std::string"
pub->publish(custom_msg);

// Pass a std::string to a subscription's callback
auto sub = node->create_subscription<MyAdaptedType>(
  "topic",
  10,
  [](const std::string & msg) {...});
```

要学得更多,看 [出版社](https://github.com/ros2/examples/blob/b83b18598b198b4a5ba44f9266c1bb39a393fa17/rclcpp/topics/minimal_publisher/member_function_with_type_adapter.cpp) 财务报告和财务报告 [订阅](https://github.com/ros2/examples/blob/b83b18598b198b4a5ba44f9266c1bb39a393fa17/rclcpp/topics/minimal_subscriber/member_function_with_type_adapter.cpp) 实例,以及更为复杂的 [演示](https://github.com/ros2/demos/pull/482)。详细情况见 [REP 2007 (中文(简体) ).](https://reps.openrobotics.org/rep-2007/).

<span id="client-asnyc-send-request-request-returns-a-std-future-instead-of-a-std-shared-future"></span>

#### `Client::asnyc_send_request(request)` 返回 a `std::future` 而不是一个 `std::shared_future`

这一变动是在下列年份实施的: [rclcpp# 1734 (英语).](https://github.com/ros2/rclcpp/pull/1734)中断 API 。 `std::future::get()` 方法从未来中提取值。这意味着,如果这种方法第二次被调用的话,它就会丢弃一个例外。这不会发生。 `std::shared_future`,作为 `get()` 方法返回 a `const &`示例:

``` cpp
auto future = client->async_send_request(req);
...
do_something_with_response(future.get());
...
do_something_else_with_response(future.get());  // this will throw an exception now!!
```

应更新为:

``` cpp
auto future = client->async_send_request(req);
...
auto response = future.get();
do_something_with_response(response);
...
do_something_else_with_response(response);
```

如果需要共同的未来, `std::future::share()` 可使用方法。

<span id="wait-for-all-acked-method-added-to-publisher"></span>

#### `wait_for_all_acked` 方法添加到 `Publisher`

这种新方法将屏蔽到出版商队列中的所有消息都因匹配的订阅或指定的超时而中断。它只对可靠的出版商有用,如最佳努力QoS没有触发。例如:

``` cpp
auto pub = node->create_publisher<std_msgs::msg::String>(...);
...
pub->publish(my_msg);
...
pub->wait_for_all_acked(); // or pub->wait_for_all_acked(timeout)
```

比较完整的例子是: [这儿](https://github.com/ros2/examples/blob/humble/rclcpp/topics/minimal_publisher/member_function_with_wait_for_all_acked.cpp).

<span id="get-callback-groups-method-removed-from-nodebase-and-node-classes"></span>

#### `get_callback_groups` 方法从 `NodeBase` 财务报告和财务报告 `Node` 类

`for_each_callback_group()` 方法已经替换 `get_callback_groups()` 通过提供一条线性安全途径来访问 `callback_groups_` 向量 。 `for_each_callback_group()` 接受一个函数作为参数,在存储的调用组上延后,并将传递的函数调用到有效的函数上。

详情请参见此。 [拉动请求](https://github.com/ros2/rclcpp/pull/1723).

<span id="add-to-wait-set-method-from-waitable-class-changes-its-return-type-from-bool-to-void"></span>

#### `add_to_wait_set` 方法从 `Waitable` 类更改返回类型从 `bool` 改为: `void`

之前,分类来源于 `Waitable` 压倒性 `add_to_wait_set` 当未向等待集添加元素时返回错误, 因此调用者必须检查返回值并丢弃或处理错误。 现在应该直接打开此错误处理程序 。 `add_to_wait_set` 如果没有出错,则不需要返回任何东西。因此,这是对下游用途的突破性改变。 `Waitable`.

见 [ros2/rclcpp#1612](https://github.com/ros2/rclcpp/pull/1612) 更多细节。

<span id="get-notify-guard-condition-method-return-type-from-nodebaseinterface-class-changed"></span>

#### `get_notify_guard_condition` 方法返回类型 `NodeBaseInterface` 类别已更改

现在 `rclcpp` 使用该 `GuardCondition` 类包装 `rcl_guard_condition_t`,这样 `get_notify_guard_condition` 返回节点的引用 `rclcpp::GuardCondition`因此,这是下游用途的突破性变化。 `NodeBaseInterface` 财务报告和财务报告 `NodeBase`.

见 [ros2/rclcpp#1612](https://github.com/ros2/rclcpp/pull/1612) 更多细节。

<span id="sleep-until-and-sleep-for-methods-added-to-clock"></span>

#### `sleep_until` 财务报告和财务报告 `sleep_for` 方法添加到 `Clock`

增加了两种新方法,允许睡在某个钟点上 [ros2/rclcpp#1814](https://github.com/ros2/rclcpp/pull/1814) 财务报告和财务报告 [ros2/rclcpp#1828](https://github.com/ros2/rclcpp/pull/1828). `Clock::sleep_until` 将暂停当前线索,直到时钟到达特定时间。 `Clock::sleep_for` 将暂停当前线程,直到时钟从调用该方法开始推进一定的时间。如果 `Context` 关闭。

<span id="rclcpp-lifecycle"></span>

### rclcpp_lifecycle

<span id="active-and-deactivate-transitions-of-publishers-will-be-triggered-automatically"></span>

#### 将自动启动出版商的主动和停用过渡

之前,用户需要覆盖 `LifecylceNode::on_activate()` 财务报告和财务报告 `LifecylceNode::on_deactivate()` 并调用类似命名方法 `LifecyclePublisher` 现在, `LifecylceNode` 提供了已这样做的这些方法的默认接口。参见执行 `lifecycle_talker` 节点 [这儿](https://github.com/ros2/demos/tree/humble/lifecycle).

<span id="rclpy"></span>

### rclpy

<span id="managed-nodes"></span>

#### 管理节点

寿命周期节点支持已被添加到 rclpy 中。 可以找到完整的演示 [这儿](https://github.com/ros2/demos/tree/humble/lifecycle_py).

<span id="id3"></span>

#### `wait_for_all_acked` 方法添加到 `Publisher`

类似于添加到rclcpp的特性.

<span id="id4"></span>

#### `sleep_until` 财务报告和财务报告 `sleep_for` 方法添加到 `Clock`

增加了两种新方法,允许睡在某个钟点上 [ros2/rclpy#858](https://github.com/ros2/rclpy/pull/858) 财务报告和财务报告 [ros2/rclpy#864](https://github.com/ros2/rclpy/pull/864). `sleep_until` 将暂停当前线索,直到时钟到达特定时间。 `sleep_for` 将暂停当前线程,直到时钟从调用该方法开始推进一定的时间。如果 `Context` 关闭。

<span id="ros1-bridge"></span>

### ros1_bridge

由于Ubuntu Jammy和前方没有正式的 ROS 1 发行量, `ros1_bridge` 现在与 ROS 1 的 Ubuntu 包装版本兼容。 `ros1_bridge` 带有 Jammy 软件包的软件包 [如何向导](../How-To-Guides/Using-ros1_bridge-Jammy-upstream.md).

<span id="id5"></span>

### 罗斯2cli

<span id="ros2-commands-disable-output-buffering-by-default"></span>

#### `ros2` 命令默认禁用输出缓冲

发布前, 运行一个命令, 如

``` default
ros2 echo /chatter | grep "Hello"
```

在输出缓冲器满载之前不会打印任何数据。 用户可以通过设置来围绕此程序工作 。 `PYTHONUNBUFFERED=1`,但这不是非常友好的用户。

相反,所有 `ros2` 命令现在默认进行行缓冲,所以一旦打印出新行,就立即执行上述命令。要禁用此行为并使用默认的蟒蛇缓冲规则,请使用此选项 `--use-python-default-buffering`。参见 [原刊](https://github.com/ros2/ros2cli/issues/595) 页:1 [拉动请求](https://github.com/ros2/ros2cli/pull/659) 以获取更多信息。

<span id="ros2-topic-pub-will-wait-for-one-matching-subscription-when-using-times-once-1"></span>

#### `ros2 topic pub` 使用时将等待一个匹配的订阅 `--times/--once/-1`

使用时 `--times/--once/-1` 旗帜, `ros2 topic pub` 将等待找到一个匹配的订阅后才能开始发布。这避免了 ros2cli 节点在发现匹配的订阅前开始发布,从而导致一些首批信件丢失。使用可靠的 qos 配置文件时特别出人意料。

在开始出版前等待的匹配订阅数量可以配置为: `-w/--wait-matching-subscriptions` 旗帜,例如:

``` console
$ ros2 topic pub -1 -w 3 /chatter std_msgs/msg/String "{data: 'foo'}"
```

以等待三个匹配的订阅,然后开始发布。

`-w` 也可以独立地使用 `--times/--once/-1` 但是,它只是默认一个 当与它们结合, 否则 `-w` 默认为零。

见 <https://github.com/ros2/ros2cli/pull/642> 更多细节。

<span id="ros2-param-dump-default-output-changed"></span>

#### `ros2 param dump` 默认输出已更改

> - `--print` 丢弃命令选项 [已贬值](https://github.com/ros2/ros2cli/pull/638).
>
>   它默认会打印为 stdout :
>
>   ``` console
>   $ ros2 param dump /my_node_name
>   ```
>
> - `--output-dir` 丢弃命令选项 [已贬值](https://github.com/ros2/ros2cli/pull/638).
>
>   要将参数丢弃到文件, 运行 :
>
>   ``` console
>   $ ros2 param dump /my_node_name > my_node_name.yaml
>   ```

<span id="ros2-param-set-now-accepts-more-yaml-syntax"></span>

#### `ros2 param set` 现在接受更多的 YAML 语法

此前,试图将像“off”这样的字符串设定为字符串类型的参数并不成功。 这是因为 `ros2 param set` 将命令行参数解释为 YAML, YAML 认为“ 关闭” 是一个布尔类型 。 <https://github.com/ros2/ros2cli/pull/684> , `ros2 param set` 现在接受 YAML 的“ ! string off” 的逃生序列,以确保该值被视为字符串。

<span id="ros2-pkg-create-can-automatically-generate-a-license-file"></span>

#### `ros2 pkg create` 能够自动生成 LICENSE 文件

如果说 `--license` 旗子传递到 `ros2 pkg create`执照是已知的执照之一, `ros2 pkg create` 现在将自动生成软件包根中的 LICENSE 文件。对于已知许可证列表,请运行 `ros2 pkg create --license ? <package_name>`。参见关联 [拉动请求](https://github.com/ros2/ros2cli/pull/650) 以获取更多信息。

<span id="robot-state-publisher"></span>

### robot_state_publisher

<span id="added-frame-prefix-parameter"></span>

#### 已添加 `frame_prefix` 参数

新参数 `frame_prefix` 已添加到 [ros/robot_state_publisher#159](https://github.com/ros/robot_state_publisher/pull/159)。这个参数是一个字符串,它准备用于所有框架名称,由 `robot_state_publisher`类似 `tf_prefix` 原文中 `tf` ROS 1中的库,此参数可用于发布相同的机器人描述,多次使用不同的帧名称.

<span id="removal-of-deprecated-use-tf-static-parameter"></span>

#### 取消折旧 `use_tf_static` 参数

堕落者 `use_tf_static` 参数已被删除 `robot_state_publisher`这意味着静态变换无条件发布给 `/tf_static` 主题,静态变换在 `transient_local` 服务质量。这是默认行为,也是 `tf2_ros::TransformListener` 类,所以大多数代码不需要更改。任何依赖的代码 `robot_state_publisher` 定期发布静态变换为 `/tf` 需要更新才能订阅 `/tf_static` 作为 `transient_local` 而不是订阅。

<span id="rosidl-cmake"></span>

### rosidl_cmake

<span id="deprecation-of-rosidl-target-interfaces"></span>

#### 折旧 `rosidl_target_interfaces()`

CMake 函数 `rosidl_target_interfaces()` 已贬值,现在调用时会发出 CMake 警告。想要在生成信件/服务/动作的ROS 软件包中使用信件/服务/动作的用户应该调用 `rosidl_get_typesupport_target()` 并随后 `target_link_libraries()` 使其目标取决于返回的类型支持目标。 <https://github.com/ros2/rosidl/pull/606> 更多细节,以及 <https://github.com/ros2/demos/pull/529> 用于使用新函数的示例。

<span id="id7"></span>

### rviz2 (中文(简体) ).

- [提高了3字节像素格式的效率](https://github.com/ros2/rviz/pull/743)

- [改变惯性用点火数学而不是Ogre数学库的计算方式](https://github.com/ros2/rviz/pull/751).

<span id="geometry2"></span>

### 几何学2

<span id="deprecation-of-tf2error-no-error-etc"></span>

#### TF2 错误的折旧: NO\_ 错误等

那个... `tf2` 库使用一个名为 `TF2Error` 返回错误。 不幸的是, 里面的一位统计员被称为 `NO_ERROR`,它与Windows上的宏相冲突。为了补救这一点,在Windows上设置了一组新的统计器。 `TF2Error` 被创建,每个都有一个 `TF2` 前缀。前面的列表仍然可用,但现在已贬值,如果使用,将打印一个折旧警告。所有使用该代码的代码 `TF2Error` 应更新用户名以使用新的 `TF2` 前缀错误。见 <https://github.com/ros2/geometry2/pull/349> 更多细节。

<span id="more-intuitive-command-line-arguments-for-static-transform-publisher"></span>

#### 静态的更直观的命令行参数\_ transform_publisher

那个... `static_transform_publisher` 程序用来进行参数如: `ros2 run tf2_ros static_transform_publisher 0 0 0 0 0 0 1 foo bar`。前三个数字是翻译x、y和z,后四个数字是四角x、y、z和w,最后两个论点是父母和子女框架的身份证。虽然这样做有效,但有一些问题:

- 用户必须指定 *全部( E)* 参数,即使只设置一个数字

- 读到命令行 找出它是什么 出版是困难的

为了解决这两个问题,命令线处理方式已经改为使用旗帜,所有旗帜除 `--frame-id` 财务报告和财务报告 `--child-frame-id` 因此,上述命令行可以简化为: `ros2 run tf2_ros static_transform_publisher --frame-id foo --child-frame-id bar` 只需更改翻译x,命令行为: `ros2 run tf2_ros static_transform_publisher --x 1.5 --frame-id foo --child-frame-id bar`.

旧式的论证在此发行中仍然被允许,但被贬低,会打印一个警告,在未来发行中会被删除. See. <https://github.com/ros2/geometry2/pull/392> 更多细节。

<span id="transform-listener-spin-thread-no-longer-executes-node-callbacks"></span>

#### 变形听器旋转线程不再执行节点调回

`tf2_ros::TransformListener` 不再在提供的节点对象上旋转。 相反, 它创建一个调用组, 以在它内部创建的实体上执行调用。 这意味着如果设置了参数 。 `spin_thread=true` 在创建变换听器时, 您无法再依赖自己的调用来被执行 。 您必须调用一个 `spin` 函数在节点上(例如) `rclcpp::spin`),或将节点添加到自己的执行器中.

相关拉动请求 : [几何2# 442](https://github.com/ros2/geometry2/pull/442)

<span id="rosbag2"></span>

### rosbag2

<span id="new-playback-and-recording-controls"></span>

#### 新建播放和录制控制

添加了若干拉动请求,以加强用户对弹出袋的控制。拉动请求 [931](https://github.com/ros2/rosbag2/pull/931) 添加指定从何时开始播放的时间戳的能力 。 [789](https://github.com/ros2/rosbag2/pull/789) 现在可以将重播的开始时间延迟到指定的间隔。

与此相关的是, `rosbag2` 已经为用户获取了新的控制播放方式。 [847](https://github.com/ros2/rosbag2/pull/847) 添加键盘控件, 用于在从终端重播时缓存、 恢复和播放下一条消息 。 由于拉请求, 也可以开始暂停播放 。 [905](https://github.com/ros2/rosbag2/pull/905) 财务报告和财务报告 [904](https://github.com/ros2/rosbag2/pull/904),这使得用户很容易启动回放然后步入消息中,比如在调试管道时。Pull request [836](https://github.com/ros2/rosbag2/pull/836) 添加一个在袋内搜索的界面,允许用户在回放时在袋内移动.

最后,在拉动请求中录制时添加了新的快照模式 [851](https://github.com/ros2/rosbag2/pull/851)。这个模式对事件记录有用,它允许记录开始填充缓冲器,但直到调用服务时才开始将数据写入磁盘。

<span id="burst-mode-playback"></span>

#### 播放时装模式

虽然实时播放袋中的数据是袋中文件最广为人知的使用例,但有些情况下您希望袋中的数据尽可能快。有拉请求 [977](https://github.com/ros2/rosbag2/pull/977), `rosbag2` 已获得从包中“爆发”数据的能力。在爆破模式下,数据将尽可能快地放回。这对机器学习等应用程序很有用。

<span id="zero-copy-playback"></span>

#### 零平反弹

默认情况下, 如果可以使用出借信件, 播放信件会作为出借信件发布。 这有助于减少数据副本的数量, 因此, 发送大数据的好处更大 。 Pull request [981](https://github.com/ros2/rosbag2/pull/981) 添加 `--disable-loan-message` 选项。

<span id="wait-for-an-acknowledgment"></span>

#### 等待承认

此新选项将等待所有订阅者确认所有已发布的消息, 或等待游戏结束前超时数在毫秒内结束。 特别是短时间发送大尺寸的消息。 此选项只有在发布者的 QOS 配置文件为 RELIABLE 时才有效 。 Pull request [951](https://github.com/ros2/rosbag2/pull/951) 添加 `--wait-for-all-acked` 选项。

<span id="bag-editing"></span>

#### 包编辑

`rosbag2` 正在采取步骤,以便编辑袋,例如删除一个主题的所有信件或将多个袋合并到一个袋中。 [921](https://github.com/ros2/rosbag2/pull/921) 添加包重写和 `ros2 bag convert` 动词.

<span id="other-changes"></span>

#### 其他变动

调用请求 [925](https://github.com/ros2/rosbag2/pull/925) 生产 `rosbag2` 在录制时忽略“叶片主题”(没有出版商的专题),这些主题将不再自动添加到包中。

<span id="known-issues"></span>

## 已知问题

- 何时 [在Ubuntu 22.04 Jammy主机上安装ROS 2](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html) 在安装 ROS 2 软件包之前,必须更新您的系统。 *特别是,* 必须确保 `systemd` 财务报告和财务报告 `udev` 更新到最新的可用版本,否则将安装 `ros-humble-desktop`,这取决于 `libudev1`,可导致删除系统关键软件包。详情可见于 [ros2/ros2#1272](https://github.com/ros2/ros2/issues/1272) 财务报告和财务报告 [发射板 #174196](https://bugs.launchpad.net/ubuntu/+source/systemd/+bug/1974196)

- 当 ROS 2 apt profile 可用时, Ubuntu 中的 ROS 1 软件包无法安装 。 [Ubuntu Jammy 上的 ros1\_ 桥](../How-To-Guides/Using-ros1_bridge-Jammy-upstream.md) 文档以获取更多信息。

- 一些主要的 Linux 发行开始补丁 Python 以安装软件包 `/usr/local`,它正在打破一些部分 `ament_package` 并与之相建构 `colcon`,特别是使用Ubuntu Jammy与 `setuptools` 从 pip 安装将显示这种不当行为,因此不推荐。 [拟议解决办法](https://github.com/colcon/colcon-core/pull/512) 在广泛释放之前,还需要进一步测试。

- 按大小或持续时间拆分的ROS 2 袋没有正确播放。只播放最后一个记录的袋。建议避免按大小或持续时间拆分袋。详情可见于 [ros2/rosbag2#966](https://github.com/ros2/rosbag2/issues/966).

<span id="release-timeline"></span>

## 发布时间线

> Mon. 2022年3月21日 - Alpha + RMW 冻结  
> ROS基地的初步测试和稳定 <span id="id20"></span>[\[1\]](#id25) 软件包,以及RAMW供应商软件包的 API 和特性冻结。
>
> 2022年4月4日 - 冻结  
> ROS Base 的 API 和特性冻结 <span id="id21"></span>[\[1\]](#id25) 在 Rolling Ridley 中的软件包。在此点之后,只应该发布错误修正。新软件包可以独立发布。
>
> Mon. 2022年4月18日 - 分会  
> 罗林瑞德利的分店 `rosdistro` 重新开放用于 ROS 基地的 滚动PRs 。 <span id="id22"></span>[\[1\]](#id25) 软件包。 `ros-rolling-*` 软件包到 `ros-humble-*` 软件包。
>
> 2022年4月25日 - 贝塔  
> ROS 桌面更新版 <span id="id23"></span>[\[2\]](#id26) 可用软件包。请进行一般测试。
>
> Mon. 2022年5月16日 - 释放候选人.  
> 创建了候选软件包。 ROS 桌面的更新版 <span id="id24"></span>[\[2\]](#id26) 可用软件包。
>
> Thu. 2022年5月19日 - 冻结  
> 冻结Rodistro 没有Humble的公关 `rosdistro` Repo将合并(发布公告后重新开放).
>
> 2022年5月23日----一般可用性  
> 发布公告. `rosdistro` 重开Humble PRs。

<span id="id25"></span>

\[1\] ([1](#id20),[2](#id21),[3](#id22))

那个... `ros_base` 变体描述于 [REP 2001(跨基)](https://reps.openrobotics.org/rep-2001/#ros-base).

<span id="id26"></span>

\[2\] ([1](#id23),[2](#id24))

那个... `desktop` 变体描述于 [REP 2001(桌面变量)](https://reps.openrobotics.org/rep-2001/#desktop-variants).
