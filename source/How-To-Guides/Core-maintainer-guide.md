<span id="ros-2-core-maintainer-guide"></span>
# ROS 2 核心软件包维护者指南

ROS 2 核心中的每个软件包都有一名或多名维护者，负责保持软件包的整体健康状态。本指南介绍核心软件包维护者的职责。

<span id="continuous-integration"></span>
## 持续集成

所有提交到 ROS 2 核心仓库的代码都必须经过持续集成检查。ROS 2 目前有两个独立的 CI 系统，PR 必须在两者中均通过检查才能合并。

<span id="pr-builds-https-build-ros2-org-view-rpr"></span>
### PR 构建（https://build.ros2.org/view/Rpr）

每次创建拉取请求时，ROS 2 PR 构建会自动运行。这类构建只构建并测试当前仓库，不构建它的依赖，也不构建依赖当前仓库软件包的其他仓库。因此，它能快速反馈改动是否通过代码风格检查、单元测试等，但存在两个主要限制：

- 不支持跨仓库改动，因此不能充分验证新增或修改 API 等情况。
- 只在 Linux 上运行，不在 macOS 或 Windows 上运行。

CI 构建用于弥补这两个限制。

<span id="ci-builds-https-ci-ros2-org"></span>
### CI 构建（https://ci.ros2.org）

创建 PR 时，CI 构建不会自动运行。仓库维护者必须前往 <https://ci.ros2.org/job/ci_launcher/> 手动发起。

默认情况下，这种任务会在所有平台（Linux、macOS 和 Windows）上构建并测试全部软件包（目前超过 300 个）。完整运行可能耗时数小时并占用 CI 机器，因此建议限制每次构建和测试的软件包数量。

可使用 colcon 的 `--packages-up-to`、`--packages-select`、`--packages-above-and-dependencies`、`--packages-above` 等参数。更多示例见 [colcon 文档](https://colcon.readthedocs.io/en/released/user/how-to.html#build-only-a-single-package-or-selected-packages)。CI 系统的详细使用说明见 <https://github.com/ros2/ci/blob/master/CI_BUILDERS.md>。

<span id="merging-pull-requests"></span>
## 合并拉取请求

只有同时满足以下条件，才能合并 PR：

- DCO 机器人检查通过。
- PR 构建通过。
- 所有平台的 CI 构建通过。
- 至少一名维护者完成审查并批准。

关于 PR 审查的更多信息，请参阅[审查 PR](../The-ROS2-Project/Contributing/Contributing-to-code/Reviewing-a-PR.md)。

合并后，改动会自动进入下一次[夜间构建](https://ci.ros2.org/view/nightly)。强烈建议在合并后检查夜间构建，确认没有引入回归问题。

<span id="keeping-ci-green"></span>
## 保持 CI 通过

夜间测试任务通常比单个 PR 的测试全面得多，因此可能发现 PR 的 CI 未暴露的回归问题。维护者有责任在以下位置检查其软件包是否出现回归：

- <https://ci.ros2.org/view/nightly>
- <https://ci.ros2.org/view/packaging>
- <https://build.ros2.org/view/Rci>
- <https://build.ros2.org/view/Rdev>

发现问题后，应在相关仓库创建 issue 和／或拉取请求。

<span id="making-releases"></span>
## 发布版本

为将新功能和问题修复交付给最终用户，维护者必须定期发布仓库的新版本，其他维护者也可能按需提出发布请求。

如[开发者指南](../The-ROS2-Project/Contributing/Developer-Guide.md#semver)所述，ROS 2 软件包的版本号遵循语义化版本规范。

在 ROS 中，一次发布包含两个独立步骤：先发布源码版本，再发布二进制版本。

<span id="source-release"></span>
### 发布源码版本

源码发布会在相关仓库创建变更日志和标签。

首先，生成或更新 CHANGELOG.rst：

```console
$ catkin_generate_changelog
```

如果仓库中有软件包还没有 CHANGELOG.rst，添加 `--all` 选项，可将各软件包以往的提交全部写入日志。`catkin_generate_changelog` 只是将仓库的提交日志填入文件；提交说明不一定适合作为变更日志，因此建议编辑 CHANGELOG.rst，提高可读性。编辑后，务必将更新的文件提交到仓库。

接着，提高 package.xml 和变更日志中的版本号：

```console
$ catkin_prepare_release
```

该命令会查找仓库中的全部软件包，检查变更日志是否存在、是否有未提交的本地改动，更新 package.xml 版本号，随后提交改动并创建与 bloom 兼容的标签。使用该命令是确保发布版本一致且兼容 bloom 的最佳方式。

默认情况下，`catkin_prepare_release` 增加补丁版本号，例如从 0.1.1 变为 0.1.2。它也可以增加次版本号、主版本号，或指定精确版本。详情见命令帮助。

如果上述步骤成功，源码版本就已发布。

<span id="binary-release"></span>
### 发布二进制版本

下一步使用 `bloom-release` 创建二进制发布。完整说明见 <http://wiki.ros.org/bloom>。发布仓库二进制版本的命令为：

```console
$ bloom-release --track <rosdistro> --rosdistro <rosdistro> <repository_name>
```

例如，将 `rclcpp` 仓库发布到 Rolling：

```console
$ bloom-release --track rolling --rosdistro rolling rclcpp
```

该命令会获取发布仓库，进行发布所需的修改，将改动推送到发布仓库，最后向 <https://github.com/ros/rosdistro> 创建拉取请求。

<span id="backporting-to-released-distributions"></span>
## 向已发布的发行版回移改动

所有新改动应先进入开发分支。合并到开发分支后，再考虑回移到已发布的发行版。回移代码不得破坏已发布发行版的 [API](https://en.wikipedia.org/wiki/API) 或 [ABI](https://en.wikipedia.org/wiki/Application_binary_interface)。

如果改动可以在不破坏 API 和 ABI 的情况下回移，就创建一个面向对应分支的新 PR，并将其加入 <https://github.com/orgs/ros2/projects> 上对应发行版的项目看板。新 PR 需要执行与之前相同的全部检查步骤，同时确保 CI 等检查针对正确的发行版。

<span id="responding-to-issues"></span>
## 处理 issue

软件包维护者还应查看仓库中新建的 issue，并对用户遇到的问题进行分类处理。

如果 issue 实际上是提问，应关闭它，并引导用户前往 [Robotics Stack Exchange](https://robotics.stackexchange.com/)。

如果报告的确是问题，但与当前仓库无关，应使用 GitHub 的 “Transfer issue” 将其转移到正确仓库。

如果报告者提供的信息不足以判断原因，应请求补充信息。

如果是新功能请求，为 issue 添加 “help-wanted” 标签。

其余问题应尝试复现，确认是否确实是缺陷；如果是，欢迎提交修复。

<span id="getting-help"></span>
## 获取帮助

维护软件包时，可能会遇到有关通用流程或具体问题的疑问。

一般性问题请遵循[贡献指南](../The-ROS2-Project/Contributing.md)。

针对具体 issue 的问题，请提及 ROS 2 GitHub 团队（@ros/team），团队成员会查看。
