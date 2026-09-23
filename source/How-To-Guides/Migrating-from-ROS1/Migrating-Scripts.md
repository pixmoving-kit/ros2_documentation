---
translation_status: machine_translated
source: How-To-Guides/Migrating-from-ROS1/Migrating-Scripts.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="migrating-scripts"></span>

# 迁移脚本

<span id="ros-cli"></span>

## 罗斯CLI

在ROS 1中,有执行各种行动的个别命令,例如: `rosrun`, `rosparam`, 等 (简体中文).

在ROS 2中,有一个单顶级命令叫做 `ros2`,所有动作都是这样的子命令,比如: `ros2 run`, `ros2 param`, 等 (简体中文).

<span id="ros-cli-arguments"></span>

## ROS CLI 参数

在ROS 1中,直接在命令线上提供了节点的参数.

ROS 2 论点的范围应当包括: `--ros-args` 和一条线索, `--` (如果没有论据,后面的双破折线可能会被打滑).

重新绘制名称与ROS 1相似,采用形式 `from:=to`,但必须先于 `--remap` (或 减) `-r`) 旗帜。例如:

``` console
$ ros2 run some_package some_ros_executable --ros-args -r foo:=bar
```

我们对参数使用类似的语法,使用 `--param` (或 减) `-p`) 旗帜:

``` console
$ ros2 run some_package some_ros_executable --ros-args -p my_param:=value
```

注意,这与ROS 1中使用领先的下划线不同.

更改节点名称使用 `__node` (ROS 1等值为: `__name`):

``` console
$ ros2 run some_package some_ros_executable --ros-args -r __node:=new_node_name
```

注意使用 `-r` 标记。更改命名空间需要同样的重新映射标记 `__ns`:

``` console
$ ros2 run some_package some_ros_executable --ros-args -r __ns:=/new/namespace
```

以下 ROS 1 键在 ROS 2 中没有对应值 :

- `__log` (不过) `--log-config-file` 可用于提供日志配置文件)

- `__ip`

- `__hostname`

- `__master`

更多信息,请参见: [设计文件](https://design.ros2.org/articles/ros_command_line_arguments.html).

<span id="quick-reference"></span>

### 快速引用

| 特性     | ROS 1         | ROS 2              |
|----------|---------------|--------------------|
| 重新绘图 | foo:=栏       | - r foo:= 栏       |
| 参数     | \_foo:=bar    | -p foo:= 栏        |
| 节点名称 | \_\_name:=foo | -r 节点:=foo       |
| 命名空间 | \_\_ns:=foo   | - r ns:=foo (帮助) |
