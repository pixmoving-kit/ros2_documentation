<span id="managing-large-projects"></span> <span id="usingros2launchforlargeprojects"></span>

# 管理大型项目

**目标：** 学习使用 ROS 2 启动文件管理大型项目的最佳实践。

**教程级别：** 中级

**预计耗时：** 20 分钟

<span id="background"></span>

## 背景

本教程介绍为大型项目编写启动文件的一些技巧，重点是如何组织文件，尽可能在不同情境中复用。还会展示参数、YAML 文件、重映射、命名空间、默认参数及 RViz 配置等 ROS 2 启动工具的用法。

<span id="prerequisites"></span>

## 前提条件

本教程使用 [turtlesim](../../Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.md) 和 [turtle_tf2_py](../Tf2/Introduction-To-Tf2.md) 包，并假设你已[创建](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md)名为 `launch_tutorial`、构建类型为 `ament_python` 的包。

<span id="introduction"></span>

## 简介

大型机器人应用通常包含多个相互关联的节点，每个节点都可能具有许多参数。海龟仿真器中的多海龟仿真就是很好的例子：系统包含多个海龟节点、世界配置，以及 TF 广播和监听节点。各节点的大量 ROS 参数会影响它们的行为和外观。ROS 2 启动文件可在一个地方启动所有节点并设置相应参数。

本教程将完成 `launch_tutorial` 包中的 `launch_turtlesim_launch` 启动文件，启动两个 turtlesim 仿真、TF 广播器和监听器，加载参数并启动带有配置的 RViz。下面逐一介绍该启动文件及其用到的功能。

> 启动文件可以采用 XML、YAML 或 Python，三者功能等价，可按喜好选择。下面出现 `launch_turtlesim_launch` 等文件名时，请使用所选格式对应的扩展名 `.xml`、`.yaml` 或 `.py`。

<span id="writing-launch-files"></span>

## 编写启动文件

<span id="top-level-organization"></span>

### 1 顶层组织

编写启动文件时，应尽量提高可复用性。可以将相关节点和配置归入独立启动文件，再为具体系统配置编写顶层启动文件。这样在相同机器人之间切换时无须修改启动文件，从真实机器人切换到仿真机器人时也只需少量修改。

先在 `launch_tutorial` 包的 `launch` 目录中创建一个调用其他启动文件的 `launch_turtlesim_launch` 文件。

> 较旧版本的启动系统可能不支持在 `include` 中使用 `let`，需要改用 `arg`。语法相同，`name` 和 `value` 属性不变，例如 `<arg name="target_frame" value="carrot1" />`。

根据所选格式，将完整示例复制到对应文件：

- [launch/launch_turtlesim_launch.xml](launch/launch_turtlesim_launch.xml)
- [launch/launch_turtlesim_launch.yaml](launch/launch_turtlesim_launch.yaml)
- [launch/launch_turtlesim_launch.py](launch/launch_turtlesim_launch.py)

顶层文件包含多个启动文件，每个文件封装系统一部分的节点、参数，以及可能嵌套包含的其他启动文件。具体包括两个 turtlesim 世界、TF 广播器、TF 监听器、mimic、固定坐标系广播器和 RViz 节点。

> 设计建议：顶层启动文件应保持简短，主要包含对应应用子组件的启动文件，以及经常修改的参数。

这种组织方式便于替换系统的某个部分，后面会看到示例。不过，由于性能和使用方式的要求，有些节点或启动文件可能需要单独启动。

> 设计建议：决定应用需要多少个顶层启动文件时，应权衡这些因素。

<span id="parameters"></span>

### 2 参数

<span id="setting-parameters-in-the-launch-file"></span>

#### 2.1 在启动文件中设置参数

先创建 `turtlesim_world_1_launch`，启动第一个海龟仿真。根据所选格式复制相应完整示例：

- [launch/turtlesim_world_1_launch.xml](launch/turtlesim_world_1_launch.xml)
- [launch/turtlesim_world_1_launch.yaml](launch/turtlesim_world_1_launch.yaml)
- [launch/turtlesim_world_1_launch.py](launch/turtlesim_world_1_launch.py)

文件启动 `turtlesim_node`，并定义、传递仿真配置参数。

<span id="loading-parameters-from-yaml-file"></span>

#### 2.2 从 YAML 文件加载参数

第二个启动文件使用不同配置启动另一个海龟仿真。创建 `turtlesim_world_2_launch`，复制对应示例：

- [launch/turtlesim_world_2_launch.xml](launch/turtlesim_world_2_launch.xml)
- [launch/turtlesim_world_2_launch.yaml](launch/turtlesim_world_2_launch.yaml)
- [launch/turtlesim_world_2_launch.py](launch/turtlesim_world_2_launch.py)

它同样启动 `turtlesim_node`，但参数值直接从 YAML 配置文件加载。YAML 便于保存和加载大量变量。这里的 YAML 是节点参数配置文件，并非另一个启动文件。还可以通过 `ros2 param` 将当前参数导出为 YAML，操作方法见[理解参数](../../Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md)教程。

在软件包的 `config` 目录中创建供启动文件加载的 `turtlesim.yaml`：

```YAML
/turtlesim2/sim:
   ros__parameters:
      background_b: 255
      background_g: 86
      background_r: 150
```

有关参数和 YAML 文件的更多用法，同样可参阅[理解参数](../../Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.md)。

<span id="using-wildcards-in-yaml-files"></span>

#### 2.3 在 YAML 文件中使用通配符

有时希望为多个名称或命名空间不同的节点设置相同参数。如果分别建立 YAML 文件并明确写出每个节点名和命名空间，效率很低。可以使用通配符代替文本中的未知字符，将同一组参数应用到多个节点。

创建与第二个文件类似的 `turtlesim_world_3_launch`，在新命名空间 `turtlesim3` 中再启动一个 `turtlesim_node`：

- [XML 示例](launch/turtlesim_world_3_launch.xml)，重点看第 3 行。
- [YAML 示例](launch/turtlesim_world_3_launch.yaml)，重点看第 7 行。
- [Python 示例](launch/turtlesim_world_3_launch.py)，重点看第 12 行。

但直接加载同一个 YAML 文件不会改变第三个仿真的外观，因为参数保存在另一个命名空间下：

```console
/turtlesim3/sim:
   background_b
   background_g
   background_r
```

无须为同类节点再建配置，可以使用通配符 `/**`，将参数应用到所有节点，而不受名称或命名空间差异影响。将 `config/turtlesim.yaml` 修改为：

```YAML
/**:
   ros__parameters:
      background_b: 255
      background_g: 86
      background_r: 150
```

再把 `turtlesim_world_3_launch` 加入主启动文件。使用这个配置后，`turtlesim3/sim` 和 `turtlesim2/sim` 的 `background_b`、`background_g`、`background_r` 都会取指定值。

<span id="namespaces"></span>

### 3 命名空间

在 `turtlesim_world_2_launch` 中已经定义命名空间。不同命名空间允许启动相似节点，而不会发生节点名或话题名冲突：

```Python
namespace='turtlesim2',
```

节点较多时，逐个指定命名空间很繁琐。可用 `PushRosNamespace` 为整个启动描述设置命名空间，所有嵌套节点会自动继承。

> `PushRosNamespace` 必须是动作列表中的第一个动作，后续动作才会应用该命名空间。

先删除 `turtlesim_world_2_launch` 中的 `namespace='turtlesim2'`，再修改顶层文件中的包含语句。

XML：

```xml
<group>
  <push_ros_namespace namespace="turtlesim2" />
  <include file="$(find-pkg-share launch_tutorial)/launch/turtlesim_world_2_launch.xml" />
</group>
```

YAML：

```yaml
- group:
    - push_ros_namespace:
        namespace: "turtlesim2"
    - include:
        file: "$(find-pkg-share launch_tutorial)/launch/turtlesim_world_2_launch.yaml"
```

Python：

```python
from launch.actions import GroupAction
from launch_ros.actions import PushRosNamespace

   ...
   GroupAction(
     actions=[
         PushRosNamespace('turtlesim2'),
         IncludeLaunchDescription(PathJoinSubstitution([launch_dir, 'turtlesim_world_2_launch.py'])),
      ]
   ),
```

这样，`turtlesim_world_2_launch` 中的所有节点都会使用 `turtlesim2` 命名空间。

<span id="reusing-nodes"></span>

### 4 复用节点

创建 `broadcaster_listener_launch`，复制对应的完整示例：

- [launch/broadcaster_listener_launch.xml](launch/broadcaster_listener_launch.xml)
- [launch/broadcaster_listener_launch.yaml](launch/broadcaster_listener_launch.yaml)
- [launch/broadcaster_listener_launch.py](launch/broadcaster_listener_launch.py)

文件声明默认值为 `turtle1` 的 `target_frame` 启动参数。启动时若传入参数，就将该值传给节点；否则使用默认值。

随后，以不同名称和参数启动两次 `turtle_tf2_broadcaster`，从而复用同一节点而不发生冲突。还会启动 `turtle_tf2_listener`，将其 `target_frame` 设置为前面声明并获取的参数值。

<span id="parameter-overrides"></span>

### 5 覆盖参数

顶层文件包含 `broadcaster_listener_launch` 时，还传入了 `target_frame`：参见 [XML 第 5–7 行](launch/launch_turtlesim_launch.xml)、[YAML 第 8–12 行](launch/launch_turtlesim_launch.yaml)或 [Python 第 16–19 行](launch/launch_turtlesim_launch.py)。

这会将目标坐标系改为 `carrot1`。如果希望 `turtle2` 跟随 `turtle1` 而非 `carrot1`，删除传递 `target_frame` 的那一行即可，此时使用默认值 `turtle1`。

<span id="remapping"></span>

### 6 重映射

创建 `mimic_launch`，复制对应的完整示例：

- [launch/mimic_launch.xml](launch/mimic_launch.xml)
- [launch/mimic_launch.yaml](launch/mimic_launch.yaml)
- [launch/mimic_launch.py](launch/mimic_launch.py)

它启动 `mimic` 节点，向一只海龟发送命令，使其跟随另一只。节点原本从 `/input/pose` 接收目标位姿，这里将其重映射为 `/turtle2/pose`；再将 `/output/cmd_vel` 重映射到 `/turtlesim2/turtle1/cmd_vel`。于是第二个仿真世界中的 `turtle1` 就会跟随第一个世界中的 `turtle2`。

<span id="config-files"></span>

### 7 配置文件

创建 `turtlesim_rviz_launch`，复制对应示例：

- [launch/turtlesim_rviz_launch.xml](launch/turtlesim_rviz_launch.xml)
- [launch/turtlesim_rviz_launch.yaml](launch/turtlesim_rviz_launch.yaml)
- [launch/turtlesim_rviz_launch.py](launch/turtlesim_rviz_launch.py)

它使用 `turtle_tf2_py` 中的配置启动 RViz，设置世界坐标系、启用 TF 可视化，并使用俯视视角。

<span id="environment-variables"></span>

### 8 环境变量

创建最后一个启动文件 `fixed_broadcaster_launch`，复制对应示例：

- [launch/fixed_broadcaster_launch.xml](launch/fixed_broadcaster_launch.xml)
- [launch/fixed_broadcaster_launch.yaml](launch/fixed_broadcaster_launch.yaml)
- [launch/fixed_broadcaster_launch.py](launch/fixed_broadcaster_launch.py)

它展示如何在启动文件中引用环境变量。环境变量可以用来定义或推入命名空间，以区分不同计算机或机器人上的节点。

> 如果运行环境未定义 `USER` 环境变量，例如 ROS Docker 环境，可以将相应环境变量引用替换为任意合适的文本。

<span id="running-launch-files"></span>

## 运行启动文件

<span id="update-setup-py"></span>

### 1 更新 setup.py

打开 `setup.py`，添加安装 `launch/` 中启动文件和 `config/` 中配置文件的条目。`data_files` 应如下：

```Python
import os
from glob import glob
from setuptools import setup
...

data_files=[
      ...
      (os.path.join('share', package_name, 'launch'),
         glob('launch/*')),
      (os.path.join('share', package_name, 'config'),
         glob('config/*.yaml')),
      (os.path.join('share', package_name, 'rviz'),
         glob('config/*.rviz')),
   ],
```

<span id="build-and-run"></span>

### 2 构建并运行

构建软件包并启动顶层文件。

XML：

```console
$ ros2 launch launch_tutorial launch_turtlesim_launch.xml
```

YAML：

```console
$ ros2 launch launch_tutorial launch_turtlesim_launch.yaml
```

Python：

```console
$ ros2 launch launch_tutorial launch_turtlesim_launch.py
```

现在可以看到两个 turtlesim 仿真，第一个包含两只海龟，第二个包含一只。在第一个世界中，`turtle2` 出生在左下方，目标是追踪 `carrot1` 坐标系；它相对于 `turtle1` 沿 x 轴偏移 5 米。

第二个世界中的 `turtlesim2/turtle1` 会模仿 `turtle2` 的行为。

要控制 `turtle1`，运行遥控节点：

```console
$ ros2 run turtlesim turtle_teleop_key
```

显示效果类似下图：

![](images/turtlesim_worlds.png)

RViz 也应已启动，显示所有海龟坐标系相对于 `world` 的关系，世界原点位于左下角。

![](images/turtlesim_rviz.png)

<span id="summary"></span>

## 小结

本教程介绍了使用 ROS 2 启动文件管理大型项目的多种技巧与实践。
