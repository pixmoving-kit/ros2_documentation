---
translation_status: machine_translated
source: How-To-Guides/Migrating-from-ROS1/Migrating-CPP-Package-Example.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="migrating-a-c-package-example"></span>

# C++ 软件包迁移示例

这个例子说明了如何将一个实例C++包从ROS 1迁移到ROS 2.

<span id="prerequisites"></span>

## 前提条件

你需要一个工作 ROS 2 安装,例如 [ROS 滚动](../../Installation.md).

<span id="the-ros-1-code"></span>

## ROS 1 代码

说你有一个ROS 1包 叫做 `talker` 用于 `roscpp` 在一个节点中,称为 `talker`。这个软件包位于一个位于 `~/ros1_talker`.

您的 ROS 1 工作空间有以下目录布局 :

``` console
$ cd ~/ros1_talker
$ find .
.
./src
./src/talker
./src/talker/package.xml
./src/talker/CMakeLists.txt
./src/talker/talker.cpp
```

文件的内容如下:

`src/talker/package.xml`:

``` xml
<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/schema/package_format2.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="2">
  <name>talker</name>
  <version>0.0.0</version>
  <description>talker</description>
  <maintainer email="gerkey@example.com">Brian Gerkey</maintainer>
  <license>Apache-2.0</license>
  <buildtool_depend>catkin</buildtool_depend>
  <depend>roscpp</depend>
  <depend>std_msgs</depend>
</package>
```

`src/talker/CMakeLists.txt`:

``` cmake
cmake_minimum_required(VERSION 2.8.3)
project(talker)
find_package(catkin REQUIRED COMPONENTS roscpp std_msgs)
catkin_package()
include_directories(${catkin_INCLUDE_DIRS})
add_executable(talker talker.cpp)
target_link_libraries(talker ${catkin_LIBRARIES})
install(TARGETS talker
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
```

`src/talker/talker.cpp`:

``` cpp
#include <sstream>
#include "ros/ros.h"
#include "std_msgs/String.h"
int main(int argc, char **argv)
{
  ros::init(argc, argv, "talker");
  ros::NodeHandle n;
  ros::Publisher chatter_pub = n.advertise<std_msgs::String>("chatter", 1000);
  ros::Rate loop_rate(10);
  int count = 0;
  std_msgs::String msg;
  while (ros::ok())
  {
    std::stringstream ss;
    ss << "hello world " << count++;
    msg.data = ss.str();
    ROS_INFO("%s", msg.data.c_str());
    chatter_pub.publish(msg);
    ros::spinOnce();
    loop_rate.sleep();
  }
  return 0;
}
```

<span id="migrating-to-ros-2"></span>

## 向 ROS 2 移动

让我们首先创建一个新的工作空间,以便开展工作:

``` console
$ mkdir ~/ros2_talker
$ cd ~/ros2_talker
```

将源树从ROS 1软件包复制到工作区,

``` console
$ mkdir src
$ cp -a ~/ros1_talker/src/talker src
```

现在我们将修改节点中的 C++ 代码。 ROS 2 C++ 库叫做 `rclcpp`,提供不同于由下列人员提供的API: `roscpp`. 两个库之间的概念非常相似,因此可以合理地直截了当地进行修改。

<span id="included-headers"></span>

### 包含标题

替换 `ros/ros.h`,这让我们可以进入 `roscpp` 库 API,我们需要包含 `rclcpp/rclcpp.hpp`,这让我们可以进入 `rclcpp` 库 API :

``` cpp
//#include "ros/ros.h"
#include "rclcpp/rclcpp.hpp"
```

为了得到 `std_msgs/String` 信件定义,替换 `std_msgs/String.h`,我们需要包括 `std_msgs/msg/string.hpp`:

``` cpp
//#include "std_msgs/String.h"
#include "std_msgs/msg/string.hpp"
```

<span id="changing-c-library-calls"></span>

### 更改 C++ 库呼叫

我们不把节点的名字传递给库初始化呼叫,而是进行初始化,然后将节点名称传递给节点对象的创建:

``` cpp
//  ros::init(argc, argv, "talker");
//  ros::NodeHandle n;
    rclcpp::init(argc, argv);
    auto node = rclcpp::Node::make_shared("talker");
```

出版商和速率对象的创建看起来相当相似,命名空间和方法的名称也有一些变化.

``` cpp
//  ros::Publisher chatter_pub = n.advertise<std_msgs::String>("chatter", 1000);
//  ros::Rate loop_rate(10);
  auto chatter_pub = node->create_publisher<std_msgs::msg::String>("chatter",
    1000);
  rclcpp::Rate loop_rate(10);
```

为进一步控制信息发送的处理方式,服务质量(`QoS`) 配置文件可以传递。默认配置文件是 `rmw_qos_profile_default`。详细情况见 [设计文件](https://design.ros2.org/articles/qos.html) 财务报告和财务报告 [概念概览](../../Concepts/Intermediate/About-Quality-of-Service-Settings.md).

发送消息的创建在命名空间中有所不同:

``` cpp
//  std_msgs::String msg;
  std_msgs::msg::String msg;
```

替换 `ros::ok()`,我们叫 `rclcpp::ok()`:

``` cpp
//  while (ros::ok())
  while (rclcpp::ok())
```

在出版圈内,我们访问 `data` 字段如前:

``` cpp
msg.data = ss.str();
```

要打印控制台消息, 而不是使用 `ROS_INFO()`,我们使用 `RCLCPP_INFO()` 和不同的堂兄弟,关键区别在于: `RCLCPP_INFO()` 以 Logger 对象作为第一个参数。

``` cpp
//    ROS_INFO("%s", msg.data.c_str());
    RCLCPP_INFO(node->get_logger(), "%s\n", msg.data.c_str());
```

更改发布调用以使用 `->` 运算符代替 `.`.

``` cpp
//    chatter_pub.publish(msg);
    chatter_pub->publish(msg);
```

旋转(即让通信系统处理任何待处理的收/发信件,直到不再有工作可用)是不同的,因为现在的呼叫以节点和超时为参数:

``` cpp
//    ros::spinOnce();
    rclcpp::spin_all(node, 0s);
```

使用速率对象睡眠不变.

把它拼凑起来,新的 `talker.cpp` 看起来像这个:

``` cpp
#include <chrono>
#include <sstream>
// #include "ros/ros.h"
#include "rclcpp/rclcpp.hpp"
// #include "std_msgs/String.h"
#include "std_msgs/msg/string.hpp"

using namespace std::chrono_literals;

int main(int argc, char **argv)
{
//  ros::init(argc, argv, "talker");
//  ros::NodeHandle n;
  rclcpp::init(argc, argv);
  auto node = rclcpp::Node::make_shared("talker");
//  ros::Publisher chatter_pub = n.advertise<std_msgs::String>("chatter", 1000);
//  ros::Rate loop_rate(10);
  auto chatter_pub = node->create_publisher<std_msgs::msg::String>("chatter", 1000);
  rclcpp::Rate loop_rate(10);
  int count = 0;
//  std_msgs::String msg;
  std_msgs::msg::String msg;
//  while (ros::ok())
  while (rclcpp::ok())
  {
    std::stringstream ss;
    ss << "hello world " << count++;
    msg.data = ss.str();
//    ROS_INFO("%s", msg.data.c_str());
    RCLCPP_INFO(node->get_logger(), "%s\n", msg.data.c_str());
//    chatter_pub.publish(msg);
    chatter_pub->publish(msg);
//    ros::spinOnce();
    rclcpp::spin_all(node, 0s);
    loop_rate.sleep();
  }
  return 0;
}
```

<span id="change-the-package-xml"></span>

### 改变 `package.xml`

ROS 2 软件包使用 CMake 函数和宏 `ament_cmake_ros` 改为 `catkin`删除对以下的依赖: `catkin`:

``` default
<!-- delete this -->
<buildtool_depend>catkin</buildtool_depend>`
```

添加一个新的依赖 `ament_cmake_ros`:

``` xml
<buildtool_depend>ament_cmake_ros</buildtool_depend>
```

ROS 2 C++ 库使用 [rclcpp](https://index.ros.org/p/rclcpp/#rolling) 改为 [罗斯普](https://index.ros.org/p/roscpp/#noetic).

删除对 `roscpp`:

``` default
<!-- delete this -->
<depend>roscpp</depend>
```

添加依赖 `rclcpp`:

``` xml
<depend>rclcpp</depend>
```

添加一个 `<export>` 要告诉 Colcon 软件包是 `ament_cmake` 包而不是一个 `catkin` 软件包。

``` xml
<export>
  <build_type>ament_cmake</build_type>
</export>
```

 `package.xml` 现在看起来是这样的:

``` xml
<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/schema/package_format2.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="2">
  <name>talker</name>
  <version>0.0.0</version>
  <description>talker</description>
  <maintainer email="gerkey@example.com">Brian Gerkey</maintainer>
  <license>Apache-2.0</license>
  <buildtool_depend>ament_cmake</buildtool_depend>
  <depend>rclcpp</depend>
  <depend>std_msgs</depend>
  <export>
    <build_type>ament_cmake</build_type>
  </export>
</package>
```

<span id="changing-the-cmake-code"></span>

### 更改 CMake 代码

需要更新 CMake 版本, 以便 `ament_cmake` 函数工作正确。

``` cmake
cmake_minimum_required(VERSION 3.14.4)
```

使用更新的 C++ 标准来匹配您的目标ROS distro 使用的版本 [REP 2000 环境方案](https://reps.openrobotics.org/rep-2000/)。如果您正在使用 C++17,那么在 `project(talker)` 。添加额外的编译器检查,因为这是一个好的做法。

``` cmake
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()
```

替换 `find_package(catkin ...)` 每一个受抚养人的电话都是单独呼叫的。

``` cmake
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
```

删除呼叫到 `catkin_package()`。添加一个呼叫到 `ament_package()` 在底部的边上 `CMakeLists.txt`.

``` cmake
ament_package()
```

做一个 `target_link_libraries` 调用现代 CMake 目标 `rclcpp` 财务报告和财务报告 `std_msgs`.

``` cmake
target_link_libraries(talker PUBLIC
  rclcpp::rclcpp
  ${std_msgs_TARGETS})
```

删除呼叫到 `include_directories()`。添加一个呼叫到 `target_include_directories()` 下级 `add_executable(talker talker.cpp)`。不要通过变量,如 `rclcpp_INCLUDE_DIRS` 输入 `target_include_directories()`。包含的目录已经通过调用处理 `target_link_libraries()` 有现代的CMake目标。

``` cmake
target_include_directories(talker PUBLIC
   "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
   "$<INSTALL_INTERFACE:include/${PROJECT_NAME}>")
```

将呼叫更改为 `install()` 因此, `talker` 可执行文件安装到项目特定目录中。

``` cmake
install(TARGETS talker
  DESTINATION lib/${PROJECT_NAME})
```

新的计划 `CMakeLists.txt` 看起来像这个:

``` cmake
cmake_minimum_required(VERSION 3.14.4)
project(talker)
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
add_executable(talker talker.cpp)
target_include_directories(talker PUBLIC
   "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
   "$<INSTALL_INTERFACE:include/${PROJECT_NAME}>")
target_link_libraries(talker PUBLIC
  rclcpp::rclcpp
  ${std_msgs_TARGETS})
install(TARGETS talker
  DESTINATION lib/${PROJECT_NAME})
ament_package()
```

<span id="building-the-ros-2-code"></span>

### 构建 ROS 2 代码

我们来自一个环境设置文件(在此情况下,由跟踪ROS 2 安装教程而生成的文件,该教程建在 `~/ros2_ws`,然后我们构建我们的软件包使用 `colcon build`:

``` console
$ . ~/ros2_ws/install/setup.bash
$ cd ~/ros2_talker
$ colcon build
```

<span id="running-the-ros-2-node"></span>

### 运行 ROS 2 节点

因为我们安装了 `talker` 可执行到正确的目录中,从我们的安装树中获取设置文件后,我们可以通过运行来引用它:

``` console
$ . ~/ros2_ws/install/setup.bash
$ ros2 run talker talker
```

<span id="conclusion"></span>

## 结论

您已经学会了如何将一个实例 C++ ROS 1 软件包迁移到 ROS 2 。 [移动 C++ 软件包参考页面](Migrating-CPP-Packages.md) 帮助您将您的 C++ 软件包从 ROS 1 迁移到 ROS 2 。
