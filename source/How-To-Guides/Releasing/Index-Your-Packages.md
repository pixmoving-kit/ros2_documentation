<span id="index-your-packages"></span>
# 为软件包建立索引

准备将一个新的 ROS 软件包发布到 ROS 发行版中？先为软件包建立索引，可以加快发布流程。

<span id="put-your-ros-packages-into-a-public-repository"></span>
## 将 ROS 软件包放入公开仓库

如果还没有这样做，请将 ROS 软件包的源码放入公开的 Git 仓库。所有发布到 ROS 中的软件包都必须开源。代码可以托管在任何地方，但推荐使用 GitHub，因为它支持启用拉取请求构建任务。可选平台包括：

- [GitHub](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)（**推荐**）。
- [GitLab](https://docs.gitlab.com/ee/user/project/repository/)。
- [Bitbucket](https://support.atlassian.com/bitbucket-cloud/docs/create-a-git-repository/)。

<span id="give-your-packages-an-osi-approved-license"></span>
## 为软件包选择 OSI 批准的许可证

为 ROS 软件包选择一种 [OSI 批准的许可证](https://opensource.org/licenses)。如果难以决定，可以考虑采用大多数 ROS 2 核心软件包使用的 [Apache-2.0 许可证](https://opensource.org/license/apache-2-0)。

在仓库中每个 `package.xml` 的 `<license>` 标签中，填写该许可证的 SPDX 短标识符。

如果所有 ROS 软件包使用相同许可证，或者仓库中只有一个 ROS 软件包，请在仓库根目录创建 `LICENSE` 文件，填入所选许可证的文本。如果各软件包使用不同许可证，则应在每个 `package.xml` 旁分别创建 `LICENSE` 文件。

<span id="give-your-packages-rep-144-compliant-names"></span>
## 使用符合 REP 144 的软件包名称

发布到 ROS 发行版中的软件包，其名称必须符合 [REP 144](https://reps.openrobotics.org/rep-0144/)。请阅读完整 REP 以了解规则。如果某个软件包名称不符合要求，请先修改名称再继续。

<span id="decide-what-ros-distribution-you-want-to-release-into"></span>
## 确定目标 ROS 发行版

决定要将软件包发布到哪些 ROS 发行版。至少应发布到 [ROS Rolling](https://docs.ros.org/en/rolling)，这样软件包就会自动包含在下一个 ROS 发行版中。也可以根据需要发布到其他仍受支持的 ROS 发行版。

<span id="create-a-github-account"></span>
## 创建 GitHub 账号

如果还没有 GitHub 账号，请[创建一个](https://docs.github.com/en/get-started/start-your-journey/creating-an-account-on-github)。ROS 软件包源码不必托管在 GitHub 上，但建立索引和发布软件包需要使用 GitHub 账号。

<span id="fork-and-clone-ros-rosdistro"></span>
## Fork 并克隆 ros/rosdistro

[Fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo) [ros/rosdistro](https://github.com/ros/rosdistro/) 仓库。每个账号只需要执行一次此步骤，之后每次发布都会使用这个 fork。

<span id="make-changes-to-your-fork"></span>
## 修改你的 fork

回顾之前选定的目标 ROS 发行版。[ros/rosdistro](https://github.com/ros/rosdistro/) 仓库为每个发行版提供一个文件夹，例如 ROS Rolling 对应的文件夹名为 `rolling`。对每个目标发行版，都执行以下步骤：

1. 填写下面的模板。
2. 将填写后的内容加入对应发行版文件夹中的 `distribution.yaml`。
3. 确保仓库名称所在的位置符合 YAML 文件中的字母排序。

```yaml
YOUR-REPO-NAME:
  source:
    type: git
    url: https://YOUR-GIT-REPO-URL.git
    version: YOUR-BRANCH-NAME
  status: YOUR-STATUS
```

各项填写方式如下：

- `YOUR-REPO-NAME`：便于阅读的仓库名称。对于托管在 GitHub 上的仓库，使用不含组织名称的小写仓库名。例如，`https://github.com/ros2/rosidl` 的仓库名为 `rosidl`。
- `YOUR-GIT-REPO-URL`：可用于 `git clone` 的 HTTPS 地址。例如，`https://github.com/ros2/rosidl` 对应的 Git 仓库 URL 为 `https://github.com/ros2/rosidl.git`。该 URL 必须以 `.git` 结尾，否则无法通过格式检查。
- `YOUR-BRANCH-NAME`：用于向此 ROS 发行版发布软件包的 Git 分支。常见值为 `main`、`master`，或 ROS 发行版名称。例如，[rosidl 仓库](https://github.com/ros2/rosidl)使用 `rolling` 分支保存要发布到 ROS Rolling 的更改。
- `YOUR-STATUS`：从 [REP 141](https://reps.openrobotics.org/rep-0141/#distribution-file) 列出的状态中选择，通常使用 `maintained` 或 `developed`。

<span id="open-a-pull-request-to-ros-rosdistro"></span>
## 向 ros/rosdistro 提交拉取请求

使用包含更改的分支，向 [ros/rosdistro](https://github.com/ros/rosdistro/) [提交拉取请求](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)，然后等待几天进行审查。

<span id="what-happens-next"></span>
## 接下来会发生什么

至此，为 ROS 软件包建立索引所需的操作已经完成。审查者会检查拉取请求是否[符合审查指南](https://github.com/ros/rosdistro/blob/master/REVIEW_GUIDELINES.md)，可能直接批准更改，也可能提出具体的修改建议。拉取请求符合要求并合并后，软件包就会出现在 [ROS Index](https://index.ros.org/) 中。

你已经完成了软件包发布前的重要一步。接下来请阅读[首次发布](First-Time-Release.md)指南。
