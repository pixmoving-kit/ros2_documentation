---
translation_status: machine_translated
source: Tutorials/Intermediate/Testing/Integration.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="writing-basic-integration-tests-with-launch-testing"></span>

# 使用 launch_testing 编写基础集成测试

**目标：** 在ROS 2龟兹姆节点上创建并运行集成测试.

**教程级别：** 中级

**用时：** 20分钟

<span id="prerequisites"></span>

## 前提条件

在开始这个教程之前,建议完成关于发射节点的下列教程:

- [启动多个节点](../../Beginner-CLI-Tools/Launching-Multiple-Nodes/Launching-Multiple-Nodes.md)

- [创建启动文件](../Launch/Creating-Launch-Files.md)

<span id="background"></span>

## 背景

当单位测试侧重于验证一个非常具体的功能块时,集成测试侧重于验证代码块之间的相互作用。在ROS 2中,这通常通过启动一个或几个节点的系统来实现,例如: [Gazebo 模拟器](https://gazebosim.org/home) 页:1 [Nav2 导航](https://github.com/ros-planning/navigation2.git) 因此,这些测试无论是设置还是运行都更为复杂。

ROS 2 集成测试的一个关键方面是,作为不同测试的一部分的节点,即使平行运行,也不应该相互交流。 这一点将在这里使用一个特定的测试跑车来选择独特的 [ROS 域名标识](../../../Concepts/Intermediate/About-Domain-ID.md)此外,集成测试必须适应整个测试工作流程,一个标准化的方法是确保每个测试输出一个XUnit文件,这些文件使用通用测试工具很容易解析.

<span id="overview"></span>

## 概述

这里使用的主要工具是: [launch_testing](https://docs.ros.org/en/rolling/p/launch_testing/index.html) 软件包( R)[启动_测试仓库](https://github.com/ros2/launch/tree/rolling/launch_testing))),这种ROS-不可知功能可以扩展一个Python发射文件,同时具有主动测试(在节点同时运行时运行)和shutdown后测试(在所有节点退出后运行一次)两种功能. `launch_testing` 依赖于 Python 标准模块 [单位测试](https://docs.python.org/3/library/unittest.html) 为了让我们的整合测试成为其中的一部分 `colcon test`,我们将发射文件登记在 `CMakeLists.txt`.

<span id="steps"></span>

## 步骤

<span id="describe-the-test-in-the-test-launch-file"></span>

### 1 在试验发射文件中描述试验

测试中的节点和测试本身都使用类似ROS 2 Python发射文件的Python发射文件进行发射,习惯的做法是使集成测试发射文件名称遵循模式. `test/test_*.py`.

在集成测试中,有两种常见的测试类型:主动测试,在测试的节点运行时运行,以及休整后测试,这些测试在退出节点后运行。我们会在本教程中同时覆盖两种测试.

<span id="imports"></span>

#### 1.1 进口

我们首先导入我们将要使用的 Python 模块。 只有两个模块是用于测试的:通用 `unittest`,以及 `launch_testing`.

``` python
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

#### 1.2 生成测试说明

职能 `generate_test_description` 描述什么是发射,类似于 `generate_launch_description` 在ROS 2 Python 发射文件中。在下面的例子中,我们发射乌龟节点,半秒后进行测试。

在更复杂的集成测试设置中,您可能想要启动一个由多个节点组成的系统,同时同时推出附加的节点,这些节点进行模拟或者必须与正在测试的节点进行互动.

``` python
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

#### 1.3 主动测试

主动测试与运行中的节点相互作用。 在这个教程中,我们将检查龟头节点是否发布摆姿势信息(通过听节点的“turtle1/pose ” ) , 以及它是否记录它产卵龟(通过听stderr ) 。

主动测试的定义是继承自 [单位测试。](https://docs.python.org/3/library/unittest.html#unittest.TestCase)孩子们的课,在这里 `TestTurtleSim`,包含下列方法:

- `test_*`:测试方法,每个测试方法与测试中的节点进行一些ROS通信,和/或监听过程输出(通过 `proc_output`。它们按顺序执行。

- `setUp`, `tearDown`: 分别运行在( 准备测试固定) 和执行每个测试方法之后。 `setUp` 方法,我们使用不同的节点实例来进行每次测试,以减少测试相互沟通的风险.

- `setUpClass`, `tearDownClass`:这些类方法分别在执行所有测试方法之前和之后运行一次.

我非常建议你走过去 [发射 \_ 测试关于这一主题的详细文件](https://docs.ros.org/en/rolling/p/launch_testing/index.html).

``` python
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

请注意,我们倾听\`turtle1/pose ' 专题的方式在 `test_publishes_pose` 与 [通常办法](../../Beginner-Client-Libraries/Writing-A-Simple-Py-Publisher-And-Subscriber.md)。而不是称屏蔽 `rclpy.spin`,我们触发 `spin_once` 方法 - 它执行第一个可用的回调( 如果信件在 1 秒内到达, 我们的订阅者会回调) - 直到我们收集到过去 10 秒内发布的所有信件。 软件包 [launch_testing_ros](https://docs.ros.org/en/rolling/p/launch_testing_ros/index.html) 提供一些实现类似行为的便利功能,例如 [等待时空](https://docs.ros.org/en/rolling/p/launch_testing_ros/launch_testing_ros.wait_for_topics.html).

如果您想更进一步, 您可以执行第三个测试, 发布一个曲折消息, 要求龟移动, 然后检查它是否移动了, 因为它断言摆放信息已经改变 。 这实际上可以自动化其中的一部分 。 [Turtlsim 介绍教程](../../Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md).

<span id="post-shutdown-tests"></span>

#### 1.4 下沉后试验

标有 `launch_testing.post_shutdown_test` 装饰器在让节点进入测试退出状态后运行。 这里一个典型的测试是节点是否干净地退出, 对于它 `launch_testing` 提供方法 [自动交换码( A)](https://docs.ros.org/en/rolling/p/launch_testing/launch_testing.asserts.html#launch_testing.asserts.assertExitCodes).

``` python
# Post-shutdown tests
@launch_testing.post_shutdown_test()
class TestTurtleSimShutdown(unittest.TestCase):
    def test_exit_codes(self, proc_info):
        """Check if the processes exited normally."""
        launch_testing.asserts.assertExitCodes(proc_info)
```

<span id="register-the-test-in-the-cmakelists-txt"></span>

### 2 在 CMakeLists.txt 中注册测试

将测试登记在 `CMakeLists.txt` 履行两项职能:

- 它将它融入到 `CTest` 框架 ROS 2 基于 CMake 的软件包依赖(因此运行时将调用) `colcon test`).

- 它允许指定 *怎么样* 测试要运行——在这种情况下,有一个独特的域名ID,以确保测试隔离.

后一方面是使用特殊测试跑车实现的 [run_test_isolated.py](https://github.com/ros2/ament_cmake_ros/blob/rolling/ament_cmake_ros/cmake/run_test_isolated.py)。为方便添加几个集成测试,我们定义了 CMake 函数 `add_ros_isolated_launch_test` 因此,每个附加测试只需要一条线。

``` cmake
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

### 3 依赖性和一揽子组织

最后,请在您 `package.xml`:

``` XML
<test_depend>ament_cmake_ros</test_depend>
<test_depend>launch</test_depend>
<test_depend>launch_ros</test_depend>
<test_depend>launch_testing</test_depend>
<test_depend>launch_testing_ament_cmake</test_depend>
<test_depend>rclpy</test_depend>
<test_depend>turtlesim</test_depend>
```

遵循上述步骤后,您的包件(此处命名为 " app " )应看以下内容:

``` default
app/
  CMakeLists.txt
  package.xml
  tests/
      test_integration.py
```

整合测试可以是 ROS 软件包的一部分。 我们可以将一个或多个软件包用于简单的整合测试, 或者把它们添加到测试功能的软件包中。 在这个教程中, 我们先选择第一个选项, 来测试已有的龟兹节点 。

<span id="running-tests-and-report-generation"></span>

### 4 运行测试和报告生成

关于综合测试的运行和结果的检查,请参见教程 [从命令行运行 ROS 2 测试](CLI.md).

<span id="summary"></span>

## 小结

在这个教程中,我们探索了在ROS 2龟兹姆节点上创建和运行集成测试的过程,我们讨论了集成测试发射文件,并覆盖了写作主动测试和下沉后测试. 重述,集成测试发射文件的四个关键要素是:

- 职能 `generate_test_description`我们的节点在测试和测试下发射

- `launch_testing.actions.ReadyToTest()`:这提醒测试框架,测试应该运行,并确保主动测试和节点一起运行.

- 未装饰的类继承自 `unittest.TestCase`:这包含积极的测试,包括设置和拆卸,并允许ROS通过 `proc_output`.

- 继承自 `unittest.TestCase` 装饰 `@launch_testing.post_shutdown_test()`:这些测试是在所有节点关闭后运行的;通常可以断言节点已经干净地退出.

发射试验随后登记在 `CMakeLists.txt` 使用自定义的 CMake 宏 `add_ros_isolated_launch_test` 确保每次发射试验都有一个独特的 `ROS_DOMAIN_ID`,避免不受欢迎的交叉交流。

<span id="related-content"></span>

## 相关内容

- [为什么是自动测试?](Testing-Main.md)

- [使用 GTest 测试的 C++ 单位](Cpp.md) 财务报告和财务报告 [用 Pytest 测试 Python 单元](Python.md)

- [启动\_ pyst 文档](https://docs.ros.org/en/rolling/p/launch_pytest/index.html),替代发射集成测试包 `launch_testing`
