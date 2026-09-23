<span id="using-urdf-with-robot-state-publisher-c"></span> <span id="urdfplusrspcpp"></span>

# 将 URDF 与 robot_state_publisher 配合使用（C++）

**目标：** 仿真一个用 URDF 建模的行走机器人，并在 RViz 中查看。

**教程级别：** 中级

**预计耗时：** 15 分钟

<span id="background"></span>

## 背景

本教程介绍如何为行走机器人建模，以 tf2 消息发布状态，并在 RViz 中查看仿真。首先创建描述机器人组成结构的 URDF 模型，然后编写模拟运动并发布 JointState 和变换的节点，再用 `robot_state_publisher` 将整个机器人的状态发布到 `/tf`。

![](images/r2d2_rviz_demo.gif)

<span id="prerequisites"></span>

## 前提条件

- [rviz2](https://index.ros.org/p/rviz2/)

与往常一样，别忘记在[每个新打开的终端](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)中加载 ROS 2 环境。

<span id="tasks"></span>

## 任务

<span id="create-a-package"></span>

### 1 创建软件包

进入 ROS 2 工作空间，创建名为 `urdf_tutorial_cpp` 的包：

```console
$ cd src
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 urdf_tutorial_cpp --dependencies rclcpp geometry_msgs sensor_msgs tf2_ros tf2_geometry_msgs
$ cd urdf_tutorial_cpp
```

现在应能看到 `urdf_tutorial_cpp` 文件夹，接下来将对其进行多项修改。

<span id="create-the-urdf-file"></span>

### 2 创建 URDF 文件

创建存放资源的目录。

Linux：

```console
$ mkdir -p urdf
```

macOS：

```console
$ mkdir -p urdf
```

Windows：

```console
$ md urdf
```

下载 [URDF 文件](documents/r2d2.urdf.xml)，保存为 `urdf_tutorial_cpp/urdf/r2d2.urdf.xml`。下载 [RViz 配置文件](documents/r2d2.rviz)，保存为 `urdf_tutorial_cpp/urdf/r2d2.rviz`。

<span id="publish-the-state"></span>

### 3 发布状态

现在需要描述机器人的当前状态，为此必须指定三个关节以及机器人整体的几何关系。

打开编辑器，将以下代码粘贴到 `urdf_tutorial_cpp/src/urdf_tutorial.cpp`：

```cpp
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

这个节点完成两项工作：

- 向 `/joint_states` 发布 `JointState` 消息，使 `robot_state_publisher` 能计算各关节的变换，并通过 `/tf` 广播。
- 广播一个根变换，将机器人模型的 `axis` 坐标系放入世界的 `odom` 坐标系中，使整个机器人沿圆周运动。

参阅 [rclcpp 便捷头文件说明](../../../_internal/Rclcpp-Convenience-Header-Note.md)。

<span id="create-a-launch-file"></span>

### 4 创建启动文件

创建 `urdf_tutorial_cpp/launch` 文件夹，将[启动文件示例](launch/launch.py)保存为 `urdf_tutorial_cpp/launch/launch.py`。

<span id="edit-the-cmakelists-txt-file"></span>

### 5 编辑 CMakeLists.txt

必须告诉 **colcon** 如何安装 C++ 软件包。按如下方式修改 `CMakeLists.txt`：

```cmake
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

`install(DIRECTORY urdf ...)` 规则会将 `r2d2.urdf.xml` 和 `r2d2.rviz` 复制到安装目录树，使程序在运行时能够找到它们。

<span id="build-the-package"></span>

### 6 构建软件包

返回工作空间根目录并构建：

```console
$ colcon build --symlink-install --packages-select urdf_tutorial_cpp
```

加载环境设置文件。

Linux：

```console
$ source install/setup.bash
```

macOS：

```console
$ source install/setup.bash
```

Windows：

```console
$ call install/setup.bat
```

<span id="view-the-results"></span>

### 7 查看结果

运行以下命令启动新软件包：

```console
$ ros2 launch urdf_tutorial_cpp launch.py
```

打开新终端，用 RViz 配置文件启动 RViz，查看结果：

```console
$ rviz2 -d install/urdf_tutorial_cpp/share/urdf_tutorial_cpp/urdf/r2d2.rviz
```

RViz 的使用方法见[用户指南](http://wiki.ros.org/rviz/UserGuide)。`r2d2.rviz` 的存放路径为 `install/urdf_tutorial_cpp/share/urdf_tutorial_cpp/urdf/r2d2.rviz`。

<span id="summary"></span>

## 小结

恭喜！你已创建一个 `JointState` 发布节点，并将它与 `robot_state_publisher` 配合使用，仿真了一个行走机器人。
