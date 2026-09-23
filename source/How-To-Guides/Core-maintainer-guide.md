---
translation_status: machine_translated
source: How-To-Guides/Core-maintainer-guide.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="ros-2-core-maintainer-guide"></span>

# ROS 2 核心维护者指南

ROS 2 核心中的每个软件包都有一个或多个维护者,负责软件包的一般健康,本指南给出一些关于ROS 2 核心软件包维护者责任的信息.

<span id="continuous-integration"></span>

## 持续整合

所有输入到ROS 2核心寄存器的代码都必须通过连续集成运行. ROS 2目前有两个独立的CI系统,需要PR在合并之前先通过这两个系统.

<span id="pr-builds-https-build-ros2-org-view-rpr"></span>

### 公关大楼(<https://build.ros2.org/view/Rpr>)

ROS 2 PR( Pull Request) 每次打开拉动请求都会自动构建运行。 这些构建运行此寄存器的构建和测试, 并且只运行此寄存器 。 这意味着它不会构建任何依赖性, 也不会构建任何依赖于此寄存器包的寄存器 。 这些构建有利于快速反馈以查看更改是否通过 linters , 单位测试等。 存在两大问题 :

- 这些构件在多个寄存器之间没有作用( 因此无法添加或更改 API 等) 。

- 这些测试只运行在 Linux 上( 它们不会运行在 macOS 或 Windows 上 )

为了解决这两个问题,还有CI建筑.

<span id="ci-builds-https-ci-ros2-org"></span>

### CI 构建( E)<https://ci.ros2.org>)

CI 构建在打开拉动请求时不会自动运行。 寄存器的维护者之一必须手动请求 CI 构建完成 <https://ci.ros2.org/job/ci_launcher/> .

默认情况下, 以这种方式运行一个任务将会在所有平台( Linux, macOS, 和 Windows) 上构建和运行所有软件包的测试( \> 300 个当前) 。 由于全程需要很多小时才能将 CI 机器捆绑起来, 建议所有运行在此会限制构建和测试的软件包的数量 。 可以通过使用 colcon 参数来实现 。 `--packages-up-to`, `--packages-select`, `--packages-above-and-dependencies`, `--packages-above`,除其他外,见《大会正式记录,第五十七届会议,补编第5号》(A/C.4/56/6),第28段。 [折叠文档](https://colcon.readthedocs.io/en/released/user/how-to.html#build-only-a-single-package-or-selected-packages) 关于如何使用 CI 机制的进一步文件,请访问 <https://github.com/ros2/ci/blob/master/CI_BUILDERS.md>.

<span id="merging-pull-requests"></span>

## 合并拉动请求

如果下列所有情况都属实,则拉动请求可以合并:

- DCO机器人报告一个传来的结果

- 公关建设报告是偶然的结果

- CI在所有平台上建立互换结果的报告

- 至少有一个维护者审查并批准了公关

关于审查公关时会发生什么的更多信息,参见: [审查拉取请求（PR）](../The-ROS2-Project/Contributing/Contributing-to-code/Reviewing-a-PR.md).

公关合并后,会自动与下一个公关一起建造 [夜莺](https://ci.ros2.org/view/nightly)。强烈建议在合并拉动请求后检查夜窗,以确保没有出现倒退。

<span id="keeping-ci-green"></span>

## 保持 CI 绿色

进行测试的夜间工作通常比为个人拉动请求所做的工作要全面得多。为此原因,在夜幕中可能会出现在CI任务中看不到的回归。在以下地点检查其包中的回归是维护者的责任:

- <https://ci.ros2.org/view/nightly>

- <https://ci.ros2.org/view/packaging>

- <https://build.ros2.org/view/Rci>

- <https://build.ros2.org/view/Rdev>

对于发现的任何问题,应打开新问题和(或)向有关储存库提出请求。

<span id="making-releases"></span>

## 释放

为了让终端用户获得新的功能和bugfixs,维护者必须定期进行寄存器的发布(还可以要求其他维护者点播发布).

如本报告所述, [开发者指南](../The-ROS2-Project/Contributing/Developer-Guide.md#semver),ROS 2软件包跟随semver为版本编号.

用ROS术语来说,发布包含两个不同的步骤:制作源发布,然后制作二进制发布.

<span id="source-release"></span>

### 来源发布

一个源发布在相关的寄存器中创建了更改日志和一个标记.

进程从生成或更新changeGELOG.rst文件开始,其命令如下:

``` console
$ catkin_generate_changelog
```

如果寄存器中的一个或多个软件包没有包含 ChangeGELOG.rst,请添加 `--all` 选项来填充每个软件包上所有先前的承诺。 `catkin_generate_changelog` 命令会简单地将文件与寄存器的输入日志一起填充。由于这些输入日志并不总是适合更改日志,建议编辑 CHANGELOG.rst 并编辑,使其更便于阅读。编辑完成后,必须将更新的 CHANGELOG.rst 文件输入寄存器。

下一步是用以下命令在软件包.xml和更改log文件中碰上版本:

``` console
$ catkin_prepare_release
```

此命令将在寄存器中找到所有软件包, 检查更改日志是否存在, 检查没有未承诺的本地更改, 在 package. xml 文件内递增版本, 并使用一个与开花兼容的标签来承诺/ 标注更改 。 使用此命令是保证发行版本与开花一致和兼容的最佳方式 。 默认情况下, `catkin_prepare_release` 但是,它也可以撞到小数字或主要数字,甚至有精确的版本集。请参见帮助输出。 `catkin_prepare_release` 以获取更多信息。

假设以上成功,则已发布来源。

<span id="binary-release"></span>

### 二进制释放

下一个步骤是使用 `bloom-release` 命令创建二进制版本。关于如何使用开花的完整指令,请参见 <http://wiki.ros.org/bloom>。要对寄存器进行二进制发布,请运行:

``` console
$ bloom-release --track <rosdistro> --rosdistro <rosdistro> <repository_name>
```

例如,释放 `rclcpp` 运行到 Rolling 分发器的存储器,命令是:

``` console
$ bloom-release --track rolling --rosdistro rolling rclcpp
```

此命令将获取发布寄存器, 作出必要的修改以进行发布, 将修改推向发布寄存器, 最后打开一个拉请求到 <https://github.com/ros/rosdistro> .

<span id="backporting-to-released-distributions"></span>

## 返回已发行的发行区

所有即将到来的更改应首先登陆开发分支。 一旦某项更改被合并到开发分支, 就可以考虑将此项更改重新传送到已发行的发行中。 然而, 任何返回代码都不得破解 。 [API](https://en.wikipedia.org/wiki/API) 或 时 间 [ABI 缩写](https://en.wikipedia.org/wiki/Application_binary_interface) 如果可以在不中断 API 或 ABI 的情况下将更改导入回放,那么应当创建针对相应分支的新的拉动请求。新的拉动请求应当添加到适当的分布式工程板上。 <https://github.com/orgs/ros2/projects>。新的拉动请求应该像以前一样运行所有步骤,但要确保针对CI等的分发。

<span id="responding-to-issues"></span>

## 答复问题

软件包维护者还应研究存储库中即将出现的问题,并对用户所面临的问题进行分类。

对于看上去像问题的问题,问题应结束,用户应重新定向到 [机器人堆栈交换](https://robotics.stackexchange.com/) .

如果一个问题看起来是一个问题,但与这个特定的存储库无关,则应用GitHub“传输问题”按钮将其移到适当的存储库。

如果记者没有提供足够的信息来确定问题的原因,应当向记者索取更多信息.

如果这是一个新特点,请用“希望得到帮助”来标注这个问题。

任何遗留的问题都应该复制,如果它们真的是一个错误的话,就确定它们。如果是一个错误,那么修正会受到高度的赞赏。

<span id="getting-help"></span>

## 获得帮助

在维持一揽子计划的同时,可能会出现关于一般程序或个别问题的问题。

关于一般性问题,请遵照 [提供指南](../The-ROS2-Project/Contributing.md).

关于个别问题,请给ROS 2 GitHub团队(@ros/team)贴上标签,团队中有人会查看.
