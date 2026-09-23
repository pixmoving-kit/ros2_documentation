提交拉取请求后，rosdistro 的维护者通常会在一到两天内审查并合并。
如果软件包构建成功，24 至 48 小时后，软件包将进入 **ros-testing** 仓库，你可以在那里[测试预发布二进制包](../../Installation/Testing.md)。

发行版的发布负责人通常每两到四周手动将 ros-testing 中的内容同步到 ROS 主仓库。
此时，ROS 社区的其他用户才能正式获取你的软件包。
要了解下一次同步的时间，请订阅 Open Robotics Discourse 的[打包与发布管理分类](https://discourse.openrobotics.org/c/ros/release/16)。
