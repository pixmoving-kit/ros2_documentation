<span id="release-schedule"></span>

# 发布计划

<span id="frequency"></span>

## 发布频率

ROS 2 每 **12 个月**发布一个新版本。更短的周期（例如 6 个月）会带来显著的维护开销，并可能使多个发行版同时处于维护期，前提是它们的支持期限相同。更长的周期（例如 2 年）则会让用户等待新功能进入 ROS 2 发行版的时间过长。

<span id="targeted-platforms"></span>

## 目标平台

Ubuntu 非 LTS（长期支持）版本的支持期只有 9 个月，因此 ROS 2 不以这些版本为目标平台。每个 ROS 2 发行版只为**一个** Ubuntu LTS 版本提供完整的一级支持。完整支持两个 Ubuntu LTS 版本会给维护者带来巨大开销，因为上游依赖可能相差两年。视具体情况，一个 ROS 2 发行版也可能将较早的 Ubuntu LTS 版本列为由社区支持的三级平台。

由于 macOS（至少 brew）和 Windows 采用滚动更新，我们力求支持 ROS 2 发行时可用的最新版本。Debian 同样以最新稳定版为目标；但如果它比对应 Ubuntu 版本落后两年，就可能无法支持。

<span id="support"></span>

## 支持期限

<span id="lts-releases"></span>

### LTS 发行版

Ubuntu LTS 提供 **5 年**标准支持，因此我们希望 ROS LTS 发行版拥有相近的支持期限。在偶数年，ROS 2 新发行版会在 Ubuntu LTS 发布一个月后发布，通常是在 5 月。ROS 2 的支持持续到对应 Ubuntu LTS 标准支持期结束，即从 ROS 2 发布之日起约 4 年 11 个月。

<span id="non-lts-releases"></span>

### 非 LTS 发行版

为社区提供更频繁的更新，奇数年会发布一个非 LTS ROS 2 发行版。它始终以之前 ROS 2 LTS 所使用的同一 Ubuntu LTS 为目标平台，但只支持 **1.5 年**。这样，它会与下一个 ROS LTS 发行版重叠 6 个月，为迁移留出足够时间。

<span id="releases-and-support-duration"></span>

### 发行版及支持时长

- 2025 年 5 月：Kilted Kaiju，非 LTS，支持 1.5 年。
- 2026 年 5 月：Lyrical Luth，LTS，支持 5 年。
- 2027 年 5 月：M Turtle，非 LTS，支持 1.5 年。
- 2028 年 5 月：N Turtle，LTS，支持 5 年。
- 此后按年交替发布 LTS 和非 LTS 版本。
