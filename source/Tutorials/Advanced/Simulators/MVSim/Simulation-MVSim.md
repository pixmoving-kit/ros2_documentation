---
translation_status: machine_translated
source: Tutorials/Advanced/Simulators/MVSim/Simulation-MVSim.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="mvsim"></span>

# MVSim 游戏

此套教程将教你如何配置 [MVSim 游戏](https://mvsimulator.readthedocs.io/) 与ROS 2. 的模拟器.

MVSim是一个轻量级,开源型,多车辆模拟器,专注于移动机器人的2D+3D可视化,它使用Box2D进行2D刚性体物理,并提供现实的车辆动力学模型(difficial driver,Ackermann direct),传感器模拟(2D/3D LiDARs,相机,IMU,GPS),以及本土ROS 2集成. MVSim特别适合测试导航,SLAM,以及具有低计算间接和快速迭代时间的多机器人协调方案.

- [安装（Ubuntu）](Installation-Ubuntu.md)
- [MVSim 入门](Getting-Started-MVSim.md)
- [定义世界、机器人和传感器](Defining-Worlds-MVSim.md)
