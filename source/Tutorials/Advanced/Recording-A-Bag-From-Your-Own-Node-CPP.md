---
translation_status: machine_translated
source: Tutorials/Advanced/Recording-A-Bag-From-Your-Own-Node-CPP.rst
---

<span id="recording-a-bag-from-a-node-c"></span> <span id="ros2bagownnode"></span>

# 在节点中录制 bag（C++）

**目标：** 从自己的 C++ 节点记录数据到一个包 。

**教程级别：** 高级

**用时：** 20分钟

<span id="background"></span>

## 背景

`rosbag2` 不只是提供 `ros2 bag` 命令行工具。它也提供了一个 C++ API ,用于从您的源代码中读取和写入一个包。这使得您可以订阅一个话题,并将所收到的数据保存到一个包中,同时在数据中执行您选择的任何其他处理。

<span id="prerequisites"></span>

## 前提条件

你应该有 `rosbag2` 作为常规 ROS 2 设置的一部分而安装的软件包。

如果您已经从 Linux 上的 deb 软件包中安装, 它可能默认会被安装。 如果不是, 您可以使用此命令安装 。

``` console
$ sudo apt install ros-rolling-rosbag2
```

此教程使用 ROS 2 袋讨论, 包括来自终端。 您应该已经完成 [基本 ROS 2 袋教程](../Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.md).

<span id="tasks"></span>

## 操作步骤

<span id="create-a-package"></span>

### 1 创建软件包

打开一个新的终端 [源代码 ROS 2 安装](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md) 这样一来 `ros2` 命令会起作用的。

导航到 `ros2_ws` 在 a 中创建目录 [上一个教程](../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md#new-directory)。引导到 `ros2_ws/src` 目录和创建新软件包 :

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 bag_recorder_nodes --dependencies example_interfaces rclcpp rosbag2_cpp std_msgs
```

您的终端将返回一个消息, 以验证您的软件包的创建 `bag_recorder_nodes` 及其所有必要的文件和文件夹。 `--dependencies` 参数将自动添加必要的依赖线到 `package.xml` 财务报告和财务报告 `CMakeLists.txt`。在这种情况下,软件包将使用 `rosbag2_cpp` 软件包和软件包 `rclcpp` 软件包。 `example_interfaces` 此教程的后期部分也需要软件包。

<span id="update-package-xml"></span>

#### 1.1 最新情况 `package.xml`

因为你用了 `--dependencies` 在创建软件包时,您不需要手动添加依赖性到 `package.xml` 或 时 间 `CMakeLists.txt`但是,一如既往,确保添加描述、维护者电子邮件和姓名,并给信息发放许可证。 `package.xml`.

``` xml
<description>C++ bag writing tutorial</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

<span id="write-the-c-node"></span>

### 2 写入 C++ 节点

内侧 `ros2_ws/src/bag_recorder_nodes/src` 目录,创建名为新文件 `simple_bag_recorder.cpp` 并粘贴下面的代码。

``` C++
#include <rclcpp/rclcpp.hpp>
#include <std_msgs/msg/string.hpp>

#include <rosbag2_cpp/writer.hpp>

using std::placeholders::_1;

class SimpleBagRecorder : public rclcpp::Node
{
public:
  SimpleBagRecorder()
  : Node("simple_bag_recorder")
  {
    writer_ = std::make_unique<rosbag2_cpp::Writer>();

    writer_->open("my_bag");

    subscription_ = create_subscription<std_msgs::msg::String>(
      "chatter", 10, std::bind(&SimpleBagRecorder::topic_callback, this, _1));
  }

private:
  void topic_callback(std::shared_ptr<rclcpp::SerializedMessage> msg) const
  {
    rclcpp::Time time_stamp = this->now();

    writer_->write(msg, "chatter", "std_msgs/msg/String", time_stamp);
  }

  rclcpp::Subscription<std_msgs::msg::String>::SharedPtr subscription_;
  std::unique_ptr<rosbag2_cpp::Writer> writer_;
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<SimpleBagRecorder>());
  rclcpp::shutdown();
  return 0;
}
```

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

<span id="examine-the-code"></span>

#### 2.1 审查守则

那个... `#include` 上方的语句是软件包的依赖性。请注意包含来自 `rosbag2_cpp` 用于处理袋文件所需的函数和结构的软件包。

在类构造器中,我们首先创建我们用来写到包里的写作对象。

``` C++
writer_ = std::make_unique<rosbag2_cpp::Writer>();
```

既然我们有了写器对象, 我们可以用它打开包。 我们只指定要创建的包的 URI , 在默认情况下留下其他选项。 默认的存储选项被使用, 这意味着 `sqlite3`-format 包将被创建。 默认的转换选项也会被使用, 它将不进行转换, 而是以序列化格式存储收到的信息 。

``` C++
writer_->open("my_bag");
```

随着编剧的设立来记录我们传递到它的数据,我们创建一个订阅并指定一个回调。我们将在回调中将数据写到包中。

``` C++
subscription_ = create_subscription<std_msgs::msg::String>(
  "chatter", 10, std::bind(&SimpleBagRecorder::topic_callback, this, _1));
```

回调本身与典型的回调不同。我们不是收到专题数据类型的实例,而是收到一个实例。 `rclcpp::SerializedMessage`我们这样做有两个原因。

1.  消息数据需要按下列顺序排列: `rosbag2` 在被写到袋子之前, 我们要求ROS在接收数据时 而不是将其解序, 然后重排序列时, 我们只要给我们序列化的信息。

2.  编剧API可以接受串行消息.

``` C++
void topic_callback(std::shared_ptr<rclcpp::SerializedMessage> msg) const
{
```

在订阅回调中, 第一件事就是确定存储信件所用的时间戳。 这可以是适合您数据的任何内容, 但有两个共同的值是数据生成的时间( 如果知道的话) 和接收的时间。 在此使用第二个选项, 接收时间 。

``` C++
rclcpp::Time time_stamp = this->now();
```

我们可以将消息写入包中。 由于我们还没有将任何主题与包一起登记, 我们必须用信息指定全部主题信息 。 这就是为什么我们通过主题名称和主题类型 。

``` C++
writer_->write(msg, "chatter", "std_msgs/msg/String", time_stamp);
```

类包含两个成员变量.

1.  订阅对象。 请注意, 模板参数是调用对象的类型, 而不是主题的类型 。 在这种情况下, 调用对象收到 `rclcpp::SerializedMessage` 共享指针,所以模板参数必须如此.

2.  用于写入包的刻录对象的管理指针 。 注意这里使用的刻录器类型是 `rosbag2_cpp::Writer`,一般写作界面。其他写作人可能有不同的行为。

``` C++
rclcpp::Subscription<std_msgs::msg::String>::SharedPtr subscription_;
std::unique_ptr<rosbag2_cpp::Writer> writer_;
```

文件以 `main` 函数用于创建节点实例并启动ROS处理。

``` C++
int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<SimpleBagRecorder>());
  rclcpp::shutdown();
  return 0;
}
```

<span id="add-executable"></span>

#### 2.2 添加可执行文件

现在打开 `CMakeLists.txt` 文档。

在文件的顶部附近, 更改 `CMAKE_CXX_STANDARD` 从 `14` 改为: `17`.

``` cmake
# Default to C++17
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()
```

附属区块下方包含: `find_package(rosbag2_cpp REQUIRED)`,添加以下代码行。

``` cmake
add_executable(simple_bag_recorder src/simple_bag_recorder.cpp)
ament_target_dependencies(simple_bag_recorder rclcpp rosbag2_cpp std_msgs)

install(TARGETS
  simple_bag_recorder
  DESTINATION lib/${PROJECT_NAME}
)
```

<span id="build-and-run"></span>

### 3 构建和运行

导航回你工作空间的根, `ros2_ws`,并构建您的新软件包。

##### Linux

``` console
$ colcon build --packages-select bag_recorder_nodes
```

##### macOS

``` console
$ colcon build --packages-select bag_recorder_nodes
```

##### Windows

``` console
$ colcon build --merge-install --packages-select bag_recorder_nodes
```

打开新终端, 导航到 `ros2_ws`,并源代码设置文件。

##### Linux

``` console
$ source install/setup.bash
```

##### macOS

``` console
$ source install/setup.bash
```

##### Windows

``` console
$ call install/setup.bat
```

现在运行节点:

``` console
$ ros2 run bag_recorder_nodes simple_bag_recorder
```

打开第二个终端并运行 `talker` 实例节点。

``` console
$ ros2 run demo_nodes_cpp talker
```

这将开始发布关于 `chatter` 主题。当袋写节点收到此数据时,它会将其写入 `my_bag` 包包。 。 。 。

终止两个节点。 然后,在一个终端开始 `listener` 实例节点。

``` console
$ ros2 run demo_nodes_cpp listener
```

在另一个终端,使用 `ros2 bag` 播放您节点记录的包。

``` console
$ ros2 bag play my_bag
```

你会看到从包里收到的信息 被人们收到 `listener` 节点。

如果您希望再次运行包写节点, 您首先需要删除 `my_bag` 目录。

<span id="record-synthetic-data-from-a-node"></span>

### 4 记录一个节点的合成数据

任何数据都可以被记录到一个包中, 而不仅仅是在某个话题下收到的数据。 从您自己的节点写入一个包的常用例是生成和存储合成数据。 在本节中, 您将学习如何写入一个生成一些数据的节点, 并将其存储在一个包中。 我们将演示两种方法。 第一个方法是使用带有定时器的节点; 如果数据生成在节点之外, 您将使用这种方法, 如直接从硬件( 如相机) 读取数据, 第二种方法是不使用节点; 这是不需要使用ROS 基础设施的任何功能时您可以使用的方法 。

<span id="write-a-c-node"></span>

#### 4.1 写入 C++ 节点

内侧 `ros2_ws/src/bag_recorder_nodes/src` 目录,创建名为新文件 `data_generator_node.cpp` 并粘贴下面的代码。

``` C++
#include <chrono>

#include <example_interfaces/msg/int32.hpp>
#include <rclcpp/rclcpp.hpp>

#include <rosbag2_cpp/writer.hpp>

using namespace std::chrono_literals;

class DataGenerator : public rclcpp::Node
{
public:
  DataGenerator()
  : Node("data_generator")
  {
    data_.data = 0;
    writer_ = std::make_unique<rosbag2_cpp::Writer>();

    writer_->open("timed_synthetic_bag");

    writer_->create_topic(
      {"synthetic",
       "example_interfaces/msg/Int32",
       rmw_get_serialization_format(),
       ""});

    timer_ = create_wall_timer(1s, std::bind(&DataGenerator::timer_callback, this));
  }

private:
  void timer_callback()
  {
    writer_->write(data_, "synthetic", now());

    ++data_.data;
  }

  rclcpp::TimerBase::SharedPtr timer_;
  std::unique_ptr<rosbag2_cpp::Writer> writer_;
  example_interfaces::msg::Int32 data_;
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<DataGenerator>());
  rclcpp::shutdown();
  return 0;
}
```

<span id="id1"></span>

#### 4.2 审查守则

此代码大部分与第一例相同,重要差异在此描述.

一,包头改名.

``` C++
writer_->open("timed_synthetic_bag");
```

在这个例子中,我们正在预先将这个议题与包一起登记,这在多数情况下是可选的,但必须在没有专题信息的情况下用序列式信息传递时进行。

``` C++
writer_->create_topic(
  {"synthetic",
   "example_interfaces/msg/Int32",
   rmw_get_serialization_format(),
   ""});
```

此节点没有订阅一个话题,而是有一个定时器。 定时器有1秒钟的时间段起火, 并在指定成员时调用其功能 。

``` C++
timer_ = create_wall_timer(1s, std::bind(&DataGenerator::timer_callback, this));
```

在定时器调用内, 我们生成( 或以其他方式获取, 例如从连接到某些硬件的序列端口读取) 我们希望存储到包中的数据 。 这和前一个样本的重要区别在于, 数据尚未序列化 。 相反, 我们正将 ROS 消息数据类型传递给写入对象, 在此例中 。 `example_interfaces/msg/Int32`编剧会为我们整理数据 然后再写到袋子里

``` C++
writer_->write(data_, "synthetic", now());
```

<span id="id2"></span>

#### 4.3 添加可执行文件

打开 `CMakeLists.txt` 文件,并在先前添加的行后添加以下行(具体地说,在 `install(TARGETS ...)` 宏调用).

``` cmake
add_executable(data_generator_node src/data_generator_node.cpp)
ament_target_dependencies(data_generator_node rclcpp rosbag2_cpp example_interfaces)

install(TARGETS
  data_generator_node
  DESTINATION lib/${PROJECT_NAME}
)
```

<span id="id3"></span>

#### 4.4 建设和运行

导航回你工作空间的根, `ros2_ws`,并构建您的软件包。

##### Linux

``` console
$ colcon build --packages-select bag_recorder_nodes
```

##### macOS

``` console
$ colcon build --packages-select bag_recorder_nodes
```

##### Windows

``` console
$ colcon build --merge-install --packages-select bag_recorder_nodes
```

打开新终端, 导航到 `ros2_ws`,并源代码设置文件。

##### Linux

``` console
$ source install/setup.bash
```

##### macOS

``` console
$ source install/setup.bash
```

##### Windows

``` console
$ call install/setup.bat
```

(如果 `timed_synthetic_bag` 目录已经存在,您必须在运行节点前先删除它。 )

现在运行节点:

``` console
$ ros2 run bag_recorder_nodes data_generator_node
```

等待30秒左右,然后终止节点 <span class="kbd kbd docutils literal notranslate">缩略语</span>-<span class="kbd kbd docutils literal notranslate">c</span>。接下来,播放创建的包。

``` console
$ ros2 bag play timed_synthetic_bag
```

打开第二个终端并回声 `/synthetic` 主题。

``` console
$ ros2 topic echo /synthetic
```

您将看到生成并存储在打印到控制台的包中的数据, 速度为每秒一信。 Name

<span id="record-synthetic-data-from-an-executable"></span>

### 5 从可执行文件记录合成数据

现在,你可以创建一个包,存储来自一个非主题来源的数据,你会学习如何生成和记录一个非节点执行器的合成数据。这种方法的优点是更简单的代码和快速创建大量数据。

<span id="write-a-c-executable"></span>

#### 5.1 写入 C++ 可执行文件

内侧 `ros2_ws/src/bag_recorder_nodes/src` 目录,创建名为新文件 `data_generator_executable.cpp` 并粘贴下面的代码。

``` C++
#include <chrono>

#include <rclcpp/clock.hpp>
#include <rclcpp/duration.hpp>
#include <rclcpp/time.hpp>
#include <example_interfaces/msg/int32.hpp>

#include <rosbag2_cpp/writer.hpp>
#include <rosbag2_cpp/writers/sequential_writer.hpp>
#include <rosbag2_storage/serialized_bag_message.hpp>

using namespace std::chrono_literals;

int main(int, char**)
{
  example_interfaces::msg::Int32 data;
  data.data = 0;
  std::unique_ptr<rosbag2_cpp::Writer> writer_ = std::make_unique<rosbag2_cpp::Writer>();

  writer_->open("big_synthetic_bag");

  writer_->create_topic(
    {"synthetic",
     "example_interfaces/msg/Int32",
     rmw_get_serialization_format(),
     ""});

  rclcpp::Clock clock;
  rclcpp::Time time_stamp = clock.now();
  for (int32_t ii = 0; ii < 100; ++ii) {
    writer_->write(data, "synthetic", time_stamp);
    ++data.data;
    time_stamp += rclcpp::Duration(1s);
  }

  return 0;
}
```

<span id="id4"></span>

#### 5.2 审查守则

将这个样本和之前的样本进行比较,可以发现它们没有那么不同。唯一显著的区别是使用循环驱动数据生成而不是定时器。

请注意, 我们现在正在为数据生成时间戳, 而不是依赖当前每个样本的系统时间。 时间戳可以是您需要的任意值。 数据会按这些时间戳给出的速度播放, 所以这是控制样本默认播放速度的有用方法 。 请注意, 虽然每个样本之间的间隔是完整的第二时间, 但是这个可执行文件不需要在每一个样本之间等待第二时间 。 这样我们就可以在比重播要短得多的时间里生成大量涵盖广泛时间段的数据 。

``` C++
rclcpp::Clock clock;
rclcpp::Time time_stamp = clock.now();
for (int32_t ii = 0; ii < 100; ++ii) {
  writer_->write(data, "synthetic", time_stamp);
  ++data.data;
  time_stamp += rclcpp::Duration(1s);
}
```

<span id="id5"></span>

#### 5.3 添加可执行文件

打开 `CMakeLists.txt` 在先前添加的行后添加以下行。

``` cmake
add_executable(data_generator_executable src/data_generator_executable.cpp)
ament_target_dependencies(data_generator_executable rclcpp rosbag2_cpp example_interfaces)

install(TARGETS
  data_generator_executable
  DESTINATION lib/${PROJECT_NAME}
)
```

<span id="id6"></span>

#### 5.4 构建和运行

导航回你工作空间的根, `ros2_ws`,并构建您的软件包。

##### Linux

``` console
$ colcon build --packages-select bag_recorder_nodes
```

##### macOS

``` console
$ colcon build --packages-select bag_recorder_nodes
```

##### Windows

``` console
$ colcon build --merge-install --packages-select bag_recorder_nodes
```

打开终端, 导航到 `ros2_ws`,并源代码设置文件。

##### Linux

``` console
$ source install/setup.bash
```

##### macOS

``` console
$ source install/setup.bash
```

##### Windows

``` console
$ call install/setup.bat
```

(如果 `big_synthetic_bag` 目录已存在, 您必须在运行可执行文件前先删除它 。)

现在运行可执行文件 :

``` console
$ ros2 run bag_recorder_nodes data_generator_executable
```

注意可执行文件运行和完成速度非常快 。

现在回放创造出来的包。

``` console
$ ros2 bag play big_synthetic_bag
```

打开第二个终端并回声 `/synthetic` 主题。

``` console
$ ros2 topic echo /synthetic
```

您将会看到在打印到控制台的袋子中生成和存储的数据, 速度为每秒一信。 尽管袋是迅速生成的, 但仍按邮票显示的速度播放 。

<span id="summary"></span>

## 小结

您创建了一个节点, 将它接收到的话题数据记录在一个包中。 您测试了使用节点记录一个包, 并且通过播放回放包来验证数据。 然后您继续创建一个节点和一个可执行文件, 生成合成数据并将其存储在一个包中 。
