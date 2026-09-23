<span id="intel-ros-2-projects"></span>

# Intel ROS 2 项目

Intel® 机器人开源项目（Intel® ROS Project）利用多种 Intel 技术和平台，实现物体检测、定位与跟踪，人员检测、车辆检测，以及工业机械臂抓取点分析等功能。使用的技术和平台包括 CPU、GPU、针对 [Intel® Movidius™ NCS](https://www.intel.com/content/www/us/en/developer/tools/neural-compute-stick/overview.html) 优化的深度学习后端、FPGA、[Intel® RealSense™](https://www.intel.com/content/www/us/en/architecture-and-technology/realsense-overview.html) 相机等。

<span id="key-projects"></span>

## 主要项目

团队正在开发以下 ROS 2 项目，并逐步通过 <https://github.com/intel/> 或 ROS 2 GitHub 仓库发布源代码。

- [ROS2 OpenVINO](https://github.com/intel/ros2_openvino_toolkit)：面向 Intel® 视觉推理和神经网络优化工具包的 ROS 2 软件包，用于开发跨平台计算机视觉方案。
- [ROS2 RealSense Camera](https://github.com/IntelRealSense/realsense-ros)：面向 Intel® RealSense™ D400 系列相机的 ROS 2 软件包。
- [ROS2 Movidius NCS](https://github.com/intel/ros2_intel_movidius_ncs)：使用 Intel® Movidius™ 神经计算棒（NCS）进行物体检测的 ROS 2 软件包。
- [ROS2 Object Messages](https://github.com/intel/ros2_object_msgs)：用于描述物体的 ROS 2 消息。
- [ROS2 Object Analytics](https://github.com/intel/ros2_object_analytics)：用于物体检测、跟踪以及二维、三维定位的 ROS 2 软件包。
- [ROS2 Message Filters](https://github.com/ros2/message_filters)：根据时间戳同步消息的 ROS 2 软件包。
- [ROS2 CV Bridge](https://github.com/ros-perception/vision_opencv/tree/ros2/cv_bridge)：连接 ROS 2 与 OpenCV 的软件包。
- [ROS2 Object Map](https://github.com/intel/ros2_object_map)：利用 ROS 2 Object Analytics 提供的信息，在 SLAM 过程中为地图上的物体添加标记。
- [ROS2 Moving Object](https://github.com/intel/ros2_moving_object)：利用 ROS 2 Object Analytics 提供的信息，提供物体运动数据，例如 x、y、z 轴上的速度。
- [ROS2 Grasp Library](https://github.com/intel/ros2_grasp_library)：用于抓取位置分析的 ROS 2 软件包，兼容 [MoveIt](https://github.com/ros-planning/moveit2.git) 的抓取接口。
- [ROS2 Navigation](https://github.com/ros-planning/navigation2)：用于机器人导航的 ROS 2 软件包，已集成到 ROS 2 Crystal 发行版。
- [Intel Robot DevKit（SDK）](https://github.com/intel/robot_devkit)：开源项目，使开发者能够基于 ROS 2 框架，便捷、高效地创建、定制、优化机器人软件栈，并部署到自主移动机器人（AMR）平台上。

<span id="reference"></span>

## 参考资料

<https://wiki.ros.org/IntelROSProject> 中的 ROS 组件说明展示了这些软件包之间的关系，这些关系同样适用于 ROS 2。
