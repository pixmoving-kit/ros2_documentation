<span id="contact"></span>
<span id="help"></span>

# 联系方式

<span id="support"></span>
<span id="using-robotics-stack-exchange"></span>

## 获取帮助

不同类型的问题或讨论适合不同的沟通渠道。请阅读下面的说明，选择合适的方式。

需要帮助排查系统问题时，请先搜索 [Robotics Stack Exchange](https://robotics.stackexchange.com/)，看看是否有人遇到过类似问题，以及他们的解决方法是否适用于你。

如果没有找到答案，请在 [Robotics Stack Exchange](https://robotics.stackexchange.com/) 上提出新问题。务必添加标签，至少包括 `ros2` 和所用发行版的版本，例如 `rolling`。如果问题与本文档有关，请添加 `docs` 标签，或更具体的 `tutorials` 标签。

请不要直接联系开发者或维护者。未公开提出或回答的问题，社区其他成员无法看到。当整个社区都能参与讨论、帮助解答问题时，开源开发才能发挥最佳效果。建议将问题发布到 [Robotics Stack Exchange](https://robotics.stackexchange.com/)，并在问题跟踪系统中报告缺陷。

<span id="contributing-support"></span>

### 帮助其他用户

ROS 2 用户的技术背景各不相同，使用的操作系统多种多样，也不一定有任何 ROS（1 或 2）使用经验。因此，无论经验多少，用户参与答疑都很重要。

如果你在 [Robotics Stack Exchange](https://robotics.stackexchange.com/) 上看到与自己经历相似的问题，可以分享当时对你有帮助的线索。不必因为不确定答案是否正确而担心。只需说明你的不确定之处，其他社区成员会在需要时补充。

<span id="issues"></span>

## 报告问题

如果发现缺陷、有改进建议，或有针对某个软件包的具体问题，可以在 GitHub 上创建 issue。

例如，学习[本站教程](Tutorials.md)时，如果发现某条操作说明在你的系统上不起作用，可以在 [ros2_documentation 仓库](https://github.com/ros2/ros2_documentation)中创建 issue。

可以在 [ROS 2 的 GitHub 组织](https://github.com/ros2)中搜索各个 ROS 2 仓库。

创建 issue 前，请先搜索 ros2 和 ament 这两个 GitHub 组织，确认其他用户是否报告过类似问题：[搜索示例](https://github.com/search?q=user%3Aros2+user%3Aament+turtlesim&type=Issues)。

然后，检查 [Robotics Stack Exchange](https://robotics.stackexchange.com/)，看看是否有人提出过相同的问题或报告过相同的故障。

如果尚未有人报告，可以在相应仓库的问题跟踪系统中创建 issue。如果不确定应该使用哪个仓库，请在 [ros2/ros2 仓库](https://github.com/ros2/ros2/issues)中提交，我们会查看。

创建 issue 时，请务必：

- 提供足够的信息，让其他人能够理解问题。准确描述你当时正在做什么、想实现什么，以及究竟哪里出了问题。如果按照教程或在线说明操作，请附上对应说明的链接。
- 使用具体明确的标题。不好的例子：“rviz 无法使用”；好的例子：“最近一次 apt 更新后，Rviz 因找不到 `.so` 文件而崩溃”。
- 提供与问题有关的具体平台、软件、版本和环境信息，包括软件的安装方式（二进制安装或源码构建），以及所用的 ROS 中间件或 DDS 供应商（如果知道）。
- 提供所有警告或错误信息。请直接从输出这些信息的终端窗口中复制粘贴，不要重新手打，也不要用截图替代。
- 如果是缺陷，请考虑提供[简短、自包含、正确且可编译的示例](https://sscce.org/)。
- 讨论编译、链接或安装问题时，同时提供编译器版本。

根据具体情况，还应提供：

- ROS 环境变量（`env | grep ROS`）
- 调用栈回溯
- 相关配置文件
- 显卡型号及驱动版本
- 如果可以，提供 rviz 的 Ogre.log（使用 `rviz -l` 运行）
- 能够复现问题的 bag 文件和示例代码
- 展示问题的 GIF 动图或视频

<span id="discussion"></span>
<span id="using-ros-discourse"></span>

## 讨论

如果想与其他 ROS 2 社区成员展开讨论，请访问官方 [Open Robotics Discourse](https://discourse.openrobotics.org/)。Discourse 适合较宏观的内容，不适合解答具体的代码问题，但适合讨论最佳实践或标准改进。

关于 ROS 2 开发和计划的讨论在 [Open Robotics Discourse 的 ROS 分类](https://discourse.openrobotics.org/c/ros/111)中进行。参与这些讨论，是对 ROS 2 各项功能的行为和实现方式发表意见的重要途径。

ROS 生态系统背后多元化的社区是它最宝贵的财富之一。我们鼓励所有 ROS 社区成员参与设计讨论，让大家的经验得到充分利用，并兼顾 ROS 的不同使用场景。

<span id="etiquette"></span>

## 交流礼仪

**善意理解他人。** 网络上的评论很容易被误解，无论是含义还是语气。先假定对方怀有善意，可以避免冒犯真心想帮助你的社区成员，也有助于维护交流氛围。即使对方最初并非善意，以善意回应通常仍然更有效。

**请勿重复发送同一个问题。** 大家已经看到了你的问题。如果没有收到回复，可能只是还没有人有时间回答，也可能没有人知道答案。无论哪种情况，重复发送都不合适，如同大声叫喊，容易让许多人反感。这也适用于在多个平台交叉发帖。请尽量选择最合适的论坛提问。如果有人建议你转到另一个论坛，请附上原讨论的链接。

在 [Robotics Stack Exchange](https://robotics.stackexchange.com/) 上，可以编辑问题以补充细节。信息越充分，其他人越容易帮你找到解决方法，你也越有可能得到回复。

**不宜强调个人的截止期限。** 回答问题的社区成员也有自己的截止期限。

**不要恳求帮助。** 如果有人愿意并且能够帮助你，通常会得到回复。催促别人更快回答，大多只会适得其反。

**不要在帖子中添加无关内容。** 帖子应围绕当前主题展开。无关的内容、链接和图片会被视为垃圾信息。

商业性质的帖子另请参阅[这项讨论](https://discourse.openrobotics.org/t/sponsorship-notation-in-posts-on-ros-org/2078)。

**尽量少引用付费内容。** 发布在 [Open Robotics Discourse](https://discourse.openrobotics.org/) 和 [Robotics Stack Exchange](https://robotics.stackexchange.com/) 上的内容，通常应对所有用户免费开放。私人期刊文章、教科书或付费新闻网站等内容的链接，虽然可能有用且与问题相关，却未必人人都能访问。尽可能以免费开放的来源作为主要资料，付费内容只作为补充。

**避免只发一个链接。** 一般来说，仅包含链接的回答帮助较小，也容易被误认为垃圾信息。此外，链接可能随着时间失效，或对应内容被替换。概述链接中的内容，并提供上下文和来源，通常会更有帮助。

<span id="private-contact"></span>

## 私下联系

如果需要私下联系我们，例如问题涉及组织或项目的敏感信息，或与安全问题有关，可以直接发送电子邮件至 `ros@osrfoundation.org`。
