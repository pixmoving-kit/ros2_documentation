---
translation_status: machine_translated
source: Releases/Release-Lyrical-Luth.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="lyrical-luth-codename-lyrical-may-2026"></span> <span id="lyrical-release"></span><span id="latest-release"></span>

# Lyrical Luth（代号 lyrical；2026 年 5 月）

*Lyrical Luth* 是 ROS 2. 的第十二版,它是长期支持(LTS)的发布,它被支持到2031年5月.

- [安装 Lyrical Luth( Lyrical Luth) 语句](https://docs.ros.org/en/lyrical/Installation.html)

- [Lyrical Luth 发布时间表](lyrical/release-timeline.md)

- [Lyrical Luth 支持的平台](lyrical/supported-platforms.md)

<span id="new-features-in-lyrical"></span>

## 语言的新特征

本节着重介绍ROS Lyrical中的一些新特征。 [完整 ROS 语言变化日志](Lyrical-Luth-Complete-Changelog.md).

<span id="callback-group-events-executor-rclcpp"></span>

### 回调组事件执行器( C) (M)`rclcpp`)

寻找更好的执行器性能 ? 请检查新的 Callback Group Evolution 执行器。 如它的前身 : `EventsExecutor`,则 `EventsCBGExecutor` 使用事件队列处理已准备好的实体。然而, `EventsCBGExecutor` 添加对 ROS 时间和多个线程的多个源的支持。 与 Single 和 Multithread 执行器相比, `EventsCBGExecutor` 使用10%到15%的CPU.

用即兴演奏来试试看 `rclcpp::executors::EventsCBGExecutor`:

``` c++
#include <rclcpp/rclcpp.hpp>

// ... class MyNode ...

int main(int argc, char ** argv)
{
  rclcpp::init(argc, argv);
  auto node = std::make_shared<MyNode>();
  rclcpp::executors::EventsCBGExecutor executor;
  executor.add_node(node);
  executor.spin();
  rclcpp::shutdown();
  return 0;
}
```

使用可混凝土节点? `EventsCBGExecutor` 使用新软件 `--executor-type` 参数。

``` console
ros2 run rclcpp_components component_container --executor-type events-cbg
```

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <node_container pkg="rclcpp_components" exec="component_container" name="my_node_container" namespace="" args="--executor-type events-cbg">
    <!-- Your composable nodes here -->
  </node_container>
</launch>
```

更多信息,见 [ros2/rclcpp#3097](https://github.com/ros2/rclcpp/pull/3097), [ros2/rclcpp#3134](https://github.com/ros2/rclcpp/pull/3134),以及 [ros2/rclcpp#3137](https://github.com/ros2/rclcpp/pull/3137).

<span id="parameter-range-descriptors-check-bounds-for-integer-and-double-arrays-rclcpp"></span>

### 参数范围描述符检查整数和双数组的界限(`rclcpp`)

您的节点是否有整数或双数组? 您需要限制这些数组中的值吗 ? `rclcpp` 节点现在验证这些阵列上的范围限制。 使用下面的代码, 节点只允许设置 。 `my_integer_array` 改为包含2至10之间的偶数整数的列表(包含)。

``` c++
rcl_interfaces::msg::ParameterDescriptor descriptor;
descriptor.integer_range.resize(1);
auto & integer_range = descriptor.integer_range.at(0);
integer_range.from_value = 2;
integer_range.to_value = 10;
integer_range.step = 2;
node->declare_parameter("my_integer_array", std::vector<int64_t>{2, 4, 6, 8, 10}, descriptor);
```

见 [ros2/rclcpp#2828](https://github.com/ros2/rclcpp/pull/2828) 为了更多的信息。

<span id="asyncnode-lets-you-use-asyncio-rclpy"></span>

### `AsyncNode` 允许您使用 `asyncio` (`rclpy`)

想要用吗? `asyncio` 财务报告和财务报告 `rclpy` 同时也是? `AsyncNode` 类。此节点运行一个 `asyncio` 事件循环。 调用 `await` 任意 `asyncio` 从任何订阅、服务和计时器回调中操作。请尝试 `await client.call(request)` 等待服务电话, 和时空知觉 `await clock.sleep(...)`。此类使用CPU比默认使用少很多 `SingleThreadedExecutor`.

``` python
import asyncio
import rclpy
from rclpy.experimental import AsyncNode

class HelloWorldNode(AsyncNode):
    def __init__(self):
        super().__init__('hello_world_node')
        self._timer = self.create_timer(5.0, self._cb)

    async def _cb(self):
        self.get_logger().info('Hello')
        await self.get_clock().sleep(1.0)
        self.get_logger().info('World!')

async def _main():
    with rclpy.init():
        await HelloWorldNode().run()

if __name__ == '__main__':
    asyncio.run(_main())
```

<span id="publish-messages-without-copying-data-using-rosidl-buffer"></span>

### 使用不复制数据而发布信件 `rosidl::Buffer`

您是否发布关于ROS主题的数据, 但使用其它地方的数据, 像 GPU 一样 ? 厌倦了将数据复制出 GPU 之后再发布, 仅仅将其复制回用户的 GPU 中 ? `rosidl::Buffer` 以发布和订阅 ROS 消息而无需移动来自其他地方的数据。

全部人员 `uint8[]` 字段现在有类型 `rosidl::Buffer<uint8_t>` 以 C++ 代替 `std::vector<uint8_t>`中定义 ROS 消息。 `uint8[]` 字段并安装适当的 `rosidl::BufferBackend` 请注意,只有出版商和订户使用 `rmw_fastrtps_cpp` 可能暂时使用这个功能,但 [支持Zenoh的人来了](https://github.com/ros2/rmw_zenoh/pull/930).

使用自定义硬件加速器或机器学习库? 您也可以从中受益 。

<span id="annotate-types-in-yaml-parameter-files"></span>

### YAML 参数文件中的注释类型

厌倦了 `rcl` 将模棱两可的 YAML 参数值解释为错误类型 ? 在 ROS Lyrical 中,使用 YAML 标记来指定正确的类型 。

``` yaml
my_node:
  ros__parameters:
    string_param: !!str true
    bool_param: !!bool yes
    int_param: !!int 0
    float_param: !!float 10
    seq_param: !!seq [10, 0, -10]
    map_param: !!map {str: string, bool: true, int: 10, float: 1.1}
```

见 [ros2/rcl#1275](https://github.com/ros2/rcl/pull/1275) 为了更多的信息。

<span id="per-message-log-severity-in-launch-files"></span>

### 发射文件中的每个消息记录严重性

ROS Lyrical 现在支持在发射文件中的每个消息日志重度级别。 这样在调试时更容易找到重要消息或者忽略日志文件中不重要的消息 !

使用新设置指定日志级别 `level` 论点: `log` 动作。或者,使用新的 `log_debug`, `log_info`, `log_warning`,或 `log_error` 行动。

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <log level="INFO" message="Hello world! (log level=INFO)" />
  <log_debug message="Hello world debug!" />
  <log_info message="Hello world!" />
  <log_warning message="Hello world warning!" />
  <log_error message="Hello world error!" />
</launch>
```

更多信息请看 [ros2/launch#866](https://github.com/ros2/launch/pull/866).

<span id="new-substitutions-in-xml-and-yaml-launch-files"></span>

### XML 和 YAML 发射文件中的新替换

使用 XML 或 YAML 发射文件吗 ? [替代](../Tutorials/Intermediate/Launch/Using-Substitutions.md) 使您的发射文件在发射时评价变量。 发射前端( 能够使用 XML 和 YAML 发射文件的东西) 现在可以使用 。 `string-join` 财务报告和财务报告 `path-join` 替换。

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <log_info message="Check out $(string-join . https://docs ros org)"/>
  <log_info message="Don't forget to source /$(path-join opt ros lyrical $(string-join . setup bash))"/>
</launch>
```

见 [ros2/launch#857](https://github.com/ros2/launch/pull/857) 财务报告和财务报告 [ros2/launch#943](https://github.com/ros2/launch/pull/943) 为了更多的信息。

<span id="choose-ros-logging-backend-at-runtime"></span>

### 在运行时选择 ROS 记录后端

您是否需要使用 ROS 和另一个有自己的日志系统的框架 ? ROS 支持替换其日志后端, 但之前需要重建 。 `rcl` 您可以在运行时更改日志执行 ! 设置 `RCL_LOGGING_IMPLEMENTATION` 要在日志后端之间切换的环境变量。 有效的值是 :

- `rcl_logging_spdlog`

- `rcl_logging_noop`

- 或者你自己的自定义日志执行!

如果未指定,ROS使用 `rcl_logging_spdlog` 默认。

见 [ros2/rcl#1178](https://github.com/ros2/rcl/issues/1178), [ros2/rcl#1276](https://github.com/ros2/rcl/pull/1276),以及 [ros2/rcl_logging#135](https://github.com/ros2/rcl_logging/pull/135) 更多细节。

<span id="control-bag-recording-remotely-using-ros-services"></span>

### 使用ROS服务远程记录控制袋

要远程控制包记录吗 ? 使用 `rosbag2`向下列机构提供的新服务:

- 开始录制 `~/record`

- 停止录制 `~/stop`

- 开始专题发现 `~/start_discovery`

- 停止专题发现 `~/stop_discovery`

- 查询发现状态 `~/is_discovery_running`

``` bash
ros2 bag record --all -o /tmp/my_awesome_bag

# In another terminal, stop the existing recording
ros2 service call /rosbag2_recorder/stop rosbag2_interfaces/srv/Stop "{}"
# Start recording again at a new location
ros2 service call /rosbag2_recorder/record rosbag2_interfaces/srv/Record "{uri: 'file:///tmp/my_awesome_bag_2'}"
```

见 [ros2/rosbag2#2248](https://github.com/ros2/rosbag2/pull/2248) 更多细节。

<span id="control-bag-playback-and-recording-using-python"></span>

### 使用 Python 播放和记录控制袋

想要从程序上控制 bag 播放和录制吗 ? 之前, Python 用户依赖阻断命令行样式的帮助。 现在 Python 用户可以调用 API 暂停、 恢复、 停止、 寻求、 播放下一步、 控制旋转和等待事件 。

记录实例 :

``` python
import rclpy
import rosbag2_py

with rclpy.init():
    # Configure storage and initialize the recorder
    storage_opts = rosbag2_py.StorageOptions(uri='/tmp/my_awesome_bag', storage_id='mcap')
    record_opts = rosbag2_py.RecordOptions()
    record_opts.all_topics = True

    recorder = rosbag2_py.Recorder(storage_opts, record_opts)

    # Start the ROS node spinner and kick off the recording thread
    recorder.start_spin()
    recorder.record()

    # Pause the recording and verify the current state
    recorder.pause()
    print(recorder.is_paused())

    # Terminate recording and shut down the spinner threads
    recorder.stop()
    recorder.stop_spin()
```

回放示例 :

``` python
import rclpy
import rosbag2_py

with rclpy.init():
    # Configure storage and initialize the player
    storage_opts = rosbag2_py.StorageOptions(uri='/tmp/my_awesome_bag', storage_id='mcap')
    play_opts = rosbag2_py.PlayOptions()
    play_opts.start_paused = True

    player = rosbag2_py.Player(storage_opts, play_opts)

    # Start the ROS node spinner and kick off the playback thread
    player.start_spin()
    player.play()

    # Verify playback startup and retrieve bag timing metadata
    print(player.wait_for_playback_to_start(1.0))
    print(player.wait_for_playback_to_start_exclusively(1.0))
    print(player.get_starting_time())
    print(player.get_playback_duration())

    # Control the playback state and step through messages manually
    print(player.is_paused())
    print(player.play_next())
    player.resume()
    player.pause()
    player.seek(0)

    # Await playback completion and terminate the worker threads
    print(player.wait_for_playback_to_finish(1.0))
    print(player.wait_for_playback_to_finish_exclusively(1.0))
    player.stop()
    player.stop_spin()
```

见 [ros2/rosbag2#2047](https://github.com/ros2/rosbag2/pull/2047), [ros2/rosbag2#2062](https://github.com/ros2/rosbag2/pull/2062), [ros2/rosbag2#2061](https://github.com/ros2/rosbag2/pull/2061),以及 [ros2/rosbag2#2095](https://github.com/ros2/rosbag2/pull/2095) 更多细节。

<span id="circular-bag-recording-with-limit-on-number-of-bags"></span>

### 限制袋数的圆形袋记录

以有限的磁盘空间记录机器人上的数据? 尝试新的 `--max-bag-files` 选项。它通过在创建新文件时自动删除最古老的拆分文件来限制存储在磁盘上的袋文件的最大数量。

``` bash
# Max bag size: 100MB
ros2 bag record --all --max-bag-size 100000000 --max-bag-files 5
```

见 [ros2/rosbag2#2218](https://github.com/ros2/rosbag2/pull/2218) 更多细节。

<span id="more-descriptive-bag-split-names"></span>

### 更多描述包拆分名称

你找不到哪个袋子吗? `rosbag2` 现在名包拆分,使每个名包都具有自我描述性,并且可以按时间顺序追踪.

``` text
{counter}_{prefix}_{timestamp}.{extension}
```

- **计数器**: 拆分指数(整数从 0 开始, *不添加为零*)

- **前缀**: 来源于包目录名称, 并删除任何默认的时间戳后缀

- **时间戳**: 文件创建时的本地时间,格式化为 `YYYY_MM_DD-HH_MM_SS`

- **扩展**: 袋文件扩展名。例如, `.mcap`, `.db3`

见 [ros2/rosbag2#2265](https://github.com/ros2/rosbag2/pull/2265) 更多细节。

<span id="catch-data-loss-early-with-rosbag2-message-loss-observability"></span>

### 提前捕捉数据丢失 `rosbag2` 信息损失可观察性

说您已经建立了一个强大的系统来记录您的机器人的数据,但有一个问题。您是如何做到的 。 *懂了吗?* 有问题吗? `rosbag2`新的讯息丢失,

`rosbag2` 现在从运输层和记录器内部收集信息损失统计数据。 `events/rosbag2_messages_lost` 主题。控制使用 `--stats_max_publishing_rate`.

``` bash
# Publish message loss statistics at most 10 Hz
ros2 bag record --all --stats_max_publishing_rate 10 -o /tmp/my_awesome_bag
# If all is going well, you should see no output from this command
ros2 topic echo /events/rosbag2_messages_lost
```

见 [ros2/rosbag2#2039](https://github.com/ros2/rosbag2/pull/2039), [ros2/rosbag2#2144](https://github.com/ros2/rosbag2/pull/2144),以及 [ros2/rosbag2#2150](https://github.com/ros2/rosbag2/pull/2150) 更多细节。

<span id="fish-shell-support"></span>

### `fish` shell 支持

喜欢吗? [鱼壳](https://fishshell.com/)\* Do you want to use with ROS? 现在可以了! 试试新的 `setup.fish` 脚本。

``` shell
source /opt/ros/lyrical/setup.fish
```

见 [ros2/ros2cli#1211](https://github.com/ros2/ros2cli/pull/1211) 财务报告和财务报告 [ament/ament_package#164](https://github.com/ament/ament_package/pull/164) 。用于更多信息。 `fish` 外壳为 `colcon`检查出来 [@Sunrisepeak (日出之声)](https://github.com/Sunrisepeak) [⁇ 鱼包](https://github.com/ros-x/colcon-fish).

<span id="ros2-param-get-a-parameter-from-all-nodes"></span>

### `ros2 param get` 来自所有节点的参数

您是否使用模拟时间 ? 你怎么知道您所有的节点是否使用模拟时间 ? `ros2 param get <param name>` 从所有节点获取参数值。

![](images/ros2_param_get_use_sim_time.gif)

见 [ros2/ros2cli#1174](https://github.com/ros2/ros2cli/pull/1174) 为了更多的信息。

<span id="ros2-param-get-and-set-multiple-parameters-on-one-node"></span>

### `ros2 param` 在一个节点上获取并设置多个参数

要同时在一个节点上获得和设置多个参数吗? 使用 `ros2 param get <node name> <param1> <param2> ...` 从单个节点获取多个值。

``` console
$ ros2 param get /robot_state_publisher frame_prefix ignore_timestamp publish_frequency
frame_prefix:
  String value is:
ignore_timestamp:
  Boolean value is: False
publish_frequency:
  Double value is: 20.0
```

使用 `ros2 param set <node name> <param1> <value1> <param2> <value2> ...` 设置单个节点上的多个值。

``` console
$ ros2 param set /robot_state_publisher frame_prefix foo ignore_timestamp True publish_frequency 10.0
frame_prefix: Set parameter successful
ignore_timestamp: Set parameter successful
publish_frequency: Set parameter successful
```

见 [ros2/ros2cli#1203](https://github.com/ros2/ros2cli/pull/1203) 财务报告和财务报告 [ros2/ros2cli#1204](https://github.com/ros2/ros2cli/pull/1204) 更多细节。

<span id="ros2-doctor-report-now-reports-actions-services-and-environment-variables"></span>

### `ros2 doctor --report` 现在报告行动、服务和环境变量

`ros2 doctor --report` 现在包含关于动作、 服务和 ROS 相关环境变量的信息 。 将此报告包含在您的 GitHub 问题或 AI 提示中, 以更快的速度调试问题 。

``` console
$ ros2 doctor --report

  ACTION LIST
action                 : none
action server count    : 0
action client count    : 0

  ROS ENVIRONMENT
ROS environment variables        : ROS_AUTOMATIC_DISCOVERY_RANGE=SUBNET, ROS_DISTRO=lyrical
rcutils environment variables    :
rmw environment variables        :
# ...

  SERVICE LIST
service          : /dummy_joint_states/describe_parameters
service count    : 1
client count     : 0
service          : /dummy_joint_states/get_parameter_types
service count    : 1
client count     : 0
# ...
```

更多信息见 [ros2/ros2cli#1059](https://github.com/ros2/ros2cli/pull/1059), [ros2/ros2cli#1076](https://github.com/ros2/ros2cli/pull/1076),以及 [ros2/ros2cli#1045](https://github.com/ros2/ros2cli/pull/1045).

<span id="verbose-service-information-ros2-service-info-verbose"></span>

### Verbose服务信息 `ros2 service info --verbose`

您是否调试 ROS 客户端和 ROS 服务之间不匹配的 QoS 设置 ? 请尝试新的 `--verbose` 选项到 `ros2 service info`类似 `ros2 topic info`,此旗帜输出关于客户端和服务的详细信息,以帮助您解决问题。

``` console
$ ros2 service info --verbose /robot_state_publisher/list_parameters
Type: rcl_interfaces/srv/ListParameters
Clients count: 0
Services count: 1
Node name: robot_state_publisher
Node namespace: /
Service type: rcl_interfaces/srv/ListParameters
Service type hash: RIHS01_3e6062bfbb27bfb8730d4cef2558221f51a11646d78e7bb30a1e83afac3aad9d
Endpoint type: SERVER
Endpoint count: 2
GIDs:
- Request Reader : 01.0f.c8.b6.fe.43.b3.44.00.00.00.00.00.00.0e.04
- Response Writer : 01.0f.c8.b6.fe.43.b3.44.00.00.00.00.00.00.0f.03
QoS profiles:
- Request Reader :
      Reliability: RELIABLE
      History (Depth): KEEP_LAST (1000)
      Durability: VOLATILE
      Lifespan: Infinite
      Deadline: Infinite
      Liveliness: AUTOMATIC
      Liveliness lease duration: Infinite
- Response Writer :
      Reliability: RELIABLE
      History (Depth): KEEP_LAST (1000)
      Durability: VOLATILE
      Lifespan: Infinite
      Deadline: Infinite
      Liveliness: AUTOMATIC
      Liveliness lease duration: Infinite
```

要程序化获取客户端和服务信息吗 ? 使用这些新的 C++ 和 Python API 。

``` python
node.get_servers_info_by_service('some/service/name')
node.get_clients_info_by_service('some/service/name')
```

``` c++
node->get_servers_info_by_service("some/service/name");
node->get_clients_info_by_service("some/service/name");
```

见 [ros2/ros2cli#916](https://github.com/ros2/ros2cli/pull/916), [ros2/rclpy#1307](https://github.com/ros2/rclpy/pull/1307),以及 [ros2/rclcpp#2569](https://github.com/ros2/rclcpp/pull/2569) 为了更多的信息。

<span id="ros2-topic-bw-multiple-topics-at-once"></span>

### `ros2 topic bw` 多个议题同时进行

试图了解哪些主题正在使用您的网络带宽吗? 现在可以使用 `ros2 topic bw` 带有多个主题同时传递多个主题的名称 :

``` bash
ros2 topic bw /tf /joint_states
```

或通过 `--all` 实时观看带宽统计。

![](images/ros2_topic_bw_all.gif)

见 [ros2/ros2cli#1124](https://github.com/ros2/ros2cli/pull/1124) 财务报告和财务报告 [ros2/ros2cli#1130](https://github.com/ros2/ros2cli/pull/1130) 为了更多的信息。

<span id="urdf-improvements"></span>

### URDF 改进

URDF发布了一些新功能:

- 质点

- 顶盖几何

- 加速、减速和混蛋限制

添加 `version="1.2"` 给机器人的描述 开始使用它们。

``` xml
<?xml version="1.0" ?>
<robot name="simple_capsule_arm" version="1.2">

  <link name="link1">
    <visual>
      <origin xyz="0 0 0.25" quat_xyzw="0 0 0 1"/>
      <geometry>
        <capsule radius="0.1" length="0.5"/>
      </geometry>
    </visual>
    <collision>
      <origin xyz="0 0 0.25" quat_xyzw="0 0 0 1"/>
      <geometry>
        <capsule radius="0.1" length="0.5"/>
      </geometry>
    </collision>
  </link>

  <joint name="joint1" type="revolute">
    <!-- ... -->
    <!-- Using quaternion for a 90-degree pitch rotation (y-axis) -->
    <origin xyz="0 0 0.5" quat_xyzw="0 0.7071068 0 0.7071068"/>
    <!-- Demonstrating new and existing limits -->
    <limit lower="-1.57" upper="1.57" effort="100.0" velocity="2.0" acceleration="10.0" deceleration="5.0" jerk="50.0"/>
  </joint>

  <!-- ... -->
</robot>
```

见 [ros/urdfdom#235](https://github.com/ros/urdfdom/pull/235), [ros/urdfdom#238](https://github.com/ros/urdfdom/pull/238),以及 [ros/urdfdom#212](https://github.com/ros/urdfdom/pull/212) 为了更多的信息。

注意机器人模型插件 [尚未支持胶囊几何](https://github.com/ros2/rviz/issues/1734)。请考虑打开此功能的拉动请求 !

<span id="robot-state-publisher-can-read-the-robot-description-from-a-topic"></span>

### `robot_state_publisher` 可以从一个话题读取机器人描述

大部分时间在ROS系统中 `robot_state_publisher` 节点做两件事:

- 它出版《公约》。 `robot_description` 在一个议题上,

- 它公布给定的共同立场的TF转型.

如果您曾尝试过将ROS接口添加到一个框架中, 并使用它自己的内部机器人模型, 您可能希望这是两个独立的工具。 现在可以了 ! `use_robot_description_topic` 参数改为 `true` 将制作 `robot_state_publisher` 订阅日期 `robot_description` 然后,让另一个机器人框架 发布它自己的机器人描述。

见 [ros/robot_state_publisher#234](https://github.com/ros/robot_state_publisher/pull/234) 为了更多的信息。

<span id="resource-retriever-service"></span>

### 资源检索服务

说您正在调试现场的机器人。 您打开笔记本电脑上的 RViz , 但您没有安装正确的机器人描述版本。 在 ROS Kilted 中, RViz 加入了使用 ROS 服务在网络上加载 meshes 的能力 。 `/rviz/get_resource`但是,这种能力仅限于RViz。 `resource_retriever_service` 这样任何节点都可以在网络上加载 meshes.

``` c++
resource_retriever::RetrieverVec plugins = resource_retriever::default_plugins();
// Create a RosServiceResourceRetriever plugin
plugins.push_back(std::make_shared<RosServiceResourceRetriever>(*node));
// Give that plugin to your Retriever instance
resource_retriever::Retriever retriever(plugins);
```

见 [ros2/rviz#1698](https://github.com/ros2/rviz/pull/1698) 更多细节。

<span id="call-ament-python-install-package-multiple-times"></span>

### 调用 `ament_python_install_package` 多次数

现在可以调用软件包 `ament_python_install_package()` 多次使用相同的 Python 软件包名称。这允许您使用 `rosidl_generate_interfaces()` 财务报告和财务报告 `ament_python_install_package()` 将生成的消息和代码放入相同的 Python 软件包。

虽然您可以在同一软件包中包含代码和信件定义,但最佳做法是将信件定义放入自己的软件包中。这让其他人只依赖信件,因为他们可能不需要代码或其依赖性。

见 [ament/ament_cmake#587](https://github.com/ament/ament_cmake/pull/587) 为了更多的信息。

<span id="new-cmake-target-ament-cmake-ros-core-ament-ros-defaults"></span>

### 新建 CMake 目标 : `ament_cmake_ros_core::ament_ros_defaults`

厌倦在不同的分支上指定不同的 C 和 C++ 版本 ? 让新的 CMake 目标 `ament_cmake_ros_core::ament_ros_defaults` 为您设定。此目标使用 [target_compile_features](https://cmake.org/cmake/help/v3.20/command/target_compile_features.html) 以指定 C 和 C++ 版本要求。

``` cmake
find_package(ament_cmake_ros REQUIRED)
target_link_libraries(my_library PUBLIC ament_cmake_ros_core::ament_ros_defaults)
```

见 [ros2/ament_cmake_ros#62](https://github.com/ros2/ament_cmake_ros/pull/62) 为了更多的信息。

<span id="new-thread-naming-utilities"></span>

### 新建线程命名公用事业

调试多条脚本问题 ? 使用两个新的公用设施 。 `rcpputils` 来获取和设置线程名称。这样可以更容易地识别调试器中的线程,例如 `gdb`.

``` c++
#include <iostream>
#include <rcpputils/thread_name.hpp>

int main() {
    rcpputils::set_thread_name("map_thread");
    std::cout << rcpputils::get_thread_name() << "\n";
}
```

见 [ros2/rcpputils#213](https://github.com/ros2/rcpputils/pull/213) 更多细节。

<span id="new-rcutils-apis"></span>

### 新设 `rcutils` APIs 辅助程序

那个... `rcutils` 软件包中包含一些新的公用设施。如果您的平台缺乏 `strnlen`,您现在可以使用 `rcutils_strnlen` 换句话说。

``` c
#include <stdio.h>

#include <rcutils/strnlen.h>

int main() {
    const char *str = "Hello world";
    size_t len = rcutils_strnlen(str, 100);
    printf("%zu\n", len);
    return 0;
}
```

需要编码或解码基础64数据吗?尝试新数据 `rcutils_encode_base64` 财务报告和财务报告 `rcutils_decode_base64` 函数。

``` c
#include <assert.h>
#include <stdio.h>
#include <string.h>

#include <rcutils/allocator.h>
#include <rcutils/base64.h>
#include <rcutils/types/uint8_array.h>

int main() {
    rcutils_allocator_t allocator = rcutils_get_default_allocator();

    rcutils_uint8_array_t input = rcutils_get_zero_initialized_uint8_array();
    assert(rcutils_uint8_array_init(&input, 11, &allocator) == RCUTILS_RET_OK);
    memcpy(input.buffer, "Hello World", 11);
    input.buffer_length = 11;

    char *encoded = NULL;
    assert(rcutils_encode_base64(&input, &encoded, &allocator) == RCUTILS_RET_OK);
    printf("%s\n", encoded);

    rcutils_uint8_array_t decoded = rcutils_get_zero_initialized_uint8_array();
    assert(rcutils_decode_base64(encoded, &decoded, &allocator) == RCUTILS_RET_OK);
    printf("%.*s\n", (int)decoded.buffer_length, (char *)decoded.buffer);

    allocator.deallocate(encoded, allocator.state);
    assert(rcutils_uint8_array_fini(&input) == RCUTILS_RET_OK);
    assert(rcutils_uint8_array_fini(&decoded) == RCUTILS_RET_OK);
    return 0;
}
```

见 [ros2/rcutils#430](https://github.com/ros2/rcutils/pull/430) 财务报告和财务报告 [ros2/rcutils#533](https://github.com/ros2/rcutils/pull/533) 为了更多的信息。

<span id="new-rcl-apis"></span>

### 新设 `rcl` APIs 辅助程序

如果你维持一个ROS客户端库,你可能会对这些新的程序感兴趣 `rcl` APIs : (英语).

<span id="rcl-lifecycle-get-transition-label-by-id"></span>

#### `rcl_lifecycle_get_transition_label_by_id`

获取寿命周期过渡 ID 的可读字符串标签。 使用此标签记录、 调试或显示状态过渡, 而无需手动将 ID 映射到字符串 。

<span id="rcl-subscription-is-cft-supported"></span>

#### `rcl_subscription_is_cft_supported`

检查订阅是否支持内置中间软件上的内容过滤主题( CFT) 。 在应用或配置信件内容过滤器之前, 安全地验证过滤支持 。

<span id="rcl-action-count-clients"></span>

#### `rcl_action_count_clients`

查询 ROS 图表以计算特定动作名称的主动动作客户端。 动作服务器可以在花费资源之前验证客户端的存在, 或者工具可以检查图表状态 。

<span id="rcl-action-count-servers"></span>

#### `rcl_action_count_servers`

查询 ROS 图表以计算一个特定动作名称的主动动作服务器。 动作客户端可以在发送目标请求之前确认服务器已经在线 。

<span id="rcl-timer-exchange-callback-data"></span>

#### `rcl_timer_exchange_callback_data`

执行时将用户数据指针更新为计时器调回。 动态互换调回上下文或状态, 而无需重新创建活动计时器实例 。

<span id="rcl-action-server-set-expired-event-callback"></span>

#### `rcl_action_server_set_expired_event_callback`

注册自定义事件召回, 当动作服务器目标过期定时器起火时触发。 启用基于事件的执行模式来同步处理已过期目标的清理程序或通知 。

更多信息见这些拉动请求:

- [ros2/rcl#1229](https://github.com/ros2/rcl/pull/1229)

- [ros2/rcl#1257](https://github.com/ros2/rcl/pull/1257)

- [ros2/rcl#1293](https://github.com/ros2/rcl/pull/1293)

- [ros2/rcl#1294](https://github.com/ros2/rcl/pull/1294)

- [ros2/rcl#1295](https://github.com/ros2/rcl/pull/1295)

<span id="pass-constructor-arguments-to-plugins-using-class-loader"></span>

### 将构造器参数传递给使用插件的插件 `class_loader`

您现在可以将参数传递给使用 `class_loader`。这可以消除插件的 API 中初始化方法的必要性。您只需要专门化 `class_loader::InterfaceTraits<>` 在您的插件基础类中。

``` c++
class MyPluginWithConstructor
{
public:
  // constructor parameters for the base class do not need to match the derived classes
  explicit MyPluginWithConstructor(std::string) {}
  virtual ~MyPluginWithConstructor() = default;

  virtual int some_api() = 0;
};

template<>
struct class_loader::InterfaceTraits<MyPluginWithConstructor>
{
  // Define constructor arguments that you must pass to instantiate a plugin
  using constructor_parameters = class_loader::ConstructorParameters<std::string,
      std::unique_ptr<int>>;
};
```

更多信息见 [ros/class_loader#223](https://github.com/ros/class_loader/pull/223).

<span id="runtime-tracing-opt-out-mechanism"></span>

### 运行时间跟踪选择退出机制

[从ROS 2中删除内置的追查仪器](https://github.com/ros2/ros2_tracing/blob/lyrical/README.md#removing-the-instrumentation) 或 时 间 [将追踪点排除在仪器之外](https://github.com/ros2/ros2_tracing/blob/lyrical/README.md#excluding-tracepoints) 。这在 Linux 二进制中都是默认启用的。

为避免在运行时装入追踪器(从而禁用所有仪器),设置 `TRACETOOLS_RUNTIME_DISABLE` 环境变量为 `1`:

``` console
$ export TRACETOOLS_RUNTIME_DISABLE=1
$ ros2 run tracetools status
Tracing disabled
```

见 [ros2/ros2_tracing#185](https://github.com/ros2/ros2_tracing/pull/185) 为了更多的信息。

<span id="long-term-tracing-improvements"></span>

### 长期追踪改进

<span id="snapshot-mode-tracing"></span>

#### 快照模式跟踪

默认情况下, 跟踪会话会连续将跟踪数据写入磁盘。 使用 LTTng 的跟踪会话 [抓图模式](https://lttng.org/docs/v2.13/#doc-tracing-session-mode) 存储内存中的微量数据, 仅在磁盘中写入 [拍摄快照](https://lttng.org/docs/v2.13/#doc-taking-a-snapshot)。当内存缓冲器被填充时,最古老的数据会被丢弃,保持一个滚动历史,其大小可以通过配置子缓冲器大小来控制。这种“飞行记录器”模式只有在发生有趣的情况时才有用,可以捕捉到微量数据,避免连续的磁盘写入,从而降低运行时间的性能影响。

[快照模式跟踪](https://github.com/ros2/ros2_tracing/tree/lyrical#tracing-in-snapshot-mode) 备查 `ros2_tracing` 通过 [ros2 跟踪命令](https://github.com/ros2/ros2_tracing/tree/lyrical#trace-command-1) 和《京都议定书》 [追踪发射文件动作](https://github.com/ros2/ros2_tracing/tree/lyrical#launch-file-trace-action-1).

见 [ros2/ros2_tracing#195](https://github.com/ros2/ros2_tracing/pull/195) 财务报告和财务报告 [ros2/ros2_tracing#206](https://github.com/ros2/ros2_tracing/pull/206) 为了更多的信息。

<span id="dual-session-tracing"></span>

#### 双会话跟踪

[双会话模式](https://github.com/ros2/ros2_tracing/tree/lyrical#dual-session-tracing) 通过使用两个独立的跟踪会话解决了丢失初始化痕量数据的问题:一个用于在快照模式下初始化事件,另一个用于运行时事件的普通跟踪会话。这允许在任何时间开始积极记录痕量数据而不丢失初始化数据。

使用该 `Trace` 动作与 `dual_session=True` 以快照模式启动初始化数据会话。然后使用微量命令 `--dual-session` 选项,以获取初始化会话的快照并启动运行时会话。

见 [ros2/ros2_tracing#191](https://github.com/ros2/ros2_tracing/pull/191) 财务报告和财务报告 [ros2/ros2_tracing#196](https://github.com/ros2/ros2_tracing/pull/196) 为了更多的信息。
