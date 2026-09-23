---
translation_status: machine_translated
source: The-ROS2-Project/Contributing/Contributing-to-code/Reviewing-a-PR.rst
---

<span id="reviewing-a-pull-request-pr-how-to"></span>

# 审查拉取请求（PR）

所有输入到ROS项目的代码和文件都必须在拉动请求中审查。此篇文章解释了如何准备和审查一个贡献者提交的拉动请求。阅读此篇文章后,您将能够确保拉动请求的更改符合要求的标准。

**领域:贡献,社区 QQ 内容类型:如何实现QQ 经验:初学者,中间人物,专家**

<span id="summary"></span>

## 小结

审查一个贡献者的拉动请求(PR),您可以检查其修改是否符合适当的准则和标准。欢迎任何人审查和批准拉动请求。修改在批准后可以合并。只有a [提交者](../../Governance.md) 对于目标寄存器,可以将一个拉动请求合并到该寄存器中,在得到批准之前,他们不会这样做.

<span id="prerequisites"></span>

## 前提条件

代码或文档贡献者有 [提出拉动请求](Making-a-PR.md) 将其修改合并为其中之一 [ROS存储器](https://github.com/ros2).

<span id="steps"></span>

## 步骤

<span id="preparing-for-review"></span>

### 1 准备审查

- 欢迎任何人审查拉动请求。

  牵引请求一般需要两次审查才能合并.

- 将审查拉动请求视为涉及提交者和其他开发者的合作活动,而不是被动或单向进程。

- 作为评论员:

  - 您可以在位时对代码或文档进行小的改进,例如修补字典或解决小的样式问题.

  - 你应尽最大努力,在提交后一周内对拉动请求作出评论。

- 当你开始审查拉动请求时,留下一个注释让其他人知道你正在进行审查.

<span id="reviewing-the-pull-request"></span>

### 2 审查拉动请求

1.  对照下列准则审查拉动请求:

    - 确认代码或文档更改对寄存器是适当的.

    - 校验代码正确而完整,范围为单一的,定义明确的修改.

    - 检查拉动请求是否针对默认分支( 通常是) `rolling`).

    - 如果修改是基于设计文件,例如: [REP](https://reps.openrobotics.org/),验证这些修改是否与设计一致。

    - 对于代码更改,确保更改:

      - 跟着 [开发者指南](../Developer-Guide.md).

      - 跟着 [代码样式指南](../Code-Style-Language-Versions.md).

      - 包含新特性或错误修复的测试 。

    - 关于文件的更改,确保改动遵循 [文献指导](../Contributing-To-ROS-2-Documentation.md).

    - 确认持续集成( CI) 运行为拉请求 。

2.  请提供评论。

    您可以在提交方的拉力请求中添加评论,或者直接建议修改拉力请求([参见 GitHub 文档的引导](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/commenting-on-a-pull-request)).

3.  遵循这些指南,确保审评意见有用和可操作性:

    - 从高端评论开始(例如,要求重构或设计修改),然后转到关于具体内容的低端评论.

    - 考虑提供下列各类评论:

      - **积极反馈** 例如:

        `Nice work on handling edge cases here — the early return makes the logic much easier to follow.`

      - **问 题** 例如:

        `Just to make sure I'm not missing a requirement, is there a reason we're using a custom sorting function here instead of localeCompare?`

      - **建议** 例如:

        `You could simplify this loop using Array.map to make it more concise:`

        ``` javascript
        const names = users.map(user => user.name)
        ```

      - **问题** 例如:

        `This function doesn't handle the case where response is null, which could cause a runtime error — add a guard clause:`

        ``` javascript
        if (!response) {
          return [...];
        }
        ```

      - **内部管理** 这个变化与拉动请求的主要目的无关,

        `Since this file is already being updated, could we also remove the unused formatDate import at the top?`

      - **小细节** ——细小,挑细细,如改进风格或可读性,例如:

        `Minor naming suggestion; user_list could be named users to better reflect that it's a collection.`

    - 清楚说明您对每个评论的预期会发生什么,包括注释块是否合并了拉动请求,以及您是否认为您的请求是可选的还是需要的.

    - 记住要包括积极的反馈和对提交者所做工作的感谢,并始终具有建设性。

<span id="approving-and-merging-the-pull-request"></span>

### 3 核准和合并拉动请求

在您审查了拉动请求并提供了反馈后,提交者可以继续讨论或对其变化进行提法,在公关中添加新的承诺.

当您对修改感到满意并准备合并时,请批准拉动请求([参见 GitHub 文档的引导](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/approving-a-pull-request-with-required-reviews)).

- 欢迎任何人审查拉动请求,即使它已经有一个审查。

- 牵引请求必须至少有一个批准,在多数情况下必须有两个批准,由开发者(作者除外)批准,然后才能与目标分支合并.

- 只有目标寄存器的提交者才能合并一个经批准的拉动请求.

  - 见 [当前 ROS 提交者](../../Governance.md) 为目标寄存器拥有合并权限的人员列表。

- 如果牵引请求有任何依赖性,确保所依赖的牵引请求按照正确的顺序合并.

<span id="related-content"></span>

## 相关内容

- [ROS 发展一般原则](../Developer-Guide.md#general-principles)

- [创建拉取请求（PR）](Making-a-PR.md)

- [关于拉动请求的审查](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/about-pull-request-reviews)
