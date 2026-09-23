---
translation_status: machine_translated
source: Related-Projects/Intel-ROS2-Projects.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="intel-ros-2-projects"></span>

# Intel ROS 2 项目

Intel ⁇ 机器人开源项目(Intel ⁇ ROS Project),以通过包括CPU,GPU在内的各种Intel技术和平台,实现物体检测/定位/跟踪,人员检测,车辆检测,工业机器人臂抓点分析, [英特尔 莫维迪乌斯TM NCS](https://www.intel.com/content/www/us/en/developer/tools/neural-compute-stick/overview.html) 优化了深层学习后端,FPGA, [Intel\_ realSenseTM 互联网档案馆的存檔,存档日期2013-03-02.](https://www.intel.com/content/www/us/en/architecture-and-technology/realsense-overview.html) 摄影机等。

<span id="key-projects"></span>

## 关键项目

我们正在研究ROS 2以下的项目,并公布源代码。 <https://github.com/intel/> 或ROS 2 GitHub Repo 渐渐.

- [ROS2 打开VINO](https://github.com/intel/ros2_openvino_toolkit):ROS 2包,用于Intel ⁇ 视觉推论和神经网络优化工具包,以开发多平台计算机视觉解决方案.

- [ROS2 真实感相机](https://github.com/IntelRealSense/realsense-ros): ROS 2 用于Intel\_ realSenseTM D400系列相机的软件包

- [ROS2 莫维迪乌斯 NCS](https://github.com/intel/ros2_intel_movidius_ncs):ROS 2包,用于用Intel ⁇  MovidiusTM神经计算棒(NCS)进行物体检测.

- [ROS2 对象信件](https://github.com/intel/ros2_object_msgs): ROS 2 给对象的信息 。

- [ROS2 对象分析](https://github.com/intel/ros2_object_analytics):ROS 2包用于物体检测,跟踪和2D/3D本地化.

- [ROS2 信件过滤器](https://github.com/ros2/message_filters):ROS 2包,用于信息与时间戳同步.

- [ROS2 CV 桥](https://github.com/ros-perception/vision_opencv/tree/ros2/cv_bridge): ROS 2 套件,以开放CV进行桥接.

- [ROS2 对象映射](https://github.com/intel/ros2_object_map):ROS 2包,用于在SLAM基于ROS 2对象分析仪提供的信息时标记地图上对象的标记.

- [ROS2 移动对象](https://github.com/intel/ros2_moving_object):ROS 2包基于ROS 2对象分析学提供的信息提供对象运动信息(类似于x,y,z轴上的物体速度).

- [ROS2 格拉斯普库](https://github.com/intel/ros2_grasp_library): ROS 2 套件,用于把握位置分析,并与 [移动它](https://github.com/ros-planning/moveit2.git) 抓住接口。

- [ROS2 导航](https://github.com/ros-planning/navigation2):ROS 2 用于机器人导航的软件包,它已经集成到ROS 2 Crystal发行.

- [英特尔机器人 DevKit (SDK)](https://github.com/intel/robot_devkit):一个开源项目,使开发者能够轻松高效地创建,定制,优化,并部署一个机器人软件堆栈到基于机器人操作系统2(ROS 2)框架的自主移动机器人(AMR)平台上.

<span id="reference"></span>

## 参考文献

ROS 组件 : <https://wiki.ros.org/IntelROSProject> 显示这些包之间的关系,这也适用于ROS 2。
