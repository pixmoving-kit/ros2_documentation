---
translation_status: machine_translated
source: Releases/Release-Foxy-Fitzroy.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="foxy-fitzroy-foxy"></span>

# Foxy Fitzroy（`foxy`)

*狐狸菲茨罗伊* 是ROS 2的第六版发布.

<span id="supported-platforms"></span>

## 支持的平台

Foxy Fitzroy 支持以下平台,根据 [平台支持级别](../The-ROS2-Project/Platform-Support-Tiers.md):

第一级平台:

- 乌本图20.04(福尔): `amd64` 财务报告和财务报告 `arm64`

- Mac macOS 10.14 (移动)

- Windows 10 (Visual Studio 2019) (英语).

第三级平台:

- 乌本图20.04(福尔): `arm32`

- 德比安·布斯特(10): `amd64`, `arm64` 财务报告和财务报告 `arm32`

- OpenEmbed Thud (2.6) / webOS OSE : (中文(简体) ). `arm32` 财务报告和财务报告 `x86`

目标平台:

| 建筑 | 乌邦图联络人(20.04) | 马科斯莫哈韦(10.14) | Windows 10 (VS2019) (英语). | 德比安·巴斯特(10) | OpenEmbed / webOS OSE 操作系统 |
|----|----|----|----|----|----|
| amd64 (中文(简体) ). | 第1级 \[d\]\[a\]\[s\] | 第1级 \[s\] | 第1级 \[s\] | 第3级 \[s\] |  |
| 军火64 | 第1级 \[d\]\[a\]\[s\] |  |  | 第3级 \[s\] | 第3级 \[s\] |
| 臂弹32 | 第3级 \[s\] |  |  | 第3级 \[s\] | 第3级 \[s\] |

以下指标显示每个平台都有何种交付机制。

" \[d\]" Debian包将为本平台提供提交rodistro的包件.

" \[a\] " 二进制版本作为每个平台的单一档案提供,包含Foxy ROS 2 repos文件中的所有软件包\[^9\].

" \[s\] " 来源汇编。

中间软件执行支持 :

| 中间软件库 | 中件提供者 | 支助级别 | 平台 | 建筑 |
|----|----|----|----|----|
| rmw_fastrtps_cpp\* | eProsima 快速RTPS | 第1级 | 所有平台 | 所有建筑 |
| rmw_cyclonedds_cpp | Eclipse Cyclone DDS | 第1级 | 所有平台 | 所有建筑 |
| rmw_connext_cpp | RTI 连接 | 第1级 | 除Debian和OpenEmbed外的所有平台 | 除arm64/arm32外的所有建筑 |
| rmw_fastrtps_dynamic_cpp | eProsima 快速RTPS | 第二级 | 所有平台 | 所有建筑 |
| rmw_gurumdds_cpp | GurumNetworks GurumDDS | 第3级 | Ubuntu 和 视窗 | 除arm32外的所有建筑 |

" \* " 是指未执行的《保护所有移徙工人及其家庭成员权利国际公约》。

Middleware 执行支持依赖于平台支持级别。例如,在 Tier 2 平台上执行的 Lier 1 中间软件只能得到 Tier 2 支持。

最低语文要求:

- C++14

- ⁇  3.7

依赖性要求:

<table class="docutils align-default">
<thead>
<tr class="row-odd">
<th class="head"></th>
<th colspan="3" class="head"><p>所需支助</p></th>
<th colspan="2" class="head"><p>建议的支助</p></th>
</tr>
<tr class="row-even">
<th class="head"><p>软件包</p></th>
<th class="head"><p>Ubuntu 焦点</p></th>
<th class="head"><p>麦克OS**</p></th>
<th class="head"><p>视窗10**</p></th>
<th class="head"><p>德比安·巴斯特(英语:Debian Buster)</p></th>
<th class="head"><p>已打开*</p></th>
</tr>
</thead>
<tbody>
<tr class="row-odd">
<td><p>CMake</p></td>
<td><p>3.16.3</p></td>
<td><p>3.14.4</p></td>
<td><p>3.14.4</p></td>
<td><p>3.13.4</p></td>
<td><p>3.16.1 / 3.12.2****</p></td>
</tr>
<tr class="row-even">
<td><p>爱咪</p></td>
<td colspan="5"><p>3.3.2</p></td>
</tr>
<tr class="row-odd">
<td><p>Gazebo</p></td>
<td><p>11.0.0*</p></td>
<td><p>11.0.0</p></td>
<td><p>N/A</p></td>
<td><p>11.0.0*</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>点火</p></td>
<td colspan="2"><p>学校*</p></td>
<td><p>N/A</p></td>
<td><p>学校*</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-odd">
<td><p>食人鱼</p></td>
<td colspan="4"><p>1.10*</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>打开CV</p></td>
<td><p>4.2.0</p></td>
<td><p>4.2.0</p></td>
<td><p>3.4.6*</p></td>
<td><p>3.2.0</p></td>
<td><p>4.1.0 / 3.2.0****</p></td>
</tr>
<tr class="row-odd">
<td><p>打开SSL</p></td>
<td><p>1.1.1d</p></td>
<td><p>1.1.1f</p></td>
<td><p>1.1.1f</p></td>
<td><p>1.1.1d</p></td>
<td><p>1.1.1d / 1.1.1b****</p></td>
</tr>
<tr class="row-even">
<td><p>宝可</p></td>
<td><p>1.9.2</p></td>
<td><p>1.9.0</p></td>
<td><p>1.8.0*</p></td>
<td><p>1.9.0</p></td>
<td><p>1.9.4</p></td>
</tr>
<tr class="row-odd">
<td><p>Python</p></td>
<td><p>3.8.0</p></td>
<td><p>3.8.2</p></td>
<td><p>3.8.0</p></td>
<td><p>3.7.3</p></td>
<td><p>3.8.2 / 3.7.5****</p></td>
</tr>
<tr class="row-even">
<td><p>Qt 键</p></td>
<td><p>5.12.5</p></td>
<td><p>5.12.3</p></td>
<td><p>5.10.0</p></td>
<td><p>5.11.3</p></td>
<td><p>5.14.1 / 5.12.5****</p></td>
</tr>
<tr class="row-odd">
<td colspan="2"></td>
<td colspan="4"><p><strong>仅限 Linux</strong></p></td>
</tr>
<tr class="row-even">
<td><p>个人计算机L</p></td>
<td><p>1.10.0</p></td>
<td><p>N/A</p></td>
<td><p>N/A</p></td>
<td><p>1.9.1</p></td>
<td><p>1.10.0</p></td>
</tr>
<tr class="row-odd">
<td colspan="6"><p><strong>ROMW DDS 中件供应商</strong></p></td>
</tr>
<tr class="row-even">
<td><p>Connext DDS</p></td>
<td colspan="3"><p>5.3.1</p></td>
<td colspan="2"><p>N/A</p></td>
</tr>
<tr class="row-odd">
<td><p>Cyclone DDS</p></td>
<td colspan="5"><p>0.7.x (科奎特语).</p></td>
</tr>
<tr class="row-even">
<td><p>快速RTPS( 快速区域贸易促进系统)</p></td>
<td colspan="5"><p>2.0.x</p></td>
</tr>
<tr class="row-odd">
<td><p>Gurum 数据交换系统</p></td>
<td><p>2.7.x</p></td>
<td><p>N/A</p></td>
<td><p>2.7.x</p></td>
<td colspan="2"><p>N/A</p></td>
</tr>
</tbody>
</table>

" \* " 是指这不是上游版本(可在官方操作系统寄存器上查阅),而是OSRF或社区分发的包(自定义寄存器上建立和分发的包)。

" \*\* " 滚动发行会看到这些依赖性在其存在期间的多个版本变化。为OpenEmberded显示的版本是3.1 Dunfell发行系列提供的版本;其他支持发行系列提供的版本在此列出: \<<https://github.com/ros/meta-ros/wiki/Package-Version-Differences>\>. 注意,根据此处显示的 OpenEmbed 支持策略,ROS distro 支持的 OpenEmbed 发布系列将在支持时间范围内改变: \<<https://github.com/ros/meta-ros/wiki/Policies#openembedded-release-series-support>\>. 然而,它将始终得到至少一个稳定的OpenEmbed发行系列的支持.

" \*\*\*\* " webOS OSE提供了这个不同的版本.

此文档只抓取 ROS 发行的首次发布时的版本, 并且不会随着依赖关系向前移动而更新, 因此这些版本是低水印 。

依赖软件包管理器的使用 :

- Ubuntu, Debian: apt (英语).

- 马科斯:土生土长,皮普

- 视窗:巧克力,pip

- 打开嵌入式: opkg

构建系统支持 :

- ament_cmake

- 制作( C)

- 设置工具

<span id="installation"></span>

## 安装

[安装 Foxy 菲茨罗伊](https://docs.ros.org/en/foxy/Installation.html)

<span id="new-features-in-this-ros-2-release"></span>

## 本 ROS 2 发布中的新功能

在开发过程中, [Foxy 元目录](https://github.com/ros2/ros2/issues/830) 在GitHub上,载有正在进行的高级别任务的最新状况,以及详细介绍的参考具体罚单。

<span id="changes-in-patch-release-8-2022-09-28"></span>

## 补丁版8的变动(2022-09-28)

<span id="launch-groupaction-scopes-environment"></span>

### 发射小组行动范围环境

那个... `SetEnvironmentVariable` 动作现在被限定为任意 `GroupAction` 它从这里返回。

例如,考虑以下发射文件:

##### Python

``` python
import launch
from launch.actions import SetEnvironmentVariable
from launch.actions import GroupAction
from launch_ros.actions import Node


def generate_launch_description():
    return launch.LaunchDescription([
        SetEnvironmentVariable(name='my_env_var', value='1'),
        Node(package='foo', executable='foo', output='screen'),
        GroupAction([
            SetEnvironmentVariable(name='my_env_var', value='2'),
        ]),
    ])
```

##### XML 数据

``` xml
<launch>
  <set_env name="my_env_var" value="1"/>
  <node pkg="foo" exec="foo" output="screen" />
  <group>
    <set_env name="my_env_var" value="2"/>
  </group>
</launch>
```

在补丁发布 8 之前, 节点 `foo` 将开始 `my_env_var=2`,但现在它会从 `my_env_var=1`.

要选择退出新行为, 您可以设置参数 `scoped=False` 编辑 `GroupAction`.

有关机票:

- [编号2# 1244](https://github.com/ros2/ros2/issues/1244)

- [第630号发射](https://github.com/ros2/launch/pull/630)

<span id="changes-in-patch-release-7-2022-02-08"></span>

## 补丁第7版的变动(2022-02-08)

<span id="launch-set-env-frontend-behavior-change"></span>

### 启动 set_env 前端行为改变

[468号发射装置](https://github.com/ros2/launch/pull/468) 无意中改变了行为范围 `set_env` 前端启动文件中的动作。使用此选项更改环境变量 `set_env` 动作不再被限定为父 `group` 动作,转而在全球应用。由于它被输入回转,变化会影响这一发布。

我们认为这个变化是一种回归,并打算在下一次补丁发布和未来的ROS发行中修正行为。我们还计划在Python发射文件中修正行为,因为Python发射文件从未正确设定环境变量。

相关问题:

- [编号2# 1244](https://github.com/ros2/ros2/issues/1244)

- [第597号发射](https://github.com/ros2/launch/issues/597)

<span id="fix-launch-frontend-parser"></span>

### 修正前端发布解析器

发射前端解析器的重构器固定了一些 [解析特殊字符的问题](https://github.com/ros2/launch_ros/issues/214)。因此,在解析字符串时,行为发生了小的改变。例如,在通过一个数字作为字符串之前,你必须添加额外的引号(如果使用替换的话,需要两组引号):

``` xml
<!-- results in the string value "'3'" -->
<param name="foo" value="''3''"/>
```

重构后, 以上将产生字符串 `"''3''"` (注意额外的一组引号)。现在,用户应该使用 `type` 属性到该值应解释为字符串的信号 :

``` xml
<param name="foo" value="3" type="str"/>
```

相关拉动请求 :

- [第530号发射](https://github.com/ros2/launch/pull/530)

- [launch_ros#265](https://github.com/ros2/launch_ros/pull/265)

<span id="fix-memory-leaks-and-undefined-behavior-in-rmw-fastrtps-dynamic-cpp"></span>

### 在rmw_fastrtps_动态_cpp中修复内存泄漏和未定义的行为

API 在以下标题文件中被更改 :

- `rmw_fastrtps_dynamic_cpp/TypeSupport.hpp`

- `rmw_fastrtps_dynamic_cpp/TypeSupport_impl.hpp`

尽管技术上它们可以公开获取,但人们不太可能直接使用它们。 因此,我们决定打破API,以便修复内存泄漏和未定义的行为。

修补最初是在 [rmw_fastrtps#429](https://github.com/ros2/rmw_fastrtps/pull/429) 之后又汇回Foxy in [rmw_fastrtps#577](https://github.com/ros2/rmw_fastrtps/pull/577).

<span id="changes-in-patch-release-2-2020-08-07"></span>

## 补丁版第2版的变动(2020-08-07)

<span id="bug-in-static-transform-publisher"></span>

### 静态中的错误\_ 变形\_ publisher

在Foxy开发过程中,一个错误被引入 tf2_ros 静态_transform_publisher 程序. Euler 角度的顺序传递到静态_transform_publisher 的实现与文档不一致. Foxy 补丁发布 2 [修补](https://github.com/ros2/geometry2/pull/296) 命令,以使执行与文档(yaw, pitch, roll)一致 。 对于已经开始使用初始 Foxy 释放或补丁释放 1 的用户,这意味着任何使用静态\_ transform_publisher 的启动文件都必须按照新命令互换命令行命令 。 对于来自 ROS 2 Dashing, ROS 2 Eloguet, 或 ROS 1 的用户,不需要修改移植到 Foxy 补丁释放 2 。

<span id="changes-since-the-eloquent-release"></span>

## 口号发布后的变化

<span id="classic-cmake-vs-modern-cmake"></span>

### 经典 CMake 对现代 CMake

在“ 经典” CMake 中, 包提供了 CMake 变量, 如 `<pkgname>_INCLUDE_DIRS` 财务报告和财务报告 `<pkgname>_LIBRARIES` 当是时 `find_package()`-是的,跟那个... `ament_cmake` 通过呼叫实现 `ament_export_include_directories` 财务报告和财务报告 `ament_export_libraries`。结合 `ament_export_dependencies`, `ament_cmake` 确保所有内容都包含递归依赖的目录和库,并纳入这些变量。

在“现代” CMake 中,一个软件包提供了接口目标(通常命名为 `<pkgname>::<pkgname>`) ,它本身就包含所有递归依赖。为了导出一个库目标来使用现代 CMake `ament_export_targets` 需要调用导出名称, 当使用此名称安装库时也会使用该名称 。 `install(TARGETS <libA> <libB> EXPORT <export_name> ...)`。导出接口目标可通过 CMake 变量获取 `<pkgname>_TARGETS`。要像这样输出库目标,它们就不得依赖影响全球状态的经典功能,例如: `include_directories()` 但将包含目录设置在目标本身上——用于构建以及安装环境——使用生成器表达式,例如: `target_include_directories(<target> PUBLIC "$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/include>" "$<INSTALL_INTERFACE:include>")`.

何时 `ament_target_dependencies` 用于为库目标添加依赖性。函数在可用时使用现代的 CMake 目标。否则它会回到使用经典的 CMake 变量。因此,如果所有依赖性也提供现代的 CMake 目标,则您只能导出现代 CMake 目标。 **否则导出界面目标将包含绝对路径, 将目录 / 库包含在生成的 CMake 逻辑中, 这使得软件包无法重排 。**

例如, 软件包是如何在 Foxy 中更新到现代 CMake 的 [ros2/ros2#904](https://github.com/ros2/ros2/issues/904).

<span id="ament-export-interfaces-replaced-by-ament-export-targets"></span>

### ament_export_interfaces 替换为 ament_export_目标

CMake 函数 `ament_export_interfaces` 从软件包中 `ament_cmake_export_interfaces` 已为偏好此函数而贬值 `ament_export_targets` 在新软件包中 `ament_cmake_export_targets`。参见 GitHub 票 [ament/ament_cmake#237](https://github.com/ament/ament_cmake/issues/237) 为了更多背景。

<span id="rosidl-generator-c-cpp-namespace-api-changes"></span>

### rosidl_generator_c ⁇ cpp 命名空间 / API 更改

套装 `rosidl_generator_c` 财务报告和财务报告 `rosidl_generator_cpp` 已重置许多信头, 源头已移入新软件包 `rosidl_runtime_c` 财务报告和财务报告 `rosidl_runtime_cpp`。目的是消除对生成器软件包的运行依赖性,因此使用 Python 的代码生成工具。在移动信头时,包含路径 / 命名空间被相应更新,所以在许多情况下,改变包括从生成器软件包到运行时软件包的指令就足够了。

生成的 C / C++ 代码也被重构 。 文件结束于 `__struct.h|hpp`, `__functions.h`, `__traits.hpp`等已移入子目录 `detail` 但大多数代码只包含以接口命名的头部,没有这些后缀.

有关字符串和序列界限的一些类型也被重新命名,以与命名惯例相匹配,但预计它们不会在用户代码中使用(高于 RMW 执行和类型支持包) 。

更多信息见 [ros2/rosidl # 446 (用于 C)](https://github.com/ros2/rosidl/issues/446) 财务报告和财务报告 [ros2/ rosidl # 447 (用于 C++)](https://github.com/ros2/rosidl/issues/447).

<span id="default-working-directory-for-ament-add-test"></span>

### ament_add\_ test 默认工作目录

用于测试的默认工作目录 `ament_add_test` 改为: `CMAKE_CURRENT_BINARY_DIR` 来匹配 CMake 的行为 `add_test`。或者更新测试以配合新的默认值,或者通过 `WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}` 以恢复先前的值。

<span id="default-console-logging-format"></span>

### 默认主控台日志格式

默认控制台日志输出格式被更改为默认包含时间戳,参见:

- <https://github.com/ros2/rcutils/pull/190>

- <https://discourse.ros.org/t/ros2-logging-format/11549>

<span id="default-console-logging-output-stream"></span>

### 默认主控台日志输出流

至 Foxy , 所有严重级的日志信息默认都会被登录到 stderr 。 这可以确保日志信息立即发出, 并使 ROS 2 日志系统与大多数其他日志系统保持一致。 可以通过 RUPATLS\_ LOGGING\_ USE\_ STDOUT 环境变量在运行时将流改为 stdout, 但所有日志信息仍然会流向同一流 。 <https://github.com/ros2/rcutils/pull/196> 更多细节。

<span id="launch-ros"></span>

### launch_ros

<span id="node-name-and-namespace-parameters-changed"></span>

#### 节点名称和命名空间参数已更改

那个... `Node` 与命名有关的动作参数已更改:

- `node_name` 已重新命名为 `name`

- `node_namespace` 已重新命名为 `namespace`

- `node_executable` 已重新命名为 `executable`

- `exec_name` 已添加用于命名与节点相关的进程。以前,用户会使用该节点。 `name` 关键词参数.

旧参数已堕.

作出这些修改是为了使发射前端更加平庸。

``` xml
<node pkg="demo_nodes_cpp" exec="talker" node-name="foo" />
```

我们现在可以写了

``` xml
<node pkg="demo_nodes_cpp" exec="talker" name="foo" />
```

这一修改也适用于: `ComposableNodeContainer`, `ComposableNode`,以及 `LifecycleNode`示例,请参见 [演示的相关更改。](https://github.com/ros2/demos/pull/431)

[发射_ros中的相关拉力请求.](https://github.com/ros2/launch_ros/pull/122)

<span id="rclcpp"></span>

### rclcpp

<span id="change-in-advanced-subscription-callback-signature"></span>

#### 高级订阅召回签名的更改

和拉动请求 <https://github.com/ros2/rclcpp/pull/1047> 接收信件信息的回调签名已经更改。 `rmw` 类型 `rmw_message_info_t`,但现在使用 `rclcpp` 类型 `rclcpp::MessageInfo`。所需的改动是直截了当的,从这些拉动请求中可以看出:

- <https://github.com/ros2/system_tests/pull/423/files>

- <https://github.com/ros2/rosbag2/pull/375/files>

- <https://github.com/ros2/ros1_bridge/pull/253/files>

<span id="change-in-serialized-message-callback-signature"></span>

#### 更改序列化信件召回签名

牵引请求 [ros2/rclcpp#1081](https://github.com/ros2/rclcpp/pull/1081) 引入新的调用签名, 用于以序列化的形式检索 ROS 信件。 先前使用的 C- Struct [rcl_serialized_message_t](https://github.com/ros2/rmw/blob/foxy/rmw/include/rmw/serialized_message.h) 正在被 C++ 数据类型取代 [rclcpp: 序列化消息](https://github.com/ros2/rclcpp/blob/foxy/rclcpp/include/rclcpp/serialized_message.hpp).

示例节点在 `demo_nodes_cpp`,即: `talker_serialized_message` (a) 与《公约》有关的其他事项; `listener_serialized_message` 反映这些变化。

<span id="breaking-change-in-node-interface-getters-signature"></span>

#### 断开节点界面获取器签名的更改

有拉动请求 [ros2/rclcpp#1069](https://github.com/ros2/rclcpp/pull/1069),修改了节点界面获取器的签名,以返回节点界面(即a)的共享所有权。 `std::shared_ptr`)而不是一个不拥有的原始指针。依赖上一个签名的下游软件包的必需更改是简单和直截了当的:使用 `std::shared_ptr::get()` 方法。

<span id="deprecate-set-on-parameters-set-callback"></span>

#### 折射设置\_ 在_参数上\_ set\_ callback

相反,使用 `rclcpp::Node` 方法( I) `add_on_set_parameters_callback` 财务报告和财务报告 `remove_on_set_parameters_callback` 用于在参数设定时添加和删除所调用的函数。

相关拉动请求 : <https://github.com/ros2/rclcpp/pull/1123>

<span id="breaking-change-in-publisher-getter-signature"></span>

#### 正在打破 publisher getter 签名的更改

有拉动请求 [ros2/rclcpp#1119](https://github.com/ros2/rclcpp/pull/1119),已修改了出版商手柄获取器的签名,以返回对基础rcl结构(即a)的共享所有权。 `std::shared_ptr`) 而不是非自有的原始指针。在某些情况下,这对修补断层是必要的。依赖上一个签名的下游软件包的必需修改是简单而直接的: `std::shared_ptr::get()` 方法。

<span id="rclcpp-action"></span>

### rclcpp_action

<span id="deprecate-clientgoalhandle-async-result"></span>

#### 折旧客户端目标Handle::async_results()

使用此 API 时, 可能会遇到导致丢弃例外的比赛条件 。 相反, 倾向于使用 `Client::async_get_result()`更安全一点

见 [ros2/rclcpp#1120](https://github.com/ros2/rclcpp/pull/1120) 和相关的问题 以获取更多信息。

<span id="rclpy"></span>

### rclpy

<span id="support-for-multiple-on-parameter-set-callbacks"></span>

#### 支持参数集回调时的多个

使用该 `Node` 方法( I) `add_on_set_parameters_callback` 财务报告和财务报告 `remove_on_set_parameters_callback` 用于在参数设定时添加和删除所调用的函数。

方法 `set_parameters_callback` 已经贬值。

相关拉动请求 : <https://github.com/ros2/rclpy/pull/457>, <https://github.com/ros2/rclpy/pull/504>

<span id="rmw-connext-cpp"></span>

### rmw_connext_cpp

<span id="connext-5-1-locator-kinds-compatibility-mode"></span>

#### 连接5.1 位址类型兼容模式

最高和包括 `Eloquent`, `rmw_connext_cpp` 正在设置 `dds.transport.use_510_compatible_locator_kinds` 属性改为 `true`。这些财产不再被强迫使用,而是相互之间共享交通通信。 `Foxy` 和以前的发布将停止工作。日志类似 :

``` bash
PRESParticipant_checkTransportInfoMatching:Warning: discovered remote participant 'RTI Administration Console' using the 'shmem' transport with class ID 16777216.
This class ID does not match the class ID 2 of the same transport in the local participant 'talker'.
These two participants will not communicate over the 'shmem' transport.
Check the value of the property 'dds.transport.use_510_compatible_locator_kinds' in the local participant.
See https://community.rti.com/kb/what-causes-error-discovered-remote-participant for additional info.
```

当出现这种不相容性时,将观察到。

如果需要兼容性,可以在包含以下内容的外部 QoS 配置文件中设置:

``` xml
<participant_qos>
   <property>
      <value>
            <element>
               <name>
                  dds.transport.use_510_compatible_locator_kinds
               </name>
               <value>1</value>
            </element>
      </value>
   </property>
</participant_qos>
```

记住,设置 `NDDS_QOS_PROFILES` QoS 配置文件路径的环境变量。关于更多信息,请参见 `How to Change Transport Settings in 5.2.0 Applications for Compatibility with 5.1.0` 节次 [Transport_Compatibility](https://community.rti.com/static/documentation/connext-dds/5.2.0/doc/manuals/connext_dds/html_files/RTI_ConnextDDS_CoreLibraries_ReleaseNotes/Content/ReleaseNotes/Transport_Compatibility.htm).

<span id="rviz"></span>

### rviz 维兹

<span id="tools-timestamp-messages-using-ros-time"></span>

#### 使用 ROS 时间的工具时间戳消息

“2D Pose Project”、“2D Nave Goal”和“Publish Point”工具现在使用ROS时间而不是系统时间为信息打上时间戳,以便 `use_sim_time` 参数可以对它们产生影响。

相关拉动请求 : <https://github.com/ros2/rviz/pull/519>

<span id="std-msgs"></span>

### std_msgs

<span id="deprecation-of-messages"></span>

#### 电文的折旧

虽然很长一段时间以来我们一直不高兴,但以下信息已经正式贬值: `std_msgs`中。有副本。 [example_interfaces](https://index.ros.org/p/example_interfaces)

- `std_msgs/msg/Bool`

- `std_msgs/msg/Byte`

- `std_msgs/msg/ByteMultiArray`

- `std_msgs/msg/Char`

- `std_msgs/msg/Float32`

- `std_msgs/msg/Float32MultiArray`

- `std_msgs/msg/Float64`

- `std_msgs/msg/Float64MultiArray`

- `std_msgs/msg/Int16`

- `std_msgs/msg/Int16MultiArray`

- `std_msgs/msg/Int32`

- `std_msgs/msg/Int32MultiArray`

- `std_msgs/msg/Int64`

- `std_msgs/msg/Int64MultiArray`

- `std_msgs/msg/Int8`

- `std_msgs/msg/Int8MultiArray`

- `std_msgs/msg/MultiArrayDimension`

- `std_msgs/msg/MultiArrayLayout`

- `std_msgs/msg/String`

- `std_msgs/msg/UInt16`

- `std_msgs/msg/UInt16MultiArray`

- `std_msgs/msg/UInt32`

- `std_msgs/msg/UInt32MultiArray`

- `std_msgs/msg/UInt64`

- `std_msgs/msg/UInt64MultiArray`

- `std_msgs/msg/UInt8`

- `std_msgs/msg/UInt8MultiArray`

<span id="security-features"></span>

### 安全性能

<span id="use-of-security-enclaves"></span>

#### 安全飞地的使用

至于Foxy,域参与者不再直接映射到ROS节点,因此ROS 2安全特性(对于域参与者来说是特有的)也不再直接映射到ROS节点,而是Foxy引入了安全“域”的概念,其中的“域”是一个进程或一组进程,将共享相同的身份和访问控制规则。

这意味着安全文物是... **没有** 基于节点名称但基于安全飞地名称检索。可以通过使用ROS 参数设置一个节点飞地名称 `--enclave`, e.g. `ros2 run demo_nodes_py talker --ros-args --enclave /my_enclave`

相关设计文件: <https://github.com/ros2/design/pull/274>

注意权限文件受基本传输包大小的限制, 因此将许多权限分组到同一个飞地下 **没有** 如果产生的权限文件超过64kB,则工作。 [\[ros2/sros2#228\]](https://github.com/ros2/sros2/issues/228)

<span id="renaming-of-the-environment-variables"></span>

#### 环境变量的重新命名

<span id="id8"></span>

| 口号中的名称                | 在 Foxy 中的名称              |
|-----------------------------|-------------------------------|
| ROS_SECURITY_ROOT_DIRECTORY | ROS_SECURITY_KEYSTORE         |
| ROS_SECURITY_NODE_DIRECTORY | ROS_SECURITY_ENCLAVE_OVERRIDE |

环境变量重命名 {.docutils .align-default}

<span id="known-issues"></span>

## 已知问题

- [\[ros2/ros2#922\]](https://github.com/ros2/ros2/issues/922) 服务性能为: `rclcpp` 节点使用eProsima Fast-RTPS或ADLINK CyprinDDS作为 RMW 执行. 具体来说,服务客户端有时得不到服务器的响应.

- [\[ros2/rclcpp#1212\]](https://github.com/ros2/rclcpp/issues/1212) ready reentrant Waitable 对象可以尝试多次执行.

<span id="timeline-before-the-release"></span>

## 发布前的时间线

发布前的几个里程碑:

> > **说明**
> >
> > 以下日期显示,由于冠状病毒大流行,延长了大约两周。
>
> 2020年4月22日 (中文(简体) ).  
> API 和特性冻结 `ros_core` <span id="id2"></span>[\[1\]](#id5) 软件包。请注意,这包括 `rmw`,这是递归依赖 `ros_core`。在此点之后,只发布错误修正。新软件包可以独立发布。
>
> (原始内容存档于2020年4月29日) (百达).  
> 最新发布 `desktop` <span id="id3"></span>[\[2\]](#id6) 软件包。测试新特性。
>
> 2020年5月27日 (释放候选人)  
> 最新发布 `desktop` <span id="id4"></span>[\[2\]](#id6) 可用软件包。
>
> 2020年6月3日 (中文(简体) ).  
> Freeze rodistro. rodistro repo上没有Foxy的PRs会被合并(发布公告后重新开放).

<span id="id5"></span>

\[[1](#id2)\]

那个... `ros_core` 变体描述于 [变体](https://github.com/ros2/variants) 存储器。

<span id="id6"></span>

\[2\] ([1](#id3),[2](#id4))

那个... `desktop` 变体描述于 [变体](https://github.com/ros2/variants) 存储器。
