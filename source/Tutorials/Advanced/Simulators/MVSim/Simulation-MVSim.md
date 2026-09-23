<span id="mvsim"></span>

# MVSim

本系列教程介绍如何配置 [MVSim](https://mvsimulator.readthedocs.io/) 仿真器，使其与 ROS 2 配合使用。

MVSim 是一个轻量级、开源的多车辆仿真器，专注于移动机器人的二维和三维可视化。它使用 Box2D 进行二维刚体物理仿真，提供逼真的车辆动力学模型（差速驱动、阿克曼转向）、传感器仿真（二维和三维激光雷达、摄像头、IMU、GPS），并原生集成 ROS 2。MVSim 计算开销低、迭代速度快，特别适合测试导航、SLAM 和多机器人协作场景。

- [在 Ubuntu 上安装](Installation-Ubuntu.md)
- [MVSim 入门](Getting-Started-MVSim.md)
- [定义仿真世界](Defining-Worlds-MVSim.md)
