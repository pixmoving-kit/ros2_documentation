---
translation_status: machine_translated
source: How-To-Guides/Migrating-from-ROS1.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="migrating-from-ros-1-to-ros-2"></span>

# 从 ROS 1 迁移到 ROS 2

这些指南显示如何将现有的ROS 1 软件包转换为ROS 2,如果在ROS 1 和ROS 2之间移植是新的,建议通过指南按顺序阅读.

- [迁移软件包](Migrating-from-ROS1/Migrating-Packages.md)
- [将 package.xml 迁移至格式 2](Migrating-from-ROS1/Migrating-Package-XML.md)
- [迁移接口](Migrating-from-ROS1/Migrating-Interfaces.md)
- [C++ 软件包迁移示例](Migrating-from-ROS1/Migrating-CPP-Package-Example.md)
- [C++ 软件包迁移参考](Migrating-from-ROS1/Migrating-CPP-Packages.md)
- [Python 软件包迁移示例](Migrating-from-ROS1/Migrating-Python-Package-Example.md)
- [Python 软件包迁移参考](Migrating-from-ROS1/Migrating-Python-Packages.md)
- [迁移启动文件](Migrating-from-ROS1/Migrating-Launch-Files.md)
- [迁移参数](Migrating-from-ROS1/Migrating-Parameters.md)
- [迁移脚本](Migrating-from-ROS1/Migrating-Scripts.md)

<span id="automatic-tools"></span>

## 自动工具

也有一些自动转换工具存在,尽管它们并非详尽无遗:

- [魔法 ROS 2 转换工具](https://github.com/DLu/roscompile/tree/main/magical_ros2_conversion_tool)

- 将 ROS 1 XML 发射文件转换为 ROS 2 Python 发射文件的发射文件模拟器 : <https://github.com/aws-robotics/ros2-launch-file-migrator>

- 亚马逊公司提供了从ROS 1移植到ROS 2的工具: <https://github.com/awslabs/ros2-migration-tools/tree/master/porting_tools>

- [缩略语2](https://github.com/dheera/rospy2) Python 工程将 Rospy 调用自动转换为 rclpy 调用
