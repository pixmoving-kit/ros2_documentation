---
translation_status: machine_translated
source: The-ROS2-Project/Release-Schedule.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="release-schedule"></span>

# 发布计划

<span id="frequency"></span>

## 频率

新的ROS 2 发布版本 **每12个月**其原理是,周期较短(如6个月)会导致大量间接费用,并可能同时出现许多活性释放(假设其支持长度相同),另一方面,较长的周期(如2年)对于用户来说太长,无法等待ROS 2的发布中提供新的功能。

<span id="targeted-platforms"></span>

## 目标平台

由于非LTS(长期支持)Ubuntu的释放只支持9个月,ROS 2将不会针对这些非LTS Ubuntu的释放。 **单人** Ubuntu LTS. 其原理是,完全支持两个Ubuntu LTS版本对我们的维护者来说是一个巨大的间接费用,因为可能存在长达两年的上游依赖关系。 根据具体情况,ROS 2 分布可以支持一个更古老的Ubuntu LTS分布,作为第3级,社区支持的平台。

由于macOS(或至少酿酒)和Windows都是滚动平台,因此我们的目标是支持ROS 2发行时可用的最新版本。 对于Debian来说,我们还打算瞄准最新的稳定版本;但是,如果该版本比Ubuntu版本落后两年,那么它可能是不可能的。

<span id="support"></span>

## 支助

<span id="lts-releases"></span>

### LTS 发布

自从Ubuntu LTS发布后 **5 岁** 在标准支持中,我们的目标是每个ROS LTS的发布都有类似的支持寿命. 甚至在Ubuntu LTS发布一个月后(通常指5月的ROS 2发布),新的ROS 2发布会发生. ROS 2发布会支持到Ubuntu LTS发布的标准支持窗口结束,距离ROS 2发布日期还有4年零11个月.

<span id="non-lts-releases"></span>

### 非 LTS 释放

为了向社区提供频繁的发布,在奇数年里将发布非LTS ROS 2发布版本。它的目标总是与之前的ROS 2 LTS发布版本相同,但只支持 **1.5岁**。这一期限可确保非LTS与下一次ROS LTS发布重叠6个月,以提供一个足够长的过渡窗口。

<span id="releases-and-support-duration"></span>

### 释放和支助期限

- 2025年5月:Kilted Kaiju:非LTS发布,支持1.5年.

- 2026年5月:Lyrical Luth:LTS发布,支持5年.

- 2027年5月:MTurtle:非LTS发布,支持1.5年.

- 2028年5月:NTurtle:LTS发布,支持5年.

- 等,每年在LTS和非LTS释放之间交替发布
