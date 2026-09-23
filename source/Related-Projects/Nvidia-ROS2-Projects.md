---
translation_status: machine_translated
source: Related-Projects/Nvidia-ROS2-Projects.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="nvidia-ros-2-projects"></span>

# NVIDIA ROS 2 项目

NVIDIA为机器人开发AI应用提供了软件包.

<span id="isaac-ros-projects"></span>

## ISAAC ROS项目

- [预建 ROS 2 宏博支持](https://nvidia-isaac-ros.github.io/getting_started/isaac_ros_buildfarm_cdn.html):为Ubuntu 20.04上的ROS 2 Humble为Jetson和其他来自NVIDIA建设农场的arch64平台预先建造的Debian包.

- [与 NITROS 的 CUDA 软件](https://nvidia-isaac-ros.github.io/concepts/nitros/cuda_with_nitros.html):这帮助用户开发自己的CUDA启用节点,与NITROS合作,Isaac ROS实施类型适应和类型谈判,从而在ROS 2中加速计算.

- [艾萨克罗斯 NITROS桥](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros_bridge): NITROS 桥用于优化Isaac ROS包与现有ROS 1 应用程序的集成。 使用此功能将您的ROS 应用程序连接到ROS 2 , 与使用传统的ROS 桥相比, 以 \> 2x 速度加速计算。

- [诺瓦·卡特](https://nvidia-isaac-ros.github.io/robots/nova_carter.html):用于机器人开发和研究的参考AMR,由Isaac ROS和Nav2提供动力,并与Open Navigation进行调制,用于远程操作,绘图和导航.

- [艾萨克·罗斯·诺瓦](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova):这个寄存器提供了一套优化的软件包,可以与艾萨克·诺瓦·奥林的传感器套件接口.

- [Isaaac ROS Pose估计](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_pose_estimation): 此寄存器包含ROS 2 套件,用以预测一个物体的姿势.

- [ROS2_Benchmark](https://github.com/NVIDIA-ISAAC-ROS/ros2_benchmark): ros2_基准标记提供了测量这些复杂图表的吞吐量,耐久性,以及计算其利用率的工具,而不改变正在测试的代码.

- [Isaac ROS 基准](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_benchmark): 本套件基于ros2_基准标记,提供配置以基准Isaac ROS图.

- [Isaac ROS 地图本地化](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_map_localization): 本模块包含 ROS 2 包, 用于 lidar 处理, 以估计相对于地图的构成。 占用网格本地化器处理一个 plamar 范围扫描, 以估计占用网格图中的布局; 对于大多数地图来说, 使用时间不到1秒 。

- [艾萨克·罗斯·尼特罗斯](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros): Isaac Transport for ROS 软件包,用于硬件-加速友好移动消息.

- [Isaac ROS 压缩](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_compression):硬件加速 NITROS 包压缩相机数据捕捉和回放,用于开发AI模型和感知功能,压缩4x1080p相机在30fps(\>120fps total)时将数据足迹减小~10x.

- [以撒 ROS DNN 立体声深度](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_dnn_stereo_depth): DNN立体差异包括预测立体输入差异的软件包.

- [Isaac ROS 深度分割](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_depth_segmentation): 硬件加速包用于深度分割.

- [伊萨克·罗斯·恩夫布洛克斯](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nvblox) :硬件加速的3D场景重建以及使用nvblox的Nav2本地成本映射提供者.

- [Isaac ROS 对象检测](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_object_detection) : 深层学习模型支持对象检测,包括DetectNet.

- [Isaaac Ros DNN 推论](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_dnn_inference) :这个寄存器提供了两个NVIDIA GPU加速的ROS 2节点,这些节点使用自定义模型进行深层的学习推论. 一个节点使用TensorRT SDK,另一个则使用Triton SDK.

- [Isaac ROS 视觉 SLAM](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_visual_slam) :这个寄存器提供了一个ROS 2软件包,该软件包使用Isaac Elbrus GPU加速库来估计立体视觉惯性读物.

- [Isaac ROS 任务客户端](http://github.com/NVIDIA-ISAAC-ROS/isaac_ros_mission_client) :本寄存器接收ROS的状态和错误更新,并将其转换为VDA5050 JSON消息,由ROS 2 - \> MQTT节点传输给Mission Department.

- [艾萨克·罗斯·阿尔格斯相机](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_argus_camera) :这个寄存器提供了单光学和立体结点,使ROS开发人员可以通过一个CSI接口使用连接到Jetson平台的相机.

- [Isaac ROS 图像管道](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_image_pipeline) :这个元包提供了与标准,基于CPU的图像_管道元包相似的功能,但是通过利用杰森平台的专业计算机视觉硬件来做到这一点.

- [Isaac ROS 常见](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common) :Isaac ROS通用设施,与Isaac ROS套件配套使用.

- [艾萨克·罗斯(Isaac ROS)](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_apriltag) :ROS 2节点使用NVIDIA GPU加速的AprilTags库在图像中检测AprilTags,并发布他们的姿势,ids,以及额外的元数据.

<span id="additional-projects"></span>

## 其他项目

- [ROS和ROS 2 多克鱼](https://github.com/dusty-nv/jetson-containers): Dockerfiles for ROS 2 基于l4t,所有您要构建自己的 Docker 图像 。

- [ROS / ROS 2 加速深层学习节点软件包](https://github.com/dusty-nv/ros_deep_learning):深层学习图像识别,对象检测,以及语义分解推断节点和相机/视频流线节点,用于ROS/ROS 2 使用 [喷嘴推论](https://github.com/dusty-nv/jetson-inference) 库和 [NVIDIA 你好 AI 世界教程](https://developer.nvidia.com/embedded/twodaystoademo).

<span id="simulation-projects"></span>

## 模拟项目

- [伊萨克·西姆·纳维](https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_ros2_navigation.html) :在这个ROS 2样本中,我们正在演示Omniverse Isaac Sim与ROS 2 Nav2项目集成.

- [Isaac Sim 多机器人 ROS 2 导航](https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_ros2_multi_navigation.html) :在这个ROS 2样本中,我们正在演示Omniverse Isaac Sim与ROS 2 Nav2堆集成,同时进行多个机器人导航.
