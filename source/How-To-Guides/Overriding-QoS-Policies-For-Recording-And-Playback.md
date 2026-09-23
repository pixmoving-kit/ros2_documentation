---
translation_status: machine_translated
source: How-To-Guides/Overriding-QoS-Policies-For-Recording-And-Playback.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="rosbag2-overriding-qos-policies"></span> <span id="ros2bag-qos-override"></span>

# rosbag2：覆盖 QoS 策略

**目标：** 覆盖 Ros2Bag QoS 配置配置设置用于录制和播放 。

<span id="background"></span>

## 背景

在ROS 2中引入DDS后,在录制和播放回放数据时,需要考虑出版商/订阅者节点的服务质量兼容性(QoS). 有关QoS工作方式的更多细节可以找到 [这儿](../Concepts/Intermediate/About-Quality-of-Service-Settings.md)为本指南的目的,只需知道只有可靠性和耐久性政策才能影响出版商/订阅商是否兼容,并能从对方接收数据即可。

Ros2Bag 在从一个话题录制/播放数据时修改其请求/出价的 QoS 配置文件, 以防止丢弃的消息。 在重播期间, Ros2bag 也试图保留该话题最初提供的政策 。 某些情况可能需要指定明确的 QoS 配置文件设置, 这样 Ros2Bag 就可以记录/ 回放主题 。 这些 QoS 配置文件的覆盖文件可以使用 CLI 指定 。 `--qos-profile-overrides-path` 旗帜。

<span id="using-qos-overrides"></span>

## 使用 QoS 覆盖

YAML 描述文件的图案是一份主题名称的词典,每个QoS策略都有密钥/值对:

``` yaml
topic_name: str
  qos_policy_name: str
  ...
  qos_duration: object
    sec: int
    nsec: int
```

如果不指定一个策略值, 则该值会回落到 Ros2Bag 使用的默认值。 如果您指定基于持续时间的策略, 如 `deadline` 或 时 间 `lifespan`,您需要同时指定秒数和纳秒数。政策值由策略的短键决定,这些键可以用 `ros2topic` 动词如: `ros2 topic pub --help`。所有数值在下文中复制,以供参考。

``` yaml
history: [keep_all, keep_last]
depth: int
reliability: [system_default, reliable, best_effort, unknown]
durability: [system_default, transient_local, volatile, unknown]
deadline:
  sec: int
  nsec: int
lifespan:
  sec: int
  nsec: int
liveliness: [system_default, automatic, manual_by_topic, unknown]
liveliness_lease_duration:
  sec: int
  nsec: int
avoid_ros_namespace_conventions: [true, false]
```

<span id="example"></span>

## 示例

考虑一个话题 `/talker` 提供 `transient_local` Durable 政策. ROS 2 出版商默认请求 `volatile` 达利布利.

``` console
$ ros2 topic pub -r 0.1 --qos-durability transient_local /talker std_msgs/String "data: Hello World"
```

为了让Ros2Bag记录数据,我们想推翻这一具体专题的录音政策,例如:

``` yaml
# durability_override.yaml
/talker:
  durability: transient_local
  history: keep_all
```

从CLI调用它:

``` console
$ ros2 bag record -a -o my_bag --qos-profile-overrides-path durability_override.yaml
```

如果我们想要播放包文件,但有不同的可靠性政策,我们可以指定一个这样的文件;

``` yaml
# reliability_override.yaml
/talker:
  reliability: best_effort
  history: keep_all
```

从CLI调用它:

``` console
$ ros2 bag play --qos-profile-overrides-path reliability_override.yaml my_bag
```

我们可以看到结果 `ros2 topic`

``` console
$ ros2 topic echo --qos-reliability best_effort /talker std_msgs/String
```
