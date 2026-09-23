---
translation_status: machine_translated
source: Releases/Release-Eloquent-Elusor.rst
---

<span id="eloquent-elusor-eloquent"></span>

# Eloquent Elusor（`eloquent`)

*埃卢索语Name* 是ROS 2的第五版发布.

<span id="supported-platforms"></span>

## 支持的平台

口号Elusor支持以下平台: [平台支持级别](../The-ROS2-Project/Platform-Support-Tiers.md):

第一级平台:

- 乌邦图18.04 (比奥尼基语: `amd64` 财务报告和财务报告 `arm64`

- Mac macOS 10.14 (移动)

- Windows 10 (Visual Studio 2019) (英语).

第二级平台:

- 乌邦图18.04 (比奥尼基语: `arm32`

第三级平台:

- Debian 伸展(9): `amd64`, `arm64` 财务报告和财务报告 `arm32`

- OpenEmbed Thud (2.6) / webOS OSE : (中文(简体) ). `arm32` 财务报告和财务报告 `x86`

目标平台:

| 建筑 | 乌邦图·比奥尼奇(18.04) | 马科斯莫哈韦(10.14) | Windows 10 (VS2019) (英语). | 德比安·巴斯特(10) | OpenEmbed / webOS OSE 操作系统 |
|----|----|----|----|----|----|
| amd64 (中文(简体) ). | 第1级 \[d\]\[a\]\[s\] | 第1级 \[s\] | 第1级 \[s\] | 第3级 \[s\] |  |
| 军火64 | 第1级 \[d\]\[a\]\[s\] |  |  | 第3级 \[s\] | 第3级 \[s\] |
| 臂弹32 | 第2级 \[a\]\[s\] |  |  | 第3级 \[s\] | 第3级 \[s\] |

以下指标显示每个平台都有何种交付机制。

" \[d\]" Debian包将为本平台提供提交rodistro的包件.

" \[a\] " 二进制版本作为每个平台的单一档案提供,其中包含Elogueent ROS 2 repos文件中的所有软件包\[^7\]。

" \[s\] " 来源汇编。

中间软件执行支持 :

| 中间软件库 | 中件提供者 | 支助级别 | 平台 | 建筑 |
|----|----|----|----|----|
| rmw_fastrtps_cpp\* | eProsima 快速RTPS | 第1级 | 所有平台 | 所有建筑 |
| rmw_connext_cpp | RTI 连接 | 第1级 | 除Debian和OpenEmbed外的所有平台 | 除arm64/arm32外的所有建筑 |
| rmw_cyclonedds_cpp | Eclipse Cyclone DDS | 第二级 | 所有平台 | 所有建筑 |
| rmw_opensplice_cpp | ADLINK 打开文件 | 第二级 | 除Debian和OpenEmbed外的所有平台 | 所有建筑 |
| rmw_fastrtps_dynamic_cpp | eProsima 快速RTPS | 第二级 | 所有平台 | 所有建筑 |

" \* " 是指未执行的《保护所有移徙工人及其家庭成员权利国际公约》。

Middleware 执行支持依赖于平台支持级别。例如,在 Tier 2 平台上执行的 Lier 1 中间软件只能得到 Tier 2 支持。

最低语文要求:

- C++14

- ⁇  3.6

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
<th class="head"><p>乌邦图比奥奇</p></th>
<th class="head"><p>麦克OS**</p></th>
<th class="head"><p>视窗10**</p></th>
<th class="head"><p>德比安·巴斯特(英语:Debian Buster)</p></th>
<th class="head"><p>已打开*</p></th>
</tr>
</thead>
<tbody>
<tr class="row-odd">
<td><p>CMake</p></td>
<td><p>3.10.2</p></td>
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
<td><p>9.0.0</p></td>
<td><p>9.9.0</p></td>
<td><p>N/A</p></td>
<td><p>9.8.0*</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>食人鱼</p></td>
<td colspan="4"><p>1.10*</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-odd">
<td><p>打开CV</p></td>
<td><p>3.2.0</p></td>
<td><p>4.1.0</p></td>
<td><p>3.4.6*</p></td>
<td><p>3.2.0</p></td>
<td><p>4.1.0 / 3.2.0****</p></td>
</tr>
<tr class="row-even">
<td><p>打开SSL</p></td>
<td><p>1.1.0g</p></td>
<td><p>1.0.2r</p></td>
<td><p>1.0.2r</p></td>
<td><p>1.1.1c</p></td>
<td><p>1.1.1d / 1.1.1b****</p></td>
</tr>
<tr class="row-odd">
<td><p>宝可</p></td>
<td><p>1.8.0</p></td>
<td><p>1.9.0</p></td>
<td><p>1.8.0*</p></td>
<td><p>1.9.0</p></td>
<td><p>1.9.4</p></td>
</tr>
<tr class="row-even">
<td><p>Python</p></td>
<td><p>3.6.5</p></td>
<td><p>3.7.3</p></td>
<td><p>3.7.3</p></td>
<td><p>3.7.3</p></td>
<td><p>3.8.2 / 3.7.5****</p></td>
</tr>
<tr class="row-odd">
<td><p>Qt 键</p></td>
<td><p>5.9.5</p></td>
<td><p>5.12.3</p></td>
<td><p>5.10.0</p></td>
<td><p>5.11.3</p></td>
<td><p>5.14.1 / 5.12.5****</p></td>
</tr>
<tr class="row-even">
<td colspan="2"></td>
<td colspan="4"><p><strong>仅限 Linux</strong></p></td>
</tr>
<tr class="row-odd">
<td><p>个人计算机L</p></td>
<td><p>1.8.1</p></td>
<td><p>N/A</p></td>
<td><p>N/A</p></td>
<td><p>1.9.1</p></td>
<td><p>1.8.1</p></td>
</tr>
<tr class="row-even">
<td colspan="6"><p><strong>ROMW DDS 中件供应商</strong></p></td>
</tr>
<tr class="row-odd">
<td><p>Connext DDS</p></td>
<td colspan="3"><p>5.3.1***</p></td>
<td colspan="2"><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>Cyclone DDS</p></td>
<td colspan="5"><p>0.7.x (科奎特语).</p></td>
</tr>
<tr class="row-odd">
<td><p>快速RTPS( 快速区域贸易促进系统)</p></td>
<td colspan="5"><p>1.9.0</p></td>
</tr>
<tr class="row-even">
<td><p>打开文件</p></td>
<td colspan="4"><p>6.9.190705 开放源码软件</p></td>
<td><p>N/A</p></td>
</tr>
</tbody>
</table>

" \* " 是指这不是上游版本(可在官方操作系统寄存器上查阅),而是OSRF或社区分发的包(自定义寄存器上建立和分发的包)。

" \*\* " 滚动发行会看到这些依赖性在其存在期间的多个版本变化。为OpenEmberded显示的版本是3.1 Dunfell发行系列提供的版本;其他支持发行系列提供的版本在此列出: \<<https://github.com/ros/meta-ros/wiki/Package-Version-Differences>\>. 注意,根据此处显示的 OpenEmbed 支持策略,ROS distro 支持的 OpenEmbed 发布系列将在支持时间范围内改变: \<<https://github.com/ros/meta-ros/wiki/Policies#openembedded-release-series-support>\>. 然而,它将始终得到至少一个稳定的OpenEmbed发行系列的支持.

" \*\*\* " 预计,在待迁移补丁\[^8\]之前,这一数字将增加到Connext DDS 6.0.0。

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

[安装口号 Elusor](https://docs.ros.org/en/eloquent/Installation.html)

<span id="new-features-in-this-ros-2-release"></span>

## 本 ROS 2 发布中的新功能

我们谨强调以下几个特点和改进:

- [支持基于标记的发射文件(XML/YAML)](https://github.com/ros2/launch/pull/226)

- [改进以发射为基础的测试](https://github.com/ros2/ros2/issues/739#issuecomment-555743540)

- [在 CLI 上传密钥值参数](https://github.com/ros2/design/pull/245)

- [支持流日志宏](https://github.com/ros2/rclcpp/pull/926)

- [每个节点的日志](https://github.com/ros2/ros2/issues/789) - 节点的所有stdout/stderr输出都登录在~/.ros中

- [ros2 医生](https://index.ros.org/doc/ros2/Tutorials/Getting-Started-With-Ros2doctor/)

- [来源设置文件的性能得到改善](https://github.com/ros2/ros2/issues/764)

- rviz: (中文(简体) ). [交互式标记](https://github.com/ros2/rviz/pull/457), [扭矩环](https://github.com/ros2/rviz/pull/396), [tf 信件过滤器](https://github.com/ros2/rviz/pull/375)

- rqt : (韩语). [参数插件](https://github.com/ros-visualization/rqt_reconfigure/pull/31), [tf 树插件](https://github.com/ros-visualization/rqt_tf_tree/pull/13), [机器人引导插件](https://github.com/ros-visualization/rqt_robot_steering/pull/7) (也汇回大兴).

- [乌龟](https://github.com/ros/ros_tutorials/pull/53) (也汇回大兴).

- 落实《保护所有移徙工人及其家庭成员权利国际公约》:

  - [API 借给零副本信件](https://github.com/ros2/design/pull/256),用于: [rmw_iceoryx](https://github.com/ros2/rmw_iceoryx)

  - [快速RTPS 1.9.3](https://github.com/ros2/ros2/issues/734#issuecomment-518018479)

  - 新的第二级实施: [rmw_cyclonedds](https://github.com/ros2/rmw_cyclonedds) (也汇回大兴).

- 环境变量 [ROS_LOCALHOST_ONLY](https://github.com/ros2/ros2/issues/798) 以限制本地主机的通信

- MacOS Mojave 支持系统

- [追查工具](https://github.com/ros2/ros2/pull/748) 对 rcl 和 rclcpp 而言

在开发过程中, [豪言壮语元票](https://github.com/ros2/ros2/issues/734) 在GitHub上载有正在进行中的高水平任务的最新状况,以及参考具体门票的详情。

<span id="changes-since-the-dashing-release"></span>

## 自Dashing发行以来的变化

<span id="geometry-msgs"></span>

### geometry_msgs

那个... `geometry_msgs/msg/Quaternion.msg` 界面现在默认初始化为有效的四角星,其值如下:

$$\begin{split}x = 0 \\ y = 0 \\ z = 0 \\ w = 1\end{split}$$

以下是关于更详细情况的拉动请求: <https://github.com/ros2/common_interfaces/pull/74>

静态改造广播和听众 现在使用QoS耐久性 `transient_local` 编辑 `/tf_static` 主题 。 与 ROS 1 的固定设置类似, 静态变换只需发布一次 。 新的收听者将接收所有在世且此前已发布的静态广播机构的变换。 所有出版者必须更新以使用此耐久性设置, 或他们的信息不会被变换的收听者收到 。 请参看此拉动请求 : <https://github.com/ros2/geometry2/pull/160>

<span id="rclcpp"></span>

### rclcpp

<span id="api-break-with-get-actual-qos"></span>

#### API 断开 `get_actual_qos()`

在达兴介绍的 `get_actual_qos()` 方法 `PublisherBase` 财务报告和财务报告 `SubscriptionBase` 先前返回了 Rmw 类型, `rmw_qos_profile_t`,但这样做会尴尬地重复使用其它实体的创建。 `rclcpp::QoS` 换句话说。

现有代码需要使用 `rclcpp::QoS::get_rmw_qos_profile()` 如果需要 Rmw 配置文件,则使用方法。例如:

``` cpp
void my_func(const rmw_qos_profile_t & rmw_qos);

/* Previously: */
// my_func(some_pub->get_actual_qos());
/* Now: */
my_func(some_pub->get_actual_qos()->get_rmw_qos_profile());
```

直接打破这个功能而不是做一个勾选的理由是,它是一个新功能,预计用户不会经常使用。此外,由于只有返回类型正在改变,添加一个不同的新功能,将只能做一个折旧周期,而且 `get_actual_qos()` 最合适的名字,所以我们不得不选一个不太明显的名字来使用这种方法。

<span id="api-break-with-publisher-and-subscription-classes"></span>

#### API 与出版商和订阅类断开

为了精简出版商和订阅商的建筑,对建筑商的API进行了修改。

无法支持贬值周期,因为旧的签名采用rcl类型,而新的签名采用rcl类型。 `NodeBaseInterface` type 这样它就可以得到它现在需要的额外信息,并且没有办法从rcl 类型中获得所需的额外信息。如果有助于贡献者的话,新的签名可能会被重新输入,但是由于出版商和订阅者几乎总是使用工厂的功能或其他更高层次的API创建,所以我们不认为这对大多数用户来说是一个问题。

如果出现问题,请参看原文:pr.

<https://github.com/ros2/rclcpp/pull/867>

<span id="compiler-warning-about-unused-result-of-add-on-set-parameters-callback"></span>

#### 编译器警告未使用的结果 `add_on_set_parameters_callback`

*自口头补丁第2版(2020-12-04)以来*

用户应当保留返回的控件 `rclcpp::Node::add_on_set_parameters_callback`,否则它们的回调可能未注册。添加了警告,帮助识别未使用返回的控点的bug。

<https://github.com/ros2/rclcpp/pull/1243>

<span id="rmw"></span>

### rmw (英语).

<span id="api-break-due-to-addition-of-publisher-and-subscription-options"></span>

#### API 因添加出版和订阅选项而中断

那个... `rmw_create_publisher()` 方法中添加了新参数类型 `const rmw_publisher_options_t *`。这一新结构为新出版商提供了选项(超出了类型支持、主题名称和QoS)。

那个... `rmw_create_subscription()` 方法取消了一个论点, `bool ignore_local_publications`,并替换为类型新选项 `const rmw_subscription_options_t *`。该词 `ignore_local_publications` 选项已移动到新 `rmw_subscription_options_t` 类型。

在这两种情况下,作为指针的新参数可能永远不会是无效的,因此rmw执行器应该检查以确保选项不是无效的。此外,选项应该复制到相应的rmw结构中.

参见此拉动请求, 以及相关的拉动请求, 以获取更多细节 :

<https://github.com/ros2/rmw/pull/187>

<span id="ros2cli"></span>

### 罗斯2cli

<span id="ros2msg-and-ros2srv-deprecated"></span>

#### ros2msg和ros2srv贬值

CLI 工具 `ros2msg` 财务报告和财务报告 `ros2srv` 已替换为工具 `ros2interface`,它也支持动作和IDL接口。您可以运行 `ros2 interface --help` 用于使用。

<span id="ros2node"></span>

#### ros2 节点

服务客户端已被添加到 ros2node 信息中。 作为该修改的一部分, Python 函数 `ros2node.api.get_service_info` 已重新命名为 `ros2node.api.get_service_server_info`.

<span id="rviz"></span>

### rviz 维兹

<span id="renamed-2d-nav-goal-tool"></span>

#### 重命名“ 2D 导航目标” 工具

该工具被改名为"2D Goal Pose",默认话题从此更改. `/move_base_simple/goal` 改为: `/goal_pose`.

以下是相关的拉动请求 :

<https://github.com/ros2/rviz/pull/455>

<span id="tf2-buffer"></span>

### TF2 缓冲

TF2缓冲器现在需要给定时器接口.

如果不给出定时器接口,则会丢出一个例外.

例如:

``` cpp
tf = std::make_shared<tf2_ros::Buffer>(get_clock());
// The next two lines are new in Eloquent
auto timer_interface = std::make_shared<tf2_ros::CreateTimerROS>(
  this->get_node_base_interface(),
  this->get_node_timers_interface());
tf->setCreateTimerInterface(timer_interface);
// Pass the Buffer to the TransformListener as before
transform_listener = std::make_shared<tf2_ros::TransformListener>(*tf);
```

<span id="rcl"></span>

### rcl (中文(简体) ).

<span id="ros-command-line-argument-changes"></span>

#### ROS 命令行参数变化

为了应付日益复杂的界面,现在有一套扩展的配置选项,ROS CLI语法已经改变。举例来说,使用Dashing语法的命令行有:

``` console
$ ros2 run some_package some_node foo:=bar __params:=/path/to/params.yaml __log_level:=WARN --user-flag
```

使用口语法(及以后)写成:

``` console
$ ros2 run some_package some_node --ros-args --remap foo:=bar --params-file /path/to/params.yaml --log-level WARN -- --user-flag
```

这种明确的语法提供了新的特性, 如单一参数指派 `--param name:=value`关于进一步参考和理由,请检查 [ROS 命令行参数设计文件](https://design.ros2.org/articles/ros_command_line_arguments.html).

> **警告**
>
> 前语法已经贬值,定于下次发行时删除.

<span id="known-issues"></span>

## 已知问题

- [\[ros2/rosidl#402\]](https://github.com/ros2/rosidl/issues/402) `find_package(PCL)` 干扰 ROS 接口生成 。 Workround: 引用 `find_package(PCL)` *之后* `rosidl_generate_interfaces()`.

- [\[ros2/rclcpp#893\]](https://github.com/ros2/rclcpp/issues/893) `rclcpp::Context` 未销毁,因为参考周期 `rclcpp::GraphListener`。这会导致记忆漏水。由于可能破坏ABI,一个固定设备没有被回放。

<span id="timeline-before-the-release"></span>

## 发布前的时间线

发布前的几个里程碑:

> 9月30日(阿尔法)  
> 已有的核心软件包的首次发布。 测试可以从现在开始进行( 有些功能可能还没有落地 ) 。
>
> 10月18日,星期五  
> API 和核心包的特性冻结 在此点之后只能发布错误修正。 新的包可以独立发布 。
>
> Thu. October 24 (β) (英语).  
> 已更新的核心软件包的发布。 对最新特性进行额外测试 。
>
> 11月13日(释放候选人)  
> 现有核心软件包的最新发布情况。
>
> Tue. 11月19日 (中文(简体) ).  
> Freeze rodistro. rodistro repo上的Eloatent不会合并(发布公告后重新开放).
