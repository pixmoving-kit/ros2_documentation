<span id="using-parameters-in-a-class-c"></span> <span id="cppparamnode"></span>
# 在类中使用参数（C++）

**目标：** 使用 C++ 创建并运行一个包含 ROS 参数的类。

**教程级别：** 初学者

**预计用时：** 20 分钟

<span id="background"></span>
## 背景

编写自己的[节点](../Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md)时，有时需要添加能够通过 launch 文件设置的参数。本教程介绍如何在 C++ 类中创建这些参数，并在 launch 文件中设置它们。

<span id="prerequisites"></span>
## 前提条件

此前教程介绍了[创建工作空间](Creating-A-Workspace/Creating-A-Workspace.md)、[创建软件包](Creating-Your-First-ROS2-Package.md)，以及[参数](../Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md)在 ROS 2 系统中的作用。

<span id="tasks"></span>
## 操作步骤

<span id="create-a-package"></span>
### 1 创建软件包

打开新终端，[加载 ROS 2 安装环境](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)，使 `ros2` 命令可用。

按照[创建目录的步骤](Creating-A-Workspace/Creating-A-Workspace.md#new-directory)创建名为 `ros2_ws` 的新工作空间。软件包应放在 `src` 而非根目录，因此进入 `ros2_ws/src` 并创建软件包：

```console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 cpp_parameters --dependencies rclcpp
```

终端会确认 `cpp_parameters` 及其必需文件和目录已经创建。`--dependencies` 自动在 `package.xml` 和 `CMakeLists.txt` 中添加所需依赖。

<span id="update-package-xml"></span>
#### 1.1 更新 package.xml

使用了 `--dependencies`，就无需手动向 `package.xml` 或 `CMakeLists.txt` 添加依赖。不过仍需填写说明、维护者邮箱和姓名，以及许可证：

```xml
<description>C++ parameter tutorial</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

<span id="write-the-c-node"></span>
### 2 编写 C++ 节点

在 `ros2_ws/src/cpp_parameters/src` 中创建 `cpp_parameters_node.cpp`，粘贴以下代码：

```C++
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

另见 [rclcpp 便捷头文件说明](../../_internal/Rclcpp-Convenience-Header-Note.md)。

<span id="examine-the-code"></span>
#### 2.1 分析代码

开头的 `#include` 对应软件包依赖。

随后定义类及其构造函数。构造函数首先声明名为 `my_parameter` 的参数，默认值为 `world`。参数类型由默认值推断，因此这里是字符串。接下来将 `timer_` 周期设为 1000 ms，使 `timer_callback` 每秒执行一次。

```C++
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

`timer_callback` 首先从节点获取 `my_parameter`，保存到 `my_param`。随后通过 `RCLCPP_INFO` 输出日志。`set_parameters` 再将参数设回默认字符串 `world`，确保即使用户从外部修改参数，也会被恢复为原值。

```C++
void timer_callback()
{
  std::string my_param = this->get_parameter("my_parameter").as_string();

  RCLCPP_INFO(this->get_logger(), "Hello %s!", my_param.c_str());

  std::vector<rclcpp::Parameter> all_new_parameters{rclcpp::Parameter("my_parameter", "world")};
  this->set_parameters(all_new_parameters);
}
```

最后声明 `timer_`：

```C++
private:
  rclcpp::TimerBase::SharedPtr timer_;
```

`MinimalParam` 之后是 `main`：初始化 ROS 2，创建 `MinimalParam` 实例，再通过 `rclcpp::spin` 开始处理节点数据。

```C++
int main(int argc, char ** argv)
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<MinimalParam>());
  rclcpp::shutdown();
  return 0;
}
```

<span id="optional-add-parameterdescriptor"></span>
##### 2.1.1 可选：添加 ParameterDescriptor

可以为参数设置描述符，提供文字说明和约束，例如只读属性、取值范围等。为此，将构造函数修改为：

```C++
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

其余代码保持不变。运行节点后，执行 `ros2 param describe /minimal_param_node my_parameter` 即可查看类型和说明。

<span id="add-executable"></span>
#### 2.2 添加可执行程序

打开 `CMakeLists.txt`，在依赖声明 `find_package(rclcpp REQUIRED)` 下方添加：

```cmake
add_executable(minimal_param_node src/cpp_parameters_node.cpp)
ament_target_dependencies(minimal_param_node rclcpp)

install(TARGETS
    minimal_param_node
  DESTINATION lib/${PROJECT_NAME}
)
```

<span id="build-and-run"></span>
### 3 构建并运行

推荐构建前在工作空间根目录 `ros2_ws` 运行 `rosdep` 检查缺失依赖。

**Linux**

```console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

**macOS 和 Windows**

本教程的 rosdep 步骤仅适用于 Linux，可跳到下一步。

返回 `ros2_ws` 根目录，构建软件包。

**Linux**

```console
$ colcon build --packages-select cpp_parameters
```

**macOS**

```console
$ colcon build --packages-select cpp_parameters
```

**Windows**

```console
$ colcon build --merge-install --packages-select cpp_parameters
```

打开新终端，进入 `ros2_ws` 并加载环境设置文件。

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

运行节点，终端应每秒显示一次 Hello World 消息：

```console
 $ ros2 run cpp_parameters minimal_param_node
[INFO] [minimal_param_node]: Hello world!
```

现在看到的是参数默认值。接下来用两种方式设置它。

<span id="change-via-the-console"></span>
#### 3.1 通过控制台修改

将[参数教程](../Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md)中的知识应用到刚创建的节点。

确认节点正在运行：

```console
$ ros2 run cpp_parameters minimal_param_node
```

打开另一个终端，在 `ros2_ws` 中加载环境，再输入：

```console
$ ros2 param list
```

列表中会显示自定义参数 `my_parameter`。运行以下命令修改它：

```console
$ ros2 param set /minimal_param_node my_parameter earth
```

输出 `Set parameter successful` 表示设置成功。另一个终端中应出现 `[INFO] [minimal_param_node]: Hello earth!`。

<span id="change-via-a-launch-file"></span>
#### 3.2 通过 launch 文件修改

也可以在 launch 文件中设置参数。先在 `ros2_ws/src/cpp_parameters/` 下创建 `launch` 目录，再创建 `cpp_parameters_launch.py`，内容见[原始 launch 示例文件](launch/cpp_parameters_launch.py)。

该文件在启动 `minimal_param_node` 时将 `my_parameter` 设为 `earth`。以下两行确保输出打印在控制台中：

```python
output="screen",
emulate_tty=True,
```

打开 `CMakeLists.txt`，在之前添加的配置下方加入：

```cmake
install(
  DIRECTORY launch
  DESTINATION share/${PROJECT_NAME}
)
```

打开终端，进入工作空间根目录 `ros2_ws`，重新构建软件包。

**Linux**

```console
$ colcon build --packages-select cpp_parameters
```

**macOS**

```console
$ colcon build --packages-select cpp_parameters
```

**Windows**

```console
$ colcon build --merge-install --packages-select cpp_parameters
```

随后在新终端中加载环境设置文件。

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

使用刚创建的 launch 文件运行节点。第一次应输出：

```console
$ ros2 launch cpp_parameters cpp_parameters_launch.py
[INFO] [custom_minimal_param_node]: Hello earth!
```

之后应每秒输出 `[INFO] [minimal_param_node]: Hello world!`。

<span id="summary"></span>
## 小结

你创建了带自定义参数的节点，能够通过 launch 文件或命令行设置参数。将依赖、可执行程序和 launch 文件加入软件包配置后，完成了构建和运行，并观察了参数的作用。

<span id="next-steps"></span>
## 后续步骤

现在你已经有了自己的软件包和 ROS 2 系统。[下一篇教程](Getting-Started-With-Ros2doctor.md)将介绍出现问题时如何检查环境和系统。
