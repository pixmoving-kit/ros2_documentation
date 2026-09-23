<span id="defining-worlds-robots-and-sensors"></span>
<span id="background"></span>
<span id="prerequisites"></span>
<span id="tasks"></span>
<span id="minimal-world-file"></span>
<span id="using-predefined-vehicles-and-sensors"></span>
<span id="world-environment-elements"></span>
<span id="vehicle-dynamics-models"></span>
<span id="sensor-noise-and-configuration"></span>
<span id="additional-features"></span>
<span id="comparison-with-other-simulators"></span>
<span id="further-resources"></span>

# 定义世界、机器人和传感器

**目标：** 学习 MVSim 世界文件的基本定义方法、如何添加车辆和传感器，以及主要可用功能。

**教程级别：** 高级

**耗时：** 30 分钟

## 背景

MVSim 世界由 XML 文件（`.world.xml`）定义。世界文件描述环境（地面、墙壁、障碍物）、车辆（动力学模型、形状、传感器），以及仿真参数（物理时间步长、图形界面选项）。

MVSim 提供预定义的车辆和传感器库，可通过 XML 包含机制在世界中复用。你也可以从零定义所有内容，以实现完全控制。

## 前提条件

应已安装 MVSim，并完成 [MVSim 入门](Getting-Started-MVSim.md)教程。

## 任务

### 1 最小世界文件

以下最小世界文件创建一个包含一台机器人的空环境：

```xml
<mvsim_world version="1.0">
  <!-- Simulation settings -->
  <simul_timestep>5e-3</simul_timestep>

  <!-- GUI options -->
  <gui>
      <cam_distance>15</cam_distance>
  </gui>

  <!-- Ground plane -->
  <element class="ground_grid">
  </element>

  <!-- A differential-drive robot -->
  <vehicle name="robot1">
    <init_pose>0 0 0</init_pose>  <!-- x y yaw(deg) -->

    <!--  Dynamical model -->
    <dynamics class="differential">
        <!-- Params -->
        <l_wheel pos="0.0  0.5" mass="4.0" width="0.20" diameter="0.40" />
        <r_wheel pos="0.0 -0.5" mass="4.0" width="0.20" diameter="0.40" />

        <!-- Visual and physical shape -->
        <chassis mass="15.0" zmin="0.05" zmax="0.6">
        </chassis>

        <!--   Motor controller -->
        <controller class="twist_pid">
            <!-- Params -->
            <KP>4.1543</KP>
            <KI>1.9118</KI>
            <KD>0.0000</KD>
            <max_torque>14.44</max_torque>
            <V>0.0</V><W>0</W>
        </controller>
    </dynamics>

    <!-- Motor controller: accept twist commands, PID controller -->
    <controller class="twist_pid">
      <KP>100</KP> <KI>5</KI> <max_torque>50</max_torque>
    </controller>
  </vehicle>
</mvsim_world>
```

将其保存为 `my_world.world.xml` 并启动：

```console
$ mvsim launch my_world.world.xml
```

### 2 使用预定义车辆和传感器

无需从零开始定义车辆，可以使用 MVSim 自带的预定义文件。这些 XML 文件位于 MVSim 软件包的 `definitions/` 目录中。

**可用车辆：**

- `turtlebot3_burger.vehicle.xml`：TurtleBot3 Burger（差速驱动）。
- `jackal.vehicle.xml`：Clearpath Jackal 无人地面车辆（四轮差速驱动）。
- `ackermann.vehicle.xml`：通用阿克曼转向车辆（类似汽车）。
- `pickup.vehicle.xml`：皮卡（阿克曼转向）。
- `agricobiot2.vehicle.xml`：农业机器人（阿克曼传动系统）。

**可用传感器：**

- `lidar2d.sensor.xml`：通用二维激光扫描仪。
- `rplidar-a2.sensor.xml`：RPLidar A2。
- `velodyne-vlp16.sensor.xml`：Velodyne VLP-16 三维激光雷达。
- `ouster-os1.sensor.xml`：Ouster OS1 三维激光雷达。
- `helios-32-FOV-70.sensor.xml`：Helios 32 线三维激光雷达。
- `camera.sensor.xml`：RGB 摄像头。
- `rgbd_camera.sensor.xml`：深度摄像头（RGBD）。
- `imu.sensor.xml`：惯性测量单元。
- `gnss.sensor.xml`：GPS/GNSS 接收器。

通过 XML 包含机制使用带传感器的预定义车辆：

```xml
<mvsim_world version="1.0">
  <simul_timestep>5e-3</simul_timestep>

  <gui>
      <cam_distance>15</cam_distance>
  </gui>

  <element class="ground_grid">
  </element>

  <!-- Include the Jackal vehicle definition -->
  <include file="$(ros2 pkg prefix mvsim)/share/mvsim/definitions/jackal.vehicle.xml"
    default_sensors="true"
    />

  <vehicle name="r1" class="jackal">
    <init_pose>0 0 170</init_pose>  <!-- In global coords: x,y, yaw(deg) -->
    <init_vel>0 0 0</init_vel>  <!-- In local coords: vx,vy, omega(deg/s) -->

    <!-- You can also attach sensors to vehicles in separate includes,
        or define them inline within the vehicle block. -->
    <!-- <include file="$(ros2 pkg prefix mvsim)/share/mvsim/definitions/lidar2d.sensor.xml" sensor_x="1.7" sensor_z="1.01" sensor_yaw="0" max_range="70.0" sensor_name="laser1" />  -->
  </vehicle>

  <variable name="WALL_THICKNESS" value="0.2"></variable>

  <!-- Wall with a single door -->
  <element class="vertical_plane">
    <x0>-10</x0> <y0>-10</y0>
    <x1>-10</x1> <y1>10</y1>
    <z>0.0</z> <height>3.0</height>
    <cull_face>NONE</cull_face>
    <texture>https://mrpt.github.io/mvsim-models/textures-cgbookcase/wall-bricks-01.png</texture>
    <texture_size_x>2.5</texture_size_x>
    <texture_size_y>2.5</texture_size_y>
    <thickness>${WALL_THICKNESS}</thickness>  <!-- Wall thickness in meters -->

    <!-- Door at 50% position (middle of wall), 1.2m wide, standard height -->
    <door>
      <position>0.5</position>  <!-- 0.0 = start point, 1.0 = end point -->
      <width>1.2</width>        <!-- meters -->
      <z_min>0.0</z_min>        <!-- bottom of door -->
      <z_max>2.1</z_max>        <!-- top of door -->
      <name>main_entrance</name>
    </door>
  </element>


</mvsim_world>
```

### 3 世界环境元素

MVSim 支持多种环境元素。

**占据栅格地图**将灰度图像加载为二维障碍物地图，常用于室内导航测试：

```xml
<element class="occupancy_grid">
  <file>map.png</file>
  <resolution>0.05</resolution>  <!-- meters/pixel -->
</element>
```

**高程地图**根据灰度高度图定义地形高度，适合室外场景：

```xml
<element class="elevation_map">
  <resolution>1.0</resolution>
  <elevation_image>terrain.png</elevation_image>
  <elevation_image_min_z>0.0</elevation_image_min_z>
  <elevation_image_max_z>5.0</elevation_image_max_z>
</element>
```

**纹理平面**用于添加可视化地面：

```xml
<element class="horizontal_plane">
  <cull_face>BACK</cull_face>
  <x_min>-25</x_min> <x_max>25</x_max>
  <y_min>-25</y_min> <y_max>25</y_max>
  <z>0.0</z>
  <texture>asphalt.png</texture>
  <texture_size_x>5.0</texture_size_x>
  <texture_size_y>5.0</texture_size_y>
</element>
```

**块体**是静态或动态刚体（箱子、自定义形状），可用作障碍物或可操作物体：

```xml
<block class="obstacle1">
  <shape_from_visual/>
  <visual>
    <model_uri>package://mvsim/models/box.dae</model_uri>
  </visual>
  <init_pose>3.0 2.0 0</init_pose>
  <mass>20</mass>
</block>
```

### 4 车辆动力学模型

MVSim 提供三种主要动力学模型：

- **差速驱动**（`class="differential"`）：用于 TurtleBot3 等双轮机器人，通过线速度和角速度控制。
- **阿克曼转向**（`class="ackermann"`）：用于前轮转向的汽车类车辆，通过线速度和转向角控制。
- **阿克曼传动系统**（`class="ackermann_drivetrain"`）：包含开放式或托森差速器的逼真传动系统模型，可更精确地仿真车辆行为。

每辆车可以使用不同的电机控制器：

- `twist_pid`：接收 `geometry_msgs/msg/Twist` 指令，通过 PID 跟踪速度。这是集成 ROS 2 时最常见的选择。
- `twist_ideal`：即时速度指令，没有动力学延迟。
- `twist_front_steer_pid`：用于通过线速度和转向角控制的阿克曼车辆。
- `raw`：直接控制车轮扭矩。

### 5 传感器噪声和配置

MVSim 传感器支持可配置的噪声模型。例如，包含噪声参数的 IMU 传感器：

```xml
<sensor class="imu" name="imu1">
  <pose>0 0 0.5 0 0 0</pose>  <!-- x y z roll pitch yaw -->
  <rate_hz>100</rate_hz>

  <!-- Gyroscope noise -->
  <gyroscope_noise>
    <noise_std>1e-3</noise_std>           <!-- rad/s -->
    <bias_initial_std>1e-4</bias_initial_std>
    <bias_drift>1e-6</bias_drift>
  </gyroscope_noise>

  <!-- Accelerometer noise -->
  <accelerometer_noise>
    <noise_std>1e-2</noise_std>           <!-- m/s^2 -->
    <bias_initial_std>1e-3</bias_initial_std>
    <bias_drift>1e-5</bias_drift>
  </accelerometer_noise>
</sensor>
```

激光雷达支持量程、角分辨率和噪声等参数：

```xml
<sensor class="laser" name="laser1">
  <pose>0.15 0 0.3 0 0 0</pose>
  <rate_hz>10</rate_hz>
  <ray_count>360</ray_count>
  <fov_degrees>360</fov_degrees>
  <range_max>20.0</range_max>
  <range_std_noise>0.01</range_std_noise>  <!-- meters -->
  <raytrace_3d>true</raytrace_3d>  <!-- use 3D collision for 2D scans -->
</sensor>
```

### 6 其他功能

**多机器人仿真：** MVSim 原生支持同一世界中的多辆车。每辆车拥有独立的 ROS 2 命名空间、TF 树和话题集合。机器人可以通过传感器检测彼此，并通过碰撞发生物理交互。

**属性区域：** 可以在世界中定义具有不同物理属性的区域，例如摩擦系数不同的区域，或 GNSS 传感器停止报告位置的 GPS 拒止区域。

**动画角色：** MVSim 支持沿航路点运动的骨骼动画三维角色（例如行人），可用于测试动态环境中的感知和规划。

**关节和铰接车辆：** 车辆可以通过距离关节（绳索／线缆）或旋转关节（铰链）连接，从而仿真拖车、牵引绳和铰接系统。

**XML 高级功能：** 世界文件支持 `<include>` 指令、变量替换、数学表达式、`<for>` 循环和 `<if>` 条件语句，可据此以程序化方式生成复杂环境。

**无界面及超实时仿真：** MVSim 可以不启动图形界面，并按可配置速度运行，适合自动化测试和强化学习工作流。

## 与其他仿真器比较

MVSim 与其他仿真器的侧重点不同。

**优势：**

- 非常轻量：CPU 和内存占用低，启动快。
- 专注于车辆动力学，提供多种摩擦和传动系统模型。
- 世界格式基于简单的 XML，易于上手。
- 原生支持多机器人，并为每辆车提供独立 ROS 2 命名空间。
- 支持超实时仿真，适合批量测试。
- 可通过 XML 循环和条件语句以程序化方式生成世界。

**局限：**

- 物理仿真是二维的（Box2D），不具备完整的三维刚体动力学。物体不会倾倒或飞起。高程地图添加了地形高度，但物理模型本质上仍为二维。
- 传感器仿真不及完整三维仿真器精细：摄像头渲染和激光雷达模型具有相应功能，但不追求照片级真实感。
- 与 Gazebo 相比，预构建模型和环境的生态较小。
- 主要面向轮式移动机器人。

## 更多资源

- [MVSim 文档](https://mvsimulator.readthedocs.io/)
- [MVSim GitHub 仓库](https://github.com/MRPT/mvsim)
- [MVSim 论文（SoftwareX）](https://doi.org/10.1016/j.softx.2023.101443)
