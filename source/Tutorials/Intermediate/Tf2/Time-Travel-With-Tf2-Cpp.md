---
translation_status: machine_translated
source: Tutorials/Intermediate/Tf2/Time-Travel-With-Tf2-Cpp.rst
---

<span id="traveling-in-time-c"></span> <span id="timetravelwithtf2cpp"></span>

# 时间回溯（C++）

**目标：** 了解 tf2 的高级时间旅行特征.

**教程级别：** 中级

**用时：** 10分钟

<span id="background"></span>

## 背景

在前一次的辅导中,我们讨论了 [tf2 和时间的基本知识](Learning-About-Tf2-And-Time-Cpp.md)。这个教程将使我们更进一步,并揭示一个强大的 tf2 技巧:时间旅行。简言之, tf2 库的关键特征之一是它能够及时、在空间中转换数据。

这个 tf2 时间旅行功能可以用于各种任务,比如长期监视机器人的姿势,或者建立一个跟随领导者的“步骤”的后继机器人。我们将利用这个时间旅行功能来追溯时间和程序的变换。 `turtle2` 后5秒钟 `carrot1`.

<span id="time-travel"></span>

## 时间旅行

首先,让我们回到上次教程结束的地方 [利用时间](Learning-About-Tf2-And-Time-Cpp.md)。转到您的 `learning_tf2_cpp` 软件包。

现在,我们不要让第二只乌龟去现在胡萝卜所在的地方,而是让第二只乌龟去5秒前的第一只胡萝卜所在的地方。 `lookupTransform()` 呼叫进来 `turtle_tf2_listener.cpp` 文件创建到

``` C++
rclcpp::Time when = this->get_clock()->now() - rclcpp::Duration(5, 0);
t = tf_buffer_->lookupTransform(
    toFrameRel,
    fromFrameRel,
    when,
    50ms);
```

现在如果你运行这个,在前5秒,第二只乌龟将不知道该去哪里,因为我们还没有5秒的胡萝卜姿势历史。 但在这5秒之后会发生什么? 让我们试试:

##### XML 数据

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_fixed_frame_demo_launch.xml
```

##### 也门

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_fixed_frame_demo_launch.yaml
```

##### Python

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_fixed_frame_demo_launch.py
```

![](images/turtlesim_delay1.png)

现在,你应该注意到,你的乌龟像这个截图一样无节制地到处乱跑。 让我们试着理解这种行为背后的原因。

1.  在我们的代码中,我们问 tf2 的如下问题:“什么是: `carrot1` 5秒钟前,与 `turtle2` 5秒钟前?”” 这意味着我们正在控制第二只海龟,其基础是5秒钟前的海龟位置以及第一只胡萝卜在5秒钟前的位置。

2.  然而,我们真正想要问的是:“我们如何看待这些冲突?” `carrot1` 5秒钟前,相对于当前位置 `turtle2`?”.

<span id="advanced-api-for-lookuptransform"></span>

## 高级 API 用于查找 Transform ()

要问 tf2 这个特定的问题,我们将使用一个高级API,它赋予我们明确表达何时获得特定变换的权力。 `lookupTransform()` 带有额外参数的方法。 您的代码现在看起来是这样的 :

``` C++
rclcpp::Time now = this->get_clock()->now();
rclcpp::Time when = now - rclcpp::Duration(5, 0);
t = tf_buffer_->lookupTransform(
    toFrameRel,
    now,
    fromFrameRel,
    when,
    "world",
    50ms);
```

高级API 用于 `lookupTransform()` 需要六个理由:

1.  目标框架

2.  转换到

3.  来源框架

4.  评估源框架的时间

5.  长期不变的框架,在此情况下 `world` 边框

6.  等待目标框架到位的时间

简言之,tf2在背景中做如下的计算。过去,它计算了从“%”到“%”的变换。 `carrot1` 页:1 `world`。在 `world` tf2时间从过去到现在。当前, tf2 计算转换 `world` 页:1 `turtle2`.

<span id="checking-the-results"></span>

## 检查结果

让我们再次进行模拟,这次是高级时间旅行API:

##### XML 数据

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_fixed_frame_demo_launch.xml
```

##### 也门

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_fixed_frame_demo_launch.yaml
```

##### Python

``` console
$ ros2 launch learning_tf2_cpp turtle_tf2_fixed_frame_demo_launch.py
```

![](images/turtlesim_delay2.png)

是的,第二只乌龟被引向 5秒前的第一只胡萝卜!

<span id="summary"></span>

## 小结

在这个教程中,您已经看到 tf2. 的高级特性之一, 您学会了 tf2 可以及时转换数据, 并学会如何用 tourtlsim 例子来做到这一点 。 tf2 允许您返回时间, 通过使用高级的来在龟的旧和现的姿势之间做框架转换 。 `lookupTransform()` API. (英语).
