---
translation_status: machine_translated
source: Tutorials/Demos/Intra-Process-Communication.rst
---

<span id="setting-up-efficient-intra-process-communication"></span>

# 配置高效的进程内通信

<span id="background"></span>

## 背景

ROS应用通常由单个“节点”组成,它们执行狭窄的任务,并且与系统其他部分脱钩。 这促进断层隔离、更快的开发、模块化和代码再利用,但往往以性能为代价。 ROS 1 最初开发后,节点有效构成的必要性变得很明显,节点也得到了开发。 ROS 2 中,我们的目标是通过解决一些需要重组节点的根本问题来改进节点的设计。

在演示中, 我们将强调节点如何手工构成, 分别定义节点,

<span id="installing-the-demos"></span>

## 安装演示

见 [安装指令](../../Installation.md) 关于安装ROS 2的详情。

如果您已经从软件包中安装了 ROS 2, 请确保您已经安装过 `ros-rolling-intra-process-demo` 已安装。如果从源头下载归档或建立ROS 2,它就已经是安装的一部分。

<span id="running-and-understanding-the-demos"></span>

## 运行和理解演示

有一些不同的演示:有些是旨在突出进程内通信功能特征的玩具问题,有些是结束使用OpenCV并展示将节点重新压缩到不同配置的能力的例子.

<span id="the-two-node-pipeline-demo"></span>

### 两个节点管道演示

此演示旨在显示,进程内发布/订阅连接在发布和订阅时可导致消息的零副本传输 `std::unique_ptr`s.

首先让我们看看来源:

<https://github.com/ros2/demos/blob/rolling/intra_process_demo/src/two_node_pipeline/two_node_pipeline.cpp>

``` c++
#include <chrono>
#include <cinttypes>
#include <cstdio>
#include <memory>
#include <string>
#include <utility>

#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/int32.hpp"

using namespace std::chrono_literals;

// Node that produces messages.
struct Producer : public rclcpp::Node
{
  Producer(const std::string & name, const std::string & output)
  : Node(name, rclcpp::NodeOptions().use_intra_process_comms(true))
  {
    // Create a publisher on the output topic.
    pub_ = this->create_publisher<std_msgs::msg::Int32>(output, 10);
    std::weak_ptr<std::remove_pointer<decltype(pub_.get())>::type> captured_pub = pub_;
    // Create a timer which publishes on the output topic at ~1Hz.
    auto callback = [captured_pub]() -> void {
        auto pub_ptr = captured_pub.lock();
        if (!pub_ptr) {
          return;
        }
        static int32_t count = 0;
        std_msgs::msg::Int32::UniquePtr msg(new std_msgs::msg::Int32());
        msg->data = count++;
        printf(
          "Published message with value: %d, and address: 0x%" PRIXPTR "\n", msg->data,
          reinterpret_cast<std::uintptr_t>(msg.get()));
        pub_ptr->publish(std::move(msg));
      };
    timer_ = this->create_wall_timer(1s, callback);
  }

  rclcpp::Publisher<std_msgs::msg::Int32>::SharedPtr pub_;
  rclcpp::TimerBase::SharedPtr timer_;
};

// Node that consumes messages.
struct Consumer : public rclcpp::Node
{
  Consumer(const std::string & name, const std::string & input)
  : Node(name, rclcpp::NodeOptions().use_intra_process_comms(true))
  {
    // Create a subscription on the input topic which prints on receipt of new messages.
    sub_ = this->create_subscription<std_msgs::msg::Int32>(
      input,
      10,
      [](std_msgs::msg::Int32::UniquePtr msg) {
        printf(
          " Received message with value: %d, and address: 0x%" PRIXPTR "\n", msg->data,
          reinterpret_cast<std::uintptr_t>(msg.get()));
      });
  }

  rclcpp::Subscription<std_msgs::msg::Int32>::SharedPtr sub_;
};

int main(int argc, char * argv[])
{
  setvbuf(stdout, NULL, _IONBF, BUFSIZ);
  rclcpp::init(argc, argv);
  rclcpp::executors::SingleThreadedExecutor executor;

  auto producer = std::make_shared<Producer>("producer", "number");
  auto consumer = std::make_shared<Consumer>("consumer", "number");

  executor.add_node(producer);
  executor.add_node(consumer);
  executor.spin();

  rclcpp::shutdown();

  return 0;
}
```

正如你从看... `main` 函数,我们有一个生产者和一个消费者节点,我们把它们添加到一个单一的线状执行器中,然后调用旋转。

如果您查看“生产者”节点在 `Producer` 我们建立了一个出版商,负责出版“数字”专题,并定期制作一个新的信息,打印其地址和内容的价值,然后出版。

“消费者”节点比较简单,可以在 `Consumer` 结构化,因为它只订阅“数字”专题,并打印它收到的信息的地址和价值。

预期制作人会打印一个地址和价值,消费者会打印一个匹配的地址和价值。 这表明,进程内部的通信确实在起作用,并且避免不必要的拷贝,至少对于简单的图表来说是如此。

让我们通过执行来运行演示 `ros2 run intra_process_demo two_node_pipeline` 可执行文件( 请不要忘记先从设置文件源出) :

``` console
$ ros2 run intra_process_demo two_node_pipeline
Published message with value: 0, and address: 0x7fb02303faf0
Published message with value: 1, and address: 0x7fb020cf0520
 Received message with value: 1, and address: 0x7fb020cf0520
Published message with value: 2, and address: 0x7fb020e12900
 Received message with value: 2, and address: 0x7fb020e12900
Published message with value: 3, and address: 0x7fb020cf0520
 Received message with value: 3, and address: 0x7fb020cf0520
Published message with value: 4, and address: 0x7fb020e12900
 Received message with value: 4, and address: 0x7fb020e12900
Published message with value: 5, and address: 0x7fb02303cea0
 Received message with value: 5, and address: 0x7fb02303cea0
[...]
```

你将会注意到的一件事是,信息在每秒1秒左右被勾选。 这是因为我们告诉计时器每秒发射一次。

您可能也注意到了第一个消息( 有值 ) `0`) 没有一个对应的“ 接收到的信息... ” 。 这是因为发布/ 订阅是“ 最大努力 ” , 而且我们没有像行为一样的“ 剪切 ” 。 这意味着如果出版商在订阅建立之前发布信息, 订阅将不会收到该信息 。 这种种族条件会导致第一批信息丢失 。 在这种情况下, 因为它们每秒只来一次, 通常只丢失第一个信息 。

最后,你可以看到,具有同样价值的“信件......”和“收到信件......”的行也具有同样的地址。这说明,收到信件的地址与所发表的地址相同,不是复制件。这是因为我们正在出版和签署这些信件。 `std::unique_ptr`s 允许将信件的所有权安全地移动到系统周围。您也可以发布和订阅 `const &` 财务报告和财务报告 `std::shared_ptr`,但在这种情况下不会出现零拷贝。

<span id="the-cyclic-pipeline-demo"></span>

### 循环管道演示

此演示与上一个演示类似, 但制作人不会为每个迭代创建新消息, 而只使用一个消息实例。 这是通过在图中创建循环和在旋转执行器前外部制作一个发布节点来达到的 :

<https://github.com/ros2/demos/blob/rolling/intra_process_demo/src/cyclic_pipeline/cyclic_pipeline.cpp>

``` c++
#include <chrono>
#include <cinttypes>
#include <cstdio>
#include <memory>
#include <string>
#include <utility>

#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/int32.hpp"

using namespace std::chrono_literals;

// This node receives an Int32, waits 1 second, then increments and sends it.
struct IncrementerPipe : public rclcpp::Node
{
  IncrementerPipe(const std::string & name, const std::string & in, const std::string & out)
  : Node(name, rclcpp::NodeOptions().use_intra_process_comms(true))
  {
    // Create a publisher on the output topic.
    pub = this->create_publisher<std_msgs::msg::Int32>(out, 10);
    std::weak_ptr<std::remove_pointer<decltype(pub.get())>::type> captured_pub = pub;
    // Create a subscription on the input topic.
    sub = this->create_subscription<std_msgs::msg::Int32>(
      in,
      10,
      [captured_pub](std_msgs::msg::Int32::UniquePtr msg) {
        auto pub_ptr = captured_pub.lock();
        if (!pub_ptr) {
          return;
        }
        printf(
          "Received message with value:         %d, and address: 0x%" PRIXPTR "\n", msg->data,
          reinterpret_cast<std::uintptr_t>(msg.get()));
        printf("  sleeping for 1 second...\n");
        if (!rclcpp::sleep_for(1s)) {
          return;    // Return if the sleep failed (e.g. on :kbd:`ctrl-c`).
        }
        printf("  done.\n");
        msg->data++;    // Increment the message's data.
        printf(
          "Incrementing and sending with value: %d, and address: 0x%" PRIXPTR "\n", msg->data,
          reinterpret_cast<std::uintptr_t>(msg.get()));
        pub_ptr->publish(std::move(msg));    // Send the message along to the output topic.
      });
  }

  rclcpp::Publisher<std_msgs::msg::Int32>::SharedPtr pub;
  rclcpp::Subscription<std_msgs::msg::Int32>::SharedPtr sub;
};

int main(int argc, char * argv[])
{
  setvbuf(stdout, NULL, _IONBF, BUFSIZ);
  rclcpp::init(argc, argv);
  rclcpp::executors::SingleThreadedExecutor executor;

  // Create a simple loop by connecting the in and out topics of two IncrementerPipe's.
  // The expectation is that the address of the message being passed between them never changes.
  auto pipe1 = std::make_shared<IncrementerPipe>("pipe1", "topic1", "topic2");
  auto pipe2 = std::make_shared<IncrementerPipe>("pipe2", "topic2", "topic1");
  rclcpp::sleep_for(1s);  // Wait for subscriptions to be established to avoid race conditions.
  // Publish the first message (kicking off the cycle).
  std::unique_ptr<std_msgs::msg::Int32> msg(new std_msgs::msg::Int32());
  msg->data = 42;
  printf(
    "Published first message with value:  %d, and address: 0x%" PRIXPTR "\n", msg->data,
    reinterpret_cast<std::uintptr_t>(msg.get()));
  pipe1->pub->publish(std::move(msg));

  executor.add_node(pipe1);
  executor.add_node(pipe2);
  executor.spin();

  rclcpp::shutdown();

  return 0;
}
```

与之前的演示不同的是,此演示只使用一个节点,两次以不同的名称和配置即时化。 `pipe1` -\> `pipe2` -\> `pipe1` ...在循环中。

线条 `pipe1->pub->publish(std::move(msg));` 将进程踢开,但从那时起,消息会由每个在自己的订阅回调范围内调用发布者在节点之间前后传递。

这里的预期是,节点一次传递一次消息,每次递增消息的值。因为消息正在发布并被订阅为 `unique_ptr` 开始时创建的相同信件被持续使用。

为了检验这些期望,让我们来做一下:

``` console
$ ros2 run intra_process_demo cyclic_pipeline
Published first message with value:  42, and address: 0x7fd2ce0a2bc0
Received message with value:         42, and address: 0x7fd2ce0a2bc0
  sleeping for 1 second...
  done.
Incrementing and sending with value: 43, and address: 0x7fd2ce0a2bc0
Received message with value:         43, and address: 0x7fd2ce0a2bc0
  sleeping for 1 second...
  done.
Incrementing and sending with value: 44, and address: 0x7fd2ce0a2bc0
Received message with value:         44, and address: 0x7fd2ce0a2bc0
  sleeping for 1 second...
  done.
Incrementing and sending with value: 45, and address: 0x7fd2ce0a2bc0
Received message with value:         45, and address: 0x7fd2ce0a2bc0
  sleeping for 1 second...
  done.
Incrementing and sending with value: 46, and address: 0x7fd2ce0a2bc0
Received message with value:         46, and address: 0x7fd2ce0a2bc0
  sleeping for 1 second...
  done.
Incrementing and sending with value: 47, and address: 0x7fd2ce0a2bc0
Received message with value:         47, and address: 0x7fd2ce0a2bc0
  sleeping for 1 second...
[...]
```

您应该看到每个迭代上的数字不断增加, 从42开始... 因为42, 并且整个时间重复使用相同的消息, 这一点从指针地址 所显示的不改变, 避免不必要的复制。

<span id="the-image-pipeline-demo"></span>

### 图像管道演示

在演示中, 我们将使用 OpenCV 来获取、注释并查看图像。

> **说明**
>
> 如果您在 macOS 上, 而这些示例没有效果, 或者您收到错误, 如 `ddsi_conn_write failed -1`,然后您需要增加您的系统宽度 UDP 包大小 :
>
> ``` console
> $ sudo sysctl -w net.inet.udp.recvspace=209715
> $ sudo sysctl -w net.inet.udp.maxdgram=65500
> ```
>
> 这些变化不会在重启后持续下去。

<span id="simple-pipeline"></span>

#### 简易管道

我们首先要用三个节点的管道, `camera_node` -\> `watermark_node` -\> `image_view_node`

那个... `camera_node` 从相机设备读取 `0` 在您的计算机上,写一些图像信息并发布。 `watermark_node` 订阅该表的输出 `camera_node` 并添加更多文本后再发布。 `image_view_node` 订阅该表的输出 `watermark_node`,将文字写到图像上,然后用图像可视化 `cv::imshow`.

在每个节点中,ROS消息的进程 ID 和指针地址都写在图像上 `cv::putText`. 水印和图像视图节点的设计是为了修改图像而无需复制,因此,只要节点处于同一过程,且图表仍按上面的草图排列,图像上刻的地址就应该都一样.

> **说明**
>
> 在一些系统中(我们已经看到在Linux上发生),打印到屏幕上的地址可能不会改变。这是因为同样的唯一指针正在被重用。在这种情况下,管道仍在运行。

让我们通过执行以下可执行文件来运行演示:

``` console
$ ros2 run intra_process_demo image_pipeline_all_in_one
```

你应该看到这样的东西:

![](images/intra-process-demo-pipeline-single-window.png)

您可以通过按空格来暂停图像的渲染, 您也可以通过按空格来恢复。 您也可以按 `q` 或 时 间 `ESC` 准备离开。

如果你暂停图像查看器,你应该能够比较图像上写的地址,并看到它们是一样的.

<span id="pipeline-with-two-image-viewers"></span>

#### 带两个图像查看器的管道

现在让我们看看一个和上面的例子一样的例子,除了它有两个图像视图节点。所有的节点都还在同一个过程中,但现在会有两种例子。 `image_view_node` 因此,有两个图像视图窗口应该显示 。 (macOS 用户注意: 您的图像视图窗口可能位于顶端 ) 让我们用命令运行它 :

``` console
$ ros2 run intra_process_demo image_pipeline_with_two_image_view
```

![](images/intra-process-demo-pipeline-two-windows-copy.png)

和上一个例子一样,您可以暂停与空间栏的渲染,再按一次空间栏。您可以停止更新以检查写在屏幕上的指针。

正如您从上面的例子图像中看到的,我们有一个图像,所有指针都一样,然后是另一个与前两个条目的第一个图像一样的指针,但第二个图像上最后一个指针是不同的。要理解为什么会发生这种情况,要考虑到图形的地形:

``` bash
camera_node -> watermark_node -> image_view_node
                              -> image_view_node2
```

与《公约》的联系 `camera_node` 页:1 `watermark_node` 可以使用同一指针而无需复制,因为只有一种程序内订阅可以发送信件。但是,对于信件之间的链接, `watermark_node` 和两个图像视图节点 关系是一对多, 所以如果图像视图节点正在使用 `unique_ptr` 调用后, 无法将同一指针的所有权交付给两者。 但是, 也可以交付给其中之一 。 将获得原始指针的哪个没有定义, 而仅仅是最后交付的。 因此, 被查看的图像之一是原始的, 所有指针都是一样的, 另一个是原始图像的复制件, 制成于 `watermark_node` 和其中一个 `image_view_node` 实例,该实例对文本第三行会有不同的指针。

<span id="pipeline-with-inter-process-viewer"></span>

#### 带进程间查看器的管道

获得正确性的另一件重要事情是避免在进行进程间订阅时中断进程内部的零复制行为。为了测试这一点,我们可以运行第一个图像管道演示, `image_pipeline_all_in_one`,然后运行一个独立实例 `image_view_node` (不要忘记用前缀) `ros2 run intra_process_demo` 这看起来会是这样的:

![](images/intra-process-demo-pipeline-inter-process.png)

很难同时暂停两个图像, 所以图像可能不排队, 但重要的是要注意的是, `image_pipeline_all_in_one` 图像视图显示每个步骤的地址相同。这意味着即使外部视图被订阅,进程内部的零副本也会保留。您也可以看到,进程间图像视图对文本前两行有不同的进程ID,对文本第三行的独立图像查看器也有不同的进程ID。
