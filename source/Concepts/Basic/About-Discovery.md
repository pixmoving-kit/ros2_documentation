<span id="discovery"></span>

# 发现机制

ROS 2 的底层中间件会自动发现节点，其过程可以概括为：

1. 节点启动时，会向网络中同一 ROS 域内的其他节点宣告自身的存在。ROS 域由 `ROS_DOMAIN_ID` 环境变量指定。其他节点会回应并提供自身信息，以便建立合适的连接并开始通信。
2. 节点会周期性地宣告自身的存在，因此即使初始发现阶段已经结束，仍能与新发现的实体建立连接。
3. 节点离线时，会通知其他节点。

只有[服务质量（QoS）](../../Tutorials/Demos/Quality-of-Service.md)设置兼容的节点才会建立连接。

以 [talker-listener 演示](../../Installation/Alternatives/Ubuntu-Development-Setup.md#talker-listener)为例：在一个终端中运行 C++ talker 节点，向某个话题发布消息；在另一个终端中运行 Python listener 节点，订阅同一个话题。

你会看到两个节点自动发现彼此，并开始交换消息。
