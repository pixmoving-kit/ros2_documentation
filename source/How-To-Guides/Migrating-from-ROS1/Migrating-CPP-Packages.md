---
translation_status: machine_translated
source: How-To-Guides/Migrating-from-ROS1/Migrating-CPP-Packages.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="migrating-c-packages-reference"></span>

# C++ 软件包迁移参考

本页面显示如何将 C++ 软件包的部件从 ROS 1 迁移到 ROS 2。 如果这是您第一次迁移 C++ 软件包, 请阅读 [C++ 迁移示例](Migrating-CPP-Package-Example.md) 首先,随后,在您移动自己的软件包时,请使用此页面作为参考。

<span id="build-tool"></span>

## 构建工具

而不是使用 `catkin_make`, `catkin_make_isolated` 或 时 间 `catkin build` ROS 2 使用命令行工具 [colcon](https://design.ros2.org/articles/build_tool.html) 来构建和安装一组软件包。 [初学者教程](../../Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.md) 开始于 `colcon`.

<span id="update-your-cmakelists-txt-to-use-ament-cmake"></span>

## 更新您的 `CMakeLists.txt` 用于: *ament_cmake*

ROS 2 C++ 软件包的使用 [CMake](https://cmake.org/) 提供方便的功能 [ament_cmake](https://index.ros.org/p/ament_cmake/)。应用以下修改来使用 `ament_cmake` 改为 `catkin`.

<span id="require-a-newer-version-of-cmake"></span>

### 需要更新 CMake 版本

ROS 2 依赖于较ROS 1 使用的更新版本的 CMake 。 寻找 ROS 发行时您想要支持的最小版本 CMake 。 [REP 2000 环境方案](https://reps.openrobotics.org/rep-2000/),然后在您的顶端使用该版本 `CMakeLists.txt`。例如, [3.14.4 最低建议支持ROS Humble](https://reps.openrobotics.org/rep-2000/#humble-hawksbill-may-2022-may-2027).

``` default
cmake_minimum_required(VERSION 3.14.4)
```

<span id="set-the-build-type-to-ament-cmake"></span>

### 将构建类型设定为 ament\_ cmake

删除任何依赖 `catkin` 从你的 `package.xml`

``` default
# Remove this!
<buildtool_depend>catkin</buildtool_depend>
```

添加一个新的依赖 `ament_cmake_ros` ([实例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/package.xml#L25)):

``` xml
<buildtool_depend>ament_cmake_ros</buildtool_depend>
```

添加一个 `<export>` 区域 `package.xml` 如果它还没有一个。 `<build_type>` 改为: `ament_cmake` ([实例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/package.xml#L43-L45))

``` xml
<export>
   <build_type>ament_cmake</build_type>
</export>
```

<span id="add-a-call-to-ament-package"></span>

### 添加一个呼叫到 `ament_package()`

插入一个呼叫到 `ament_package()` 在你的底边 `CMakeLists.txt` ([实例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/CMakeLists.txt#L127))

``` cmake
# Add this to the bottom of your CMakeLists.txt
ament_package()
```

<span id="update-find-package-calls"></span>

### 更新 `find_package()` 电话

替换 `find_package(catkin COMPONENTS ...)` 与个人通话 `find_package()` 电话(电话)[实例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/CMakeLists.txt#L14-L18)):

例如,改变这个:

``` default
find_package(catkin REQUIRED COMPONENTS foo bar std_msgs)
find_package(baz REQUIRED)
```

为此:

``` cmake
find_package(ament_cmake_ros REQUIRED)
find_package(foo REQUIRED)
find_package(bar REQUIRED)
find_package(std_msgs REQUIRED)
find_package(baz REQUIRED)
```

<span id="use-modern-cmake-targets"></span>

### 使用现代 CMake 目标

倾向于使用每个目标 CMake 函数,以便您的软件包可以导出现代 CMake 目标 。

狦 `CMakeLists.txt` 用途 `include_directories()`,然后删除这些电话。

``` default
# Delete calls to include_directories like this one!
include_directories(include ${catkin_INCLUDE_DIRS})
```

添加一个呼叫 `target_include_directories()` 用于您软件包中的每个库( Y)[实例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/CMakeLists.txt#L24-L26)).

``` cmake
target_include_directories(my_library PUBLIC
   "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
   "$<INSTALL_INTERFACE:include/${PROJECT_NAME}>")
```

全部改变 `target_link_libraries()` 调用现代 CMake 目标。例如,如果你在 ROS 1 中的软件包使用这种老式的标准 CMake 变量。

``` default
target_link_libraries(my_library ${catkin_LIBRARIES} ${baz_LIBRARIES})
```

然后修改为使用特定的现代 CMake 目标。使用 `${package_name_TARGETS}` 如果您所依赖的软件包是一个消息包, 例如: `std_msgs`.

``` cmake
target_link_libraries(my_library PUBLIC foo::foo bar::bar ${std_msgs_TARGETS} baz::baz)
```

选择 `PUBLIC` 或 时 间 `PRIVATE` 基于您的库如何使用依赖性( Y)[实例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/CMakeLists.txt#L27-L31)).

- 使用 `PUBLIC` 如果下游用户需要依赖,例如您的库公共API会使用它。

- 使用 `PRIVATE` 如果依赖仅在您的库内部使用。

<span id="replace-catkin-package-with-various-ament-cmake-calls"></span>

### 替换 `catkin_package()` 使用各种调用(\_C)

想象一下你的样子 `CMakeLists.txt` 有电话打给 `catkin_package` 像这样:

``` default
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

#### 替换 `catkin_package(INCLUDE_DIRS ...)`

如果你使用了现代的 CMake 目标 `target_include_directories()`,您不需要再做任何事情。下游用户会根据您现代的 CMake 目标获得包含目录 。

<span id="replacing-catkin-package-libraries"></span>

#### 替换 `catkin_package(LIBRARIES ...)`

使用 `ament_export_targets()` 财务报告和财务报告 `install(TARGETS ... EXPORT ...)` 替换 `LIBRARIES` 参数。

使用该 `EXPORT` 安装您时的关键字 `my_library` 目标([实例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/CMakeLists.txt#L37-L41)).

``` cmake
install(TARGETS my_library EXPORT export_my_package
   ARCHIVE DESTINATION lib
   LIBRARY DESTINATION lib
   RUNTIME DESTINATION bin
)
```

以上是库目标的良好默认值。 如果您的软件包使用了不同的 `CATKIN_*_DESTINATION` 变量,将其转换如下:

| **猫金**                           | **ament_cmake**          |
|------------------------------------|--------------------------|
| CATKIN_GLOBAL_BIN_DESTINATION      | 弹夹                     |
| CATKIN_GLOBAL_INCLUDE_DESTINATION  | 包含                     |
| CATKIN_GLOBAL_LIB_DESTINATION      | 独立                     |
| CATKIN_GLOBAL_LIBEXEC_DESTINATION  | 独立                     |
| CATKIN_GLOBAL_SHARE_DESTINATION    | 份额                     |
| CATKIN_PACKAGE_BIN_DESTINATION     | lib/\${PROJECT_NAME}     |
| CATKIN_PACKAGE_INCLUDE_DESTINATION | include/\${PROJECT_NAME} |
| CATKIN_PACKAGE_LIB_DESTINATION     | 独立                     |
| CATKIN_PACKAGE_SHARE_DESTINATION   | share/\${PROJECT_NAME}   |

添加一个呼叫到 `ament_export_targets()` 跟你给的同名名字 `EXPORT` 关键词( E)[实例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/CMakeLists.txt#L124-L125)).

``` cmake
ament_export_targets(export_my_package)
```

<span id="replacing-catkin-package-catkin-depends-depends"></span>

#### 替换 `catkin_package(CATKIN_DEPENDS .. DEPENDS ..)`

您的软件包的用户必须 `find_package()` 您软件包的公共 API 使用的依赖性 。 在 ROS 1 中, 下游用户使用 `CATKIN_DEPENDS` 财务报告和财务报告 `DEPENDS` 参数。使用 [ament_export_dependencies](https://github.com/ament/ament_cmake/blob/rolling/ament_cmake_export_dependencies/cmake/ament_export_dependencies.cmake) 在ROS 2中做到这一点。

``` cmake
ament_export_dependencies(
   foo
   bar
   std_msgs
   baz
)
```

<span id="generate-messages"></span>

### 生成信件

如果您的软件包同时包含 C++ 代码和ROS 消息、服务或动作定义,那么考虑将其分为两个软件包:

- 只包含ROS消息、服务和/或动作定义的软件包

- C++ 代码的软件包

添加以下依赖关系到 `package.xml` 中包含 ROS 消息的软件包 :

1.  添加一个 `<buildtool_depend>` 打开 `rosidl_default_generators` ([实例](https://github.com/ros2/common_interfaces/blob/d685509e9cb9f80bd320a347f2db954a73397ae7/std_msgs/package.xml#L19))

    ``` xml
    <buildtool_depend>rosidl_default_generators</buildtool_depend>
    ```

2.  添加一个 `<exec_depend>` 打开 `rosidl_default_runtime` ([实例](https://github.com/ros2/common_interfaces/blob/d685509e9cb9f80bd320a347f2db954a73397ae7/std_msgs/package.xml#L22))

    ``` xml
    <exec_depend>rosidl_default_runtime</exec_depend>
    ```

3.  添加一个 `<member_of_group>` 带有组名称的标签 `rosidl_interface_packages` ([实例](https://github.com/ros2/common_interfaces/blob/d685509e9cb9f80bd320a347f2db954a73397ae7/std_msgs/package.xml#L26))

    ``` xml
    <member_of_group>rosidl_interface_packages</member_of_group>
    ```

在你身边 `CMakeLists.txt`,取代援引 `add_message_files`, `add_service_files` 财务报告和财务报告 `generate_messages` 与 [rosidl_generate_interfaces](https://github.com/ros2/rosidl/blob/rolling/rosidl_cmake/cmake/rosidl_generate_interfaces.cmake)。第一个论点必须是 `${PROJECT_NAME}` 应付 [此错误](https://github.com/ros2/rosidl_typesupport/issues/120).

例如,如果你的ROS 1 包看起来像这样:

``` default
add_message_files(DIRECTORY msg FILES FooBar.msg Baz.msg)
add_service_files(DIRECTORY srv FILES Ping.srv)

add_action_files(DIRECTORY action FILES DoPong.action)
generate_messages(
   DEPENDENCIES actionlib_msgs std_msgs geometry_msgs
)
```

那就换成这个[实例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2_msgs/CMakeLists.txt#L18-L25))

``` cmake
rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/FooBar.msg"
  "msg/Baz.msg"
  "srv/Ping.srv"
  "action/DoPong.action"
  DEPENDENCIES actionlib_msgs std_msgs geometry_msgs
)
```

<span id="remove-references-to-the-devel-space"></span>

### 删除显示空格的引用

删除任何引用 *缩放空间* 例如, `CATKIN_DEVEL_PREFIX`。没有相当于 *缩放空间* 在罗斯2号线上

<span id="unit-tests"></span>

### 单位测试

如果您的软件包使用 [测试](https://github.com/google/googletest) 然后:

- 替换 `CATKIN_ENABLE_TESTING` 与 `BUILD_TESTING`.

- 替换 `catkin_add_gtest` 与 `ament_add_gtest`.

- 添加一个 `find_package()` (单位:千美元) `ament_cmake_gtest` 改为 `GTest`

例如,如果你的ROS 1 包增加了这样的测试:

``` default
if (CATKIN_ENABLE_TESTING)
  find_package(GTest REQUIRED)
  include_directories(${GTEST_INCLUDE_DIRS})
  catkin_add_gtest(my_test src/test/some_test.cpp)
  target_link_libraries(my_test
    # ...
    ${GTEST_LIBRARIES})
endif()
```

那就换成这样:

``` CMake
if (BUILD_TESTING)
  find_package(ament_cmake_gtest REQUIRED)
  ament_add_gtest(my_test src/test/test_something.cpp)
  target_link_libraries(my_test
    #...
   )
endif()
```

添加 `<test_depend>ament_cmake_gtest</test_depend>` 给您的 `package.xml` ([实例](https://github.com/ros2/geometry2/blob/d85102217f692746abea8546c8e41f0abc95c8b8/tf2/package.xml#L35)).

``` xml
<test_depend>ament_cmake_gtest</test_depend>
```

<span id="linters"></span>

### 林特尔

ROS 2 代码 [样式指南](../../The-ROS2-Project/Contributing/Developer-Guide.md) 与ROS 1 不同.

如果您选择遵循ROS 2 样式指南,那么打开自动穿插测试,在其中添加这些线条 `if(BUILD_TESTING)` 块 :

``` cmake
if(BUILD_TESTING)
   find_package(ament_lint_auto REQUIRED)
   ament_lint_auto_find_test_dependencies()
   # ...
endif()
```

将以下依赖性添加到您的 `package.xml`:

``` xml
<test_depend>ament_lint_auto</test_depend>
<test_depend>ament_lint_common</test_depend>
```

<span id="update-source-code"></span>

## 更新源代码

<span id="messages-services-and-actions"></span>

### 信息、服务和行动

ROS 2 信件、服务和动作的命名空间使用一个子名称空间(`msg`, `srv`,或 `action`在软件包名称之后。因此,包含的内容看起来像 : `#include <my_interfaces/msg/my_message.hpp>`。然后将 C++ 类型命名为: `my_interfaces::msg::MyMessage`.

共享指针类型作为消息结构中的类型保护符提供 : `my_interfaces::msg::MyMessage::SharedPtr` (a) 与《公约》有关的其他事项; `my_interfaces::msg::MyMessage::ConstSharedPtr`.

详情请见有关下列事项的文章: [生成的 C++ 接口](https://design.ros2.org/articles/generated_interfaces_cpp.html).

移徙需要改变方式包括:

- 插入子文件夹 `msg` 介于软件包名称和消息数据类型之间

- 从 CamelCase 更改包含的文件名以强调分隔

- 更改从 `*.h` 改为: `*.hpp`

``` cpp
// ROS 1 style is in comments, ROS 2 follows, uncommented.
// # include <geometry_msgs/PointStamped.h>
#include <geometry_msgs/msg/point_stamped.hpp>

// geometry_msgs::PointStamped point_stamped;
geometry_msgs::msg::PointStamped point_stamped;
```

迁移需要代码来插入 `msg` 所有实例中的命名空间。

<span id="use-of-service-objects"></span>

### 服务对象的使用

ROS 2 中的服务调用没有布尔返回值。 建议放弃例外, 而不是在失败时错误返回 。

``` cpp
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

### 罗斯的使用:时间

用于: `ros::Time`:

- 替换所有实例 `ros::Time` 与 `rclcpp::Time`

- 如果您的消息或代码使用 std\_ msgs :: 时间 :

  - 将所有 std\_ msgs 的例转换为: 时间到内建\_ 界面: : msg: 时间

  - 全部转换 `#include "std_msgs/time.h` 改为: `#include "builtin_interfaces/msg/time.hpp"`

  - 使用 std\_ msgs 转换所有实例:: 时间字段 `nsec` 到内建 \_ 界面: : msg: 时间字段 `nanosec`

<span id="usages-of-ros-rate"></span>

### 罗斯的用途: 时间

有一个等效的类型 `rclcpp::Rate` 对象,它基本上是替换的下降 `ros::Rate`.

<span id="boost"></span>

### 脚步

Boost 先前提供的许多功能已经整合到 C++ 标准库中。 因此,我们希望利用新的核心功能,尽可能避免依赖助推。

<span id="shared-pointers"></span>

#### 共享指针

将共享指针从助推器切换到标准的C++,以替换下列实例:

- `#include <boost/shared_ptr.hpp>` 与 `#include <memory>`

- `boost::shared_ptr` 与 `std::shared_ptr`

也可能有一些变体,例如: `weak_ptr` 您也想要转换它。

也建议采用下列做法: `using` 改为 `typedef`. `using` 具有在模板逻辑中更好地工作的能力。 [看这里](https://stackoverflow.com/questions/10747810/what-is-the-difference-between-typedef-and-using-in-c11)

<span id="thread-mutexes"></span>

#### Thread/Mutexes

ROS编码库中常用的助推器的另一个常见部分是: `boost::thread`.

- 替换 `boost::mutex::scoped_lock` 与 `std::unique_lock<std::mutex>`

- 替换 `boost::mutex` 与 `std::mutex`

- 替换 `#include <boost/thread/mutex.hpp>` 与 `#include <mutex>`

<span id="unordered-map"></span>

#### 未排序的地图

替换 :

- `#include <boost/unordered_map.hpp>` 与 `#include <unordered_map>`

- `boost::unordered_map` 与 `std::unordered_map`

<span id="function"></span>

#### 函数

替换 :

- `#include <boost/function.hpp>` 与 `#include <functional>`

- `boost::function` 与 `std::function`
