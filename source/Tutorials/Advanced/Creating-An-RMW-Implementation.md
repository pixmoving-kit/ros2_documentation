---
translation_status: machine_translated
source: Tutorials/Advanced/Creating-An-RMW-Implementation.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="creating-an-rmw-implementation"></span>

# 创建一个 `rmw` 执行

**目标：** 学习如何创建新 `rmw` 执行,从基本中间软件所需的特性到 `rmw` 执行细节。

**教程级别：** 高级

**用时：** 30分钟+分钟

<span id="introduction"></span>

## 导言

ROS 2 的建筑主要有两个 [抽象层](../../Concepts/Advanced/About-Internal-Interfaces.md)。从上到下:

1.  客户端库界面, `rcl`,支持用户界面 [客户端库](../../Concepts/Basic/About-Client-Libraries.md),例如, `rclcpp` 财务报告和财务报告 `rclpy`

2.  中间软件接口, `rmw`,将其中的 [基本中间软件执行](../../Concepts/Intermediate/About-Different-Middleware-Vendors.md),例如特定的DDS执行,Zenoh等.

那个... `rmw` [API 包含函数级文档](https://docs.ros.org/en/rolling/p/rmw/generated/index.html#functions),但对于界面的特性以及它从基础中间软件中期望的,没有更高层次的文档.

本指南是针对想要执行此操作的开发者的。 `rmw` 对特定中间软件的接口。它首先要检查 `rmw` 界面和操作方式。然后,它将涵盖一个中间软件执行必须支持的主要概念或特征。最后,它将讨论一些执行细节,包括如何创建执行骨架和执行接口功能的一些提示。

本指南旨在成为启动开发新的 `rmw` 它将与其他页面和源代码链接,以便酌情提供更多细节。

> **说明**
>
> ROS 2号设计文章 [设计. ros2.org.](https://design.ros2.org/) 它们是历史文件,可能不反映ROS 2.的现状,但在某些情况下,它们提供了有用的上下文和信息,因此本指南或本指南链接的页仍然可以参考它们。

<span id="the-rmw-interface"></span>

## 那个... `rmw` 接口

那个... `rmw` 接口由 `rmw` 软件包通过 [C 页眉文件](https://github.com/ros2/rmw/tree/rolling/rmw/include/rmw)执行这些标题中宣布的C职能的情况由下列机构提供: `rmw` 执行,它们是单独的软件包。例如, `rmw_fastrtps_cpp` 软件包执行eProsima Fast DDS的接口。

<span id="example-implementations"></span>

### 执行实例

以下内容: `rmw` [执行情况](../../Concepts/Advanced/About-Middleware-Implementations.md) 请注意,有不同的 [由2000年《环境方案》界定的支助级别](https://reps.openrobotics.org/rep-2000/#support-tiers).

1.  DDS : (英语).

    > 1.  `rmw_fastrtps_cpp`, `rmw_fastrtps_dynamic_cpp`: [ros2/rmw_fastrtps](https://github.com/ros2/rmw_fastrtps)
    >
    > 2.  `rmw_cyclonedds_cpp`: [ros2/rmw_cyclonedds](https://github.com/ros2/rmw_cyclonedds)
    >
    > 3.  `rmw_connextdds`: [ros2/rmw_connextdds](https://github.com/ros2/rmw_connextdds)
    >
    > 4.  `rmw_gurumdds_cpp`: [ros2/rmw_gurumdds](https://github.com/ros2/rmw_gurumdds)
    >
    > - 见 [本概览](../../Concepts/Advanced/About-Middleware-Implementations.md#about-middleware-impls-struct-dds)

2.  `rmw_zenoh_cpp`: [ros2/rmw_zenoh](https://github.com/ros2/rmw_zenoh)

    > - 见 [设计文件](https://github.com/ros2/rmw_zenoh/blob/rolling/docs/design.md)

3.  `rmw_email_cpp`,基于电子邮件的执行: [christophebedard/rmw_email](https://github.com/christophebedard/rmw_email)

    > - 见 [基本电子邮件中间软件的设计文件](https://christophebedard.com/rmw_email/design/email/) 页:1 [一些背景的博客文章](https://christophebedard.com/ros-2-over-email/)

<span id="build-time-and-runtime-rmw-implementation-selection-mechanism"></span> <span id="rmw-impl-guide-selection-mechanism"></span>

### 构建时间和运行时间 `rmw` 执行选择机制

对实际开支的依赖 `rmw` 执行是通过下列方式进行的: `rmw_implementation` [软件包](https://index.ros.org/p/rmw_implementation/#rolling)用户。 `rmw`,例如, `rcl`,则取决于 `rmw` 用于接口(标题)和一些公用功能的软件包。它们也取决于 `rmw_implementation` 以获得实际执行。

默认情况下,ROS 2允许您选择哪个 `rmw` 执行在运行时使用 。 比较同一机上的两个执行是方便的, 它允许 ROS 2 分发一组与多个兼容的二进制 `rmw` 执行。 [运行时选择执行](../../How-To-Guides/Working-with-multiple-RMW-implementations.md) 通过 `RMW_IMPLEMENTATION` 环境变量,或者,如果该变量未设置,则默认 `rmw` 已装入执行 。

这项工作由下列机构完成: `rmw_implementation` 软件包,该软件包作为实际 `rmw` 执行。它通过创建占位符发挥作用 `rmw` 函数。当它们被调用时,它会 `dlopen()` 选定对象的合适库 `rmw` 执行,然后在装入的共享库中使用 `dlsym()` 函数,然后调用它们。

那个... `rmw_implementation` 软件包可以在构建时配置,以更改默认选项或禁用运行时选择。默认执行可以在构建时随构建时选择。 `RMW_IMPLEMENTATION` CMake变量(例如, `-DRMW_IMPLEMENTATION=rmw_other`)或为: `RMW_IMPLEMENTATION` 环境变量。如果在构建时只有一个执行,或者在运行时选择被禁用(`-DRMW_IMPLEMENTATION_DISABLE_RUNTIME_SELECTION=ON`),则 `rmw_implementation` 目标将是一个简单的 `INTERFACE` 用于单一执行的库。

由于上述代理机制和CMake逻辑,a `rmw` 不执行接口中所有函数的执行,只有在运行时才会失败,当符号搜索失败时才会失败,而如果运行时选择被禁用,则在构建时(特别是在链接时)才会失败。

<span id="features"></span>

## 特征

本节回顾了《公约》的主要特点。 `rmw` 接口,基础的中间软件必须支持或处理的接口。取决于中间软件 — 以及它与接口预期的特性的相似程度。 `rmw` 执行可能或多或少是微不足道的,即它可能必须做更多的“光滑”工作。 对于一些非关键特性或配置选项,执行可以表明它们没有通过下列方式得到支持: `rmw_feature_supported()` 或以返回方式 `RMW_RET_UNSUPPORTED`。无论如何,任何特殊行为, `rmw` 最好能将执行情况记录在案。

<span id="topics-pub-sub-services"></span>

### 专题、酒吧/分公司、服务

[话题](../../Concepts/Basic/About-Topics.md) 是出版/订阅中间软件中常见的概念。然而,ROS 2 有自己的主题名称公约,使用 `rmw_validate_full_topic_name()`。该词 `rmw` 执行只需使用给定(已解决)的主题名称。这可能涉及修改或操纵ROS的主题名称,以适应中枢软件的主题名称的常规或限制,或编码有用的信息。例如,一个叫做 pub/sub 的专题。 `/chatter` 通常被掺入 `rt/chatter` 用于基于 DDS 的应用,使关于 DDS 的ROS 主题容易与普通 DDS 主题区分。 [本设计文件“将ROS 2主题和服务名称绘制到DDS概念”部分](https://design.ros2.org/articles/topic_and_service_names.html#mapping-of-ros-2-topic-and-service-names-to-dds-concepts)。对于 Zenoh ,域名ID、已解决的主题名称、主题类型名称和主题类型散列是 [编码在底端的 Zenoh 密钥中](https://github.com/ros2/rmw_zenoh/blob/rolling/docs/design.md#topic-and-service-name-mapping-to-zenoh-key-expressions) 以避免不同ROS主题名称和类型之间的通信。

至于 [服务](../../Concepts/Basic/About-Services.md),它们并不总是由内在的中间软件支持。对于基于 DDS 的执行,它们只是建立在 pub/ sub 之上: 1 请求主题和 1 响应主题。 <span id="id1"></span>[\[1\]](#fn-dds-rpc) 另一方面,Zenoh在本地通过下列方式支持服务: [可查询](https://github.com/ros2/rmw_zenoh/blob/rolling/docs/design.md#service-servers),因此,它们被用于在下列领域提供服务: `rmw_zenoh_cpp`.

请注意,虽然服务是 `rmw` 接口, [动作](../../Concepts/Basic/About-Actions.md) 才不是,他们是一个 `rcl` 概念的实现 `rcl_action` 包装在服务和酒吧/分店的顶部。

<span id="nodes"></span>

### 节点

[节点](../../Concepts/Basic/About-Nodes.md) 大部分是ROS概念。DDS和Zenoh都没有相应的概念,因此它们大多是逻辑概念。 `rmw` 执行。如果需要,主题名称用节点名称空间/名称解决,请使用 `rcl` 在他们被传递到 `rmw` 当创建 pub/ sub 对象时。执行只需确保将节点包含在内 [内向数据](#rmw-impl-guide-introspection).

<span id="wait-sets-and-waiting"></span> <span id="rmw-impl-guide-waitsets"></span>

### 等待,等待,等待,等待,等待,等待,等待

[执行器](../../Concepts/Intermediate/About-Executors.md) 负责在新信件收到时触发用户提供的回调,例如,执行器在客户端库一级执行(`rclcpp`, `rclpy`)),但是它们依靠基础的中间软件使用投票机制等待新消息。这是使用等待器完成的,它允许以标准的方式同时等待不同的实体,例如订阅、服务客户端和服务服务器。 `rmw_wait()` [函数](https://docs.ros.org/en/rolling/p/rmw/generated/function_rmw_8h_1a5f480dd59075e80288fb596b2951be2b.html) 它会将所有实体添加到等待集中,并要求它等待至少一个实体有新的数据或数据过期。然后执行者会检查实体名单,看看哪些实体有新的数据可用,并触发相应的回调。

这里的关键机制是检查一个特定实体是否准备好的能力, 例如检查订阅是否有新消息。 然后等待只需持续检查一个实体, 直到一个实体准备好或等待时间结束 。

看看怎么样 `rmw_email_cpp` [执行等待设置和等待](https://github.com/christophebedard/rmw_email/blob/72742241d55f306d1dddcaf5dd6a5d6c2d402433/rmw_email_cpp/src/rmw_wait.cpp#L133) 挖到中间的器皿, `email`因为事情很简单。

<span id="taking-data"></span>

### 获取数据

一次 一次 一次 一次 [执行器已等待完成](#rmw-impl-guide-waitsets) 并有新的消息,请求,或响应,它从中间软件中提取并触发相应的回调。例如, `rmw_take()` 使用订阅和类型化指针来调用对应信件类型的实例。

`rmw_email_cpp` 从内置的电子邮件中软件订阅对象中取出新信件(YAML字符串),并将其转换为ROS消息,将其写入提供的消息中.

<span id="metadata-gids-timestamps-sequence-numbers"></span>

### 元数据: GID、时间戳、序列号

除了用户指定的实际数据外,信息出版物、服务请求、服务响应等也包含与之相关的元数据:

- GID: 识别一个实体( 例如 pub, sub, 客户端, 服务器) 的全球唯一ID

  > - 一个实体的GID应在ROS域内是独一无二的,在本地和远程上报时应该是一样的。例如,正在发布消息的出版商GID应该是在对方上报的同一位出版商GID,如果该消息是通过订阅收到的。 <span id="id2"></span>[\[2\]](#fn-gid-remote-matching)

- 来源和收到时间戳:分别是出版和订阅接收时间戳

- 出版和接收序列号

这意味着服务请求元数据包括提出请求的客户端的GID和请求序列号. 服务响应元数据还包括它响应的请求的客户端GID和序列号.

此元数据可通过结构通过 `rmw_take_with_info()` 用于订阅信件和 `rmw_take_{request,response}()` 用于服务请求/回复,由客户端库包裹并提供给用户回调。

部分元数据可能由内基中间软件提供本土支持和提供,而另一部分则可能需与应用程序数据一并列入和传输。 `rmw` 执行 。 例如, DDS 本地通过 DDS 样本信息支持所有 pub/ sub , 但客户端请求元数据需要与服务响应数据一起由 `rmw` 执行。 `email` 本地支持所有这些元数据,这些元数据包含在标准电子邮件头中(即不包含在电子邮件正文中).

<span id="type-support"></span> <span id="rmw-impl-guide-typesupport"></span>

### 类型支持

缩小ROS 2之间的差距 [接口](../../Concepts/Basic/About-Interfaces.md) (具体为: [自定义接口](../Beginner-Client-Libraries/Custom-ROS2-Interfaces.md))和内置中间软件,需要一些胶体代码。这被称为 [类型支持](../../Concepts/Advanced/About-Internal-Interfaces.md#type-specific-interfaces)。当发布类型信息时, [std_msgs/msg/String](https://docs.ros.org/en/rolling/p/std_msgs/msg/String.html), `rmw_publish()` 只得到一个 `void *` 到信件,可以指向 C++ 实例,或者 C 实例,等等。指针将根据出版商创建时所提供的类型支持信息来解释。

首先,为界面类型和用户界面语言的每种组合生成代码,独立于基本的中间软件。 [std_msgs/msg/String](https://docs.ros.org/en/rolling/p/std_msgs/msg/String.html) 消息类型, 数据结构生成 :

1.  C++: `std_msgs/msg/string.hpp` 页眉带有 `std_msgs::msg::String` 生成的类 `rosidl_generator_cpp` 软件包

2.  C: `std_msgs/msg/string.h` 页眉带有 `std_msgs__msg__String` 由该表生成结构 `rosidl_generator_c` 软件包

3.  ⁇ : `std_msgs` 模块 `std_msgs.msg.String` 类(这只是一个环绕 C 结构的包装) `rosidl_generator_py` 软件包

4.  (等等,例如,为Rust)

第二,要使基本中间软件能够发送和接收消息,它需要知道如何解释用户数据结构。这是最关键的部分之一。 `rmw` 有两种选择: [静态类型支持](../../Concepts/Advanced/About-Internal-Interfaces.md#internal-interfaces-static-type-support) 财务报告和财务报告 [动态类型支持](../../Concepts/Advanced/About-Internal-Interfaces.md#internal-interfaces-dynamic-type-support)。静态类型支持涉及为每个接口生成特定中间软件的代码。例如, `rosidl_typesupport_fastrtps_cpp` 使用 CDR 生成代码,将每种接口类型的 C++ 类序列化/去序列化为 CDR [快速CDR( 快速CDR)](https://github.com/eProsima/Fast-CDR) (单位:千美元) `rmw_fastrtps_cpp` 传递到快速DDS。 <span id="id4"></span>[\[3\]](#fn-ts-fastrtps) `rmw_connextdds` 连 `rmw_zenoh_cpp` 使用CDR进行序列化,所以他们也使用这种类型的支持包. 另一方面,动态类型支持涉及生成一些中枢软件的独立代码,提供每种界面类型的通用信息. <span id="id5"></span>[\[4\]](#fn-ts-dynamic)

此信息可在运行时被任意 `rmw` 执行来解释一个类型已过时的指针到数据:字段的名称和类型,根据类型从字段读取/写入到字段的函数,获得数组字段大小的函数等。对于 C++,这是 `rosidl_typesupport_introspection_cpp`,用于 `rmw_fastrtps_dynamic_cpp` (例如,“动力”部分。)

动态类型支持一般比运行时的静态类型支持要慢,因为它必须在每条消息字段上标出它是什么类型,然后进行处理,例如序列化。静态类型支持完全知道如何通过它为每条接口类型生成的代码来处理消息。这就是为什么大多数 `rmw` 执行时使用静态类型支持。然而,动态类型支持并不需要生成特定中间软件的代码。在静态类型支持和动态类型支持之间进行选择是正向决定。 `rmw` 执行本身。

`rmw_email_cpp` 使用动态类型支持将信件转换为通过电子邮件发送的 YAML 字符串。它得到类型支持的内存信息,并将其和信件传递给外部/实验包, [符号](https://github.com/osrf/dynamic_message_introspection/),将信件转换为/从 YAML 。然后,YAML 对象通过电子邮件以 YAML 格式化的字符串发送。当中间软件收到新信件时,YAML 字符串将转换成消息。

<span id="domain-id"></span>

### 域名标识

[域名标识](../../Concepts/Intermediate/About-Domain-ID.md) 是在同一物理网络上建立单独的逻辑网络的一种方法。它是DDS的本土特性,但不是Zenoh。DDS通过使用域ID作为网络端口抵消来实现这一点,而Zenoh则通过使域ID成为每个ROS 2主题对应的内部Zenoh密钥的第一个组件来实现这一点。

<span id="quality-of-service-qos"></span>

### 服务质量(质量)

[服务质量](../../Concepts/Intermediate/About-Quality-of-Service-Settings.md) 在ROS 2中,基本QoS政策主要来源于DDS。像历史、深度和耐久性这样的基本QoS政策与ROS 1政策相同,但更先进的政策只是来自DDS。执行可能只是忽略一些设置。例如, `rmw_zenoh_cpp` QoS政策不会执行期限和寿命。

QoS的一个重要方面是两种配置,如出版商的简介和订阅者简介,可能不兼容,意味着他们无法沟通。 两种QoS配置是否兼容,取决于执行: `rmw_qos_profile_check_compatible()`. 依赖基于DDS的应用程序 `rmw_dds_common::qos_profile_check_compatible()`,从 [QoS 配置文件兼容性](../../Concepts/Intermediate/About-Quality-of-Service-Settings.md#about-qos-compatibilities) 在DDS中是标准的。在Zenoh, [QoS 设置从本质上说从不兼容](https://github.com/ros2/rmw_zenoh/blob/rolling/docs/design.md#quality-of-service).

为了支持普遍的“违约”行为,QoS政策包括: `*_SYSTEM_DEFAULT` 设置(例如, `rmw_qos_reliability_policy_t`’s `RMW_QOS_POLICY_RELIABILITY_SYSTEM_DEFAULT`)),将值留给中间软件执行。 `rmw_*_get_actual_qos()` 函数获取执行所使用的实际 QoS 配置。

<span id="ros-graph-introspection"></span> <span id="rmw-impl-guide-introspection"></span>

### ROS 图表回顾

节点可以获取其他节点、主题等的列表。 这也可以让出版商知道其主题是否存在任何订阅, 例如。 此机制用于 [列表节点](../Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md), [话题](../Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md),等等与ROS 2 CLI:: `ros2 node list`, `ros2 topic list`, 等 (简体中文).

这一点得到若干非政府组织的支持。 `rmw` 函数 : `rmw_get_node_names()`, `rmw_get_topic_names_and_types()`, `rmw_publisher_count_matched_subscriptions()`,还有更多。虽然界面没有指定执行, `rmw` 执行通常维护 ROS 图的缓存。 当创建新实体( 如节点、 发布器、 订阅、 服务、 客户端) 时, 它们会在其内部图缓存中记下来, 并通过中间软件特定机制通知其他参与者, 以便将其添加到缓存中。 图表缓存属于 `rmw` 上下文,所以初始化为: `rmw_init()` 名称。此上下文间接属于 `rclcpp` 上下文(例如: `rclcpp::init()`),因此每个过程通常只有一个图缓存.

自DS系统以来 `rmw` 在这方面,执行非常相似,它们共享一个通用的图表缓存执行。 `rmw_dds_common` [软件包](https://github.com/ros2/rmw_dds_common)。它使用一个内部主题(通常是 `ros_discovery_info`分享有关新实体的信息。 `rmw_zenoh_cpp` [创建 Zenoh 活泼的标志](https://github.com/ros2/rmw_zenoh/blob/rolling/docs/design.md#graph-cache) 与实体类型信息并与其他参与者共享。

<span id="events"></span>

### 活动

用户可以对某些事件(但由客户端库执行)的中间软件所触发的出版商和订阅提供回调( C)`rmw_event_type_t`),例如, [与服务有关的活动的质量](../../Concepts/Intermediate/About-Quality-of-Service-Settings.md#about-qos-qos-events) 和 pub- sub 匹配事件。 其中一些事件可以在图缓存的相关更改中触发 。

<span id="security"></span>

### 安全

[安全](../../Concepts/Intermediate/About-Security.md) 具体内容不详细 `rmw` 接口; 大部分由 [SROS2 请检查url=值 (帮助)](Security/Introducing-ros2-security.md)。接口仅定义了几个安全选项,作为上下文初始化选项的一部分, `rmw_init_options_t`:

1.  `rmw_security_options_t`,它包括安全政策(强制/允许)和一条包含安全文物的目录的路径,即密钥。 `rcl` 基于环境变量: `ROS_SECURITY_ENABLE` & `ROS_SECURITY_STRATEGY` 财务报告和财务报告 `ROS_SECURITY_KEYSTORE`.

2.  从密钥托尔到给定进程使用的安全飞地的名称 。 例如, 此设定通过 `--enclave` 选项 `ros2 run`.

然而,在实践中,《公约》的结构是: [密钥托](Security/The-Keystore.md) 目录及其安全飞地以DDS安全规格为基础。 [生产的安全文物](Security/Introducing-ros2-security.md) 与 `sros2` 软件包只能由基于DS的软件包直接使用 `rmw` 执行。 `rmw_zenoh_cpp`, [Zenoh 特定安全配置文件可以生成](https://github.com/ros2/rmw_zenoh/tree/rolling/zenoh_security_tools) 从 `sros2`- 利用该设备生成的文物 `zenoh_security_tools` 软件包,并通过 `ZENOH_SESSION_CONFIG_URI` 环境变量,绕过 `ROS_SECURITY_*` 环境变量。

<span id="implementation"></span>

## 执行情况

<span id="implementation-skeleton"></span>

### 执行骨架

本节涵盖为新执行套件创建基础文件和目录的具体步骤,包括: `package.xml` 财务报告和财务报告 `CMakeLists.txt`.

开始于 [软件包创建教程](../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md) 以创建空软件包。然后进行以下修改:

1.  `package.xml`

    > 1.  定义软件包/ 执行名称
    >
    >     > 软件包名称也是 `rmw` 将用来: [选择执行](#rmw-impl-guide-selection-mechanism) 通过 `RMW_IMPLEMENTATION` 例如,环境变量或 CMake 选项。名称通常从 `rmw_` 后面是内置中间器件的名称。 [二. 实施《业务战略》2的生态系统](../../Concepts/Intermediate/About-Different-Middleware-Vendors.md) 然后附加一个后缀, 如 `_cpp` 以表示执行用 C++ 来写。但是,不需要这样做。 `rmw_fastrtps_cpp`, `rmw_cyclonedds_cpp`, `rmw_connextdds`, `rmw_zenoh_cpp`,以及 `rmw_email_cpp`.
    >     >
    >     > ``` xml
    >     > <!-- TODO replace with the actual implementation name -->
    >     > <name>rmw_IMPLEMENTATION_NAME_cpp</name>
    >     > ```
    >
    > 2.  宣布依赖 `rmw`
    >
    >     > 由于软件包将执行在 `rmw` 软件包,并取决于一些公用功能。
    >     >
    >     > ``` xml
    >     > <depend>rmw</depend>
    >     > ```
    >
    > 3.  宣布对所需类型支持包的依赖
    >
    >     > 见 [类型支助科](#rmw-impl-guide-typesupport) 详细情况。
    >     >
    >     > ``` xml
    >     > <!-- keep or add what is necessary -->
    >     > <depend>rosidl_typesupport_fastrtps_c</depend>
    >     > <depend>rosidl_typesupport_fastrtps_cpp</depend>
    >     > <depend>rosidl_typesupport_introspection_c</depend>
    >     > <depend>rosidl_typesupport_introspection_cpp</depend>
    >     > ```
    >
    > 4.  A. 宣布加入《公约》 `rmw_implementation_packages` 组
    >
    >     > 这允许 `rmw_implementation` 软件包改为 [取决于执行](https://github.com/ros2/rmw_implementation/blob/4dd5d571a5bfa1a67183acf271dfa442932c7572/rmw_implementation/package.xml#L38) 以便与其他执行一起建造,因为没有包件明确取决于任何 `rmw` 执行。如果选中的话,可以找到并使用这种方式。
    >     >
    >     > ``` xml
    >     > <member_of_group>rmw_implementation_packages</member_of_group>
    >     > ```

2.  `CMakeLists.txt`

    > 1.  创建库目标
    >
    >     > 库必须是一个共享的库。它应该取决于 `rmw` 用于信头和公用功能以及所需的类型支持软件包。它也取决于基本的中间软件。
    >     >
    >     > ``` cmake
    >     > add_library(${PROJECT_NAME} SHARED
    >     >   src/file.cpp
    >     >   # ...
    >     > )
    >     > target_link_libraries(${PROJECT_NAME} PUBLIC
    >     >   rmw::rmw
    >     > )
    >     > target_link_libraries(${PROJECT_NAME} PRIVATE
    >     >   rosidl_typesupport_fastrtps_c::rosidl_typesupport_fastrtps_c
    >     >   rosidl_typesupport_fastrtps_cpp::rosidl_typesupport_fastrtps_cpp
    >     >   rosidl_typesupport_introspection_c::rosidl_typesupport_introspection_c
    >     >   rosidl_typesupport_introspection_cpp::rosidl_typesupport_introspection_cpp
    >     >   # TODO add any implementation-specific dependencies, e.g., underlying middleware
    >     > )
    >     > ```
    >
    > 2.  配置执行库目标
    >
    >     > 在实践中,这仅仅是使符号默认隐藏来隐藏内部符号,即非-`rmw` 界面符号。如果执行用 C 写( 不常见) , 请指定 `LANGUAGE "C"`.
    >     >
    >     > ``` cmake
    >     > configure_rmw_library(${PROJECT_NAME})
    >     > ```
    >
    > 3.  登记 `rmw` 执行
    >
    >     > 记录了《公约》的执行情况。 [军衔指数](../../How-To-Guides/Ament-CMake-Documentation.md#ament-cmake-doc-adding-resources) 以便可以在建设时找到(`get_available_rmw_implementations()`, `get_rmw_typesupport()`)或运行时间(`ament_index_cpp::get_resources("rmw_typesupport")`)它也注册了执行支持的语言和类型支持软件包列表。例如,如果执行只使用类型支持内观(即动态而非静态)用于C和C++消息:
    >     >
    >     > ``` cmake
    >     > register_rmw_implementation(
    >     >   "c:rosidl_typesupport_introspection_c"
    >     >   "cpp:rosidl_typesupport_introspection_cpp"
    >     > )
    >     > ```
    >
    > 4.  安装和导出目标
    >
    >     > ``` cmake
    >     > install(
    >     >   TARGETS ${PROJECT_NAME}
    >     >   EXPORT ${PROJECT_NAME}
    >     >   ARCHIVE DESTINATION lib
    >     >   LIBRARY DESTINATION lib
    >     >   RUNTIME DESTINATION bin
    >     > )
    >     >
    >     > ament_export_targets(${PROJECT_NAME})
    >     > # ament_export_libraries(${PROJECT_NAME})  # Old-style CMake
    >     >
    >     > # ...
    >     > ```

<span id="interface-functions-implementation"></span>

### 接口功能执行

第一步是定义在 `rmw` 标题。 以简单的返回的空函数开始 `RMW_RET_OK`,然后按照运行时可能被调用顺序逐一执行。例如: `rmw_init()`, `rmw_create_node()`, `rmw_create_publisher()`, `rmw_create_subscription()`这将允许逐步建立和运行/测试执行。

多数 `rmw` 函数必须进行由函数文档定义的输入验证。有各种功能宏来简化,例如 `RMW_CHECK_ARGUMENT_FOR_NULL()` 财务报告和财务报告 `RMW_CHECK_TYPE_IDENTIFIERS_MATCH()`.

`rmw` structs通常包括一个类型变换的指针(或有时不透明指针),用于: `rmw` 具体执行数据。 `rmw_publisher_t` 拥有 `void * data`。执行可以将它想要的放在那里,例如,指向一个内部对象的指针,该指针将内置的中间软件的发布器对象和任何相关信息包裹在一起,如类型支持。此数据/对象可以在稍后的时间获取和使用。 `rmw_publish()` 与相应的 `rmw_publisher_t`。以确保一个不同的 `rmw` 执行不会试图解释这些数据, `rmw_publisher_t` 将实施工作名称列入《公约》 `implementation_identifier` 字段键入。

<span id="id6"></span>

### 类型支持

类型支持 structs 可能令人困惑。 以下是给发布者/ 订阅者的信息类型支持的示例 。

出版商通过 `rmw_create_publisher()`,用于类型支持信息的控件: `const rosidl_message_type_support_t *`。这是基于语言的类型支持 : `rosidl_typesupport_c` / `rosidl_typesupport_cpp`。从这个角度,我们可以根据可用的类型支持获得具体的类型支持手柄,例如, `rosidl_typesupport_fastrtps_c` / `rosidl_typesupport_fastrtps_cpp` 财务报告和财务报告 `rosidl_typesupport_introspection_c` / `rosidl_typesupport_introspection_cpp`。令人困惑的是,这些也是类型 `const rosidl_message_type_support_t *`然而,具体类型的支持手柄是包含实际有用信息的手柄。 [此示例函数](https://github.com/christophebedard/rmw_email/blob/f5e622bab24edaad8e0da054c7dbc698c6fb809c/rmw_email_cpp/src/type_support.cpp#L29-L62),它提取具体的 C 或 C++ 动态消息类型支持手柄(`rosidl_typesupport_introspection_{c,cpp}`)给定一个基础类型的支持手柄(`rosidl_typesupport_{c,cpp}`) 创建的出版商 `rclcpp` 将使用 C++ 类型支持,而由 `rclpy` 将使用 C 类型支持,因为 Python 信件会被转换成 C 信件。 `/rosout` 出版商由 `rcl`,其写法为C,所以它使用C类型支持.

然后,使用混凝土类型支持手柄的型式变换指针, `const void * data`中,我们得到了类型支持的特定信息。例如,对于 C++ 动态类型支持,这将是一个 `const rosidl_typesupport_introspection_cpp::MessageMembers *`,其中包含关于消息中每个字段的信息。 [此示例函数](https://github.com/christophebedard/rmw_email/blob/f5e622bab24edaad8e0da054c7dbc698c6fb809c/rmw_email_cpp/src/conversion.cpp#L116-L153),它从混凝土类型支持手柄中提取语言依赖型支持信息。该信息用于读取被打磨过的消息指针,并将消息转换为YAML对象,然后转换为字符串,供基础中间软件发布。

服务类型支持类似,但 `rosidl_service_type_support_t` 指向请求和响应信息类型的独立类型支持信息。

<span id="tests"></span>

## 测试

那个... `rmw` 软件包中包含一些测试,但它们主要用于公用事业(例如,获得零初始化结构)和专题/节点名称/命名空间验证等非具体执行功能。

至于测试新的 `rmw` 执行《公约》的情况 `test_rmw_implementation` 软件包 [包含接口测试](https://github.com/ros2/rmw_implementation/tree/rolling/test_rmw_implementation/test)。测试可执行文件首先被定义,然后一个 CMake 函数为给定的创建测试目标 `rmw` 通过设置 `RMW_IMPLEMENTATION` 环境变量。 `rmw_implementation_cmake`’s `call_for_each_rmw_implementation()` 调用和提供 CMake 函数,每个可用的执行都调用该函数。 [CMakeLists.txt 文件 页面存档备份,存于互联网档案馆](https://github.com/ros2/rmw_implementation/blob/rolling/test_rmw_implementation/CMakeLists.txt)。许多其他软件包,包括: `test_rclcpp` 仅测试的软件包, 也 [使用此机制](https://github.com/ros2/system_tests/blob/rolling/test_rclcpp/CMakeLists.txt) 测试所有可用的 `rmw` 执行,否则测试只是随默认执行运行。软件包也可以使用。 `get_available_rmw_implementations()` 以获得现有执行的实际清单。

有些测试有针对执行的代码,这是出于各种原因进行的,例如不支持的接口子集。这些测试可以使用 `rmw`’s `rmw_get_implementation_identifier()` [函数](https://docs.ros.org/en/rolling/p/rmw/generated/function_rmw_8h_1aeb8a815b9be5eb3f38ab28363ef63920.html) 为了这个。

<span id="middleware-and-rmw-implementation-specific-configuration"></span>

## 中间器件 - 和 `rmw` 具体实施组合

那个... `rmw` 界面允许为出版商提供任意的针对执行的配置有效载荷,并通过类型化的订阅 `rmw_specific_publisher_payload` / `rmw_specific_subscription_payload` 字段内 `rmw_publisher_options_t` / `rmw_subscription_options_t`。此选项由用户通过 `RMWImplementationSpecificPublisherPayload` / `RMWImplementationSpecificSubscriptionPayload` 输入 `rclcpp`例如,这是一个先进的、非便携式的特点,目前没有任何一级执行项目使用。

更灵活一点,一些执行使用环境变量: `RMW_FASTRTPS_*`, `RMW_CONNEXT_*`等。 基本的中间软件也可能通过环境变量来配置: `FASTDDS_*`, `ZENOH_*`, `CYCLONEDDS_*`, `EMAIL_*`等,例如, `CYCLONEDDS_URI`, `FASTRTPS_DEFAULT_PROFILES_FILE`,以及 `ZENOH_SESSION_CONFIG_URI` 如果使用相关的中间软件,环境变量可以用来提供完整配置文件的路径.

<span id="footnotes"></span>

## 脚注

<span id="fn-dds-rpc"></span>

\[[1](#id1)\]

现在有一个DDS RPC的规格,但是 [最初设计ROS 2时,DDS供应商没有执行](https://design.ros2.org/articles/ros_on_dds.html#services-and-actions)。自 `rmw` 界面也正式为 DDS- 不可知性, 服务可达 `rmw` 执行,这解释了为什么 [跨DDS供应商通信得不到保障](../../Concepts/Intermediate/About-Different-Middleware-Vendors.md#different-middleware-vendors-cross-vendor-communication),即使酒吧/分店一般工作。

<span id="fn-gid-remote-matching"></span>

\[[2](#id2)\]

在实践中,情况并非总是如此,因此这一要求有些放松。 [ros2/rmw_cyclonedds#377](https://github.com/ros2/rmw_cyclonedds/issues/377).

<span id="fn-ts-fastrtps"></span>

\[[3](#id4)\]

例如,C++消息快速CDR序列化/去序化代码生成用于 [std_msgs/msg/Header](https://docs.ros.org/en/rolling/p/std_msgs/msg/Header.html) 处于 `std_msgs/rosidl_typesupport_fastrtps_cpp/std_msgs/msg/detail/dds_fastrtps/header__type_support.cpp` 下划线 `build/` 目录。

<span id="fn-ts-dynamic"></span>

\[[4](#id5)\]

例如,为 [std_msgs/msg/Header](https://docs.ros.org/en/rolling/p/std_msgs/msg/Header.html) 处于 `std_msgs/rosidl_typesupport_introspection_cpp/std_msgs/msg/detail/header__type_support.cpp` 下划线 `build/` 目录。
