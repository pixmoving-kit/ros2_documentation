---
translation_status: machine_translated
source: Tutorials/Beginner-Client-Libraries/Custom-ROS2-Interfaces.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="creating-custom-msg-and-srv-files"></span> <span id="custominterfaces"></span>

# 创建自定义 msg 和 srv 文件

**目标：** 定义自定义接口文件( E)`.msg` 财务报告和财务报告 `.srv`),并使用Python和C++节点.

**教程级别：** 入门

**用时：** 20分钟

<span id="background"></span>

## 背景

在之前的教程中, 您使用信件和服务接口来了解 [话题](../Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.md), [服务](../Beginner-CLI-Tools/Understanding-ROS2-Services/Understanding-ROS2-Services.md),以及简单的出版商/订阅商([C++](Writing-A-Simple-Cpp-Publisher-And-Subscriber.md)/[Python](Writing-A-Simple-Py-Publisher-And-Subscriber.md))和服务/客户([C++](Writing-A-Simple-Cpp-Service-And-Client.md)/[Python](Writing-A-Simple-Py-Service-And-Client.md)) 节点。您使用的接口在这些情况下是预先定义的。

虽然使用预先定义的界面定义是好的做法,但有时你可能也需要定义自己的消息和服务。这个教程会向您介绍创建自定义界面定义的最简单方法。

<span id="prerequisites"></span>

## 前提条件

你应该有一个 [ROS 2 工作空间](Creating-A-Workspace/Creating-A-Workspace.md).

此教程还使用出版商/订阅商中创建的软件包( Name[C++](Writing-A-Simple-Cpp-Publisher-And-Subscriber.md) 财务报告和财务报告 [Python](Writing-A-Simple-Py-Publisher-And-Subscriber.md))和服务/客户([C++](Writing-A-Simple-Cpp-Service-And-Client.md) 财务报告和财务报告 [Python](Writing-A-Simple-Py-Service-And-Client.md)) 测试新自定义消息的教程。

<span id="tasks"></span>

## 操作步骤

<span id="create-a-new-package"></span>

### 1 创建新软件包

您将会为此教程创建自定义 `.msg` 财务报告和财务报告 `.srv` 文件在自己的软件包中,然后在单独的软件包中使用。两个软件包应该在同一工作空间中。

既然我们将使用在早期的教程中创建的 pub/sub 和服务/客户端软件包, 请确保您与这些软件包处于相同的工作空间(`ros2_ws/src`),然后运行以下命令来创建新软件包:

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 tutorial_interfaces
```

`tutorial_interfaces` 是新软件包的名称。请注意它是一个,而且只能是ament_cmake软件包,但这并不限制您可以使用您的信件和服务的类型。您可以在 ament_cmake软件包中创建自己的自定义接口,然后在C++或Python节点中使用,该节点将在最后一节中覆盖。

那个... `.msg` 财务报告和财务报告 `.srv` 需要将文件放置在名为“ ” 的目录中 `msg` 财务报告和财务报告 `srv` 创建目录。 `ros2_ws/src/tutorial_interfaces`:

``` console
$ mkdir msg srv
```

<span id="create-custom-definitions"></span>

### 2 创建自定义

<span id="msg-definition"></span>

#### 2.1 msg 定义

在那个 `tutorial_interfaces/msg` 您刚刚创建的目录, 创建一个新文件 `Num.msg` 并用一行代码声明其数据结构:

``` bash
int64 num
```

这是一个自定义消息, 它可以传输一个名为 64 位整数的单个 `num`.

同样在... `tutorial_interfaces/msg` 您刚刚创建的目录, 创建一个新文件 `Sphere.msg` 内容如下:

``` bash
geometry_msgs/Point center
float64 radius
```

此自定义消息使用来自另一个消息包的消息( Name`geometry_msgs/Point` (第6条)。

<span id="srv-definition"></span>

#### 2.2 srv 定义

回到过去 `tutorial_interfaces/srv` 您刚刚创建的目录, 创建一个新文件 `AddThreeInts.srv` 附有下列请求和答复结构:

``` bash
int64 a
int64 b
int64 c
---
int64 sum
```

这是您的自定义服务, 需要三个整数 。 `a`, `b`,以及 `c`,然后用整数响应 `sum`.

<span id="cmakelists-txt"></span>

### 3 `CMakeLists.txt`

要将您定义的界面转换成语言专用代码( 如 C++ 和 Python) , 以便用于这些语言, 请添加以下行到 `CMakeLists.txt`:

``` cmake
find_package(geometry_msgs REQUIRED)
find_package(rosidl_default_generators REQUIRED)

rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/Num.msg"
  "msg/Sphere.msg"
  "srv/AddThreeInts.srv"
  DEPENDENCIES geometry_msgs # Add packages that above messages depend on, in this case geometry_msgs for Sphere.msg
)
```

> **说明**
>
> 中的第一个参数( 库名称) `rosidl_generate_interfaces` 必须从软件包的名称开始,例如简单 `${PROJECT_NAME}` 或 时 间 `${PROJECT_NAME}_suffix`。见 <https://github.com/ros2/rosidl/issues/441#issuecomment-591025515>.

<span id="package-xml"></span>

### 4 `package.xml`

因为界面依赖 `rosidl_default_generators` 用于生成语言特定代码,您需要声明一个构建工具依赖它。 `rosidl_default_runtime` 是一个运行时间或执行阶段的依赖性,需要后期才能使用接口。 `rosidl_interface_packages` 您的软件包是依赖组的名称, `tutorial_interfaces`,应当与使用 `<member_of_group>` 标记 。

在下图中添加以下行 `<package>` 要素 `package.xml`:

``` xml
<depend>geometry_msgs</depend>
<buildtool_depend>rosidl_default_generators</buildtool_depend>
<exec_depend>rosidl_default_runtime</exec_depend>
<member_of_group>rosidl_interface_packages</member_of_group>
```

<span id="build-the-tutorial-interfaces-package"></span>

### 5 建设 `tutorial_interfaces` 软件包

现在您的自定义接口软件包的所有部分都已经到位,您可以构建软件包。 在您工作空间的根部( R)`~/ros2_ws`),运行以下命令:

##### Linux

``` console
$ colcon build --packages-select tutorial_interfaces
```

##### macOS

``` console
$ colcon build --packages-select tutorial_interfaces
```

##### Windows

``` console
$ colcon build --merge-install --packages-select tutorial_interfaces
```

现在这些接口将被其他ROS 2软件包发现.

<span id="confirm-msg-and-srv-creation"></span>

### 6 确认 msg 和 srv 创建

在新的终端中, 从工作空间内运行以下命令( Q) :`ros2_ws`来源:

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

现在您可以确认您的界面创建通过使用 `ros2 interface show` 命令。您在终端中看到的输出应该与下列输出相似 :

``` console
$ ros2 interface show tutorial_interfaces/msg/Num
int64 num
```

``` console
$ ros2 interface show tutorial_interfaces/msg/Sphere
geometry_msgs/Point center
        float64 x
        float64 y
        float64 z
float64 radius
```

``` console
$ ros2 interface show tutorial_interfaces/srv/AddThreeInts
int64 a
int64 b
int64 c
---
int64 sum
```

<span id="test-the-new-interfaces"></span>

### 7 测试新接口

对于此步骤, 您可以使用您在前一个教程中创建的软件包 。 对节点进行一些简单的修改 , `CMakeLists.txt` 财务报告和财务报告 `package.xml` 文件将允许您使用新的界面。

<span id="testing-num-msg-with-pub-sub"></span>

#### 7.1 测试 `Num.msg` 与酒吧/子公司

对前一个教程中创建的出版商/订阅者包进行了几处修改([C++](Writing-A-Simple-Cpp-Publisher-And-Subscriber.md) 或 时 间 [Python](Writing-A-Simple-Py-Publisher-And-Subscriber.md),你可以看到 `Num.msg` 。由于您将把标准字符串 msg 更改为数字,输出会略有不同。

**出版商**

##### C++

``` c++
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

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

##### Python

``` python
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

**订阅者**

##### C++

``` c++
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

##### Python

``` python
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

**CMakeLists.txt (中文(简体) ).**

添加以下行(仅C++):

``` cmake
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

增加以下一行:

##### C++

``` c++
<depend>tutorial_interfaces</depend>
```

##### Python

``` python
<exec_depend>tutorial_interfaces</exec_depend>
```

在进行上述编辑和保存所有修改后,构建软件包:

##### C++

在 Linux/macOS 上:

``` console
$ colcon build --packages-select cpp_pubsub
```

在 Windows 上:

``` console
$ colcon build --merge-install --packages-select cpp_pubsub
```

##### Python

在 Linux/macOS 上:

``` console
$ colcon build --packages-select py_pubsub
```

在 Windows 上:

``` console
$ colcon build --merge-install --packages-select py_pubsub
```

然后打开两个新的终端, 源 `ros2_ws` 中,并运行:

##### C++

``` console
$ ros2 run cpp_pubsub talker
```

``` console
$ ros2 run cpp_pubsub listener
```

##### Python

``` console
$ ros2 run py_pubsub talker
```

``` console
$ ros2 run py_pubsub listener
```

自兹 `Num.msg` 中继仅是一个整数,说话者应该只发布整数值,而不是它以前发布的字符串:

``` console
[INFO] [minimal_publisher]: Publishing: '0'
[INFO] [minimal_publisher]: Publishing: '1'
[INFO] [minimal_publisher]: Publishing: '2'
```

<span id="testing-addthreeints-srv-with-service-client"></span>

#### 7.2 测试 `AddThreeInts.srv` 有服务/客户服务

在前一个教程中创建的服务/客户软件包的几处修改后( Name[C++](Writing-A-Simple-Cpp-Service-And-Client.md) 或 时 间 [Python](Writing-A-Simple-Py-Service-And-Client.md),你可以看到 `AddThreeInts.srv` 。由于您将把原来的两个整数请求srv修改为三个整数请求srv,输出会略有不同。

**服务**

##### C++

``` c++
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

##### Python

``` python
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

**客户端**

##### C++

``` c++
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

##### Python

``` python
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

**CMakeLists.txt (中文(简体) ).**

添加以下行(仅C++):

``` cmake
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

增加以下一行:

##### C++

``` c++
<depend>tutorial_interfaces</depend>
```

##### Python

``` python
<exec_depend>tutorial_interfaces</exec_depend>
```

在进行上述编辑和保存所有修改后,构建软件包:

##### C++

在 Linux/macOS 上:

``` console
$ colcon build --packages-select cpp_srvcli
```

在 Windows 上:

``` console
$ colcon build --merge-install --packages-select cpp_srvcli
```

##### Python

在 Linux/macOS 上:

``` console
$ colcon build --packages-select py_srvcli
```

在 Windows 上:

``` console
$ colcon build --merge-install --packages-select py_srvcli
```

然后打开两个新的终端, 源 `ros2_ws` 中,并运行:

##### C++

``` console
$ ros2 run cpp_srvcli server
```

``` console
$ ros2 run cpp_srvcli client 2 3 1
```

##### Python

``` console
$ ros2 run py_srvcli service
```

``` console
$ ros2 run py_srvcli client 2 3 1
```

<span id="summary"></span>

## 小结

在这个教程中,你学会了如何在他们自己的包中创建自定义接口,以及如何在其他包中使用这些接口.

此教程只抓取关于定义自定义界面的表面。 您可以在 [关于 ROS 2 接口](../../Concepts/Basic/About-Interfaces.md).

<span id="next-steps"></span>

## 后续步骤

那个... [下一个教程](Single-Package-Define-And-Use-Interface.md) 在ROS 2中涵盖更多使用界面的方式.
