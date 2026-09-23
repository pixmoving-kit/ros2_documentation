<span id="platform-eol-policy"></span>
<span id="platformeolpolicy"></span>

# 平台支持终止政策

即使 [ROS 发行版](../Releases.md)仍在维护，它也不会继续支持已终止支持（End of Life，EOL）的平台。本文说明使用 EOL 平台的用户应有的预期，以及 ROS 发行版负责人（ROS Boss）需要采取的措施。

<span id="policy"></span>

## 政策

每个 ROS 发行版支持特定的**平台**，例如 Windows 11 或 Ubuntu 24.04。Microsoft、Canonical 等平台**供应商**决定各自平台的支持期限。平台进入 EOL 后，供应商通常停止发布关键缺陷修复和安全更新。为避免潜在的未修补安全漏洞，我们会主动从 ROS 构建农场中移除该平台的全部构建任务。

如果使用的平台已不再受供应商支持，你应预期将停止收到 ROS 软件包更新。现有软件包仍然可用且能够运行，但不再更新。在特殊情况下，ROS 发行版负责人也可以选择为 EOL 平台更新软件包。

<span id="for-ros-bosses"></span>

## ROS 发行版负责人的工作

目标平台进入 EOL 之前：

- 如果某个平台会早于 ROS 发行版本身进入 EOL，确保发行版文档列出该平台的 EOL 日期。
- 至少提前两次软件包同步（约 60～90 天）发布平台即将进入 EOL 的公告，让软件包维护者有时间更新。
- 创建[禁用该平台构建农场任务的拉取请求](https://github.com/ros2/ros_buildfarm_config)，并请[基础设施项目管理委员会（Infrastructure PMC）](https://osralliance.org/wp-content/uploads/2024/03/infrastructure_project_charter.pdf)审查。
- 为该平台执行最后一次同步。

目标平台进入 EOL 之后：

- 更新 ROS 发行版文档，说明该平台将不再收到 ROS 软件包更新。
- 在 Discourse 上公告该 ROS 发行版已停止支持此平台。
- 如果 EOL 前尚未完成最后一次发布、此次更新不太可能导致回归问题，并且 ROS 构建农场仍有该平台的运行器，可考虑为该平台再发布一次。
- 合并禁用构建农场任务的拉取请求。
