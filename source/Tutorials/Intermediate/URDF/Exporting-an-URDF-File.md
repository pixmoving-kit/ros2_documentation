---
translation_status: machine_translated
source: Tutorials/Intermediate/URDF/Exporting-an-URDF-File.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="generating-an-urdf-file"></span>

# 生成 URDF 文件

**目标：** 学习如何导出 URDF 文件

**教程级别：** 中级

**用时：** 5分钟

大多数机器人学家在团队中工作,这些团队中往往包括一个机械工程师,他开发了一个机器人的CAD模型。与其用手设计一个URDF模型,不如从许多不同的CAD和模型化程序中输出一个URDF模型。这些输出工具往往是由熟悉他们使用的特定CAD程序的个人开发的。下面你会找到一个可供使用的URDF出口商列表,用于各种CAD和3D模型化软件系统。 *ROS核心维护者不维护这些软件包。 因此,我们不声称其性能或使用方便。* 但是,我们认为,编制一份可用的乌拉圭发展基金出口商名单会有所帮助。

**CAD 出口商**

> - [Blender URDF 导出器](https://github.com/dfki-ric/phobos)
>
> - [CREO 参数URDF 导出程序](https://github.com/icub-tech-iit/creo2urdf)
>
> - [FreeCAD ROS 工作台](https://github.com/galou/freecad.cross)
>
> - [机器人(FreeCAD overcross)](https://github.com/drfenixion/freecad.overcross)
>
> - [解锁到 Gazebo 出口商](https://github.com/Dave-Elec/freecad_to_gazebo)
>
> - [Fusion 360 URDF 输出器](https://github.com/dheena2k2/fusion2urdf-ros2)
>
> - [聚变2URDF(Fusion 360, ros2_control, 闭环)](https://github.com/Adriaeik/fusion2URDF)
>
> - [FusionSDF:向SDF出口商Fusion 360](https://github.com/andreasBihlmaier/FusionSDF)
>
> - [在形状 URDF 导出器上](https://github.com/Rhoban/onshape-to-robot)
>
> - [SolidWorks URDF 输出器](https://github.com/ros/solidworks_urdf_exporter)
>
> - [sw2robot(SolidWorks mate-power + 跨平台的URDF编辑器)](https://github.com/jsk-ros-pkg/solidworks_urdf_exporter2)
>
> - [导出URDF 库 (Fusion360, OnShape, Solidworks)](https://github.com/daviddorf2023/ExportURDF)

**其他 URDF 导出和转换工具**

> - [Gazebo SDFormat 到 URDF 解析器](https://github.com/ros/sdformat_urdf)
>
> - [SDF 到 Python 的 URDF 转换器](https://github.com/andreasBihlmaier/pysdf)
>
> - [URDF 到 Webots 模拟器格式](https://github.com/cyberbotics/urdf2webots)
>
> - 那个... [Blender 机器人工具](https://github.com/robotology/blender-robotics-utils/) 仓库包括一些有用的工具,包括导出工具 [Blender提供的URDF文件.](https://github.com/robotology/blender-robotics-utils/tree/master?tab=readme-ov-file#urdftoblender)
>
> - [CoppeliaSim URDF 出口国](https://manual.coppeliarobotics.com/en/importExport.htm#urdf)
>
> - [Isaac Sim URDF 出口商](https://docs.omniverse.nvidia.com/isaacsim/latest/advanced_tutorials/tutorial_advanced_export_urdf.html)

**正在查看 URDF 和 SDF 文件**  
- [通用 URDF 启动文件示例](https://github.com/ros/urdf_launch)

- URDF 文件的网络查看器 : [GitHub 重现](https://github.com/gkjohnson/urdf-loaders/) & [现场网站](https://gkjohnson.github.io/urdf-loaders/javascript/example/bundle/index.html)

- [在 RViz 中查看 SDF 模型](https://github.com/Yadunund/view_sdf_rviz)

- [Jupyterlab URDF 查看器](https://github.com/IsabelParedes/jupyterlab-urdf)

如果您有您喜欢的URDF工具, 请考虑添加到上面的列表中 !
