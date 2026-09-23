---
translation_status: machine_translated
source: Releases/Release-Bouncy-Bolson.rst
---

<span id="bouncy-bolson-bouncy"></span>

# Bouncy Bolson（`bouncy`)

*博尔森* 是ROS 2的第二次发布.

<span id="supported-platforms"></span>

## 支持的平台

这个版本的ROS 2在四个平台上得到支持(参见: [REP 2000 环境方案](https://reps.openrobotics.org/rep-2000/#bouncy-bolson-june-2018-june-2019) 详细情况:

- Ubuntu 18.04 (比奥尼克语).

  - Amd64和arm64的Debian包件

- 乌邦图16.04 (日语).

  - 没有 Debian 软件包, 但支持从源头构建

- Mac macOS 10.12 (塞拉利昂)

- Windows 10 与 Visual Studio 2017 (英语).

提供了二进制软件包和如何从源头编译的指令(见 [安装指令](../Installation.md) (a) 与《公约》有关的其他事项; [文档](https://docs.ros2.org/bouncy/)).

目标平台:

<table class="docutils align-default">
<thead>
<tr class="row-odd">
<th class="head"></th>
<th colspan="4" class="head"><p>所需支助</p></th>
<th class="head"><p>建议的支助</p></th>
</tr>
<tr class="row-even">
<th class="head"><p>建筑</p></th>
<th class="head"><p>乌邦图·比奥尼奇(18.04)</p></th>
<th class="head"><p>MacOS Sierra (10.12) (英语).</p></th>
<th class="head"><p>Windows 10 (VS 2017) (英语).</p></th>
<th class="head"><p>乌邦图薛尼勒(16.04)[s].</p></th>
<th class="head"><p>Debian 伸展 (9) [s]</p></th>
</tr>
</thead>
<tbody>
<tr class="row-odd">
<td><p>amd64 (中文(简体) ).</p></td>
<td><p>X</p></td>
<td><p>X</p></td>
<td><p>X</p></td>
<td><p>X [s]</p></td>
<td><p>X [s]</p></td>
</tr>
<tr class="row-even">
<td><p>军火64</p></td>
<td><p>X</p></td>
<td></td>
<td></td>
<td><p>X [s]</p></td>
<td><p>X [s]</p></td>
</tr>
</tbody>
</table>

" \[s\] " 从源头汇编,ROS建设农场不会为这些平台生产任何二进制包。

最低语文要求:

- C11\[^3\]

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
<th class="head"><p>视窗 10 **</p></th>
<th class="head"><p>乌本图仙霞</p></th>
<th class="head"><p>Debian 伸缩</p></th>
</tr>
</thead>
<tbody>
<tr class="row-odd">
<td><p>CMake</p></td>
<td><p>3.10.2</p></td>
<td><p>3.11.0</p></td>
<td><p>3.10.2</p></td>
<td><p>3.5.1</p></td>
<td><p>3.7.2</p></td>
</tr>
<tr class="row-even">
<td><p>爱咪</p></td>
<td><p>3.3.2</p></td>
<td><p>3.6.5</p></td>
<td><p>3.3.2</p></td>
<td><p>3.3.2</p></td>
<td><p>3.3.2</p></td>
</tr>
<tr class="row-odd">
<td><p>食人鱼</p></td>
<td><p>1.10*</p></td>
<td><p>1.10*</p></td>
<td><p>1.10*</p></td>
<td><p>1.10*</p></td>
<td><p>1.10*</p></td>
</tr>
<tr class="row-even">
<td><p>打开CV</p></td>
<td><p>3.2.0</p></td>
<td><p>3.4.1</p></td>
<td><p>3.4.1*</p></td>
<td><p>2.4.9</p></td>
<td><p>3.2*</p></td>
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
<td><p>3.6.5</p></td>
<td><p>3.6.5</p></td>
<td><p>3.5.1</p></td>
<td><p>3.5.3</p></td>
</tr>
<tr class="row-odd">
<td><p>Qt 键</p></td>
<td><p>5.9.5</p></td>
<td><p>5.10.0</p></td>
<td><p>5.10.0</p></td>
<td><p>5.5.1</p></td>
<td><p>5.7.1</p></td>
</tr>
<tr class="row-even">
<td colspan="6"><p><strong>仅限 Linux( 用于龟块演示)</strong></p></td>
</tr>
<tr class="row-odd">
<td><p>个人计算机L</p></td>
<td><p>1.8.1</p></td>
<td><p>N/A</p></td>
<td><p>N/A</p></td>
<td><p>1.7.2</p></td>
<td><p>1.8.0</p></td>
</tr>
</tbody>
</table>

" \* " 是指这不是上游版本(可在官方操作系统寄存器上查阅),而是OSRF或社区分发的包(自定义寄存器上建立和分发的包)。

" \*\* " 滚动分发会看到这些依附关系在其存在期间的多个版本的变化。

" \[s\] " 从源头汇编,ROS建设农场不会为这些平台生产任何二进制包。

此文档只抓取 ROS 发行的首次发布时的版本, 并且不会随着依赖关系向前移动而更新, 因此这些版本是低水印 。

依赖软件包管理器的使用 :

- 乌邦图比奥尼奇: apt

- 马科斯:土生土长,皮普

- 视窗:巧克力,pip

- Ubuntu Xenial, Debian 伸展: apt

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

- [新的发射系统](../Tutorials/Intermediate/Launch/Launch-system.md) 其特点是Python API的能力和灵活性要大得多。

- 参数可以作为 [命令行参数](../How-To-Guides/Node-arguments.md) 改为 C++ 可执行文件。

- 通过静态重新绘图 [命令行参数](../How-To-Guides/Node-arguments.md).

- Python客户端库的各种改进.

- 支持发布和订阅序列化数据,这是即将开展的本地罗包执行工作的基础。

- 更多( E) [命令行工具](../Concepts/Basic/About-Command-Line-Tools.md),例如,与参数和生命周期状态合作。

- 二进制包/脂肪档案默认支持三个 RMW 执行(不需要从源头构建) :

  - eProsima 快速RTPS( 默认)

  - RTI的连锁店

  - ADLINK 的 OpenSplice 软件

关于所有现有特征的概述,包括早先释放的特征,请参见: [特征](../The-ROS2-Project/Features.md) 页面。

<span id="changes-since-the-ardent-release"></span>

### 自参数发布以来的变化

自《公约》生效以来的变化 [缩写独立](Release-Ardent-Apalone.md) 释放 :

- Python 软件包 `launch` 已重新设计。上一个 Python API 已被移入子模块 `launch.legacy`。如果不需要向新的 Python API 过渡,您可以更新已有的发射文件,继续使用遗留的 API 。

- 包含命名空间的ROS主题名称被映射到DDS主题,包括其命名空间. DDS分区不再用于此.

- 推荐的建设工具现在 `colcon` 改为 `ament_tools`。此开关没有 [影响](https://design.ros2.org/articles/build_tool.html#implications) 用于每个ROS 2 包中的代码。安装指令已经更新, [读取文件页面](https://colcon.readthedocs.io/en/main/migration/ament_tools.html) 描述如何映射已有的 `ament_tools` 调用 `colcon`.

- 辩论顺序 [此 rclcpp: 节点: 创建\_ 订阅 () 签名](https://docs.ros2.org/bouncy/api/rclcpp/classrclcpp_1_1_node.html#a283fb006c46470cf43a4ae5ef4a16ccd) 已修改。

<span id="known-issues"></span>

## 已知问题

- 新风格的发射文件 [可能挂断](https://github.com/ros2/launch/issues/89) 用于某些平台和RMW执行的组合。

- 静态重映射命名空间 [工作不当](https://github.com/ros2/rcl/issues/262) 发往某个特定节点时。

- [可打印 Openspilice 错误消息](https://github.com/ros2/rmw_opensplice/issues/237) 使用时 `ros2 param` 财务报告和财务报告 `ros2 lifecycle` 命令行工具。
