---
translation_status: machine_translated
source: Releases/Release-Ardent-Apalone.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="ardent-apalone-ardent"></span>

# Ardent Apalone（`ardent`)

欢迎收看 ROS 2 软件的首次非β发布 *缩写独立*!

<span id="supported-platforms"></span>

## 支持的平台

这个版本的ROS 2在三个平台上得到了支持:

- 乌邦图16.04 (日语).

- Mac macOS 10.12 (塞拉利昂)

- 视窗 10

所有3个平台都提供二进制软件包以及如何从源代码编译的指令(见 [安装指令](../Installation.md) (a) 与《公约》有关的其他事项; [文档](https://docs.ros2.org/ardent/)).

<table class="docutils align-default">
<thead>
<tr class="row-odd">
<th colspan="4" class="head"><p>所需支助</p></th>
</tr>
<tr class="row-even">
<th class="head"><p>建筑</p></th>
<th class="head"><p>乌本图·谢尼尔(16.04.</p></th>
<th class="head"><p>MacOS Sierra (10.12) (英语).</p></th>
<th class="head"><p>Windows 10 (VS2015) (英语).</p></th>
</tr>
</thead>
<tbody>
<tr class="row-odd">
<td><p>amd64 (中文(简体) ).</p></td>
<td><p>X</p></td>
<td><p>X</p></td>
<td><p>X</p></td>
</tr>
<tr class="row-even">
<td><p>军火64</p></td>
<td><p>X</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

最低语文要求:

- C11\[^2\]

- C++14

- ⁇  3.5

\[^2\]:需要C11,但对一些不符合要求的系统的支持  
还提供了,例如,MSVC。

依赖性要求:

<table class="docutils align-default">
<thead>
<tr class="row-odd">
<th class="head"><p>软件包</p></th>
<th class="head"><p>乌本图 Xenial</p></th>
<th class="head"><p>麦克OS**</p></th>
<th class="head"><p>视窗10**</p></th>
</tr>
</thead>
<tbody>
<tr class="row-even">
<td><p>CMake</p></td>
<td><p>3.5.1</p></td>
<td><p>3.11.0</p></td>
<td><p>3.10.2</p></td>
</tr>
<tr class="row-odd">
<td><p>爱咪</p></td>
<td><p>3.3.2</p></td>
<td><p>3.6.5</p></td>
<td><p>3.3.2</p></td>
</tr>
<tr class="row-even">
<td><p>食人鱼</p></td>
<td><p>1.10*</p></td>
<td><p>1.10*</p></td>
<td><p>1.10*</p></td>
</tr>
<tr class="row-odd">
<td><p>打开CV</p></td>
<td><p>2.4.9</p></td>
<td><p>3.4.1</p></td>
<td><p>2.4.13.2*</p></td>
</tr>
<tr class="row-even">
<td><p>宝可</p></td>
<td><p>1.7.7*</p></td>
<td><p>1.7.7*</p></td>
<td><p>1.7.7*</p></td>
</tr>
<tr class="row-odd">
<td><p>Python</p></td>
<td><p>3.5.1</p></td>
<td><p>3.6.5</p></td>
<td><p>3.6.4</p></td>
</tr>
<tr class="row-even">
<td><p>Qt 键</p></td>
<td><p>5.5.1</p></td>
<td><p>5.10.0</p></td>
<td><p>5.10.0</p></td>
</tr>
<tr class="row-odd">
<td colspan="4"><p><strong>仅限 Linux( 用于龟块演示)</strong></p></td>
</tr>
<tr class="row-even">
<td><p>个人计算机L</p></td>
<td><p>1.7.2</p></td>
<td><p>N/A</p></td>
<td><p>N/A</p></td>
</tr>
</tbody>
</table>

" \* " 是指这不是上游版本(可在官方操作系统寄存器上查阅),而是OSRF或社区分发的包(自定义寄存器上建立和分发的包)。

" \*\* " 滚动分发会看到这些依附关系在其存在期间的多个版本的变化。

此文档只抓取 ROS 发行的首次发布时的版本, 并且不会随着依赖关系向前移动而更新, 因此这些版本是低水印 。

依赖软件包管理器的使用 :

- 乌本图Xenial: apt

- 马科斯:土生土长,皮普

- 视窗:巧克力,pip

构建系统支持 :

- ament_cmake

- 制作( C)

- 设置工具

中间软件执行支持 :

- eProsima 快速RTPS

- RTI 连接

- ADLINK 打开文件

<span id="features"></span>

## 特征

<span id="new-features-in-this-ros-2-release"></span>

### 本 ROS 2 发布中的新功能

- 分布式发现,发布/订阅,请求/回复通信

  - 由消费物价指数提供

  - 使用不同的供应商执行:

    - eProsima 快速RTPS 以及 ADLINK 的 OpenSplice( 来自二进制和来源)

    - RTI 的 connext( 仅来自来源)

  - 处理非理想网络的众多服务质量

  - DDS 安全支助(包括Connext和Fast RTPS)

- C++ 和 Python 3 客户端库

  - 共享C中的共同代码以统一执行

  - 执行模式从节点中分离出来,可复合节点

  - 节点特定参数(仅在 C++ atm 中)

  - 生命周期(仅在 C++ atm 中)

  - 使用相同的 API 可选进程内通信( 仅在 C++ 中)

- 信件定义( 带有边框数组和字符串以及默认值)

- 命令行工具(例如: `ros2 run`)

- `rviz` 带有少数显示类型( Windows 版本可能在几周后跟进)

- 基于文件系统的资源索引( 在不重复爬行的情况下征服信息)

- 用于 pub / sub 的实时安全代码路径( 仅包含兼容的 DDS 执行)

- ROS 1和ROS 2之间的桥梁

- HSR 演示 [见Beta 3](Beta3-Overview.md)

- Turtlebot 演示 [见Beta 2](Beta2-Overview.md)

详细情况请参见: [特征](../The-ROS2-Project/Features.md) 页面。

<span id="changes-since-beta-3-release"></span>

### Beta 3 发布后的变化

自Beta 3发布以来的改进:

- `rviz`

- C++中消息数据结构不同的初始化选项(参见 [设计文件](https://design.ros2.org/articles/generated_interfaces_cpp.html#constructors))

- 登录 API 改进, 现在也用于演示

- C++ 中不同时钟的时间支持

- Python 客户端库中的等待服务支持

- 《公约》实施情况草案 [REP 149号文件](https://reps.openrobotics.org/rep-0149/) 指定软件包显示文件的格式 3

<span id="known-issues"></span>

## 已知问题

- 有图像演示等较大数据的快速RTPS性能

- 使用 Connext 目前不允许两个具有相同基名但不同命名空间的话题有不同的类型( 请参见 ) [问题](https://github.com/ros2/rmw_connext/issues/234)).

- 列出节点名称(例如: `ros2 node list`)在一些 Rmw 执行过程中没有发挥作用。

- 在 Windows Python 启动文件上, 当尝试中止时可能会挂起 `Ctrl-C` (见 [问题](https://github.com/ros2/launch/issues/64)。为了继续使用被挂起命令屏蔽的 shell, 您可能想要使用进程显示器结束挂起的 Python 进程 。
