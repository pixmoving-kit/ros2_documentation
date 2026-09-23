---
translation_status: machine_translated
source: Tutorials/Beginner-Client-Libraries/Pluginlib.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="creating-and-using-plugins-c"></span>

# 创建与使用插件（C++）

**目标：** 学会使用简单的插件创建和装入 `pluginlib`.

**教程级别：** 入门

**用时：** 20分钟

<span id="background"></span>

## 背景

此教程来源于 <http://wiki.ros.org/pluginlib> 财务报告和财务报告 [编写和使用简单的插件教程](http://wiki.ros.org/pluginlib/Tutorials/Writing%20and%20Using%20a%20Simple%20Plugin).

`pluginlib` 用于从 ROS 软件包内装入和卸载插件的 C++ 库。 插件是动态可加载的类,这些类是从运行时库( 共享对象, 动态链接库) 中装入的。 有了 插件lib , 您不必将您的应用程序与包含类的库明确连接起来 。 `pluginlib` 可以在任何时间打开包含导出类的库,而应用程序事先对库或包含类定义的页眉文件有任何了解。插件对于扩展/修改应用程序行为是有用的,而不需要应用程序源代码。

<span id="prerequisites"></span>

## 前提条件

此教程假设 C++ 基本知识, 您已经成功 [已安装 ROS 2](../../Installation.md).

<span id="tasks"></span>

## 操作步骤

在此教程中, 您将创建两个新软件包, 一个是定义基类, 另一个是提供插件。 基类将定义一个通用的多边形类, 然后我们的插件将定义特定的形状 。

<span id="create-the-base-class-package"></span>

### 1 创建基础类软件包

在您的空包中创建新包 `ros2_ws/src` 带有以下命令的文件夹 :

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 --dependencies pluginlib --node-name area_node polygon_base
```

打开您最喜欢的编辑器, 编辑 `ros2_ws/src/polygon_base/include/polygon_base/regular_polygon.hpp`,并粘贴其中的以下内容:

``` C++
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

上面的代码创建了一个抽象类,叫做 `RegularPolygon`。需要注意的一件事是初始化方法的存在。 `pluginlib`,需要一个没有参数的构造器,所以如果需要任何参数到类,我们使用初始化方法将它们传递给对象.

我们需要通过导出此信头作为界面库来让其他类获得此信头。 要做到这一点, 请打开 `~/ros2_ws/src/polygon_base/CMakeLists.txt` 编辑并添加以下行 `find_package(pluginlib REQUIRED)` 命令 :

``` cmake
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

并在此命令之前添加 `ament_package` 命令 :

``` cmake
# Export old-style CMake variables
ament_export_include_directories(
  include
)

# Export modern CMake targets
ament_export_targets(
  export_${PROJECT_NAME}
)
```

我们稍后会回到这个包里来写测试节点.

<span id="create-the-plugin-package"></span>

### 2 创建插件包

现在我们要写两个非虚拟的抽象类执行。 在您创建第二个空包 `ros2_ws/src` 带有以下命令的文件夹 :

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 --dependencies polygon_base pluginlib --library-name polygon_plugins polygon_plugins
```

<span id="source-code-for-the-plugins"></span>

#### 2.1 插件的源代码

打开 `ros2_ws/src/polygon_plugins/src/polygon_plugins.cpp` 用于编辑,并粘贴其内部如下:

``` C++
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

方块和三角类的落实相当直接:保存边长,并用它来计算区域。唯一一个是插件lib 特定的部分是最后三行,它引用了一些将类作为实际插件注册的神奇宏。 `PLUGINLIB_EXPORT_CLASS` 宏 :

1.  插件类的完全合格类型,在这种情况下, `polygon_plugins::Square`.

2.  完全合格的基类类型,在这种情况下, `polygon_base::RegularPolygon`.

<span id="plugin-declaration-xml"></span>

#### 2.2 插件声明 XML

以上步骤允许在装入库时创建插件实例, 但插件加载器仍然需要找到该库的方法, 并知道该库内要引用什么 。 为此, 我们还将创建 XML 文件, 该文件连同包中的特殊导出行一起, 向 ROS 工具链提供我们插件的所有必要信息 。

创建 `ros2_ws/src/polygon_plugins/plugins.xml` 使用下列代码:

``` XML
<library path="polygon_plugins">
  <class type="polygon_plugins::Square" base_class_type="polygon_base::RegularPolygon">
    <description>This is a square plugin.</description>
  </class>
  <class type="polygon_plugins::Triangle" base_class_type="polygon_base::RegularPolygon" name="awesome_triangle">
    <description>This is a triangle plugin.</description>
  </class>
</library>
```

有几个事情需要注意:

1.  那个... `library` 标记给出包含要导出插件的库的相对路径。 在 ROS 2 中, 它只是库的名称。 在 ROS 1 中, 它包含前缀 `lib` 有时 `lib/lib` (i.e. `lib/libpolygon_plugins`)),但这里比较简单.

2.  那个... `class` 标签宣布要从库中导出一个插件。 让我们通过它的参数 :

> - `type`: 插件的完全合格类型。 对我们来说, 就是 `polygon_plugins::Square`.
>
> - `base_class`: 插件的完全合格的基类类型。 对我们来说, 就是 `polygon_base::RegularPolygon`.
>
> - `description`: 插件及其作用的描述.
>
> - `name` (可选):一个由类加载器使用的查询名称(即魔法名称).

<span id="cmake-plugin-declaration"></span>

#### 2.3 CMake 插件声明

最后一个步骤是将您的插件导出通过 `CMakeLists.txt`。这与ROS 1有所变化,因为该出口是通过以下方式进行的: `package.xml`中添加以下一行 `ros2_ws/src/polygon_plugins/CMakeLists.txt` 行读完后 `find_package(pluginlib REQUIRED)`:

``` cmake
pluginlib_export_plugin_description_file(polygon_base plugins.xml)
```

B. 对法院的论据 `pluginlib_export_plugin_description_file` 命令为:

1.  带基类的包,即: `polygon_base`.

2.  插件声明的相对路径 xml,即 。 `plugins.xml`.

<span id="use-the-plugins"></span>

### 3 使用插件

现在该是使用插件的时候了。 这可以在任何软件包中完成, 但在这里我们将在基础软件包中完成。 编辑 `ros2_ws/src/polygon_base/src/area_node.cpp` 包含下列内容:

``` C++
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

那个... `ClassLoader` 是需要理解的关键类,定义于 `class_loader.hpp` [页眉文件](https://github.com/ros/pluginlib/blob/ros2/pluginlib/include/pluginlib/class_loader.hpp):

> - 其模板为基类,即: `polygon_base::RegularPolygon`.
>
> - 第一个参数是基类的包名的字符串,即: `polygon_base`.
>
> - 第二个参数是插件具有完全合格基础类类型的字符串,即. `polygon_base::RegularPolygon`.

有很多方法可以快速切换一个类的例子。在这个例子中,我们正在使用共享的指针。我们只需要拨打 `createSharedInstance` 引用插件 : 这可以是完全合格的插件类类型( 即 `type` 声明 XML 文件的属性,例如: `polygon_plugins::Square`),或可选魔法名称(the `name` 声明 XML 文件的属性,例如, `awesome_triangle`).

重要说明: `polygon_base` 软件包中定义了此节点的软件包并不取决于 `polygon_plugins` 类。插件将被动态地加载,而不需要任何依赖性来宣布。此外,我们正在用硬码插件名称即时进行分类,但您也可以用参数等动态方式进行。

<span id="build-and-run"></span>

### 4 构建和运行

导航回你工作空间的根, `ros2_ws`,并构建您的新软件包:

``` console
$ colcon build --packages-select polygon_base polygon_plugins
```

从 `ros2_ws`,请确定源代码设置文件 :

##### Linux

``` console
$ source install/setup.bash
```

##### macOS

``` console
$ . install/setup.bash
```

##### Windows

``` console
$ call install/setup.bat
```

那个... `ros2 plugin` 命令由 `ros2plugin` 软件包。如果在 Debian 软件包安装上无法使用此命令,请用 :

``` console
$ sudo apt install ros-rolling-ros2plugin
```

您可以通过列表验证您的插件已成功注册 :

``` console
$ ros2 plugin list
polygon_plugins:
   Plugin(name='polygon_plugins::Square', type='polygon_plugins::Square', base='polygon_base::RegularPolygon')
   Plugin(name='polygon_plugins::Triangle', type='polygon_plugins::Triangle', base='polygon_base::RegularPolygon')
```

现在运行节点:

``` console
$ ros2 run polygon_base area_node
Triangle area: 43.30
Square area: 100.00
```

<span id="summary"></span>

## 小结

恭喜您! 您刚刚写出并使用了您的第一个插件 。
