---
translation_status: machine_translated
source: Releases/Alpha-Overview.rst
---

<span id="alphas"></span>

# Alpha 版本

这是之前为ROS 2 8 alpha 发行而分离的页面的合并版本.

我们希望你们能试一试 [提供反馈](../Contact.md).

<span id="ros-2-alpha8-release-code-name-hook-and-loop-october-2016"></span>

## ROS 2 alpha8 发布(代码名称) *钩和圈*; 2016年10月 (中文(简体) ).

<span id="changes-to-supported-dds-vendors"></span>

### 支持的DDS供应商的变动

ROS 2支持多个中件执行(参见 [此页面](../Concepts/Intermediate/About-Different-Middleware-Vendors.md) 直到Alpha 8, ROS 2 支持 ROS 中间软件执行 eProsima的快速 RTPS 、 RTI 的 Connext 和 PrismTech 的 OpenSplice 。 为了精简我们的努力,从 Alpha 8 开始, Fast RTPS 和 Connext ( static) 将会支持 快速 RTPS () 。[现在 Apache 2.0 许可](http://www.eprosima.com/index.php/company-all/news/61-eprosima-goes-apache))作为默认运出。

<span id="scope"></span>

### 范围

正如“alpha”修饰语所显示的,ROS 2的发布远未完成。 你不应该期望从ROS 1 转换为ROS 2,你不应该期望用ROS 2 构建一个新的机器人控制系统,相反,你应该期望尝试一些演示,探索代码,也许写你自己的演示。

该版的改进包括:

- 对快速RTPS及其rmw执行的几项改进

  - 在 Fast RTPS 中支持大型( 图像) 消息

  - `wait_for_service` Fast RTPS 中的功能

- 支持 Python 和 C 中的所有 ROS 2 信件类型

- 添加对 Python 服务质量设置的支持

- 用上一个 alpha 释放修正了各种错误

以上未列出的几乎都未列入本版。 [路线图](../The-ROS2-Project/Roadmap.md).

<span id="ros-2-alpha7-release-code-name-glue-gun-july-2016"></span>

## ROS 2 alpha7 发布(代码名称) *胶枪*; July 2016) (中文(简体) ).

<span id="new-version-of-ubuntu-required"></span>

### 需要新版本的 Ubuntu

直到Alpha 6 ROS 2以Ubuntu Trusty Tahr(14.04)为攻击目标. 截至此Alpha ROS 2以Ubuntu Xenial Xerus(16.04)为攻击目标,从编译器,CMake,Python等较新的版本中受益.

<span id="id2"></span>

### 范围

正如“alpha”修饰语所显示的,ROS 2的发布远未完成。 你不应该期望从ROS 1 转换为ROS 2,你不应该期望用ROS 2 构建一个新的机器人控制系统,相反,你应该期望尝试一些演示,探索代码,也许写你自己的演示。

该版的主要内容包括:

- 图表 API 功能: 等待\_ for\_ service

  - 在 rclcpp 中添加接口,并在实例、演示和测试中使用它们

- 改进对Connext和Fast-RTPS(部分用于Fast-RTPS)中大型信息的支持

- Turtlebot 演示,使用ROS 1的移植代码

  - 见: <https://github.com/ros2/turtlebot2_demo>

以上未列出的几乎都未列入本版。 [路线图](../The-ROS2-Project/Roadmap.md).

<span id="ros-2-alpha6-release-code-name-fastener-june-2016"></span>

## ROS 2 alpha6 发布(代码名称) *快捷键*; 2016年6月) (中文(简体) ).

<span id="id4"></span>

### 范围

正如“alpha”修饰语所显示的,ROS 2的发布远未完成。 你不应该期望从ROS 1 转换为ROS 2,你不应该期望用ROS 2 构建一个新的机器人控制系统,相反,你应该期望尝试一些演示,探索代码,也许写你自己的演示。

该版的主要内容包括:

- 图表 API 功能: 等待\_ for\_ service

  - 将图形守护条件添加到等待图形更改的节点中

  - 已添加 `rmw_service_server_is_available` 用于核查是否有服务

- 已重构 `rclcpp` 用于: `rcl`

- 改进对 Python 中复杂消息类型的支持

  - 嵌入式信件

  - 阵列

  - 字符串

以上未列出的几乎都未列入本版。 [路线图](../The-ROS2-Project/Roadmap.md).

<span id="ros-2-alpha5-release-code-name-epoxy-april-2016"></span>

## ROS 2 alpha5 发布(代码名称) *叶片*; 2016年4月 (中文(简体) ).

<span id="id6"></span>

### 范围

正如“alpha”修饰语所显示的,ROS 2的发布远未完成。 你不应该期望从ROS 1 转换为ROS 2,你不应该期望用ROS 2 构建一个新的机器人控制系统,相反,你应该期望尝试一些演示,探索代码,也许写你自己的演示。

该版的主要内容包括:

- 在Fast RTPS和Connext Dynamic rmw执行中支持C数据结构.

- C类支助服务。

- 增加了32位和64位的ARM作为实验支持的平台.

以上未列出的几乎都未列入本版。 [路线图](../The-ROS2-Project/Roadmap.md).

<span id="ros-2-alpha4-release-code-name-duct-tape-february-2016"></span>

## ROS 2 alpha4 发布(代码名称) *底盘磁带*; 2月 2016) (中文(简体) ).

<span id="background"></span>

### 背景

a 如解释。 [设计文章](https://design.ros2.org/articles/why_ros2.html)我们正利用这一机会对系统进行实质性的改变,包括改变一些核心API。 [ROS 2 设计文章](https://design.ros2.org).

<span id="status"></span>

### 状态

2016年2月17日,我们发布ROS 2 alpha4代号 **底盘磁带**. 我们通过本版的主要目的是增加更多的功能,同时处理我们收到的对前几期版的反馈。 [演示](../Tutorials.md) 我们鼓励你们尝试这些演示,看看执行这些演示的代码, [提供反馈](../Contact.md)我们特别想知道,我们在处理对你们很重要的个案方面做得有多好(或很差)。

<span id="intended-audience"></span>

### 预定受众

虽然欢迎每个人尝试演示并查看代码,但我们的目标是让那些已经体验过ROS 1开发的人来发布。 此时,ROS 2文件相当稀少,系统的大部分内容都用它与ROS 1的对比来解释。

<span id="id8"></span>

### 范围

正如“alpha”修饰语所显示的,ROS 2的发布远未完成。 你不应该期望从ROS 1 转换为ROS 2,你不应该期望用ROS 2 构建一个新的机器人控制系统,相反,你应该期望尝试一些演示,探索代码,也许写你自己的演示。

该版的主要内容包括:

- 改进型支助基础设施,包括对C的支助

- 初步 Python 客户端库,仅支持出版商和订阅。 小心, API 可能会有变化, 而且还远未完成 !

- 在 CPI 中添加 ROS 时间的结构( 仍然需要 C++ API)

  - ROS Time的扩展“时间源”新概念,默认时间源将像ROS 1(待执行)

以上未列出的几乎都未列入本版。 [路线图](../The-ROS2-Project/Roadmap.md).

<span id="ros-2-alpha3-release-code-name-cement-december-2015"></span>

## ROS 2 alpha3 发布( 代码名称) *水泥*; 2015年12月) (中文(简体) ).

<span id="id10"></span>

### 背景

a 如解释。 [设计文章](https://design.ros2.org/articles/why_ros2.html)我们正利用这一机会对系统进行实质性的改变,包括改变一些核心API。 [ROS 2 设计文章](https://design.ros2.org).

<span id="id11"></span>

### 状态

2015年12月18日,我们将发布ROS 2 alpha3,代号为: **水泥**. 我们通过本版的主要目的是增加更多的功能,同时处理我们收到的对前几期版的反馈。 [演示](../Tutorials.md) 我们鼓励你们尝试这些演示,看看执行这些演示的代码, [提供反馈](../Contact.md)我们特别想知道,我们在处理对你们很重要的个案方面做得有多好(或很差)。

<span id="id12"></span>

### 预定受众

虽然欢迎每个人尝试演示并查看代码,但我们的目标是让那些已经体验过ROS 1开发的人来发布。 此时,ROS 2文件相当稀少,系统的大部分内容都用它与ROS 1的对比来解释。

<span id="id13"></span>

### 范围

正如“alpha”修饰语所显示的,ROS 2的发布远未完成。 你不应该期望从ROS 1 转换为ROS 2,你不应该期望用ROS 2 构建一个新的机器人控制系统,相反,你应该期望尝试一些演示,探索代码,也许写你自己的演示。

该版的主要内容包括:

- 更新 `rcl` 接口。

  - 此界面将被包裹,以创建语言绑定,例如. `rclpy`.

  - 这个接口比目前已有的接口改进了文件和测试覆盖面,例如: `rmw` 财务报告和财务报告 `rclcpp`.

  - 见 [rcl 信头](https://github.com/ros2/rcl/tree/release-alpha3/rcl/include/rcl).

- 在rclpp中增加了支持使用TLSF(双层隔离适配)分配器,即嵌入式和实时系统的内存分配器设计.

- 多线程执行器效率提高,并用多线程执行方式固定了众多的bug,现在正在CI上测试.

- 添加了从旋中调用回调中取消执行器的能力 。

- 通过支持接受自己作为函数参数的引用的计时器调回调用,增加了计时器自行取消的能力.

- 添加选中的多个线索以输入执行器: spin 。

- 多次试验的可靠性得到提高,这些试验时断时续地失败。

- 添加了对使用快RTPS的支持(而不是OpenSplice或Connext).

- tf2的部分端口包括核心库和核心命令行工具.

以上未列出的几乎都未列入本版。 [路线图](../The-ROS2-Project/Roadmap.md).

<span id="ros-2-alpha2-release-code-name-baling-wire-october-2015"></span>

## ROS 2 alpha2 发布( 代码名称) *弯曲线*; 2015年10月) (中文(简体) ).

<span id="id15"></span>

### 背景

a 如解释。 [设计文章](https://design.ros2.org/articles/why_ros2.html)我们正利用这一机会对系统进行实质性的改变,包括改变一些核心API。 [ROS 2 设计文章](https://design.ros2.org).

<span id="id16"></span>

### 状态

2015年11月3日,我们发布ROS 2 alpha2代号 **弯曲线**。我们的首要目标是增加更多的功能,同时处理我们收到的关于上一期α 1 发布情况的反馈。为此,我们建立了一套 [演示](../Tutorials.md) 我们鼓励你们尝试这些演示,看看执行这些演示的代码, [提供反馈](../Contact.md)我们特别想知道,我们在处理对你们很重要的个案方面做得有多好(或很差)。

<span id="id17"></span>

### 预定受众

虽然欢迎每个人尝试演示并查看代码,但我们的目标是让那些已经体验过ROS 1开发的人来发布。 此时,ROS 2文件相当稀少,系统的大部分内容都用它与ROS 1的对比来解释。

<span id="id18"></span>

### 范围

正如“alpha”修饰语所显示的,ROS 2的发布远未完成。 你不应该期望从ROS 1 转换为ROS 2,你不应该期望用ROS 2 构建一个新的机器人控制系统,相反,你应该期望尝试一些演示,探索代码,也许写你自己的演示。

该版的主要内容包括:

- 支持 Rclcpp 中的自定义分配符, 用于实时消息

- Windows与Linux/OSX的特性均等,包括工作空间管理,服务和参数

- rclcpp API 改进

- FreeRTPS 改进

以上未列出的几乎都未列入本版。 [路线图](../The-ROS2-Project/Roadmap.md).

<span id="ros-2-alpha1-release-code-name-anchor-august-2015"></span>

## ROS 2 alpha1 发布(代码名称) *锁定*; 2015年8月 (中文(简体) ).

<span id="id20"></span>

### 背景

a 如解释。 [设计文章](https://design.ros2.org/articles/why_ros2.html)我们正利用这一机会对系统进行实质性的改变,包括改变一些核心API。 [ROS 2 设计文章](https://design.ros2.org).

<span id="id21"></span>

### 状态

2015年8月31日,我们发布ROS 2 α1代号 **锁定**我们的首要目标是让你有机会了解ROS 2是如何运作的,特别是它与ROS1有何不同。 [演示](../Tutorials.md) 我们鼓励你们尝试这些演示,看看执行这些演示的代码, [提供反馈](../Contact.md)我们特别想知道,我们在处理对你们很重要的个案方面做得有多好(或很差)。

<span id="id22"></span>

### 预定受众

虽然欢迎每个人尝试演示并查看代码,但我们的目标是让那些已经体验过ROS 1开发的人来发布。 此时,ROS 2文件相当稀少,系统的大部分内容都用它与ROS 1的对比来解释。

<span id="id23"></span>

### 范围

正如“alpha”修饰语所显示的,ROS 2的发布远未完成。 你不应该期望从ROS 1 转换为ROS 2,你不应该期望用ROS 2 构建一个新的机器人控制系统,相反,你应该期望尝试一些演示,探索代码,也许写你自己的演示。

该版的主要内容包括:

- 发现、运输和系列化 [使用 DDS 软件](https://design.ros2.org/articles/ros_on_dds.html)

- 支助 [多个DDS供应商](https://design.ros2.org/articles/ros_on_dds.html#vendors-and-licensing)

- 支持消息原始:主题(打印/订阅)、服务(请求/响应)和参数

- 支持 Linux (Ubuntu Trusty), OS X (Yosemite) 和 Windows (8)

- [使用服务质量设置来处理丢失的网络](../Tutorials/Demos/Quality-of-Service.md)

- [与相同的 API 进行进程间或进程内通信](../Tutorials/Demos/Intra-Process-Communication.md)

- [写入使用 ROS 2 API 的实时安全代码](../Tutorials/Demos/Real-Time-Programming.md)

- [在“光金属”微控制器上运行 ROS 2( 无操作系统)](https://github.com/ros2/freertps/wiki)

- [ROS 1和ROS 2之间的桥梁通信](https://github.com/ros2/ros1_bridge/blob/master/README.md)

以上未列出的几乎都未列入本版。 [路线图](../The-ROS2-Project/Roadmap.md).
