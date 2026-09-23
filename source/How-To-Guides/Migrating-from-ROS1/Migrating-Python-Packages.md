---
translation_status: machine_translated
source: How-To-Guides/Migrating-from-ROS1/Migrating-Python-Packages.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="migrating-python-packages-reference"></span>

# Python 软件包迁移参考

此页面是关于如何将 Python 软件包从 ROS 1 迁移到 ROS 2. 如果这是您第一次迁移 Python 软件包的参考文献, 那么遵循 [用于迁移示例 Python 软件包的此指南](Migrating-Python-Package-Example.md) 先说.

<span id="build-tool"></span>

## 构建工具

而不是使用 `catkin_make`, `catkin_make_isolated` 或 时 间 `catkin build` ROS 2 使用命令行工具 [colcon](https://design.ros2.org/articles/build_tool.html) 来构建和安装一组软件包。 [初学者教程](../../Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.md) 开始于 `colcon`.

<span id="build-system"></span>

## 构建系统

对于纯 Python 软件包, ROS 2 使用标准 `setup.py` Python 开发者熟悉的安装机制。

<span id="update-the-files-to-use-setup-py"></span>

### 更新要使用的文件 *setup.py*

如果ROS 1 软件包只使用 CMake 来引用 `setup.py` 文件,除了Python代码(例如没有信件,服务等)外,没有包含任何东西,它应该在ROS 2:中转换成纯Python软件包.

- 更新或添加构建类型 `package.xml` 文件 :

  ``` xml
  <export>
    <build_type>ament_python</build_type>
  </export>
  ```

- 删除 `CMakeLists.txt` 文件

- 更新 `setup.py` 文件为标准 Python 设置脚本

ROS 2只支持Python 3。虽然每个软件包也可以选择支持Python 2,但如果使用其他ROS 2 软件包提供的任何API,则必须使用Python 3来引用可执行文件。

<span id="update-source-code"></span>

## 更新源代码

<span id="node-initialization"></span>

### 节点初始化

在 ROS 1 中:

``` python
rospy.init_node('asdf')

rospy.loginfo('Created node')
```

在ROS 2中:

``` python
rclpy.init(args=sys.argv)
node = rclpy.create_node('asdf')

node.get_logger().info('Created node')
```

<span id="ros-parameters"></span>

### ROS 参数

在 ROS 1 中:

``` python
 port = rospy.get_param('port', '/dev/ttyUSB0')
 assert isinstance(port, str), 'port parameter must be a str'

 baudrate = rospy.get_param('baudrate', 115200)
 assert isinstance(baudrate, int), 'baudrate parameter must be an integer'

rospy.logwarn('port: ' + port)
```

在ROS 2中:

``` python
port = node.declare_parameter('port', '/dev/ttyUSB0').value
assert isinstance(port, str), 'port parameter must be a str'

baudrate = node.declare_parameter('baudrate', 115200).value
assert isinstance(baudrate, int), 'baudrate parameter must be an integer'

node.get_logger().warn('port: ' + port)
```

<span id="creating-a-publisher"></span>

### 创建出版商

在 ROS 1 中:

``` python
pub = rospy.Publisher('chatter', String)
# or
pub = rospy.Publisher('chatter', String, queue_size=10)
```

在ROS 2中:

``` python
pub = node.create_publisher(String, 'chatter', rclpy.qos.QoSProfile())
# or
pub = node.create_publisher(String, 'chatter', 10)
```

<span id="creating-a-subscriber"></span>

### 创建订阅者

在 ROS 1 中:

``` python
sub = rospy.Subscriber('chatter', String, callback)
# or
sub = rospy.Subscriber('chatter', String, callback, queue_size=10)
```

在ROS 2中:

``` python
sub = node.create_subscription(String, 'chatter', callback, rclpy.qos.QoSProfile())
# or
sub = node.create_subscription(String, 'chatter', callback, 10)
```

<span id="creating-a-service"></span>

### 创建服务

在 ROS 1 中:

``` python
srv = rospy.Service('add_two_ints', AddTwoInts, add_two_ints_callback)
```

在ROS 2中:

``` python
srv = node.create_service(AddTwoInts, 'add_two_ints', add_two_ints_callback)
```

<span id="creating-a-service-client"></span>

### 创建服务客户端

在 ROS 1 中:

``` python
rospy.wait_for_service('add_two_ints')
add_two_ints = rospy.ServiceProxy('add_two_ints', AddTwoInts)
resp = add_two_ints(req)
```

在ROS 2中:

``` python
add_two_ints = node.create_client(AddTwoInts, 'add_two_ints')
while not add_two_ints.wait_for_service(timeout_sec=1.0):
    node.get_logger().info('service not available, waiting again...')
resp = add_two_ints.call_async(req)
rclpy.spin_until_future_complete(node, resp)
```

> **警告**
>
> 不使用 `rclpy.spin_until_future_complete` 更多详情请参见 [同步僵局文章](../Sync-Vs-Async.md).
