---
translation_status: machine_translated
source: Tutorials/Intermediate/Testing/BuildFarmTesting.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="testing-your-code-with-the-ros-build-farm"></span>

# 使用 ROS 构建农场测试代码

那个... [ROS 2 建设农场](https://build.ros2.org/) 除了创建二进制外, 它也会通过在 PR 合并之前为您的ROS 软件包编译和运行所有测试来测试拉动请求 。

有四个先决条件。

> - GitHub 用户 [@ros-pull-request-buildinger (英语).](https://github.com/ros-pull-request-builder) 必须能够进入仓库。
>
> - GitHub 寄存器必须设置 Webhooks 。
>
> - [您的软件包必须用 rosdistro 索引](../../../How-To-Guides/Releasing/Index-Your-Packages.md)
>
> - 那个... `test_pull_requests` 旗帜必须是真实的。

<span id="github-access"></span>

## GitHub 访问

您可以在 GitHub 组织级别上或只给单一的 GitHub 存储器访问 PR 构建器 。

<span id="github-organization"></span>

### GitHub 组织

1.  打开 [https://github.com/orgs/%YOUR_ORG%/people](https://github.com/orgs/%YOUR_ORG%/people) (替换时) `%YOUR_ORG%` (与有关组织联系)

2.  单击 `Invite Member` 输入 `ros-pull-request-builder`

<span id="github-repository"></span>

### GitHub 仓库

1.  打开 [https://github.com/%YOUR_ORG%/%YOUR_REPO%/settings/access](https://github.com/%YOUR_ORG%/%YOUR_REPO%/settings/access) (替换时) `%YOUR_ORG%/%YOUR_REPO$` 与适当的组织/repo)

2.  单击 `Add people` 输入 `ros-pull-request-builder`

3.  选择 `Admin` 或 时 间 `Write` (见下节)

<span id="webhooks"></span>

## WebHooks 网络用户

若您对下列事项给予充分的行政权利: `ros-pull-request-builder`它会自动设置钩子。

或者,你可以避免完全行政权的需要,只设置行政权。 **写入** 权限。

1.  打开 [https://github.com/%YOUR_ORG%/%YOUR_REPO%/settings/hooks/new](https://github.com/%YOUR_ORG%/%YOUR_REPO%/settings/hooks/new))

2.  输入 `"https://build.ros2.org/ghprbhook/` 作为有效载荷 URL

3.  检查以下选项 :  
    - 让我选择个别事件。

    - 问题评论

    - 调用请求

<span id="test-pull-requests"></span>

## test_pull_requests

对于您想要进行牵引请求测试的每个ROS distro, 您必须启用 `test_pull_requests` 标记在表格中的适当部分中 [rostro 维基月球](https://github.com/ros/rosdistro/).

> - **备选案文1** - 你跑步的时候有选择权 [开花](../../../How-To-Guides/Releasing/Releasing-a-Package.md) 以启动拉动请求测试。
>
> - **备选案文2** - 你当然可以 **小心点** 手动编辑 rosdistro repo 中相应的文件,并提交新的拉动请求. [示例](https://github.com/ros/rosdistro/blob/3c295f76b0755989e9ed526c0b5f28a5f6a94da3/rolling/distribution.yaml#L4708). [载于REP 143号文件](http://docs.ros.org/en/independent/api/rep/html/rep-0143.html#distribution-file).

请注意,在拉力请求被添加后,通常直到晚间Jenkins重组后才创建该工作.
