<span id="nvidia-ros-2-projects"></span>

# NVIDIA ROS 2 项目

NVIDIA 提供用于开发机器人 AI 应用的软件包。

<span id="isaac-ros-projects"></span>

## Isaac ROS 项目

- [预构建的 ROS 2 Humble 支持](https://nvidia-isaac-ros.github.io/getting_started/isaac_ros_buildfarm_cdn.html)：NVIDIA 构建农场为运行 Ubuntu 20.04 的 Jetson 及其他 aarch64 平台提供预构建的 ROS 2 Humble Debian 软件包。
- [CUDA with NITROS](https://nvidia-isaac-ros.github.io/concepts/nitros/cuda_with_nitros.html)：帮助用户开发可与 NITROS 配合使用、支持 CUDA 的节点。NITROS 是 Isaac ROS 对类型适配和类型协商机制的实现，可在 ROS 2 中实现加速计算。
- [Isaac ROS NITROS Bridge](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros_bridge)：用于优化 Isaac ROS 软件包与现有 ROS 1 应用集成的 NITROS 桥。将 ROS 应用桥接到 ROS 2 进行加速计算时，与传统 ROS 桥相比，可实现超过 2 倍的加速。
- [Nova Carter](https://nvidia-isaac-ros.github.io/robots/nova_carter.html)：面向机器人开发和研究的 AMR 参考平台，使用 Isaac ROS 和 Nav2，并与 Open Navigation 合作调优，支持远程操控、建图和导航。
- [Isaac ROS Nova](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nova)：提供一组经过优化的软件包，用于连接 Isaac Nova Orin 传感器套件。
- [Isaac ROS Pose Estimation](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_pose_estimation)：包含用于预测物体位姿的 ROS 2 软件包。
- [ROS2_Benchmark](https://github.com/NVIDIA-ISAAC-ROS/ros2_benchmark)：无需修改被测代码，即可测量复杂计算图的吞吐量、延迟和计算资源利用率。
- [Isaac ROS Benchmark](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_benchmark)：基于 ros2_benchmark，为 Isaac ROS 计算图提供基准测试配置。
- [Isaac ROS Map Localization](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_map_localization)：包含通过激光雷达数据处理来估计相对于地图的位姿的 ROS 2 软件包。占据栅格定位器处理平面距离扫描数据，在占据栅格地图中估计位姿；对大多数地图，这一过程不到 1 秒。
- [Isaac ROS Nitros](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros)：Isaac Transport for ROS 软件包，提供适合硬件加速的消息传输方式。
- [Isaac ROS Compression](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_compression)：使用硬件加速的 NITROS 软件包压缩相机数据，支持采集和回放，用于开发 AI 模型和感知功能。可以按每路 30 fps 压缩 4 路 1080p 相机数据（总计超过 120 fps），将数据占用降低约 10 倍。
- [Isaac ROS DNN Stereo Depth](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_dnn_stereo_depth)：DNN Stereo Disparity 包含预测双目输入视差的软件包。
- [Isaac ROS Depth Segmentation](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_depth_segmentation)：用于深度分割的硬件加速软件包。
- [Isaac ROS Nvblox](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nvblox)：使用 nvblox 进行硬件加速的三维场景重建，并为 Nav2 提供局部代价地图。
- [Isaac ROS Object Detection](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_object_detection)：提供 DetectNet 等深度学习物体检测模型支持。
- [Isaac ROS DNN Inference](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_dnn_inference)：提供两个 NVIDIA GPU 加速的 ROS 2 节点，使用自定义模型执行深度学习推理。一个节点使用 TensorRT SDK，另一个使用 Triton SDK。
- [Isaac ROS Visual SLAM](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_visual_slam)：提供使用 Isaac Elbrus GPU 加速库来估计双目视觉惯性里程计的 ROS 2 软件包。
- [Isaac ROS Mission Client](http://github.com/NVIDIA-ISAAC-ROS/isaac_ros_mission_client)：接收 ROS 的状态和错误更新，将其转换为 VDA5050 JSON 消息，再通过 ROS 2 → MQTT 节点发送给 Mission Dispatch。
- [Isaac ROS Argus Camera](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_argus_camera)：提供单目和双目节点，让 ROS 开发者能够使用通过 CSI 接口连接到 Jetson 平台的相机。
- [Isaac ROS Image Pipeline](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_image_pipeline)：功能类似于标准的、基于 CPU 的 image_pipeline 元软件包，但使用 Jetson 平台专用的计算机视觉硬件实现。
- [Isaac ROS Common](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common)：供 Isaac ROS 软件包套件配合使用的通用工具。
- [Isaac ROS AprilTag](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_apriltag)：使用 NVIDIA GPU 加速的 AprilTags 库检测图像中的 AprilTag，并发布其位姿、ID 和其他元数据的 ROS 2 节点。

<span id="additional-projects"></span>

## 其他项目

- [ROS 和 ROS 2 DockerFiles](https://github.com/dusty-nv/jetson-containers)：基于 l4t 的 ROS 2 Dockerfile，可用于构建自己的 Docker 镜像。
- [用于加速深度学习节点的 ROS / ROS 2 软件包](https://github.com/dusty-nv/ros_deep_learning)：基于 [jetson-inference](https://github.com/dusty-nv/jetson-inference) 库及 [NVIDIA Hello AI World 教程](https://developer.nvidia.com/embedded/twodaystoademo)，为 ROS / ROS 2 提供深度学习图像识别、物体检测和语义分割推理节点，以及相机和视频流节点。

<span id="simulation-projects"></span>

## 仿真项目

- [Isaac Sim Nav2](https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_ros2_navigation.html)：展示 Omniverse Isaac Sim 与 ROS 2 Nav2 项目的集成。
- [Isaac Sim 多机器人 ROS 2 导航](https://docs.omniverse.nvidia.com/app_isaacsim/app_isaacsim/tutorial_ros2_multi_navigation.html)：展示 Omniverse Isaac Sim 与 ROS 2 Nav2 软件栈集成，实现多个机器人同时导航。
