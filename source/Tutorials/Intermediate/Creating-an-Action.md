---
translation_status: machine_translated
source: Tutorials/Intermediate/Creating-an-Action.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="creating-an-action"></span> <span id="actioncreate"></span>

# 创建动作

**目标：** 在 ROS 2 包中定义动作 。

**教程级别：** 中级

**用时：** 5分钟

<span id="background"></span>

## 背景

之前你学到了什么? [理解动作](../Beginner-CLI-Tools/Understanding-ROS2-Actions/Understanding-ROS2-Actions.md) 。与其他通信类型及其相应的接口(主题/msg和服务/srv)一样,您也可以在您的软件包中自定义动作。此教程显示您如何定义和构建您可以在下一个教程中写入的动作服务器和动作客户端的动作。

<span id="prerequisites"></span>

## 前提条件

你应该有 [ROS 2](../../Installation.md) 财务报告和财务报告 [colcon](https://colcon.readthedocs.org) 已安装。

设置一个 [工作空间](../Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.md) 创建名为 `action_tutorials_interfaces`:

(记住: [源代码 ROS 2 安装](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md) (第一编)

##### Linux

``` console
$ mkdir -p ros2_ws/src # you can reuse an existing workspace with this naming convention
$ cd ros2_ws/src
$ ros2 pkg create --build-type ament_cmake action_tutorials_interfaces
```

##### macOS

``` console
$ mkdir -p ros2_ws/src
$ cd ros2_ws/src
$ ros2 pkg create --build-type ament_cmake action_tutorials_interfaces
```

##### Windows

``` console
$ md ros2_ws\src
$ cd ros2_ws\src
$ ros2 pkg create --build-type ament_cmake action_tutorials_interfaces
```

`custom_action_interfaces` 是新软件包的名称。 请注意, 它是一个并且只能是一个 CMake 软件包, 但这并不限制您可以使用哪种软件包 。 `--build-type ament_cmake` 创建新的 ROS 2 软件包时, 旗帜基本上是可选的, 但我们为了完整性而将它列入其中。 您可以在 CMake 软件包中创建自定义接口, 然后在 C++ 或 Python 节点中使用它 。

> **说明**
>
> 保持良好的做法 `.msg`, `.srv`,以及 `.action` 文档中与使用它们的节点分开的软件包中。这使不同软件包的界面定义更容易重新使用。

<span id="tasks"></span>

## 操作步骤

<span id="defining-an-action"></span>

### 1 界定一项行动

行动定义如下: `.action` 窗体文件 :

``` bash
# Request
---
# Result
---
# Feedback
```

动作定义由三个电文定义组成,由下列三个词分隔: `---`.

- A *请求* 消息从动作客户端发送到启动新目标的动作服务器.

- A *结果* 当一个目标完成后,消息从动作服务器发送到动作客户端.

- *反馈* 消息会定期从动作服务器发送到动作客户端,并更新一个目标.

诉讼案件通常称为: *目标*.

说我们要定义一个新的动作“Fibonacci”来计算 [Fibonacci 序列](https://en.wikipedia.org/wiki/Fibonacci_number).

创建一个 `action` 我们ROS 2 软件包中的目录 `action_tutorials_interfaces`:

##### Linux

``` console
$ cd action_tutorials_interfaces
$ mkdir action
```

##### macOS

``` console
$ cd action_tutorials_interfaces
$ mkdir action
```

##### Windows

``` console
$ cd action_tutorials_interfaces
$ md action
```

内部 `action` 目录,创建名为的文件 `Fibonacci.action` 内容如下:

``` bash
int32 order
---
int32[] sequence
---
int32[] partial_sequence
```

目标要求是 `order` Fibonacci序列中,我们想要计算的结果是最终结果 `sequence`,反馈是 `partial_sequence` 计算到目前为止。

<span id="building-an-action"></span>

### 2 采取行动

在使用我们代码中新的Fibonacci动作类型之前,我们必须将定义传递给rosidl代码生成管道.

为此,我们增加了以下几行: `CMakeLists.txt` 开始前 `ament_package()` 线条,在 `action_tutorials_interfaces`:

``` cmake
find_package(rosidl_default_generators REQUIRED)

rosidl_generate_interfaces(${PROJECT_NAME}
  "action/Fibonacci.action"
)
```

我们还应该增加必要的依赖性 `package.xml`:

``` xml
<buildtool_depend>rosidl_default_generators</buildtool_depend>

<depend>action_msgs</depend>

<member_of_group>rosidl_interface_packages</member_of_group>
```

注意,我们需要依赖 `action_msgs` 由于动作定义包括额外的元数据(例如目标ID).

我们现在应该能够制定一揽子计划,其中包括: `Fibonacci` 动作定义 :

``` console
$ cd ~/ros2_ws # Change to the root of the workspace
$ colcon build # Build
```

我们说完了!

根据惯例,动作类型将按其软件包名称和单词前缀 `action`因此,当我们想提及我们的新行动时,它将有全名 `action_tutorials_interfaces/action/Fibonacci`.

我们可以检查一下我们的行动是否成功地用命令行工具构建:

``` console
$ . install/setup.bash  # Source our workspace. On Windows: call install/setup.bat
$ ros2 interface show action_tutorials_interfaces/action/Fibonacci  # Check that our action definition exists
```

您应该看到 Fibonacci 动作定义打印到屏幕上 。

<span id="summary"></span>

## 小结

在此教程中, 您学会了动作定义的结构。 您还学会了如何正确构建新的动作界面 。 `CMakeLists.txt` 财务报告和财务报告 `package.xml`,以及如何验证一个成功的建筑。

<span id="next-steps"></span>

## 后续步骤

接着,让我们通过创建动作服务和客户端来利用您新定义的动作界面(在 [Python](Writing-an-Action-Server-Client/Py.md) 或 时 间 [C++](Writing-an-Action-Server-Client/Cpp.md)).

<span id="related-content"></span>

## 相关内容

欲了解关于ROS行动的更详细资料,请参见: [设计文章](http://design.ros2.org/articles/actions.html).
