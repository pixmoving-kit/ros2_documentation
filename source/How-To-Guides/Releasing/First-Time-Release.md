<span id="first-time-release"></span>
# 首次发布

本指南介绍如何发布此前尚未发布过的 ROS 2 软件包。由于发布 ROS 软件包时有许多可选方式，本指南只介绍最常见的场景，不涵盖所有特殊情况。

<span id="be-part-of-a-release-team"></span>
## 加入发布团队

你必须属于某个[发布团队](Release-Team-Repository.md#what-is-a-release-team)。如果还没有加入，请选择以下一种方式：

- [加入现有发布团队](Release-Team-Repository.md#join-a-release-team)。
- [创建新的发布团队](Release-Team-Repository.md#start-a-new-release-team)。

<span id="create-a-new-release-repository"></span>
## 创建新的发布仓库

发布软件包需要一个[发布仓库](Release-Team-Repository.md#what-is-a-release-repository)。请按[创建新的发布仓库](Release-Team-Repository.md#create-a-new-release-repository)中的说明操作。

<span id="install-dependencies"></span>
## 安装依赖项

参见[安装依赖项](_Install-Dependencies.md)。

<span id="set-up-a-personal-access-token"></span>
## 设置个人访问令牌

参见[设置个人访问令牌](_Personal-Access-Token.md)。

<span id="ensure-repositories-are-up-to-date"></span>
## 确保仓库保持最新

参见[更新仓库](_Ensure-Repositories-Are-Up-To-Date.md)。

<span id="generate-changelog"></span>
## 生成变更日志

使用以下命令，为仓库中的每个软件包生成一个 `CHANGELOG.rst` 文件：

```console
$ catkin_generate_changelog --all
```

然后[整理变更日志](_Clean-Up-Changelog.md)。

<span id="bump-the-package-version"></span>
## 更新软件包版本号

参见[更新软件包版本号](_Bump-Package-Version.md)。

<span id="bloom-release"></span>
## 使用 Bloom 发布

运行以下命令，将 `my_repo` 替换为你的仓库名称：

```console
$ bloom-release --new-track --rosdistro rolling --track rolling my_repo
```

!!! tip "提示"
    - `--new-track` 告诉 bloom 创建并配置一个新的[发布轨道](Release-Track.md#what-is-a-track)。
    - `--rosdistro rolling` 表示本次发布面向 `rolling` 发行版。
    - `--track rolling` 表示发布轨道的名称为 `rolling`。

随后，工具会提示你输入信息，配置新的发布轨道。假设你的情况如下：

- 软件包位于名为 `my_repo` 的仓库中。
- 要发布的分支名为 `main`。
- 仓库托管在 GitHub，地址为 `https://github.com/my_organization/my_repo.git`。
- 发布仓库的地址为 `https://github.com/ros2-gbp/my_repo-release.git`。

应按下表回答提示：

| 配置项 | 值 |
| --- | --- |
| [发布仓库 URL](Release-Track.md#release-repository-url) | `https://github.com/ros2-gbp/my_repo-release.git` |
| [仓库名称](Release-Track.md#repository-name) | `my_repo` |
| [上游仓库 URI](Release-Track.md#upstream-repository-uri) | `https://github.com/my_organization/my_repo.git` |
| [上游版本控制系统类型](Release-Track.md#upstream-vcs-type) | |
| [版本](Release-Track.md#version) | |
| [发布标签](Release-Track.md#release-tag) | |
| [上游开发分支](Release-Track.md#upstream-devel-branch) | `main` |
| [ROS 发行版](Release-Track.md#ros-distro) | |
| [补丁目录](Release-Track.md#patches-directory) | |
| [发布仓库推送 URL](Release-Track.md#release-repository-push-url) | |

!!! note "说明"
    表格中的空白单元格表示使用默认值，在相应提示处直接按 Enter 即可。

Bloom 会自动向 [rosdistro](https://github.com/ros/rosdistro) 提交拉取请求。

!!! note "说明"
    默认情况下，bloom 会发布源码仓库中的所有软件包。如果希望在 `rolling` 发行版中排除某些软件包，请在发布仓库的 `master` 分支添加 `rolling.ignored` 文件。文件中每行列出一个要排除的软件包名称。[rosidl-release](https://github.com/ros2-gbp/rosidl-release) 仓库可作为这种配置的参考。

<span id="next-steps"></span>
## 后续步骤

参见[发布后的后续步骤](_Next-Steps.md)。
