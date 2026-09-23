---
translation_status: machine_translated
source: How-To-Guides/Releasing/Release-Team-Repository.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="release-team-repository"></span>

# 发布团队与仓库

本页面解释您所推荐的发布寄存器的托管方法 [ros2-gbp 缩写](https://github.com/ros2-gbp).

<span id="what-is-ros-2-gbp"></span>

## 罗斯2英镑是什么?

[ros2-gbp 缩写](https://github.com/ros2-gbp) 是一个 GitHub 组织,它托管ROS 包的发布存储器。它还维护着发布团队列表,每个发布团队的成员列表以及发布团队维护的发布存储器列表. <https://github.com/ros2-gbp/ros2-gbp-github-org>。与 ros2- gbp- github- org 的互动是通过提出 GitHub 问题完成的。建议您请求加入释放小组,并尽早建立一个释放存储器,因为 ros2- gbp 维护者需要一些时间才能响应您的请求。

<span id="what-is-a-release-team"></span> <span id="id2"></span>

## 释放小组是什么?

释放团队是 [GitHub 团队](https://docs.github.com/en/organizations/organizing-members-into-teams/about-teams) 释放小组通常由一个组织、一个工作组甚至一个个人组成,并以他们所代表的小组或团体的名字命名。 [罗斯2-gbp-github-org](https://github.com/ros2-gbp/ros2-gbp-github-org).

**你一定是计划放出项目的释放团队的一员** 如果您打算在现有团队下发布存储器, 请遵循 [加入释放小组](#join-a-release-team)。如果打算组建新的团队,请跟随 [启动新的发布团队](#start-a-new-release-team).

<span id="join-a-release-team"></span> <span id="id3"></span>

### 加入释放小组

充电 [更新发布小组成员问题](https://github.com/ros2-gbp/ros2-gbp-github-org/issues/new?assignees=&labels=&template=update_release_team_membership.md&title=Update+release+team+membership) 如果您的工程已经存在释放团队, 但您不是其中的一部分, 则发布模板 。

<span id="start-a-new-release-team"></span> <span id="id4"></span>

### 启动新的发布团队

如果您的项目还没有解禁小组,请填写 [新发行小组问题](https://github.com/ros2-gbp/ros2-gbp-github-org/issues/new?assignees=&labels=&template=new_release_team.md&title=Add+release+team) 创建要请求的发行模板。

<span id="what-is-a-release-repository"></span> <span id="id5"></span>

## 什么是发布存储器?

释放寄存器是一个寄存器

- 存储发布过程中生成的文件,供 ROS 构建农场使用

- 发布过程中的缓存配置,以简化寄存器日后的发布

从您的源代码寄存器中分离一个发布存储器,是ROS 2中实现发布的一个要求.

<span id="create-a-new-release-repository"></span> <span id="id6"></span>

### 创建新发行仓库

如果您的寄存器是 ROS 社区的新文件, 您应该首先打开一个拉请求 [ros/rosdistro](https://github.com/ros/rosdistro) 添加一个 `source` 输入您的寄存器( 例如) 。 <https://github.com/ros/rosdistro/pull/39513>) Rostistro数据库的审查进程将确保您的存储器和软件包符合 [REP 144 一揽子命名公约](https://reps.openrobotics.org/rep-0144/) 和发布前的其他要求。一旦您的软件包名称得到批准和合并,请填写 [添加新发行仓库问题](https://github.com/ros2-gbp/ros2-gbp-github-org/issues/new?assignees=&labels=&template=new_release_repository.md&title=Add+new+release+repositories) 如果您还没有为您的项目重新发布协议, 则会发布模板 。

<span id="what-if-my-existing-release-repo-isn-t-on-ros2-gbp"></span>

## 如果我现在的释放回波不在ROS2-gbp上呢?

在 ros2- gbp 存在之前发布的软件包可能会被托管到其他地方。 现在强烈建议将软件包放入这个专门的 GitHub 组织。 如果您要将 ROS 1 软件包移植到 ROS 2 , 并计划首次将您的软件包放入 ROS 2 , 请遵循标准程序, 为您的 ROS 2 版本请求新的软件包放入 ROS 2 。 如果您先前已经将您的软件包放入 ROS 2 时, 请遵循标准程序 。 [添加新发行仓库问题](https://github.com/ros2-gbp/ros2-gbp-github-org/issues/new?assignees=&labels=&template=new_release_repository.md&title=Add+new+release+repositories), **指定您的当前发布仓库 URL**,并遵循其余的标准程序。

> **说明**
>
> **在将您的软件包放入滚滚分发时, 您必须使用 ros2- gbp 组织中托管的发布寄存器**。如果您不计划将寄存器放入滚存器中,则其他寄存器的释放寄存器仍然被支持进行稳定的分发。由于从滚存器创造的稳定分发将始于在 ros2-gbp 组织中的释放寄存器,因此建议您对所有 ROS 2 分发器使用 ros2-gbp 释放寄存器,以避免分散释放信息。
>
> ros2-gbp发布存储器将来可能会成为所有Distro的硬性要求,并且对所有ROS 2发布器维持一个单一的发布存储器,简化了Rolling发布器和包维护器的发布器的维护.
