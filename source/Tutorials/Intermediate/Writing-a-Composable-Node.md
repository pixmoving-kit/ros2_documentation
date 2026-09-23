---
translation_status: machine_translated
source: Tutorials/Intermediate/Writing-a-Composable-Node.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="writing-a-composable-node-c"></span>

# 编写可组合节点（C++）

<span id="starting-place"></span>

## 开始位置

让我们假设你有一个常客 `rclcpp::Node` 要运行的可执行文件与其他节点相同,以便实现更高效的通信。

我们从拥有直接继承的阶级开始, `Node`,这也有一个主要方法定义。

``` c++
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

这将通常被编译为可执行文件 。

``` cmake
# ...
add_executable(vincent_driver src/vincent_driver.cpp)
# ...
install(TARGETS vincent_driver
    DESTINATION lib/${PROJECT_NAME}
)
```

<span id="code-updates"></span>

## 代码更新

<span id="add-the-package-dependency"></span>

### 添加软件包依赖性

 [package.xml](https://github.com/ros2/demos/tree/rolling/composition/package.xml) 应依赖 `rclcpp_components`,一个啦

``` xml
<depend>rclcpp_components</depend>
```

或者,您可以独立添加一个 `build_depend/exec_depend`.

<span id="class-definition"></span>

### 类别 定义

您可能必须做的对班级定义的唯一改变就是确保 [类的构造器](https://github.com/ros2/demos/tree/rolling/composition/src/talker_component.cpp) 使用一个 `NodeOptions` 参数。

``` c++
VincentDriver(const rclcpp::NodeOptions & options) : Node("vincent_driver", options)
{
  // ...
}
```

<span id="no-more-main-method"></span>

### 不再使用主要方法

将您的主要方法替换为 `pluginlib`- 典型的宏观引用。

``` c++
#include <rclcpp_components/register_node_macro.hpp>
RCLCPP_COMPONENTS_REGISTER_NODE(palomino::VincentDriver)
```

> **注意**
>
> 如果您所替换的主要方法包含一个 `MultiThreadedExecutor`中,请注意,并确保您的容器节点是多行读的。见下文。

<span id="cmake-changes"></span>

### CMake 更改

第一,增加一个 `rclcpp_components` 在 CMakeLists.txt 中作为依赖:

``` cmake
find_package(rclcpp_components REQUIRED)
```

第二,我们要取代我们 `add_executable` 带一个 `add_library` 带有新的目标名称。

``` cmake
add_library(vincent_driver_component SHARED src/vincent_driver.cpp)
```

第三,替换使用旧目标执行新目标的其他构建命令。不要忘记添加 `rclcpp_components` 输入 `ament_target_dependencies`. i.e. `ament_target_dependencies(vincent_driver ...)` 变成 `ament_target_dependencies(vincent_driver_component "rclcpp_components" ...)`

第四,添加新的命令来声明您的组件 。

``` cmake
rclcpp_components_register_node(
    vincent_driver_component
    PLUGIN "palomino::VincentDriver"
    EXECUTABLE vincent_driver
)
```

第五也是最后, 更改 CMake 中运行于旧目标上的任何安装命令, 以安装库版本。 例如, 不将任一目标安装到 `lib/${PROJECT_NAME}`。替换为库安装。

``` cmake
ament_export_targets(export_vincent_driver_component)
install(TARGETS vincent_driver_component
        EXPORT export_vincent_driver_component
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        RUNTIME DESTINATION bin
)
```

<span id="running-your-node"></span>

## 运行您的节点

见 [组成辅导](Composition.md) 快速而肮脏的版本是,如果你的 Python 发射文件中有以下内容,

``` python
from launch_ros.actions import Node

# ..

ld.add_action(Node(
    package='palomino',
    executable='vincent_driver',
    # ..
))
```

你可以把它替换为

``` python
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

> **注意**
>
> 如果您需要多条线索, 而不是设置您的可执行文件到 `component_container`,设置它 `component_container_mt`
