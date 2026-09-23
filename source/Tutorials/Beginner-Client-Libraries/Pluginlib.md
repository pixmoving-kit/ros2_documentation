<span id="creating-and-using-plugins-c"></span>
# 创建和使用插件（C++）

**目标：** 学习使用 `pluginlib` 创建并加载简单插件。

**教程级别：** 初学者

**预计用时：** 20 分钟

<span id="background"></span>
## 背景

本教程改编自 [pluginlib 文档](http://wiki.ros.org/pluginlib)和[编写并使用简单插件教程](http://wiki.ros.org/pluginlib/Tutorials/Writing%20and%20Using%20a%20Simple%20Plugin)。

`pluginlib` 是一个 C++ 库，用于在 ROS 软件包中加载和卸载插件。插件是可以从运行时库（共享对象或动态链接库）中动态加载的类。使用 `pluginlib` 时，应用程序无需显式链接包含这些类的库，也不必预先知道该库或类定义所在的头文件；`pluginlib` 可以在运行时打开包含已导出类的库。这样，即使没有应用程序源代码，也能通过插件扩展或修改应用行为。

<span id="prerequisites"></span>
## 前提条件

需要具备基本的 C++ 知识，并已成功[安装 ROS 2](../../Installation.md)。

<span id="tasks"></span>
## 操作步骤

本教程创建两个软件包：一个定义基类，另一个提供插件。基类表示通用的多边形，插件则定义具体形状。

<span id="create-the-base-class-package"></span>
### 1 创建基类软件包

在 `ros2_ws/src` 中运行以下命令，创建新的空软件包：

```console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 --dependencies pluginlib --node-name area_node polygon_base
```

用编辑器打开 `ros2_ws/src/polygon_base/include/polygon_base/regular_polygon.hpp`，写入：

```C++
#ifndef POLYGON_BASE_REGULAR_POLYGON_HPP
#define POLYGON_BASE_REGULAR_POLYGON_HPP

namespace polygon_base
{
  class RegularPolygon
  {
    public:
      virtual void initialize(double side_length) = 0;
      virtual double area() = 0;
      virtual ~RegularPolygon(){}

    protected:
      RegularPolygon(){}
  };
}  // namespace polygon_base

#endif  // POLYGON_BASE_REGULAR_POLYGON_HPP
```

这段代码定义了名为 `RegularPolygon` 的抽象类。注意其中的 `initialize` 方法：`pluginlib` 要求类具有无参数构造函数，所以类需要的参数通过初始化方法传给对象。

为了让其他类使用该头文件，需要将其导出为接口库。编辑 `~/ros2_ws/src/polygon_base/CMakeLists.txt`，在 `find_package(pluginlib REQUIRED)` 后添加：

```cmake
# Library (this will be used as the base class for plugins)
add_library(${PROJECT_NAME} INTERFACE)
add_library(${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
target_compile_features(${PROJECT_NAME} INTERFACE c_std_99 cxx_std_17)
target_include_directories(${PROJECT_NAME} INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include/${PROJECT_NAME}>
)
target_link_libraries(${PROJECT_NAME} INTERFACE ${pluginlib_TARGETS})

# Install headers
install(DIRECTORY include/
  DESTINATION include/${PROJECT_NAME}
)

# Install library and export targets
install(TARGETS ${PROJECT_NAME}
  EXPORT export_${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)
install(EXPORT export_${PROJECT_NAME}
  NAMESPACE ${PROJECT_NAME}::
  DESTINATION share/${PROJECT_NAME}/cmake
)
```

再在 `ament_package` 之前添加：

```cmake
# Export old-style CMake variables
ament_export_include_directories(
  include
)

# Export modern CMake targets
ament_export_targets(
  export_${PROJECT_NAME}
)
```

稍后回到这个软件包编写测试节点。

<span id="create-the-plugin-package"></span>
### 2 创建插件软件包

接下来为抽象类编写两个具体实现。在 `ros2_ws/src` 中创建第二个空软件包：

```console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 --dependencies polygon_base pluginlib --library-name polygon_plugins polygon_plugins
```

<span id="source-code-for-the-plugins"></span>
#### 2.1 插件源代码

编辑 `ros2_ws/src/polygon_plugins/src/polygon_plugins.cpp`，写入：

```C++
#include <polygon_base/regular_polygon.hpp>
#include <cmath>

namespace polygon_plugins
{
  class Square : public polygon_base::RegularPolygon
  {
    public:
      void initialize(double side_length) override
      {
        side_length_ = side_length;
      }

      double area() override
      {
        return side_length_ * side_length_;
      }

    protected:
      double side_length_;
  };

  class Triangle : public polygon_base::RegularPolygon
  {
    public:
      void initialize(double side_length) override
      {
        side_length_ = side_length;
      }

      double area() override
      {
        return 0.5 * side_length_ * getHeight();
      }

      double getHeight()
      {
        return sqrt((side_length_ * side_length_) - ((side_length_ / 2) * (side_length_ / 2)));
      }

    protected:
      double side_length_;
  };
}

#include <pluginlib/class_list_macros.hpp>

PLUGINLIB_EXPORT_CLASS(polygon_plugins::Square, polygon_base::RegularPolygon)
PLUGINLIB_EXPORT_CLASS(polygon_plugins::Triangle, polygon_base::RegularPolygon)
```

`Square` 和 `Triangle` 的实现很直接：保存边长，再用它计算面积。只有最后三行与 pluginlib 特有的机制有关，它们通过宏将这两个类注册为插件。

`PLUGINLIB_EXPORT_CLASS` 的两个参数分别是：

1. 插件类的完全限定类型名，例如 `polygon_plugins::Square`。
2. 基类的完全限定类型名，这里是 `polygon_base::RegularPolygon`。

<span id="plugin-declaration-xml"></span>
#### 2.2 插件声明 XML

上述步骤让库加载后能够创建插件实例，但插件加载器还需要知道如何找到该库，以及该引用库中的哪些类。因此还需创建 XML 文件，并配合导出声明，将插件所需的信息提供给 ROS 工具链。

创建 `ros2_ws/src/polygon_plugins/plugins.xml`：

```XML
<library path="polygon_plugins">
  <class type="polygon_plugins::Square" base_class_type="polygon_base::RegularPolygon">
    <description>This is a square plugin.</description>
  </class>
  <class type="polygon_plugins::Triangle" base_class_type="polygon_base::RegularPolygon" name="awesome_triangle">
    <description>This is a triangle plugin.</description>
  </class>
</library>
```

需要注意以下内容：

- `library` 标签指定包含待导出插件的库的相对路径。在 ROS 2 中只需填写库名；ROS 1 中则包含 `lib`，有时是 `lib/lib` 前缀，例如 `lib/libpolygon_plugins`。
- `class` 标签声明从库中导出的插件。`type` 是插件的完全限定类型名，例如 `polygon_plugins::Square`；`base_class_type` 是基类的完全限定类型名，这里是 `polygon_base::RegularPolygon`；`description` 描述插件及其用途；可选的 `name` 是供类加载器查找插件的名称，也可以理解为别名。

<span id="cmake-plugin-declaration"></span>
#### 2.3 在 CMake 中声明插件

最后通过 `CMakeLists.txt` 导出插件。与 ROS 1 不同，ROS 1 是通过 `package.xml` 完成导出的。在 `ros2_ws/src/polygon_plugins/CMakeLists.txt` 的 `find_package(pluginlib REQUIRED)` 后添加：

```cmake
pluginlib_export_plugin_description_file(polygon_base plugins.xml)
```

`pluginlib_export_plugin_description_file` 的参数分别是：

1. 基类所在的软件包，这里是 `polygon_base`。
2. 插件声明 XML 文件的相对路径，这里是 `plugins.xml`。

<span id="use-the-plugins"></span>
### 3 使用插件

任何软件包都可以使用插件，这里在基类软件包中演示。将 `ros2_ws/src/polygon_base/src/area_node.cpp` 修改为：

```C++
#include <pluginlib/class_loader.hpp>
#include <polygon_base/regular_polygon.hpp>

int main(int argc, char** argv)
{
  // To avoid unused parameter warnings
  (void) argc;
  (void) argv;

  pluginlib::ClassLoader<polygon_base::RegularPolygon> poly_loader("polygon_base", "polygon_base::RegularPolygon");

  try
  {
    std::shared_ptr<polygon_base::RegularPolygon> triangle = poly_loader.createSharedInstance("awesome_triangle");
    triangle->initialize(10.0);

    std::shared_ptr<polygon_base::RegularPolygon> square = poly_loader.createSharedInstance("polygon_plugins::Square");
    square->initialize(10.0);

    printf("Triangle area: %.2f\n", triangle->area());
    printf("Square area: %.2f\n", square->area());
  }
  catch(pluginlib::PluginlibException& ex)
  {
    printf("The plugin failed to load for some reason. Error: %s\n", ex.what());
  }

  return 0;
}
```

关键是理解定义在 [class_loader.hpp 头文件](https://github.com/ros/pluginlib/blob/ros2/pluginlib/include/pluginlib/class_loader.hpp)中的 `ClassLoader`：

- 模板参数是基类，即 `polygon_base::RegularPolygon`。
- 第一个构造参数是基类所在的软件包名称字符串，即 `polygon_base`。
- 第二个构造参数是插件基类的完全限定类型名字符串，即 `polygon_base::RegularPolygon`。

创建类实例有多种方式，本例使用共享指针。调用 `createSharedInstance` 时传入插件标识即可：既可以是插件类的完全限定类型名，即声明 XML 中的 `type` 属性，例如 `polygon_plugins::Square`；也可以是可选的别名，即 `name` 属性，例如 `awesome_triangle`。

这里定义节点的 `polygon_base` 软件包不需要依赖 `polygon_plugins` 中的类。插件会被动态加载，无需声明这种依赖。本例将插件名称直接写在代码中，也可以通过参数等方式动态选择。

<span id="build-and-run"></span>
### 4 构建并运行

返回工作空间根目录 `ros2_ws`，构建新软件包：

```console
$ colcon build --packages-select polygon_base polygon_plugins
```

在 `ros2_ws` 中加载环境设置文件。

**Linux**

```console
$ source install/setup.bash
```

**macOS**

```console
$ . install/setup.bash
```

**Windows**

```console
$ call install/setup.bat
```

`ros2 plugin` 命令由 `ros2plugin` 软件包提供。如果通过 Debian 软件包安装 ROS 2 后没有此命令，可以运行：

```console
$ sudo apt install ros-rolling-ros2plugin
```

列出插件，确认它们注册成功：

```console
$ ros2 plugin list
polygon_plugins:
   Plugin(name='polygon_plugins::Square', type='polygon_plugins::Square', base='polygon_base::RegularPolygon')
   Plugin(name='polygon_plugins::Triangle', type='polygon_plugins::Triangle', base='polygon_base::RegularPolygon')
```

运行节点：

```console
$ ros2 run polygon_base area_node
Triangle area: 43.30
Square area: 100.00
```

<span id="summary"></span>
## 小结

你已经编写并使用了自己的第一组插件。
