<span id="rosbag2-overriding-qos-policies"></span>
<span id="ros2bag-qos-override"></span>
# rosbag2：覆盖 QoS 策略

**目标：** 覆盖 Ros2Bag 在录制和回放时使用的 QoS 配置。

<span id="background"></span>
## 背景

ROS 2 引入 DDS 后，录制和回放数据时需要考虑发布者与订阅者节点的服务质量（QoS）兼容性。QoS 的详细工作原理见[服务质量设置](../Concepts/Intermediate/About-Quality-of-Service-Settings.md)。对于本指南，只需了解：可靠性和持久性策略会影响发布者与订阅者是否兼容，以及能否接收彼此的数据。

Ros2Bag 在录制或回放话题数据时，会调整请求或提供的 QoS 配置，以避免消息丢失。回放时，Ros2Bag 也会尝试保留话题原先提供的策略。某些情况下，需要显式指定 QoS 配置才能让 Ros2Bag 录制或回放话题。可以通过命令行参数 `--qos-profile-overrides-path` 指定这些覆盖设置。

<span id="using-qos-overrides"></span>
## 使用 QoS 覆盖设置

覆盖配置的 YAML 结构是一个以话题名称为键的字典，每个话题下通过键值对设置各项 QoS 策略：

```yaml
topic_name: str
  qos_policy_name: str
  ...
  qos_duration: object
    sec: int
    nsec: int
```

未指定的策略会使用 Ros2Bag 的默认值。对于 `deadline` 或 `lifespan` 等基于时长的策略，需要同时指定秒和纳秒。策略值使用各策略的简写键，可通过 `ros2 topic pub --help` 等 `ros2topic` 子命令查看。所有值列于下方，供参考：

```yaml
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

假设话题 `/talker` 提供 `transient_local` 持久性策略。ROS 2 发布者默认使用 `volatile` 持久性策略。

```console
$ ros2 topic pub -r 0.1 --qos-durability transient_local /talker std_msgs/String "data: Hello World"
```

要让 Ros2Bag 录制这些数据，可以为该话题覆盖录制时的策略：

```yaml
# durability_override.yaml
/talker:
  durability: transient_local
  history: keep_all
```

然后从命令行调用：

```console
$ ros2 bag record -a -o my_bag --qos-profile-overrides-path durability_override.yaml
```

如果希望回放 bag 文件时采用不同的可靠性策略，可以这样指定：

```yaml
# reliability_override.yaml
/talker:
  reliability: best_effort
  history: keep_all
```

从命令行调用：

```console
$ ros2 bag play --qos-profile-overrides-path reliability_override.yaml my_bag
```

可以使用 `ros2 topic` 查看结果：

```console
$ ros2 topic echo --qos-reliability best_effort /talker std_msgs/String
```
