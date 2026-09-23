<span id="contributing"></span>
<span id="id1"></span>

# 参与贡献

开始为 ROS 2 项目作贡献之前，请牢记以下几点。

<span id="tenets"></span>

## 基本原则

### 尊重已有工作

ROS 已发展十余年，世界各地的开发者都在使用它。参与贡献时，请保持谦逊和开放的心态。

### 尽早与 Open Robotics 沟通

Open Robotics 为 ROS 社区把关，并代表社区的利益。设计阶段就应借助他们的经验和技术判断。

尽早与 Open Robotics 和社区讨论。长期参与 ROS 的贡献者可能对整体方向有更清晰的认识。如果在未与社区讨论的情况下实现功能并提交拉取请求，就可能面临被拒绝或被要求大幅重新设计的风险。

通常，最好先创建 issue 或在 Discourse 上交流想法，再开始实现。

### 尽可能采用社区最佳实践

尽量遵循社区已有的最佳实践，避免临时自定流程。开发和贡献时，请考虑最终用户的体验，避免使用并非所有人都能获得的非标准工具或库。

### 从整个社区的角度考虑

着眼于全局。不同开发者构建不同机器人，面临的约束也各不相同。ROS 需要兼顾整个社区的需求。

为 ROS 2 项目作贡献有多种方式。

<span id="discussions-and-support"></span>

## 讨论与支持

参与社区讨论、帮助其他用户，是为 ROS 2 作贡献最容易的方式之一。如何参与，参见[联系页面](../Contact.md)。

<span id="contributing-code"></span>

## 贡献代码

<span id="setting-up-your-development-environment"></span>

### 设置开发环境

首先需要从源码安装 ROS 2，请按照适用于你所用平台的[源码安装说明](../Installation.md#building-from-source)操作。

<span id="development-guides"></span>

### 开发指南

- [开发者指南](Contributing/Developer-Guide.md)
- [版本控制最佳实践](Contributing/Source-Control-Best-Practices.md)
- [代码风格与语言版本](Contributing/Code-Style-Language-Versions.md)
- [质量指南](Contributing/Quality-Guide.md)
- [构建农场](Contributing/Build-Farms.md)
- [Windows 使用技巧](Contributing/Windows-Tips-and-Tricks.md)
- [贡献代码](Contributing/Contributing-to-code.md)
- [为 ROS 2 文档作贡献](Contributing/Contributing-To-ROS-2-Documentation.md)

<span id="what-to-work-on"></span>

### 选择参与的任务

社区已整理了一些适合参与的任务，可以[跨 ROS 2 仓库搜索带有“help wanted”标签的 issue](https://github.com/search?q=user%3Aament+user%3Aros2+is%3Aopen+label%3A%22help+wanted%22&type=Issues)。如果发现想参与的任务，请在对应 issue 下留言，让其他人知道你正在研究它。

更适合首次贡献者的任务使用 [“good first issue”标签](https://github.com/search?q=user%3Aament+user%3Aros2+is%3Aopen+label%3A%22good+first+issue%22&type=Issues)。如果有兴趣参与 ROS 2，建议先看看这些 issue。也欢迎处理其他开放的 issue，或提出自己的任务，尤其是带有下一个 ROS 2 发行版里程碑的任务；里程碑名称通常是下一个发行版的名称，例如 `crystal`。

如果你的代码修复了缺陷或改进了文档，请向对应仓库提交拉取请求。对于较大的修改，最好在开始前先到 [ROS 2 论坛](https://discourse.openrobotics.org/c/ros/111)讨论，以确认是否已有其他人在做类似工作。涉及 API 变更时，更应提前讨论方案。

<span id="becoming-a-core-maintainer"></span>

### 成为核心维护者

ROS 2 维护者负责确保项目持续推进，职责包括：

- 审查提交的代码，检查风格、质量，以及是否符合仓库和 ROS 2 的整体目标。
- 确保持续集成（CI）保持通过状态。
- 合并符合质量和 CI 标准的拉取请求。
- 处理用户提交的 issue。

[ros2](https://github.com/ros2) 和 [ament](https://github.com/ament) 组织下的每个仓库都有各自的维护者。成为一个或多个仓库的维护者需要收到邀请，通常需要满足以下条件：

- 在过去一年内，为该仓库作出相当数量的代码贡献。
- 在过去一年内，审查相当数量的该仓库拉取请求。

ROS 2 团队大约每三个月审视一次各仓库的贡献情况，并向新的维护者发出邀请。接受邀请后，新维护者需要参加简短培训，了解 ROS 2 仓库的工作机制和政策。完成培训后，将获得相应仓库的写入权限。
