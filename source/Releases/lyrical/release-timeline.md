<span id="lyrical-luth-release-timeline"></span>

# Lyrical Luth 发布时间表

Lyrical Luth 的开发进度可在[项目看板](https://github.com/orgs/ros2/projects/70)查看。整体发布流程参见[流程说明](../Release-Process.md)。

**尽快：将 ROS Rolling 迁移至 ROS Lyrical 的目标平台**

- RHEL 10 和 Ubuntu 26.04：核心软件包在两个平台上成功构建后立即迁移。
- Windows 11：构建通过后立即迁移。

**2026 年 4 月 13 日，星期一：Alpha 和 RMW 冻结**（已延期，原定 4 月 6 日）

- 对 ROS Base 软件包进行初步测试。
- 冻结 RMW 提供者软件包的 API 和功能。

**2026 年 4 月 20 日，星期一：冻结**（已延期，原定 4 月 13 日）

- 冻结 Rolling Ridley 中 ROS Base 软件包的 API 和功能。
- 此后仅发布缺陷修复。
- 仍可发布新软件包。

**2026 年 4 月 21 日，星期一：创建分支**（已延期，原定 4 月 20 日）

- 从 Rolling Ridley 创建分支。
- `rosdistro` 重新接受 ROS Base 软件包面向 Rolling 的 PR。
- Lyrical 开发从 `ros-rolling-*` 软件包转向 `ros-lyrical-*` 软件包。

**2026 年 4 月 27 日，星期一：Beta**

- 提供更新后的 ROS Desktop 软件包。
- 邀请广泛测试。

**2026 年 4 月 30 日，星期四：启动教程测试活动**

- 开放教程供社区测试。

**2026 年 5 月 11 日，星期一：候选发布版**

- 构建包含 ROS Desktop 在内的候选发布版软件包。

**2026 年 5 月 18 日，星期一：发行版冻结**

- 冻结所有 ROS Desktop 软件包的全部 Lyrical 分支。
- 不再合并面向任何 Lyrical 分支，或面向 `rosdistro` 仓库中 `lyrical/distribution.yaml` 的拉取请求。

**2026 年 5 月 22 日，星期五：正式发布**

- 发布公告。
- 解除 ROS Desktop 软件包的源码冻结，`rosdistro` 重新接受面向 Lyrical 的拉取请求。

**2031 年 5 月：停止支持**

- ROS Lyrical 停止接收更新，包括安全更新。
