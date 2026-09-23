<span id="migrating-scripts"></span>
# 迁移脚本

<span id="ros-cli"></span>
## ROS 命令行接口

ROS 1 使用不同的独立命令执行各种操作，例如 `rosrun`、`rosparam` 等。

ROS 2 只有一个顶层命令 `ros2`，所有操作都是它的子命令，例如 `ros2 run`、`ros2 param` 等。

<span id="ros-cli-arguments"></span>
## ROS 命令行参数

ROS 1 直接在命令行上传递节点参数。

ROS 2 参数应放在 `--ros-args` 与结尾的 `--` 之间。如果后面没有其他参数，可以省略结尾的双连字符。

名称重映射与 ROS 1 类似，采用 `from:=to` 形式，但前面必须加上 `--remap` 或 `-r` 标志。例如：

```console
$ ros2 run some_package some_ros_executable --ros-args -r foo:=bar
```

设置参数时使用类似语法，但标志改为 `--param` 或 `-p`：

```console
$ ros2 run some_package some_ros_executable --ros-args -p my_param:=value
```

注意，这与 ROS 1 中使用前导下划线的方式不同。

修改节点名称使用 `__node`，对应 ROS 1 的 `__name`：

```console
$ ros2 run some_package some_ros_executable --ros-args -r __node:=new_node_name
```

注意这里使用了 `-r` 标志。更改命名空间 `__ns` 时，也需要同样的重映射标志：

```console
$ ros2 run some_package some_ros_executable --ros-args -r __ns:=/new/namespace
```

ROS 2 没有与以下 ROS 1 键对应的项：

- `__log`，但可以使用 `--log-config-file` 提供日志记录器配置文件。
- `__ip`
- `__hostname`
- `__master`

更多信息见[设计文档](https://design.ros2.org/articles/ros_command_line_arguments.html)。

<span id="quick-reference"></span>
### 速查表

| 功能 | ROS 1 | ROS 2 |
| --- | --- | --- |
| 重映射 | `foo:=bar` | `-r foo:=bar` |
| 参数 | `_foo:=bar` | `-p foo:=bar` |
| 节点名称 | `__name:=foo` | `-r __node:=foo` |
| 命名空间 | `__ns:=foo` | `-r __ns:=foo` |
