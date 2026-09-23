---
translation_status: machine_translated
source: Tutorials/Intermediate/Tf2/Learning-About-Tf2-And-Time-Cpp.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-time-c"></span> <span id="learningabouttf2andtimecpp"></span>

# 使用时间（C++）

**目标：** 学习如何在特定时间获得一个变换, 等待在 tf2 树上可用一个变换 `lookupTransform()` 函数。

**教程级别：** 中级

**用时：** 10分钟

<span id="background"></span>

## 背景

在之前的教程中,我们通过写一个 [tf2 广播机](Writing-A-Tf2-Broadcaster-Cpp.md) 备注a [tf2 收听器](Writing-A-Tf2-Listener-Cpp.md)我们还学会了如何 [在变换树上添加新框架](Adding-A-Frame-Cpp.md) 并学习了 tf2 如何跟踪坐标框树 。 此树会随时间变化, tf2 保存每次变换的时间快照( 默认为10秒)。 直到现在, 我们使用 `lookupTransform()` 函数可以访问 tf2 树中最新的变换,而不知道何时记录了变换。此教程将教你如何在特定时间获得变换。

<span id="tasks"></span>

## 操作步骤

<span id="update-the-listener-node"></span>

### 1 更新收听器节点

让我们回到我们结束于 [添加框架教程](Adding-A-Frame-Cpp.md)。转到 `learning_tf2_cpp` 软件包。打开 `turtle_tf2_listener.cpp` 看看这个... `lookupTransform()` 调用 :

``` C++
t = tf_buffer_->lookupTransform(
   toFrameRel,
   fromFrameRel,
   tf2::TimePointZero);
```

你可以看到,我们指定了一个时间等于0,通过呼叫 `tf2::TimePointZero`.

> **说明**
>
> 那个... `tf2` 软件包有自己的时间类型 `tf2::TimePoint`,这与 `rclcpp::Time`软件包中的许多 API `tf2_ros` 自动转换 `rclcpp::Time` 财务报告和财务报告 `tf2::TimePoint`.
>
> `rclcpp::Time(0, 0, this->get_clock()->get_clock_type())` 本来可以在这里使用,但本来可以转换成 `tf2::TimePointZero` 无论如何。

对于 tf2,时间0 表示缓冲器中的“最新可用”变换。现在,修改此线以获得当前变换, `this->get_clock()->now()`:

``` C++
rclcpp::Time now = this->get_clock()->now();
t = tf_buffer_->lookupTransform(
   toFrameRel,
   fromFrameRel,
   now);
```

现在试运行发射文件。

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_demo_launch.xml # .py or .yaml are also acceptable
[INFO] [1629873136.345688064] [listener]: Could not transform turtle2 to turtle1: Lookup would
require extrapolation into the future.  Requested time 1629873136.345539 but the latest data
is at time 1629873136.338804, when looking up transform from frame [turtle1] to frame [turtle2]
```

输出告诉你,这个框架不存在,或者数据是在未来.

为了理解为什么会发生这种情况,我们需要了解缓冲作用。 首先,每个听众都有一个缓冲器,存储来自不同tf2广播机构的所有坐标转换。 其次,当一个广播员发出一个转换器时,需要一些时间才能进入缓冲器(通常需要几毫秒 ) 。 因此,当你在“现在”要求一个帧转换时,你应该等待几毫秒才能得到这一信息。

<span id="fix-the-listener-node"></span>

### 2 修补收听器节点

tf2 提供了一个很好的工具, 将等待变换可用。 您使用此工具时会添加超时参数到 `lookupTransform()`。要修复此选项,请编辑您的代码如下(添加最后的超时参数):

``` C++
rclcpp::Time now = this->get_clock()->now();
t = tf_buffer_->lookupTransform(
   toFrameRel,
   fromFrameRel,
   now,
   50ms);
```

那个... `lookupTransform()` 可以用四个参数, 其中最后一个是可选的超时。 它会屏蔽最长的时间等待超时 。

<span id="check-the-results"></span>

### 3 检查结果

你现在可以运行 发射文件。

##### XML 数据

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_demo_launch.xml
```

##### 也门

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_demo_launch.yaml
```

##### Python

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_demo_launch.py
```

你应该注意 `lookupTransform()` 将实际阻断到两只龟之间的变换( 这通常需要几毫秒) 。 一旦超时( 在此情况下为五十毫秒) , 只有在变换仍未可用时才会提出例外 。

<span id="summary"></span>

## 小结

在这个教程中,你学会了如何在特定的时间戳上获得一个变换,以及如何等待一个变换在 tf2 树上可用时使用 `lookupTransform()` 函数。
