---
translation_status: machine_translated
source: How-To-Guides/Releasing/Index-Your-Packages.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="index-your-packages"></span>

# 为软件包建立索引

您是否将一个新的 ROS 软件包放入 ROS 分布中 ? 通过先对您的软件包进行索引, 使进程更快 。

<span id="put-your-ros-packages-into-a-public-repository"></span>

## 将您的ROS 软件包放入公共仓库

如果您还没有这样做, 请将 ROS 软件包的源代码放入一个公共 git 仓库。 所有放入 ROS 的软件包都必须是开源的。 您可以在任何地方主机代码, 但是 GitHub 被推荐, 因为它给了您一个选项来启用拉动请求任务 。 以下是一些选项 :

- [GitHub](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository) **建议**

- [吉特拉布语Name](https://docs.gitlab.com/ee/user/project/repository/)

- [位点](https://support.atlassian.com/bitbucket-cloud/docs/create-a-git-repository/)

<span id="give-your-packages-an-osi-approved-license"></span>

## 给您的包 OSI 许可

选择一个 [OSI 核准许可证](https://opensource.org/licenses) 如果您无法决定, 请考虑使用 ROS 2 核心软件包所使用的许可证 : [Apache-2.0 许可证](https://opensource.org/license/apache-2-0).

每笔 `package.xml` 在您的仓库中, 将 SPDX 许可证的简短标识符放入 `<license>` 标签在您的 `package.xml`.

如果您的所有ROS软件包都有相同的许可证, 或者您的仓库中只有一个ROS软件包, 请创建一个名为 `LICENSE` 将您选择的许可证文本放在您的仓库的根部。 如果您仓库中的ROS 软件包有不同的许可证, 请创建 `LICENSE` 与每个文件相邻 `package.xml` 文档。

<span id="give-your-packages-rep-144-compliant-names"></span>

## 给您的包 REP 144 符合要求的名称

释放到ROS分发的软件包必须具有符合下列要求的名称: [REP 144 (中文(简体) ).](https://reps.openrobotics.org/rep-0144/)。读取完整的REP以了解规则。如果您的ROS 包名称不符合,请在继续前更改名称。

<span id="decide-what-ros-distribution-you-want-to-release-into"></span>

## 决定您想要放入的 ROS 分布

决定您想要放出您的软件包的ROS 分布。 至少您应该放入您的软件包 。 [ROS 滚转](https://docs.ros.org/en/rolling) 这样,您的ROS软件包将自动包含在下一次ROS发行中。您也可能想要释放到任何活跃的ROS发行中,但这取决于您。

<span id="create-a-github-account"></span>

## 创建 GitHub 账户

[创建 GitHub 账户](https://docs.github.com/en/get-started/start-your-journey/creating-an-account-on-github) 如果您还没有。 您不需要在 GitHub 上托管 ROS 软件包的源代码, 但是您需要一个账户来索引和发布软件包 。

<span id="fork-and-clone-ros-rosdistro"></span>

## 叉和克隆 ros/rosdistro

[叉子](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo) 编号 [ros/rosdistro](https://github.com/ros/rosdistro/) 仓库。您只需在您的账户上做一次这一步骤。每次您进行发布时,叉子都会被使用。

<span id="make-changes-to-your-fork"></span>

## 改变你的叉子

记得您决定放入的 ROS 发行版本吗 ? 每个 ROS 发行版本都有文件夹 。 [ros/rosdistro](https://github.com/ros/rosdistro/) 存储器。例如,ROS滚动文件夹的名称是 `rolling`。对于每个 ROS 分布,您想要放入:

1.  填写以下模板

2.  将填充模板放入 `distribution.yaml` 在相应的 ROS 分发文件夹中的文件

3.  确保MY-REPO-NAME在Yaml文件中的字母顺序中位于正确的位置

``` yaml
YOUR-REPO-NAME:
  source:
    type: git
    url: https://YOUR-GIT-REPO-URL.git
    version: YOUR-BRANCH-NAME
  status: YOUR-STATUS
```

以下是每个项目的填写方式:

- Your-REPO-NAME : 这是一个任意的人类可读名称。 对于在 GitHub 上主机的重置程序, 请使用您寄存器的小写名称, 不包括组织。 例如, 寄存器名称 `https://github.com/ros2/rosidl` 是,这是 `rosidl`.

- 这是https URL,人们可以从中找到的 `git clone` 您的仓库, 例如: git repo URL `https://github.com/ros2/rosidl` 是,这是 `https://github.com/ros2/rosidl.git`。重要的是,此 URL 结束于 `.git`,否则它不会通过 linters 。

- 这是您寄存器上的 git 分支, 您将从中将您的软件包放入此 ROS 分布 。 这通常是 : `main`, `master`,或 ROS 分布本身的名称。例如, [rosidl 仓库](https://github.com/ros2/rosidl) 使用分支 `rolling` 将更改保存到 ROS Rolling 中。

- 这是从列表中的状态 。 [REP 141 (中文(简体) ).](https://reps.openrobotics.org/rep-0141/#distribution-file)。您可能想要的任意一个 `maintained` 或 时 间 `developed`.

<span id="open-a-pull-request-to-ros-rosdistro"></span>

## 打开向 ros/ rosdistro 的拉动请求

[打开拉动请求](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request) 改为: [ros/rosdistro](https://github.com/ros/rosdistro/) 与您更改的分支。 等待几天, 以便它被审查 。

<span id="what-happens-next"></span>

## 接下来会发生什么?

您已经做了所有必要的工作来索引您的 ROS 软件包。 其中一位审查员将查看您的拉动请求并决定是否 [符合审查准则](https://github.com/ros/rosdistro/blob/master/REVIEW_GUIDELINES.md)。审查者可以按原样批准您的修改,也可以向您提供可操作的反馈。一旦拉动请求符合审查准则,将合并,您的软件包将出现在 [ROS 指数](https://index.ros.org/).

您已完成了释放您软件包的重要一步。 请前往下一个指南 : [首次发布](First-Time-Release.md).
