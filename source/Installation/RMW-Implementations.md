<span id="rmw-implementations"></span>

# RMW 实现

ROS 2 默认使用 DDS 作为[中间件](https://design.ros2.org/articles/ros_on_dds.html)，兼容多家供应商的 DDS 或 RTPS（DDS 的网络传输协议）实现。目前支持 eProsima 的 Fast DDS、RTI 的 Connext DDS、Eclipse Cyclone DDS，以及 GurumNetworks 的 GurumDDS。

ROS 2 也支持 Zenoh 等非 DDS 的 RMW 实现。

各发行版支持的 RMW 供应商见 [REP-2000](https://reps.openrobotics.org/rep-2000/)。

默认的 RMW 供应商是 eProsima，其实现为 Fast DDS。

可选实现如下：

- [DDS 实现](RMW-Implementations/DDS-Implementations.md)：介绍如何使用 DDS。
- [非 DDS 实现](RMW-Implementations/Non-DDS-Implementations.md)：介绍如何使用非 DDS 实现。
