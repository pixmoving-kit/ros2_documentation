<span id="writing-a-composable-node-c"></span>

# 编写可组合节点（C++）

<span id="starting-place"></span>

## 起点

假设你已有一个普通的 `rclcpp::Node` 可执行程序，希望让它与其他节点在同一进程中运行，以提高通信效率。

起始代码是一个直接继承 `Node` 的类，并定义了主函数：

```c++
namespace palomino
{
    class VincentDriver : public rclcpp::Node
    {
        // ...
    };
}

int main(int argc, char * argv[])
{
    rclcpp::init(argc, argv);
    rclcpp::spin(std::make_shared<palomino::VincentDriver>());
    rclcpp::shutdown();
    return 0;
}
```

通常通过 CMake 将其编译为可执行程序：

```cmake
# ...
add_executable(vincent_driver src/vincent_driver.cpp)
# ...
install(TARGETS vincent_driver
    DESTINATION lib/${PROJECT_NAME}
)
```

<span id="code-updates"></span>

## 修改代码

<span id="add-the-package-dependency"></span>

### 添加软件包依赖

在 [package.xml](https://github.com/ros2/demos/tree/rolling/composition/package.xml) 中声明对 `rclcpp_components` 的依赖：

```xml
<depend>rclcpp_components</depend>
```

也可以分别添加 `build_depend` 和 `exec_depend`。

<span id="class-definition"></span>

### 类定义

类定义中可能唯一需要的修改，是确保[构造函数](https://github.com/ros2/demos/tree/rolling/composition/src/talker_component.cpp)接收 `NodeOptions` 参数：

```c++
VincentDriver(const rclcpp::NodeOptions & options) : Node("vincent_driver", options)
{
  // ...
}
```

<span id="no-more-main-method"></span>

### 替换主函数

将主函数替换为 `pluginlib` 风格的宏调用：

```c++
#include <rclcpp_components/register_node_macro.hpp>
RCLCPP_COMPONENTS_REGISTER_NODE(palomino::VincentDriver)
```

> 如果原主函数使用 `MultiThreadedExecutor`，请记下这一点，并确保容器节点也采用多线程，具体见下文。

<span id="cmake-changes"></span>

### 修改 CMake

首先，在 `CMakeLists.txt` 中添加依赖：

```cmake
find_package(rclcpp_components REQUIRED)
```

其次，将 `add_executable` 替换为 `add_library`，并使用新的目标名：

```cmake
add_library(vincent_driver_component SHARED src/vincent_driver.cpp)
```

第三，将引用旧目标的其他构建命令改为引用新目标。别忘记在 `ament_target_dependencies` 中添加 `rclcpp_components`，例如将 `ament_target_dependencies(vincent_driver ...)` 改为 `ament_target_dependencies(vincent_driver_component "rclcpp_components" ...)`。

第四，添加声明组件的命令：

```cmake
rclcpp_components_register_node(
    vincent_driver_component
    PLUGIN "palomino::VincentDriver"
    EXECUTABLE vincent_driver
)
```

最后，将旧目标的安装命令改为安装库。不要把这些目标安装到 `lib/${PROJECT_NAME}`，而应采用库的安装规则：

```cmake
ament_export_targets(export_vincent_driver_component)
install(TARGETS vincent_driver_component
        EXPORT export_vincent_driver_component
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        RUNTIME DESTINATION bin
)
```

<span id="running-your-node"></span>

## 运行节点

关于节点组合的详细介绍，参阅[组合教程](Composition.md)。简要来说，如果 Python 启动文件中原本有：

```python
from launch_ros.actions import Node

# ..

ld.add_action(Node(
    package='palomino',
    executable='vincent_driver',
    # ..
))
```

可以替换为：

```python
from launch_ros.actions import ComposableNodeContainer
from launch_ros.descriptions import ComposableNode

# ..
ld.add_action(ComposableNodeContainer(
    name='a_buncha_nodes',
    namespace='',
    package='rclcpp_components',
    executable='component_container',
    composable_node_descriptions=[
        ComposableNode(
            package='palomino',
            plugin='palomino::VincentDriver',
            name='vincent_driver',
            # ..
            extra_arguments=[{'use_intra_process_comms': True}],
        ),
    ]
))
```

> 如果需要多线程，将可执行程序从 `component_container` 改为 `component_container_mt`。
