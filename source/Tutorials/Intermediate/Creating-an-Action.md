<span id="creating-an-action"></span> <span id="actioncreate"></span>

# 创建动作

**目标：** 在 ROS 2 软件包中定义动作。

**教程级别：** 中级

**预计耗时：** 5 分钟

<span id="background"></span>

## 背景

此前的[理解 ROS 2 动作](../Beginner-CLI-Tools/Understanding-ROS2-Actions/Understanding-ROS2-Actions.md)教程介绍了动作。与其他通信类型及其接口（话题/msg、服务/srv）一样，也可以在软件包中自定义动作。本教程介绍如何定义并构建动作，以供下一篇教程编写的动作服务端和客户端使用。

<span id="prerequisites"></span>

## 前提条件

应先安装 [ROS 2](../../Installation.md) 和 [colcon](https://colcon.readthedocs.org)。

建立[工作空间](../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md)，创建名为 `action_tutorials_interfaces` 的包。记得先[加载 ROS 2 环境](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)。

Linux：

```console
$ mkdir -p ros2_ws/src # you can reuse an existing workspace with this naming convention
$ cd ros2_ws/src
$ ros2 pkg create --build-type ament_cmake action_tutorials_interfaces
```

macOS：

```console
$ mkdir -p ros2_ws/src
$ cd ros2_ws/src
$ ros2 pkg create --build-type ament_cmake action_tutorials_interfaces
```

Windows：

```console
$ md ros2_ws\src
$ cd ros2_ws\src
$ ros2 pkg create --build-type ament_cmake action_tutorials_interfaces
```

新建的接口包必须是 CMake 包，但这不限制在哪类包中使用动作。创建 ROS 2 包时，`--build-type ament_cmake` 通常可以省略，这里为完整起见显式写出。可以在 CMake 包中创建自定义接口，再在 C++ 或 Python 节点中使用。

> 建议将 `.msg`、`.srv` 和 `.action` 文件放在独立于使用它们的节点的软件包中，便于在多个包之间复用接口定义。

<span id="tasks"></span>

## 任务

<span id="defining-an-action"></span>

### 1 定义动作

动作在 `.action` 文件中定义，形式为：

```bash
# Request
---
# Result
---
# Feedback
```

一个动作定义由三个消息定义组成，以 `---` 分隔：

- **请求**：由动作客户端发送给动作服务端，发起新目标。
- **结果**：目标完成后，由服务端发送给客户端。
- **反馈**：由服务端定期发送给客户端，报告目标的进展。

动作的一个实例通常称为一个**目标**。

假设要定义名为 `Fibonacci` 的新动作，用于计算[斐波那契数列](https://en.wikipedia.org/wiki/Fibonacci_number)。在 `action_tutorials_interfaces` 包中创建 `action` 目录。

Linux：

```console
$ cd action_tutorials_interfaces
$ mkdir action
```

macOS：

```console
$ cd action_tutorials_interfaces
$ mkdir action
```

Windows：

```console
$ cd action_tutorials_interfaces
$ md action
```

在 `action` 目录中创建 `Fibonacci.action`，内容如下：

```bash
int32 order
---
int32[] sequence
---
int32[] partial_sequence
```

目标请求为要计算的斐波那契数列阶数 `order`，结果为最终的 `sequence`，反馈为当前已计算出的 `partial_sequence`。

<span id="building-an-action"></span>

### 2 构建动作

在代码中使用新动作类型前，必须将定义交给 rosidl 代码生成流程。

在 `action_tutorials_interfaces` 的 `CMakeLists.txt` 中，`ament_package()` 之前添加：

```cmake
find_package(rosidl_default_generators REQUIRED)

rosidl_generate_interfaces(${PROJECT_NAME}
  "action/Fibonacci.action"
)
```

在 `package.xml` 中添加所需依赖：

```xml
<buildtool_depend>rosidl_default_generators</buildtool_depend>

<depend>action_msgs</depend>

<member_of_group>rosidl_interface_packages</member_of_group>
```

由于动作定义包含目标 ID 等额外元数据，因此需要依赖 `action_msgs`。

现在可以构建包含 `Fibonacci` 定义的软件包：

```console
$ cd ~/ros2_ws # Change to the root of the workspace
$ colcon build # Build
```

按惯例，动作类型以前缀“包名/action”限定，因此新动作的完整名称为 `action_tutorials_interfaces/action/Fibonacci`。

通过命令行工具检查构建是否成功：

```console
$ . install/setup.bash  # Source our workspace. On Windows: call install/setup.bat
$ ros2 interface show action_tutorials_interfaces/action/Fibonacci  # Check that our action definition exists
```

屏幕上应显示 Fibonacci 动作定义。

<span id="summary"></span>

## 小结

本教程介绍了动作定义的结构、如何通过 `CMakeLists.txt` 和 `package.xml` 正确构建新动作接口，以及如何验证构建成功。

<span id="next-steps"></span>

## 后续步骤

接下来用新接口创建动作服务端和客户端，可选择 [Python](Writing-an-Action-Server-Client/Py.md) 或 [C++](Writing-an-Action-Server-Client/Cpp.md)。

<span id="related-content"></span>

## 相关内容

ROS 动作的更多细节见[设计文章](http://design.ros2.org/articles/actions.html)。
