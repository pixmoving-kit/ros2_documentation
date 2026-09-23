---
translation_status: machine_translated
source: Tutorials/Beginner-Client-Libraries/Using-Parameters-In-A-Class-CPP.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-parameters-in-a-class-c"></span> <span id="cppparamnode"></span>

# 在类中使用参数（C++）

**目标：** 使用 C++ 创建和运行带有 ROS 参数的类.

**教程级别：** 入门

**用时：** 20分钟

<span id="background"></span>

## 背景

当自己做的时候 [节点](../Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md) 您有时需要添加可以从发射文件中设定的参数。

此教程将显示如何在 C++ 类中创建这些参数, 以及如何在发射文件中设置这些参数 。

<span id="prerequisites"></span>

## 前提条件

在之前的教程中,你学会了如何 [创建工作空间](Creating-A-Workspace/Creating-A-Workspace.md) 财务报告和财务报告 [创建软件包](Creating-Your-First-ROS2-Package.md)。您还了解到 [参数](../Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md) 及其在ROS 2系统中的功能.

<span id="tasks"></span>

## 操作步骤

<span id="create-a-package"></span>

### 1 创建软件包

打开一个新的终端 [源代码 ROS 2 安装](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md) 这样一来 `ros2` 命令会起作用的。

跟着 [这些指示](Creating-A-Workspace/Creating-A-Workspace.md#new-directory) 创建新工作空间 `ros2_ws`.

回顾 应在 `src` 目录,不是工作空间的根。导航到 `ros2_ws/src` 并创建新软件包 :

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 cpp_parameters --dependencies rclcpp
```

您的终端将返回一个消息, 以验证您的软件包的创建 `cpp_parameters` 以及所有必要的文件和文件夹。

那个... `--dependencies` 参数将自动添加必要的依赖线到 `package.xml` 财务报告和财务报告 `CMakeLists.txt`.

<span id="update-package-xml"></span>

#### 1.1 最新情况 `package.xml`

因为你用了 `--dependencies` 在创建软件包时,您不需要手动添加依赖性到 `package.xml` 或 时 间 `CMakeLists.txt`.

但是,与往常一样,确保添加描述、维护者电子邮件和姓名,并给信息发放许可证。 `package.xml`.

``` xml
<description>C++ parameter tutorial</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

<span id="write-the-c-node"></span>

### 2 写入 C++ 节点

内侧 `ros2_ws/src/cpp_parameters/src` 目录,创建名为新文件 `cpp_parameters_node.cpp` 并粘贴下列编码:

``` C++
#include <chrono>
#include <functional>
#include <string>

#include <rclcpp/rclcpp.hpp>

using namespace std::chrono_literals;

class MinimalParam : public rclcpp::Node
{
public:
  MinimalParam()
  : Node("minimal_param_node")
  {
    this->declare_parameter("my_parameter", "world");

    timer_ = this->create_wall_timer(
      1000ms, std::bind(&MinimalParam::timer_callback, this));
  }

  void timer_callback()
  {
    std::string my_param = this->get_parameter("my_parameter").as_string();

    RCLCPP_INFO(this->get_logger(), "Hello %s!", my_param.c_str());

    std::vector<rclcpp::Parameter> all_new_parameters{rclcpp::Parameter("my_parameter", "world")};
    this->set_parameters(all_new_parameters);
  }

private:
  rclcpp::TimerBase::SharedPtr timer_;
};

int main(int argc, char ** argv)
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<MinimalParam>());
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

那个... `#include` 顶端的语句是软件包的依赖性。

下一个代码块创建类和构造器。 此构造器的第一行创建一个带有名称的参数 `my_parameter` 和默认值 `world`。从默认值中推断出参数类型,因此在此情况下,参数类型将被设定为字符串类型。 `timer_` 初始化的时期为1000毫秒,从而导致 `timer_callback` 函数将每秒执行一次。

``` C++
class MinimalParam : public rclcpp::Node
{
public:
  MinimalParam()
  : Node("minimal_param_node")
  {
    this->declare_parameter("my_parameter", "world");

    timer_ = this->create_wall_timer(
      1000ms, std::bind(&MinimalParam::timer_callback, this));
  }
```

我们的第一线 `timer_callback` 函数获得参数 `my_parameter` 从节点,并储存在 `my_param`下一个 `RCLCPP_INFO` 函数确保该事件被记录。 `set_parameters` 函数然后设置参数 `my_parameter` 返回默认字符串值 `world`。如果用户外部更改了参数,这将保证它总是被重置为原参数。

``` C++
void timer_callback()
{
  std::string my_param = this->get_parameter("my_parameter").as_string();

  RCLCPP_INFO(this->get_logger(), "Hello %s!", my_param.c_str());

  std::vector<rclcpp::Parameter> all_new_parameters{rclcpp::Parameter("my_parameter", "world")};
  this->set_parameters(all_new_parameters);
}
```

最后一个是宣布 `timer_`.

``` C++
private:
  rclcpp::TimerBase::SharedPtr timer_;
```

跟着我们 `MinimalParam` 是我们的 `main`。这里,ROS 2是初始化的。 `MinimalParam` 类别是构建的,以及 `rclcpp::spin` 开始从节点处理数据。

``` C++
int main(int argc, char ** argv)
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<MinimalParam>());
  rclcpp::shutdown();
  return 0;
}
```

<span id="optional-add-parameterdescriptor"></span>

##### 2.1.1(备选) 添加参数描述符

可以选择设置参数的描述符。描述符允许您指定参数及其限制的文本描述,比如使其只读,指定范围等等。要做到这一点,构建符中的代码必须更改为:

``` C++
// ...

class MinimalParam : public rclcpp::Node
{
public:
  MinimalParam()
  : Node("minimal_param_node")
  {
    auto param_desc = rcl_interfaces::msg::ParameterDescriptor{};
    param_desc.description = "This parameter is mine!";

    this->declare_parameter("my_parameter", "world", param_desc);

    timer_ = this->create_wall_timer(
      1000ms, std::bind(&MinimalParam::timer_callback, this));
  }
```

其余代码保持不变。一旦运行了节点,您就可以运行 `ros2 param describe /minimal_param_node my_parameter` 以查看类型和描述。

<span id="add-executable"></span>

#### 2.2 添加可执行文件

现在打开 `CMakeLists.txt` 文件。在依赖下方 `find_package(rclcpp REQUIRED)` 添加以下代码行。

``` cmake
add_executable(minimal_param_node src/cpp_parameters_node.cpp)
ament_target_dependencies(minimal_param_node rclcpp)

install(TARGETS
    minimal_param_node
  DESTINATION lib/${PROJECT_NAME}
)
```

<span id="build-and-run"></span>

### 3 构建和运行

运行是好的做法 `rosdep` 在工作空间的根部(`ros2_ws`在建构前检查缺失的依赖性 :

##### Linux

``` console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

##### macOS

rosdep只运行在Linux上,所以可以提前跳到下一步.

##### Windows

rosdep只运行在Linux上,所以可以提前跳到下一步.

导航回你工作空间的根, `ros2_ws`,并构建您的新软件包:

##### Linux

``` console
$ colcon build --packages-select cpp_parameters
```

##### macOS

``` console
$ colcon build --packages-select cpp_parameters
```

##### Windows

``` console
$ colcon build --merge-install --packages-select cpp_parameters
```

打开新终端, 导航到 `ros2_ws`,并源代码设置文件 :

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

现在运行节点,终端应该返回 `Hello World` 消息每秒钟 :

``` console
 $ ros2 run cpp_parameters minimal_param_node
[INFO] [minimal_param_node]: Hello world!
```

现在您可以看到您参数的默认值, 但是您想要自己设置它。 有两种方法可以实现 。

<span id="change-via-the-console"></span>

#### 3.1 通过控制台进行更改

这部分将利用你从 [关于参数的教程](../Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md) 并将其应用到您刚刚创建的节点上。

确保节点运行中 :

``` console
$ ros2 run cpp_parameters minimal_param_node
```

打开另一个终端, 从内部源出设置文件 `ros2_ws` ,并输入以下行:

``` console
$ ros2 param list
```

您将在此看到自定义参数 `my_parameter`。为了改变它,只需在控制台上运行以下一行:

``` console
$ ros2 param set /minimal_param_node my_parameter earth
```

你知道,如果你得到输出它很好 `Set parameter successful`。如果查看另一个终端,则应当看到输出更改为 `[INFO] [minimal_param_node]: Hello earth!`

<span id="change-via-a-launch-file"></span>

#### 3.2 通过发射文件更改

也可以在发射文件中设置参数,但首先需要添加发射目录。 `ros2_ws/src/cpp_parameters/` 目录,创建新的目录,名为 `launch`中,创建名为“新文件”的文件 `cpp_parameters_launch.py`

``` python
from launch import LaunchDescription
from launch_ros.actions import Node


def generate_launch_description():
    return LaunchDescription([
        Node(
            package='cpp_parameters',
            executable='minimal_param_node',
            name='custom_minimal_param_node',
            output='screen',
            emulate_tty=True,
            parameters=[
                {'my_parameter': 'earth'}
            ]
        )
    ])
```

在这里,你可以看到,我们设置 `my_parameter` 改为: `earth` 当我们发射节点时 `minimal_param_node`。通过在下面增加两行,我们保证我们的输出在我们的控制台上打印。

``` python
output="screen",
emulate_tty=True,
```

现在打开 `CMakeLists.txt` 文件。在您先前添加的行下面,添加以下的行码。

``` cmake
install(
  DIRECTORY launch
  DESTINATION share/${PROJECT_NAME}
)
```

打开一个控制台 导航到您工作空间的根, `ros2_ws`,并构建您的新软件包:

##### Linux

``` console
$ colcon build --packages-select cpp_parameters
```

##### macOS

``` console
$ colcon build --packages-select cpp_parameters
```

##### Windows

``` console
$ colcon build --merge-install --packages-select cpp_parameters
```

然后从新终端中获取设置文件 :

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

现在使用我们刚刚创建的发射文件运行节点。 终端应该第一次返回以下信息 :

``` console
$ ros2 launch cpp_parameters cpp_parameters_launch.py
[INFO] [custom_minimal_param_node]: Hello earth!
```

其他产出应显示 `[INFO] [minimal_param_node]: Hello world!` 每一秒钟。

<span id="summary"></span>

## 小结

您创建了一个自定义参数的节点, 可以从发射文件或命令行中设置。 您在软件包配置文件中添加了依赖性、 可执行文件以及启动文件, 以便构建和运行它们, 并在操作中看到参数 。

<span id="next-steps"></span>

## 后续步骤

现在,你有一些包 和ROS 2系统你自己的, [下一个教程](Getting-Started-With-Ros2doctor.md) 将教你如何检查环境和系统中的问题,以防出现问题。
