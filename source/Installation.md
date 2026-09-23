---
translation_status: machine_translated
source: Installation.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="installation"></span> <span id="rollinginstall"></span><span id="installationguide"></span>

# 安装

安装 ROS 2 Rolling Ridley 的选项 :

<span id="binary-packages"></span> <span id="binary-package-platforms"></span>

## 二进制软件包

只为第一级操作系统创建二进制 [REP-2000号报告](https://reps.openrobotics.org/rep-2000/#rolling-ridley-june-2020---ongoing)。如果您没有运行以下任何操作系统,您可能需要从源头构建或使用一个 [容器溶液](How-To-Guides/Run-2-nodes-in-single-or-separate-docker-containers.md) 运行 ROS 2 在你的平台上。

我们为以下平台提供ROS 2 二进制软件包:

- Ubuntu Linux (amd64 / arch64) - 贾米哲鱼(22.04).

  - [deb 软件包](Installation/Ubuntu-Install-Debs.md) (建议)

  - [二进制归档](Installation/Alternatives/Ubuntu-Install-Binary.md)

- 红帽企业 Linux 8 (amd64)

  - [RPM 软件包](Installation/RHEL-Install-RPMs.md) (建议)

  - [二进制归档](Installation/Alternatives/RHEL-Install-Binary.md)

- Windows 10 (amd64) (英语).

  - [Windows 二进制( VS 2019)](Installation/Windows-Install-Binary.md)

<span id="building-from-source"></span> <span id="id1"></span>

## 从源头建楼

我们支持在以下平台上从源头构建ROS 2:

- [Ubuntu Linux 22.04 (英语).](Installation/Alternatives/Ubuntu-Development-Setup.md)

- [视窗 10](Installation/Alternatives/Windows-Development-Setup.md)

- [莱尔-8](Installation/Alternatives/RHEL-Development-Setup.md)

- [macOS](Installation/Alternatives/macOS-Development-Setup.md)

<span id="which-install-should-you-choose"></span>

## 你选哪一个安装?

从二进制软件包或来源安装将同时导致一个功能完备且可用的ROS 2安装. 选项之间的差异取决于您计划对ROS 2 做什么.

**二进制软件包** 用于一般用途,并提供已经安装的 ROS 2. 这对想潜入并立即开始使用 ROS 2 的人来说是巨大的。

Linux 用户有两种安装二进制包的选项:

- 软件包(视平台而定,是数据或RPMS)

- 二进制归档

从软件包安装是推荐的方法,因为它会自动安装必要的依赖性,同时也在常规系统更新的同时进行更新。然而,您需要root访问才能安装 deb 软件包。如果您没有root访问权限,二进制归档是下一个最佳选择。

从二进制包中选择安装的Windows用户只有二进制存档选项(deb包是Ubuntu/Debian独家的).

**从源头建楼** 用于正在寻找修改或明确省略 ROS 2 基础的开发者。对于不支持二进制的平台,也建议这样做。从源头建置还允许您选择安装 ROS 2 的绝对最新版本。

<span id="contributing-to-ros-2-core"></span>

### 为ROS 2核心做贡献?

如果您计划直接为ROS 2 核心软件包做出贡献,您可以安装此软件包 [来源的最新发展](Installation/Alternatives/Latest-Development-Setup.md) 与该设备共享安装指令 [滚动分发](Releases.md#rolling-distribution).
