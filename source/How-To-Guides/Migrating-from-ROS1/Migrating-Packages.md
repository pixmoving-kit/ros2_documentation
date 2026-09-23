---
translation_status: machine_translated
source: How-To-Guides/Migrating-from-ROS1/Migrating-Packages.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="migrating-packages"></span>

# 迁移软件包

包迁移有两种不同的类型: 包迁移.

- 将一个现有软件包的源代码从ROS 1 移动到ROS 2 , 其意图是源代码的相当一部分将保持不变或至少相似。 例如, [插件lib](https://github.com/ros/pluginlib) 其中源代码在同一个寄存器的不同分支中维护,必要时可以在这些分支之间移植共同的补丁.

- 为ROS 2 执行 ROS 1 软件包的相同或类似功能,但假定源代码会有很大不同。 [罗斯普](https://github.com/ros/ros_comm/tree/melodic-devel/clients/roscpp) 在《俄罗斯联邦规则》第1条和 [rclcpp](https://github.com/ros2/rclcpp/tree/rolling/rclcpp) 在ROS 2中, 它们是单独的寄存器, 并且不共享任何代码 。

<span id="prerequisites"></span>

## 前提条件

在能够将 ROS 1 套件迁移到 ROS 2 之前,其所有依赖必须可用 ROS 2 。

<span id="package-xml-format-version"></span>

## 软件包.xml 格式版本

ROS 2 仅支持 `package.xml` 格式 2 及更高版本。如果您的软件包 `package.xml` 使用格式 1, 然后使用 [package.xml 格式 1 到 2 迁移指南](Migrating-Package-XML.md).

<span id="dependency-names"></span>

## 附属名称

来源的依赖性名称 [rosdep](../../Tutorials/Intermediate/Rosdep.md) 无需改变,因为这些做法在俄罗斯第一区域办事处和俄罗斯第二区域办事处之间共有。

一些放入ROS的软件包在ROS 2中可能有不同的名称,因此依赖性可能需要相应更新.

<span id="metapackages"></span>

## 元数据包

ROS 2 没有特殊元包类型。 元包仍然可以作为只包含运行时依赖性的常规包存在。 当从 ROS 1 迁移元包时, 只需移除 `<metapackage />` 标记在您的软件包列表中。参见 [使用变体](../Using-Variants.md) 需要更多关于元包/变异物的信息。

<span id="licensing"></span>

## 许可证发放

在ROS 1中,我们推荐的执照是 [3- Clause BSD 许可证](https://opensource.org/licenses/BSD-3-Clause)在ROS 2中,我们推荐的执照是 [Apache 2.0 许可证](https://www.apache.org/licenses/LICENSE-2.0).

对于任何新项目,我们建议使用Apache 2.0许可,无论是ROS 1还是ROS 2.

然而,当从ROS 1 向ROS 2 迁移时,我们不能简单地改变许可证。 现有的许可证必须保留,以备任何原有的缴款。

为此,如果正在迁移一个软件包,我们建议保留现有的许可证,并继续根据现有的OSI许可证为这个软件包作出贡献,我们期望这是核心要素的BSD许可证。

这将使事情变得清晰和容易理解。

<span id="changing-the-license"></span>

### 更改许可证

修改许可证是可能的, 但是您需要联系所有投稿人并获得许可。 对于大多数的软件包来说, 这可能是一项重大的努力, 也不值得考虑。 如果软件包有少量投稿人, 那么这可能可行 。
