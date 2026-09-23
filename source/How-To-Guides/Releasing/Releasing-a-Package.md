<span id="releasing-a-package"></span>
# 发布软件包

**发布软件包，可以让它进入公共 ROS 2 构建农场（buildfarm）。** 这样可以：

- 让用户通过软件包管理器（例如 Ubuntu 上的 `apt`）安装软件包，覆盖 [REP 2000](https://reps.openrobotics.org/rep-2000/) 中所列的、相应 ROS 发行版支持的所有 Linux 平台。
- 自动生成软件包的 API 文档。
- 将软件包纳入 [ROS Index](https://index.ros.org)。
- 按需为仓库中的拉取请求自动运行持续集成（CI）。

**请根据情况选择以下指南发布软件包：**

- [为软件包建立索引](Index-Your-Packages.md)：软件包首次发布时，从这里开始。
- [首次发布](First-Time-Release.md)：软件包首次发布，但已经建立索引。
- [后续发布](Subsequent-Releases.md)：为已经发布过的软件包发布新版本。

成功完成指南中的操作后，软件包将在下一次发行版同步时进入 ROS 生态系统！

相关参考：[发布团队与发布仓库](Release-Team-Repository.md)、[发布轨道](Release-Track.md)。
