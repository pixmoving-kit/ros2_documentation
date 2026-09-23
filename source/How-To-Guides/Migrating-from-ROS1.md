<span id="migrating-from-ros-1-to-ros-2"></span>
# 从 ROS 1 迁移到 ROS 2

这些指南介绍如何将现有的 ROS 1 软件包转换为 ROS 2 软件包。如果尚不熟悉 ROS 1 与 ROS 2 之间的移植，建议按顺序阅读。

- [迁移软件包](Migrating-from-ROS1/Migrating-Packages.md)
- [迁移 package.xml](Migrating-from-ROS1/Migrating-Package-XML.md)
- [迁移接口](Migrating-from-ROS1/Migrating-Interfaces.md)
- [C++ 软件包迁移示例](Migrating-from-ROS1/Migrating-CPP-Package-Example.md)
- [迁移 C++ 软件包](Migrating-from-ROS1/Migrating-CPP-Packages.md)
- [Python 软件包迁移示例](Migrating-from-ROS1/Migrating-Python-Package-Example.md)
- [迁移 Python 软件包](Migrating-from-ROS1/Migrating-Python-Packages.md)
- [迁移启动文件](Migrating-from-ROS1/Migrating-Launch-Files.md)
- [迁移参数](Migrating-from-ROS1/Migrating-Parameters.md)
- [迁移脚本](Migrating-from-ROS1/Migrating-Scripts.md)

<span id="automatic-tools"></span>
## 自动化工具

也有一些自动转换工具可供使用，但它们不能覆盖所有迁移需求：

- [Magical ROS 2 Conversion Tool](https://github.com/DLu/roscompile/tree/main/magical_ros2_conversion_tool)。
- [Launch File migrator](https://github.com/aws-robotics/ros2-launch-file-migrator)：将 ROS 1 XML 启动文件转换为 ROS 2 Python 启动文件。
- Amazon 发布的 [ROS 1 到 ROS 2 移植工具](https://github.com/awslabs/ros2-migration-tools/tree/master/porting_tools)。
- [rospy2](https://github.com/dheera/rospy2)：自动将 rospy 调用转换为 rclpy 调用的 Python 项目。
