<span id="writing-basic-integration-tests-with-launch-testing"></span>

# 使用 launch_testing 编写基本集成测试

**目标：** 为 ROS 2 turtlesim 节点创建并运行集成测试。

**教程级别：** 中级

**预计耗时：** 20 分钟

<span id="prerequisites"></span>

## 前提条件

开始前，建议先完成以下有关启动节点的教程：

- [启动多个节点](../../Beginner-CLI-Tools/Launching-Multiple-Nodes/Launching-Multiple-Nodes.md)
- [创建启动文件](../Launch/Creating-Launch-Files.md)

<span id="background"></span>

## 背景

单元测试着重验证某个具体功能，集成测试则着重验证不同代码部分之间的交互。在 ROS 2 中，这通常通过启动由一个或多个节点组成的系统来实现，例如 [Gazebo 仿真器](https://gazebosim.org/home)和 [Nav2 导航软件栈](https://github.com/ros-planning/navigation2.git)。因此，这类测试的配置和运行都更复杂。

ROS 2 集成测试的关键要求之一是：即使测试并行运行，不同测试中的节点也不应相互通信。本教程使用专门的测试运行器，为各测试选择不同的 [ROS 域 ID](../../../Concepts/Intermediate/About-Domain-ID.md) 来实现隔离。此外，集成测试必须融入整体测试流程。一种标准做法是让每个测试输出 XUnit 文件，以便常用测试工具解析。

<span id="overview"></span>

## 概述

本教程主要使用 [launch_testing](https://docs.ros.org/en/rolling/p/launch_testing/index.html) 包（[代码仓库](https://github.com/ros2/launch/tree/rolling/launch_testing)）。它本身不依赖 ROS，可为 Python 启动文件添加运行期间测试和关闭后测试：前者在节点仍运行时执行，后者在所有节点退出后执行一次。`launch_testing` 使用 Python 标准库 [unittest](https://docs.python.org/3/library/unittest.html) 实施具体测试。为了让 `colcon test` 运行集成测试，需要在 `CMakeLists.txt` 中注册该启动文件。

<span id="steps"></span>

## 步骤

<span id="describe-the-test-in-the-test-launch-file"></span>

### 1 在测试启动文件中描述测试

被测节点和测试本身都通过 Python 启动文件启动，其形式类似 ROS 2 的 Python 启动文件。集成测试启动文件通常采用 `test/test_*.py` 的命名方式。

集成测试常见的两种类型是运行期间测试和关闭后测试。本教程将介绍这两种类型。

<span id="imports"></span>

#### 1.1 导入模块

首先导入需要的 Python 模块。其中只有两个模块专门用于测试：通用的 `unittest` 和 `launch_testing`。

```python
import os
import sys
import time
import unittest

import launch
import launch_ros
import launch_testing.actions
import rclpy
from turtlesim.msg import Pose
```

<span id="generate-the-test-description"></span>

#### 1.2 生成测试描述

`generate_test_description` 函数描述要启动的内容，类似 ROS 2 Python 启动文件中的 `generate_launch_description`。下面的示例启动 turtlesim 节点，并在半秒后启动测试。

对于更复杂的集成测试，通常需要启动由多个节点组成的系统，以及用于模拟或以其他方式与被测节点交互的辅助节点。

```python
def generate_test_description():
    return (
        launch.LaunchDescription(
            [
                # Nodes under test
                launch_ros.actions.Node(
                    package='turtlesim',
                    namespace='',
                    executable='turtlesim_node',
                    name='turtle1',
                ),
                # Launch tests 0.5 s later
                launch.actions.TimerAction(
                    period=0.5, actions=[launch_testing.actions.ReadyToTest()]),
            ]
        ), {},
    )
```

<span id="active-tests"></span>

#### 1.3 运行期间测试

运行期间测试与正在运行的节点交互。本教程将检查 turtlesim 节点是否发布位姿消息（监听其 `turtle1/pose` 话题），以及是否记录了生成海龟的日志（监听标准错误输出）。

运行期间测试定义为继承 [unittest.TestCase](https://docs.python.org/3/library/unittest.html#unittest.TestCase) 的类中的方法。本例中的子类 `TestTurtleSim` 包含以下方法：

- `test_*`：测试方法，与被测节点进行 ROS 通信，和/或监听通过 `proc_output` 传入的进程输出。各方法按顺序执行。
- `setUp`、`tearDown`：分别在每个测试方法执行前后运行，用于准备和清理测试环境。在 `setUp` 中创建节点，可让每个测试使用独立的节点实例，减少测试之间相互通信的风险。
- `setUpClass`、`tearDownClass`：类方法，分别在所有测试方法执行之前和之后运行一次。

强烈建议阅读 [launch_testing 对这一主题的详细文档](https://docs.ros.org/en/rolling/p/launch_testing/index.html)。

```python
# Active tests
class TestTurtleSim(unittest.TestCase):
    @classmethod
    def setUpClass(cls):
        rclpy.init()

    @classmethod
    def tearDownClass(cls):
        rclpy.shutdown()

    def setUp(self):
        self.node = rclpy.create_node('test_turtlesim')

    def tearDown(self):
        self.node.destroy_node()

    def test_publishes_pose(self, proc_output):
        """Check whether pose messages published"""
        msgs_rx = []
        sub = self.node.create_subscription(
            Pose, 'turtle1/pose',
            lambda msg: msgs_rx.append(msg), 100)
        try:
            # Listen to the pose topic for 10 s
            end_time = time.time() + 10
            while time.time() < end_time:
                # spin to get subscriber callback executed
                rclpy.spin_once(self.node, timeout_sec=1)
            # There should have been 100 messages received
            assert len(msgs_rx) > 100
        finally:
            self.node.destroy_subscription(sub)

    def test_logs_spawning(self, proc_output):
        """Check whether logging properly"""
        proc_output.assertWaitFor(
            'Spawning turtle [turtle1] at x=',
            timeout=5, stream='stderr')
```

注意，`test_publishes_pose` 中监听 `turtle1/pose` 的方式与[通常的做法](../../Beginner-Client-Libraries/Writing-A-Simple-Py-Publisher-And-Subscriber.md)不同。这里没有调用阻塞的 `rclpy.spin`，而是反复调用 `spin_once`，直到收集完 10 秒内发布的消息。`spin_once` 执行第一个可用的回调；如果 1 秒内收到消息，就会执行订阅回调。[launch_testing_ros](https://docs.ros.org/en/rolling/p/launch_testing_ros/index.html) 提供了实现类似行为的便捷功能，例如 [WaitForTopics](https://docs.ros.org/en/rolling/p/launch_testing_ros/launch_testing_ros.wait_for_topics.html)。

如果想进一步练习，可以添加第三个测试：发布 Twist 消息让海龟移动，再通过断言位姿消息发生变化来验证移动。这实际上将 [Turtlesim 入门教程](../../Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md)中的部分操作自动化了。

<span id="post-shutdown-tests"></span>

#### 1.4 关闭后测试

带有 `launch_testing.post_shutdown_test` 装饰器的类会在被测节点退出后运行。典型测试是检查节点是否正常退出，`launch_testing` 为此提供了 [asserts.assertExitCodes](https://docs.ros.org/en/rolling/p/launch_testing/launch_testing.asserts.html#launch_testing.asserts.assertExitCodes) 方法。

```python
# Post-shutdown tests
@launch_testing.post_shutdown_test()
class TestTurtleSimShutdown(unittest.TestCase):
    def test_exit_codes(self, proc_info):
        """Check if the processes exited normally."""
        launch_testing.asserts.assertExitCodes(proc_info)
```

<span id="register-the-test-in-the-cmakelists-txt"></span>

### 2 在 CMakeLists.txt 中注册测试

在 `CMakeLists.txt` 中注册测试有两个作用：

- 将测试集成到 ROS 2 的 CMake 软件包所使用的 `CTest` 框架中，使其在运行 `colcon test` 时被调用。
- 指定测试的运行方式。本例为每个测试分配不同的域 ID，以保证隔离。

后者通过专用测试运行器 [run_test_isolated.py](https://github.com/ros2/ament_cmake_ros/blob/rolling/ament_cmake_ros/cmake/run_test_isolated.py) 实现。为了方便添加多个集成测试，我们定义 CMake 函数 `add_ros_isolated_launch_test`，这样每增加一个测试只需添加一行。

```cmake
cmake_minimum_required(VERSION 3.8)
project(app)

########
# test #
########

if(BUILD_TESTING)
  # Integration tests
  find_package(ament_cmake_ros REQUIRED)
  find_package(launch_testing_ament_cmake REQUIRED)
  function(add_ros_isolated_launch_test path)
    set(RUNNER "${ament_cmake_ros_DIR}/run_test_isolated.py")
    add_launch_test("${path}" RUNNER "${RUNNER}" ${ARGN})
  endfunction()
  add_ros_isolated_launch_test(test/test_integration.py)
endif()
```

<span id="dependencies-and-package-organization"></span>

### 3 依赖项与软件包组织

最后，在 `package.xml` 中添加以下依赖：

```XML
<test_depend>ament_cmake_ros</test_depend>
<test_depend>launch</test_depend>
<test_depend>launch_ros</test_depend>
<test_depend>launch_testing</test_depend>
<test_depend>launch_testing_ament_cmake</test_depend>
<test_depend>rclpy</test_depend>
<test_depend>turtlesim</test_depend>
```

完成上述步骤后，软件包（本例名为 `app`）应具有以下结构：

```
app/
  CMakeLists.txt
  package.xml
  tests/
      test_integration.py
```

集成测试可以放在任何 ROS 软件包中。可以专门创建一个或多个包存放集成测试，也可以将测试加入被测功能所在的包。本教程采用第一种方式，因为我们测试的是现有的 turtlesim 节点。

<span id="running-tests-and-report-generation"></span>

### 4 运行测试并生成报告

运行集成测试和查看结果的方法，请参阅[从命令行运行 ROS 2 测试](CLI.md)。

<span id="summary"></span>

## 小结

本教程介绍了为 ROS 2 turtlesim 节点创建并运行集成测试的过程，包括集成测试启动文件、运行期间测试和关闭后测试。测试启动文件的四个关键组成部分是：

- `generate_test_description` 函数：启动被测节点和测试。
- `launch_testing.actions.ReadyToTest()`：通知测试框架可以运行测试，保证运行期间测试与节点同时运行。
- 继承 `unittest.TestCase` 且不带装饰器的类：包含运行期间测试及其准备、清理逻辑，并通过 `proc_output` 访问 ROS 日志。
- 继承 `unittest.TestCase` 且带有 `@launch_testing.post_shutdown_test()` 装饰器的第二个类：在所有节点关闭后运行测试，通常用于断言节点正常退出。

随后，在 `CMakeLists.txt` 中使用自定义 CMake 宏 `add_ros_isolated_launch_test` 注册启动测试。它确保每个测试使用不同的 `ROS_DOMAIN_ID`，避免意外的跨测试通信。

<span id="related-content"></span>

## 相关内容

- [为什么要自动化测试？](Testing-Main.md)
- [使用 GTest 进行 C++ 单元测试](Cpp.md)及[使用 Pytest 进行 Python 单元测试](Python.md)
- [launch_pytest 文档](https://docs.ros.org/en/rolling/p/launch_pytest/index.html)：另一种可替代 `launch_testing` 的启动集成测试包。
