---
translation_status: machine_translated
source: Concepts/Basic/About-Discovery.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="discovery"></span>

# 发现机制

通过ROS 2的内置中间软件自动发现节点,可以归纳如下:

1.  当一个节点启动时,它会以相同的ROS域名(ROS_DOMAIN_ID环境变量设置)在网络上向其他节点发布其存在广告. 节点会以自己的信息来响应这个广告,从而可以进行适当的连接,节点也可以进行通信.

2.  节点定期公布其存在情况,以便与新发现的实体建立联系,即使在初始发现期之后也是如此。

3.  节点下线时向其他节点发布广告.

节点只有在具有兼容性时才会与其他节点建立连接 [服务质量](../../Tutorials/Demos/Quality-of-Service.md) 设置。

拿着 [谈话者-听众演示](../../Installation/Alternatives/Ubuntu-Development-Setup.md#talker-listener) 例如,在一个终端中运行 C++ 聊天器节点将发布关于一个话题的信息,在另一个终端中运行的 Python 聆听器节点将订阅关于同一话题的信息。

您应该看到这些节点会自动发现彼此, 并开始交换消息 。
