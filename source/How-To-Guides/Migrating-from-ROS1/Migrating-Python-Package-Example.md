<span id="migrating-a-python-package-example"></span>
# Python 软件包迁移示例

本指南介绍如何将一个 Python 软件包从 ROS 1 迁移到 ROS 2。

<span id="prerequisites"></span>
## 前提条件

需要一个可正常工作的 ROS 2 环境，例如 [ROS Rolling](../../Installation.md)。

<span id="the-ros-1-code"></span>
## ROS 1 代码

本指南不会使用 [catkin](https://index.ros.org/p/catkin/)，因此不需要安装 ROS 1，而是使用 ROS 2 的构建工具 [Colcon](https://colcon.readthedocs.io/)。

本节提供一个 ROS 1 Python 软件包的代码。包名为 `talker_py`，包含一个名为 `talker_py_node` 的节点。为了方便后续运行 Colcon，将在 [Colcon 工作空间](https://colcon.readthedocs.io/en/released/user/what-is-a-workspace.html)内创建该包。

首先，创建 `~/ros2_talker_py` 文件夹，作为工作空间的根目录。

**Linux：**

```console
$ mkdir -p ~/ros2_talker_py/src
```

**macOS：**

```console
$ mkdir -p ~/ros2_talker_py/src
```

**Windows：**

```console
$ md \ros2_talker_py\src
```

接下来创建 ROS 1 软件包的文件。

**Linux：**

```console
$ cd ~/ros2_talker_py
$ mkdir -p src/talker_py/src/talker_py
$ mkdir -p src/talker_py/scripts
$ touch src/talker_py/package.xml
$ touch src/talker_py/CMakeLists.txt
$ touch src/talker_py/src/talker_py/__init__.py
$ touch src/talker_py/scripts/talker_py_node
$ touch src/talker_py/setup.py
```

**macOS：**

```console
$ cd ~/ros2_talker_py
$ mkdir -p src/talker_py/src/talker_py
$ mkdir -p src/talker_py/scripts
$ touch src/talker_py/package.xml
$ touch src/talker_py/CMakeLists.txt
$ touch src/talker_py/src/talker_py/__init__.py
$ touch src/talker_py/scripts/talker_py_node
$ touch src/talker_py/setup.py
```

**Windows：**

```console
$ cd \ros2_talker_py
$ md src\talker_py\src\talker_py
$ md src\talker_py\scripts
$ type nul > src\talker_py\package.xml
$ type nul > src\talker_py\CMakeLists.txt
$ type nul > src\talker_py\src\talker_py\__init__.py
$ type nul > src\talker_py\scripts/talker_py_node
$ type nul > src\talker_py\setup.py
```

将以下内容填入对应文件。

`src/talker_py/package.xml`：

```xml
<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/schema/package_format2.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="2">
    <name>talker_py</name>
    <version>1.0.0</version>
    <description>The talker_py package</description>
    <maintainer email="gerkey@example.com">Brian Gerkey</maintainer>
    <license>BSD</license>

    <buildtool_depend>catkin</buildtool_depend>

    <depend>rospy</depend>
    <depend>std_msgs</depend>
</package>
```

`src/talker_py/CMakeLists.txt`：

```cmake
cmake_minimum_required(VERSION 3.0.2)
project(talker_py)

find_package(catkin REQUIRED)

catkin_python_setup()

catkin_package()

catkin_install_python(PROGRAMS
    scripts/talker_py_node
    DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```

`src/talker/src/talker_py/__init__.py`：

```python
import rospy
from std_msgs.msg import String

def main():
    rospy.init_node('talker')
    pub = rospy.Publisher('chatter', String, queue_size=10)
    rate = rospy.Rate(10)  # 10hz
    while not rospy.is_shutdown():
        hello_str = "hello world %s" % rospy.get_time()
        rospy.loginfo(hello_str)
        pub.publish(hello_str)
        rate.sleep()
```

`src/talker_py/scripts/talker_py_node`：

```python
#!/usr/bin/env python

import talker_py

if __name__ == '__main__':
    talker_py.main()
```

`src/talker_py/setup.py`：

```python
from setuptools import setup
from catkin_pkg.python_setup import generate_distutils_setup

setup_args = generate_distutils_setup(
    packages=['talker_py'],
    package_dir={'': 'src'}
)

setup(**setup_args)
```

以上构成了完整的 ROS 1 Python 软件包。

<span id="migrate-the-package-xml"></span>
## 迁移 package.xml

向 ROS 2 迁移时，应先迁移构建系统文件，便于在修改过程中通过构建和运行代码验证结果。始终从 `package.xml` 开始。

ROS 2 不使用 `catkin`，因此删除对应的 `<buildtool_depend>`：

```
<!-- delete this -->
<buildtool_depend>catkin</buildtool_depend>
```

ROS 2 使用 `rclpy` 替代 `rospy`。删除 `rospy` 依赖：

```
<!-- Delete this -->
<depend>rospy</depend>
```

替换为 `rclpy` 依赖：

```xml
<depend>rclpy</depend>
```

添加 `<export>` 部分，告诉 ROS 2 构建工具 [Colcon](https://colcon.readthedocs.io/) 这是 `ament_python` 包，而不是 `catkin` 包：

```xml
<export>
  <build_type>ament_python</build_type>
</export>
```

`package.xml` 迁移完成，现在应如下所示：

```xml
<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/schema/package_format2.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="2">
    <name>talker_py</name>
    <version>1.0.0</version>
    <description>The talker_py package</description>
    <maintainer email="gerkey@example.com">Brian Gerkey</maintainer>
    <license>BSD</license>

    <depend>rclpy</depend>
    <depend>std_msgs</depend>

    <export>
        <build_type>ament_python</build_type>
    </export>
</package>
```

<span id="delete-the-cmakelists-txt"></span>
## 删除 CMakeLists.txt

ROS 2 的 Python 软件包不使用 CMake，因此删除 `CMakeLists.txt`。

<span id="migrate-the-setup-py"></span>
## 迁移 setup.py

`setup.py` 中 `setup()` 的参数不能再通过 `catkin_pkg` 自动生成，必须手动传入。因此，部分信息会与 `package.xml` 重复。

先删除从 `catkin_pkg` 导入的语句：

```
# Delete this
from catkin_pkg.python_setup import generate_distutils_setup
```

将传给 `generate_distutils_setup()` 的所有参数移到 `setup()` 调用中，再添加 `install_requires` 和 `zip_safe`。调用应如下所示：

```python
setup(
    packages=['talker_py'],
    package_dir={'': 'src'},
    install_requires=['setuptools'],
    zip_safe=True,
)
```

删除 `generate_distutils_setup()` 调用：

```
# Delete this
setup_args = generate_distutils_setup(
    packages=['talker_py'],
    package_dir={'': 'src'}
)
```

`setup()` 还需要从 `package.xml` 复制一些[额外元数据](https://docs.python.org/3.11/distutils/setupscript.html#additional-meta-data)：

- `name`：包名。
- `version`：软件包版本。
- `maintainer` 和 `maintainer_email`：维护者信息。
- `description`：描述。
- `license`：许可证。

包名会使用多次，因此在 `setup()` 调用之前创建 `package_name` 变量：

```python
package_name = 'talker_py'
```

将其余信息复制到 `setup.py` 的 `setup()` 参数中。现在调用应如下所示：

```python
setup(
    name=package_name,
    version='1.0.0',
    install_requires=['setuptools'],
    zip_safe=True,
    packages=['talker_py'],
    package_dir={'': 'src'},
    maintainer='Brian Gerkey',
    maintainer_email='gerkey@example.com',
    description='The talker_py package',
    license='BSD',
)
```

ROS 2 软件包必须安装两个数据文件：`package.xml` 和软件包标记文件。

现有的 `package.xml` 描述软件包的依赖项。标记文件则告诉 `ros2 run` 等工具到哪里找到软件包。

在 `package.xml` 旁创建 `resource` 目录，然后在其中创建一个与软件包同名的空文件。

**Linux：**

```console
$ mkdir resource
$ touch resource/talker_py
```

**macOS：**

```console
$ mkdir resource
$ touch resource/talker_py
```

**Windows：**

```console
$ md resource
$ type nul > resource\talker_py
```

`setup.py` 中的 `setup()` 必须告诉 `setuptools` 如何安装这些文件。添加以下 `data_files` 参数：

```python
data_files=[
    ('share/ament_index/resource_index/packages',
        ['resource/' + package_name]),
    ('share/' + package_name, ['package.xml']),
],
```

此时 `setup.py` 已接近完成。

<span id="migrate-python-scripts-and-create-setup-cfg"></span>
## 迁移 Python 脚本并创建 setup.cfg

ROS 2 Python 软件包通过 `console_scripts` [入口点](https://python-packaging.readthedocs.io/en/latest/command-line-scripts.html#the-console-scripts-entry-point)，将 Python 脚本安装为可执行程序。[配置文件](https://setuptools.pypa.io/en/latest/userguide/declarative_config.html) `setup.cfg` 告诉 `setuptools` 将这些程序安装到软件包专用目录，供 `ros2 run` 等工具查找。

在 `package.xml` 旁创建 `setup.cfg`。

**Linux：**

```console
$ touch setup.cfg
```

**macOS：**

```console
$ touch setup.cfg
```

**Windows：**

```console
$ type nul > touch setup.cfg
```

填入以下内容：

```ini
[develop]
script_dir=$base/lib/talker_py
[install]
install_scripts=$base/lib/talker_py
```

使用 `console_scripts` 入口点定义要安装的可执行程序。每个条目格式为 `executable_name = some.module:function`：前半部分是可执行程序名称，后半部分是启动时调用的函数。

本软件包需要创建 `talker_py_node` 可执行程序，并调用 `talker_py` 模块中的 `main` 函数。将以下入口点定义作为另一个参数，添加到 `setup.py` 的 `setup()` 调用中：

```python
entry_points={
    'console_scripts': [
        'talker_py_node = talker_py:main',
    ],
},
```

不再需要原来的 `talker_py_node` 文件，删除该文件和 `scripts/` 目录。

**Linux：**

```console
$ rm scripts/talker_py_node
$ rmdir scripts
```

**macOS：**

```console
$ rm scripts/talker_py_node
$ rmdir scripts
```

**Windows：**

```console
$ del scripts/talker_py_node
$ rd scripts
```

添加 `console_scripts` 是对 `setup.py` 的最后一处修改。最终文件应如下所示：

```python
from setuptools import setup

package_name = 'talker_py'

setup(
    name=package_name,
    version='1.0.0',
    packages=['talker_py'],
    package_dir={'': 'src'},
    install_requires=['setuptools'],
    zip_safe=True,
    data_files=[
        ('share/ament_index/resource_index/packages',
            ['resource/' + package_name]),
        ('share/' + package_name, ['package.xml']),
    ],
    maintainer='Brian Gerkey',
    maintainer_email='gerkey@example.com',
    description='The talker_py package',
    license='BSD',
    entry_points={
        'console_scripts': [
            'talker_py_node = talker_py:main',
        ],
    },
)
```

<span id="migrate-python-code-in-src-talker-py-init-py"></span>
## 迁移 src/talker_py/__init__.py 中的 Python 代码

ROS 2 改变了许多 Python 代码的推荐写法。先按照现有结构迁移，得到可运行的结果后，再重构会更容易。

<span id="use-rclpy-instead-of-rospy"></span>
### 使用 rclpy 替代 rospy

ROS 2 软件包使用 [rclpy](https://index.ros.org/p/rclpy) 替代 `rospy`。使用 `rclpy` 需要两个步骤：导入它，然后初始化它。

删除导入 `rospy` 的语句：

```python
# Remove this
import rospy
```

改为导入 `rclpy`：

```python
import rclpy
```

在 `main()` 中，将 `rclpy.init()` 作为第一条语句：

```python
def main():
    # Add this line
    rclpy.init()
```

<span id="execute-callbacks-in-the-background"></span>
### 在后台执行回调

ROS 1 和 ROS 2 都使用[回调](https://en.wikipedia.org/wiki/Callback_(computer_programming))。ROS 1 总是在后台线程中执行回调，因此用户可以通过 `rate.sleep()` 等调用阻塞主线程。ROS 2 的 `rclpy` 使用[执行器](../../Concepts/Intermediate/About-Executors.md)，让用户更精确地控制回调的执行位置。

移植使用 `rate.sleep()` 等阻塞调用的代码时，必须确保这些调用不妨碍执行器。可以为执行器创建一个专用线程。

先添加以下两个导入语句：

```python
import threading

from rclpy.executors import ExternalShutdownException
```

再添加顶层函数 `spin_in_background()`，让默认执行器持续执行回调，直到被关闭：

```python
def spin_in_background():
    executor = rclpy.get_global_executor()
    try:
        executor.spin()
    except ExternalShutdownException:
        pass
```

在 `main()` 中的 `rclpy.init()` 之后添加以下代码，启动调用 `spin_in_background()` 的线程：

```python
# In rospy callbacks are always called in background threads.
# Spin the executor in another thread for similar behavior in ROS 2.
t = threading.Thread(target=spin_in_background)
t.start()
```

最后，在 `main()` 底部加入以下语句，在程序结束时等待线程退出：

```python
t.join()
```

<span id="create-a-node"></span>
### 创建节点

ROS 1 的 Python 脚本每个进程只能创建一个节点，使用 `init_node()` API 创建。ROS 2 的单个 Python 脚本可以创建多个节点，创建节点的 API 名为 `create_node`。

删除 `rospy.init_node()` 调用：

```
rospy.init_node('talker')
```

改用 `rclpy.create_node()`，并将结果保存到 `node` 变量：

```python
node = rclpy.create_node('talker')
```

还必须将此节点告知执行器。在创建节点的语句后添加：

```python
rclpy.get_global_executor().add_node(node)
```

<span id="create-a-publisher"></span>
### 创建发布者

ROS 1 通过实例化 `Publisher` 类创建发布者。ROS 2 通过节点的 `create_publisher()` API 创建发布者。需要注意，话题名称和话题类型参数的顺序与 ROS 1 相反。

删除 `rospy.Publisher` 实例的创建语句：

```
pub = rospy.Publisher('chatter', String, queue_size=10)
```

替换为 `node.create_publisher()`：

```python
pub = node.create_publisher(String, 'chatter', 10)
```

<span id="create-a-rate"></span>
### 创建频率对象

ROS 1 直接创建 `Rate` 实例，ROS 2 则通过节点的 `create_rate()` API 创建。

删除 `rospy.Rate` 的创建语句：

```
rate = rospy.Rate(10)  # 10hz
```

替换为 `node.create_rate()`：

```python
rate = node.create_rate(10)  # 10hz
```

<span id="loop-on-rclpy-ok"></span>
### 使用 rclpy.ok() 作为循环条件

ROS 1 使用 `rospy.is_shutdown()` 判断进程是否收到关闭请求。ROS 2 对应使用 `rclpy.ok()` 判断是否继续运行。

删除 `not rospy.is_shutdown()` 条件：

```
while not rospy.is_shutdown():
```

替换为 `rclpy.ok()`：

```python
while rclpy.ok():
```

<span id="create-a-string-message-with-the-current-time"></span>
### 创建包含当前时间的 String 消息

以下语句需要几处修改：

```
hello_str = "hello world %s" % rospy.get_time()
```

在 ROS 2 中：

- 必须从 `Clock` 实例获取时间。
- 应使用 [f-string](https://docs.python.org/3/reference/lexical_analysis.html#f-strings) 格式化字符串，因为[当前 Python 版本不推荐使用百分号格式化](https://docs.python.org/3/library/stdtypes.html#printf-style-string-formatting)。
- 必须创建 `std_msgs.msg.String` 实例。

先处理时间。ROS 2 节点具有 `Clock` 实例，将 `rospy.get_time()` 替换为 `node.get_clock().now()`，从节点时钟获取当前时间。

然后将百分号格式化替换为 f-string：`f'hello world {node.get_clock().now()}'`。

最后创建 `std_msgs.msg.String()` 实例，并将上述字符串赋给它的 `data` 属性：

```python
hello_str = String()
hello_str.data = f'hello world {node.get_clock().now()}'
```

<span id="log-an-informational-message"></span>
### 输出信息级别日志

ROS 2 必须通过 `Logger` 实例发送日志消息，节点自身就带有这样的实例。

删除 `rospy.loginfo()` 调用：

```
rospy.loginfo(hello_str)
```

替换为节点 `Logger` 的 `info()` 调用：

```python
node.get_logger().info(hello_str.data)
```

至此，`src/talker_py/__init__.py` 修改完成。完整文件应如下所示：

```python
import threading

import rclpy
from rclpy.executors import ExternalShutdownException
from std_msgs.msg import String


def spin_in_background():
    executor = rclpy.get_global_executor()
    try:
        executor.spin()
    except ExternalShutdownException:
        pass


def main():
    rclpy.init()
    # In rospy callbacks are always called in background threads.
    # Spin the executor in another thread for similar behavior in ROS 2.
    t = threading.Thread(target=spin_in_background)
    t.start()

    node = rclpy.create_node('talker')
    rclpy.get_global_executor().add_node(node)
    pub = node.create_publisher(String, 'chatter', 10)
    rate = node.create_rate(10)  # 10hz

    while rclpy.ok():
        hello_str = String()
        hello_str.data = f'hello world {node.get_clock().now()}'
        node.get_logger().info(hello_str.data)
        pub.publish(hello_str)
        rate.sleep()

    t.join()
```

<span id="build-and-run-talker-py-node"></span>
### 构建并运行 talker_py_node

打开三个终端，分别用于构建 `talker_py`、运行 `talker_py_node`，以及显示该节点发布的消息。

在第一个终端构建工作空间。

**Linux：**

```console
$ cd ~/ros2_talker_py
$ . /opt/ros/rolling/setup.bash
$ colcon build
```

**macOS：**

```console
$ cd ~/ros2_talker_py
$ . /opt/ros/rolling/setup.bash
$ colcon build
```

**Windows：**

```console
$ cd \ros2_talker_py
$ call C:\dev\ros2\local_setup.bat
$ colcon build
```

在第二个终端加载工作空间环境，并运行 `talker_py_node`。

**Linux：**

```console
$ cd ~/ros2_talker_py
$ . install/setup.bash
$ ros2 run talker_py talker_py_node
```

**macOS：**

```console
$ cd ~/ros2_talker_py
$ . install/setup.bash
$ ros2 run talker_py talker_py_node
```

**Windows：**

```console
$ cd \ros2_talker_py
$ call install\setup.bat
$ ros2 run talker_py talker_py_node
```

在第三个终端显示节点发布的消息。

**Linux：**

```console
$ . /opt/ros/rolling/setup.bash
$ ros2 topic echo /chatter
```

**macOS：**

```console
$ . /opt/ros/rolling/setup.bash
$ ros2 topic echo /chatter
```

**Windows：**

```console
$ call C:\dev\ros2\local_setup.bat
$ ros2 topic echo /chatter
```

第二个终端应显示正在发布的、包含当前时间的消息，第三个终端应显示接收到的相同消息。

<span id="refactor-code-to-use-ros-2-conventions"></span>
## 按 ROS 2 惯例重构代码

现在已经成功将 ROS 1 Python 软件包迁移到 ROS 2。得到可运行的结果后，可以考虑重构，使其更符合 ROS 2 Python API 的使用习惯。遵循以下原则：

- 创建继承自 `Node` 的类。
- 在回调中完成所有工作，并且不要阻塞回调。

例如，创建继承自 `Node` 的 `Talker` 类。使用带回调的 `Timer` 替代 `rate.sleep()`，让定时器回调发布消息后立即返回。将 `main()` 改为创建 `Talker` 实例，而不是调用 `rclpy.create_node()`，并让执行器在主线程中运行。

重构后的代码可以如下所示：

```python
import rclpy
from rclpy.node import Node
from rclpy.executors import ExternalShutdownException
from std_msgs.msg import String


class Talker(Node):

    def __init__(self, **kwargs):
        super().__init__('talker', **kwargs)

        self._pub = self.create_publisher(String, 'chatter', 10)
        self._timer = self.create_timer(1 / 10, self.do_publish)

    def do_publish(self):
        hello_str = String()
        hello_str.data = f'hello world {self.get_clock().now()}'
        self.get_logger().info(hello_str.data)
        self._pub.publish(hello_str)


def main():
    rclpy.init()
    try:
        rclpy.spin(Talker())
    except (ExternalShutdownException, KeyboardInterrupt):
        pass
    finally:
        rclpy.try_shutdown()
```

<span id="conclusion"></span>
## 小结

你已了解如何将一个 ROS 1 Python 软件包迁移到 ROS 2。迁移自己的软件包时，可以查阅 [Python 软件包迁移参考](Migrating-Python-Packages.md)。
