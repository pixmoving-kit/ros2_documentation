---
translation_status: machine_translated
source: Releases/Release-Kilted-Kaiju.rst
---

<span id="kilted-kaiju-codename-kilted-may-2025"></span> <span id="kilted-release"></span>

# Kilted Kaiju（代号 kilted；2025 年 5 月）

*Kilted Kaiju* 以下是Kilted Kaiju自上次发布以来的重要变化和特征。 [长窗体变化日志](Kilted-Kaiju-Complete-Changelog.md)

<span id="supported-platforms"></span>

## 支持的平台

Kilted Kaiju 支持以下平台: [平台支持级别](../The-ROS2-Project/Platform-Support-Tiers.md):

第一级平台:

- 乌班图24.04(诺布尔语: `amd64` 财务报告和财务报告 `arm64`

- Windows 10 (Visual Studio 2019): (英语). `amd64`

第二级平台:

- 莱尔9: `amd64`

第三级平台:

- 马科斯: `amd64`

- 底栖书虫 : `amd64`

目标平台:

| 建筑 | 乌本图·诺贝尔(24.04) | Windows 10 (VS2019) (英语). | 第9条 | macOS | 德比亚书虫(12) | OpenEmbed / Yocto 项目 |
|----|----|----|----|----|----|----|
| amd64 (中文(简体) ). | 第1级 \[d\]\[a\]\[s\] | 第1级 \[s\] | 第二级\[d\]\[a\]\[s\] | 第3级 \[s\] | 第3级 \[s\] | 第3级 \[s\] |
| 军火64 | 第1级 \[d\]\[a\]\[s\] |  |  |  | 第3级 \[s\] | 第3级 \[s\] |
| 臂弹32 | 第3级 \[s\] |  |  |  | 第3级 \[s\] | 第3级 \[s\] |

以下指标显示每个平台都有何种交付机制。

"\[d\]" 发行专用(Debian,RPM等)包将提供给本平台,用于提交rosdistro的包.

" \[a\] " 二进制版本作为每个平台的单一档案提供,包含Jazzy ROS 2 repos文件中的所有软件包\[^14\].

" \[s\] " 来源汇编。

中间软件执行支持 :

| 中间软件库 | 中件提供者 | 支助级别 | 平台 | 建筑 |
|----|----|----|----|----|
| rmw_fastrtps_cpp\* | eProsima Fast-DDS 软件 | 第1级 | 所有平台 | 所有建筑 |
| rmw_connextdds | RTI 连接 | 第1级 | Ubuntu, Windows, 和 macOS 软件 | 除arm64外的所有建筑 |
| rmw_cyclonedds_cpp | Eclipse Cyclone DDS | 第1级 | 所有平台 | 所有建筑 |
| rmw_zenoh_cpp | Eclipse Zenoh (英语). | 第1级 | 所有平台 | 所有建筑 |
| rmw_fastrtps_dynamic_cpp | eProsima Fast-DDS 软件 | 第二级 | 所有平台 | 所有建筑 |
| rmw_gurumdds_cpp | GurumNetworks GurumDDS | 第3级 | Ubuntu 和 视窗 | 除arm32外的所有建筑 |

" \* " 是指未执行的《保护所有移徙工人及其家庭成员权利国际公约》。

Middleware 执行支持依赖于平台支持级别。例如,在 Tier 2 平台上执行的 Lier 1 中间软件只能得到 Tier 2 支持。

最低语文要求:

- C++17

- ⁇  3.9

依赖性要求:

<table class="docutils align-default">
<thead>
<tr class="row-odd">
<th class="head"></th>
<th colspan="2" class="head"><p>所需支助</p></th>
<th colspan="4" class="head"><p>建议的支助</p></th>
</tr>
</thead>
<tbody>
<tr class="row-even">
<td><p>软件包</p></td>
<td><p>乌邦图诺布尔</p></td>
<td><p>视窗10**</p></td>
<td><p>第9条</p></td>
<td><p>马科斯**</p></td>
<td><p>底栖书虫</p></td>
<td><p>已打开*</p></td>
</tr>
<tr class="row-odd">
<td><p>CMake</p></td>
<td><p>3.28.3</p></td>
<td><p>3.28.3</p></td>
<td><p>3.26.5</p></td>
<td><p>3.31.1</p></td>
<td><p>3.25.1</p></td>
<td><p>3.22.3</p></td>
</tr>
<tr class="row-even">
<td><p>爱咪</p></td>
<td><p>3.3.4</p></td>
<td><p>3.3.4</p></td>
<td><p>3.3.4a</p></td>
<td colspan="3"><p>3.3.4</p></td>
</tr>
<tr class="row-odd">
<td><p>Gazebo</p></td>
<td><p>离子体*</p></td>
<td><p>N/A</p></td>
<td><p>N/A</p></td>
<td><p>离子体*</p></td>
<td><p>离子体*</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>数字</p></td>
<td><p>1.26.4</p></td>
<td><p>1.26.4</p></td>
<td><p>1.20.1</p></td>
<td><p>2.1.3</p></td>
<td><p>1.24.2</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-odd">
<td><p>食人鱼</p></td>
<td colspan="5"><p>1.12.10</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>打开CV</p></td>
<td><p>4.6.0</p></td>
<td><p>4.9.0</p></td>
<td><p>4.6.0</p></td>
<td><p>4.10.0</p></td>
<td><p>4.6.0</p></td>
<td><p>4.1.0 / 3.2.0***</p></td>
</tr>
<tr class="row-odd">
<td><p>打开SSL</p></td>
<td><p>3.0.13</p></td>
<td><p>3.3.2</p></td>
<td><p>3.2.2</p></td>
<td><p>1.1.1w</p></td>
<td><p>3.0.15</p></td>
<td><p>1.1.1d / 1.1.1b***</p></td>
</tr>
<tr class="row-even">
<td><p>Python</p></td>
<td><p>3.12.3</p></td>
<td><p>3.12.3</p></td>
<td><p>3.9.19</p></td>
<td><p>3.13.0</p></td>
<td><p>3.11.2</p></td>
<td><p>3.8.2 / 3.7.5***</p></td>
</tr>
<tr class="row-odd">
<td><p>Qt 键</p></td>
<td><p>5.15.13</p></td>
<td><p>5.15.8</p></td>
<td><p>5.15.9</p></td>
<td><p>5.15.16</p></td>
<td><p>5.15.8</p></td>
<td><p>5.14.1 / 5.12.5***</p></td>
</tr>
<tr class="row-even">
<td colspan="2"></td>
<td colspan="5"><p><strong>仅限 Linux</strong></p></td>
</tr>
<tr class="row-odd">
<td><p>个人计算机L</p></td>
<td><p>1.14.0</p></td>
<td><p>N/A</p></td>
<td><p>1.12.0</p></td>
<td><p>1.14.1</p></td>
<td><p>1.13.0</p></td>
<td><p>1.10.0</p></td>
</tr>
<tr class="row-even">
<td colspan="7"><p><strong>RMW 中间软件</strong></p></td>
</tr>
<tr class="row-odd">
<td colspan="5"><p>紧接DDS = 7.3.0.0</p></td>
<td colspan="2"><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>Cyclone DDS</p></td>
<td colspan="6"><p>0.10.5</p></td>
</tr>
<tr class="row-odd">
<td><p>快速数据交换系统</p></td>
<td colspan="6"><p>2.14.4</p></td>
</tr>
<tr class="row-even">
<td><p>Gurum 数据交换系统</p></td>
<td colspan="2"><p>4.2.0</p></td>
<td colspan="4"><p>N/A</p></td>
</tr>
<tr class="row-odd">
<td><p>Zenoh</p></td>
<td colspan="6"><p>1.0.4</p></td>
</tr>
</tbody>
</table>

" \* " 是指这不是上游版本(可在官方操作系统寄存器上查阅),而是OSRF或社区分发的包(自定义寄存器上建立和分发的包)。

" \*\*"是指依赖可能看到多个版本的改变,因为依赖使用一个包管理器,在没有稳定的API的情况下不断更新依赖.

" \*\*\* " WebOS OSE提供了这个不同的版本.

此文档只抓取 ROS 发行的首次发布时的版本, 并且不会随着依赖关系向前移动而更新, 因此这些版本是低水印 。

依赖软件包管理器的使用 :

- Ubuntu, Debian: apt, pip) 维基语录链接:名人名言 - 文学作品 - 谚语 - 谚语 - 谚语

- 视窗: pixi/conda, pip

- 马科斯: 土生土长,皮普

- (原始内容存档于2019-09-31) (英语). RHEL: dnf

- 打开嵌入式: opkg

构建系统支持 :

- ament_cmake

- 货物

- 制作( C)

- 设置工具

<span id="installation"></span>

## 安装

[安装 Kilted Kaiju 设备](https://docs.ros.org/en/kilted/Installation.html)

<span id="supported-gazebo-release"></span>

## 支持的 Gazebo 发布

对于Kilted Kaiju来说,建议释放的Gazebo是 [离子](https://gazebosim.org/docs/ionic/ros_installation).

<span id="new-features-in-this-ros-2-release"></span>

## 本 ROS 2 发布中的新功能

<span id="ament-cmake-ros"></span>

### `ament_cmake_ros`

<span id="add-rmw-test-fixture-for-supporting-rmw-isolated-testing"></span>

#### 添加 rmw\_ test_fixture 用于支持 RTW- 同位化测试

包括两个新的软件包,为创建基于RTW的通信隔离的测试固定装置提供了可扩展的机制。它与 Rmw 和 rmw\_ 执行 API 紧密相仿。

那个... `rmw_test_fixture` 软件包目前只提供 API, 由 RTW 提供商执行, 用于配置他们的 RTW 进行测试。

那个... `rmw_test_fixture_implementation` 包提供了发现、加载和引用适当扩展的切入点。

见 <https://github.com/ros2/ament_cmake_ros/pull/21> 更多细节。

<span id="common-interfaces"></span>

### `common_interfaces`

<span id="new-nav-msgs-goals-message"></span>

#### 新建 Nav\_ msgs/目标消息

一个新消息类型, [nav_msgs/msg/Goals](https://docs.ros.org/en/rolling/p/nav_msgs/msg/Goals.html),用于支持在nav_msgs软件包内的一系列导航目标。

见 <https://github.com/ros2/common_interfaces/pull/269> 更多细节。

<span id="ros2cli"></span>

### `ros2cli`

<span id="action-introspection"></span>

#### 动作回顾

这样可以对命令行的动作进行回顾。使用 `ros2cli` 工具 : `ros2 action echo <action name>`.

见 <https://github.com/ros2/ros2cli/pull/978> 以获取更多信息。

<span id="rclcpp"></span>

### `rclcpp`

<span id="action-generic-client"></span>

#### 动作通用客户端

支持动作通用客户端,用于支持rosbag2中的动作.

见 <https://github.com/ros2/rclcpp/pull/2759> 更多细节。

<span id="rclpy"></span>

### `rclpy`

<span id="static-type-checking"></span>

#### 静态类型检查

添加静态类型提示到 `ActionClient` 财务报告和财务报告 `ActionServer`.

见 <https://github.com/ros2/rclpy/pull/1349> 更多细节。

添加对 [通用软件](https://typing.python.org/en/latest/reference/generics.html) 输入 `pub/sub/client/server/actions`, `Future/Task`,以及 `Parameter`.

`Publisher`, `Subscription`, `Server`, `Task`,以及 `Parameter` 不需要更新以添加对通用软件的支持。

`Client` 需要更新以类似于以下内容,以便进行改进型号检查。

``` python
self._get_parameter_client: Client[GetParameters.Request,
                                   GetParameters.Response] = self.node.create_client(
                                    GetParameters, '/get_parameters',
                                    qos_profile=qos_profile, callback_group=callback_group)
```

`ActionClient` 需要更新以类似于以下内容,以便进行改进型号检查。

``` python
ac: ActionClient[Fibonacci.Goal,
                 Fibonacci.Result,
                 Fibonacci.Feedback] = ActionClient(self.node, Fibonacci, 'fibonacci')
```

`Future` 需要更新以类似于以下内容,以便进行改进型号检查。

``` python
log_msgs_future: Future[bool] = Future()
```

见 <https://github.com/ros2/rclpy/pull/1239>, <https://github.com/ros2/rclpy/pull/1275>, <https://github.com/ros2/rclpy/pull/1246>,以及 <https://github.com/ros2/rclpy/pull/1254/files> 更多细节。

此外,在所有这些方面,还作了各种其他小的改进和纠正。 `rclpy`.

使用 Python 类型可静态检查 [ament_mypy](https://github.com/ament/ament_lint/tree/kilted/ament_mypy) 关闭 [神秘点](https://www.mypy-lang.org/).

<span id="eventsexecutor"></span>

#### 事件执行器

支持实验事件执行者 `rclpy`,这是原始端口 `rclcpp` 事件执行器概念。

见 <https://github.com/ros2/rclpy/pull/1391> 更多细节。

<span id="rosbag2"></span>

### `Rosbag2`

<span id="action-introspection-rosbag2-support"></span>

#### 行动回顾 Rosbag2 支持

允许录制和播放一个罗素包的动作.

见 <https://github.com/ros2/rosbag2/pull/1955> 更多信息。设计文档 <https://github.com/ros2/rosbag2/pull/1928>.

<span id="progress-bar-for-ros2-bag-play"></span>

#### 进度栏 `ros2 bag play`

添加进度栏 `ros2 bag play` CLI,显示包的时间和持续时间,类似于ROS 1中看到的.

见 <https://github.com/ros2/rosbag2/pull/1836> 更多细节。

<span id="added-support-for-replaying-multiple-bags-with-ros2-bag-play-cli"></span>

#### 添加了用于重放多个袋的支援 `ros2 bag play` 国 际

要重放多个包, 请使用新包 `-i, --input` CLI 选项 :

``` console
$ ros2 bag play -i bag1 -i bag2 -i bag3 [storage_id]
```

见 <https://github.com/ros2/rosbag2/pull/1848> 以获取更多信息。

<span id="added-support-for-replaying-messages-chronologically-based-on-their-publication-timestamp"></span>

#### 根据消息的发布时间戳按时间顺序重新播放消息的添加支持

这个被曝光了 `ros2 bag play` 带有新的 `--message-order {received,sent}` 选项。默认行为是按收到消息的顺序播放消息。

见 <https://github.com/ros2/rosbag2/pull/1876> 以获取更多信息。

<span id="make-snapshot-writing-into-a-new-file-each-time-it-is-triggered"></span>

#### 每次触发时将快照写入新文件

见 <https://github.com/ros2/rosbag2/pull/1842> 更多细节。

<span id="new-sort-cli-option-in-the-ros2-bag-info-command"></span>

#### 新设 `--sort` CLI 选项 `ros2 bag info` 命令

有了新东西 `--sort` CLI 选项用户将能够按名称,主题类型或录制消息的数量排序主题,服务和动作.

见 <https://github.com/ros2/rosbag2/pull/1804> 更多细节。

<span id="show-size-contribution-of-each-topic-with-ros2-bag-info"></span>

#### 显示每个主题的大小贡献 `ros2 bag info`

有了新东西 `--size-contribution` 选项与 `ros2 bag info -v` 用户可以在包文件中看到每个话题的大小贡献。

见 <https://github.com/ros2/rosbag2/pull/1726> 以获取更多信息。

<span id="added-log-level-option-to-ros2-bag-play-and-ros2-bag-record-to-allow-printing-debug-messages"></span>

#### 已添加 `--log-level` 选项到 `ros2 bag play` 财务报告和财务报告 `ros2 bag record` 以允许打印调试消息

见 <https://github.com/ros2/rosbag2/pull/1625> 更多细节。

<span id="rosidl-rust"></span>

### `rosidl_rust`

<span id="added-rosidl-rust"></span>

#### 已添加 `rosidl_rust`

默认代码生成器列表中增加了一个 Rust idl 生成器.

见 <https://github.com/ros2/ros2/pull/1674> 更多细节。

<span id="ros2"></span>

### `ros2`

<span id="switch-to-using-pixi-conda-for-windows"></span>

#### 切换为 Windows 使用 Pixi/ Conda

这可以方便地管理依赖,并在未来更新它们。 安装过程被大大简化。 相对于安装依赖的数十个步骤, 它只是几个命令。 更新依赖性要容易得多。 依赖性是在单个工作空间安装的, 没有“ 全球” 安装 。

见 <https://github.com/ros2/ci/pull/802> 财务报告和财务报告 <https://github.com/ros2/ros2/pull/1642> 详细情况。访问 [Windows 来源安装指令](../Installation/Alternatives/Windows-Development-Setup.md) 以安装在 Windows 上。

<span id="support-topic-instances-in-dds-topics"></span>

#### 支持 DDS 专题中的专题实例

主题实例是将数个相同逻辑类型的对象的更新传输到同一资源,即主题的倍增方式.

见 <https://github.com/ros2/ros2/issues/1538> 更多信息。您也可以检查文档 : <https://github.com/ros2/design/pull/340/files>.

<span id="changes-since-the-jazzy-release"></span>

## 自Jazzy发布以来的变化

<span id="id1"></span>

### `common_interfaces`

<span id="added-nv12-to-pixel-formats"></span>

#### 添加到像素格式的 NV12

将NV12添加到像素格式中,这是硬件加速解码器的一种常见输出格式.

见 <https://github.com/ros2/common_interfaces/pull/253> 更多细节。

<span id="id2"></span>

### `rclcpp`

<span id="consistent-behavior-for-subordinate-nodes"></span>

#### 下级节点的一致行为

下级节点的不一致行为被固定. 下级节点是一个与主节点相连的二级节点,在保持单独的名称和命名空间的同时,共享相同的内在上下文和资源. 行为修改可能会影响依赖前一次执行的现有应用程序:

1.  从下级节点创建的普通客户端现在正确尊重下级节点的子名称空间

2.  使用下级节点获得的参数现在正确使用(父母)节点 `rclcpp::node_interfaces::NodeParametersInterface`

见 <https://github.com/ros2/rclcpp/pull/2822> 更多细节。

<span id="rmw-connextdds-cpp"></span>

### `rmw_connextdds_cpp`

<span id="version-bumped-to-7-3"></span>

#### 版本为7.3

RTI Connext DDS版本被撞到7.3.0.

见 <https://github.com/ros2/ci/pull/811> 更多细节。

<span id="connextmicro"></span>

### `Connextmicro`

<span id="deprecated-connextmicro"></span>

#### 已贬值的 Connexmiro

RTI Connext 微微RMW软件包, `rmw_connextddsmicro`在Kilted Kaiju中不再接受更新, 并在未来的ROS 2版中被删除。

见 <https://github.com/ros2/rmw_connextdds/pull/182> 以获取更多信息。

<span id="rosidl-dynamic-typesupport"></span>

### `rosidl_dynamic_typesupport`

<span id="removing-support-for-float128"></span>

#### 删除对浮点128的支持

删除对浮128的支持,因为定义中存在不一致之处.

见 <https://github.com/ros2/rosidl_dynamic_typesupport/issues/11> 更多细节。

<span id="rmw-fastrtps-cpp"></span>

### `rmw_fastrtps_cpp`

<span id="renaming-package-from-fastrtps-to-fastdds"></span>

#### 从快件到快件的重命名软件包

`fastrtps` 改名为 `fastdds`。rmw 执行的名称保持不变。XML Profile ENV字符串将更改。

见 <https://github.com/ros2/ros2/pull/1641> 更多细节。

<span id="ament-target-dependencies-is-deprecated"></span>

### ament_object_dependents 已贬值

CMake 宏 `ament_target_dependencies()` 已贬值,以利 `target_link_libraries()` 和现代的 CMake 目标。 宏仍然有效, 但它在构建时发出 CMake 贬值警告 :

``` default
CMake Deprecation Warning at [...]/ament_cmake_target_dependencies/share/ament_cmake_target_dependencies/cmake/ament_target_dependencies.cmake:89 (message):
ament_target_dependencies() is deprecated.  Use target_link_libraries()
with modern CMake targets instead.  Try replacing this call with:

    target_link_libraries([...] PUBLIC
    [...]
    )
```

尝试替换 `ament_target_dependencies()` 与电话 `target_link_libraries()` 警告中建议的电话。

更多信息见 [ament/ament_cmake#572](https://github.com/ament/ament_cmake/pull/572) 财务报告和财务报告 [ament/ament_cmake#292](https://github.com/ament/ament_cmake/issues/292).

<span id="launch"></span>

### `launch`

<span id="pathjoinsubstitution"></span>

#### `PathJoinSubstitution`

`PathJoinSubstitution` 现在支持将字符串或替换为单一路径组件。例如:

``` python
PathJoinSubstitution(['robot_description', 'urdf', [LaunchConfiguration('model'), '.xacro']])
```

如果说 `model` 发射配置设定为 `my_model`,这将导致一条与下列内容等同的道路:

``` python
'robot_description/urdf/my_model.xacro'
```

更多信息见: [ros2/launch#835](https://github.com/ros2/launch/issues/835) 财务报告和财务报告 [ros2/launch#838](https://github.com/ros2/launch/pull/838).

<span id="rmw-zenoh-cpp"></span>

### `rmw_zenoh_cpp`

<span id="tier-1"></span>

#### `Tier 1`

那个... `rmw_zenoh_cpp` 现在被认为是第1级,有多种PR(摘要如下: [ros2/rmw_zenoh#265](https://github.com/ros2/rmw_zenoh/issues/265))在ROS 2核心包中,例如:

> - 让rmw通过所有核心测试.
>
> - 执行和文件安全
>
> - 让它在一级平台工作。
>
> - 添加的质量申报
>
> - 添加到2005年REP中
>
> - 一份全心全意的夜线工作
>
> - 其它情况

更多信息见 <https://github.com/ros2/rmw_zenoh/issues/265>.

<span id="development-progress"></span>

## 发展进度

关于基尔特莱德开珠开发的进展情况,参见: [这个项目的董事会](https://github.com/orgs/ros2/projects/63).

关于Kilted Kaiju所遵循的广泛进程,见 [进程描述页面](Release-Process.md).

<span id="release-timeline"></span>

## 发布时间线

> 2024年12月 - 平台决定.  
> 《2000年区域环境方案》与目标平台和主要依赖性版本一起更新。
>
> 2025年4月7日 - Alpha + RMW 冻结  
> ROS基地的初步测试和稳定 <span id="id3"></span>[\[1\]](#id8) 软件包,以及RAMW供应商软件包的 API 和特性冻结。
>
> 2025年4月14日-冻结  
> ROS Base 的 API 和特性冻结 <span id="id4"></span>[\[1\]](#id8) 在 Rolling Ridley 中的软件包。在此点之后,只应该发布错误修正。新软件包可以独立发布。
>
> 2025年4月21日-分部  
> 罗林瑞德利的分店 `rosdistro` 重新开放用于 ROS 基地的 滚动PRs 。 <span id="id5"></span>[\[1\]](#id8) 软件包。 `ros-rolling-*` 软件包到 `ros-kilted-*` 软件包。
>
> (原始内容存档于2025年4月28日) (中文(简体) ). Mon. 2025 - Beta.  
> ROS 桌面更新版 <span id="id6"></span>[\[2\]](#id9) 可用软件包。请进行一般测试。
>
> Thu, 2025年5月1日 - 踢开教习党  
> 教师: <https://github.com/ros2/kilted_tutorial_party> 开放给社区测试。
>
> 2025年5月12日- 释放候选人  
> 创建了候选软件包。 ROS 桌面的更新版 <span id="id7"></span>[\[2\]](#id9) 可用软件包。
>
> 2025年5月19日-冻结  
> 全部冻结 Kilted 树枝 [ROS 2 桌面软件包](https://reps.openrobotics.org/rep-2001/#kilted-kaiju-may-2025-november-2026) 财务报告和财务报告 `rosdistro`。没有拉动请求。 `kilted` 分支或目标 `kilted/distribution.yaml` 输入 `rosdistro` Repo将合并.
>
> Fri. 2025年5月23日 - 一般可用性  
> 发布公告. [ROS 2 桌面软件包](https://reps.openrobotics.org/rep-2001/#kilted-kaiju-may-2025-november-2026) 源冻结解除, `rosdistro` 为 Kilted 拉动请求重新打开 。

<span id="id8"></span>

\[1\] ([1](#id3),[2](#id4),[3](#id5))

那个... `ros_base` 变体描述于 [REP 2001(跨基)](https://reps.openrobotics.org/rep-2001/#ros-base).

<span id="id9"></span>

\[2\] ([1](#id6),[2](#id7))

那个... `desktop` 变体描述于 [REP 2001(桌面变量)](https://reps.openrobotics.org/rep-2001/#desktop-variants).
