---
translation_status: machine_translated
source: Tutorials/Intermediate/Tf2/Quaternion-Fundamentals.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="quaternion-fundamentals"></span> <span id="quaternionfundamentals"></span>

# 四元数基础

**目标：** 在ROS 2中学习四元使用的基本原理.

**教程级别：** 中级

**用时：** 10分钟

<span id="background"></span>

## 背景

Aquennion是定向的4图表示,它比旋转矩阵更为简洁. Quaternion对于分析涉及三个维度旋转的情况非常有效. Quaternion被广泛应用于机器人,量子力学,计算机视觉,以及3D动画.

你可以更多地了解 基本的数学概念 关于 [维基百科](https://en.wikipedia.org/wiki/Quaternion)。您也可以查看一个可扩展的视频系列 [可视化之四](https://eater.net/quaternions) 制作人: [3蓝1棕色](https://www.youtube.com/3blue1brown).

在此教程中,您将学习 ROS 2 中的四元和转换方法如何工作.

<span id="prerequisites"></span>

## 前提条件

你可以看看库,像 [变换3d](https://github.com/matthew-brett/transforms3d), [scipy.spatial.transform (英语).](https://github.com/scipy/scipy/tree/master/scipy/spatial/transform), [pytransform3d 变形器](https://github.com/rock-learning/pytransform3d), [数字- quarternion](https://github.com/moble/quaternion) 或 时 间 [搅拌机. mathutils](https://docs.blender.org/api/master/mathutils.html).

然而,这不是一个困难的要求,你可以坚持其他最适合你的几何转换库.

<span id="components-of-a-quaternion"></span>

## 之四的组成部分

ROS 2 使用四元来跟踪和应用旋转。 `(x, y, z, w)`在ROS 2中, `w` 虽然是最后的 但是在艾根这样的库里 `w` 通常使用的单位之四,不产生 x/y/z 轴的旋转是 `(0, 0, 0, 1)`,并可以如下方式创建:

``` C++
#include <tf2/LinearMath/Quaternion.h>
...

tf2::Quaternion q;
// Create a quaternion from roll/pitch/yaw in radians (0, 0, 0)
q.setRPY(0, 0, 0);
// Print the quaternion components (0, 0, 0, 1)
RCLCPP_INFO(this->get_logger(), "%f %f %f %f",
            q.x(), q.y(), q.z(), q.w());
```

四角星的大小应该始终是之一。如果数字错误导致一个四角星的大小, ROS 2 会打印警告。 要避免这些警告, 将四角星正常化 :

``` C++
q.normalize();
```

<span id="quaternion-types-in-ros-2"></span>

## ROS 2中的 Quarternion 类型

ROS 2使用两个四角数据类型: `tf2::Quaternion` 及其等同条款 `geometry_msgs::msg::Quaternion`要在 C++中转换它们,请使用 `tf2_geometry_msgs`.

``` C++
#include <tf2_geometry_msgs/tf2_geometry_msgs.hpp>
...

tf2::Quaternion tf2_quat, tf2_quat_from_msg;
tf2_quat.setRPY(roll, pitch, yaw);
// Convert tf2::Quaternion to geometry_msgs::msg::Quaternion
geometry_msgs::msg::Quaternion msg_quat = tf2::toMsg(tf2_quat);

// Convert geometry_msgs::msg::Quaternion to tf2::Quaternion
tf2::convert(msg_quat, tf2_quat_from_msg);
// or
tf2::fromMsg(msg_quat, tf2_quat_from_msg);
```

没有 `tf2::Quaternion` 等同于 Python 中。相反,内置 `list` 已使用。

``` python
from geometry_msgs.msg import Quaternion
...

# Create a list of floats, which is compatible with tf2
# Quaternion methods
quat_tf = [0.0, 1.0, 0.0, 0.0]

# Convert a list to geometry_msgs.msg.Quaternion
msg_quat = Quaternion(x=quat_tf[0], y=quat_tf[1], z=quat_tf[2], w=quat_tf[3])
```

<span id="quaternion-operations"></span>

## Quartorn业务

<span id="think-in-rpy-then-convert-to-quaternion"></span>

### 1 在 RPY 中思考, 然后转换为四角形

我们很容易想到轴的旋转,但很难想到四分法。 一项建议是用三个单个旋转来计算目标旋转。 *滚动* (大约X轴), *发球* (关于Y轴),和 *哟* (约Z轴),然后转换为四角形.

``` python
# quaternion_from_euler method is available in turtle_tf2_py/turtle_tf2_py/turtle_tf2_broadcaster.py
q = quaternion_from_euler(1.5707, 0, -1.5707)
print(f'The quaternion representation is x: {q[0]} y: {q[1]} z: {q[2]} w: {q[3]}.')
```

这种方法涉及: [欧拉角度](https://en.wikipedia.org/wiki/Euler_angles)。应用Euler角度有几种方法。上面提到的 ROS 2 所采用的角度叫做 *固定(或静态)框架* RPY. 这意味着三个单独的旋转被应用到原来的不动坐标轴上。这与 *相对边框*,其中转动适用于通过前次转动而变换的坐标轴。

<span id="applying-a-quaternion-rotation"></span>

### 2 应用四角旋转

将一个四元的旋转应用到一个姿势上, 只需将前一个四元的姿势乘以代表所期望的旋转的四元。 此乘法的顺序 。

C++

``` C++
#include <tf2_geometry_msgs/tf2_geometry_msgs.hpp>
...

tf2::Quaternion q_orig, q_rot, q_new;

q_orig.setRPY(0.0, 0.0, 0.0);
// Rotate the previous pose by 180* about X
q_rot.setRPY(3.14159, 0.0, 0.0);
q_new = q_rot * q_orig;
q_new.normalize();
```

Python

``` python
q_orig = quaternion_from_euler(0, 0, 0)
# Rotate the previous pose by 180* about X
q_rot = quaternion_from_euler(3.14159, 0, 0)
q_new = quaternion_multiply(q_rot, q_orig)
```

<span id="inverting-a-quaternion"></span>

### 3 倒置四角线

反之之四的一个简单方法就是否定x、y和z的组件:

``` python
q[0] = -q[0]
q[1] = -q[1]
q[2] = -q[2]
```

> **说明**
>
> 这不应该与否认混淆 *全部( E)* 之四的要点。

<span id="relative-rotations"></span>

### 4 相对轮换

说你有两个四分之一 从同一个框, `q_1` 财务报告和财务报告 `q_2`您想要找到相对旋转, `q_r`,即转换 `q_1` 改为: `q_2` 以下列方式:

``` C++
q_2 = q_r * q_1
```

你可以解决 `q_r` 类似解析矩阵方程。反转 `q_1` 和右乘的两侧,乘法的顺序也很重要:

``` C++
q_r = q_2 * q_1_inverse
```

以下是从之前的机器人姿势到现在的Python机器人姿势的相对旋转的例子:

``` python
def quaternion_multiply(q0, q1):
    """
    Multiplies two quaternions.

    Input
    :param q0: A 4 element array containing the first quaternion (q01, q11, q21, q31)
    :param q1: A 4 element array containing the second quaternion (q02, q12, q22, q32)

    Output
    :return: A 4 element array containing the final quaternion (q03,q13,q23,q33) in (w, x, y, z) order

    """
    # Extract the values from q0
    x0 = q0[0]
    y0 = q0[1]
    z0 = q0[2]
    w0 = q0[3]

    # Extract the values from q1
    x1 = q1[0]
    y1 = q1[1]
    z1 = q1[2]
    w1 = q1[3]

    # Compute the product of the two quaternions, term by term
    q0q1_w = w0 * w1 - x0 * x1 - y0 * y1 - z0 * z1
    q0q1_x = w0 * x1 + x0 * w1 + y0 * z1 - z0 * y1
    q0q1_y = w0 * y1 - x0 * z1 + y0 * w1 + z0 * x1
    q0q1_z = w0 * z1 + x0 * y1 - y0 * x1 + z0 * w1

    # Create a 4 element array containing the final quaternion
    final_quaternion = np.array([q0q1_w, q0q1_x, q0q1_y, q0q1_z])

    # Return a 4 element array containing the final quaternion (q02,q12,q22,q32)
    return final_quaternion

q1_inv[0] = -prev_pose.pose.orientation.x   # Negate for inverse
q1_inv[1] = -prev_pose.pose.orientation.y   # Negate for inverse
q1_inv[2] = -prev_pose.pose.orientation.z   # Negate for inverse
q1_inv[3] = prev_pose.pose.orientation.w

q2[0] = current_pose.pose.orientation.x
q2[1] = current_pose.pose.orientation.y
q2[2] = current_pose.pose.orientation.z
q2[3] = current_pose.pose.orientation.w

qr = quaternion_multiply(q2, q1_inv)
```

<span id="summary"></span>

## 小结

在此教程中, 你学到了一个四元的基本概念及其相关的数学操作, 比如反演和旋转。 你也知道它用在 ROS 2 中的例子, 以及两个独立的 Quaternion 类之间的转换方法。
