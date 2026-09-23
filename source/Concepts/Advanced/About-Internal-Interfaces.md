---
translation_status: machine_translated
source: Concepts/Advanced/About-Internal-Interfaces.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="internal-ros-2-interfaces"></span>

# ROS 2 内部接口

内部ROS接口为公用C [APIs 辅助程序](../../Glossary.md#term-API) 用于正在创建的开发者 [客户端库](../../Glossary.md#term-client_library) 或添加新的内置中间软件,但并非用于典型的ROS用户。 [客户端库](../../Glossary.md#term-client_library) 提供用户面对的 [APIs 辅助程序](../../Glossary.md#term-API) 大多数ROS用户熟悉,并且可能以多种编程语言出现.

<span id="internal-api-architecture-overview"></span>

## 内部 API 架构概览

主要有两种内部接口:

- ROS 中间软件接口(`rmw` [API](../../Glossary.md#term-API))

- ROS 客户端库接口( )`rcl` [API](../../Glossary.md#term-API))

那个... `rmw` [API](../../Glossary.md#term-API) 是ROS 2 软件堆栈与基础中件执行的接口。ROS 2 使用的基础中件要么是DDS,要么是RTPS执行,负责发现、发布和订阅力学、服务请求复制力学以及消息类型的序列化。

那个... `rcl` [API](../../Glossary.md#term-API) 是一个稍高的级别 [API](../../Glossary.md#term-API) 用于执行 [客户端库](../../Glossary.md#term-client_library) 并且不直接触摸中间软件执行,而是通过ROS中间软件接口进行(`rmw` [API](../../Glossary.md#term-API))抽象化.

<figure class="align-default">
<img src="../images/ros_client_library_api_stack.png" alt="ros2 软件堆栈" />
</figure>

如图所示,这些 [APIs 辅助程序](../../Glossary.md#term-API) 堆叠到这样一来,典型的ROS用户将使用 [客户端库](../../Glossary.md#term-client_library) [API](../../Glossary.md#term-API), e.g. `rclcpp`,以实施其代码(可执行或库)。 [客户端库](../../Glossary.md#term-client_library), e.g. `rclcpp`时,使用 `rcl` 用于访问 ROS 图形和图表事件的界面。 `rcl` 反过来又使用 `rmw` [API](../../Glossary.md#term-API) 以访问 ROS 图表。 `rcl` 执行的目的是为更复杂的ROS概念和公用设施提供一个共同的实施,这些概念和公用设施可供各种企业使用。 [客户端库](../../Glossary.md#term-client_library),同时对正在使用的基本中间软件保持不可知性。 `rmw` 界面是获取支持 ROS 客户端库所需的绝对最小的中间软件功能。 `rmw` [API](../../Glossary.md#term-API) 由中间软件具体执行提供 [软件包](../../Glossary.md#term-package), e.g. `rmw_fastrtps_cpp`,其库根据供应商特定的DDS接口和类型编译.

上面的图中还有一个框标注着 `ros_to_dds`, 而这个框的目的是要代表一组可能的软件包, 允许用户使用 ROS 等同软件访问 DDS 供应商的特定对象和设置。 这个抽象界面的目标之一是将 ROS 用户的空间代码与正在使用的中间软件完全隔绝, 这样改变 DDS 供应商甚至中间软件技术对用户代码影响最小。 然而, 我们承认, 有时尽管有后果, 仍可以接触到执行并手动调整设置。 通过要求使用其中一种软件包来访问基本的 DDS 供应商对象, 我们就可以避免在正常界面中暴露出供应商的特定符号和标题 。 还可以通过检查软件包的依赖性以查看其中之一, 很容易看到哪些代码可能违反供应商的可移植性 。 `ros_to_dds` 正在使用软件包。

<span id="type-specific-interfaces"></span> <span id="id1"></span>

## 类型 特定接口

一直以来,这里有些地方 [APIs 辅助程序](../../Glossary.md#term-API) 必然针对正在交换的信件类型,例如发布一个信件或签署一个主题,因此需要为每个信件类型生成代码。以下图表从用户定义的路径进行布局 `rosidl` 文档,例如: `.msg` 文件,到用户和系统用于执行类型特定功能的特定类型代码:

<span id="id2"></span>

<figure class="align-default">
<img src="../images/ros_idl_api_stack_static.png" alt="ros2 idl 静态类型支持堆栈" />
<figcaption><p>图: " 静态 " 类型支持生成流程图,来自 <code class="docutils literal notranslate">rosidl</code> 用于用户面对代码的文件。</p></figcaption>
</figure>

图表的右手侧显示 `.msg` 文件直接传递给特定语言的代码生成器,例如: `rosidl_generator_cpp` 或 时 间 `rosidl_generator_py`。这些生成器负责创建用户将包含(或导入)的代码,并用作信件的内在表示。 `.msg` 文件。例如,考虑信件 `std_msgs/String`,用户可能会使用 C++ 中的此文件并配有语句 `#include <std_msgs/msg/string.hpp>`,或者他们可能使用声明 `from std_msgs.msg import String` 在 Python 中。这些语句工作是因为这些语言特定(但中间软件不可知)生成的生成器软件包所产生的文件。

分别是: `.msg` 文档用于生成每种类型的类型支持代码。在这种情况下,类型支持意味着:特定类型并被系统用于执行特定类型特定任务的元数据或函数。对特定信息的类型支持可能包含诸如消息中每个字段的名称和类型列表等内容。它也可能包含可以执行该类型特定任务的代码的引用,例如发布消息。

<span id="static-type-support"></span> <span id="internal-interfaces-static-type-support"></span>

### 静态类型支持

当类型支持引用代码来为特定信息类型执行特定功能时,该代码有时需要做中件特定的工作。例如,考虑特定类型发布功能,当使用“vendor A”时,该功能需要调用一些“vendor A's” [API](../../Glossary.md#term-API),但是在使用“供应商B”时,需要将“供应商B”称为“供应商B”\`s”。 [API](../../Glossary.md#term-API)为允许中件供应商特定代码,用户定义 `.msg` 文件可能导致生成供应商特定代码。这种供应商特定代码仍然通过类型支持抽象来隐藏在用户的手中,这与“私人执行”(或简便)模式的运作方式相似。

<span id="static-type-support-with-dds"></span>

### 带有 DDS 的静态类型支持

对于基于DDS的中间软件供应商,特别是那些基于OMG IDL文件生成代码的供应商(`.idl` 文件,用户定义 `rosidl` 文档( E)`.msg` 文件)被转换成等效的OMG IDL文件(`.idl` 。从这些 OMG IDL 文件中,创建了供应商特定代码,然后在类型特定函数范围内使用,这些函数由给定类型的类型支持引用。上面的图表在左手边显示这一点。 `.msg` 文件为 `rosidl_dds` 要生产的软件包 `.idl` 文档,然后是 `.idl` 文件提供给特定语言和DDS供应商特定类型的支持生成软件包。

例如,考虑快速DDS执行,其中有一个软件包叫做 `rosidl_typesupport_fastrtps_cpp`。这个软件包负责生成代码来处理诸如将一个 C++ 消息对象转换成一个序列化的 octet 缓冲器,以便在网络上写入。这个代码虽然是针对快速DS的,但由于类型支持代码中的抽象,仍然不向用户曝光。

<span id="dynamic-type-support"></span> <span id="internal-interfaces-dynamic-type-support"></span>

### 动态类型支持

执行类型支持的另一种方式是,对诸如发布到一个主题之类的事物具有通用功能,而不是为每个消息类型生成一个版本的功能。为了实现这一点,这个通用功能需要一些关于正在发布消息类型的元信息,比如按消息类型中出现的顺序列出字段名称和类型。然后,要发布一个消息,您就叫作通用发布功能,并传递一个包含关于消息类型的必要元数据的结构。这被称为“动态”类型支持,而不是“静态”类型支持,它需要为每个类型生成一个函数的版本。

<span id="id3"></span>

<figure class="align-default">
<img src="../images/ros_idl_api_stack_dynamic.png" alt="ros2 idl 动态类型支持堆栈" />
<figcaption><p>图: " 动态 " 类型支持生成流程图,来自 <code class="docutils literal notranslate">rosidl</code> 用于用户面对代码的文件。</p></figcaption>
</figure>

上图显示了用户定义的流量 `rosidl` 用于生成的用户面对代码。它与静态类型支持的图表非常相似,并且仅以图的左手侧代表类型支持的生成方式有所不同。在动态类型中, `.msg` 文件直接转换为面临代码的用户。

这个代码也是中间软件不可知的,因为它只包含关于信件的元信息。 实际进行工作的功能, 如发布到一个主题, 是信件类型的通用功能, 并且会给中间软件进行任何必要的呼叫 。 [APIs 辅助程序](../../Glossary.md#term-API)。请注意,该方法不是dds供应商提供类型支持代码的特定软件包,而是对每种语言都有中间软件不可知软件包,例如。 `rosidl_typesupport_introspection_c` 财务报告和财务报告 `rosidl_typesupport_introspection_cpp`。该词 `introspection` 软件包名称的一部分是指能够用生成的元数据对消息类型进行回顾。这是基本能力,能够对“向一个主题”等功能进行通用执行。

这个方法的优点是所有生成的代码都是中件不可知的,这意味着只要允许动态类型支持,它就可以被重复用于不同的中件执行,这也会导致生成的代码较少,从而减少编译时间和代码大小.

然而,动态类型支持需要基础的中间软件支持类似形式的动态类型支持。在DDS的情况下,DDS-XTypes标准允许使用元信息而不是生成代码发布消息。DDS-XTypes,或类似的东西,需要在基础中间软件中支持动态类型支持。此外,这种类型支持方法通常比静态类型支持替代方法慢。静态类型支持中的特定类型生成代码可以被写入来提高效率,因为它不需要在消息类型的元数据上进行排列,以完成序列化等事务。

<span id="the-rcl-repository"></span>

## 那个... `rcl` 存储器

ROS客户端库界面(`rcl` [API](../../Glossary.md#term-API))可用于: [客户端库](../../Glossary.md#term-client_library) (e.g. `rclc`, `rclcpp`, `rclpy`,以避免重复逻辑和特性。 `rcl` [API](../../Glossary.md#term-API),客户端库可以更小,更相互一致. 客户端库的某些部分被故意省去. `rcl` [API](../../Glossary.md#term-API) 因为应该使用语言平庸的方法来实施系统的这些部分。一个很好的例子就是执行模式。 `rcl` 。相反,客户端库应该提供语言平庸的解决方案,比如 `pthreads` 中文本无需改动。 `std::thread` 在 C++11 中,以及 `threading.Thread` 在 Python 中。一般为 `rcl` 界面提供了非特定语言模式且非特定信息类型的功能。

那个... `rcl` [API](../../Glossary.md#term-API) 位于该 [ros2/rcl](https://github.com/ros2/rcl) 运行于 [GitHub](https://github.com/) 并包含作为 C 标题的接口。 `rcl` C 执行由 `rcl` [软件包](../../Glossary.md#term-package) 此执行可避免直接与中间软件接触,而是使用 `rmw` 财务报告和财务报告 `rosidl` [APIs 辅助程序](../../Glossary.md#term-API).

完整定义: `rcl` [API](../../Glossary.md#term-API),见 [rcl 文档](http://docs.ros.org/en/rolling/p/rcl/).

<span id="the-rmw-repository"></span>

## 那个... `rmw` 存储器

ROS 中间软件接口(`rmw` [API](../../Glossary.md#term-API))是顶部构建ROS所需的最低限度的原始中件能力. 不同中件执行的供应商必须执行这个接口,以便支持顶部的整个ROS堆栈. 目前大多数中件执行是针对不同的DDS供应商的.

那个... `rmw` [API](../../Glossary.md#term-API) 位于该 [ros2/rmw](https://github.com/ros2/rmw) 数据库。 `rmw` [软件包](../../Glossary.md#term-package) 包含定义接口的 C 标题,其执行由各种 [软件包](../../Glossary.md#term-package) 用于不同DDS供应商的 Rmw 执行。

定义: `rmw` [API](../../Glossary.md#term-API),见 [rmw 文件](http://docs.ros.org/en/rolling/p/rmw/).

关于ROS 2如何与不同的中间软件执行集成的更实际的深入概述,参见: [中间软件执行教程](../../Tutorials/Advanced/Creating-An-RMW-Implementation.md).

<span id="the-rosidl-repository"></span>

## 那个... `rosidl` 存储器

那个... `rosidl` [API](../../Glossary.md#term-API) 包含一些与信件相关的静态功能和类型,以及定义不同语言信件应生成何种代码。 [API](../../Glossary.md#term-API) 将指定语言,但可能重复使用或可能不重复使用其他语言生成的代码。 [API](../../Glossary.md#term-API) 包含信件数据结构、用于构建、销毁等功能。 [API](../../Glossary.md#term-API) 还将执行一种方法,以获取消息类型的类型支持结构,在发布或签名该消息类型主题时使用该类型。

有几个寄存器在其中发挥作用。 `rosidl` [API](../../Glossary.md#term-API) 执行。

那个... `rosidl` 寄存器,位于 [GitHub](https://github.com/) 现时 [ros2/rosidl](https://github.com/ros2/rosidl),定义信件 IDL 语法,即: `.msg` 文档, `.srv` 文件等,并包含 [软件包](../../Glossary.md#term-package) 用于解析文件、提供 CMake 基础设施以生成信件中的代码、生成执行不可知代码(标题和源文件)以及建立默认的生成器集。寄存器包含这些 [软件包](../../Glossary.md#term-package):

- `rosidl_cmake`: 提供 CMake 函数和模块,用于从 `rosidl` 文档,例如: `.msg` 文档, `.srv` 文档等。

- `rosidl_default_generators`: 定义默认发电机列表, 以确保它们作为依赖性安装, 但其他注入的发电机也可以使用.

- `rosidl_generator_c`: 提供生成 C 页眉文件的工具 (`.h`用于: `rosidl` 文档。

- `rosidl_generator_cpp`: 提供生成 C++ 头文件的工具( )`.hpp`用于: `rosidl` 文档。

- `rosidl_generator_py`: 提供生成 Python 模块的工具 `rosidl` 文档。

- `rosidl_parser`: 提供 Python 语句 [API](../../Glossary.md#term-API) 用于解析 `rosidl` 文档。

其他语文的发电机,例如: `rosidl_generator_java`,在外部(不同储存库)托管,但将使用上述发电机所用的相同机制,作为“登记”本身的一种。 `rosidl` 发电机。

除上述情况外, [软件包](../../Glossary.md#term-package) 用于解析和生成标题 `rosidl` 文档中, `rosidl` 存储器还包含 [软件包](../../Glossary.md#term-package) 对于文件中定义的信息类型,类型支持是指能够解释和操纵特定类型的ROS消息实例(例如,发布消息)所代表的信息。类型支持可以是编译时生成的代码提供的,也可以是程序上基于编辑时的内容提供的。 `rosidl` 文档,例如 `.msg` 或 时 间 `.srv` 文件,以及收到的数据,通过对数据的回顾。对于后者,如果类型支持是通过对消息的运行时间解释来完成的,ROS 2生成的消息代码可以对rmw执行进行不可知论。通过对数据的回顾来提供这种类型的支持的软件包有:

- `rosidl_typesupport_introspection_c`: 提供生成用于支持的 C 代码的工具 `rosidl` 消息数据类型。

- `rosidl_typesupport_introspection_cpp`: 提供生成支持的 C++ 代码的工具 `rosidl` 消息数据类型。

如果在编译时生成类型支持而不是程序生成,则需要使用一个针对rmw执行的软件包。这是因为,典型的rmw执行需要以DDS供应商特有的方式存储和操纵数据,以便DDS执行加以利用。 [类型 特定接口](#type-specific-interfaces) 详见上文一节。

欲了解更多关于《公约》中具体内容的信息。 `rosidl` [API](../../Glossary.md#term-API) (静态和生成)参见此页:

<span id="the-rcutils-repository"></span>

## 那个... `rcutils` 存储器

ROS 2 C 公用事业`rcutils`)为C. [API](../../Glossary.md#term-API) 由整个ROS 2 代码库中使用的宏、函数和数据结构组成。这些主要用于错误处理、命令行参数解析和记录,这些不是客户端或中间软件层所特有的,可以由两者共享。

那个... `rcutils` [API](../../Glossary.md#term-API) 执行地点在 [ros2/rcutils](https://github.com/ros2/rcutils) 运行于 [GitHub](https://github.com/) 它包含作为 C 标题的接口。

完整定义: `rcutils` [API](../../Glossary.md#term-API),见 [rcutils 文档](https://docs.ros.org/en/rolling/p/rcutils/).
