<span id="migrating-python-packages-reference"></span>
# Python 软件包迁移参考

本页介绍将 Python 软件包从 ROS 1 迁移到 ROS 2 时的常用修改。如果是首次迁移 Python 软件包，请先完成 [Python 软件包迁移示例](Migrating-Python-Package-Example.md)。

<span id="build-tool"></span>
## 构建工具

ROS 2 使用命令行工具 [colcon](https://design.ros2.org/articles/build_tool.html) 构建和安装一组软件包，替代 ROS 1 中的 `catkin_make`、`catkin_make_isolated` 或 `catkin build`。colcon 入门可参见[初级教程](../../Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.md)。

<span id="build-system"></span>
## 构建系统

对于纯 Python 软件包，ROS 2 使用 Python 开发者熟悉的标准 `setup.py` 安装机制。

<span id="update-the-files-to-use-setup-py"></span>
### 更新文件以使用 setup.py

如果 ROS 1 软件包只用 CMake 调用 `setup.py`，而且除 Python 代码外没有其他内容，例如消息或服务定义，那么应将其转换为 ROS 2 纯 Python 软件包：

1. 在 `package.xml` 中更新或添加构建类型：

```xml
<export>
  <build_type>ament_python</build_type>
</export>
```

2. 删除 `CMakeLists.txt`。
3. 将 `setup.py` 更新为标准的 Python 安装脚本。

ROS 2 仅支持 Python 3。软件包可以自行选择同时支持 Python 2，但只要使用其他 ROS 2 软件包提供的 API，就必须使用 Python 3 执行程序。

<span id="update-source-code"></span>
## 更新源码

<span id="node-initialization"></span>
### 初始化节点

ROS 1：

```python
rospy.init_node('asdf')

rospy.loginfo('Created node')
```

ROS 2：

```python
rclpy.init(args=sys.argv)
node = rclpy.create_node('asdf')

node.get_logger().info('Created node')
```

<span id="ros-parameters"></span>
### ROS 参数

ROS 1：

```python
 port = rospy.get_param('port', '/dev/ttyUSB0')
 assert isinstance(port, str), 'port parameter must be a str'

 baudrate = rospy.get_param('baudrate', 115200)
 assert isinstance(baudrate, int), 'baudrate parameter must be an integer'

rospy.logwarn('port: ' + port)
```

ROS 2：

```python
port = node.declare_parameter('port', '/dev/ttyUSB0').value
assert isinstance(port, str), 'port parameter must be a str'

baudrate = node.declare_parameter('baudrate', 115200).value
assert isinstance(baudrate, int), 'baudrate parameter must be an integer'

node.get_logger().warn('port: ' + port)
```

<span id="creating-a-publisher"></span>
### 创建发布者

ROS 1：

```python
pub = rospy.Publisher('chatter', String)
# or
pub = rospy.Publisher('chatter', String, queue_size=10)
```

ROS 2：

```python
pub = node.create_publisher(String, 'chatter', rclpy.qos.QoSProfile())
# or
pub = node.create_publisher(String, 'chatter', 10)
```

<span id="creating-a-subscriber"></span>
### 创建订阅者

ROS 1：

```python
sub = rospy.Subscriber('chatter', String, callback)
# or
sub = rospy.Subscriber('chatter', String, callback, queue_size=10)
```

ROS 2：

```python
sub = node.create_subscription(String, 'chatter', callback, rclpy.qos.QoSProfile())
# or
sub = node.create_subscription(String, 'chatter', callback, 10)
```

<span id="creating-a-service"></span>
### 创建服务

ROS 1：

```python
srv = rospy.Service('add_two_ints', AddTwoInts, add_two_ints_callback)
```

ROS 2：

```python
srv = node.create_service(AddTwoInts, 'add_two_ints', add_two_ints_callback)
```

<span id="creating-a-service-client"></span>
### 创建服务客户端

ROS 1：

```python
rospy.wait_for_service('add_two_ints')
add_two_ints = rospy.ServiceProxy('add_two_ints', AddTwoInts)
resp = add_two_ints(req)
```

ROS 2：

```python
add_two_ints = node.create_client(AddTwoInts, 'add_two_ints')
while not add_two_ints.wait_for_service(timeout_sec=1.0):
    node.get_logger().info('service not available, waiting again...')
resp = add_two_ints.call_async(req)
rclpy.spin_until_future_complete(node, resp)
```

!!! warning "警告"
    不要在 ROS 2 回调中使用 `rclpy.spin_until_future_complete`。详情见[同步调用死锁说明](../Sync-Vs-Async.md)。
