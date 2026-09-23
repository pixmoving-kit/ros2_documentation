---
translation_status: machine_translated
source: Tutorials/Demos/Managed-Nodes.rst
---

<span id="managing-node-lifecycles-example"></span>

# 管理节点生命周期：示例

为节点管理寿命周期,可以对ROS系统状态进行更大的控制。这个例子使用一个简单的谈话者/听众对节点管理,以显示如何执行和使用所管理的生命周期。您可以用这个例子来理解和实验如何以这种方式管理节点。

**领域:ROS-框架 QQ 内容类型:实例 QQ 经验:专家**

<span id="summary"></span>

## 小结

ROS 2 引入了管理节点的概念,也称为生命周期节点。 这些节点可以用来确保资源在节点在生命周期状态之间移动时被正确关闭、激活、关闭和清理。 一个常见的用途是控制硬件的节点,在节点中必须启动、配置和关闭相机、lidar、机动车驱动器和其他传感器和激活器等设备。

使用寿命周期节点有助于确保硬件在准备完毕时才被禁用,并且在关闭或错误恢复时安全释放。以下软件包使您能够执行这些管理的节点: [rclcpp_lifecycle](https://index.ros.org/p/rclcpp_lifecycle/) (执行库)和 [lifecycle_msgs](https://index.ros.org/p/lifecycle_msgs/) (交叉定义).

<span id="prerequisites"></span>

## 前提条件

见 [安装指令](../../Installation.md) 关于安装ROS 2的详情。

<span id="example"></span>

## 示例

<span id="access-the-example"></span>

### 访问示例

有关如何运行示例的信息如下: [lifecycle_demo_launch.py](https://github.com/ros2/demos/blob/rolling/lifecycle_py/launch/lifecycle_demo_launch.py)

<span id="commentary"></span>

### 评 注

有关如何运行以及发生什么的更多信息,请参见: [生命周期读取](https://github.com/ros2/demos/blob/rolling/lifecycle/README.rst)

<span id="related-content"></span>

## 相关内容

Packages/reference:

- [rclcpp_lifecycle](https://index.ros.org/p/rclcpp_lifecycle/) (实施库):包含生命周期实施原型的软件包.

- [lifecycle_msgs](https://index.ros.org/p/lifecycle_msgs/) (界面定义):包含一些与生命周期相关的信息和服务定义的软件包.

- [寿命周期](https://docs.ros.org/en/rolling/p/lifecycle/):包含用于生命周期执行的演示文稿的软件包。
