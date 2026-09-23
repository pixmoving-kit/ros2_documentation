---
translation_status: machine_translated
source: Releases/Release-Dashing-Diademata.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="dashing-diademata-dashing"></span>

# Dashing Diademata（`dashing`)

*达兴迪阿迪玛塔* 是ROS 2的第四版发布.

<span id="supported-platforms"></span>

## 支持的平台

Dashing Diademata 支持以下平台: 根据 [平台支持级别](../The-ROS2-Project/Platform-Support-Tiers.md):

第一级平台:

- 乌邦图18.04 (比奥尼基语: `amd64` 财务报告和财务报告 `arm64`

- Mac macOS 10.12 (塞拉利昂)

- Windows 10 (Visual Studio 2019) (英语).

第二级平台:

- 乌邦图18.04 (比奥尼基语: `arm32`

第三级平台:

- Debian 伸展(9): `amd64`, `arm64` 财务报告和财务报告 `arm32`

- OpenEmbed Thud (2.6) / webOS OSE : (中文(简体) ). `arm32` 财务报告和财务报告 `x86`

目标平台:

| 建筑 | 乌邦图·比奥尼奇(18.04) | MacOS Sierra (10.12) (英语). | Windows 10 (VS2019) (英语). | Debian 伸展(9) | OpenEmbed / webOS OSE 操作系统 |
|----|----|----|----|----|----|
| amd64 (中文(简体) ). | 第1级 \[d\]\[a\]\[s\] | 第1级 \[s\] | 第1级 \[s\] | 第3级 \[s\] |  |
| 军火64 | 第1级 \[d\]\[a\]\[s\] |  |  | 第3级 \[s\] | 第3级 \[s\] |
| 臂弹32 | 第2级 \[a\]\[s\] |  |  | 第3级 \[s\] | 第3级 \[s\] |

以下指标显示每个平台都有何种交付机制。

" \[d\]" Debian包将为本平台提供提交rodistro的包件.

" \[a\] " 二进制版本作为每个平台的单一档案提供,包含Dashing ROS 2 repos文件中的所有软件包\[^6\]。

" \[s\] " 来源汇编。

中间软件执行支持 :

| 中间软件库 | 中件提供者 | 支助级别 | 平台 | 建筑 |
|----|----|----|----|----|
| rmw_fastrtps_cpp\* | eProsima 快速RTPS | 第1级 | 所有平台 | 所有建筑 |
| rmw_connext_cpp | RTI 连接 | 第1级 | 除Debian和OpenEmbed外的所有平台 | 除arm64/arm32外的所有建筑 |
| rmw_cyclonedds_cpp | Eclipse Cyclone DDS | 第二级 | 所有平台 | 所有建筑 |
| rmw_opensplice_cpp | 自动链接 OpenSplice | 第二级 | 除Debian和OpenEmbed外的所有平台 | 所有建筑 |
| rmw_fastrtps_dynamic_cpp | eProsima 快速RTPS | 第二级 | 所有平台 | 所有建筑 |

" \* " 是指未执行的《保护所有移徙工人及其家庭成员权利国际公约》。

Middleware 执行支持依赖于平台支持级别。例如,在 Tier 2 平台上执行的 Lier 1 中间软件只能得到 Tier 2 支持。

最低语文要求:

- C++14

- ⁇  3.5

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
<th class="head"><p>Debian 伸缩</p></th>
<th class="head"><p>已打开*</p></th>
</tr>
</thead>
<tbody>
<tr class="row-odd">
<td><p>CMake</p></td>
<td><p>3.10.2</p></td>
<td><p>3.14.4</p></td>
<td><p>3.14.4</p></td>
<td><p>3.7.2</p></td>
<td><p>3.16.1 / 3.12.2***</p></td>
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
<td><p>3.2*</p></td>
<td><p>4.1.0 / 3.2.0***</p></td>
</tr>
<tr class="row-even">
<td><p>打开SSL</p></td>
<td><p>1.1.0g</p></td>
<td><p>1.0.2r</p></td>
<td><p>1.0.2r</p></td>
<td><p>1.1.0j</p></td>
<td><p>1.1.1d / 1.1.1b***</p></td>
</tr>
<tr class="row-odd">
<td><p>宝可</p></td>
<td><p>1.8.0</p></td>
<td><p>1.9.0</p></td>
<td><p>1.8.0*</p></td>
<td><p>1.8.0*</p></td>
<td><p>1.9.4</p></td>
</tr>
<tr class="row-even">
<td><p>Python</p></td>
<td><p>3.6.5</p></td>
<td><p>3.7.3</p></td>
<td><p>3.7.3</p></td>
<td><p>3.5.3</p></td>
<td><p>3.8.2 / 3.7.5***</p></td>
</tr>
<tr class="row-odd">
<td><p>Qt 键</p></td>
<td><p>5.9.5</p></td>
<td><p>5.12.3</p></td>
<td><p>5.10.0</p></td>
<td><p>5.7.1</p></td>
<td><p>5.14.1 / 5.12.5***</p></td>
</tr>
<tr class="row-even">
<td></td>
<td></td>
<td colspan="2"><p><strong>仅限 Linux</strong></p></td>
<td colspan="2"></td>
</tr>
<tr class="row-odd">
<td><p>个人计算机L</p></td>
<td><p>1.8.1</p></td>
<td><p>N/A</p></td>
<td><p>N/A</p></td>
<td><p>1.8.0</p></td>
<td><p>1.8.1</p></td>
</tr>
<tr class="row-even">
<td colspan="6"><p><strong>ROMW DDS 中件供应商</strong></p></td>
</tr>
<tr class="row-odd">
<td><p>Connext DDS</p></td>
<td colspan="3"><p>5.3.1</p></td>
<td colspan="2"><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>Cyclone DDS</p></td>
<td colspan="5"><p>0.7.x (科奎特语).</p></td>
</tr>
<tr class="row-odd">
<td><p>快速RTPS( 快速区域贸易促进系统)</p></td>
<td colspan="5"><p>1.8.0</p></td>
</tr>
<tr class="row-even">
<td><p>打开文件</p></td>
<td colspan="4"><p>6.9.190403 开放源码软件</p></td>
<td><p>N/A</p></td>
</tr>
</tbody>
</table>

" \* " 是指这不是上游版本(可在官方操作系统寄存器上查阅),而是OSRF或社区分发的包(自定义寄存器上建立和分发的包)。

" \*\* " 滚动发行会看到这些依赖性在其存在期间的多个版本变化。为OpenEmberded显示的版本是3.1 Dunfell发行系列提供的版本;其他支持发行系列提供的版本在此列出: \<<https://github.com/ros/meta-ros/wiki/Package-Version-Differences>\>. 注意,根据此处显示的 OpenEmbed 支持策略,ROS distro 支持的 OpenEmbed 发布系列将在支持时间范围内改变: \<<https://github.com/ros/meta-ros/wiki/Policies#openembedded-release-series-support>\>. 然而,它将始终得到至少一个稳定的OpenEmbed发行系列的支持.

" \*\*\* " WebOS OSE提供了这个不同的版本.

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

[安装 Dashing Diademata 程序](https://docs.ros.org/en/dashing/Installation.html)

<span id="new-features-in-this-ros-2-release"></span>

## 本 ROS 2 发布中的新功能

我们谨强调以下几个特点和改进:

- [构成部分](../Tutorials/Intermediate/Composition.md) 它们既可以独立使用,也可以在一个进程内组成,两种方式都得到来自 `launch` 文档。

- 那个... [流程内通信](../Tutorials/Demos/Intra-Process-Communication.md) (仅C++)得到了改进,既包括时间上的改进,也包括尽量减少复制。

- Python客户端库已经更新,以匹配大多数C++等同器,一些与内存使用和性能相关的重要bug修正和改进已经登陆.

- 参数现在是一个完全替代 `dynamic_reconfigure` 从ROS 1 中包含诸如范围或只读的限制。

- 依靠(一个子集) [IDL 4.2 (英语).](https://www.omg.org/spec/IDL/4.2) 对于信件生成管道,现在可以使用 `.idl` 文件( 除此之外) `.msg` / `.srv` / `.action` 文件 。 此更改与支持普通字符串的可选 UTF-8 编码以及 UTF-16 编码的多字节字符串(参见 [宽字符串设计文章](https://design.ros2.org/articles/wide_strings.html)).

- 命令行工具与 `actions` 财务报告和财务报告 `components`.

- 支持服务设置的截止日期, lifespan和lifeality质量.

- 移动它 2 [α 释放](https://github.com/AcutronicRobotics/moveit2/releases/tag/moveit_2_alpha).

请看 [发车票](https://github.com/ros2/ros2/issues/607) 在GitHub上,其中包含更多信息以及具体车票的参考文献,并附有更多细节。

<span id="changes-since-the-crystal-release"></span>

## Crystal 发布后的变化

<span id="declaring-parameters"></span>

### 宣告参数

从Dashing开始的参数行为发生了一些变化,也导致了一些新的API和对其他API的贬值。 `rclcpp` 财务报告和财务报告 `rclpy` 有关API变化的更多信息,请在下面的章节中查阅。

<span id="getting-and-setting-undeclared-parameters"></span>

#### 获取和设置未宣布参数

截至Dashing,在访问或设定之前,现在需要宣布参数。

在达兴之前,你可以打电话 `get_parameter(name)` 并获得一个数值,如果它以前已经设定过,或者获得一个类型参数 `PARAMETER_NOT_SET`。也可以拨打 `set_parameter(name, value)` 在任何时间,即使该参数先前未被设置。

自 Dashing 以来, 您需要先声明一个参数然后才能获取或设置它。 如果您尝试获取或设置一个未宣布的参数, 您将获得一个例外, 例如参数NotDeclaredExcuseion, 或者在某些情况下, 您将获得以多种方式传达的不成功结果( 更多细节请参见特定函数) 。

然而,您可以通过使用以下方法获得旧行为(多数见下一段中的注解): `allow_undeclared_parameters` 选项来创建节点。您可能想要这样做,以避免代码更改,或满足一些不寻常的使用例。例如,“全球参数服务器”或“参数黑板”可能希望允许外部节点在自己上设置新参数而不首先声明,因此它可能使用 `allow_undeclared_parameters` 但是,在多数情况下,不建议采用这个选项,因为它使其他参数API对参数名称类型和“设定前使用”逻辑错误等错误不太安全。

注意使用 `allow_undeclared_parameters` 将会为“ 获得” 和“ 设置” 方法获得大部分的旧行为, 但是它不会将所有与参数有关的行为变化还原为ROS Crystal 。 因为您还需要设置 `automatically_declare_parameters_from_overrides` 选项到 `true`,下文将对此加以说明。 [使用 YAML 文件的参数配置](#parameter-configuration-using-a-yaml-file).

<span id="declaring-a-parameter-with-a-parameterdescriptor"></span>

#### 使用参数描述符宣布参数

在使用参数之前声明参数的另一个好处是,它允许您同时声明一个参数描述符.

现在,在宣布一个参数时,您可能包含一个自定义 `ParameterDescriptor` 以及名称和默认值。 `ParameterDescriptor` 定义为 `rcl_interfaces/msg/ParameterDescriptor` 并包含元数据,例如: `description` 和限制,例如 `read_only` 或 时 间 `integer_range`。这些限制可用于在设定参数时拒绝无效值和/或作为外部工具的提示,说明哪些值对给定参数有效。 `read_only` 约束将防止参数在被宣布后值发生改变,并防止被宣布。

参考一下,这里有一个链接: `ParameterDescriptor` 在撰写此信件时:

<https://github.com/ros2/rcl_interfaces/blob/0aba5a142878c2077d7a03977087e7d74d40ee68/rcl_interfaces/msg/ParameterDescriptor.msg#L1>

<span id="parameter-configuration-using-a-yaml-file"></span> <span id="id1"></span>

#### 使用 YAML 文件的参数配置

至 Dashing , YAML 配置文件中的参数, 例如通过命令行参数传递到节点 `__params:=`,仅用于在声明参数时覆盖一个参数的默认值。

在Dashing之前,通过YAML文件通过的任何参数都会隐含地设置在节点上.

自达兴以来,情况已不再如此,因为需要公布参数,以便出现在节点上,供外部观察者使用,例如: `ros2 param list`.

旧行为可以使用 `automatically_declare_parameters_from_overrides` 选项。如果设置为 `true`,在构建节点时将自动声明输入的 YAML 文件的所有参数。这可以用来避免对您现有代码的重大修改或服务特定使用例。例如,一个“全球参数服务器”可能希望在发射时以任意参数进行播种,它不可能提前宣布。然而,大多数时候不推荐这个选项,因为它可能导致在YAML文件中设置一个参数,并假设节点将使用它,即使节点没有实际使用它。

将来我们希望有一个检查器,它会警告你 如果你通过一个参数 到一个节点,它不期待。

YAML文件中的参数会在首次宣布时继续影响参数的值.

<span id="ament-cmake"></span>

### ament_cmake

CMake 函数 `ament_index_has_resource` 也回来啦 `TRUE` 或 时 间 `FALSE`截至2007年12月31日的 [此释放](https://github.com/ament/ament_cmake/pull/155) 它返回前缀路径,以防找到资源,或者 `FALSE`.

如果您在这样的 CMake 条件中使用返回值 :

``` cmake
ament_index_has_resource(var ...)
if(${var})
```

您需要更新条件以确保它考虑字符串值为 `TRUE`:

``` cmake
if(var)
```

<span id="rclcpp"></span>

### rclcpp

<span id="behavior-change-for-node-get-node-names"></span>

#### 行为改变 `Node::get_node_names()`

职能 `NodeGraph::get_node_names()`,因此,还 `Node::get_node_names()`,现在返回 a `std::vector<std::string>` 包含完全合格的节点名称并包含其命名空间,而不仅仅是节点名称。

<span id="changed-the-way-that-options-are-passed-to-nodes"></span>

#### 将选项传递到节点的方式更改

扩展参数( 超出名称和名称空间) 到 `rclcpp::Node()` 构造器已替换为 `rclcpp::NodeOptions` 结构。 [ros2/rclcpp#622](https://github.com/ros2/rclcpp/pull/622/files) 关于选项的结构和默认值的详细信息。

如果您正在使用扩展参数中的任何一个 `rclcpp::Node()` 像这样:

``` cpp
auto context = rclcpp::contexts::default_context::get_global_default_context();
std::vector<std::string> args;
std::vector<rclcpp::Parameter> params = { rclcpp::Parameter("use_sim_time", true) };
auto node = std::make_shared<rclcpp::Node>("foo_node", "bar_namespace", context, args, params);
```

您需要更新以使用 `NodeOptions` 结构

``` cpp
std::vector<std::string> args;
std::vector<rclcpp::Parameter> params = { rclcpp::Parameter("use_sim_time", true) };
rclcpp::NodeOptions node_options;
node_options.arguments(args);
node_options.parameter_overrides(params);
auto node = std::make_shared<rclcpp::Node>("foo_node", "bar_namespace", node_options);
```

<span id="changes-to-creating-publishers-and-subscriptions"></span>

#### 创建出版商和订阅的更改

在Dashing, 创建出版商和订阅商方面有一些新变化:

- QoS 设置现在使用新的 `rclcpp::QoS` 分类,且 API 鼓励用户至少指定历史深度。

- 选项现在作为对象传递,即: `rclcpp::PublisherOptions` 财务报告和财务报告 `rclcpp::SubscriptionOptions`.

所有更改都是向后兼容的(不需要代码更改),但已有的几种调用样式已经贬值,鼓励用户更新到新的签名.

------------------------------------------------------------------------

过去,在创建出版商或订阅时,你要么不能指定任何QoS设置(例如只为出版商提供主题名称),要么可以指定“qos profile”数据结构(类型) `rmw_qos_profile_t`)所有设置已经设置。现在必须使用新设置 `rclcpp::QoS` 对象指定您的 QoS 和至少您的 QoS 的历史设置。这鼓励用户在使用时指定历史深度 `KEEP_LAST`,而不是将其默认为可能或可能不合适的值。

在ROS 1中,这被称为 `queue_size` 而C++和Python都要求这样做。 我们正在修改ROS 2 API,以恢复这一要求。

------------------------------------------------------------------------

此外,以前在创建出版商或订阅时可能通过的任何选择现在都封装在一种出版物中。 `rclcpp::PublisherOptions` 财务报告和财务报告 `rclcpp::SubscriptionOptions` 。这允许缩短签名,更方便的使用,并且可以添加新的未来选项而不突破 API。

------------------------------------------------------------------------

一些创建出版商和订户的签名现已贬值,并增加了新的签名,允许您使用新的签名。 `rclcpp::QoS` 和出版商/订阅选项类。

这些是新的和推荐的API:

``` cpp
template<
  typename MessageT,
  typename AllocatorT = std::allocator<void>,
  typename PublisherT = ::rclcpp::Publisher<MessageT, AllocatorT>>
std::shared_ptr<PublisherT>
create_publisher(
  const std::string & topic_name,
  const rclcpp::QoS & qos,
  const PublisherOptionsWithAllocator<AllocatorT> & options =
  PublisherOptionsWithAllocator<AllocatorT>()
);

template<
  typename MessageT,
  typename CallbackT,
  typename AllocatorT = std::allocator<void>,
  typename SubscriptionT = rclcpp::Subscription<
    typename rclcpp::subscription_traits::has_message_type<CallbackT>::type, AllocatorT>>
std::shared_ptr<SubscriptionT>
create_subscription(
  const std::string & topic_name,
  const rclcpp::QoS & qos,
  CallbackT && callback,
  const SubscriptionOptionsWithAllocator<AllocatorT> & options =
  SubscriptionOptionsWithAllocator<AllocatorT>(),
  typename rclcpp::message_memory_strategy::MessageMemoryStrategy<
    typename rclcpp::subscription_traits::has_message_type<CallbackT>::type, AllocatorT
  >::SharedPtr
  msg_mem_strat = nullptr);
```

这等人,确是堕落的。

``` cpp
template<
  typename MessageT,
  typename AllocatorT = std::allocator<void>,
  typename PublisherT = ::rclcpp::Publisher<MessageT, AllocatorT>>
[[deprecated("use create_publisher(const std::string &, const rclcpp::QoS &, ...) instead")]]
std::shared_ptr<PublisherT>
create_publisher(
  const std::string & topic_name,
  size_t qos_history_depth,
  std::shared_ptr<AllocatorT> allocator);

template<
  typename MessageT,
  typename AllocatorT = std::allocator<void>,
  typename PublisherT = ::rclcpp::Publisher<MessageT, AllocatorT>>
[[deprecated("use create_publisher(const std::string &, const rclcpp::QoS &, ...) instead")]]
std::shared_ptr<PublisherT>
create_publisher(
  const std::string & topic_name,
  const rmw_qos_profile_t & qos_profile = rmw_qos_profile_default,
  std::shared_ptr<AllocatorT> allocator = nullptr);

template<
  typename MessageT,
  typename CallbackT,
  typename Alloc = std::allocator<void>,
  typename SubscriptionT = rclcpp::Subscription<
    typename rclcpp::subscription_traits::has_message_type<CallbackT>::type, Alloc>>
[[deprecated(
  "use create_subscription(const std::string &, const rclcpp::QoS &, CallbackT, ...) instead"
)]]
std::shared_ptr<SubscriptionT>
create_subscription(
  const std::string & topic_name,
  CallbackT && callback,
  const rmw_qos_profile_t & qos_profile = rmw_qos_profile_default,
  rclcpp::callback_group::CallbackGroup::SharedPtr group = nullptr,
  bool ignore_local_publications = false,
  typename rclcpp::message_memory_strategy::MessageMemoryStrategy<
    typename rclcpp::subscription_traits::has_message_type<CallbackT>::type, Alloc>::SharedPtr
  msg_mem_strat = nullptr,
  std::shared_ptr<Alloc> allocator = nullptr);

template<
  typename MessageT,
  typename CallbackT,
  typename Alloc = std::allocator<void>,
  typename SubscriptionT = rclcpp::Subscription<
    typename rclcpp::subscription_traits::has_message_type<CallbackT>::type, Alloc>>
[[deprecated(
  "use create_subscription(const std::string &, const rclcpp::QoS &, CallbackT, ...) instead"
)]]
std::shared_ptr<SubscriptionT>
create_subscription(
  const std::string & topic_name,
  CallbackT && callback,
  size_t qos_history_depth,
  rclcpp::callback_group::CallbackGroup::SharedPtr group = nullptr,
  bool ignore_local_publications = false,
  typename rclcpp::message_memory_strategy::MessageMemoryStrategy<
    typename rclcpp::subscription_traits::has_message_type<CallbackT>::type, Alloc>::SharedPtr
  msg_mem_strat = nullptr,
  std::shared_ptr<Alloc> allocator = nullptr);
```

------------------------------------------------------------------------

QoS的通过方式的改变最有可能影响用户.

一个出版商的典型变化是这样的:

``` diff
- pub_ = create_publisher<std_msgs::msg::String>("chatter");
+ pub_ = create_publisher<std_msgs::msg::String>("chatter", 10);
```

还有订阅费:

``` diff
- sub_ = create_subscription<std_msgs::msg::String>("chatter", callback);
+ sub_ = create_subscription<std_msgs::msg::String>("chatter", 10, callback);
```

如果你不知道该用什么深度, 也不在乎现在(也许只是原型), `10`,因为那是之前的默认,应该保留现有的行为.

将提供更多关于如何选择适当深度的深度文件。

以避免新贬值的API的改变:

``` diff
- // Creates a latched topic
- rmw_qos_profile_t qos = rmw_qos_profile_default;
- qos.depth = 1;
- qos.durability = RMW_QOS_POLICY_DURABILITY_TRANSIENT_LOCAL;
-
  model_xml_.data = model_xml;
  node_handle->declare_parameter("robot_description", model_xml);
  description_pub_ = node_handle->create_publisher<std_msgs::msg::String>(
-   "robot_description", qos);
+   "robot_description",
+   // Transient local is similar to latching in ROS 1.
+   rclcpp::QoS(1).transient_local());
```

请参看引入 QoS 更改的牵引请求(和连接的牵引请求) 更多示例和细节 :

- <https://github.com/ros2/rclcpp/pull/713>

  - <https://github.com/ros2/demos/pull/332>

  - <https://github.com/ros2/robot_state_publisher/pull/19>

  - 还有其他人...

<span id="changes-due-to-declare-parameter-change"></span>

#### 因宣布参数更改而发生的变更

关于实际行为变化的详细情况,参见: [宣告参数](#declaring-parameters) 页:1

有好几个新的API呼叫在 `rclcpp::Node`界面 :

- 宣告参数给定名称、可选默认值、可选描述符并返回实际设定值的方法:

  ``` c++
  const rclcpp::ParameterValue &
  rclcpp::Node::declare_parameter(
    const std::string & name,
    const rclcpp::ParameterValue & default_value = rclcpp::ParameterValue(),
    const rcl_interfaces::msg::ParameterDescriptor & parameter_descriptor =
    rcl_interfaces::msg::ParameterDescriptor());

  template<typename ParameterT>
  auto
  rclcpp::Node::declare_parameter(
    const std::string & name,
    const ParameterT & default_value,
    const rcl_interfaces::msg::ParameterDescriptor & parameter_descriptor =
    rcl_interfaces::msg::ParameterDescriptor());

  template<typename ParameterT>
  std::vector<ParameterT>
  rclcpp::Node::declare_parameters(
    const std::string & namespace_,
    const std::map<std::string, ParameterT> & parameters);

  template<typename ParameterT>
  std::vector<ParameterT>
  rclcpp::Node::declare_parameters(
    const std::string & namespace_,
    const std::map<
      std::string,
      std::pair<ParameterT, rcl_interfaces::msg::ParameterDescriptor>
    > & parameters);
  ```

- 解密参数和检查参数是否已被宣布的方法 :

  ``` c++
  void
  rclcpp::Node::undeclare_parameter(const std::string & name);

  bool
  rclcpp::Node::has_parameter(const std::string & name) const;
  ```

- 一些以前不存在的便利方法:

  ``` c++
  rcl_interfaces::msg::SetParametersResult
  rclcpp::Node::set_parameter(const rclcpp::Parameter & parameter);

  std::vector<rclcpp::Parameter>
  rclcpp::Node::get_parameters(const std::vector<std::string> & names) const;

  rcl_interfaces::msg::ParameterDescriptor
  rclcpp::Node::describe_parameter(const std::string & name) const;
  ```

- 新的设置回调方法, 每当一个参数被更改时, 都会被更改, 让你有机会拒绝它 :

  ``` c++
  using OnParametersSetCallbackType =
    rclcpp::node_interfaces::NodeParametersInterface::OnParametersSetCallbackType;

  OnParametersSetCallbackType
  rclcpp::Node::set_on_parameters_set_callback(
    OnParametersSetCallbackType callback);
  ```

还有几种贬值的方法:

> ``` c++
> template<typename ParameterT>
> [[deprecated("use declare_parameter() instead")]]
> void
> rclcpp::Node::set_parameter_if_not_set(
>   const std::string & name,
>   const ParameterT & value);
>
> template<typename ParameterT>
> [[deprecated("use declare_parameters() instead")]]
> void
> rclcpp::Node::set_parameters_if_not_set(
>   const std::string & name,
>   const std::map<std::string, ParameterT> & values);
>
> template<typename ParameterT>
> [[deprecated("use declare_parameter() and it's return value instead")]]
> void
> rclcpp::Node::get_parameter_or_set(
>   const std::string & name,
>   ParameterT & value,
>   const ParameterT & alternative_value);
>
> template<typename CallbackT>
> [[deprecated("use set_on_parameters_set_callback() instead")]]
> void
> rclcpp::Node::register_param_change_callback(CallbackT && callback);
> ```

<span id="memory-strategy"></span>

#### 记忆战略

接口 `rclcpp::memory_strategy::MemoryStrategy` 正在使用类型def `WeakNodeVector` 在各种方法签名中。至 Dashing 时,类型def 已被更改为 `WeakNodeList` 并以此为不同方法中的参数类型。任何自定义内存策略都需要更新,以匹配修改过的界面。

有关API的修改请参见: [ros2/rclcpp#741](https://github.com/ros2/rclcpp/pull/741).

<span id="rclcpp-components"></span>

### rclcpp_components

在Dashing中实施构成的正确方式是: `rclcpp_components` 软件包。

为了正确执行运行时间构成,必须对节点进行以下修改:

节点必须有一个 构造器需要 `rclcpp::NodeOptions`:

``` cpp
class Listener: public rclcpp::Node {
  Listener(const rclcpp::NodeOptions & options)
  : Node("listener", options)
  {
  }
};
```

C++ 注册宏( 如果有) 需要更新以使用 `rclcpp_components` 如果不存在,则必须在一个翻译单元中添加注册宏。

``` cpp
// Insert at bottom of translation unit, e.g. listener.cpp
#include "rclcpp_components/register_node_macro.hpp"
// Use fully-qualifed name in registration
RCLCPP_COMPONENTS_REGISTER_NODE(composition::Listener);
```

CMake 注册宏(如果有的话)需要更新。如果没有,注册宏必须添加到项目的 CMake 中。

``` cmake
add_library(listener src/listener.cpp)
rclcpp_components_register_nodes(listener "composition::Listener")
```

有关组成情况的更多信息,见: [教程](../Tutorials/Intermediate/Writing-a-Composable-Node.md)

<span id="rclpy"></span>

### rclpy

<span id="changes-to-creating-publishers-subscriptions-and-qos-profiles"></span>

#### 创建出版商、订阅和 QoS 配置文件的更改

在达兴之前,你可以选择提供 `QoSProfile` 对象在创建发布器或订阅时。为了鼓励用户为信件队列指定历史深度,我们现在 **需求** 深度值,或 `QoSProfile` 对象在创建出版商或订阅时给出。

要创建出版社,你以前会写:

``` python
node.create_publisher(Empty, 'chatter')
# Or using a keyword argument for QoSProfile
node.create_publisher(Empty, 'chatter', qos_profile=qos_profile_sensor_data)
```

在 Dashing 中,偏好提供深度值的下列 API 或 `QoSProfile` 对象作为第三个位置参数 :

``` python
# Assume a history setting of KEEP_LAST with depth 10
node.create_publisher(Empty, 'chatter', 10)
# Or pass a QoSProfile object directly
node.create_publisher(Empty, 'chatter', qos_profile_sensor_data)
```

同样,对于订阅,你以前会写:

``` python
node.create_subscription(BasicTypes, 'chatter', lambda msg: print(msg))
# Or using a keyword argument for QoSProfile
node.create_subscription(BasicTypes, 'chatter', lambda msg: print(msg), qos_profile=qos_profile_sensor_data)
```

在达兴:

``` python
# Assume a history setting of KEEP_LAST with depth 10
node.create_subscription(BasicTypes, 'chatter', lambda msg: print(msg), 10)
# Or pass a QoSProfile object directly
node.create_subscription(BasicTypes, 'chatter', lambda msg: print(msg), qos_profile_sensor_data)
```

为方便过渡,不使用新API的用户会看到贬值警告.

此外,我们还要求在建造时, `QoSProfile` 对象为设置历史政策和/或深度。如果历史政策 `KEEP_LAST` 提供深度参数。例如,这些调用是有效的:

``` python
QoSProfile(history=QoSHistoryPolicy.RMW_QOS_POLICY_HISTORY_KEEP_ALL)
QoSProfile(history=QoSHistoryPolicy.RMW_QOS_POLICY_HISTORY_KEEP_LAST, depth=10)
QoSProfile(depth=10)  # equivalent to the previous line
```

这些电话将发出警告:

``` python
QoSProfile()
QoSProfile(reliability=QoSReliabilityPolicy.RMW_QOS_POLICY_RELIABILITY_BEST_EFFORT)
# KEEP_LAST but no depth
QoSProfile(history=QoSHistoryPolicy.RMW_QOS_POLICY_HISTORY_KEEP_LAST)
```

详情请见与引入这一改动有关的问题和拉动请求:

- <https://github.com/ros2/rclpy/issues/342>

- <https://github.com/ros2/rclpy/pull/344>

<span id="id2"></span>

#### 因宣布参数更改而发生的变更

关于实际行为变化的详细情况,参见: [宣告参数](#declaring-parameters) 这些变化与上文中的变化类似。 `rclcpp`.

这些是现有的新的API方法。 `rclpy.node.Node` 接口 :

- 要声明给定名称的参数,可选的默认值(由 `rcl_interfaces.msg.ParameterValue`)和一个可选描述符,返回实际设定的值:

  ``` python
  def declare_parameter(
      name: str,
      value: Any = None,
      descriptor: ParameterDescriptor = ParameterDescriptor()
  ) -> Parameter

  def declare_parameters(
    namespace: str,
    parameters: List[Union[
        Tuple[str],
        Tuple[str, Any],
        Tuple[str, Any, ParameterDescriptor],
    ]]
  ) -> List[Parameter]
  ```

- 解密先前已宣布的参数,并检查是否已事先宣布一个参数:

  ``` python
  def undeclare_parameter(name: str) -> None

  def has_parameter(name: str) -> bool
  ```

- 要获取和设置参数描述符 :

  ``` python
  def describe_parameter(name: str) -> ParameterDescriptor

  def describe_parameters(names: List[str]) -> List[ParameterDescriptor]

  def set_descriptor(
      name: str,
      descriptor: ParameterDescriptor,
      alternative_value: Optional[ParameterValue] = None
  ) -> ParameterValue
  ```

- 获取可能尚未宣布的参数的方便方法 :

  ``` python
  def get_parameter_or(name: str, alternative_value: Optional[Parameter] = None) -> Parameter
  ```

<span id="other-changes"></span>

#### 其他变动

`rclpy.parameter.Parameter` 现在可以猜测它的型号,而无需明确设置它(只要它是支持的型号之一) `rcl_interfaces.msg.ParameterValue`。例如,此代码:

> ``` python
> p = Parameter('myparam', Parameter.Type.DOUBLE, 2.41)
> ```

相当于此代码 :

> ``` python
> p = Parameter('myparam', value=2.41)
> ```

此更改不会打破已有的 API 。

<span id="rosidl"></span>

### 罗西德

直到使用 Crystal 的每个消息生成器包自行注册 `ament_cmake` 扩展点 `rosidl_generate_interfaces` 并被通过 一组 `.msg` / `.srv` / `.action` 文件。到 Dashing 时,信件生成管道基于 `.idl` 替换文件。

任何信件生成器包都需要使用新的扩展点来更改和注册 `rosidl_generate_idl_interfaces` 仅通过 `.idl` 文件。通常支持的语言 C、 C++ 和 Python 的邮件生成器,以及内视、快速RTPS、Connext 和 OpenSplices 的类型支持软件包已经更新(参见 [ros2/rosidl#334](https://github.com/ros2/rosidl/pull/334/files)。CMake 代码调用 `rosidl_generate_interfaces()` 也可以通过 `.idl` 直接或通过文件 `.msg` / `.srv` / `.action` 然后在内部转换为 `.idl` 在传递到每个信件生成器之前的文件 。

格式 `.msg` / `.srv` / `.action` 文件不会在将来被演化。 `.msg` / `.srv` / `.action` 文档和 `.idl` 文件描述于 [此设计文章](https://design.ros2.org/articles/legacy_interface_definition.html). A [第二条 设计条款](https://design.ros2.org/articles/idl_interface_definition.html) 描述支持的特性 `.idl` 文件。为了利用现有任何新功能,需要转换接口(例如使用命令行工具) `msg2idl` / `srv2idl` / `action2idl`).

要区分相同的类型名称,但名称空间不同,内观结构现在包含一个取代软件包名称的命名空间字段(参见 [ros2/rosidl#335](https://github.com/ros2/rosidl/pull/355/files)).

<span id="mapping-of-char-in-msg-files"></span>

#### .msg文件中的字符映射

内 [ROS 1](https://wiki.ros.org/msg#Fields) `char` 已长期贬值,并正在绘制到 `uint8`在 ROS 2 中直到 Crystal `char` 被映射为单个字符( E)`char` 在C / C++中, `str` 为了提供更自然的绘图,ROS 1语义已经恢复, `char` 地图到 `uint8` 再来一次

<span id="rosidl-generator-cpp"></span>

### rosidl_generator_cpp

为消息、服务和动作生成的 C++ 数据结构为每个字段提供了设置方法。 直到 Crystal 每个设置器返回一个指针到数据结构本身, 以启用指定的参数 idiom 。 至于 Dashing 这些设置器 [返回一个引用](https://github.com/ros2/rosidl/pull/353) 更何况这似乎是更常见的签名, `nullptr`.

<span id="rosidl-generator-py"></span>

### rosidl_generator_py

在Crystal之前,电文中的数组(固定大小)或序列(动态大小,可选用上边界)字段被存储为 `list` 在 Python 中。关于数组/数组序列的 Python 类型已更改:

- 数组数值作为 `numpy.ndarray` (单位:千美元) `dtype` 为匹配数值类型而选择的)

- 数字值的序列被存储为 `array.array` (单位:千美元) `typename` 为匹配数值类型而选择的)

和以前一样,非数字类型的数组/序列仍作为 `list` 在Python语中。

这一变化带来若干好处:

- 新的数据结构确保阵列 / 序列中的每个项目都符合数值类型的值范围限制.

- 数字值可以更高效地存储在内存中,这可以避免Python对象对每个项目的管理.

- 两种数据结构的内存布局允许在单项操作中读写数组/序列的所有项目,这使得从Python转换到Python的速度显著加快/效率更高.

<span id="launch"></span>

### 发射

那个... `launch_testing` 套件夹上 `launch` 在 Bouncy Bolson 中完成的软件包重新设计。遗留的 Python API 已移入 `launch.legacy` 子模块,因此已贬值并删除。

见 `launch` [实例](https://github.com/ros2/launch/tree/dashing/launch/examples) 财务报告和财务报告 [文档](https://github.com/ros2/launch/tree/dashing/launch/doc) 用于参考如何使用新的API。

见 [演示测试](https://github.com/ros2/demos) 如何使用新的 `launch_testing` API. (英语).

<span id="rmw"></span>

### rmw (英语).

自《公约》生效以来的变化 [水晶克莱米斯](Release-Crystal-Clemmys.md) 释放 :

- 新建 API 中 `rmw`,用于 `rmw_context_t`:

> - [rmw_context_fini](https://github.com/ros2/rmw/blob/c518842f6f82910482470b40c221c268d30691bd/rmw/include/rmw/init.h#L111-L136)

- 修改 `rmw`,现在通过 `rmw_context_t` 改为: `rmw_create_wait_set`:

> - [rmw_create_wait_set](https://github.com/ros2/rmw/blob/c518842f6f82910482470b40c221c268d30691bd/rmw/include/rmw/rmw.h#L522-L543)

- 新建 API 中 `rmw` 用于预排已发布和已订阅信件的空间:

> - [rmw_init_publisher_allocation](https://github.com/ros2/rmw/blob/dc7b2f49f1f961d6cf2c173adc54736451be8938/rmw/include/rmw/rmw.h#L262)
>
> - [rmw_fini_publisher_allocation](https://github.com/ros2/rmw/blob/dc7b2f49f1f961d6cf2c173adc54736451be8938/rmw/include/rmw/rmw.h#L279)
>
> - [rmw_init_subscription_allocation](https://github.com/ros2/rmw/blob/dc7b2f49f1f961d6cf2c173adc54736451be8938/rmw/include/rmw/rmw.h#L489)
>
> - [rmw_fini_subscription_allocation](https://github.com/ros2/rmw/blob/dc7b2f49f1f961d6cf2c173adc54736451be8938/rmw/include/rmw/rmw.h#L506)
>
> - [rmw_serialized_message_size](https://github.com/ros2/rmw/blob/dc7b2f49f1f961d6cf2c173adc54736451be8938/rmw/include/rmw/rmw.h#L395)

- 修改 `rmw`,现在通过 `rmw_publisher_allocation_t` 或 时 间 `rmw_subscription_allocation_t` 改为: `rmw_publish` 财务报告和财务报告 `rmw_take`请注意,这一论点可以是: `NULL` 或 时 间 `nullptr`,保持已有的Crystal行为。

> - [rmw_publish](https://github.com/ros2/rmw/blob/dc7b2f49f1f961d6cf2c173adc54736451be8938/rmw/include/rmw/rmw.h#L310)
>
> - [rmw_take](https://github.com/ros2/rmw/blob/dc7b2f49f1f961d6cf2c173adc54736451be8938/rmw/include/rmw/rmw.h#L556)

- 类型名称返回 `rmw_get_*_names_and_types*` 函数应该有一个完全合格的命名空间。例如,而不是 `rcl_interfaces/Parameter` 财务报告和财务报告 `rcl_interfaces/GetParameters`,返回的类型名称应为 `rcl_interface/msg/Parameter` 财务报告和财务报告 `rcl_interfaces/srv/GetParameters`.

<span id="actions"></span>

### 动作

- 变动至 `rclcpp_action::Client` 签名 :

  签署: [rclcpp_action::Client::async_send_goal](https://github.com/ros2/rclcpp/blob/ef41059a751702274667e2164182c062b47c453d/rclcpp_action/include/rclcpp_action/client.hpp#L343) 已更改。现在用户可以选择为该程序提供回调功能。 **目标响应** 页:1 **结果** 使用新软件 [发送目标选项](https://github.com/ros2/rclcpp/blob/ef41059a751702274667e2164182c062b47c453d/rclcpp_action/include/rclcpp_action/client.hpp#L276) struct. 当一个动作服务器接受或拒绝目标时调用目标响应回调, 并且收到目标结果时调用结果响应回调。 还可以选择回调。 [rclcpp_action::Client::async_cancel_goal](https://github.com/ros2/rclcpp/blob/ef41059a751702274667e2164182c062b47c453d/rclcpp_action/include/rclcpp_action/client.hpp#L432-L434) 财务报告和财务报告 [rclcpp_action::Client::async_get_result](https://github.com/ros2/rclcpp/blob/ef41059a751702274667e2164182c062b47c453d/rclcpp_action/include/rclcpp_action/client.hpp#L399-L401).

- 目标过渡名称的更改 :

  目标状态过渡的名称已重构以反映设计文档。 这会影响 `rcl_action`, `rclcpp_action`,以及 `rclpy`。这里列出事件名称的更改(*旧名称 - \> 新名称*):

  - 目标_EVENT_CANT- \> 目标_EVENT_CANT-GOAL

  - 目标_EVENT_SET_SUCED - \> 目标_EVENT_SUCED

  - 目标_EVENT_SET_ABORT - \> 目标_EVENT_ABORT

  - 目标 \_ 目标 \_ 目标 \_ 目标 \_ 目标 \_ 目标 \_ 目标

- 变动至 `CancelGoal.srv`:

  A `return_code` 字段已添加到 `CancelGoal` 服务。这是为了更好地传达服务失败的原因。请参见 [拉动请求](https://github.com/ros2/rcl_interfaces/pull/76) 和关联问题的细节。

<span id="rviz"></span>

### rviz 维兹

- 插件应使用完全合格的类型名称,否则会记录一个警告。 [实例](https://github.com/ros2/rviz/blob/dfceae319d49546f1e4ad39689853c18fef0001e/rviz_default_plugins/plugins_description.xml#L13),使用类型 `sensor_msgs/msg/Image` 改为 `sensor_msgs/Image`。见 [实行这一改革的公关](https://github.com/ros2/rviz/pull/387) 更多细节。

<span id="known-issues"></span>

## 已知问题

- [\[ros2/rclcpp#715\]](https://github.com/ros2/rclcpp/issues/715) 参数YAML文件在独立ROS 2节点和组成ROS 2节点之间加载的方式不一致。 [问题评论](https://github.com/ros2/rclcpp/issues/715#issuecomment-497392626)

- [\[ros2/rclpy#360\]](https://github.com/ros2/rclpy/issues/360) 忽略rclpy 节点 <span class="kbd kbd docutils literal notranslate">缩略语</span>-<span class="kbd kbd docutils literal notranslate">c</span> 当在 Windows 上使用 OpenSplice 时。

- [\[ros2/rosidl_typesupport_opensplice#30\]](https://github.com/ros2/rosidl_typesupport_opensplice/issues/30) 使用 OpenSplices 时, 服务或动作定义内有防止嵌入信件的错误 。

- [\[ros2/rclcpp#781\]](https://github.com/ros2/rclcpp/pull/781) 调用 `get_parameter`/`list_parameter` 从内部 `on_set_parameter_callback` 这会给Dashing造成僵局。这对Elopeent来说是固定的,但是是ABI的突破,所以没有被送回Dashing。

- [\[ros2/rclcpp#912\]](https://github.com/ros2/rclcpp/issues/912) 当进程内部通信发生时,进程间通信会迫使信件副本 `std::unique_ptr` 出版商和独家 `std::unique_ptr` 订阅(出版) `std::unique_ptr` 内部晋升为 `std::shared_ptr`).

- [\[ros2/rosbag2#125\]](https://github.com/ros2/rosbag2/issues/125) 没有记录QOS不可靠的话题.

- [\[ros2/rclcpp#715\]](https://github.com/ros2/rclcpp/issues/715) 可编译节点无法通过重映射接收参数。 使用其中描述的方法可以完成向可编译节点提供参数的工作。 [\[本评论\]](https://github.com/ros2/rclcpp/issues/715#issuecomment-497392626).

- [\[ros2/rclcpp#893\]](https://github.com/ros2/rclcpp/issues/893) `rclcpp::Context` 未销毁,因为参考周期 `rclcpp::GraphListener`。这会导致记忆漏水。由于可能破坏ABI,一个固定设备没有被回放。

<span id="timeline-before-the-release"></span>

## 发布前的时间线

发布前的几个里程碑:

> 4月8日(阿尔法)  
> 已有的核心软件包的首次发布。 测试可以从现在开始进行( 有些功能可能还没有落地 ) 。
>
> Thu. 5月2日 (中文(简体) ).  
> 核心软件包的 API 冻结
>
> 5月6日(百达).  
> 已更新的核心软件包的发布。 对最新特性进行额外测试 。
>
> Thu. 5月16日 (中文(简体) ).  
> 特性冻结 。 在此点之后, 只能进行错误修复释放 。 新的软件包可以独立发布 。
>
> 5月20日(释放候选人)  
> 现有核心软件包的最新发布情况。
>
> 5月29日结婚  
> 冻结rodistro. rodistro repo上的Dashing不进行PRs合并(发布公告后重新开放).
