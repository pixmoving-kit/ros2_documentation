<span id="package-docs"></span>

# 软件包文档

ROS 软件包文档，即通过 apt 或其他工具安装的具体软件包的文档，可以在多个位置找到。下面简要列出查找方式。

- 大多数 ROS 2 软件包的文档都收录在[软件包文档索引](https://docs.ros.org/en/rolling/p/)中。
- 所有 ROS 2 软件包的文档均可通过 [ROS Index](https://index.ros.org/) 中的软件包信息获取。在 ROS Index 中搜索软件包，可以查到已发布的发行版、`README.md` 文件、网址以及其他重要元数据。

<span id="larger-packages"></span>

## 大型软件包

MoveIt、Nav2、microROS 等大型软件包拥有独立域名或 ros.org 子域名，例如：

- [MoveIt](https://moveit.ai/)
- [Navigation2](https://nav2.org/)
- [Control](https://control.ros.org/master/index.html)
- [microROS（嵌入式系统）](https://micro.ros.org/)

<span id="api-documentation"></span>

## API 文档

以下链接提供 Rolling 发行版中 ROS 客户端库的 API 文档：

- [rclcpp：C++ 客户端库](https://docs.ros.org/en/rolling/p/rclcpp/generated/index.html)
- [rclcpp_lifecycle：C++ 生命周期库](https://docs.ros.org/en/rolling/p/rclcpp_lifecycle/generated/index.html)
- [rclcpp_components：C++ 组件库](https://docs.ros.org/en/rolling/p/rclcpp_components/generated/index.html)
- [rclcpp_action：C++ 动作库](https://docs.ros.org/en/rolling/p/rclcpp_action/generated/index.html)

<span id="adding-your-package-to-docs-ros-org"></span>

## 将软件包添加到 docs.ros.org

所有已发布的 ROS 2 软件包都会自动添加到 docs.ros.org 和 [ROS Index](https://index.ros.org/)。要启用或配置自己软件包的文档，请参阅[编写 ROS 2 软件包文档](How-To-Guides/Documenting-a-ROS-2-Package.md)。
