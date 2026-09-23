<span id="migrating-a-c-package-example"></span>
# C++ 软件包迁移示例

本示例介绍如何将一个 C++ 软件包从 ROS 1 迁移到 ROS 2。

<span id="prerequisites"></span>
## 前提条件

需要一个可正常工作的 ROS 2 环境，例如 [ROS Rolling](../../Installation.md)。

<span id="the-ros-1-code"></span>
## ROS 1 代码

假设有一个名为 `talker` 的 ROS 1 软件包，其中的 `talker` 节点使用 `roscpp`。该软件包位于 `~/ros1_talker` 中的 catkin 工作空间。

工作空间的目录结构如下：

```console
$ cd ~/ros1_talker
$ find .
.
./src
./src/talker
./src/talker/package.xml
./src/talker/CMakeLists.txt
./src/talker/talker.cpp
```

各文件内容如下。

`src/talker/package.xml`：

```xml
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

`src/talker/CMakeLists.txt`：

```cmake
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

`src/talker/talker.cpp`：

```cpp
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
## 迁移到 ROS 2

首先创建新的工作空间：

```console
$ mkdir ~/ros2_talker
$ cd ~/ros2_talker
```

将 ROS 1 软件包的源码树复制到该工作空间，以便修改：

```console
$ mkdir src
$ cp -a ~/ros1_talker/src/talker src
```

接下来修改节点中的 C++ 代码。ROS 2 的 C++ 库 `rclcpp` 与 `roscpp` 提供不同的 API，但两者的概念非常相似，因此修改相对直接。

<span id="included-headers"></span>
### 引入的头文件

将用于访问 `roscpp` API 的 `ros/ros.h` 替换为用于访问 `rclcpp` API 的 `rclcpp/rclcpp.hpp`：

```cpp
//#include "ros/ros.h"
#include "rclcpp/rclcpp.hpp"
```

要获取 `std_msgs/String` 消息定义，应引入 `std_msgs/msg/string.hpp`，替代 `std_msgs/String.h`：

```cpp
//#include "std_msgs/String.h"
#include "std_msgs/msg/string.hpp"
```

<span id="changing-c-library-calls"></span>
### 修改 C++ 库调用

不再在初始化库时传入节点名称，而是先初始化库，再在创建节点对象时传入名称：

```cpp
//  ros::init(argc, argv, "talker");
//  ros::NodeHandle n;
    rclcpp::init(argc, argv);
    auto node = rclcpp::Node::make_shared("talker");
```

创建发布者和频率对象的方式类似，主要变化是命名空间和方法名称：

```cpp
//  ros::Publisher chatter_pub = n.advertise<std_msgs::String>("chatter", 1000);
//  ros::Rate loop_rate(10);
  auto chatter_pub = node->create_publisher<std_msgs::msg::String>("chatter",
    1000);
  rclcpp::Rate loop_rate(10);
```

如果需要进一步控制消息传递方式，可以传入服务质量（QoS）配置，默认配置为 `rmw_qos_profile_default`。更多信息见[设计文档](https://design.ros2.org/articles/qos.html)和[概念概述](../../Concepts/Intermediate/About-Quality-of-Service-Settings.md)。

创建待发送消息时，命名空间也有所变化：

```cpp
//  std_msgs::String msg;
  std_msgs::msg::String msg;
```

使用 `rclcpp::ok()` 替代 `ros::ok()`：

```cpp
//  while (ros::ok())
  while (rclcpp::ok())
```

在发布循环内部，仍以原方式访问 `data` 字段：

```cpp
msg.data = ss.str();
```

向控制台输出日志时，改用 `RCLCPP_INFO()` 及其相关宏，而不是 `ROS_INFO()`。主要区别是 `RCLCPP_INFO()` 的第一个参数为 Logger 对象：

```cpp
//    ROS_INFO("%s", msg.data.c_str());
    RCLCPP_INFO(node->get_logger(), "%s\n", msg.data.c_str());
```

发布调用中的 `.` 运算符改为 `->`：

```cpp
//    chatter_pub.publish(msg);
    chatter_pub->publish(msg);
```

spin 操作让通信系统处理待收发消息，直到没有工作需要处理；现在调用时需要传入节点和超时参数：

```cpp
//    ros::spinOnce();
    rclcpp::spin_all(node, 0s);
```

使用频率对象休眠的方式不变。

将以上修改合并后，新的 `talker.cpp` 如下：

```cpp
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
### 修改 package.xml

ROS 2 软件包使用 `ament_cmake_ros` 提供的 CMake 函数和宏，替代 `catkin`。删除对 `catkin` 的依赖：

```
<!-- delete this -->
<buildtool_depend>catkin</buildtool_depend>`
```

添加对 `ament_cmake_ros` 的依赖：

```xml
<buildtool_depend>ament_cmake_ros</buildtool_depend>
```

ROS 2 的 C++ 库使用 [rclcpp](https://index.ros.org/p/rclcpp/#rolling)，替代 [roscpp](https://index.ros.org/p/roscpp/#noetic)。删除 `roscpp` 依赖：

```
<!-- delete this -->
<depend>roscpp</depend>
```

添加 `rclcpp` 依赖：

```xml
<depend>rclcpp</depend>
```

添加 `<export>` 部分，告诉 colcon 这是 `ament_cmake` 包，而不是 `catkin` 包：

```xml
<export>
  <build_type>ament_cmake</build_type>
</export>
```

现在 `package.xml` 应如下所示：

```xml
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
### 修改 CMake 代码

要求更新的 CMake 版本，以确保 `ament_cmake` 函数正常工作：

```cmake
cmake_minimum_required(VERSION 3.14.4)
```

根据 [REP 2000](https://reps.openrobotics.org/rep-2000/)，使用与目标 ROS 发行版匹配的较新 C++ 标准。如果使用 C++17，请在 `project(talker)` 后添加以下内容。同时启用额外的编译器检查，这是一种良好实践：

```cmake
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()
```

将 `find_package(catkin ...)` 替换为对各依赖项的独立调用：

```cmake
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
```

删除 `catkin_package()` 调用，并在 `CMakeLists.txt` 末尾添加 `ament_package()`：

```cmake
ament_package()
```

在 `target_link_libraries` 中使用 `rclcpp` 和 `std_msgs` 提供的现代 CMake 目标：

```cmake
target_link_libraries(talker PUBLIC
  rclcpp::rclcpp
  ${std_msgs_TARGETS})
```

删除 `include_directories()`，在 `add_executable(talker talker.cpp)` 后添加 `target_include_directories()`。不要向其中传入 `rclcpp_INCLUDE_DIRS` 等变量，因为使用现代 CMake 目标调用 `target_link_libraries()` 时，已经处理了依赖项的头文件目录：

```cmake
target_include_directories(talker PUBLIC
   "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
   "$<INSTALL_INTERFACE:include/${PROJECT_NAME}>")
```

修改 `install()`，将 `talker` 可执行文件安装到项目专用目录：

```cmake
install(TARGETS talker
  DESTINATION lib/${PROJECT_NAME})
```

新的 `CMakeLists.txt` 如下：

```cmake
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

加载环境设置文件，此处使用按照 ROS 2 安装教程在 `~/ros2_ws` 构建后生成的文件，然后用 `colcon build` 构建软件包：

```console
$ . ~/ros2_ws/install/setup.bash
$ cd ~/ros2_talker
$ colcon build
```

<span id="running-the-ros-2-node"></span>
### 运行 ROS 2 节点

由于已将 `talker` 安装到正确目录，加载安装空间中的环境设置文件后，就能运行：

```console
$ . ~/ros2_ws/install/setup.bash
$ ros2 run talker talker
```

<span id="conclusion"></span>
## 小结

你已了解如何将一个 ROS 1 C++ 软件包迁移到 ROS 2。迁移自己的软件包时，可以使用 [C++ 软件包迁移参考](Migrating-CPP-Packages.md)。
