<span id="creating-custom-msg-and-srv-files"></span> <span id="custominterfaces"></span>
# 创建自定义 msg 和 srv 文件

**目标：** 定义自定义接口文件（`.msg` 和 `.srv`），并在 Python 和 C++ 节点中使用。

**教程级别：** 初学者

**预计用时：** 20 分钟

<span id="background"></span>
## 背景

此前教程使用消息和服务接口介绍了[话题](../Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md)、[服务](../Beginner-CLI-Tools/Understanding-ROS2-Services/Understanding-ROS2-Services.md)，以及简单的发布者/订阅者（[C++](Writing-A-Simple-Cpp-Publisher-And-Subscriber.md) / [Python](Writing-A-Simple-Py-Publisher-And-Subscriber.md)）和服务端/客户端节点（[C++](Writing-A-Simple-Cpp-Service-And-Client.md) / [Python](Writing-A-Simple-Py-Service-And-Client.md)）。这些示例使用的都是预定义接口。

虽然推荐复用预定义接口，但有时仍需要定义自己的消息和服务。本教程介绍创建自定义接口定义的最简单方法。

<span id="prerequisites"></span>
## 前提条件

需要准备一个 [ROS 2 工作空间](Creating-A-Workspace/Creating-A-Workspace.md)。

本教程还会使用此前发布者/订阅者（[C++](Writing-A-Simple-Cpp-Publisher-And-Subscriber.md)、[Python](Writing-A-Simple-Py-Publisher-And-Subscriber.md)）和服务端/客户端（[C++](Writing-A-Simple-Cpp-Service-And-Client.md)、[Python](Writing-A-Simple-Py-Service-And-Client.md)）教程中创建的软件包，测试新的自定义接口。

<span id="tasks"></span>
## 操作步骤

<span id="create-a-new-package"></span>
### 1 创建新软件包

本教程将自定义 `.msg` 和 `.srv` 文件放在独立的软件包中，再由另一个软件包使用它们。两个软件包应位于同一工作空间。

由于要使用此前的发布/订阅和服务端/客户端软件包，请进入它们所在工作空间的 `ros2_ws/src`，运行：

```console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 tutorial_interfaces
```

新软件包名为 `tutorial_interfaces`。定义这些接口的软件包只能使用 `ament_cmake` 构建类型，但这并不限制使用接口的软件包类型。可以在 `ament_cmake` 软件包中定义接口，再由 C++ 或 Python 节点使用，最后一节将演示这一点。

`.msg` 和 `.srv` 文件必须分别放在名为 `msg` 和 `srv` 的目录中。在 `ros2_ws/src/tutorial_interfaces` 中创建它们：

```console
$ mkdir msg srv
```

<span id="create-custom-definitions"></span>
### 2 创建自定义定义文件

<span id="msg-definition"></span>
#### 2.1 msg 定义

在刚创建的 `tutorial_interfaces/msg` 中新建 `Num.msg`，用一行声明数据结构：

```bash
int64 num
```

这个自定义消息传递一个名为 `num` 的 64 位整数。

在同一目录中新建 `Sphere.msg`，内容为：

```bash
geometry_msgs/Point center
float64 radius
```

该消息使用了其他消息软件包中的消息，这里是 `geometry_msgs/Point`。

<span id="srv-definition"></span>
#### 2.2 srv 定义

在 `tutorial_interfaces/srv` 中新建 `AddThreeInts.srv`，定义以下请求和响应结构：

```bash
int64 a
int64 b
int64 c
---
int64 sum
```

自定义服务的请求包含 `a`、`b`、`c` 三个整数，响应包含一个名为 `sum` 的整数。

<span id="cmakelists-txt"></span>
### 3 CMakeLists.txt

要将接口定义转换为 C++、Python 等语言的代码，使这些语言能够使用接口，请在 `CMakeLists.txt` 中添加：

```cmake
find_package(geometry_msgs REQUIRED)
find_package(rosidl_default_generators REQUIRED)

rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/Num.msg"
  "msg/Sphere.msg"
  "srv/AddThreeInts.srv"
  DEPENDENCIES geometry_msgs # Add packages that above messages depend on, in this case geometry_msgs for Sphere.msg
)
```

!!! note "注意"
    `rosidl_generate_interfaces` 的第一个参数是库名称，必须以软件包名称开头，例如 `${PROJECT_NAME}` 或 `${PROJECT_NAME}_suffix`。详见[相关讨论](https://github.com/ros2/rosidl/issues/441#issuecomment-591025515)。

<span id="package-xml"></span>
### 4 package.xml

生成各语言的接口代码依赖 `rosidl_default_generators`，因此需要将其声明为构建工具依赖。`rosidl_default_runtime` 则是之后使用接口时所需的运行时依赖。`rosidl_interface_packages` 是 `tutorial_interfaces` 应加入的依赖组，通过 `<member_of_group>` 声明。

在 `package.xml` 的 `<package>` 元素中添加：

```xml
<depend>geometry_msgs</depend>
<buildtool_depend>rosidl_default_generators</buildtool_depend>
<exec_depend>rosidl_default_runtime</exec_depend>
<member_of_group>rosidl_interface_packages</member_of_group>
```

<span id="build-the-tutorial-interfaces-package"></span>
### 5 构建 tutorial_interfaces 软件包

自定义接口软件包的各部分已经准备好。在工作空间根目录 `~/ros2_ws` 中运行对应命令进行构建。

**Linux**

```console
$ colcon build --packages-select tutorial_interfaces
```

**macOS**

```console
$ colcon build --packages-select tutorial_interfaces
```

**Windows**

```console
$ colcon build --merge-install --packages-select tutorial_interfaces
```

构建后，其他 ROS 2 软件包就能发现这些接口。

<span id="confirm-msg-and-srv-creation"></span>
### 6 确认 msg 和 srv 已创建

打开新终端，在工作空间 `ros2_ws` 中加载环境。

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

使用 `ros2 interface show` 确认接口生成成功。输出应类似于：

```console
$ ros2 interface show tutorial_interfaces/msg/Num
int64 num
```

```console
$ ros2 interface show tutorial_interfaces/msg/Sphere
geometry_msgs/Point center
        float64 x
        float64 y
        float64 z
float64 radius
```

```console
$ ros2 interface show tutorial_interfaces/srv/AddThreeInts
int64 a
int64 b
int64 c
---
int64 sum
```

<span id="test-the-new-interfaces"></span>
### 7 测试新接口

可以使用之前教程创建的软件包进行测试。简单修改节点代码、`CMakeLists.txt` 和 `package.xml`，就能使用新接口。

<span id="testing-num-msg-with-pub-sub"></span>
#### 7.1 用发布/订阅系统测试 Num.msg

修改此前的发布者/订阅者软件包（[C++](Writing-A-Simple-Cpp-Publisher-And-Subscriber.md) 或 [Python](Writing-A-Simple-Py-Publisher-And-Subscriber.md)），即可观察 `Num.msg` 的运行效果。由于将标准字符串消息改为了数值消息，输出会略有不同。

**C++ 发布者**

```c++
#include <chrono>
#include <memory>

#include "rclcpp/rclcpp.hpp"
#include "tutorial_interfaces/msg/num.hpp"                                            // CHANGE

using namespace std::chrono_literals;

class MinimalPublisher : public rclcpp::Node
{
public:
  MinimalPublisher()
  : Node("minimal_publisher"), count_(0)
  {
    publisher_ = this->create_publisher<tutorial_interfaces::msg::Num>("topic", 10);  // CHANGE
    timer_ = this->create_wall_timer(
      500ms, std::bind(&MinimalPublisher::timer_callback, this));
  }

private:
  void timer_callback()
  {
    auto message = tutorial_interfaces::msg::Num();                                   // CHANGE
    message.num = this->count_++;                                                     // CHANGE
    RCLCPP_INFO_STREAM(this->get_logger(), "Publishing: '" << message.num << "'");    // CHANGE
    publisher_->publish(message);
  }
  rclcpp::TimerBase::SharedPtr timer_;
  rclcpp::Publisher<tutorial_interfaces::msg::Num>::SharedPtr publisher_;             // CHANGE
  size_t count_;
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<MinimalPublisher>());
  rclcpp::shutdown();
  return 0;
}
```

另见 [rclcpp 便捷头文件说明](../../_internal/Rclcpp-Convenience-Header-Note.md)。

**Python 发布者**

```python
import rclpy
from rclpy.node import Node

from tutorial_interfaces.msg import Num                            # CHANGE


class MinimalPublisher(Node):

    def __init__(self):
        super().__init__('minimal_publisher')
        self.publisher_ = self.create_publisher(Num, 'topic', 10)  # CHANGE
        timer_period = 0.5
        self.timer = self.create_timer(timer_period, self.timer_callback)
        self.i = 0

    def timer_callback(self):
        msg = Num()                                                # CHANGE
        msg.num = self.i                                           # CHANGE
        self.publisher_.publish(msg)
        self.get_logger().info('Publishing: "%d"' % msg.num)       # CHANGE
        self.i += 1


def main(args=None):
    rclpy.init(args=args)

    minimal_publisher = MinimalPublisher()

    rclpy.spin(minimal_publisher)

    minimal_publisher.destroy_node()
    rclpy.shutdown()


if __name__ == '__main__':
    main()
```

**C++ 订阅者**

```c++
#include <functional>
#include <memory>

#include "rclcpp/rclcpp.hpp"
#include "tutorial_interfaces/msg/num.hpp"                                       // CHANGE

using std::placeholders::_1;

class MinimalSubscriber : public rclcpp::Node
{
public:
  MinimalSubscriber()
  : Node("minimal_subscriber")
  {
    subscription_ = this->create_subscription<tutorial_interfaces::msg::Num>(    // CHANGE
      "topic", 10, std::bind(&MinimalSubscriber::topic_callback, this, _1));
  }

private:
  void topic_callback(const tutorial_interfaces::msg::Num & msg) const  // CHANGE
  {
    RCLCPP_INFO_STREAM(this->get_logger(), "I heard: '" << msg.num << "'");     // CHANGE
  }
  rclcpp::Subscription<tutorial_interfaces::msg::Num>::SharedPtr subscription_;  // CHANGE
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<MinimalSubscriber>());
  rclcpp::shutdown();
  return 0;
}
```

**Python 订阅者**

```python
import rclpy
from rclpy.node import Node

from tutorial_interfaces.msg import Num                        # CHANGE


class MinimalSubscriber(Node):

    def __init__(self):
        super().__init__('minimal_subscriber')
        self.subscription = self.create_subscription(
            Num,                                               # CHANGE
            'topic',
            self.listener_callback,
            10)
        self.subscription

    def listener_callback(self, msg):
        self.get_logger().info('I heard: "%d"' % msg.num)  # CHANGE


def main(args=None):
    rclpy.init(args=args)

    minimal_subscriber = MinimalSubscriber()

    rclpy.spin(minimal_subscriber)

    minimal_subscriber.destroy_node()
    rclpy.shutdown()


if __name__ == '__main__':
    main()
```

**CMakeLists.txt**

添加以下内容，仅适用于 C++：

```cmake
#...

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(tutorial_interfaces REQUIRED)                      # CHANGE

add_executable(talker src/publisher_lambda_function.cpp)
ament_target_dependencies(talker rclcpp tutorial_interfaces)    # CHANGE

add_executable(listener src/subscriber_lambda_function.cpp)
ament_target_dependencies(listener rclcpp tutorial_interfaces)  # CHANGE

install(TARGETS
  talker
  listener
  DESTINATION lib/${PROJECT_NAME})

ament_package()
```

**package.xml**

C++ 添加：

```c++
<depend>tutorial_interfaces</depend>
```

Python 添加：

```python
<exec_depend>tutorial_interfaces</exec_depend>
```

保存以上修改后，构建软件包。

**C++，Linux/macOS**

```console
$ colcon build --packages-select cpp_pubsub
```

**C++，Windows**

```console
$ colcon build --merge-install --packages-select cpp_pubsub
```

**Python，Linux/macOS**

```console
$ colcon build --packages-select py_pubsub
```

**Python，Windows**

```console
$ colcon build --merge-install --packages-select py_pubsub
```

打开两个新终端，分别加载 `ros2_ws` 环境，再分别运行发布者和订阅者。

**C++**

```console
$ ros2 run cpp_pubsub talker
```

```console
$ ros2 run cpp_pubsub listener
```

**Python**

```console
$ ros2 run py_pubsub talker
```

```console
$ ros2 run py_pubsub listener
```

`Num.msg` 只传递一个整数，因此 talker 现在发布的是整数值，而非之前的字符串：

```console
[INFO] [minimal_publisher]: Publishing: '0'
[INFO] [minimal_publisher]: Publishing: '1'
[INFO] [minimal_publisher]: Publishing: '2'
```

<span id="testing-addthreeints-srv-with-service-client"></span>
#### 7.2 用服务端/客户端系统测试 AddThreeInts.srv

修改此前的服务端/客户端软件包（[C++](Writing-A-Simple-Cpp-Service-And-Client.md) 或 [Python](Writing-A-Simple-Py-Service-And-Client.md)），即可观察 `AddThreeInts.srv` 的运行效果。请求从两个整数改为三个整数，因此输出也会略有不同。

**C++ 服务端**

```c++
#include "rclcpp/rclcpp.hpp"
#include "tutorial_interfaces/srv/add_three_ints.hpp"                                        // CHANGE

#include <memory>

void add(const std::shared_ptr<tutorial_interfaces::srv::AddThreeInts::Request> request,     // CHANGE
          std::shared_ptr<tutorial_interfaces::srv::AddThreeInts::Response>       response)  // CHANGE
{
  response->sum = request->a + request->b + request->c;                                      // CHANGE
  RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "Incoming request\na: %ld" " b: %ld" " c: %ld",  // CHANGE
                request->a, request->b, request->c);                                         // CHANGE
  RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "sending back response: [%ld]", (long int)response->sum);
}

int main(int argc, char **argv)
{
  rclcpp::init(argc, argv);

  std::shared_ptr<rclcpp::Node> node = rclcpp::Node::make_shared("add_three_ints_server");   // CHANGE

  rclcpp::Service<tutorial_interfaces::srv::AddThreeInts>::SharedPtr service =               // CHANGE
    node->create_service<tutorial_interfaces::srv::AddThreeInts>("add_three_ints",  &add);   // CHANGE

  RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "Ready to add three ints.");                     // CHANGE

  rclcpp::spin(node);
  rclcpp::shutdown();
}
```

**Python 服务端**

```python
from tutorial_interfaces.srv import AddThreeInts                                                           # CHANGE

import rclpy
from rclpy.node import Node


class MinimalService(Node):

    def __init__(self):
        super().__init__('minimal_service')
        self.srv = self.create_service(AddThreeInts, 'add_three_ints', self.add_three_ints_callback)       # CHANGE

    def add_three_ints_callback(self, request, response):                                                  # CHANGE
        response.sum = request.a + request.b + request.c                                                   # CHANGE
        self.get_logger().info('Incoming request\na: %d b: %d c: %d' % (request.a, request.b, request.c))  # CHANGE

        return response

def main(args=None):
    rclpy.init(args=args)

    minimal_service = MinimalService()

    rclpy.spin(minimal_service)

    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

**C++ 客户端**

```c++
#include "rclcpp/rclcpp.hpp"
#include "tutorial_interfaces/srv/add_three_ints.hpp"                                       // CHANGE

#include <chrono>
#include <cstdlib>
#include <memory>

using namespace std::chrono_literals;

int main(int argc, char **argv)
{
  rclcpp::init(argc, argv);

  if (argc != 4) { // CHANGE
      RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "usage: add_three_ints_client X Y Z");      // CHANGE
      return 1;
  }

  std::shared_ptr<rclcpp::Node> node = rclcpp::Node::make_shared("add_three_ints_client");  // CHANGE
  rclcpp::Client<tutorial_interfaces::srv::AddThreeInts>::SharedPtr client =                // CHANGE
    node->create_client<tutorial_interfaces::srv::AddThreeInts>("add_three_ints");          // CHANGE

  auto request = std::make_shared<tutorial_interfaces::srv::AddThreeInts::Request>();       // CHANGE
  request->a = atoll(argv[1]);
  request->b = atoll(argv[2]);
  request->c = atoll(argv[3]);                                                              // CHANGE

  while (!client->wait_for_service(1s)) {
    if (!rclcpp::ok()) {
      RCLCPP_ERROR(rclcpp::get_logger("rclcpp"), "Interrupted while waiting for the service. Exiting.");
      return 0;
    }
    RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "service not available, waiting again...");
  }

  auto result = client->async_send_request(request);
  // Wait for the result.
  if (rclcpp::spin_until_future_complete(node, result) ==
    rclcpp::FutureReturnCode::SUCCESS)
  {
    RCLCPP_INFO(rclcpp::get_logger("rclcpp"), "Sum: %ld", result.get()->sum);
  } else {
    RCLCPP_ERROR(rclcpp::get_logger("rclcpp"), "Failed to call service add_three_ints");    // CHANGE
  }

  rclcpp::shutdown();
  return 0;
}
```

**Python 客户端**

```python
from tutorial_interfaces.srv import AddThreeInts                            # CHANGE
import sys
import rclpy
from rclpy.node import Node


class MinimalClientAsync(Node):

    def __init__(self):
        super().__init__('minimal_client_async')
        self.cli = self.create_client(AddThreeInts, 'add_three_ints')       # CHANGE
        while not self.cli.wait_for_service(timeout_sec=1.0):
            self.get_logger().info('service not available, waiting again...')
        self.req = AddThreeInts.Request()                                   # CHANGE

    def send_request(self):
        self.req.a = int(sys.argv[1])
        self.req.b = int(sys.argv[2])
        self.req.c = int(sys.argv[3])                                       # CHANGE
        self.future = self.cli.call_async(self.req)


def main(args=None):
    rclpy.init(args=args)

    minimal_client = MinimalClientAsync()
    minimal_client.send_request()

    while rclpy.ok():
        rclpy.spin_once(minimal_client)
        if minimal_client.future.done():
            try:
                response = minimal_client.future.result()
            except Exception as e:
                minimal_client.get_logger().info(
                    'Service call failed %r' % (e,))
            else:
                minimal_client.get_logger().info(
                    'Result of add_three_ints: for %d + %d + %d = %d' %                                # CHANGE
                    (minimal_client.req.a, minimal_client.req.b, minimal_client.req.c, response.sum))  # CHANGE
            break

    minimal_client.destroy_node()
    rclpy.shutdown()


if __name__ == '__main__':
    main()
```

**CMakeLists.txt**

添加以下内容，仅适用于 C++：

```cmake
#...

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(tutorial_interfaces REQUIRED)         # CHANGE

add_executable(server src/add_two_ints_server.cpp)
ament_target_dependencies(server
  rclcpp tutorial_interfaces)                      # CHANGE

add_executable(client src/add_two_ints_client.cpp)
ament_target_dependencies(client
  rclcpp tutorial_interfaces)                      # CHANGE

install(TARGETS
  server
  client
  DESTINATION lib/${PROJECT_NAME})

ament_package()
```

**package.xml**

C++ 添加：

```c++
<depend>tutorial_interfaces</depend>
```

Python 添加：

```python
<exec_depend>tutorial_interfaces</exec_depend>
```

保存以上修改后，构建软件包。

**C++，Linux/macOS**

```console
$ colcon build --packages-select cpp_srvcli
```

**C++，Windows**

```console
$ colcon build --merge-install --packages-select cpp_srvcli
```

**Python，Linux/macOS**

```console
$ colcon build --packages-select py_srvcli
```

**Python，Windows**

```console
$ colcon build --merge-install --packages-select py_srvcli
```

打开两个新终端，分别加载 `ros2_ws` 环境，再分别运行服务端和客户端。

**C++**

```console
$ ros2 run cpp_srvcli server
```

```console
$ ros2 run cpp_srvcli client 2 3 1
```

**Python**

```console
$ ros2 run py_srvcli service
```

```console
$ ros2 run py_srvcli client 2 3 1
```

<span id="summary"></span>
## 小结

本教程介绍了如何在独立软件包中创建自定义接口，并在其他软件包中使用。

这里只介绍了自定义接口的基础内容，更多细节见 [ROS 2 接口](../../Concepts/Basic/About-Interfaces.md)。

<span id="next-steps"></span>
## 后续步骤

[下一篇教程](Single-Package-Define-And-Use-Interface.md)将介绍 ROS 2 接口的更多用法。
