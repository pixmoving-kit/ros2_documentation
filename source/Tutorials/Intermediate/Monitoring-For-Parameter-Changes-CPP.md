<span id="monitoring-for-parameter-changes-c"></span>

# 监控参数变化（C++）

**目标：** 学习使用 ParameterEventHandler 类监控并响应参数变化。

**教程级别：** 中级

**预计耗时：** 20 分钟

**最低版本：** Galactic

<span id="background"></span>

## 背景

节点经常需要响应自身或其他节点的参数变化。`ParameterEventHandler` 可方便地监听这些变化，供代码作出响应。本教程介绍其 C++ 版本，分别监控本节点和其他节点的参数。

<span id="prerequisites"></span>

## 前提条件

开始前应完成：

- [理解 ROS 2 参数](../Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md)
- [在 C++ 类中使用参数](../Beginner-Client-Libraries/Using-Parameters-In-A-Class-CPP.md)

此外，本教程要求运行 ROS 2 Galactic 发行版。

<span id="tasks"></span>

## 任务

创建一个存放示例代码的软件包，编写使用 ParameterEventHandler 的 C++ 代码，并测试结果。

<span id="create-a-package"></span>

### 1 创建软件包

打开新终端，[加载 ROS 2 环境](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)，使 `ros2` 命令可用。按照[这些步骤](../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md#new-directory)创建 `ros2_ws` 工作空间。

软件包应创建在 `src` 内，而不是工作空间根目录。进入 `ros2_ws/src` 后运行：

```console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 cpp_parameter_event_handler --dependencies rclcpp
```

终端会显示 `cpp_parameter_event_handler` 及其所需文件、目录已创建。`--dependencies` 会自动在 `package.xml` 和 `CMakeLists.txt` 中加入依赖声明。

<span id="update-package-xml"></span>

#### 1.1 更新 package.xml

由于创建时使用了 `--dependencies`，无须手动添加依赖。但仍应在 `package.xml` 中填写描述、维护者姓名和邮箱、许可证信息：

```xml
<description>C++ parameter events client tutorial</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

<span id="write-the-c-node"></span>

### 2 编写 C++ 节点

在 `ros2_ws/src/cpp_parameter_event_handler/src` 中创建 `parameter_event_handler.cpp`，填入：

```C++
#include <memory>

#include "rclcpp/rclcpp.hpp"

class SampleNodeWithParameters : public rclcpp::Node
{
public:
  SampleNodeWithParameters()
  : Node("node_with_parameters")
  {
    this->declare_parameter("an_int_param", 0);

    // Create a parameter subscriber that can be used to monitor parameter changes
    // (for this node's parameters as well as other nodes' parameters)
    param_subscriber_ = std::make_shared<rclcpp::ParameterEventHandler>(this);

    // Set a callback for this node's integer parameter, "an_int_param"
    auto cb = [this](const rclcpp::Parameter & p) {
        RCLCPP_INFO(
          this->get_logger(), "cb: Received an update to parameter \"%s\" of type %s: \"%ld\"",
          p.get_name().c_str(),
          p.get_type_name().c_str(),
          p.as_int());
      };
    cb_handle_ = param_subscriber_->add_parameter_callback("an_int_param", cb);
  }

private:
  std::shared_ptr<rclcpp::ParameterEventHandler> param_subscriber_;
  std::shared_ptr<rclcpp::ParameterCallbackHandle> cb_handle_;
};

int main(int argc, char ** argv)
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<SampleNodeWithParameters>());
  rclcpp::shutdown();

  return 0;
}
```

<span id="examine-the-code"></span>

#### 2.1 分析代码

`#include <memory>` 让代码可以使用 `std::make_shared` 模板；`#include "rclcpp/rclcpp.hpp"` 引入 rclcpp 功能，包括 ParameterEventHandler。

参阅 [rclcpp 便捷头文件说明](../../_internal/Rclcpp-Convenience-Header-Note.md)。

`SampleNodeWithParameters` 的构造函数声明默认值为 0 的整数参数 `an_int_param`，随后创建 `ParameterEventHandler` 监控参数变化，再创建 lambda 回调，使其在 `an_int_param` 更新时被调用。

> 务必保存 `add_parameter_callback` 返回的句柄，否则回调无法正确注册。

```C++
SampleNodeWithParameters()
: Node("node_with_parameters")
{
  this->declare_parameter("an_int_param", 0);

  // Create a parameter subscriber that can be used to monitor parameter changes
  // (for this node's parameters as well as other nodes' parameters)
  param_subscriber_ = std::make_shared<rclcpp::ParameterEventHandler>(this);

  // Set a callback for this node's integer parameter, "an_int_param"
  auto cb = [this](const rclcpp::Parameter & p) {
      RCLCPP_INFO(
        this->get_logger(), "cb: Received an update to parameter \"%s\" of type %s: \"%ld\"",
        p.get_name().c_str(),
        p.get_type_name().c_str(),
        p.as_int());
    };
  cb_handle_ = param_subscriber_->add_parameter_callback("an_int_param", cb);
}
```

类定义之后是常规主函数：初始化 ROS，运行节点以收发消息，并在用户按下 `Ctrl+C` 后关闭。

```C++
int main(int argc, char ** argv)
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<SampleNodeWithParameters>());
  rclcpp::shutdown();

  return 0;
}
```

<span id="add-executable"></span>

#### 2.2 添加可执行程序

打开 `CMakeLists.txt`，在 `find_package(rclcpp REQUIRED)` 后加入：

```console
add_executable(parameter_event_handler src/parameter_event_handler.cpp)
ament_target_dependencies(parameter_event_handler rclcpp)

install(TARGETS
  parameter_event_handler
  DESTINATION lib/${PROJECT_NAME}
)
```

<span id="build-and-run"></span>

### 3 构建并运行

构建前，建议在工作空间根目录 `ros2_ws` 运行 `rosdep`，检查缺失依赖。

Linux：

```console
$ rosdep install -i --from-path src --rosdistro $ROS_DISTRO -y
```

本教程中，macOS 和 Windows 可跳过这一步，因为此处的 rosdep 流程仅适用于 Linux。

返回 `ros2_ws` 并构建：

```console
$ colcon build --packages-select cpp_parameter_event_handler
```

打开新终端，进入 `ros2_ws` 并加载环境。

Linux：

```console
$ . install/setup.bash
```

macOS：

```console
$ . install/setup.bash
```

Windows：

```console
$ call install/setup.bat
```

运行节点：

```console
$ ros2 run cpp_parameter_event_handler parameter_event_handler
```

节点已有一个参数，每次参数更新都会打印消息。打开另一终端，像之前一样加载环境（`. install/setup.bash`），然后执行：

```console
$ ros2 param set node_with_parameters an_int_param 43
```

节点终端应出现类似输出：

```console
[INFO] [1606950498.422461764] [node_with_parameters]: cb: Received an update to parameter "an_int_param" of type integer: "43"
```

先前设置的回调已执行，并显示更新后的值。现在可以在节点终端按 `Ctrl+C` 停止示例。

<span id="extensions"></span>

## 扩展

目前的节点只监控自身的一个参数。以下在此基础上介绍另外两个适合 ParameterEventHandler 的使用场景。

<span id="monitor-changes-to-another-node-s-parameters"></span>

### 监控其他节点的参数变化

ParameterEventHandler 也能监控其他节点。下面修改 `SampleNodeWithParameters`，监听 `parameter_blackboard` 演示节点中的一个 double 参数。

在构造函数现有代码后加入：

```C++
// Now, add a callback to monitor any changes to the remote node's parameter. In this
// case, we supply the remote node name.
auto cb2 = [this](const rclcpp::Parameter & p) {
    RCLCPP_INFO(
      this->get_logger(), "cb2: Received an update to parameter \"%s\" of type: %s: \"%.02lf\"",
      p.get_name().c_str(),
      p.get_type_name().c_str(),
      p.as_double());
  };
auto remote_node_name = std::string("parameter_blackboard");
auto remote_param_name = std::string("a_double_param");
cb_handle2_ = param_subscriber_->add_parameter_callback(remote_param_name, cb2, remote_node_name);
```

再添加成员变量 `cb_handle2`，保存新增回调的句柄：

```C++
private:
  std::shared_ptr<rclcpp::ParameterEventHandler> param_subscriber_;
  std::shared_ptr<rclcpp::ParameterCallbackHandle> cb_handle_;
  std::shared_ptr<rclcpp::ParameterCallbackHandle> cb_handle2_;  // Add this
};
```

返回工作空间根目录 `ros2_ws` 并重新构建：

```console
$ colcon build --packages-select cpp_parameter_event_handler
```

加载环境。

Linux：

```console
$ . install/setup.bash
```

macOS：

```console
$ . install/setup.bash
```

Windows：

```console
$ call install/setup.bat
```

先运行更新后的参数事件处理节点：

```console
$ ros2 run cpp_parameter_event_handler parameter_event_handler
```

在另一个已加载 ROS 环境的终端中，运行 `parameter_blackboard`：

```console
$ ros2 run demo_nodes_cpp parameter_blackboard
```

最后，在第三个已加载 ROS 环境的终端中设置该节点的参数：

```console
$ ros2 param set parameter_blackboard a_double_param 3.45
```

参数事件处理节点应输出以下内容，说明参数更新触发了回调：

```console
[INFO] [1606952588.237531933] [node_with_parameters]: cb2: Received an update to parameter "a_double_param" of type: double: "3.45"
```

<span id="monitor-all-node-parameters-simultaneously"></span>

### 同时监控所有节点参数

要同时监控多个节点或参数，逐个调用 `add_parameter_callback` 会很繁琐。可以用 `add_parameter_event_callback` 注册单个回调，在**任何节点的任何参数**变化时触发。

首先在构造函数中加入：

```C++
this->declare_parameter("another_double_param", 0.0);

...

auto event_cb = [this](const rcl_interfaces::msg::ParameterEvent & parameter_event) {
    RCLCPP_INFO(
      this->get_logger(), "Received parameter event from node \"%s\"",
      parameter_event.node.c_str());

    for (const auto& p : parameter_event.changed_parameters) {
      RCLCPP_INFO(
        this->get_logger(), "Inside event: \"%s\" changed to %s",
        p.name.c_str(),
        rclcpp::Parameter::from_parameter_msg(p).value_to_string().c_str());
    };
  };
event_cb_handle_ = param_subscriber_->add_parameter_event_callback(event_cb);
```

这会声明新的 double 参数 `another_double_param`，并添加监控两个参数的事件回调。`parameter_event` 的类型为 [rcl_interfaces/msg/ParameterEvent](https://docs.ros.org/en/rolling/p/rcl_interfaces/msg/ParameterEvent.html)。虽然示例未展示，事件回调也可以监控参数的添加和删除。

别忘记将事件回调句柄添加为私有成员：

```C++
private:
  ...
  std::shared_ptr<rclcpp::ParameterEventCallbackHandle> event_cb_handle_;
```

返回 `ros2_ws` 重新构建：

```console
$ colcon build --packages-select cpp_parameter_event_handler
```

加载环境。

Linux：

```console
$ . install/setup.bash
```

macOS：

```console
$ . install/setup.bash
```

Windows：

```console
$ call install\setup.bat
```

运行节点测试事件回调：

```console
$ ros2 run cpp_parameter_event_handler parameter_event_handler
```

在第二个已加载 ROS 环境的终端中设置原来的整数参数：

```console
$ ros2 param set node_with_parameters an_int_param 44
```

单参数回调和事件回调都应被触发：

```console
[INFO] [1747144403.418980063] [node_with_parameters]: cb: Received an update to parameter "an_int_param" of type integer: "44"
[INFO] [1747144403.419086611] [node_with_parameters]: Received parameter event from node "/node_with_parameters"
[INFO] [1747144403.419114103] [node_with_parameters]: Inside event: "an_int_param" changed to 44
```

再设置新的 double 参数：

```console
$ ros2 param set node_with_parameters another_double_param 4.4
```

由于没有通过 `add_parameter_callback` 为这个参数添加单参数回调，此时只会触发事件回调：

```console
[INFO] [1747144452.917437113] [node_with_parameters]: Received parameter event from node "/node_with_parameters"
[INFO] [1747144452.917591649] [node_with_parameters]: Inside event: "another_double_param" changed to 4.400000
```

> 一次设置多个参数时，建议使用[参数概念文档](../../Concepts/Basic/About-Parameters.md)介绍的 `set_parameters_atomically`，这样事件回调只触发一次。

<span id="summary"></span>

## 小结

本教程创建了带参数的节点，通过 ParameterEventHandler 注册回调，监控自身参数、远程节点参数，以及在单个事件回调中监控全部参数。该类能方便地监听参数变化，以便响应更新后的值。

<span id="related-content"></span>

## 相关内容

有关将 ROS 1 参数文件迁移到 ROS 2，参阅[迁移 YAML 参数文件](../../How-To-Guides/Migrating-from-ROS1/Migrating-Parameters.md)。
