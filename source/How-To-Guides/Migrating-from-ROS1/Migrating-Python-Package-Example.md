---
translation_status: machine_translated
source: How-To-Guides/Migrating-from-ROS1/Migrating-Python-Package-Example.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="migrating-a-python-package-example"></span>

# Python 软件包迁移示例

本指南显示如何将一个实例 Python 包从 ROS 1 迁移到 ROS 2 。

<span id="prerequisites"></span>

## 前提条件

你需要一个工作 ROS 2 安装,例如 [ROS 滚动](../../Installation.md).

<span id="the-ros-1-code"></span>

## ROS 1 代码

您不会使用 [猫金](https://index.ros.org/p/catkin/) 在此指南中, 您不需要工作 ROS 1 安装。 您将使用 ROS 2 的构建工具 [科尔康](https://colcon.readthedocs.io/) 换句话说。

本节为您提供ROS 1 Python 包的代码。该包叫做 `talker_py`,它有一个节点叫做 `talker_py_node`。为了方便以后运行 Colcon ,这些指令使您可以在 a 内创建软件包 [Colcon 工作空间](https://colcon.readthedocs.io/en/released/user/what-is-a-workspace.html),

首先,在 `~/ros2_talker_py` 成为Colcon工作空间的根.

##### Linux

``` console
$ mkdir -p ~/ros2_talker_py/src
```

##### macOS

``` console
$ mkdir -p ~/ros2_talker_py/src
```

##### Windows

``` console
$ md \ros2_talker_py\src
```

下一步,为ROS 1 软件包创建文件.

##### Linux

``` console
$ cd ~/ros2_talker_py
$ mkdir -p src/talker_py/src/talker_py
$ mkdir -p src/talker_py/scripts
$ touch src/talker_py/package.xml
$ touch src/talker_py/CMakeLists.txt
$ touch src/talker_py/src/talker_py/__init__.py
$ touch src/talker_py/scripts/talker_py_node
$ touch src/talker_py/setup.py
```

##### macOS

``` console
$ cd ~/ros2_talker_py
$ mkdir -p src/talker_py/src/talker_py
$ mkdir -p src/talker_py/scripts
$ touch src/talker_py/package.xml
$ touch src/talker_py/CMakeLists.txt
$ touch src/talker_py/src/talker_py/__init__.py
$ touch src/talker_py/scripts/talker_py_node
$ touch src/talker_py/setup.py
```

##### Windows

``` console
$ cd \ros2_talker_py
$ md src\talker_py\src\talker_py
$ md src\talker_py\scripts
$ type nul > src\talker_py\package.xml
$ type nul > src\talker_py\CMakeLists.txt
$ type nul > src\talker_py\src\talker_py\__init__.py
$ type nul > src\talker_py\scripts/talker_py_node
$ type nul > src\talker_py\setup.py
```

将以下内容放入每个文件 。

`src/talker_py/package.xml`:

``` xml
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

`src/talker_py/CMakeLists.txt`:

``` cmake
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

`src/talker/src/talker_py/__init__.py`:

``` Python
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

`src/talker_py/scripts/talker_py_node`:

``` Python
#!/usr/bin/env python

import talker_py

if __name__ == '__main__':
    talker_py.main()
```

`src/talker_py/setup.py`:

``` Python
from setuptools import setup
from catkin_pkg.python_setup import generate_distutils_setup

setup_args = generate_distutils_setup(
    packages=['talker_py'],
    package_dir={'': 'src'}
)

setup(**setup_args)
```

这是完整的ROS 1 Python包.

<span id="migrate-the-package-xml"></span>

## 迁入 `package.xml`

当将软件包迁移到 ROS 2 时, 首先将构建系统文件迁移到您手中, 这样您就可以通过构建和运行代码来检查您的工作。 总是从迁移开始 。 `package.xml`.

首先,ROS 2 不使用 `catkin`删除 `<buildtool_depend>` 来 来 来

``` default
<!-- delete this -->
<buildtool_depend>catkin</buildtool_depend>
```

下一步, ROS 2 用途 `rclpy` 改为 `rospy`删除对以下的依赖: `rospy`.

``` default
<!-- Delete this -->
<depend>rospy</depend>
```

替换为新依赖 `rclpy`.

``` xml
<depend>rclpy</depend>
```

添加一个 `<export>` 显示 ROS 2 的构建工具的段落 [科尔康](https://colcon.readthedocs.io/) 这是... `ament_python` 包而不是一个 `catkin` 软件包。

``` xml
<export>
  <build_type>ament_python</build_type>
</export>
```

 `package.xml` 完全迁移,现在应该这样:

``` xml
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

## 删除 `CMakeLists.txt`

ROS 2 中的 Python 包不使用 CMake, 所以删除 `CMakeLists.txt`.

<span id="migrate-the-setup-py"></span>

## 迁入 `setup.py`

论点 `setup()` 输入 `setup.py` 无法再自动生成 `catkin_pkg`。您必须手动通过这些参数,这意味着您将遇到一些重复。 `package.xml`.

从删除导入开始 `catkin_pkg`.

``` default
# Delete this
from catkin_pkg.python_setup import generate_distutils_setup
```

将所有参数移动到 `generate_distutils_setup()` 致电: `setup()`,然后添加 `install_requires` 财务报告和财务报告 `zip_safe` 参数。您拨打到 `setup()` 应该是这样的:

``` Python
setup(
    packages=['talker_py'],
    package_dir={'': 'src'},
    install_requires=['setuptools'],
    zip_safe=True,
)
```

删除呼叫到 `generate_distutils_setup()`.

``` default
# Delete this
setup_args = generate_distutils_setup(
    packages=['talker_py'],
    package_dir={'': 'src'}
)
```

呼唤 `setup()` 需要一点 [额外元数据](https://docs.python.org/3.11/distutils/setupscript.html#additional-meta-data) 复制自 `package.xml`:

- 通过软件包名称 `name` 参数

- 通过软件包版本 `version` 参数

- 维护者通过 `maintainer` 财务报告和财务报告 `maintainer_email` 参数

- 说明,通过 `description` 参数

- 许可证通过 `license` 参数

软件包名称会被多次使用。创建一个名为 `package_name` 在呼吁之上 `setup()`.

``` Python
package_name = 'talker_py'
```

将所有剩余信息复制到 `setup()` 输入 `setup.py`。您的决定 `setup()` 应该是这样的:

``` Python
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

ROS 2 软件包必须安装两个数据文件:

- a `package.xml`

- 软件包标记文件

你的包裹已经有一个 `package.xml`。它描述了您的软件包的依赖性。一个软件包标记文件告诉工具,比如 `ros2 run` 在哪里找到你的包裹。

创建一个位于 `package.xml` 调用 `resource`中创建空文件 `resource` 与软件包同名的目录。

##### Linux

``` console
$ mkdir resource
$ touch resource/talker_py
```

##### macOS

``` console
$ mkdir resource
$ touch resource/talker_py
```

##### Windows

``` console
$ md resource
$ type nul > resource\talker_py
```

那个... `setup()` 呼叫进来 `setup.py` 一定要告诉 `setuptools` 如何安装这些文件。添加以下内容 `data_files` 调用参数 `setup()`.

``` Python
data_files=[
    ('share/ament_index/resource_index/packages',
        ['resource/' + package_name]),
    ('share/' + package_name, ['package.xml']),
],
```

 `setup.py` 已经差不多完成了。

<span id="migrate-python-scripts-and-create-setup-cfg"></span>

## 移动 Python 脚本并创建 `setup.cfg`

ROS 2 Python 软件包的使用 `console_scripts` [入境点](https://python-packaging.readthedocs.io/en/latest/command-line-scripts.html#the-console-scripts-entry-point) 将 Python 脚本安装为可执行文件。 [配置文件](https://setuptools.pypa.io/en/latest/userguide/declarative_config.html) `setup.cfg` 告诉 `setuptools` 在软件包特定目录中安装这些可执行文件,使工具像 `ros2 run` 可以找到它们。创建 `setup.cfg` 文档旁边 `package.xml`.

##### Linux

``` console
$ touch setup.cfg
```

##### macOS

``` console
$ touch setup.cfg
```

##### Windows

``` console
$ type nul > touch setup.cfg
```

将以下内容放入其中:

``` ini
[develop]
script_dir=$base/lib/talker_py
[install]
install_scripts=$base/lib/talker_py
```

您需要使用此工具 `console_scripts` 定义要安装的可执行文件的条目点。每个条目都有格式 `executable_name = some.module:function`。第一部分指定要创建的可执行文件的名称。第二部分指定了在可执行文件启动时要运行的函数。此软件包需要创建名为可执行文件的可执行文件 `talker_py_node`,而可执行文件需要调用函数 `main` 输入 `talker_py` 模块。添加以下的切入点规格作为另一个参数 `setup()` 在您的帐号中 `setup.py`.

``` Python
entry_points={
    'console_scripts': [
        'talker_py_node = talker_py:main',
    ],
},
```

那个... `talker_py_node` 文件已不再需要。 删除文件 `talker_py_node` 删除 `scripts/` 目录。

##### Linux

``` console
$ rm scripts/talker_py_node
$ rmdir scripts
```

##### macOS

``` console
$ rm scripts/talker_py_node
$ rmdir scripts
```

##### Windows

``` console
$ del scripts/talker_py_node
$ rd scripts
```

增设: `console_scripts` 是您最后的改变 `setup.py`。 。 您的最后一个 `setup.py` 应该是这样的:

``` Python
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

## 移动 Python 代码于 `src/talker_py/__init__.py`

ROS 2 更改了许多 Python 代码的最佳做法。 开始将代码迁移为- is 。 在工作完成后, 代码会更容易重构 。

<span id="use-rclpy-instead-of-rospy"></span>

### 使用 `rclpy` 改为 `rospy`

ROS 2 套件的使用 [rclpy](https://index.ros.org/p/rclpy) 改为 `rospy`。您必须做两件事才能使用 `rclpy`:

> 1.  导入 `rclpy`
>
> 2.  初始化 `rclpy`

删除导入的语句 `rospy`.

``` Python
# Remove this
import rospy
```

改为进口说明 `rclpy`.

``` Python
import rclpy
```

添加一个呼叫到 `rclpy.init()` 作为第一篇声明 `main()` 函数。

``` Python
def main():
    # Add this line
    rclpy.init()
```

<span id="execute-callbacks-in-the-background"></span>

### 在背景中执行召回

使用 ROS 1 和 ROS 2 [调用](https://en.wikipedia.org/wiki/Callback_(computer_programming))。在ROS 1中,回调总是在背景线程中执行,用户可以自由地用类似调用来屏蔽主线程. `rate.sleep()`在ROS 2中, `rclpy` 用途 [执行器](../../Concepts/Intermediate/About-Executors.md) 以让用户对调用回调的位置有更大的控制。当移植使用诸如阻断调用之类的代码时, `rate.sleep()`,您必须确保这些电话不会干扰执行者。这样做的一个方法是为执行者创建一条专用线程。

首先,添加这两个导入语句.

``` Python
import threading

from rclpy.executors import ExternalShutdownException
```

下,添加名为顶级的函数 `spin_in_background()`。此函数要求默认执行器执行回调,直到某事关闭。

``` Python
def spin_in_background():
    executor = rclpy.get_global_executor()
    try:
        executor.spin()
    except ExternalShutdownException:
        pass
```

添加以下代码 `main()` 函数在调用到 `rclpy.init()` 以启动一个调用线程 `spin_in_background()`.

``` Python
# In rospy callbacks are always called in background threads.
# Spin the executor in another thread for similar behavior in ROS 2.
t = threading.Thread(target=spin_in_background)
t.start()
```

最后,当程序结束时,加入线程,将此语句放在下方 `main()` 函数。

``` Python
t.join()
```

<span id="create-a-node"></span>

### 创建一个节点

在ROS 1 中, Python 脚本只能每个进程创建一个单一的节点, 而 API `init_node()` 创建它。在 ROS 2 中,一个 Python 脚本可以创建多个节点,创建节点的 API 被命名为 `create_node`.

删除调用到 `rospy.init_node()`:

``` default
rospy.init_node('talker')
```

添加新调用到 `rclpy.create_node()` 并存储结果到一个名为 `node`:

``` Python
node = rclpy.create_node('talker')
```

我们必须告诉执行者这个节点。 在节点的创建下面添加以下一行:

``` Python
rclpy.get_global_executor().add_node(node)
```

<span id="create-a-publisher"></span>

### 创建出版社

在ROS 1中,用户通过即时发布创建出版社 `Publisher` 在 ROS 2 中,用户通过节点创建出版商 `create_publisher()` API. 那个 `create_publisher()` API与ROS 1有一个不幸的区别:主题名称和主题类型参数互换.

删除创建 `rospy.Publisher` 举个例子

``` default
pub = rospy.Publisher('chatter', String, queue_size=10)
```

换成呼叫 `node.create_publisher()`.

``` Python
pub = node.create_publisher(String, 'chatter', 10)
```

<span id="create-a-rate"></span>

### 创建比率

在 ROS 1 中,用户创建 `Rate` 实例直接,而ROS 2 中的用户通过节点创建它们 `create_rate()` API. (英语).

删除创建 `rospy.Rate` 举个例子

``` default
rate = rospy.Rate(10)  # 10hz
```

换成呼叫 `node.create_rate()`.

``` Python
rate = node.create_rate(10)  # 10hz
```

<span id="loop-on-rclpy-ok"></span>

### 循环 `rclpy.ok()`

在ROS 1中, `rospy.is_shutdown()` API表示是否要求该程序关闭。 `rclpy.ok()` API这样做。

删除语句 `not rospy.is_shutdown()`

``` default
while not rospy.is_shutdown():
```

换成呼叫 `rclpy.ok()`.

``` Python
while rclpy.ok():
```

<span id="create-a-string-message-with-the-current-time"></span>

### 创建一个 `String` 以当前时间发送信件

您必须在此行中做一些修改

``` default
hello_str = "hello world %s" % rospy.get_time()
```

在ROS2中,你:

- 一定要从一个时间 `Clock` 实例

- 格式化为 `str` 数据使用 [f 字符串](https://docs.python.org/3/reference/lexical_analysis.html#f-strings) 自此以来 [% 在活跃的 Python 版本中被抑制](https://docs.python.org/3/library/stdtypes.html#printf-style-string-formatting)

- 必须即兴演奏 `std_msgs.msg.String` 实例

从时间开始,ROS 2节点有一个 `Clock` 实例。将调用改为 `rospy.get_time()` 与 `node.get_clock().now()` 从节点的时钟获取当前时间。

下,替换使用 `%` 带有 f 字符串 : `f'hello world {node.get_clock().now()}'`.

最后,即兴演奏a `std_msgs.msg.String()` 实例,并指定上列 `data` 属性。您的最终代码应该像这样 :

``` Python
hello_str = String()
hello_str.data = f'hello world {node.get_clock().now()}'
```

<span id="log-an-informational-message"></span>

### 登录信息消息

在 ROS 2 中,您必须通过 `Logger` 举例来说,节点有一个。

删除调用到 `rospy.loginfo()`.

``` default
rospy.loginfo(hello_str)
```

换成呼叫 `info()` 在节点上 `Logger` 举个例子

``` Python
node.get_logger().info(hello_str.data)
```

这是最后一次更改到 `src/talker_py/__init__.py`。您的文件应该看起来像 :

``` Python
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

### 构建和运行 `talker_py_node`

创建三个终端:

1.  一个要建 `talker_py`

2.  跑一个 `talker_py_node`

3.  一个来回呼应 消息由发表 `talker_py_node`

在第一个终端建立工作空间。

##### Linux

``` console
$ cd ~/ros2_talker_py
$ . /opt/ros/rolling/setup.bash
$ colcon build
```

##### macOS

``` console
$ cd ~/ros2_talker_py
$ . /opt/ros/rolling/setup.bash
$ colcon build
```

##### Windows

``` console
$ cd \ros2_talker_py
$ call C:\dev\ros2\local_setup.bat
$ colcon build
```

源代码到第二终端的工作空间,并运行 `talker_py_node`.

##### Linux

``` console
$ cd ~/ros2_talker_py
$ . install/setup.bash
$ ros2 run talker_py talker_py_node
```

##### macOS

``` console
$ cd ~/ros2_talker_py
$ . install/setup.bash
$ ros2 run talker_py talker_py_node
```

##### Windows

``` console
$ cd \ros2_talker_py
$ call install\setup.bat
$ ros2 run talker_py talker_py_node
```

回声第三个终端中节点发布的消息 :

##### Linux

``` console
$ . /opt/ros/rolling/setup.bash
$ ros2 topic echo /chatter
```

##### macOS

``` console
$ . /opt/ros/rolling/setup.bash
$ ros2 topic echo /chatter
```

##### Windows

``` console
$ call C:\dev\ros2\local_setup.bat
$ ros2 topic echo /chatter
```

您应该看到当前时间在第二个终端上发布的信息, 以及第三个终端上收到的同样信息 。

<span id="refactor-code-to-use-ros-2-conventions"></span>

## 重构使用 ROS 2 公约的代码

您已成功将 ROS 1 Python 软件包迁移到 ROS 2 。 既然您有工作, 请考虑重新设计它, 使其更好地与 ROS 2 的 Python API 保持一致。 遵循这两项原则 。

- 创建继承从 `Node`.

- 做回调工作 永远不要阻止回调

例如,创建一个 `Talker` 继承从 `Node`至于回调工作,请使用 `Timer` 以回调代替 `rate.sleep()`。让计时器调回消息并返回。 `main()` 创建一个 `Talker` 实例,而不是使用 `rclpy.create_node()`,并给予执行者要执行的主线程。

你重构的代码可能是这样的:

``` Python
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

## 结论

您已经学会了如何将一个例 Python ROS 1 软件包迁移到 ROS 2 , 从现在开始, 请参考 [移动 Python 套件参考页面](Migrating-Python-Packages.md) 移动自己的 Python 包。
