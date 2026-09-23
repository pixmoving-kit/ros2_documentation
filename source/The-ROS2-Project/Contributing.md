---
translation_status: machine_translated
source: The-ROS2-Project/Contributing.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="contributing"></span> <span id="id1"></span>

# 参与贡献

在你开始为ROS 2计划捐款之前,要记住一些事情.

<span id="tenets"></span>

## 特纳

- 尊重从前的风光

  ROS已经存在十多年了,并被开发商和世界各地的开发商所使用。 在贡献的同时保持谦卑的态度和开放的心态。

- 尽早启用开放机器人

  - 开放机器人公司充当守门人,并倡导ROS社区,从设计阶段开始,重新利用他们的专长和技术判断。

  - 尽早开始与开放机器人科和社区讨论。 长时间的ROS 撰稿人可能会对大局有更清晰的视野。 如果您执行一个功能并发送一个拉动请求而不首先与社区讨论, 你会冒着被拒绝的风险, 或者你可能会被要求基本上重新考虑你的设计。

  - 在开始执行之前,通常最好先提出问题或使用讨论来将一个想法社会化。

- 尽可能采用社区最佳做法,而不是临时程序

  在开发和贡献时考虑一下最终用户的经验。 避免使用非标准工具或可能无法为所有人访问的库。

- 考虑整个社区

  想想更大的画面,有开发者在不同的限制下建造不同的机器人,ROS需要满足整个社区的要求.

有许多方法可以帮助ROS 2项目.

<span id="discussions-and-support"></span>

## 讨论和支持

协助ROS 2的一些最简便的方法包括参与社区讨论和支持。您可以找到更多关于如何参与方案的信息。 [联系](../Contact.md) 页面。

<span id="contributing-code"></span>

## 贡献代码

<span id="setting-up-your-development-environment"></span>

### 创造你的发展环境

要启动, 您需要从源头安装; 遵循 [源安装指令](../Installation.md#building-from-source) 为您的平台。

<span id="development-guides"></span>

### 发展指南

- [ROS 2 开发者指南](Contributing/Developer-Guide.md)
- [版本控制最佳实践](Contributing/Source-Control-Best-Practices.md)
- [代码风格与语言版本](Contributing/Code-Style-Language-Versions.md)
- [质量指南：确保代码质量](Contributing/Quality-Guide.md)
- [ROS 构建农场](Contributing/Build-Farms.md)
- [Windows 使用技巧](Contributing/Windows-Tips-and-Tricks.md)
- [贡献代码](Contributing/Contributing-to-code.md)
- [为 ROS 2 文档作贡献](Contributing/Contributing-To-ROS-2-Documentation.md)

<span id="what-to-work-on"></span>

### 怎样做?

我们确定了一些可由社区成员执行的任务: [在 ROS 2 仓库中搜索标为“ 需要的帮助” 的问题](https://github.com/search?q=user%3Aament+user%3Aros2+is%3Aopen+label%3A%22help+wanted%22&type=Issues)。如果在清单中看到您想研究的东西,请就该项目发表评论,让其他人知道您正在研究它。

我们还对一些我们认为应该让初次提交文件者更容易了解的问题贴上了标签。 [标注为“第一个好问题”](https://github.com/search?q=user%3Aament+user%3Aros2+is%3Aopen+label%3A%22good+first+issue%22&type=Issues)如果您有兴趣为ROS 2 计划做出贡献,我们鼓励您首先审视这些问题。 如果您想扩大网络范围,我们欢迎在任何未决问题(或你可能提议的其他问题)上作出贡献,特别是那些具有里程碑式的任务,这些任务表明他们的目标是下一次ROS 2 发布(里程碑将是下一次发布,如 " 晶体 " ) 。

如果您有一些代码可以帮助修复错误或改进文档, 请将此代码作为拉动请求提交相关的存储器。 对于更大的修改, 讨论提案是一个好主意 。 [二号轨道运行平台](https://discourse.openrobotics.org/c/ros/111) 在开始研究之前,您可以确定是否有其他人在研究类似问题。如果您的建议涉及修改API,则特别建议您在开始工作前讨论这一方法。

<span id="becoming-a-core-maintainer"></span>

### 成为核心维护者

ROS 2维护者确保项目普遍取得进展。

- 审查收到的代码投稿的风格、质量和总体情况是否符合存储处/ROS 2的目标。

- 确保CI继续保持绿色.

- 合并符合以上质量和CI标准的拉力请求.

- 解决用户提出的问题。

该数据库中的每个存储器 [编号2](https://github.com/ros2) 财务报告和财务报告 [ament](https://github.com/ament) 成为其中一个或多个寄存器的维护者是一个只邀请的过程,一般涉及下列步骤:

- 在过去一年中,对寄存处有大量代码贡献.

- 在过去一年中,对寄到存放处的拉动请求进行大量审查。

大约每3个月,ROS 2小组将审查所有储存库中的贡献,并向新的维护者发出邀请,一旦接受邀请,将要求新的维护者接受关于ROS 2储存库的机制和政策的短期培训,在培训完成后,新的维护者将有机会进入适当的储存库。
