---
translation_status: machine_translated
source: Tutorials/Beginner-Client-Libraries/Using-Parameters-In-A-Class-Python.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-parameters-in-a-class-python"></span> <span id="pythonparamnode"></span>

# 在类中使用参数（Python）

**目标：** 使用 Python 创建并运行一个带有 ROS 参数的类.

**教程级别：** 入门

**用时：** 20分钟

<span id="background"></span>

## 背景

当自己做的时候 [节点](../Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md) 您有时需要添加可以从发射文件中设定的参数。

此教程将演示如何在 Python 类中创建这些参数,以及如何在发射文件中设置这些参数.

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
$ ros2 pkg create --build-type ament_python --license Apache-2.0 python_parameters --dependencies rclpy
```

您的终端将返回一个消息, 以验证您的软件包的创建 `python_parameters` 以及所有必要的文件和文件夹。

那个... `--dependencies` 参数将自动添加必要的依赖线到 `package.xml`.

<span id="update-package-xml"></span>

#### 1.1 最新情况 `package.xml`

因为你用了 `--dependencies` 在创建软件包时,您不需要手动添加依赖性到 `package.xml`.

但是,与往常一样,确保添加描述、维护者电子邮件和姓名,并给信息发放许可证。 `package.xml`.

``` xml
<description>Python parameter tutorial</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

<span id="write-the-python-node"></span>

### 2 写入 Python 节点

内侧 `ros2_ws/src/python_parameters/python_parameters` 目录,创建名为新文件 `python_parameters_node.py` 并粘贴下列编码:

``` Python
import rclpy
import rclpy.node

class MinimalParam(rclpy.node.Node):
    def __init__(self):
        super().__init__('minimal_param_node')

        self.declare_parameter('my_parameter', 'world')

        self.timer = self.create_timer(1, self.timer_callback)

    def timer_callback(self):
        my_param = self.get_parameter('my_parameter').get_parameter_value().string_value

        self.get_logger().info('Hello %s!' % my_param)

        my_new_param = rclpy.parameter.Parameter(
            'my_parameter',
            rclpy.Parameter.Type.STRING,
            'world'
        )
        all_new_parameters = [my_new_param]
        self.set_parameters(all_new_parameters)

def main():
    rclpy.init()
    node = MinimalParam()
    rclpy.spin(node)

if __name__ == '__main__':
    main()
```

<span id="examine-the-code"></span>

#### 2.1 审查守则

那个... `import` 顶部的语句用于导入软件包的依赖性。

下一块代码创建类和构造器。线条 `self.declare_parameter('my_parameter', 'world')` 创建带有名称的参数 `my_parameter` 和默认值 `world`。从默认值中推断出参数类型,因此在此情况下,参数类型将被设定为字符串类型。 `timer` 初始化的时期为1, 导致 `timer_callback` 函数将每秒执行一次。

``` Python
class MinimalParam(rclpy.node.Node):
    def __init__(self):
        super().__init__('minimal_param_node')

        self.declare_parameter('my_parameter', 'world')

        self.timer = self.create_timer(1, self.timer_callback)
```

我们的第一线 `timer_callback` 函数获得参数 `my_parameter` 从节点,并储存在 `my_param`下一个 `get_logger` 函数确保该事件被记录。 `set_parameters` 函数然后设置参数 `my_parameter` 返回默认字符串值 `world`。如果用户外部更改了参数,这将保证它总是被重置为原参数。

``` Python
def timer_callback(self):
    my_param = self.get_parameter('my_parameter').get_parameter_value().string_value

    self.get_logger().info('Hello %s!' % my_param)

    my_new_param = rclpy.parameter.Parameter(
        'my_parameter',
        rclpy.Parameter.Type.STRING,
        'world'
    )
    all_new_parameters = [my_new_param]
    self.set_parameters(all_new_parameters)
```

紧接着 `timer_callback` 是我们的 `main`。这里,ROS 2是初始化的。 `MinimalParam` 类别是构建的,以及 `rclpy.spin` 开始从节点处理数据。

``` Python
def main():
    rclpy.init()
    node = MinimalParam()
    rclpy.spin(node)

if __name__ == '__main__':
    main()
```

<span id="optional-add-parameterdescriptor"></span>

##### 2.1.1(备选) 添加参数描述符

可以选择设置参数的描述符。描述符允许您指定参数及其约束的文本描述,如使其只读,指定范围等。要工作,请使用 `__init__` 代码必须更改为:

``` Python
# ...

class MinimalParam(rclpy.node.Node):
    def __init__(self):
        super().__init__('minimal_param_node')

        from rcl_interfaces.msg import ParameterDescriptor
        my_parameter_descriptor = ParameterDescriptor(description='This parameter is mine!')

        self.declare_parameter('my_parameter', 'world', my_parameter_descriptor)

        self.timer = self.create_timer(1, self.timer_callback)
```

既然我们进口了 `rcl_interfaces`,我们需要添加依赖性到 `package.xml` 今后避免任何依赖性问题:

``` xml
# ...
<depend>rclpy</depend>
<depend>rcl_interfaces</depend>
```

其余代码保持不变。一旦运行了节点,您就可以运行 `ros2 param describe /minimal_param_node my_parameter` 以查看类型和描述。

<span id="add-an-entry-point"></span>

#### 2.2 增加一个切入点

打开 `setup.py` 文档。再次,匹配 `maintainer`, `maintainer_email`, `description` 财务报告和财务报告 `license` 字段为您 `package.xml`:

``` python
maintainer='YourName',
maintainer_email='you@email.com',
description='Python parameter tutorial',
license='Apache License 2.0',
```

在下行中添加以下行 `console_scripts` 括号 `entry_points` 字段 :

``` python
entry_points={
    'console_scripts': [
        'minimal_param_node = python_parameters.python_parameters_node:main',
    ],
},
```

不要忘记拯救。

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
$ colcon build --packages-select python_parameters
```

##### macOS

``` console
$ colcon build --packages-select python_parameters
```

##### Windows

``` console
$ colcon build --merge-install --packages-select python_parameters
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

现在运行节点,终端应该返回 `Hello world!` 每秒钟:

``` console
 $ ros2 run python_parameters minimal_param_node
[INFO] [parameter_node]: Hello world!
```

现在您可以看到您参数的默认值, 但是您想要自己设置它。 有两种方法可以实现 。

<span id="change-via-the-console"></span>

#### 3.1 通过控制台进行更改

这部分将利用你从 [关于参数的教程](../Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md) 并将其应用到您刚刚创建的节点上。

确保节点运行中 :

``` console
$ ros2 run python_parameters minimal_param_node
```

打开另一个终端, 从内部源出设置文件 `ros2_ws` ,并输入以下行:

``` console
$ ros2 param list
```

您将在此看到自定义参数 `my_parameter`。为了改变它,只需在控制台上运行以下一行:

``` console
$ ros2 param set /minimal_param_node my_parameter earth
```

你知道,如果你得到输出,它很顺利 `Set parameter successful`。如果查看另一个终端,则应当看到输出更改为 `[INFO] [minimal_param_node]: Hello earth!`

由于节点之后将参数放回 `world`,进一步产出显示 `[INFO] [minimal_param_node]: Hello world!`

<span id="change-via-a-launch-file"></span>

#### 3.2 通过发射文件更改

也可以在发射文件中设置参数,但首先需要添加发射目录。 `ros2_ws/src/python_parameters/` 目录,创建新的目录,名为 `launch`中,创建名为“新文件”的文件 `python_parameters_launch.py`

``` python
from launch import LaunchDescription
from launch_ros.actions import Node


def generate_launch_description():
    return LaunchDescription([
        Node(
            package='python_parameters',
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

在这里,你可以看到,我们设置 `my_parameter` 改为: `earth` 当我们发射节点时 `parameter_node`。通过在下面增加两行,我们保证我们的输出在我们的控制台上打印。

``` console
output="screen",
emulate_tty=True,
```

现在打开 `setup.py` 文件。添加 `import` 对文件顶端的语句,对文件的其他新语句 `data_files` 包含所有发射文件的参数 :

``` Python
import os
from glob import glob
# ...

setup(
  # ...
  data_files=[
      # ...
      (os.path.join('share', package_name, 'launch'), glob('launch/*')),
    ]
  )
```

打开一个控制台 导航到您工作空间的根, `ros2_ws`,并构建您的新软件包:

##### Linux

``` console
$ colcon build --packages-select python_parameters
```

##### macOS

``` console
$ colcon build --packages-select python_parameters
```

##### Windows

``` console
$ colcon build --merge-install --packages-select python_parameters
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
$ ros2 launch python_parameters python_parameters_launch.py
[INFO] [custom_minimal_param_node]: Hello earth!
```

其他产出应显示 `[INFO] [minimal_param_node]: Hello world!` 每一秒钟。

<span id="summary"></span>

## 小结

您创建了一个自定义参数的节点, 可以从发射文件或命令行中设置。 您在软件包配置文件中添加了依赖性、 可执行文件以及启动文件, 以便构建和运行它们, 并在操作中看到参数 。

<span id="next-steps"></span>

## 后续步骤

现在,你有一些包 和ROS 2系统你自己的, [下一个教程](Getting-Started-With-Ros2doctor.md) 将教你如何检查环境和系统中的问题,以防出现问题。
