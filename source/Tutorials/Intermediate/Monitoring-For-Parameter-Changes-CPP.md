---
translation_status: machine_translated
source: Tutorials/Intermediate/Monitoring-For-Parameter-Changes-CPP.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="monitoring-for-parameter-changes-c"></span>

# 监控参数变化（C++）

**目标：** 学习使用参数EventHandler类来监视和响应参数变化.

**教程级别：** 中级

**用时：** 20分钟

**最小平台 :** 银河

<span id="background"></span>

## 背景

一个节点通常需要响应它自己的参数或另一个节点参数的改变。参数EventHandler类可以方便地听取参数的改变,这样你的代码就可以响应这些变化。这个教程将显示如何使用参数EventHandler类的C++版本来监视一个节点自己的参数的改变以及另一个节点参数的改变。

<span id="prerequisites"></span>

## 前提条件

在开始此教程之前, 您应该先完成以下教程 :

- [理解参数](../Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md)

- [在类中使用参数（C++）](../Beginner-Client-Libraries/Using-Parameters-In-A-Class-CPP.md)

此外,您必须运行 ROS 2 的银河分布.

<span id="tasks"></span>

## 操作步骤

在此教程中, 您将创建新包, 包含一些样本代码, 编写一些 C++ 代码, 使用 ParameterEventHandler 类, 并测试生成的代码 。

<span id="create-a-package"></span>

### 1 创建软件包

首先,打开一个新的终端和 [源代码 ROS 2 安装](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md) 这样一来 `ros2` 命令会起作用的。

跟着 [这些指示](../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md#new-directory) 创建新工作空间 `ros2_ws`.

回顾 应在 `src` 目录,不是工作空间的根。所以,导航到 `ros2_ws/src` 然后在那里创建新软件包:

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 cpp_parameter_event_handler --dependencies rclcpp
```

您的终端将返回一个消息, 以验证您的软件包的创建 `cpp_parameter_event_handler` 以及所有必要的文件和文件夹。

那个... `--dependencies` 参数将自动添加必要的依赖线到 `package.xml` 财务报告和财务报告 `CMakeLists.txt`.

<span id="update-package-xml"></span>

#### 1.1 最新情况 `package.xml`

因为你用了 `--dependencies` 在创建软件包时,您不需要手动添加依赖性到 `package.xml` 或 时 间 `CMakeLists.txt`但是,一如既往,确保添加描述、维护者电子邮件和姓名,并给信息发放许可证。 `package.xml`.

``` xml
<description>C++ parameter events client tutorial</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

<span id="write-the-c-node"></span>

### 2 写入 C++ 节点

内侧 `ros2_ws/src/cpp_parameter_event_handler/src` 目录,创建名为新文件 `parameter_event_handler.cpp` 并粘贴下列编码:

``` C++
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

#### 2.1 审查守则

第一次发言, `#include <memory>` 包含其中,以便代码能够使用 std: make_shared 模板。下一个, `#include "rclcpp/rclcpp.hpp"` 包含用于允许代码引用 rclcpp 界面提供的各种功能,包括 ParameterEventHandler 类。

> **说明**
>
> `rclcpp/rclcpp.hpp` 是一个 *便利性* 头部 整个都拉着 `rclcpp` API同时——节点,出版商,订阅,服务,定时器,参数,执行器,速率,等位集,等等——所以每个包含它的翻译单元都是根据它从未使用过的特性编译的.
>
> 在教程之外, 偏爱只包含您实际使用的 API 特定调用时的页眉 。 例如, `rclcpp::Node` 已声明为 `rclcpp/node.hpp`, `rclcpp::spin` 输入 `rclcpp/executors.hpp`,以及 `rclcpp::init` 财务报告和财务报告 `rclcpp::shutdown` 输入 `rclcpp/utilities.hpp`。保存量最大的是从未创建或旋转节点的翻译单位——标题、插件和辅助工具库,它们只需要像 `rclcpp/qos.hpp` 或 时 间 `rclcpp/time.hpp` - 因为... `rclcpp/node.hpp` 财务报告和财务报告 `rclcpp/executors.hpp` 他们本身就很大。 `rclcpp/rclcpp.hpp` 只不过是这些信头的列表,所以在研究你需要哪个信头的时候,这是一个很好的开始。

班级声明后,代码定义一个班级, `SampleNodeWithParameters`。该类的构造器宣布一个整数参数 `an_int_param`,默认值为 0。下一步,代码创建一个 `ParameterEventHandler` 用于监视参数的更改。最后,代码创建了 lambda 函数,并将其设定为每次引用时的回调。 `an_int_param` 将更新。

> **说明**
>
> 保存返回的手柄非常重要 `add_parameter_callback`;否则,回调不会被适当注册.

``` C++
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

紧接着 `SampleNodeWithParameters` 是一个典型的 `main` 函数初始化 ROS,旋转样本节点,以便发送和接收消息,然后在用户进入控制台的 ^C 后关闭。

``` C++
int main(int argc, char ** argv)
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<SampleNodeWithParameters>());
  rclcpp::shutdown();

  return 0;
}
```

<span id="add-executable"></span>

#### 2.2 添加可执行文件

要构建此代码,首先打开 `CMakeLists.txt` 文档,并在依赖性下方添加以下代码行 `find_package(rclcpp REQUIRED)`

``` console
add_executable(parameter_event_handler src/parameter_event_handler.cpp)
ament_target_dependencies(parameter_event_handler rclcpp)

install(TARGETS
  parameter_event_handler
  DESTINATION lib/${PROJECT_NAME}
)
```

<span id="build-and-run"></span>

### 3 构建和运行

运行是好的做法 `rosdep` 在工作空间的根部(`ros2_ws`在建构前检查缺失的依赖性 :

##### Linux

``` console
$ rosdep install -i --from-path src --rosdistro $ROS_DISTRO -y
```

##### macOS

rosdep只运行在Linux上,所以可以提前跳到下一步.

##### Windows

rosdep只运行在Linux上,所以可以提前跳到下一步.

导航回你工作空间的根, `ros2_ws`,并构建您的新软件包:

``` console
$ colcon build --packages-select cpp_parameter_event_handler
```

打开新终端, 导航到 `ros2_ws`,并源代码设置文件 :

##### Linux

``` console
$ . install/setup.bash
```

##### macOS

``` console
$ . install/setup.bash
```

##### Windows

``` console
$ call install/setup.bat
```

现在运行节点:

``` console
$ ros2 run cpp_parameter_event_handler parameter_event_handler
```

节点现在已激活, 并有一个单一参数, 每当更新此参数时都会打印一个消息 。 要测试, 请打开另一个终端并像以前一样源源 ROS 设置文件( ROS) 。`. install/setup.bash`)并执行以下命令:

``` console
$ ros2 param set node_with_parameters an_int_param 43
```

运行节点的终端将显示类似以下的信件:

``` console
[INFO] [1606950498.422461764] [node_with_parameters]: cb: Received an update to parameter "an_int_param" of type integer: "43"
```

我们先前在节点中设置的调用符已被引用并显示新的更新值。 您现在可以在终端中使用 ^C 终止运行的参数\_ event\_ handler 样本 。

<span id="extensions"></span>

## 扩展

迄今为止,我们建造并测试了一个小节点,用来监视节点本身拥有的单个参数。下面将用这个节点作为基准,介绍另外两个可以使用参数EventHandler的用例。

<span id="monitor-changes-to-another-node-s-parameters"></span>

### 监视另一个节点参数的更改

您也可以使用参数EventHandler来监视另一个节点参数的参数变化。 让我们更新“标本节点”类, 也监视另一个节点参数的变化。 我们将使用参数\_ 黑板演示应用程序来托管我们将监测更新的双参数 。

第一次更新构造器以在现有代码后添加以下代码:

``` C++
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

然后添加另一个成员变量, `cb_handle2` 用于附加的回调控手柄:

``` C++
private:
  std::shared_ptr<rclcpp::ParameterEventHandler> param_subscriber_;
  std::shared_ptr<rclcpp::ParameterCallbackHandle> cb_handle_;
  std::shared_ptr<rclcpp::ParameterCallbackHandle> cb_handle2_;  // Add this
};
```

在终端,导航回你工作空间的根部, `ros2_ws`,并像以前一样构建更新的软件包:

``` console
$ colcon build --packages-select cpp_parameter_event_handler
```

然后源代码设置文件 :

##### Linux

``` console
$ . install/setup.bash
```

##### macOS

``` console
$ . install/setup.bash
```

##### Windows

``` console
$ call install/setup.bat
```

现在,为了测试远程参数的监测,首先运行新建的参数_event_handler代码:

``` console
$ ros2 run cpp_parameter_event_handler parameter_event_handler
```

接下来,从另一个终端(已初始化ROS)运行参数_黑板演示应用程序如下:

``` console
$ ros2 run demo_nodes_cpp parameter_blackboard
```

最后,从第三个终端(ROS已初始化),让我们在参数_黑板节点上设置一个参数:

``` console
$ ros2 param set parameter_blackboard a_double_param 3.45
```

执行此命令时, 您应该在参数\_ event\_ handler 窗口中看到输出, 显示在参数更新时引用了召回函数 :

``` console
[INFO] [1606952588.237531933] [node_with_parameters]: cb2: Received an update to parameter "a_double_param" of type: double: "3.45"
```

<span id="monitor-all-node-parameters-simultaneously"></span>

### 同步监视所有节点参数

如果你需要同时监视多个节点或参数,那么不得不调用会很麻烦 `add_parameter_callback` 每人一次,在这种情况下,你可以使用 `add_parameter_event_callback` 以注册一个单调回调值,当 *任何* 参数 *任何* 节点变化.

为此,首先更新标本节点With Parameters构建器,以添加以下代码: 1.

``` C++
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

此声明一个新的双参数 `another_double_param` 并添加一个事件召回,以监视这两个参数。请注意 `parameter_event` 类型 [rcl_interfaces/msg/ParameterEvent](https://docs.ros.org/en/rolling/p/rcl_interfaces/msg/ParameterEvent.html)。尽管在此教程中未显示, 事件召回也可以用于监视参数的添加或删除 。

以私人身份加入事件召回手柄:

``` C++
private:
  ...
  std::shared_ptr<rclcpp::ParameterEventCallbackHandle> event_cb_handle_;
```

导航回你工作空间的根, `ros2_ws`,并像以前一样重建更新的软件包:

``` console
$ colcon build --packages-select cpp_parameter_event_handler
```

然后源代码设置文件 :

##### Linux

``` console
$ . install/setup.bash
```

##### macOS

``` console
$ . install/setup.bash
```

##### Windows

``` console
$ call install\setup.bat
```

要测试新事件召回, 请先运行参数\_ event\_ handler 节点 :

``` console
$ ros2 run cpp_parameter_event_handler parameter_event_handler
```

然后,从第二个终端(使用ROS源代码),让我们设置原始的内置参数:

``` console
$ ros2 param set node_with_parameters an_int_param 44
```

执行此命令时, 您应该看到单参数回调, 以及事件回调被发射 :

``` console
[INFO] [1747144403.418980063] [node_with_parameters]: cb: Received an update to parameter "an_int_param" of type integer: "44"
[INFO] [1747144403.419086611] [node_with_parameters]: Received parameter event from node "/node_with_parameters"
[INFO] [1747144403.419114103] [node_with_parameters]: Inside event: "an_int_param" changed to 44
```

现在设置新的双参数 :

``` console
$ ros2 param set node_with_parameters another_double_param 4.4
```

由于未添加单参数回调(通过 `add_parameter_callback`) 对于双参数,我们应该只看到事件召回火:

``` console
[INFO] [1747144452.917437113] [node_with_parameters]: Received parameter event from node "/node_with_parameters"
[INFO] [1747144452.917591649] [node_with_parameters]: Inside event: "another_double_param" changed to 4.400000
```

> **说明**
>
> 当同时设置多个参数时,最好使用 `set_parameters_atomically`,解释在 [参数](../../Concepts/Basic/About-Parameters.md)。这样,事件召回只能发射一次。

<span id="summary"></span>

## 小结

您创建了一个带有参数的节点, 并使用 ParameterEventHandler 类设置了调用符来监视该参数的更改。 您还使用同一类来监视远程节点的更改, 并监视单个事件调用符中的所有参数。 ParameterEventHandler 是监视参数更改的方便方式, 这样您就可以对更新的值做出响应 。

<span id="related-content"></span>

## 相关内容

要学习如何修改 ROS 1 的 ROS 2 参数文件,请参见 [将 YAML 参数文件从 ROS 1 移动到 ROS 2](../../How-To-Guides/Migrating-from-ROS1/Migrating-Parameters.md) 教学。
