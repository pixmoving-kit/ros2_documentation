---
translation_status: machine_translated
source: The-ROS2-Project/Contributing/Contributing-to-code/Making-a-PR.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="making-a-pull-request-pr-how-to"></span>

# 创建拉取请求（PR）

拖动请求用于为 ROS 项目提供代码和文件修改。 本文解释了如何从 ROS 存储器的叉子上编写和创建拖动请求。 有了这些信息, 您就可以提交拖动请求中的有重点的修改, 供审查 。

**领域:贡献,社区 QQ 内容类型:如何实现QQ 经验:初学者,中间人物,专家**

<span id="summary"></span>

## 小结

[拉动请求( PR)](https://docs.github.com/en/pull-requests) 是将您的更改合并到 ROS 仓库的建议 。 进行拉动请求允许您与其他 ROS 贡献者合作, 在 ROS 维护者合并之前提供一个空间来讨论和审查您的代码更改 。 [ROS存储器](https://github.com/ros2).

有关贡献礼仪的更多信息,请参见: [参与贡献](../../Contributing.md).

<span id="prerequisites"></span>

## 前提条件

1.  [创建叉](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo) 用于修改代码的目标 ROS 仓库。

2.  完成您的代码更改 。 **滚动** 树枝,在你的叉子 [目标ROS存储器](https://github.com/ros2).

3.  确保您的修改符合ROS指南 。

    - 如果您的拉动请求是更改代码 :

      - 确定您是否遵循了指南 。 [开发者指南](../Developer-Guide.md).

      - 请检查date=中的日期值 (帮助) [代码样式指南](../Code-Style-Language-Versions.md).

      - 确定你是否已经 [运行测试](../../../Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.md#colcon-run-the-tests) 和用于修改代码的合适的插件。

    - 如果您的拉动请求是更改文档 :

      - 确定您是否遵循了该指引 。 [为 ROS 2 文档作贡献](../Contributing-To-ROS-2-Documentation.md).

<span id="steps"></span>

## 步骤

<span id="preparing-the-pull-request"></span>

### 1 准备拉动请求

使用下列指南来准备您的拉动请求:

- **范围和重点**  
  - 将每个拉动请求限制在单个,定义清晰的更改中.

  - 作为单独的拉动请求提交不相关的修改 。

  - 保持小补丁并避免不必要的或偶然的更改.

- **提交历史和压实**  
  - 平面变换为最小数量的清晰语义承诺保留一个可读的项目历史.

  - 在审查拉动请求时, 不要打压, 因为审查人员可能不会注意到可能导致混乱的变化。

  - 正在审查拉动请求时, 您可以创建新任务 。

- **牵引请求草案**  
  - 在工作进行期间,使用拉动请求草稿来请求早期反馈 。

  - 在你标出已准备好之前, 不要期望拉动请求草案会被正式审查或合并。

  - 如果您希望某个特定的人对拉动请求草稿的早期反馈,请提及(使用 `@`)在拉动请求描述或注释中。

- **提及和参考**  
  - 如果您的修改是基于设计文件, 如 [REP](https://reps.openrobotics.org/),在拉力请求描述中提及其他参与设计的人,例如那些审查REP的人.

  - 如果您的拉动请求依赖于另一个拉动请求, 请在拉动请求描述中明确引用依赖性 。 请使用 `#` 标注.

  - 如果计划以特定的ROS版本发布您的更改,请在拉动请求描述中加入ROS的该版本.

- **记录您的代码更改**  
  - 如果您的拉动请求是为了代码更改,请尝试在同一拉动请求中做出任何相关的文档更新(包括API文档,特征文档,以及发布注释).

<span id="submitting-the-pull-request"></span>

### 2 提交拉动请求

1.  从分支创建拉动请求,其中包含您在叉子中的更改,到 **滚动** 目标 ROS 寄存器的分支。 您可以使用 GitHub CLI、 GitHub 桌面或 GitHub 网页界面创建您的拉动请求 。

    关于从叉子创建拉动请求的更多信息,请参见 [GitHub 文档](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork).

    关于每种可用的牵引请求方法的更多信息,请参见: [GitHub 文档](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request).

2.  通过完成描述模板中显示的章节来填充拉动请求,包括:

    - **说明**: 总结您的代码变化,通过ID链接到相关的 GitHub 问题和 PR,突出任何关键点或关注领域.

    - **问题**: 以格式包含您更改后固定的 GitHub 问题的标识 `Fixes #(issue)`。这保证了在拉力请求合并时自动结束这一问题。

    - **遗传性人工智能**:如果此拉动请求是使用Generative AI生成的,请指定模型和版本(例如GitHub Copilot v3.2).

    - **补充资料**:提供您认为有助于理解您更改的任何上下文或细节。

3.  选择 [允许由维护者编辑](https://github.blog/news-insights/product-news/improving-collaboration-with-forks/) 复选框,帮助ROS维护者在需要时直接做出小的修改.

在您提交拉动请求后, ROS 社区的其他开发者和贡献者会检讨您的修改, 包括对照相关指南检查 。

<span id="responding-to-review-comments"></span>

### 3 对审查意见的答复

当另一个开发者或贡献者在您的拉动请求中添加审查评论或建议时,您将收到 GitHub 的通知 。

您可以在 GitHub 中直接查看和讨论审查评论(参见 [用于援助的 GitHub 文件](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/viewing-a-pull-request-review)),并在您的分支中进一步添加承诺,以便在需要时解决。您也可以直接接受拉动请求中的任何建议更改,这将自动为您的分支添加新的承诺(参见 [如何接受建议更改的 GitHub 文件](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/incorporating-feedback-in-your-pull-request)).

使用这些反馈来讨论和调整您的修改, 并按需要修改和更新您的发展分支, 目的是在一周内回复评论, 这样您和审查人员不会失去您修改的背景 。

<span id="merging-the-pull-request"></span>

### 4 合并牵引请求

在您收到任何反馈后, 您的拉动请求必须得到一个 [提交目标ROS寄存器](../../Governance.md) 在可以合并之前。

当委员会批准您的拉动请求时, 他们会将其合并到目标分支( 通常是) 。 **滚动**)),您将会收到GitHub的通知书.

您的更改也可能被反馈到 ROS 的旧版本 。

<span id="related-content"></span>

## 相关内容

- [ROS 发展一般原则](../Developer-Guide.md#general-principles)

- [审查拉取请求（PR）](Reviewing-a-PR.md)
