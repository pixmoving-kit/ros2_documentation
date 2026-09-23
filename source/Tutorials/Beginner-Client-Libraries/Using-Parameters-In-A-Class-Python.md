<span id="using-parameters-in-a-class-python"></span> <span id="pythonparamnode"></span>
# 在类中使用参数（Python）

**目标：** 使用 Python 创建并运行一个包含 ROS 参数的类。

**教程级别：** 初学者

**预计用时：** 20 分钟

<span id="background"></span>
## 背景

编写自己的[节点](../Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.md)时，有时需要添加能够通过 launch 文件设置的参数。本教程介绍如何在 Python 类中创建这些参数，并在 launch 文件中设置它们。

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
$ ros2 pkg create --build-type ament_python --license Apache-2.0 python_parameters --dependencies rclpy
```

终端会确认 `python_parameters` 及其必需文件和目录已经创建。`--dependencies` 会自动将所需依赖写入 `package.xml`。

<span id="update-package-xml"></span>
#### 1.1 更新 package.xml

使用了 `--dependencies`，就无需手动添加依赖。不过仍需填写说明、维护者邮箱和姓名，以及许可证：

```xml
<description>Python parameter tutorial</description>
<maintainer email="you@email.com">Your Name</maintainer>
<license>Apache License 2.0</license>
```

<span id="write-the-python-node"></span>
### 2 编写 Python 节点

在 `ros2_ws/src/python_parameters/python_parameters` 中创建 `python_parameters_node.py`，粘贴以下代码：

```Python
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
#### 2.1 分析代码

开头的 `import` 语句用于导入软件包依赖。

随后定义类及其构造函数。`self.declare_parameter('my_parameter', 'world')` 声明名为 `my_parameter` 的参数，默认值为 `world`。参数类型由默认值推断，因此这里是字符串。定时器周期设为 1，使 `timer_callback` 每秒执行一次。

```Python
class MinimalParam(rclpy.node.Node):
    def __init__(self):
        super().__init__('minimal_param_node')

        self.declare_parameter('my_parameter', 'world')

        self.timer = self.create_timer(1, self.timer_callback)
```

`timer_callback` 首先从节点获取 `my_parameter`，将字符串值保存到 `my_param`。随后通过 `get_logger` 输出日志。`set_parameters` 再将 `my_parameter` 设回默认字符串 `world`，确保即使用户从外部修改参数，也会被恢复为原值。

```Python
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

`timer_callback` 之后是 `main`：初始化 ROS 2，创建 `MinimalParam` 实例，再通过 `rclpy.spin` 开始处理节点数据。

```Python
def main():
    rclpy.init()
    node = MinimalParam()
    rclpy.spin(node)

if __name__ == '__main__':
    main()
```

<span id="optional-add-parameterdescriptor"></span>
##### 2.1.1 可选：添加 ParameterDescriptor

可以为参数设置描述符，提供文字说明和约束，例如只读属性、取值范围等。为此，将 `__init__` 修改为：

```Python
# ...

class MinimalParam(rclpy.node.Node):
    def __init__(self):
        super().__init__('minimal_param_node')

        from rcl_interfaces.msg import ParameterDescriptor
        my_parameter_descriptor = ParameterDescriptor(description='This parameter is mine!')

        self.declare_parameter('my_parameter', 'world', my_parameter_descriptor)

        self.timer = self.create_timer(1, self.timer_callback)
```

由于导入了 `rcl_interfaces`，还需要在 `package.xml` 中声明依赖，避免后续出现依赖问题：

```xml
# ...
<depend>rclpy</depend>
<depend>rcl_interfaces</depend>
```

其余代码保持不变。运行节点后，执行 `ros2 param describe /minimal_param_node my_parameter` 即可查看类型和说明。

<span id="add-an-entry-point"></span>
#### 2.2 添加入口点

打开 `setup.py`，使 `maintainer`、`maintainer_email`、`description` 和 `license` 与 `package.xml` 一致：

```python
maintainer='YourName',
maintainer_email='you@email.com',
description='Python parameter tutorial',
license='Apache License 2.0',
```

在 `entry_points` 的 `console_scripts` 列表中添加：

```python
entry_points={
    'console_scripts': [
        'minimal_param_node = python_parameters.python_parameters_node:main',
    ],
},
```

保存文件。

<span id="build-and-run"></span>
### 3 构建并运行

推荐构建前在工作空间根目录 `ros2_ws` 运行 `rosdep` 检查缺失依赖。

**Linux**

```console
$ rosdep install -i --from-path src --rosdistro rolling -y
```

**macOS 和 Windows**

本教程的 rosdep 步骤仅适用于 Linux，可跳到下一步。

返回 `ros2_ws` 根目录，构建新软件包。

**Linux**

```console
$ colcon build --packages-select python_parameters
```

**macOS**

```console
$ colcon build --packages-select python_parameters
```

**Windows**

```console
$ colcon build --merge-install --packages-select python_parameters
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

运行节点，终端应每秒显示一次 `Hello world!`：

```console
 $ ros2 run python_parameters minimal_param_node
[INFO] [parameter_node]: Hello world!
```

现在看到的是参数默认值。接下来用两种方式设置它。

<span id="change-via-the-console"></span>
#### 3.1 通过控制台修改

将[参数教程](../Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md)中的知识应用到刚创建的节点。

确认节点正在运行：

```console
$ ros2 run python_parameters minimal_param_node
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

节点随后将参数设回 `world`，所以之后又会显示 `[INFO] [minimal_param_node]: Hello world!`。

<span id="change-via-a-launch-file"></span>
#### 3.2 通过 launch 文件修改

也可以在 launch 文件中设置参数。先在 `ros2_ws/src/python_parameters/` 下创建 `launch` 目录，再在其中创建 `python_parameters_launch.py`，内容见[原始 launch 示例文件](launch/python_parameters_launch.py)。

该文件在启动节点时将 `my_parameter` 设为 `earth`。以下两行确保输出打印在控制台中：

```console
output="screen",
emulate_tty=True,
```

打开 `setup.py`，在文件开头添加 import 语句，再向 `data_files` 添加配置，以包含所有 launch 文件：

```Python
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

打开终端，进入工作空间根目录 `ros2_ws`，重新构建软件包。

**Linux**

```console
$ colcon build --packages-select python_parameters
```

**macOS**

```console
$ colcon build --packages-select python_parameters
```

**Windows**

```console
$ colcon build --merge-install --packages-select python_parameters
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
$ ros2 launch python_parameters python_parameters_launch.py
[INFO] [custom_minimal_param_node]: Hello earth!
```

之后应每秒输出 `[INFO] [minimal_param_node]: Hello world!`。

<span id="summary"></span>
## 小结

你创建了带自定义参数的节点，能够通过 launch 文件或命令行设置参数。将依赖、可执行程序和 launch 文件加入软件包配置后，完成了构建和运行，并观察了参数的作用。

<span id="next-steps"></span>
## 后续步骤

现在你已经有了自己的软件包和 ROS 2 系统。[下一篇教程](Getting-Started-With-Ros2doctor.md)将介绍出现问题时如何检查环境和系统。
