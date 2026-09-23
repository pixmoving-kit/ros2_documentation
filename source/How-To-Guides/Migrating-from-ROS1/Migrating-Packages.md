<span id="migrating-packages"></span>
# 迁移软件包

软件包迁移分为两类：

- 将现有软件包的源码从 ROS 1 迁移到 ROS 2，并希望大部分源码保持不变，或至少保持相似。例如，[pluginlib](https://github.com/ros/pluginlib) 在同一仓库的不同分支中维护源码，必要时可以在分支之间移植通用补丁。
- 在 ROS 2 中实现与某个 ROS 1 软件包相同或相似的功能，但允许源码有很大差异。例如，ROS 1 的 [roscpp](https://github.com/ros/ros_comm/tree/melodic-devel/clients/roscpp) 和 ROS 2 的 [rclcpp](https://github.com/ros2/rclcpp/tree/rolling/rclcpp) 位于独立仓库中，不共享代码。

<span id="prerequisites"></span>
## 前提条件

将 ROS 1 软件包迁移到 ROS 2 之前，它的所有依赖项都必须已在 ROS 2 中可用。

<span id="package-xml-format-version"></span>
## package.xml 格式版本

ROS 2 只支持格式版本为 2 或更高的 `package.xml`。如果软件包仍使用格式 1，请按照 [package.xml 从格式 1 迁移到格式 2 的指南](Migrating-Package-XML.md)更新。

<span id="dependency-names"></span>
## 依赖项名称

来自 [rosdep](../../Tutorials/Intermediate/Rosdep.md) 的依赖项名称应无需修改，因为 ROS 1 和 ROS 2 共享这些名称。

某些已发布到 ROS 的软件包在 ROS 2 中可能使用不同名称，因此可能需要相应更新依赖项。

<span id="metapackages"></span>
## 元包

ROS 2 没有专门的元包类型。元包仍可以作为只包含运行时依赖项的普通软件包存在。从 ROS 1 迁移元包时，只需删除软件包清单中的 `<metapackage />` 标签。有关元包和变体的更多信息，见[使用变体](../Using-Variants.md)。

<span id="licensing"></span>
## 许可证

ROS 1 推荐使用 [BSD 3-Clause 许可证](https://opensource.org/licenses/BSD-3-Clause)，ROS 2 推荐使用 [Apache 2.0 许可证](https://www.apache.org/licenses/LICENSE-2.0)。

对于任何新项目，无论使用 ROS 1 还是 ROS 2，都推荐采用 Apache 2.0 许可证。

不过，将 ROS 1 代码迁移到 ROS 2 时，不能直接更改许可证。已有贡献必须保留原有许可证。

因此，迁移软件包时，建议保留现有许可证，并继续按原有的 OSI 许可证向该软件包贡献代码；对于核心组件，原许可证通常是 BSD 许可证。这样可以保持许可证关系清晰、易于理解。

<span id="changing-the-license"></span>
### 更改许可证

可以更改许可证，但需要联系所有贡献者并获得许可。对于大多数软件包，这可能需要大量工作，不值得考虑。如果贡献者人数很少，则可能可行。
