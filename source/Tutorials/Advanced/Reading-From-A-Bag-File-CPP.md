---
translation_status: machine_translated
source: Tutorials/Advanced/Reading-From-A-Bag-File-CPP.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="reading-from-a-bag-file-c"></span>

# 读取 bag 文件（C++）

**目标：** 在不使用CLI的情况下从包中读取数据.

**教程级别：** 高级

**用时：** 10分钟

<span id="background"></span>

## 背景

`rosbag2` 不只是提供 `ros2 bag` 命令行工具。它也提供了一个 C++ API ,用于从您的源代码中读取和写入一个包。这允许您从一个包中读取内容,而无需播放包,这有时是有用的。

<span id="prerequisites"></span>

## 前提条件

你应该有 `rosbag2` 作为常规 ROS 2 设置的一部分而安装的软件包。

如果需要安装ROS 2,请查看 [安装指令](../../Installation.md).

你应该已经完成了 [基本 ROS 2 袋教程](../Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.md),我们将使用 `subset` 你在那里创建的包。

<span id="tasks"></span>

## 操作步骤

<span id="create-a-package"></span>

### 1 创建软件包

打开一个新的终端 [源代码 ROS 2 安装](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md) 这样一来 `ros2` 命令会起作用的。

在新的或现有的 [工作空间](../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md#new-directory),导航到 `src` 目录和创建新软件包 :

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 bag_reading_cpp --dependencies rclcpp rosbag2_transport turtlesim
```

您的终端将返回一个消息, 以验证您的软件包的创建 `bag_reading_cpp` 及其所有必要的文件和文件夹。 `--dependencies` 参数将自动添加必要的依赖线到 `package.xml` 财务报告和财务报告 `CMakeLists.txt`。在这种情况下,软件包将使用 `rosbag2_transport` 软件包和软件包 `rclcpp` 软件包。 `turtlesim` 软件包也用于处理自定义的tolsim消息。

<span id="update-package-xml"></span>

#### 1.1 最新情况 `package.xml`

因为你用了 `--dependencies` 在创建软件包时,您不需要手动添加依赖性到 `package.xml` 或 时 间 `CMakeLists.txt`但是,一如既往,确保添加描述、维护者电子邮件和姓名,并给信息发放许可证。 `package.xml`.

``` xml
<description>C++ bag reading tutorial</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache-2.0</license>
```

<span id="write-the-c-reader"></span>

### 2 写入 C++ 阅读器

在你的包裹里 `src` 目录,创建名为新文件 `simple_bag_reader.cpp` 并粘贴下面的代码。

``` C++
#include <chrono>
#include <functional>
#include <iostream>
#include <memory>
#include <string>

#include "rclcpp/rclcpp.hpp"
#include "rclcpp/serialization.hpp"
#include "rosbag2_transport/reader_writer_factory.hpp"
#include "turtlesim/msg/pose.hpp"

using namespace std::chrono_literals;

class PlaybackNode : public rclcpp::Node
{
  public:
    PlaybackNode(const std::string & bag_filename)
    : Node("playback_node")
    {
      publisher_ = this->create_publisher<turtlesim::msg::Pose>("/turtle1/pose", 10);
      timer_ = this->create_wall_timer(
          100ms, std::bind(&PlaybackNode::timer_callback, this));

      rosbag2_storage::StorageOptions storage_options;
      storage_options.uri = bag_filename;
      reader_ = rosbag2_transport::ReaderWriterFactory::make_reader(storage_options);
      reader_->open(storage_options);
    }

  private:
    void timer_callback()
    {
      while (reader_->has_next()) {
        rosbag2_storage::SerializedBagMessageSharedPtr msg = reader_->read_next();

        if (msg->topic_name != "/turtle1/pose") {
          continue;
        }

        rclcpp::SerializedMessage serialized_msg(*msg->serialized_data);
        turtlesim::msg::Pose::SharedPtr ros_msg = std::make_shared<turtlesim::msg::Pose>();

        serialization_.deserialize_message(&serialized_msg, ros_msg.get());

        publisher_->publish(*ros_msg);
        std::cout << '(' << ros_msg->x << ", " << ros_msg->y << ")\n";

        break;
      }
    }

    rclcpp::TimerBase::SharedPtr timer_;
    rclcpp::Publisher<turtlesim::msg::Pose>::SharedPtr publisher_;

    rclcpp::Serialization<turtlesim::msg::Pose> serialization_;
    std::unique_ptr<rosbag2_cpp::Reader> reader_;
};

int main(int argc, char ** argv)
{
  if (argc != 2) {
    std::cerr << "Usage: " << argv[0] << " <bag>" << std::endl;
    return 1;
  }

  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<PlaybackNode>(argv[1]));
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

那个... `#include` 上方的语句是软件包的依赖性。请注意包含来自 `rosbag2_transport` 用于处理袋文件所需的函数和结构的软件包。

下一行将创建节点, 从包文件中读取并播放数据 。

``` C++
class PlaybackNode : public rclcpp::Node
```

现在,我们可以创建一个计时器调用器,在10hz运行。我们的目标是重播一个信息给 `/turtle1/pose` 每次调用时都注意主题。请注意,构建器将一个路径作为参数带入袋文件。

``` C++
public:
  PlaybackNode(const std::string & bag_filename)
  : Node("playback_node")
  {
    publisher_ = this->create_publisher<turtlesim::msg::Pose>("/turtle1/pose", 10);
    timer_ = this->create_wall_timer(
        100ms, std::bind(&PlaybackNode::timer_callback, this));
```

我们还打开了建筑工的包 `rosbag2_transport::ReaderWriterFactory` 是一个能够根据存储选项构建一个压缩或未压缩的读取器或写入器的类.

``` C++
rosbag2_storage::StorageOptions storage_options;
storage_options.uri = bag_filename;
reader_ = rosbag2_transport::ReaderWriterFactory::make_reader(storage_options);
reader_->open(storage_options);
```

现在,在计时器回调中,我们通过包里的信息循环,直到我们读到从我们想要的主题中录制的信息。请注意,序列化的信息除了主题名称之外,还有时间戳元数据。

``` C++
void timer_callback()
{
  while (reader_->has_next()) {
    rosbag2_storage::SerializedBagMessageSharedPtr msg = reader_->read_next();

    if (msg->topic_name != "/turtle1/pose") {
      continue;
    }
```

然后我们建造一个 `rclcpp::SerializedMessage` 此外,我们还需要创建 ROS 2 的去序列化消息,它将保存我们去序列化的结果。然后,我们就可以将这两个对象传递给 `rclcpp::Serialization::deserialize_message` 方法。

``` C++
rclcpp::SerializedMessage serialized_msg(*msg->serialized_data);
turtlesim::msg::Pose::SharedPtr ros_msg = std::make_shared<turtlesim::msg::Pose>();

serialization_.deserialize_message(&serialized_msg, ros_msg.get());
```

最后,我们发布解序消息,并打印 xy 坐标到终端。我们也打破了循环,以便在下一个计时器回调时器时发布下一个消息。

``` C++
  publisher_->publish(*ros_msg);
  std::cout << '(' << ros_msg->x << ", " << ros_msg->y << ")\n";

  break;
}
```

我们还必须宣布整个节点所使用的私人变量。

``` C++
  rclcpp::TimerBase::SharedPtr timer_;
  rclcpp::Publisher<turtlesim::msg::Pose>::SharedPtr publisher_;

  rclcpp::Serialization<turtlesim::msg::Pose> serialization_;
  std::unique_ptr<rosbag2_cpp::Reader> reader_;
};
```

最后,我们创建了主要功能,该功能将检查用户是否为包文件路径通过一个参数并旋转我们的节点。

``` C++
int main(int argc, char ** argv)
{
  if (argc != 2) {
    std::cerr << "Usage: " << argv[0] << " <bag>" << std::endl;
    return 1;
  }

  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<PlaybackNode>(argv[1]));
  rclcpp::shutdown();

  return 0;
}
```

<span id="add-executable"></span>

#### 2.2 添加可执行文件

现在打开 `CMakeLists.txt` 文档。

附属区块下方包含: `find_package(rosbag2_transport REQUIRED)`,添加以下代码行。

``` console
add_executable(simple_bag_reader src/simple_bag_reader.cpp)
ament_target_dependencies(simple_bag_reader rclcpp rosbag2_transport turtlesim)

install(TARGETS
  simple_bag_reader
  DESTINATION lib/${PROJECT_NAME}
)
```

<span id="build-and-run"></span>

### 3 构建和运行

导航返回您工作空间的根并构建您的新软件包 。

##### Linux

``` console
$ colcon build --packages-select bag_reading_cpp
```

##### macOS

``` console
$ colcon build --packages-select bag_reading_cpp
```

##### Windows

``` console
$ colcon build --merge-install --packages-select bag_reading_cpp
```

下一步, 提供设置文件 。

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

现在,运行剧本,确保替换 `/path/to/subset` 和通往你的路径 `subset` 包包。 。 。 。

``` console
$ ros2 run bag_reading_cpp simple_bag_reader /path/to/subset
```

您应该看到印到控制台上的龟的(x,y)坐标.

<span id="summary"></span>

## 小结

您创建了 C++ 可执行文件, 读取了包中的数据。 然后您编译并运行了将一些信息从包中打印到控制台的可执行文件 。
