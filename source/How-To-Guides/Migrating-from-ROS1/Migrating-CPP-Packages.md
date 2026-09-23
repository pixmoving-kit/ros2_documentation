<span id="migrating-c-packages-reference"></span>
# C++ 软件包迁移参考

本页介绍如何将 C++ 软件包中的各部分从 ROS 1 迁移到 ROS 2。如果是首次迁移 C++ 软件包，请先阅读 [C++ 迁移示例](Migrating-CPP-Package-Example.md)，之后迁移自己的软件包时，可以将本页作为参考。

<span id="build-tool"></span>
## 构建工具

ROS 2 使用命令行工具 [colcon](https://design.ros2.org/articles/build_tool.html) 构建并安装一组软件包，替代 `catkin_make`、`catkin_make_isolated` 或 `catkin build`。colcon 入门见[初级教程](../../Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.md)。

<span id="update-your-cmakelists-txt-to-use-ament-cmake"></span>
## 更新 CMakeLists.txt，改用 ament_cmake

ROS 2 C++ 软件包使用 [CMake](https://cmake.org/)，并借助 [ament_cmake](https://index.ros.org/p/ament_cmake/) 提供的便捷函数。按以下步骤将 `catkin` 替换为 `ament_cmake`。

<span id="require-a-newer-version-of-cmake"></span>
### 要求更新的 CMake 版本

ROS 2 依赖的 CMake 版本比 ROS 1 更新。请在 [REP 2000](https://reps.openrobotics.org/rep-2000/) 中找到目标 ROS 发行版使用的最低 CMake 版本，并在 `CMakeLists.txt` 开头指定。例如，[ROS Humble 推荐支持的最低版本为 3.14.4](https://reps.openrobotics.org/rep-2000/#humble-hawksbill-may-2022-may-2027)：

```
cmake_minimum_required(VERSION 3.14.4)
```

<span id="set-the-build-type-to-ament-cmake"></span>
### 将构建类型设为 ament_cmake

删除 `package.xml` 中对 `catkin` 的依赖：

```
# Remove this!
<buildtool_depend>catkin</buildtool_depend>
```

添加对 `ament_cmake_ros` 的依赖，参见[示例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/package.xml#L25)：

```xml
<buildtool_depend>ament_cmake_ros</buildtool_depend>
```

如果 `package.xml` 还没有 `<export>`，则添加该部分，并将 `<build_type>` 设为 `ament_cmake`，参见[示例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/package.xml#L43-L45)：

```xml
<export>
   <build_type>ament_cmake</build_type>
</export>
```

<span id="add-a-call-to-ament-package"></span>
### 添加 ament_package() 调用

在 `CMakeLists.txt` 末尾调用 `ament_package()`，参见[示例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/CMakeLists.txt#L127)：

```cmake
# Add this to the bottom of your CMakeLists.txt
ament_package()
```

<span id="update-find-package-calls"></span>
### 更新 find_package() 调用

将 `find_package(catkin COMPONENTS ...)` 替换为多个独立的 `find_package()` 调用，参见[示例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/CMakeLists.txt#L14-L18)。例如，将：

```
find_package(catkin REQUIRED COMPONENTS foo bar std_msgs)
find_package(baz REQUIRED)
```

改为：

```cmake
find_package(ament_cmake_ros REQUIRED)
find_package(foo REQUIRED)
find_package(bar REQUIRED)
find_package(std_msgs REQUIRED)
find_package(baz REQUIRED)
```

<span id="use-modern-cmake-targets"></span>
### 使用现代 CMake 目标

优先使用针对单个目标的 CMake 函数，以便软件包导出现代 CMake 目标。

如果 `CMakeLists.txt` 使用了 `include_directories()`，请删除这些调用：

```
# Delete calls to include_directories like this one!
include_directories(include ${catkin_INCLUDE_DIRS})
```

为软件包中的每个库添加 `target_include_directories()` 调用，参见[示例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/CMakeLists.txt#L24-L26)：

```cmake
target_include_directories(my_library PUBLIC
   "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
   "$<INSTALL_INTERFACE:include/${PROJECT_NAME}>")
```

将所有 `target_link_libraries()` 调用改为使用现代 CMake 目标。例如，ROS 1 软件包可能使用以下旧式 CMake 变量：

```
target_link_libraries(my_library ${catkin_LIBRARIES} ${baz_LIBRARIES})
```

请改为具体的现代 CMake 目标。如果依赖的是 `std_msgs` 等消息包，请使用 `${package_name_TARGETS}`：

```cmake
target_link_libraries(my_library PUBLIC foo::foo bar::bar ${std_msgs_TARGETS} baz::baz)
```

根据库使用依赖项的方式，选择 `PUBLIC` 或 `PRIVATE`，参见[示例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/CMakeLists.txt#L27-L31)：

- 下游用户也需要此依赖时，使用 `PUBLIC`，例如库的公开 API 使用了该依赖。
- 仅在库内部使用时，使用 `PRIVATE`。

<span id="replace-catkin-package-with-various-ament-cmake-calls"></span>
### 用多个 ament_cmake 调用替换 catkin_package()

假设 `CMakeLists.txt` 中有以下 `catkin_package` 调用：

```
catkin_package(
    INCLUDE_DIRS include
    LIBRARIES my_library
    CATKIN_DEPENDS foo bar std_msgs
    DEPENDS baz
)

install(TARGETS my_library
   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
   RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
)
```

<span id="replacing-catkin-package-include-dirs"></span>
#### 替换 catkin_package(INCLUDE_DIRS ...)

如果已经使用现代 CMake 目标和 `target_include_directories()`，就无需额外操作。下游用户依赖这些目标时，会自动获得相应头文件目录。

<span id="replacing-catkin-package-libraries"></span>
#### 替换 catkin_package(LIBRARIES ...)

使用 `ament_export_targets()` 和 `install(TARGETS ... EXPORT ...)` 替代 `LIBRARIES` 参数。

安装 `my_library` 目标时使用 `EXPORT` 关键字，参见[示例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/CMakeLists.txt#L37-L41)：

```cmake
install(TARGETS my_library EXPORT export_my_package
   ARCHIVE DESTINATION lib
   LIBRARY DESTINATION lib
   RUNTIME DESTINATION bin
)
```

以上是适合库目标的默认设置。如果软件包使用其他 `CATKIN_*_DESTINATION` 变量，请按下表转换：

| catkin | ament_cmake |
| --- | --- |
| `CATKIN_GLOBAL_BIN_DESTINATION` | `bin` |
| `CATKIN_GLOBAL_INCLUDE_DESTINATION` | `include` |
| `CATKIN_GLOBAL_LIB_DESTINATION` | `lib` |
| `CATKIN_GLOBAL_LIBEXEC_DESTINATION` | `lib` |
| `CATKIN_GLOBAL_SHARE_DESTINATION` | `share` |
| `CATKIN_PACKAGE_BIN_DESTINATION` | `lib/${PROJECT_NAME}` |
| `CATKIN_PACKAGE_INCLUDE_DESTINATION` | `include/${PROJECT_NAME}` |
| `CATKIN_PACKAGE_LIB_DESTINATION` | `lib` |
| `CATKIN_PACKAGE_SHARE_DESTINATION` | `share/${PROJECT_NAME}` |

添加 `ament_export_targets()` 调用，名称必须与 `EXPORT` 后的名称一致，参见[示例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/CMakeLists.txt#L124-L125)：

```cmake
ament_export_targets(export_my_package)
```

<span id="replacing-catkin-package-catkin-depends-depends"></span>
#### 替换 catkin_package(CATKIN_DEPENDS .. DEPENDS ..)

软件包使用者需要通过 `find_package()` 查找公开 API 使用的依赖。在 ROS 1 中，`CATKIN_DEPENDS` 和 `DEPENDS` 参数会为下游用户完成这一步。ROS 2 中改用 [`ament_export_dependencies`](https://github.com/ament/ament_cmake/blob/rolling/ament_cmake_export_dependencies/cmake/ament_export_dependencies.cmake)：

```cmake
ament_export_dependencies(
   foo
   bar
   std_msgs
   baz
)
```

<span id="generate-messages"></span>
### 生成消息

如果软件包同时包含 C++ 代码与 ROS 消息、服务或动作定义，可以考虑拆成两个包：一个只包含接口定义，另一个包含 C++ 代码。

在包含 ROS 消息的包的 `package.xml` 中添加以下依赖：

1. 对 `rosidl_default_generators` 的 `<buildtool_depend>`，参见[示例](https://github.com/ros2/common_interfaces/blob/d685509e9cb9f80bd320a347f2db954a73397ae7/std_msgs/package.xml#L19)：

```xml
<buildtool_depend>rosidl_default_generators</buildtool_depend>
```

2. 对 `rosidl_default_runtime` 的 `<exec_depend>`，参见[示例](https://github.com/ros2/common_interfaces/blob/d685509e9cb9f80bd320a347f2db954a73397ae7/std_msgs/package.xml#L22)：

```xml
<exec_depend>rosidl_default_runtime</exec_depend>
```

3. 组名为 `rosidl_interface_packages` 的 `<member_of_group>`，参见[示例](https://github.com/ros2/common_interfaces/blob/d685509e9cb9f80bd320a347f2db954a73397ae7/std_msgs/package.xml#L26)：

```xml
<member_of_group>rosidl_interface_packages</member_of_group>
```

在 `CMakeLists.txt` 中，将 `add_message_files`、`add_service_files` 和 `generate_messages` 替换为 [`rosidl_generate_interfaces`](https://github.com/ros2/rosidl/blob/rolling/rosidl_cmake/cmake/rosidl_generate_interfaces.cmake)。由于[这个问题](https://github.com/ros2/rosidl_typesupport/issues/120)，第一个参数必须是 `${PROJECT_NAME}`。

例如，将 ROS 1 中的：

```
add_message_files(DIRECTORY msg FILES FooBar.msg Baz.msg)
add_service_files(DIRECTORY srv FILES Ping.srv)

add_action_files(DIRECTORY action FILES DoPong.action)
generate_messages(
   DEPENDENCIES actionlib_msgs std_msgs geometry_msgs
)
```

改为以下内容，参见[示例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2_msgs/CMakeLists.txt#L18-L25)：

```cmake
rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/FooBar.msg"
  "msg/Baz.msg"
  "srv/Ping.srv"
  "action/DoPong.action"
  DEPENDENCIES actionlib_msgs std_msgs geometry_msgs
)
```

<span id="remove-references-to-the-devel-space"></span>
### 移除对 devel 空间的引用

删除所有对 *devel 空间*的引用，例如 `CATKIN_DEVEL_PREFIX`。ROS 2 没有对应的 devel 空间。

<span id="unit-tests"></span>
### 单元测试

如果软件包使用 [gtest](https://github.com/google/googletest)：

- 将 `CATKIN_ENABLE_TESTING` 替换为 `BUILD_TESTING`。
- 将 `catkin_add_gtest` 替换为 `ament_add_gtest`。
- 使用 `find_package()` 查找 `ament_cmake_gtest`，替代 `GTest`。

例如，将 ROS 1 中的测试配置：

```
if (CATKIN_ENABLE_TESTING)
  find_package(GTest REQUIRED)
  include_directories(${GTEST_INCLUDE_DIRS})
  catkin_add_gtest(my_test src/test/some_test.cpp)
  target_link_libraries(my_test
    # ...
    ${GTEST_LIBRARIES})
endif()
```

改为：

```cmake
if (BUILD_TESTING)
  find_package(ament_cmake_gtest REQUIRED)
  ament_add_gtest(my_test src/test/test_something.cpp)
  target_link_libraries(my_test
    #...
   )
endif()
```

在 `package.xml` 中添加 `<test_depend>ament_cmake_gtest</test_depend>`，参见[示例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/package.xml#L35)：

```xml
<test_depend>ament_cmake_gtest</test_depend>
```

<span id="linters"></span>
### 代码检查工具

ROS 2 的[代码风格指南](../../The-ROS2-Project/Contributing/Developer-Guide.md)与 ROS 1 不同。

如果选择遵循 ROS 2 风格，请在 `if(BUILD_TESTING)` 块中添加以下内容，启用自动代码检查测试：

```cmake
if(BUILD_TESTING)
   find_package(ament_lint_auto REQUIRED)
   ament_lint_auto_find_test_dependencies()
   # ...
endif()
```

在 `package.xml` 中添加以下依赖：

```xml
<test_depend>ament_lint_auto</test_depend>
<test_depend>ament_lint_common</test_depend>
```

<span id="update-source-code"></span>
## 更新源码

<span id="messages-services-and-actions"></span>
### 消息、服务和动作

ROS 2 消息、服务和动作的命名空间，会在包名之后分别增加 `msg`、`srv` 或 `action` 子命名空间。因此，头文件引入形式为 `#include <my_interfaces/msg/my_message.hpp>`，C++ 类型名则为 `my_interfaces::msg::MyMessage`。

消息结构体中提供了共享指针类型别名：`my_interfaces::msg::MyMessage::SharedPtr` 和 `my_interfaces::msg::MyMessage::ConstSharedPtr`。详情见[生成的 C++ 接口](https://design.ros2.org/articles/generated_interfaces_cpp.html)。

迁移时，需要对头文件引入语句进行以下修改：

- 在包名与消息数据类型之间插入 `msg` 子目录。
- 将文件名从驼峰形式改为下划线分隔。
- 将 `.h` 扩展名改为 `.hpp`。

```cpp
// ROS 1 style is in comments, ROS 2 follows, uncommented.
// # include <geometry_msgs/PointStamped.h>
#include <geometry_msgs/msg/point_stamped.hpp>

// geometry_msgs::PointStamped point_stamped;
geometry_msgs::msg::PointStamped point_stamped;
```

代码中所有相应类型的使用处都需要加入 `msg` 命名空间。

<span id="use-of-service-objects"></span>
### 使用服务对象

ROS 2 的服务回调不再返回布尔值。发生失败时，建议抛出异常，而不是返回 false。

```cpp
// ROS 1 style is in comments, ROS 2 follows, uncommented.
// #include "nav_msgs/GetMap.h"
#include "nav_msgs/srv/get_map.hpp"

// bool service_callback(
//   nav_msgs::GetMap::Request & request,
//   nav_msgs::GetMap::Response & response)
void service_callback(
  const std::shared_ptr<nav_msgs::srv::GetMap::Request> request,
  std::shared_ptr<nav_msgs::srv::GetMap::Response> response)
{
  // ...
  // return true;  // or false for failure
}
```

<span id="usages-of-ros-time"></span>
### ros::Time 的用法

将所有 `ros::Time` 替换为 `rclcpp::Time`。

如果消息或代码使用 `std_msgs::Time`：

- 将该类型替换为 `builtin_interfaces::msg::Time`。
- 将 `std_msgs/time.h` 头文件替换为 `builtin_interfaces/msg/time.hpp`。
- 将 `std_msgs::Time` 的 `nsec` 字段改为 `builtin_interfaces::msg::Time` 的 `nanosec` 字段。

<span id="usages-of-ros-rate"></span>
### ros::Rate 的用法

ROS 2 提供等价的 `rclcpp::Rate`，基本可以直接替代 `ros::Rate`。

<span id="boost"></span>
### Boost

以前由 Boost 提供的许多功能已加入 C++ 标准库，因此应尽量使用这些新的标准功能，避免依赖 Boost。

<span id="shared-pointers"></span>
#### 共享指针

将 Boost 共享指针替换为标准 C++ 共享指针：

- 将 `#include <boost/shared_ptr.hpp>` 替换为 `#include <memory>`。
- 将 `boost::shared_ptr` 替换为 `std::shared_ptr`。

也可能需要转换 `weak_ptr` 等相关类型。

此外，建议使用 `using` 代替 `typedef`，因为它更适合模板场景，详情见[此说明](https://stackoverflow.com/questions/10747810/what-is-the-difference-between-typedef-and-using-in-c11)。

<span id="thread-mutexes"></span>
#### 线程与互斥锁

ROS 代码中还经常使用 `boost::thread` 中的互斥锁：

- 将 `boost::mutex::scoped_lock` 替换为 `std::unique_lock<std::mutex>`。
- 将 `boost::mutex` 替换为 `std::mutex`。
- 将 `#include <boost/thread/mutex.hpp>` 替换为 `#include <mutex>`。

<span id="unordered-map"></span>
#### 无序映射

- 将 `#include <boost/unordered_map.hpp>` 替换为 `#include <unordered_map>`。
- 将 `boost::unordered_map` 替换为 `std::unordered_map`。

<span id="function"></span>
#### function

- 将 `#include <boost/function.hpp>` 替换为 `#include <functional>`。
- 将 `boost::function` 替换为 `std::function`。
