---
translation_status: machine_translated
source: Tutorials/Advanced/Simulators/MVSim/Defining-Worlds-MVSim.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="defining-worlds-robots-and-sensors"></span>

# 定义世界、机器人和传感器

**目标：** 学习定义MVSim世界文件的基本原理,添加车辆和传感器,以及可用的主要功能.

**教程级别：** 高级

**用时：** 30分钟

<span id="background"></span>

## 背景

MVSim Worlds在XML文件中定义(`.world.xml`),一个世界文件描述了环境(地面,墙壁,障碍),载体(动力模型,形状,传感器),以及模拟参数(物理时序,GUI选项).

MVSim 提供了一个预定义的载体和传感器定义库,您可以通过 XML 包括的XML 在您的世界中重新使用。您也可以从头到尾定义全部控制 。

<span id="prerequisites"></span>

## 前提条件

你应该完成的 [MVSim 入门](Getting-Started-MVSim.md) 教程并安装了 MVSim 。

<span id="tasks"></span>

## 操作步骤

<span id="minimal-world-file"></span>

### 1 最小世界文件

以下是一个最小的世界文件,它用一个机器人创造了一个空的环境:

``` xml
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

另存为 `my_world.world.xml` 并发射:

``` console
$ mvsim launch my_world.world.xml
```

<span id="using-predefined-vehicles-and-sensors"></span>

### 2 使用预先界定的车辆和传感器

而不是从零开始定义车辆, 您可以使用使用 MVSim 的预定义。 这些是 XML 文件 。 `definitions/` MVSim软件包目录.

**可用车辆:**

- `turtlebot3_burger.vehicle.xml` - TurtleBot3汉堡(差别驱动器)

- `jackal.vehicle.xml` - Clearpath Jackal UGV(轮差)

- `ackermann.vehicle.xml` - 一般Ackermann(类似汽车)车辆

- `pickup.vehicle.xml` - 皮卡车(阿克曼)

- `agricobiot2.vehicle.xml` - 农业机器人(Ackermann drivetrain)

**可用的传感器 :**

- `lidar2d.sensor.xml` - 通用2D激光扫描仪

- `rplidar-a2.sensor.xml` - RPLidar A2号

- `velodyne-vlp16.sensor.xml` - Velodyne VLP-16 3D LiDAR(英语:

- `ouster-os1.sensor.xml` – Ouster OS1 3D LiDAR(英语:Ouster OS1 3D LiDAR) 互联网电影数据库(IMDb)上"Ouster OS1"的资料(英文)

- `helios-32-FOV-70.sensor.xml` - 赫利俄斯32波 3D LiDAR

- `camera.sensor.xml` - RGB摄像头

- `rgbd_camera.sensor.xml` - 深度摄像头(RGBD)

- `imu.sensor.xml` - 惰性计量单位

- `gnss.sensor.xml` - 全球定位系统/全球导航卫星系统接收器

使用带有传感器的预定义飞行器,使用XML包括:

``` xml
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

<span id="world-environment-elements"></span>

### 3 世界环境要素

MVSim支持几种类型的环境元素:

**占用网格图** 装入灰度图像作为二维障碍图,通常用于室内导航测试:

``` xml
<element class="occupancy_grid">
  <file>map.png</file>
  <resolution>0.05</resolution>  <!-- meters/pixel -->
</element>
```

**升降图** 从灰度高度图图像定义地形高度, 对室外情景有用 :

``` xml
<element class="elevation_map">
  <resolution>1.0</resolution>
  <elevation_image>terrain.png</elevation_image>
  <elevation_image_min_z>0.0</elevation_image_min_z>
  <elevation_image_max_z>5.0</elevation_image_max_z>
</element>
```

**有纹理的飞机** 添加视觉地面表面 :

``` xml
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

**块** 属于静态或动态的刚性体(框,自定义形状),可用作障碍或可操纵对象:

``` xml
<block class="obstacle1">
  <shape_from_visual/>
  <visual>
    <model_uri>package://mvsim/models/box.dae</model_uri>
  </visual>
  <init_pose>3.0 2.0 0</init_pose>
  <mass>20</mass>
</block>
```

<span id="vehicle-dynamics-models"></span>

### 4 车辆动力学模型

MVSim提供三个主要动态模型:

- **差异驱动器** (`class="differential"`:TurtleBot3等双轮机器人,通过线性速度和角速度控制.

- **阿克尔曼** (`class="ackermann"`:具有前轮转向架的车型车辆,通过线性速度和转向角度控制.

- **阿克尔曼驱动列车** (`class="ackermann_drivetrain"`:具有开放或托尔森差分的实事求是的驱动列车模型,可用于更精确的车辆行为模拟.

每辆车可以使用不同的机动控制器:

- `twist_pid`编号: 接受 `geometry_msgs/msg/Twist` 命令使用 PID 速度跟踪。这是ROS 2 集成的最常用选择。

- `twist_ideal`:即时速度指令(无动态延迟).

- `twist_front_steer_pid`:用于通过线性速度和转向角控制的Ackermann车辆.

- `raw`:直接轮扭控制.

<span id="sensor-noise-and-configuration"></span>

### 5 传感器噪音和配置

MVSim中的传感器支持可配置噪声模型。例如,具有噪声参数的IMU传感器:

``` xml
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

射程、角分辨率和噪声的LiDAR传感器支持参数:

``` xml
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

<span id="additional-features"></span>

### 6 其他特点

**多机器人模拟 :** MVSim在本土上支持同一世界的多种车辆,每辆车辆都得到了自己的ROS 2命名空间,TF树,以及一组主题. 机器人可以用他们的传感器互相探测,并通过碰撞进行物理交互.

**地产区:** 您可以定义世界上具有不同物理特性的区域,如不同摩擦系数或全球导航卫星系统传感器停止报告位置的GPS拒绝区。

**动画演员:** MVSim支持遵循路标路径的骨骼动画3D字符(如行人),在动态环境中用于测试知觉和规划.

**接头和装配的车辆:** 车辆可以使用距离关节(ropes/cables)或回转关节(hinges)连接,可以模拟拖车,拖绳,以及清晰的系统.

**XML 高级特性 :** World 文件支持 `<include>` 指令、可变替换、数学表达式、 `<for>` 循环,和 `<if>` 有条件的,使得在程序上产生复杂的环境成为可能.

**无头且速度快于实时 :** MVSim可以在没有图形用户界面和可配置的模拟速度的情况下运行,这对自动化测试和强化学习工作流程有用.

<span id="comparison-with-other-simulators"></span>

## 与其他模拟器的比较

MVSim 与其他模拟器相比占据了不同的位置:

**强度 :**

- 非常轻量级:低CPU和内存使用率,快速启动时间.

- 具有多种摩擦力和驱动力模型的有重点车辆动力学.

- 基于 XML 的简单世界格式, 容易启动 。

- 原生多机器人支持 与车辆 ROS 2 命名空间。

- 批量测试的比实时更快的模拟.

- 通过XML循环和条件程序世界生成.

**限制:**

- 物理是2D(Box2D):没有完整的3D刚性体动力学,物体不会向上倾斜或飞翔,高度图会增加地形高度,但物理基本上仍然是2D.

- 传感器模拟比完整的3D模拟器更不详细:相机渲染和LiDAR模型是功能性的,但并非光现实性的.

- 与Gazebo相比,预建模型和环境的生态系统较小.

- 专注于轮式移动机器人.

<span id="further-resources"></span>

## 进一步资源

- [MVSim 文档](https://mvsimulator.readthedocs.io/)

- [MVSim GitHub 存储器](https://github.com/MRPT/mvsim)

- [MVSim纸(软件X)](https://doi.org/10.1016/j.softx.2023.101443)
