---
translation_status: machine_translated
source: The-ROS2-Project/Platform-EOL-Policy.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="platform-eol-policy"></span> <span id="platformeolpolicy"></span>

# 平台支持终止政策

[ROS 发行版](../Releases.md) 不支持报废(EOL)平台,即使ROS发行仍在运行中。本页解释:

- EOL平台的用户应该期待什么?

- 罗斯老板应该做什么?

<span id="policy"></span>

## 政策

每个ROS分布支持某些 **平台**,例如Windows 11或Ubuntu 24.04. **供应商** 在这些平台中,例如微软或Canonical,可以决定它们支持其中一个平台的时间。当一个供应商决定一个平台已经到达EOL时,它们通常会停止发布关键的bug和安全修正。为了保护自己,我们主动地从ROS建设农场中移除EOL平台上的所有工作。

如果您正在使用一个不再得到其供应商支持的平台,您应该期待停止收到更新的ROS软件包。现有的ROS软件包将仍然可用且可操作,但将不再更新。然而,ROS Bosses可能选择在特殊情况下在EOL软件平台上更新软件包。

<span id="for-ros-bosses"></span>

## 给罗斯老板

在目标平台到达EOL之前:

- 确保ROS发行文档包括ROS发行前到达EOL的任何平台的EOL日期.

- 发布关于平台达到EOL至少2同步(大约60-90天)的公告,以使软件包维护者有时间更新其软件包.

- 打开 a [拉动请求使该平台的建设农业工作失去功能](https://github.com/ros2/ros_buildfarm_config) ,并寻求审查 [基础设施PMC](https://osralliance.org/wp-content/uploads/2024/03/infrastructure_project_charter.pdf).

- 最后一次同步到那个平台

在目标平台到达EOL后:

- 更新 ROS 分发文件以声明平台不会收到更新的 ROS 软件包 。

- 宣布ROS的发行 已经放弃了对Discour的平台的支持。

- 考虑最后一次向该平台发布,如果:  
  - 在EOL之前你还没有这么做,而且

  - 更新似乎不可能出现倒退,以及

  - ROS Buildfarm 仍然有选手在那个平台。

- 合并您的拉动请求以禁用建设农场的工作 。
