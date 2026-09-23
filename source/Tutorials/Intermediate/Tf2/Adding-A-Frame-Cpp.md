---
translation_status: machine_translated
source: Tutorials/Intermediate/Tf2/Adding-A-Frame-Cpp.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="adding-a-frame-c"></span>

# 添加坐标系（C++）

**目标：** 学习如何在 tf2 中添加一个额外的框架.

**教程级别：** 中级

**用时：** 15分钟

<span id="background"></span>

## 背景

在之前的教程中,我们通过写一个 [tf2 广播机](Writing-A-Tf2-Broadcaster-Cpp.md) 备注a [tf2 收听器](Writing-A-Tf2-Listener-Cpp.md)。此教程将教你如何在变换树上添加额外的固定和动态框架。事实上,在 tf2 中添加一个框架与创建 tf2 广播机非常相似,但这个例子将显示 tf2 的一些附加功能.

对于许多与变换有关的任务,比较容易在局部帧内思考. 例如,在激光扫描仪中心一个帧内进行激光扫描测量是最容易解释的. tf2允许您定义每个传感器,链接,或连接在您的系统中的局部帧. tf2在从一个帧转换到另一个帧时,会照顾所有引入的隐藏中间帧变换.

<span id="tf2-tree"></span>

## tf2 树

tf2 构建了框架的树状结构,因此不允许在框架结构中有一个闭环。这意味着框架只有一个单亲,但可以有多个孩子。目前,我们的 tf2 树包含三个框架: `world`, `turtle1` 财务报告和财务报告 `turtle2`。两只龟框是... `world` 框。如果我们想要在 tf2 中添加一个新的框,那么现有的三个框之一就需要是父框,而新的框将成为其子框。

![](images/turtlesim_frames.png) <span id="tasks"></span>

## 操作步骤

<span id="write-the-fixed-frame-broadcaster"></span>

### 1 写入固定帧播放器

以海龟为例,我们将增加一个新的框架 `carrot1`,这将是孩子 `turtle1`这个框架将成为第二只龟的目标。

让我们首先创建源文件。请到 `learning_tf2_cpp` 我们在以前的教程中创建了软件包。 `src` 目录通过输入以下命令来下载固定帧广播器代码:

##### Linux

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/fixed_frame_tf2_broadcaster.cpp
```

##### macOS

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/fixed_frame_tf2_broadcaster.cpp
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/fixed_frame_tf2_broadcaster.cpp -o fixed_frame_tf2_broadcaster.cpp
```

或于权壳中:

``` console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/fixed_frame_tf2_broadcaster.cpp -o fixed_frame_tf2_broadcaster.cpp
```

现在打开名为 `fixed_frame_tf2_broadcaster.cpp`.

``` C++
#include <chrono>
#include <functional>
#include <memory>

#include "geometry_msgs/msg/transform_stamped.hpp"
#include "rclcpp/rclcpp.hpp"
#include "tf2_ros/transform_broadcaster.h"

using namespace std::chrono_literals;

class FixedFrameBroadcaster : public rclcpp::Node
{
public:
  FixedFrameBroadcaster()
  : Node("fixed_frame_tf2_broadcaster")
  {
    tf_broadcaster_ = std::make_shared<tf2_ros::TransformBroadcaster>(this);
    timer_ = this->create_wall_timer(
      100ms, std::bind(&FixedFrameBroadcaster::broadcast_timer_callback, this));
  }

private:
  void broadcast_timer_callback()
  {
    geometry_msgs::msg::TransformStamped t;

    t.header.stamp = this->get_clock()->now();
    t.header.frame_id = "turtle1";
    t.child_frame_id = "carrot1";
    t.transform.translation.x = 0.0;
    t.transform.translation.y = 2.0;
    t.transform.translation.z = 0.0;
    t.transform.rotation.x = 0.0;
    t.transform.rotation.y = 0.0;
    t.transform.rotation.z = 0.0;
    t.transform.rotation.w = 1.0;

    tf_broadcaster_->sendTransform(t);
  }

rclcpp::TimerBase::SharedPtr timer_;
  std::shared_ptr<tf2_ros::TransformBroadcaster> tf_broadcaster_;
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<FixedFrameBroadcaster>());
  rclcpp::shutdown();
  return 0;
}
```

代码与tf2广播机的教程示例非常相似,唯一的区别在于这里的变换不会随着时间的变化而改变.

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

<span id="examine-the-code"></span>

#### 1.1 审查守则

让我们看看这个代码中的关键线条。在这里,我们从父代码创建了新的转换 `turtle1` 给新孩子的 `carrot1`。该词 `carrot1` 框架以 y 轴表示 `turtle1` 边框。

``` C++
geometry_msgs::msg::TransformStamped t;

t.header.stamp = this->get_clock()->now();
t.header.frame_id = "turtle1";
t.child_frame_id = "carrot1";
t.transform.translation.x = 0.0;
t.transform.translation.y = 2.0;
t.transform.translation.z = 0.0;
```

<span id="cmakelists-txt"></span>

#### 1.2 CMakeLists.txt

导航一个关卡返回 `learning_tf2_cpp` 目录,其中 `CMakeLists.txt` 财务报告和财务报告 `package.xml` 文件已经找到 。

现在打开 `CMakeLists.txt` 添加可执行文件并命名它 `fixed_frame_tf2_broadcaster`.

``` console
add_executable(fixed_frame_tf2_broadcaster src/fixed_frame_tf2_broadcaster.cpp)
ament_target_dependencies(
    fixed_frame_tf2_broadcaster
    geometry_msgs
    rclcpp
    tf2_ros
)
```

最后,添加: `install(TARGETS…)` 第 15 条 `ros2 run` 能找到您的可执行文件 :

``` console
install(TARGETS
    fixed_frame_tf2_broadcaster
    DESTINATION lib/${PROJECT_NAME})
```

<span id="write-the-launch-file"></span>

#### 1.3 编写发射文件

现在让我们为这个例子创建一个启动文件。 使用您的文本编辑器, 创建一个名为新文件 `turtle_tf2_fixed_frame_demo_launch` 带有扩展的 `.py`, `.xml`,或 `.yaml` 输入 `src/learning_tf2_cpp/launch` 目录,并添加以下行:

##### Python

``` python
from launch import LaunchDescription
from launch.actions import IncludeLaunchDescription
from launch.substitutions import PathJoinSubstitution
from launch_ros.actions import Node
from launch_ros.substitutions import FindPackageShare


def generate_launch_description():
    return LaunchDescription([
        IncludeLaunchDescription(
            PathJoinSubstitution([
                FindPackageShare('learning_tf2_cpp'), 'launch', 'turtle_tf2_demo.launch.py'])
        ),
        Node(
            package='learning_tf2_cpp',
            executable='fixed_frame_tf2_broadcaster',
            name='fixed_broadcaster',
        ),
    ])
```

##### XML 数据

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <include file="$(find-pkg-share learning_tf2_cpp)/launch/turtle_tf2_demo_launch.py" />
  <node pkg="learning_tf2_cpp" exec="fixed_frame_tf2_broadcaster" name="fixed_broadcaster" />
</launch>
```

##### 也门

``` yaml
%YAML 1.2
---
launch:
  - include:
      file: "$(find-pkg-share learning_tf2_cpp)/launch/turtle_tf2_demo_launch.xml"
  - node:
      pkg: "learning_tf2_cpp"
      exec: "fixed_frame_tf2_broadcaster"
      name: "fixed_broadcaster"
```

此发射文件导入所需的软件包, 然后创建一个 `demo_nodes` 变量将存储我们在上一个教程的启动文件中创建的节点。

代码的最后一部分会加入我们的固定 `carrot1` 利用我们 `fixed_frame_tf2_broadcaster` 节点。

##### Python

``` python
        Node(
            package='learning_tf2_cpp',
            executable='fixed_frame_tf2_broadcaster',
            name='fixed_broadcaster',
        ),
```

##### XML 数据

``` xml
  <include file="$(find-pkg-share learning_tf2_cpp)/launch/turtle_tf2_demo_launch.py" />
  <node pkg="learning_tf2_cpp" exec="fixed_frame_tf2_broadcaster" name="fixed_broadcaster" />
```

##### 也门

``` yaml
  - node:
      pkg: "learning_tf2_cpp"
      exec: "fixed_frame_tf2_broadcaster"
      name: "fixed_broadcaster"
```

<span id="build"></span>

#### 1.4 建设

运行 `rosdep` 在工作空间的根中检查缺失的依赖性。

##### Linux

``` console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

##### macOS

rosdep 只运行在 Linux 上, 因此您需要安装 `geometry_msgs` 财务报告和财务报告 `turtlesim` 依附关系

##### Windows

rosdep 只运行在 Linux 上, 因此您需要安装 `geometry_msgs` 财务报告和财务报告 `turtlesim` 依附关系

仍然在工作区根部,构建您的软件包:

##### Linux

``` console
$ colcon build --packages-select learning_tf2_cpp
```

##### macOS

``` console
$ colcon build --packages-select learning_tf2_cpp
```

##### Windows

``` console
$ colcon build --merge-install --packages-select learning_tf2_cpp
```

打开新终端, 导航到您工作空间的根, 并源代码设置文件 :

##### Linux

``` console
$ . install/setup.bash
```

##### macOS

``` console
$ . install/setup.bash
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ call install\setup.bat
```

或于权壳中:

``` console
$ .\install\setup.ps1
```

<span id="run"></span>

#### 1.5 运行

现在你可以开始海龟播音员演示:

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_fixed_frame_demo_launch.xml # .py or .yaml are also acceptable
```

你应该注意的是,新的 `carrot1` 框架出现在变换树上。

![](images/turtlesim_frames_carrot.png)

如果您在周围驱动第一只龟,您应该注意到,尽管我们增加了一个新的框架,但行为并没有改变,这是因为添加一个额外的框架不会影响其他框架,而我们的听众仍在使用先前定义的框架。

因此,如果我们想要我们的第二只海龟 跟着胡萝卜而不是第一只海龟, 我们需要改变价值 `target_frame`。这可以有两种方法,一种方法是通过 `target_frame` 参数直接从控制台到发射文件:

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_fixed_frame_demo_launch.xml target_frame:=carrot1 # .py or .yaml are also acceptable
```

第二种方法是更新发射文件。 `turtle_tf2_fixed_frame_demo.launch.py` 文档,并添加 `'target_frame': 'carrot1'` 参数通过 `launch_arguments` 参数。

``` python
def generate_launch_description():
    demo_nodes = IncludeLaunchDescription(
        ...,
        launch_arguments={'target_frame': 'carrot1'}.items(),
        )
```

现在重建软件包,重新启动 `turtle_tf2_fixed_frame_demo.launch.py`将看到第二只海龟跟随胡萝卜,

![](images/carrot_static.png) <span id="write-the-dynamic-frame-broadcaster"></span>

### 2 写入动态帧播放器

我们在此教程中公布的额外框架是一个固定的框架,与父框架相比不会随时间而改变。但是,如果想要发布一个移动的框架,您可以编码广播机,以随时间而改变框架。让我们改变我们 `carrot1` 框架,使其相对于 `turtle1` 框,随时间推移。跳转到 `learning_tf2_cpp` 我们在上一个教程中创建的软件包。 `src` 目录通过输入以下命令来下载动态帧广播器代码:

##### Linux

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/dynamic_frame_tf2_broadcaster.cpp
```

##### macOS

``` console
$ wget https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/dynamic_frame_tf2_broadcaster.cpp
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ curl -sk https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/dynamic_frame_tf2_broadcaster.cpp -o dynamic_frame_tf2_broadcaster.cpp
```

或于权壳中:

``` console
$ curl https://raw.githubusercontent.com/ros/geometry_tutorials/rolling/turtle_tf2_cpp/src/dynamic_frame_tf2_broadcaster.cpp -o dynamic_frame_tf2_broadcaster.cpp
```

现在打开名为 `dynamic_frame_tf2_broadcaster.cpp`:

``` C++
#include <chrono>
#include <functional>
#include <memory>

#include "geometry_msgs/msg/transform_stamped.hpp"
#include "rclcpp/rclcpp.hpp"
#include "tf2_ros/transform_broadcaster.h"

using namespace std::chrono_literals;

const double PI = 3.141592653589793238463;

class DynamicFrameBroadcaster : public rclcpp::Node
{
public:
  DynamicFrameBroadcaster()
  : Node("dynamic_frame_tf2_broadcaster")
  {
    tf_broadcaster_ = std::make_shared<tf2_ros::TransformBroadcaster>(this);
    timer_ = this->create_wall_timer(
      100ms, std::bind(&DynamicFrameBroadcaster::broadcast_timer_callback, this));
  }

private:
  void broadcast_timer_callback()
  {
    rclcpp::Time now = this->get_clock()->now();
    double x = now.seconds() * PI;

    geometry_msgs::msg::TransformStamped t;
    t.header.stamp = now;
    t.header.frame_id = "turtle1";
    t.child_frame_id = "carrot1";
    t.transform.translation.x = 10 * sin(x);
    t.transform.translation.y = 10 * cos(x);
    t.transform.translation.z = 0.0;
    t.transform.rotation.x = 0.0;
    t.transform.rotation.y = 0.0;
    t.transform.rotation.z = 0.0;
    t.transform.rotation.w = 1.0;

    tf_broadcaster_->sendTransform(t);
  }

  rclcpp::TimerBase::SharedPtr timer_;
  std::shared_ptr<tf2_ros::TransformBroadcaster> tf_broadcaster_;
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<DynamicFrameBroadcaster>());
  rclcpp::shutdown();
  return 0;
}
```

<span id="id1"></span>

#### 2.1 审查守则

而不是一个固定的定义 我们的x和y抵消, 我们正在使用 `sin()` 财务报告和财务报告 `cos()` 函数,以抵消当前时间 `carrot1` 正在不断改变。

``` C++
double x = now.seconds() * PI;
...
t.transform.translation.x = 10 * sin(x);
t.transform.translation.y = 10 * cos(x);
```

<span id="id2"></span>

#### 2.2 CMakeLists.txt

导航一个关卡返回 `learning_tf2_cpp` 目录,其中 `CMakeLists.txt` 财务报告和财务报告 `package.xml` 文件已经找到 。

现在打开 `CMakeLists.txt` 添加可执行文件并命名它 `dynamic_frame_tf2_broadcaster`.

``` console
add_executable(dynamic_frame_tf2_broadcaster src/dynamic_frame_tf2_broadcaster.cpp)
ament_target_dependencies(
    dynamic_frame_tf2_broadcaster
    geometry_msgs
    rclcpp
    tf2_ros
)
```

最后,添加: `install(TARGETS…)` 第 15 条 `ros2 run` 能找到您的可执行文件 :

``` console
install(TARGETS
    dynamic_frame_tf2_broadcaster
    DESTINATION lib/${PROJECT_NAME})
```

<span id="id3"></span>

#### 2.3 编写发射文件

要测试此代码, 请创建一个新的发射文件 `turtle_tf2_dynamic_frame_demo_launch` 带有扩展的 `.py`, `.xml`,或 `.yaml` 输入 `src/learning_tf2_cpp/launch` 目录并粘贴以下代码:

##### Python

``` python
from launch import LaunchDescription
from launch.actions import IncludeLaunchDescription
from launch.substitutions import PathJoinSubstitution
from launch_ros.actions import Node
from launch_ros.substitutions import FindPackageShare


def generate_launch_description():
    return LaunchDescription([
        IncludeLaunchDescription(
            PathJoinSubstitution([
                FindPackageShare('learning_tf2_cpp'), 'launch', 'turtle_tf2_demo.launch.py']),
            launch_arguments={'target_frame': 'carrot1'}.items(),
        ),
        Node(
            package='learning_tf2_cpp',
            executable='dynamic_frame_tf2_broadcaster',
            name='dynamic_broadcaster',
        ),
    ])
```

##### XML 数据

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<launch>
  <include file="$(find-pkg-share learning_tf2_cpp)/launch/turtle_tf2_demo_launch.xml">
    <let name="target_frame" value="carrot1" />
  </include>
  <node pkg="learning_tf2_cpp" exec="dynamic_frame_tf2_broadcaster" name="dynamic_broadcaster" />
</launch>
```

##### 也门

``` yaml
%YAML 1.2
---
launch:
  - include:
      file: "$(find-pkg-share learning_tf2_cpp)/launch/turtle_tf2_demo_launch.py"
      let:
      - name: "target_frame"
        value: "carrot1"
  - node:
      pkg: "learning_tf2_cpp"
      exec: "dynamic_frame_tf2_broadcaster"
      name: "dynamic_broadcaster"
```

<span id="id4"></span>

#### 2.4 构建

运行 `rosdep` 在工作空间的根中检查缺失的依赖性。

##### Linux

``` console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

##### macOS

rosdep 只运行在 Linux 上, 因此您需要安装 `geometry_msgs` 财务报告和财务报告 `turtlesim` 依附关系

##### Windows

rosdep 只运行在 Linux 上, 因此您需要安装 `geometry_msgs` 财务报告和财务报告 `turtlesim` 依附关系

仍然在工作区根部,构建您的软件包:

##### Linux

``` console
$ colcon build --packages-select learning_tf2_cpp
```

##### macOS

``` console
$ colcon build --packages-select learning_tf2_cpp
```

##### Windows

``` console
$ colcon build --merge-install --packages-select learning_tf2_cpp
```

打开新终端, 导航到您工作空间的根, 并源代码设置文件 :

##### Linux

``` console
$ . install/setup.bash
```

##### macOS

``` console
$ . install/setup.bash
```

##### Windows

在 Windows 命令行提示中 :

``` console
$ call install\setup.bat
```

或于权壳中:

``` console
$ .\install\setup.ps1
```

<span id="id5"></span>

#### 2.5 运行

现在你可以开始动态帧演示:

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_dynamic_frame_demo_launch.xml # .py or .yaml are also acceptable
```

第二只乌龟遵循着胡萝卜的姿势,

![](images/carrot_dynamic.png) <span id="summary"></span>

## 小结

在这个教程中,你学到了 tf2 变换树、其结构及其特征。你还学会了在本地框架内思考最容易,并学会了为本地框架添加额外的固定和动态框架。
