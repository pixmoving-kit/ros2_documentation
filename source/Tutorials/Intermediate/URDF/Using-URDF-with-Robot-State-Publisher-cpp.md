---
translation_status: machine_translated
source: Tutorials/Intermediate/URDF/Using-URDF-with-Robot-State-Publisher-cpp.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-urdf-with-robot-state-publisher-c"></span> <span id="urdfplusrspcpp"></span>

# 使用URDF为 `robot_state_publisher` (C++)

**目标：** 模拟一个以URDF为模型的行走机器人,并在Rviz查看.

**教程级别：** 中级

**用时：** 15分钟

<span id="background"></span>

## 背景

这个教程将教你如何模拟一个行走机器人, 将状态作为 tf2 消息发布, 并在 Rviz 查看模拟。 首先, 我们创建描述机器人组装的 URDF 模型。 然后我们写一个节点, 模拟运动并发布 Joint State 和变形。 然后我们使用 `robot_state_publisher` 以发布整个机器人状态 `/tf`.

![](images/r2d2_rviz_demo.gif) <span id="prerequisites"></span>

## 前提条件

- [rviz2 (中文(简体) ).](https://index.ros.org/p/rviz2/)

与往常一样, [您打开的每个新终端](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md).

<span id="tasks"></span>

## 操作步骤

<span id="create-a-package"></span>

### 1 创建软件包

转到您的 ROS 2 工作空间并创建一个名为 的软件包 `urdf_tutorial_cpp`:

``` console
$ cd src
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 urdf_tutorial_cpp --dependencies rclcpp geometry_msgs sensor_msgs tf2_ros tf2_geometry_msgs
$ cd urdf_tutorial_cpp
```

你现在应该看看 `urdf_tutorial_cpp` 文件夹。接下来将对它进行若干修改。

<span id="create-the-urdf-file"></span>

### 2 创建 URDF 文件

创建目录, 用于存储一些资产 :

##### Linux

``` console
$ mkdir -p urdf
```

##### macOS

``` console
$ mkdir -p urdf
```

##### Windows

``` console
$ md urdf
```

下载 [`URDF file`](documents/r2d2.urdf.xml) 并保存为 `urdf_tutorial_cpp/urdf/r2d2.urdf.xml`下载 [`Rviz configuration file`](documents/r2d2.rviz) 并保存为 `urdf_tutorial_cpp/urdf/r2d2.rviz`.

<span id="publish-the-state"></span>

### 3 公布国家

现在我们需要一种方法来说明机器人处于何种状态。

要做到这一点,我们必须具体说明所有三个关节和整体机器人几何.

点燃您最喜欢的编辑器并粘贴以下代码

`urdf_tutorial_cpp/src/urdf_tutorial.cpp`

``` cpp
#include <rclcpp/rclcpp.hpp>
#include <geometry_msgs/msg/quaternion.hpp>
#include <sensor_msgs/msg/joint_state.hpp>
#include <tf2_ros/transform_broadcaster.h>
#include <tf2_geometry_msgs/tf2_geometry_msgs.hpp>
#include <cmath>
#include <thread>
#include <chrono>

using namespace std::chrono;

class StatePublisher : public rclcpp::Node {
    public:

    StatePublisher(rclcpp::NodeOptions options=rclcpp::NodeOptions()):
        Node("state_publisher", options){
            joint_pub_ = this->create_publisher<sensor_msgs::msg::JointState>("joint_states",10);
            // create a publisher to tell robot_state_publisher the JointState information.
            // robot_state_publisher will deal with this transformation
            broadcaster = std::make_shared<tf2_ros::TransformBroadcaster>(this);
            // create a broadcaster to tell the tf2 state information
            // this broadcaster will determine the position of coordinate system 'axis' in coordinate system 'odom'
            RCLCPP_INFO(this->get_logger(),"Starting state publisher");

            timer_=this->create_wall_timer(33ms,std::bind(&StatePublisher::publish,this));
        }

    private:
    rclcpp::Publisher<sensor_msgs::msg::JointState>::SharedPtr joint_pub_;
    std::shared_ptr<tf2_ros::TransformBroadcaster> broadcaster;
    rclcpp::TimerBase::SharedPtr timer_;

    // Robot state variables (one degree in radians)
    const double degree = M_PI/180.0;
    double tilt = 0.;
    double tinc = degree;
    double swivel = 0.;
    double angle = 0.;
    double height = 0.;
    double hinc = 0.005;

    void publish();
};

void StatePublisher::publish(){
    // create the necessary messages
    geometry_msgs::msg::TransformStamped t;
    sensor_msgs::msg::JointState joint_state;

    const auto ts = this->get_clock()->now();
    joint_state.header.stamp = ts;
    // Specify joints' name which are defined in the r2d2.urdf.xml and their content
    joint_state.name={"swivel","tilt","periscope"};
    joint_state.position={swivel,tilt,height};

    // add time stamp
    t.header.stamp = ts;
    // specify the father and child frame

    // odom is the base coordinate system of tf2
    t.header.frame_id="odom";
    // axis is defined in r2d2.urdf.xml file and it is the base coordinate of model
    t.child_frame_id="axis";

    // add translation change
    t.transform.translation.x=cos(angle)*2;
    t.transform.translation.y=sin(angle)*2;
    t.transform.translation.z=0.7;
    tf2::Quaternion q;
    // euler angle into Quaternion and add rotation change
    q.setRPY(0,0,angle+M_PI/2);
    t.transform.rotation.x=q.x();
    t.transform.rotation.y=q.y();
    t.transform.rotation.z=q.z();
    t.transform.rotation.w=q.w();

    // update state for next time
    tilt+=tinc;
    if (tilt<-0.5 || tilt>0.0){
        tinc*=-1;
    }
    height+=hinc;
    if (height>0.2 || height<0.0){
        hinc*=-1;
    }
    swivel+=degree;  // Increment by 1 degree (in radians)
    angle+=degree;    // Change angle at a slower pace

    // send message
    broadcaster->sendTransform(t);
    joint_pub_->publish(joint_state);

    RCLCPP_INFO_THROTTLE(this->get_logger(), *this->get_clock(), 1000, "Publishing joint state");
}

int main(int argc, char * argv[]){
    rclcpp::init(argc,argv);
    rclcpp::spin(std::make_shared<StatePublisher>());
    rclcpp::shutdown();
    return 0;
}
```

这个节点做两件事: - 出版 `JointState` 给您的消息 `/joint_states` 专题 `robot_state_publisher` 可以计算所有每台联调联变换,并通过 `/tf`- 广播一个将机器人模型置于位置的单一根变换(`axis` (框架),世界的(框架)`odom` 框架),使整个机器人在一个圆圈中行走.

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

<span id="create-a-launch-file"></span>

### 4 创建发射文件

创建新 `urdf_tutorial_cpp/launch` 文件夹。打开编辑器并粘贴以下代码,保存为 `urdf_tutorial_cpp/launch/launch.py`

``` python
from launch import LaunchDescription
from launch.actions import DeclareLaunchArgument
from launch.substitutions import FileContent, LaunchConfiguration, PathJoinSubstitution
from launch_ros.actions import Node
from launch_ros.substitutions import FindPackageShare


def generate_launch_description():
    # ''use_sim_time'' is used to have ros2 use /clock topic for the time source
    use_sim_time = LaunchConfiguration('use_sim_time', default='false')

    urdf = FileContent(
        PathJoinSubstitution([FindPackageShare('urdf_tutorial_cpp'), 'urdf', 'r2d2.urdf.xml']))

    return LaunchDescription([
        DeclareLaunchArgument(
            'use_sim_time',
            default_value='false',
            description='Use simulation (Gazebo) clock if true'),
        Node(
            package='robot_state_publisher',
            executable='robot_state_publisher',
            name='robot_state_publisher',
            output='screen',
            parameters=[{'use_sim_time': use_sim_time, 'robot_description': urdf}],
            arguments=[urdf]),
        Node(
            package='urdf_tutorial_cpp',
            executable='urdf_tutorial_cpp',
            name='urdf_tutorial_cpp',
            output='screen'),
    ])
```

<span id="edit-the-cmakelists-txt-file"></span>

### 5 编辑 CMakeLists.txt 文件

你必须告诉 **colcon** 构建如何安装您的 cpp 软件包的工具。 编辑 `CMakeLists.txt` 文件如下:

``` cmake
cmake_minimum_required(VERSION 3.8)
project(urdf_tutorial_cpp)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)
find_package(rclcpp REQUIRED)

add_executable(urdf_tutorial_cpp src/urdf_tutorial.cpp)

ament_target_dependencies(urdf_tutorial_cpp
  geometry_msgs
  sensor_msgs
  tf2_ros
  tf2_geometry_msgs
  rclcpp
)

install(TARGETS
  urdf_tutorial_cpp
  DESTINATION lib/${PROJECT_NAME}
)

install(DIRECTORY
  launch
  DESTINATION share/${PROJECT_NAME}
)

install(DIRECTORY
  urdf
  DESTINATION share/${PROJECT_NAME}
)

ament_package()
```

那个... `install(DIRECTORY urdf ...)` 规则副本 `r2d2.urdf.xml` 财务报告和财务报告 `r2d2.rviz` 进入安装树,以便在运行时找到它们。

<span id="build-the-package"></span>

### 6 构建软件包

返回工作空间根并构建 :

``` console
$ colcon build --symlink-install --packages-select urdf_tutorial_cpp
```

来源设置文件 :

##### Linux

``` console
$ source install/setup.bash
```

##### macOS

``` console
$ source install/setup.bash
```

##### Windows

``` console
$ call install/setup.bat
```

<span id="view-the-results"></span>

### 7 查看结果

要启动您的新软件包, 请运行以下命令 :

``` console
$ ros2 launch urdf_tutorial_cpp launch.py
```

要可视化您的结果, 您需要打开一个新的终端, 并使用您的 rviz 配置文件运行 Rviz 。

``` console
$ rviz2 -d install/urdf_tutorial_cpp/share/urdf_tutorial_cpp/urdf/r2d2.rviz
```

见 [用户指南](http://wiki.ros.org/rviz/UserGuide) 详细介绍如何使用Rviz。

`install/urdf_tutorial_cpp/share/urdf_tutorial_cpp/urdf/r2d2.rviz` 是目录,其中 `r2d2.rviz` 存储。

<span id="summary"></span>

## 小结

恭喜你,你创造了一个 `JointState` 出版商节点并结合 `robot_state_publisher` 模拟一个行走的机器人。
