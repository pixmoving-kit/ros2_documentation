---
translation_status: machine_translated
source: How-To-Guides/Releasing/Releasing-a-Package.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="releasing-a-package"></span>

# 发布软件包

**发布一个包 让你的包 在公共ROS 2建设农场。** 这将:

- 通过软件包管理器(如: `apt` 在Ubuntu上),用于所有支持Linux平台的ROS发行版,描述如下: [REP 2000 环境方案](https://reps.openrobotics.org/rep-2000/).

- 允许您的软件包自动生成 API 文档 。

- 让你的包裹的一部分 [ROS 指数](https://index.ros.org).

- (可选)允许您在您的存储器中自动进行 CI 运行以获取拖动请求 。

**遵循下面的指南之一来释放您的软件包:**

- [为软件包建立索引](Index-Your-Packages.md) - 如果这是软件包的首次发布

- [首次发布](First-Time-Release.md) - 如果这是软件包的第一个版本, 但是它已经索引了

- [后续发布](Subsequent-Releases.md) - 如果您正在发布新版本的已发布的软件包

在成功遵循指令后,您的软件包将在下一次Dentro同步时释放到ROS生态系统中!
