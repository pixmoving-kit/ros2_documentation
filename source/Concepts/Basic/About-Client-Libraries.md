---
translation_status: machine_translated
source: Concepts/Basic/About-Client-Libraries.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="client-libraries"></span>

# 客户端库

<span id="overview"></span>

## 概述

客户端库是允许用户执行ROS 2代码的API. 使用客户端库,用户可以获得ROS 2的概念,如节点,话题,服务等. 客户端库以多种编程语言出现,用户可以使用最适合其应用的语言写ROS 2代码. 例如,您可能更喜欢在Python中写入可视化工具,因为它使得原型迭代更快,而对于您系统中关注效率的部分,节点可能更好在C++中执行.

使用不同客户端库书写的节点可以互相共享消息,因为所有客户端库都执行代码生成器,为用户提供以各自语言与ROS 2接口文件互动的能力.

除了特定语言的通信工具外,客户端库向用户暴露了使ROS“ROS”成为核心功能的功能。例如,这里列出了一般可以通过客户端库访问的功能列表:

- 名称和命名空间

- 时间(实际或模拟)

- 参数

- 控制台记录

- 线索模型

- 流程内通信

<span id="supported-client-libraries"></span>

## 支持的客户端库

C++ 客户端库( S)`rclcpp`)和 Python 客户端库(`rclpy`)是两个客户端库,它们都使用共同的功能。 `rcl`.

<span id="the-rclcpp-package"></span>

### 那个... `rclcpp` 软件包

C++ 的 ROS 客户端库 (`rclcpp`)是用户面对的C++ idiomatic接口,它提供ROS客户端的所有功能,如创建节点,出版商,以及订阅. `rclcpp` 建在顶端 `rcl` 页:1 `rosidl` [API](../../Glossary.md#term-API),它被设计为与 C++ 生成的 C++ 消息一起使用,由 `rosidl_generator_cpp`.

`rclcpp` 使用 C++ 和 C++17 的所有特性,使界面尽可能容易使用,但由于它重新使用 `rcl` 它能够与其他使用该软件的客户端库保持一致的行为 `rcl` [API](../../Glossary.md#term-API).

那个... `rclcpp` 寄存器位于 GitHub at [ros2/rclcpp](https://github.com/ros2/rclcpp) 并包含 [软件包](../../Glossary.md#term-package) `rclcpp`创建 [API](../../Glossary.md#term-API) 文档为 <https://docs.ros.org/en/rolling/p/rclcpp/>.

<span id="the-rclpy-package"></span>

### 那个... `rclpy` 软件包

Python 的 ROS 客户端库(`rclpy`)是 C++ 客户端库的 Python 对应程序。就像 C++ 客户端库, `rclpy` 此外,还将在《公约》第2条第1款(b)项(a)项和(b)项(c)项(c)项(c)项(c)项(c)目中增加下列内容: `rcl` 用于执行的 CPI。 界面提供了一种平顶字体验, 使用本地的平顶字类型和模式, 如列表和上下文对象。 通过使用 `rcl` [API](../../Glossary.md#term-API) 在执行中,它与其他客户端库在特征对等和行为方面保持一致。 `rcl` [API](../../Glossary.md#term-API) Python 客户端库负责处理执行模式,使用 `threading.Thread` 或类似于运行在 `rcl` [API](../../Glossary.md#term-API).

如C++,它为用户交互的每个ROS消息生成自定义的Python代码,但与C++不同的是,它最终将本地的Python消息对象转换为消息的C版本. 所有操作都发生在消息的Python版本上,直到需要传递到消息中. `rcl` 层,此时它们被转换成信件的平面 C 版本,以便传递到 `rcl` C [API](../../Glossary.md#term-API)。当出版商和订阅商在同一过程中进行交流以减少Python的转换时,如果可能,就避免了这种情况。

那个... `rclpy` 寄存器位于 GitHub at [ros2/rclpy](https://github.com/ros2/rclpy) 并包含 [软件包](../../Glossary.md#term-package) `rclpy`创建 [API](../../Glossary.md#term-API) 文档为 <https://docs.ros.org/en/rolling/p/rclpy/>.

<span id="community-maintained"></span>

### 社区维护

C++和Python客户端库由核心ROS 2团队维护,而ROS 2社区的成员则维持额外的客户端库:

- [爱达](https://github.com/ada-ros/ada4ros2) 这是一套软件包(绑定到 `rcl`,信件生成器,装订到 `tf2`,示例和教程),允许为ROS 2编写Ada应用程序.

- [C](https://github.com/ros2/rclc) `rclc` 不在 rcl 上加一层,而是对 rcl 进行补充,使 rcl+rclc 成为 C 中的功能完整的客户端库。见 。 [micro.ros.org (英语).](https://micro.ros.org/) 用于教学。

- [JVM和Android 互联网档案馆的存檔,存档日期2013-12-21.](https://github.com/ros2-java) Java和Android为ROS 2装订.

- [.NET 核心、UWP和C#](https://github.com/esteve/ros2_dotnet) 这是用于为.NET Core和.NET Standard编写ROS 2应用程序的集项目(绑定,代码生成器,示例等).

- [节点.js](https://www.npmjs.com/package/rclnodejs) rclnodejs是ROS 2的Node.js客户端,它为ROS 2编程提供了简单易行的JavaScript API.

- [锈](https://github.com/ros2-rust/ros2_rust) 这是一组项目(rclers客户端库,代码生成器,示例等),使开发者能够在Rust中写入ROS 2应用程序.

- [小蝶和达特](https://github.com/rcldart) Flutter和Dart为ROS 2装订.

旧的、未维护的客户端库是:

- [C#](https://github.com/firesurfer/rclcs)

- [目标C和iOS](https://github.com/esteve/ros2_objc)

- [齐格](https://github.com/jacobperron/rclzig)

<span id="common-functionality-rcl"></span>

## 共同功能 : `rcl`

客户端库中发现的功能大多不是针对客户端库的编程语言的,例如,所有编程语言的参数行为和命名空间逻辑最好都是一样的。由于这个原因,客户端库不是从零开始实施共同的功能,而是使用一个共同的核心ROS客户端库(RCL)接口,执行ROS概念的逻辑和行为,而不是语言特定。因此,客户端库只需要用外语功能界面来包裹RCL中的共同功能。这样可以使客户端库更薄,更容易开发。因此,共同的RCL功能被C界面所曝光,因为C语言通常是客户端库最容易包装的语言。

除了让客户端库轻量级外,拥有共同核心的一个优点是语言之间的行为更加一致。 如果对核心RCL — — 比如命名空间 — — 功能的逻辑/行为有任何改变,所有使用RCL的客户端库都会有这些变化的反映。 此外,拥有共同核心意味着在维护多个客户端库时,当涉及到错误修正时,其工作就会减少。

API 文档 `rcl` 可见 [这儿](https://docs.ros.org/en/rolling/p/rcl/).

<span id="language-specific-functionality"></span>

## 语言专用功能

客户端库概念需要语言特性/属性,但不会在RCL中实施,而是在每个客户端库中实施。例如,“spin”函数使用的线程模型将具有客户端库语言特有的执行模式。

<span id="demo"></span>

## 演示

用于在出版商之间通过信件交换 `rclpy` 并使用 `rclcpp`我们鼓励你们看 [这个ROSCON谈话](https://vimeo.com/187696091) 17:25开始([参见这里的幻灯片](https://roscon.ros.org/2016/presentations/ROSCon%202016%20-%20ROS%202%20Update.pdf)).

<span id="comparison-to-ros-1"></span>

## 与ROS 1的比较

在ROS 1 中,所有客户端库都是“从底层开始”开发的,这使得ROS 1 Python客户端库能够完全在Python中执行,比如说,这带来了不需要编译代码等好处。 然而,命名惯例和行为在客户端库之间并不总是一致,错误修正必须在多个地方完成,并且很多功能只在一个客户端库中(如UDPROS)实施过.

<span id="summary"></span>

## 小结

通过使用通用核心ROS客户端库,以多种编程语言书写的客户端库更容易写,而且行为更加一致.
