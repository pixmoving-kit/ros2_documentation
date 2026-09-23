<span id="using-the-node-interfaces-template-class-c"></span>

# 使用节点接口模板类（C++）

**目标：** 学习通过 `rclcpp::NodeInterfaces<>` 访问节点信息。

**教程级别：** 中级

**预计耗时：** 10 分钟

<span id="overview"></span>

## 概述

并非所有 ROS 节点都具有相同的类型层次。`rclcpp::Node` 与 `rclcpp_lifecycle::LifecycleNode` 不在同一继承树中，因此编写接收 ROS 节点指针的函数时，可能遇到编译期类型问题。为解决这一问题，`rclcpp` 提供了 `rclcpp::NodeInterfaces<>` 模板类型，推荐用它向函数传递普通节点和生命周期节点。[ROSCon 2023 闪电演讲](https://vimeo.com/879001243#t=16m0s)简要概述了问题及解决办法。本教程展示如何将其作为适用于各种 ROS 节点的可靠、简洁接口。

`rclcpp::NodeInterfaces<>` 提供紧凑、高效的节点接口管理方式，特别适用于处理不共享继承树的多种节点类型。

<span id="accessing-node-information-with-a-sharedptr"></span>

## 1 通过 SharedPtr 访问节点信息

下面创建名为 `Simple_Node` 的简单节点，并定义接收该节点 `SharedPtr` 的 `node_info` 函数，获取并打印节点名。

```c++
#include <memory>
#include "rclcpp/rclcpp.hpp"

void node_info(rclcpp::Node::SharedPtr node)
{
  RCLCPP_INFO(node->get_logger(), "Node name: %s", node->get_name());
}

class SimpleNode : public rclcpp::Node
{
public:
  SimpleNode(const std::string & node_name)
  : Node(node_name)
  {
  }
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  auto node = std::make_shared<SimpleNode>("Simple_Node");
  node_info(node);
}
```

输出：

```console
[INFO] [Simple_Node]: Node name: Simple_Node
```

参阅 [rclcpp 便捷头文件说明](../../_internal/Rclcpp-Convenience-Header-Note.md)。

这种方法适用于 `rclcpp::Node`，但不适用于 `rclcpp_lifecycle::LifecycleNode` 等其他节点类型。

<span id="explicitly-pass-rclcpp-node-interfaces"></span>

## 2 显式传递 rclcpp::node_interfaces

更稳健且适用于所有节点类型的方式，是显式传递 `rclcpp::node_interfaces`。下面的 `node_info` 接收 `NodeBaseInterface` 和 `NodeLoggingInterface` 两个接口，并打印节点名。随后创建生命周期节点和普通节点，分别将其接口传入该函数。

```c++
void node_info(std::shared_ptr<rclcpp::node_interfaces::NodeBaseInterface> base_interface,
               std::shared_ptr<rclcpp::node_interfaces::NodeLoggingInterface> logging_interface)
{
  RCLCPP_INFO(logging_interface->get_logger(), "Node name: %s", base_interface->get_name());
}

class SimpleNode : public rclcpp::Node
{
public:
  SimpleNode(const std::string & node_name)
  : Node(node_name)
  {
  }
};

class LifecycleTalker : public rclcpp_lifecycle::LifecycleNode
{
public:
  explicit LifecycleTalker(const std::string & node_name, bool intra_process_comms = false)
  : rclcpp_lifecycle::LifecycleNode(node_name,
      rclcpp::NodeOptions().use_intra_process_comms(intra_process_comms))
  {}
}

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::executors::SingleThreadedExecutor exe;
  auto node = std::make_shared<SimpleNode>("Simple_Node");
  auto lc_node = std::make_shared<LifecycleTalker>("Simple_LifeCycle_Node");
  node_info(node->get_node_base_interface(),node->get_node_logging_interface());
  node_info(lc_node->get_node_base_interface(),lc_node->get_node_logging_interface());
}
```

输出：

```console
[INFO] [Simple_Node]: Node name: Simple_Node
[INFO] [Simple_LifeCycle_Node]: Node name: Simple_LifeCycle_Node
```

函数变复杂时，接口参数数量也会增加，影响可读性和简洁性。为了让代码更灵活并兼容不同节点类型，可使用 `rclcpp::NodeInterfaces<>`。

<span id="using-rclcpp-nodeinterfaces"></span>

## 3 使用 rclcpp::NodeInterfaces<>

推荐通过节点接口访问节点信息。与前一个示例相同，下面创建一个生命周期节点和一个普通节点：

```c++
#include <memory>
#include <string>
#include <thread>
#include "lifecycle_msgs/msg/transition.hpp"
#include "rclcpp/rclcpp.hpp"
#include "rclcpp_lifecycle/lifecycle_node.hpp"
#include "rclcpp_lifecycle/lifecycle_publisher.hpp"
#include "rclcpp/node_interfaces/node_interfaces.hpp"

using MyNodeInterfaces =
  rclcpp::node_interfaces::NodeInterfaces<rclcpp::node_interfaces::NodeBaseInterface, rclcpp::node_interfaces::NodeLoggingInterface>;

void node_info(MyNodeInterfaces interfaces)
{
  auto base_interface = interfaces.get_node_base_interface();
  auto logging_interface = interfaces.get_node_logging_interface();
  RCLCPP_INFO(logging_interface->get_logger(), "Node name: %s", base_interface->get_name());
}

class SimpleNode : public rclcpp::Node
{
public:
  SimpleNode(const std::string & node_name)
  : Node(node_name)
  {
  }
};

class LifecycleTalker : public rclcpp_lifecycle::LifecycleNode
{
public:
  explicit LifecycleTalker(const std::string & node_name, bool intra_process_comms = false)
  : rclcpp_lifecycle::LifecycleNode(node_name,
      rclcpp::NodeOptions().use_intra_process_comms(intra_process_comms))
  {}
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::executors::SingleThreadedExecutor exe;
  auto node = std::make_shared<SimpleNode>("Simple_Node");
  auto lc_node = std::make_shared<LifecycleTalker>("Simple_LifeCycle_Node");
  node_info(*node);
  node_info(*lc_node);
}
```

输出：

```console
[INFO] [Simple_Node]: Node name: Simple_Node
[INFO] [Simple_LifeCycle_Node]: Node name: Simple_LifeCycle_Node
```

<span id="examine-the-code"></span>

### 3.1 分析代码

```c++
using MyNodeInterfaces =
  rclcpp::node_interfaces::NodeInterfaces<rclcpp::node_interfaces::NodeBaseInterface, rclcpp::node_interfaces::NodeLoggingInterface>;

void node_info(MyNodeInterfaces interfaces)
{
  auto base_interface = interfaces.get_node_base_interface();
  auto logging_interface = interfaces.get_node_logging_interface();
  RCLCPP_INFO(logging_interface->get_logger(), "Node name: %s", base_interface->get_name());
}
```

函数接收 `rclcpp::node_interfaces::NodeInterfaces` 对象的引用，而不是 `SharedPtr` 或单独的节点接口。这种方法还支持类节点对象的隐式转换，因此可以直接将类节点对象传给期望接收该接口对象的函数。

函数提取：

- `NodeBaseInterface`：提供基本节点功能。
- `NodeLoggingInterface`：提供日志功能。

随后获取并打印节点名称。

```c++
class SimpleNode : public rclcpp::Node
{
public:
  SimpleNode(const std::string & node_name)
  : Node(node_name)
  {
  }
};

class LifecycleTalker : public rclcpp_lifecycle::LifecycleNode
{
public:
  explicit LifecycleTalker(const std::string & node_name, bool intra_process_comms = false)
  : rclcpp_lifecycle::LifecycleNode(node_name,
      rclcpp::NodeOptions().use_intra_process_comms(intra_process_comms))
  {}
};
```

接下来创建 `rclcpp::Node` 和 `rclcpp_lifecycle::LifecycleNode` 派生类。生命周期节点通常包含处理 `Unconfigured`、`Inactive`、`Active`、`Finalized` 等状态转换的函数，本示例为简洁起见省略。

```c++
int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::executors::SingleThreadedExecutor exe;
  auto node = std::make_shared<SimpleNode>("Simple_Node");
  auto lc_node = std::make_shared<LifecycleTalker>("Simple_LifeCycle_Node");
  node_info(*node);
  node_info(*lc_node);
}
```

在主函数中，为两类节点创建 `SharedPtr`，分别将节点作为参数调用上述函数。

> 模板接收的是 `NodeT` 对象的引用，因此需要先解引用 `SharedPtr`。
