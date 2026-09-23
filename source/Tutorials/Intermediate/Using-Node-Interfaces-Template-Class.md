---
translation_status: machine_translated
source: Tutorials/Intermediate/Using-Node-Interfaces-Template-Class.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-the-node-interfaces-template-class-c"></span>

# 使用节点接口模板类（C++）

**目标：** 学习如何进入 `Node` 信息使用情况 `rclcpp::NodeInterfaces<>`

**教程级别：** 中级

**用时：** 10分钟

<span id="overview"></span>

## 概述

并不是所有的ROS节点都是平等地创建的! `rclcpp::Node` 财务报告和财务报告 `rclcpp_lifecycle::LifecycleNode` 类不共享继承树,这意味着 ROS 开发者在想要写入一个以 ROS 节点指针为参数的函数时可以运行到编译时间类型问题。要解决这个问题, `rclcpp` 包括 `rclcpp::NodeInterfaces<>` 模板类型,应用作常规和生命周期节点通过函数的首选公约。 [ROSCON 2023闪电谈话](https://vimeo.com/879001243#t=16m0s) 简洁地总结问题和补救。以下教程将演示您如何使用 `rclcpp::NodeInterfaces<>` 作为所有ROS节点类型的可靠而紧凑的接口.

那个... `rclcpp::NodeInterfaces<>` 模板类提供了一种紧凑而高效的方法,用于管理ROS 2中的节点接口。 `Nodes`,例如, `rclcpp::Node` 财务报告和财务报告 `rclcpp_lifecycle::LifecycleNode`,它们不能共享相同的继承树。

<span id="accessing-node-information-with-a-sharedptr"></span>

## 1 访问节点信息 `SharedPtr`

在下面的例子中,我们创建了一个简单的 `Node` 调用 `Simple_Node` 并定义函数 `node_info` 接受一个 `SharedPtr` 页:1 `Node`。函数检索并打印 `Node`.

``` c++
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

输出 :

``` console
[INFO] [Simple_Node]: Node name: Simple_Node
```

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

虽然这种方法对类型论据很有效 `rclcpp::Node`,它不适用于其他节点类型,例如: `rclcpp_lifecycle::LifecycleNode`.

<span id="explicitly-pass-rclcpp-node-interfaces"></span>

## 2 明确通过 `rclcpp::node_interfaces`

适用于所有节点类型的更强有力的办法是明确通过 `rclcpp::node_interfaces` 作为函数参数,如下文示例所示。在下文示例中,我们创建名为 `node_info` 以二为论据 `rclcpp::node_interfaces`, `NodeBaseInterface` 财务报告和财务报告 `NodeLoggingInterface` 并打印 `Node` 名称。然后我们创建两个类型节点 `rclcpp_lifecycle::LifecycleNode` 财务报告和财务报告 `rclcpp::Node` 并传出它们的界面 `node_info`.

``` c++
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

输出 :

``` console
[INFO] [Simple_Node]: Node name: Simple_Node
[INFO] [Simple_LifeCycle_Node]: Node name: Simple_LifeCycle_Node
```

随着各种功能的复杂程度的增加, `rclcpp::node_interfaces` 参数也会增加,导致可读性和紧凑性问题。为了使代码更加灵活,与不同的节点类型兼容,我们使用 `rclcpp::NodeInterfaces<>`.

<span id="using-rclcpp-nodeinterfaces"></span>

## 3 使用 `rclcpp::NodeInterfaces<>`

建议使用的方法 `Node` 类型信息通过 `Node Interfaces`.

下面,与前一个例子类似,a `rclcpp_lifecycle::LifecycleNode` 备注a `rclcpp::Node` 被创建。

``` c++
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

输出 :

``` console
[INFO] [Simple_Node]: Node name: Simple_Node
[INFO] [Simple_LifeCycle_Node]: Node name: Simple_LifeCycle_Node
```

<span id="examine-the-code"></span>

### 3.1 审查守则

``` c++
using MyNodeInterfaces =
  rclcpp::node_interfaces::NodeInterfaces<rclcpp::node_interfaces::NodeBaseInterface, rclcpp::node_interfaces::NodeLoggingInterface>;

void node_info(MyNodeInterfaces interfaces)
{
  auto base_interface = interfaces.get_node_base_interface();
  auto logging_interface = interfaces.get_node_logging_interface();
  RCLCPP_INFO(logging_interface->get_logger(), "Node name: %s", base_interface->get_name());
}
```

而不是接受 `SharedPtr` 或节点接口,此函数引用一个 `rclcpp::node_interfaces::NodeInterfaces` 对象。使用此方法的另一个优点是支持将类似节点的物体暗中转换。这意味着可以直接将任何类似节点的物体传递给期望一个函数。 `rclcpp::node_interfaces::NodeInterfaces` 对象。

它提取到:

- `NodeBaseInterface` 提供基本节点功能。

- `NodeLoggingInterface` 启用日志 。

然后,它检索并打印出节点名称.

``` c++
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

接下来,我们创建一个 `rclcpp::Node` 页:1 `rclcpp_lifecycle::LifecycleNode` 班级。 `rclcpp_lifecycle::LifecycleNode` 类往往包括状态过渡的功能 `Unconfigured`, `Inactive`, `Active`,以及 `Finalized`然而,它们不包括在示威活动中。

``` c++
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

A. 主要职能 `SharedPtr` 两者 `rclcpp_lifecycle::LifecycleNode` 财务报告和财务报告 `rclcpp::Node` 。上面声明的函数称为一次,每个节点类型作为参数。

> **说明**
>
> 那个... `SharedPtr` 由于模板接受提及 `NodeT` 对象。
