---
translation_status: machine_translated
source: How-To-Guides/Using-callback-groups.rst
---

<span id="using-callback-groups"></span>

# 使用回调组

当在多路径执行器中运行节点时, ROS 2 提供调用回调组作为控制不同调用回调执行的工具。 此页面旨在作为如何高效使用调用回调组的指南。 假设读者对调用回调组的概念有基本的理解 。 [执行器](../Concepts/Intermediate/About-Executors.md).

<span id="basics-of-callback-groups"></span>

## 召回组的基本情况

当在多路径执行器中运行节点时,ROS 2会提供两种不同类型的调用回调组来控制调用回调的执行:

- 相互禁用召回组

- 复召组

这些回调组以不同的方式限制其回调的执行。 简言之:

- 互相排斥的回调组防止其回调被平行执行 - 基本上使回调组中的回调被一个SingleThreadedExecutor执行.

- Reentrant Callback Group 允许执行者以它认为合适的任何方式安排和执行该组的召回,而不受限制。这意味着,除了不同的召回相互平行运行之外,同样的召回的不同事件也可能同时执行.

- 属于不同召回组(属于任何类型)的召回总是可以平行执行的.

同样重要的是要记住,不同的ROS 2实体会把他们的召回组转发给它们所培育的所有召回组。例如,如果有人将召回组指派给动作客户端,那么客户端创建的所有召回组都会被指派给该召回组.

可以通过节点创建回调组 `create_callback_group` 函数在 rclcpp 中,并通过调用 rclpy 中的组的构造器。在创建订阅、计时器等时,调用组可以被作为参数/选项来传递。应该保留对调用组的引用,否则与调用组相关的调用组将不会被执行器调用。

##### C++

``` cpp
my_callback_group = create_callback_group(rclcpp::CallbackGroupType::MutuallyExclusive);

rclcpp::SubscriptionOptions options;
options.callback_group = my_callback_group;

my_subscription = create_subscription<Int32>("/topic", rclcpp::SensorDataQoS(),
                                              callback, options);
```

##### Python

``` python
my_callback_group = MutuallyExclusiveCallbackGroup()
my_subscription = self.create_subscription(Int32, "/topic", self.callback, qos_profile=1,
                                            callback_group=my_callback_group)
```

如果用户在创建订阅、计时器等时未指定任何召回组,则该实体将被指定给节点默认召回组。默认召回组是一个相互排斥的召回组,可以通过 `NodeBaseInterface::get_default_callback_group()` 在rclpp和通过 `Node.default_callback_group` 在rclpy。 (原始内容存档于2018-09-21).

<span id="about-callbacks"></span>

### 关于召回

在ROS 2 和执行器中,召回是指由执行器处理调度和执行的函数。

- 订阅回调(接收和处理一个专题的数据),

- 计时器召回,

- 服务召回(用于在服务器中执行服务请求),

- 动作服务器和客户端中不同的召回,

- 做回召 未来。

下面是关于召回的几个要点,在与召回团体合作时应当记住这些要点.

- ROS 2 中几乎所有的功能都是回调! 执行器运行的每个功能顾名思义都是回调. ROS 2 系统中的非回调功能主要存在于系统的边缘(用户和传感器输入等).

- 有时回调被隐藏起来,并且从用户/开发者API中可能看不出它们的存在。对于任何一种服务或动作的“同步”调用(rclpy)来说,情况尤其如此。例如,同步调用(system) `Client.call(request)` 添加到服务中添加了 Future 的已完成调用后需要在执行函数调用时执行的调用,但这种调用后无法直接为用户所看到.

<span id="controlling-execution"></span>

## 控制执行

为了用召回组来控制执行,可以考虑以下准则.

个人回调与自己的互动:

- 如果它应该同时执行, 注册到 Reentrant Callback Group 中。 例如, 动作/ 服务服务器需要能够并行处理多个动作呼叫 。

- 登记在相互排斥的召回小组,如果它应该 **永远** 一个实例可以是运行一个发布控制命令的控制循环的计时器调用。

对于不同的调用相互间的相互作用:

- 如果它们应该登记在同一个相互排斥的召回小组中 **永远** 一个实例是,回调器正在访问共享的关键和非线性安全资源。

如果它们应该平行执行,则您有两种选择,取决于单个回调是否应该能够相互重叠:

- 将它们注册到不同的互免召回组(不重复单个召回组)

- 将它们注册到 Reentrant 召回组( 单个召回的重叠)

并行运行不同调用的一个实例是节点,它有一个同步服务客户端和一个调用此服务的定时器。请参见下面的详细示例。

<span id="avoiding-deadlocks"></span>

## 避免陷入僵局

设立节点的召回组是不正确的,可能导致僵局(或其他不想要的行为 ) , 尤其是如果人们想要使用同步调用服务或行动的话。 事实上,即使ROS 2的API文件也提到不应在召回中同步调用行动或服务,因为它可能导致僵局。 虽然使用同步调用在这方面的确更安全,但同步调用也可以起作用。 另一方面,同步调用也有其优点,比如使代码更简单易理解。 因此,本节为正确设置节点召回组以避免僵局提供了一些指导方针。

这里要注意的第一件事是,每个节点的默认调回组是一个相互排斥的调回组。 如果用户在创建计时器、订阅器、客户端等时没有指定任何其他调回组,那么这些实体当时或之后创建的任何调回组都会使用节点的默认调回组。 此外,如果节点中的一切都使用相同的相互排斥的调回组,那么节点基本上就如同它由单线执行器处理一样,即使指定了多线执行器。 因此,每当人们决定使用多线执行器时,就应该总是指定一些调回组,以便执行器的选择有意义。

考虑到上述情况,现提出几项准则,帮助避免僵局:

- 如果您在任何类型的召回中同步呼叫, 此召回和客户端需要归属

  - 不同的召回组(任何类型),或

  - a Reentrant召回小组。

- 如果由于其他要求,如线程安全和/或在等待结果时阻断其他回调(或者如果你想绝对确保永远不可能陷入僵局),上述配置是不可能的,使用同步调用.

如果第一点失败,将永远造成僵局。 类似情况的一个例子是在计时器召回中同步服务(例见下一节 ) 。

<span id="examples"></span>

## 实例

让我们看看不同调用组设置的一些简单例子。 以下的演示代码考虑在计时器调用中同步调用服务 。

<span id="demo-code"></span>

### 演示码

我们有两个节点 一个提供简单的服务:

##### C++

``` cpp
#include <memory>
#include "rclcpp/rclcpp.hpp"
#include "std_srvs/srv/empty.hpp"

using namespace std::placeholders;

namespace cb_group_demo
{
class ServiceNode : public rclcpp::Node
{
public:
    ServiceNode() : Node("service_node")
    {
        service_ptr_ = this->create_service<std_srvs::srv::Empty>(
                "test_service",
                std::bind(&ServiceNode::service_callback, this, _1, _2, _3)
        );
    }

private:
    rclcpp::Service<std_srvs::srv::Empty>::SharedPtr service_ptr_;

    void service_callback(
            const std::shared_ptr<rmw_request_id_t> request_header,
            const std::shared_ptr<std_srvs::srv::Empty::Request> request,
            const std::shared_ptr<std_srvs::srv::Empty::Response> response)
    {
        (void)request_header;
        (void)request;
        (void)response;
        RCLCPP_INFO(this->get_logger(), "Received request, responding...");
    }
};  // class ServiceNode
}   // namespace cb_group_demo

int main(int argc, char* argv[])
{
    rclcpp::init(argc, argv);
    auto service_node = std::make_shared<cb_group_demo::ServiceNode>();

    RCLCPP_INFO(service_node->get_logger(), "Starting server node, shut down with CTRL-C");
    rclcpp::spin(service_node);
    RCLCPP_INFO(service_node->get_logger(), "Keyboard interrupt, shutting down.\n");

    rclcpp::shutdown();
    return 0;
}
```

##### Python

``` python
import rclpy
from rclpy.node import Node
from std_srvs.srv import Empty

class ServiceNode(Node):
    def __init__(self):
        super().__init__('service_node')
        self.srv = self.create_service(Empty, 'test_service', callback=self.service_callback)

    def service_callback(self, request, result):
        self.get_logger().info('Received request, responding...')
        return result


if __name__ == '__main__':
    rclpy.init()
    node = ServiceNode()
    try:
        node.get_logger().info("Starting server node, shut down with CTRL-C")
        rclpy.spin(node)
    except KeyboardInterrupt:
        node.get_logger().info('Keyboard interrupt, shutting down.\n')
    node.destroy_node()
    rclpy.shutdown()
```

和另一个包含服务客户端以及用于打服务电话的计时器:

##### C++

*说明:* rclcpp中服务客户端的API不提供类似于rclpy中的同步调用方法,因此我们等待未来的对象来模拟同步调用的效果.

``` cpp
#include <chrono>
#include <memory>
#include "rclcpp/rclcpp.hpp"
#include "std_srvs/srv/empty.hpp"

using namespace std::chrono_literals;

namespace cb_group_demo
{
class DemoNode : public rclcpp::Node
{
public:
    DemoNode() : Node("client_node")
    {
        client_cb_group_ = nullptr;
        timer_cb_group_ = nullptr;
        client_ptr_ = this->create_client<std_srvs::srv::Empty>("test_service", rmw_qos_profile_services_default,
                                                                client_cb_group_);
        timer_ptr_ = this->create_wall_timer(1s, std::bind(&DemoNode::timer_callback, this),
                                            timer_cb_group_);
    }

private:
    rclcpp::CallbackGroup::SharedPtr client_cb_group_;
    rclcpp::CallbackGroup::SharedPtr timer_cb_group_;
    rclcpp::Client<std_srvs::srv::Empty>::SharedPtr client_ptr_;
    rclcpp::TimerBase::SharedPtr timer_ptr_;

    void timer_callback()
    {
        RCLCPP_INFO(this->get_logger(), "Sending request");
        auto request = std::make_shared<std_srvs::srv::Empty::Request>();
        auto result_future = client_ptr_->async_send_request(request);
        std::future_status status = result_future.wait_for(10s);  // timeout to guarantee a graceful finish
        if (status == std::future_status::ready) {
            RCLCPP_INFO(this->get_logger(), "Received response");
        }
    }
};  // class DemoNode
}   // namespace cb_group_demo

int main(int argc, char* argv[])
{
    rclcpp::init(argc, argv);
    auto client_node = std::make_shared<cb_group_demo::DemoNode>();
    rclcpp::executors::MultiThreadedExecutor executor;
    executor.add_node(client_node);

    RCLCPP_INFO(client_node->get_logger(), "Starting client node, shut down with CTRL-C");
    executor.spin();
    RCLCPP_INFO(client_node->get_logger(), "Keyboard interrupt, shutting down.\n");

    rclcpp::shutdown();
    return 0;
}
```

##### Python

``` python
import rclpy
from rclpy.executors import MultiThreadedExecutor
from rclpy.callback_groups import MutuallyExclusiveCallbackGroup, ReentrantCallbackGroup
from rclpy.node import Node
from std_srvs.srv import Empty


class CallbackGroupDemo(Node):
    def __init__(self):
        super().__init__('client_node')

        client_cb_group = None
        timer_cb_group = None
        self.client = self.create_client(Empty, 'test_service', callback_group=client_cb_group)
        self.call_timer = self.create_timer(1, self._timer_cb, callback_group=timer_cb_group)

    def _timer_cb(self):
        self.get_logger().info('Sending request')
        _ = self.client.call(Empty.Request())
        self.get_logger().info('Received response')


if __name__ == '__main__':
    rclpy.init()
    node = CallbackGroupDemo()
    executor = MultiThreadedExecutor()
    executor.add_node(node)

    try:
        node.get_logger().info('Beginning client, shut down with CTRL-C')
        executor.spin()
    except KeyboardInterrupt:
        node.get_logger().info('Keyboard interrupt, shutting down.\n')
    node.destroy_node()
    rclpy.shutdown()
```

客户端节点的构建器包含设置服务客户端和定时器的调回组的选项。 上面有默认设置( 两者都是) `nullptr` / `None`)),计时器和客户端都会使用节点默认的互禁召回组.

<span id="the-problem"></span>

### 问题

由于我们用1秒计时器打服务电话,预期的结果是服务每秒拨打一次,客户端总是得到回复和打印. `Received response`。如果我们尝试在终端中运行服务器和客户端节点,我们就得到以下输出。

##### 客户端

``` console
[INFO] [1653034371.758739131] [client_node]: Starting client node, shut down with CTRL-C
[INFO] [1653034372.755865649] [client_node]: Sending request
^C[INFO] [1653034398.161674869] [client_node]: Keyboard interrupt, shutting down.
```

##### 服务器

``` console
[INFO] [1653034355.308958238] [service_node]: Starting server node, shut down with CTRL-C
[INFO] [1653034372.758197320] [service_node]: Received request, responding...
^C[INFO] [1653034416.021962246] [service_node]: Keyboard interrupt, shutting down.
```

因此,事实证明,不是服务被反复调用,而是从来没有收到过第一次通话的响应,之后客户端的节点似乎卡住了,不再打进一步电话,即执行在僵局中停止了!

原因是计时器召回和客户端正在使用相同的相互排斥调回组(节点的默认值 ) 。 当服务调回时,客户端会将其召回组传递给未来对象(隐藏在 Python 版本的调回方式内), 其已完成的召回需要执行服务调回的结果才能可用。 但是,由于此已完成的召回和定时器召回处于相同的相互排斥组中,而且计时器召回仍在执行中(等待服务调回的结果), 已完成的召回永远不会执行。 被卡住的计时器召回也阻止了其他任何执行本身, 所以计时器不会第二次开火 。

<span id="solution"></span>

### 解决方案

我们可以轻松地解决这个问题,例如,将计时器和客户端分配给不同的调回组。 因此,让我们将客户端节点的构造器的前两行修改如下(其他的都将保持不变):

##### C++

``` cpp
client_cb_group_ = this->create_callback_group(rclcpp::CallbackGroupType::MutuallyExclusive);
timer_cb_group_ = this->create_callback_group(rclcpp::CallbackGroupType::MutuallyExclusive);
```

##### Python

``` python
client_cb_group = MutuallyExclusiveCallbackGroup()
timer_cb_group = MutuallyExclusiveCallbackGroup()
```

现在我们得到预期的结果,即计时器多次起火,每个服务电话都会得到预期的结果:

##### 客户端

``` console
[INFO] [1653067523.431731177] [client_node]: Starting client node, shut down with CTRL-C
[INFO] [1653067524.431912821] [client_node]: Sending request
[INFO] [1653067524.433230445] [client_node]: Received response
[INFO] [1653067525.431869330] [client_node]: Sending request
[INFO] [1653067525.432912803] [client_node]: Received response
[INFO] [1653067526.431844726] [client_node]: Sending request
[INFO] [1653067526.432893954] [client_node]: Received response
[INFO] [1653067527.431828287] [client_node]: Sending request
[INFO] [1653067527.432848369] [client_node]: Received response
^C[INFO] [1653067528.400052749] [client_node]: Keyboard interrupt, shutting down.
```

##### 服务器

``` console
[INFO] [1653067522.052866001] [service_node]: Starting server node, shut down with CTRL-C
[INFO] [1653067524.432577720] [service_node]: Received request, responding...
[INFO] [1653067525.432365009] [service_node]: Received request, responding...
[INFO] [1653067526.432300261] [service_node]: Received request, responding...
[INFO] [1653067527.432272441] [service_node]: Received request, responding...
^C[INFO] [1653034416.021962246] [service_node]: KeyboardInterrupt, shutting down.
```

人们或许会考虑仅仅避免节点默认的回调组是否足够。 情况并非如此:用另一个互不相干的组取代默认组不会有什么变化。 因此,以下的配置也会导致先前发现的僵局。

##### C++

``` cpp
client_cb_group_ = this->create_callback_group(rclcpp::CallbackGroupType::MutuallyExclusive);
timer_cb_group_ = client_cb_group_;
```

##### Python

``` python
client_cb_group = MutuallyExclusiveCallbackGroup()
timer_cb_group = client_cb_group
```

事实上,本案中万事俱备的条件是定时器和客户不得属于同一相互排斥的团体。 因此,以下所有配置(以及其他一些配置)都会在定时器多次起火和完成服务呼叫时产生预期结果。

##### C++

``` cpp
client_cb_group_ = this->create_callback_group(rclcpp::CallbackGroupType::Reentrant);
timer_cb_group_ = client_cb_group_;
```

或 时 间

``` cpp
client_cb_group_ = this->create_callback_group(rclcpp::CallbackGroupType::MutuallyExclusive);
timer_cb_group_ = nullptr;
```

或 时 间

``` cpp
client_cb_group_ = nullptr;
timer_cb_group_ = this->create_callback_group(rclcpp::CallbackGroupType::MutuallyExclusive);
```

或 时 间

``` cpp
client_cb_group_ = this->create_callback_group(rclcpp::CallbackGroupType::Reentrant);
timer_cb_group_ = nullptr;
```

##### Python

``` python
client_cb_group = ReentrantCallbackGroup()
timer_cb_group = client_cb_group
```

或 时 间

``` python
client_cb_group = MutuallyExclusiveCallbackGroup()
timer_cb_group = None
```

或 时 间

``` python
client_cb_group = None
timer_cb_group = MutuallyExclusiveCallbackGroup()
```

或 时 间

``` python
client_cb_group = ReentrantCallbackGroup()
timer_cb_group = None
```
