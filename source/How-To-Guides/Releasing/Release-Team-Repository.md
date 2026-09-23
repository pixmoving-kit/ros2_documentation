<span id="release-team-repository"></span>
# 发布团队与发布仓库

本页介绍将发布仓库托管在 [ros2-gbp](https://github.com/ros2-gbp) 上的推荐方式。

<span id="what-is-ros-2-gbp"></span>
## 什么是 ROS 2 GBP？

[ros2-gbp](https://github.com/ros2-gbp) 是一个 GitHub 组织，用于托管 ROS 软件包的发布仓库。它还通过 [ros2-gbp-github-org](https://github.com/ros2-gbp/ros2-gbp-github-org) 维护发布团队列表、各团队成员列表，以及各团队负责的发布仓库列表。与 ros2-gbp-github-org 的交互通过提交 GitHub issue 完成。建议尽早申请加入发布团队并建立发布仓库，因为维护者处理请求可能需要一些时间。

<span id="what-is-a-release-team"></span> <span id="id2"></span>
## 什么是发布团队？

发布团队是一个 [GitHub 团队](https://docs.github.com/en/organizations/organizing-members-into-teams/about-teams)，由负责一个或多个仓库发布流程的人员组成。发布团队通常由一个组织、工作组，甚至单个人组成，并以其所代表的团队或群体命名。发布团队及其关联发布仓库的列表由 [ros2-gbp-github-org](https://github.com/ros2-gbp/ros2-gbp-github-org) 维护。

**你必须是所要发布项目对应发布团队的成员。** 如果要在现有团队下发布仓库，请[加入发布团队](#join-a-release-team)。如果要组建新团队，请[创建新的发布团队](#start-a-new-release-team)。

<span id="join-a-release-team"></span> <span id="id3"></span>
### 加入发布团队

如果项目已有发布团队，但你尚未加入，请填写 [Update Release Team Membership issue 模板](https://github.com/ros2-gbp/ros2-gbp-github-org/issues/new?assignees=&labels=&template=update_release_team_membership.md&title=Update+release+team+membership)。

<span id="start-a-new-release-team"></span> <span id="id4"></span>
### 创建新的发布团队

如果项目尚无发布团队，请填写 [New Release Team issue 模板](https://github.com/ros2-gbp/ros2-gbp-github-org/issues/new?assignees=&labels=&template=new_release_team.md&title=Add+release+team)，申请创建团队。

<span id="what-is-a-release-repository"></span> <span id="id5"></span>
## 什么是发布仓库？

发布仓库用于：

- 保存发布流程生成的文件，供 ROS 构建农场使用。
- 缓存发布流程的配置，以简化该仓库之后的发布操作。

在 ROS 2 中发布软件包，必须使用独立于源码仓库的发布仓库。

<span id="create-a-new-release-repository"></span> <span id="id6"></span>
### 创建新的发布仓库

如果你的仓库尚未加入 ROS 社区，应先向 [ros/rosdistro](https://github.com/ros/rosdistro) 提交拉取请求，为仓库添加一个 `source` 条目，参见[示例](https://github.com/ros/rosdistro/pull/39513)。rosdistro 数据库的审查流程会在发布前确认仓库及软件包符合 [REP 144 软件包命名规范](https://reps.openrobotics.org/rep-0144/)和其他要求。软件包名称获批、相关请求合并后，如果项目尚无发布仓库，请填写 [Add New Release Repositories issue 模板](https://github.com/ros2-gbp/ros2-gbp-github-org/issues/new?assignees=&labels=&template=new_release_repository.md&title=Add+new+release+repositories)。

<span id="what-if-my-existing-release-repo-isn-t-on-ros2-gbp"></span>
## 现有发布仓库不在 ros2-gbp 上怎么办？

在 ros2-gbp 成立前发布的软件包，其发布仓库可能托管在其他位置。现在强烈建议将发布仓库放在这个专用 GitHub 组织中。

如果正在将 ROS 1 软件包移植到 ROS 2，并计划首次发布 ROS 2 版本，请按标准流程为 ROS 2 发布申请新的发布仓库。如果之前已经发布过 ROS 2 版本，请在提交 [Add New Release Repositories issue](https://github.com/ros2-gbp/ros2-gbp-github-org/issues/new?assignees=&labels=&template=new_release_repository.md&title=Add+new+release+repositories) 时，**注明现有发布仓库的 URL**，其余步骤按标准流程操作。

!!! note "说明"
    **向 Rolling 发行版发布软件包时，必须使用托管在 ros2-gbp 组织中的发布仓库。** 如果不打算向 Rolling 发布，稳定发行版仍支持使用托管在其他位置的发布仓库。由于从 Rolling 派生的稳定发行版最初就会使用 ros2-gbp 中的发布仓库，建议所有 ROS 2 发行版都使用 ros2-gbp 发布仓库，以免发布信息分散。

    未来，所有发行版都可能强制要求使用 ros2-gbp 发布仓库。为所有 ROS 2 发行版维护同一个发布仓库，也能减轻 Rolling 发行版维护者和软件包维护者的发布维护工作。
