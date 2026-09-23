<span id="generating-an-urdf-file"></span>

# 生成 URDF 文件

**目标：** 学习如何导出 URDF 文件。

**教程级别：** 中级

**预计耗时：** 5 分钟

大多数机器人开发者以团队形式工作，团队中通常有机械工程师负责建立机器人的 CAD 模型。许多 CAD 和建模软件支持导出 URDF 模型，因此不必手工编写。这些导出工具往往由熟悉相应 CAD 软件的个人开发。下面列出了适用于多种 CAD 和三维建模软件的 URDF 导出工具。*ROS 核心维护者不维护这些软件包，也不对其性能或易用性作出保证。* 不过，我们认为整理一份可用工具清单仍然很有帮助。

**CAD 导出工具**

- [Blender URDF 导出工具](https://github.com/dfki-ric/phobos)
- [CREO Parametric URDF 导出工具](https://github.com/icub-tech-iit/creo2urdf)
- [FreeCAD ROS Workbench](https://github.com/galou/freecad.cross)
- [RobotCAD（FreeCAD OVERCROSS）](https://github.com/drfenixion/freecad.overcross)
- [FreeCAD 到 Gazebo 导出工具](https://github.com/Dave-Elec/freecad_to_gazebo)
- [Fusion 360 URDF 导出工具](https://github.com/dheena2k2/fusion2urdf-ros2)
- [fusion2URDF（支持 Fusion 360、ros2_control、闭环）](https://github.com/Adriaeik/fusion2URDF)
- [FusionSDF：Fusion 360 到 SDF 导出工具](https://github.com/andreasBihlmaier/FusionSDF)
- [OnShape URDF 导出工具](https://github.com/Rhoban/onshape-to-robot)
- [SolidWorks URDF 导出工具](https://github.com/ros/solidworks_urdf_exporter)
- [sw2robot（基于 SolidWorks 配合关系的导出工具和跨平台 URDF 编辑器）](https://github.com/jsk-ros-pkg/solidworks_urdf_exporter2)
- [ExportURDF 库（Fusion360、OnShape、Solidworks）](https://github.com/daviddorf2023/ExportURDF)

**其他 URDF 导出与转换工具**

- [Gazebo SDFormat 到 URDF 解析器](https://github.com/ros/sdformat_urdf)
- [Python 实现的 SDF 到 URDF 转换器](https://github.com/andreasBihlmaier/pysdf)
- [URDF 到 Webots 仿真器格式转换工具](https://github.com/cyberbotics/urdf2webots)
- [Blender Robotics Tools](https://github.com/robotology/blender-robotics-utils/) 仓库包含多种实用工具，其中包括[从 Blender 导出 URDF 文件的工具](https://github.com/robotology/blender-robotics-utils/tree/master?tab=readme-ov-file#urdftoblender)。
- [CoppeliaSim URDF 导出工具](https://manual.coppeliarobotics.com/en/importExport.htm#urdf)
- [Isaac Sim URDF 导出工具](https://docs.omniverse.nvidia.com/isaacsim/latest/advanced_tutorials/tutorial_advanced_export_urdf.html)

**查看 URDF 和 SDF 文件**

- [常见 URDF 启动文件示例](https://github.com/ros/urdf_launch)
- URDF 网页查看器：[GitHub 仓库](https://github.com/gkjohnson/urdf-loaders/)和[在线网站](https://gkjohnson.github.io/urdf-loaders/javascript/example/bundle/index.html)
- [在 RViz 中查看 SDF 模型](https://github.com/Yadunund/view_sdf_rviz)
- [JupyterLab URDF 查看器](https://github.com/IsabelParedes/jupyterlab-urdf)

如果你有喜欢的 URDF 工具，欢迎将其加入上述清单！
