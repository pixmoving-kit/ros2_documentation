---
translation_status: machine_translated
source: Tutorials/Intermediate/Launch/Launch-Main.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="launch"></span> <span id="launchfilesmain"></span>

# 启动

ROS 2 发射文件允许您同时启动并配置包含 ROS 2 节点的若干可执行文件.

1.  [创建启动文件](Creating-Launch-Files.md).

    学习如何创建一个将同时启动节点及其全部配置的发射文件.

2.  [发射和监测多个节点](Launch-system.md).

    获得发射文件如何工作的更高级的综述.

3.  [使用替换表达式](Using-Substitutions.md).

    在描述可重复使用的发射文件时使用替代来提供更大的灵活性.

4.  [使用事件处理器](Using-Event-Handlers.md).

    使用事件处理器来监视进程状态或者定义一套复杂的规则,可以用来动态修改发射文件.

5.  [管理大型项目](Using-ROS2-Launch-For-Large-Projects.md).

    大型项目的结构启动文件, 这样它们可以在不同的情况下尽可能地被重新使用。 请参见参数, YAML 文件, 重映射, 命名空间, 默认参数, 和 RViz 配置等不同启动工具的用例 。

> **说明**
>
> 如果你来自ROS 1,你可以使用 [ROS 启动迁移指南](../../../How-To-Guides/Migrating-from-ROS1/Migrating-Launch-Files.md) 帮助您将您的发射文件迁移到 ROS 2 。
