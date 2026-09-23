---
translation_status: machine_translated
source: Releases/Release-Crystal-Clemmys.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="crystal-clemmys-crystal"></span>

# Crystal Clemmys（`crystal`)

*水晶克莱米斯* 是ROS 2的第三次发布.

<span id="supported-platforms"></span>

## 支持的平台

Crystal Clemmis 支持以下平台: [平台支持级别](../The-ROS2-Project/Platform-Support-Tiers.md):

第一级平台:

- Ubuntu 18.04 (比奥尼克语).

- Mac macOS 10.12 (塞拉利昂)

- 视窗 10

第二级平台:

- 乌邦图16.04 (日语).

目标平台:

| 建筑 | 乌邦图·比奥尼奇(18.04) | MacOS Sierra (10.12) (英语). | Windows 10 (VS 2017) (英语). | 乌本图·谢尼尔(16.04. | Debian 伸展(9) |
|----|----|----|----|----|----|
| amd64 (中文(简体) ). | 第1级 \[d\]\[a\]\[s\] | 第1级 \[s\] | 第1级 \[s\] | 第二级 \[s\] | 第3级 \[s\] |
| 军火64 | 第1级 \[d\]\[a\]\[s\] |  |  | 第二级 \[s\] | 第3级 \[s\] |

以下指标显示每个平台都有何种交付机制。

" \[d\]" Debian包将为本平台提供提交rodistro的包件.

" \[a\] " 二进制释放作为每个平台的单一档案提供,包含Crystal ROS 2 repos文件中的所有包\[^4\].

" \[s\] " 来源汇编。

中间软件执行支持 :

| 中间软件库 | 中件提供者 | 支助级别 | 平台 | 建筑 |
|----|----|----|----|----|
| rmw_fastrtps_cpp\* | eProsima 快速RTPS | 第1级 | 所有平台 | 所有建筑 |
| rmw_connext_cpp | RTI 连接 | 第1级 | 除Debian以外的所有平台 | 除arm64外的所有建筑 |
| rmw_opensplice_cpp | ADLINK 打开文件 | 第二级 | 除Debian以外的所有平台 | 所有建筑 |
| rmw_fastrtps_dynamic_cpp | eProsima 快速RTPS | 第二级 | 所有平台 | 所有建筑 |
| rmw_connext_dynamic_cpp | RTI 连接 | 第二级 | 除Debian之外的所有平台 | 除arm64外的所有建筑 |

" \* " 是指未执行的《保护所有移徙工人及其家庭成员权利国际公约》。

Middleware 执行支持依赖于平台支持级别。例如,在 Tier 2 平台上执行的 Lier 1 中间软件只能得到 Tier 2 支持。

最低语文要求:

- C11\[^5\]

- C++14

- ⁇  3.5

依赖性要求:

<table class="docutils align-default">
<thead>
<tr class="row-odd">
<th class="head"></th>
<th colspan="4" class="head"><p>所需支助</p></th>
<th class="head"><p>建议的支助</p></th>
</tr>
<tr class="row-even">
<th class="head"><p>软件包</p></th>
<th class="head"><p>乌邦图比奥奇</p></th>
<th class="head"><p>麦克OS**</p></th>
<th class="head"><p>视窗10*</p></th>
<th class="head"><p>乌本图Xenial [s]</p></th>
<th class="head"><p>Debian 伸缩</p></th>
</tr>
</thead>
<tbody>
<tr class="row-odd">
<td><p>CMake</p></td>
<td><p>3.10.2</p></td>
<td><p>3.13.3</p></td>
<td><p>3.13.3</p></td>
<td><p>3.5.1</p></td>
<td><p>3.7.2</p></td>
</tr>
<tr class="row-even">
<td><p>爱咪</p></td>
<td><p>3.3.2</p></td>
<td><p>3.3.2</p></td>
<td><p>3.3.2</p></td>
<td><p>3.3.2</p></td>
<td><p>3.3.2</p></td>
</tr>
<tr class="row-odd">
<td><p>Gazebo</p></td>
<td><p>9.0.0</p></td>
<td><p>9.9.0</p></td>
<td><p>N/A</p></td>
<td><p>9.9.0*</p></td>
<td><p>9.8.0*</p></td>
</tr>
<tr class="row-even">
<td><p>食人鱼</p></td>
<td colspan="5"><p>1.10*</p></td>
</tr>
<tr class="row-odd">
<td><p>打开CV</p></td>
<td><p>3.2.0</p></td>
<td><p>4.0.1</p></td>
<td><p>3.4.1*</p></td>
<td><p>2.4.9</p></td>
<td><p>3.2*</p></td>
</tr>
<tr class="row-even">
<td><p>打开SSL</p></td>
<td><p>1.1.0g</p></td>
<td><p>1.0.2q</p></td>
<td><p>1.0.2q</p></td>
<td><p>1.0.2g</p></td>
<td><p>1.1.0j</p></td>
</tr>
<tr class="row-odd">
<td><p>宝可</p></td>
<td><p>1.8.0</p></td>
<td><p>1.9.0</p></td>
<td><p>1.8.0*</p></td>
<td><p>1.8.0*</p></td>
<td><p>1.8.0*</p></td>
</tr>
<tr class="row-even">
<td><p>Python</p></td>
<td><p>3.6.5</p></td>
<td><p>3.7.2</p></td>
<td><p>3.7.2</p></td>
<td><p>3.5.1</p></td>
<td><p>3.5.3</p></td>
</tr>
<tr class="row-odd">
<td><p>Qt 键</p></td>
<td><p>5.9.5</p></td>
<td><p>5.12.0</p></td>
<td><p>5.10.0</p></td>
<td><p>5.5.1</p></td>
<td><p>5.7.1</p></td>
</tr>
<tr class="row-even">
<td colspan="2"></td>
<td colspan="2"><p><strong>仅限 Linux</strong></p></td>
<td colspan="2"></td>
</tr>
<tr class="row-odd">
<td><p>个人计算机L</p></td>
<td><p>1.8.1</p></td>
<td><p>N/A</p></td>
<td><p>N/A</p></td>
<td><p>1.7.2</p></td>
<td><p>1.8.0</p></td>
</tr>
<tr class="row-even">
<td colspan="6"><p><strong>ROMW DDS 中件供应商</strong></p></td>
</tr>
<tr class="row-odd">
<td><p>Connext DDS</p></td>
<td colspan="4"><p>5.3.1</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>快速RTPS( 快速区域贸易促进系统)</p></td>
<td colspan="5"><p>1.7.0</p></td>
</tr>
<tr class="row-odd">
<td><p>打开文件</p></td>
<td colspan="5"><p>6.9.181127 开放源码软件</p></td>
</tr>
</tbody>
</table>

" \* " 是指这不是上游版本(可在官方操作系统寄存器上查阅),而是OSRF或社区分发的包(自定义寄存器上建立和分发的包)。

" \*\* " 滚动分发会看到这些依附关系在其存在期间的多个版本的变化。

" \[s\] " 从源头汇编,ROS建设农场不会为这些平台生产任何二进制包。

此文档只抓取 ROS 发行的首次发布时的版本, 并且不会随着依赖关系向前移动而更新, 因此这些版本是低水印 。

依赖软件包管理器的使用 :

- Ubuntu, Debian: apt (英语).

- 马科斯:土生土长,皮普

- 视窗:巧克力,pip

构建系统支持 :

- ament_cmake

- 制作( C)

- 设置工具

<span id="new-features-in-this-ros-2-release"></span>

## 本 ROS 2 发布中的新功能

- C/C++行动[服务器](https://github.com/ros2/examples/tree/af08e6f7ac50f7808dbe6165f1adfd8e6cd3a79c/rclcpp/minimal_action_server) / [客户端](https://github.com/ros2/examples/tree/af08e6f7ac50f7808dbe6165f1adfd8e6cd3a79c/rclcpp/minimal_action_client) 实例)

- [gazebo_ros_pkgs](http://gazebosim.org/tutorials?tut=ros2_overview)

- [image_transport](https://github.com/ros-perception/image_common/wiki/ROS2-Migration)

- [导航2](https://github.com/ros-planning/navigation2/blob/master/README.md)

- [rosbag2](https://index.ros.org/r/rosbag2/github-ros2-rosbag2/#crystal)

- [rqt](../Concepts/Intermediate/About-RQt.md)

- 改善内存管理

- 关于节点的回顾信息

- 发射系统改进

  - [参数](https://github.com/ros2/launch/pull/123)

  - [嵌入式发射文件](https://github.com/ros2/launch/issues/116)

  - [条件](https://github.com/ros2/launch/issues/105)

  - [将参数传递到节点](https://github.com/ros2/launch/issues/117)

- 奠定基础 [基于文件的日志和/rosout 发布](https://github.com/ros2/rcl/pull/327)

- [Python 中的时间和期限 API](https://github.com/ros2/rclpy/issues/186)

- [参数与 Python 节点配合](https://github.com/ros2/rclpy/issues/202)

<span id="changes-since-the-bouncy-release"></span>

## 弹簧释放后的变化

自《公约》生效以来的变化 [博尔森](Release-Bouncy-Bolson.md) 释放 :

- 几何学2 - `tf2_ros::Buffer` API 更改

  `tf2_ros::Buffer` 现在使用 `rclcpp::Time`,而构造器则需要 `shared_ptr` 改为: `rclcpp::Clock` 实例。见 <https://github.com/ros2/geometry2/pull/67> 详细情况,示例使用:

  ``` c++
  #include <tf2_ros/transform_listener.h>
  #include <rclcpp/rclcpp.hpp>
  ...
  # Assuming you have a rclcpp::Node my_node
  tf2_ros::Buffer buffer(my_node.get_clock());
  tf2_ros::TransformListener tf_listener(buffer);
  ```

- 全部人员 `rclcpp` 财务报告和财务报告 `rcutils` 日志宏需要分号.

  见 <https://github.com/ros2/rcutils/issues/113> 详细情况。

- `rcutils_get_error_string_safe()` 财务报告和财务报告 `rcl_get_error_string_safe()` 已被替换为 `rcutils_get_error_string().str` 财务报告和财务报告 `rcl_get_error_string().str`.

  见 <https://github.com/ros2/rcutils/pull/121> 详细情况。

- 额,额... `rmw_init` API 更改

  有两个新结构,即 `rcl_context_t` 页:1 `rcl_init_options_t`,用于: `rmw_init`中选项,用于将选项传递到中间软件,并输入到 `rmw_init`。上下文是一个控件,它是一个输出 `rmw_init` 函数用于确定每个实体与哪一个内自旋循环有关,其中“实体”是像节点、守卫条件等创建的。

  之所以在此列出,是因为替代rmw执行的维护者需要执行这些新功能,才能使其rmw执行工作在Crystal.

  此功能有签名更改 :

  - [rmw_init](https://github.com/ros2/rmw/blob/b7234243588a70fce105ea20b073f5ef6c1b685c/rmw/include/rmw/init.h#L54-L82)

  此外,这些新职能需要由每个 Rmw 执行单位履行:

  - [rmw_shutdown](https://github.com/ros2/rmw/blob/b7234243588a70fce105ea20b073f5ef6c1b685c/rmw/include/rmw/init.h#L84-L109)

  - [rmw_init_options_init](https://github.com/ros2/rmw/blob/b7234243588a70fce105ea20b073f5ef6c1b685c/rmw/include/rmw/init_options.h#L62-L92)

  - [rmw_init_options_copy](https://github.com/ros2/rmw/blob/b7234243588a70fce105ea20b073f5ef6c1b685c/rmw/include/rmw/init_options.h#L94-L128)

  - [rmw_init_options_fini](https://github.com/ros2/rmw/blob/b7234243588a70fce105ea20b073f5ef6c1b685c/rmw/include/rmw/init_options.h#L130-L153)

  以下是在Rmw执行中为了坚持这一API改变而需要做的最小改变的例子:

  - [rmw_fastrtps pr (中文(简体) ).](https://github.com/ros2/rmw_fastrtps/pull/237/files)

- rcl - (英语). `rcl_init` API 更改

  喜欢 `rmw` 上面的更改,有两个新的结构 `rcl` 调用 `rcl_context_t` 财务报告和财务报告 `rcl_init_options_t`。输入选项被传递到 `rcl_init` 上下文用于将所有其他 RCL 实体与特定的内置-shutdown 循环连接起来,有效地使内置和关闭功能不再具有全球功能,或者这些功能不再使用全球状态,而是将所有状态囊括在上下文类型中。

  客户端库执行中的任何维护者(也使用) `rcl` 在引擎盖下)需要做出改变才能与Crystal合作.

  这些功能被移除:

  - `rcl_get_global_arguments`

  - `rcl_get_instance_id`

  - `rcl_ok`

  这些功能的签名有变化:

  - [rcl_init](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/init.h#L30-L82)

  - [rcl_shutdown](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/init.h#L84-L111)

  - [rcl_guard_condition_init](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/guard_condition.h#L54-L99)

  - [rcl_guard_condition_init_from_rmw](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/guard_condition.h#L101-L140)

  - [rcl_node_init](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/node.h#L100-L194)

  - [rcl_timer_init](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/timer.h#L64-L159)

  这些是新的职能和类型:

  - [rcl_context_t](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/context.h#L36-L136)

  - [rcl_get_zero_initialized_context](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/context.h#L138-L142)

  - [rcl_context_fini](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/context.h#L146-L171)

  - [rcl_context_get_init_options](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/context.h#L175-L205)

  - [rcl_context_get_instance_id](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/context.h#L207-L233)

  - [rcl_context_is_valid](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/context.h#L235-L255)

  - [rcl_init_options_t](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/init_options.h#L32-L37)

  - [rcl_get_zero_initialized_init_options](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/init_options.h#L39-L43)

  - [rcl_init_options_init](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/init_options.h#L45-L73)

  - [rcl_init_options_copy](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/init_options.h#L75-L105)

  - [rcl_init_options_fini](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/init_options.h#L107-L128)

  - [rcl_init_options_get_rmw_init_options](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/init_options.h#L130-L153)

  - [rcl_node_is_valid_except_context](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/node.h#L288-L299)

  - [rcl_publisher_get_context](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/publisher.h#L378-L404)

  - [rcl_publisher_is_valid_except_context](https://github.com/ros2/rcl/blob/657d9e84c73e4268176efd163e96fda73c1a76d9/rcl/include/rcl/publisher.h#L428-L439)

  这些新的和已更改的功能将影响您如何在客户端库中处理输入和关闭。 例如, 请查看以下 `rclcpp` 财务报告和财务报告 `rclpy` 公关:

  - [rclcpp](https://github.com/ros2/rclcpp/pull/587)

  - [rclpy](https://github.com/ros2/rclpy/pull/249)

  然而,您可能只是继续提供您的客户端库中一个单一的,全局的输入和关闭,并且只存储一个全局的上下文对象.

<span id="known-issues"></span>

## 已知问题

- Fast-RTPS 1.7.0中的种族条件可能导致消息降压([问题](https://github.com/ros2/rmw_fastrtps/issues/258)).

- 使用带有rmw_fastrtps_cpp的TRANSINT_LOCAL QoS设置,可以使用大信件崩溃应用程序([问题](https://github.com/ros2/rmw_fastrtps/issues/257)).

- rmw_fastrtps_cpp与其他执行之间的交叉报仇通信在Windows上无法运行([问题](https://github.com/ros2/rmw_fastrtps/issues/246)).

- 当在 macOS 和 Windows 上使用 OpenSplice(版本 \< 6.9.190227) 时,如果在当前软件包中也存在相同名称,您可能会遇到使用其他软件包中的名称引用字段类型时的命名冲突([问题](https://github.com/ros2/rmw_opensplice/issues/259)。通过更新到更新的 OpenSplices 版本以及至少Crystal 的第三次补丁发布,这个问题应该得到解决。在 Linux 更新到最新的 Debian 软件包时,将包含最新的 OpenSplices 版本。
