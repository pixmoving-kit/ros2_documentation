<span id="installation"></span>
<span id="rollinginstall"></span>
<span id="installationguide"></span>

# 安装

ROS 2 Rolling Ridley 的安装选项如下。

<span id="binary-packages"></span>
<span id="binary-package-platforms"></span>

## 二进制软件包

我们仅为 [REP-2000](https://reps.openrobotics.org/rep-2000/#rolling-ridley-june-2020---ongoing) 中列出的 Tier 1 操作系统构建二进制软件包。如果你使用的操作系统不在以下列表中，可能需要从源码构建，或使用[容器方案](How-To-Guides/Run-2-nodes-in-single-or-separate-docker-containers.md)在你的平台上运行 ROS 2。

我们为以下平台提供 ROS 2 二进制软件包：

- Ubuntu Linux（amd64 / aarch64）— Jammy Jellyfish（22.04）：[deb 软件包](Installation/Ubuntu-Install-Debs.md)（推荐）或[二进制归档包](Installation/Alternatives/Ubuntu-Install-Binary.md)。
- Red Hat Enterprise Linux 8（amd64）：[RPM 软件包](Installation/RHEL-Install-RPMs.md)（推荐）或[二进制归档包](Installation/Alternatives/RHEL-Install-Binary.md)。
- Windows 10（amd64）：[Windows 二进制归档包（VS 2019）](Installation/Windows-Install-Binary.md)。

<span id="building-from-source"></span>
<span id="id1"></span>

## 从源码构建

我们支持在以下平台上从源码构建 ROS 2：

- [Ubuntu Linux 22.04](Installation/Alternatives/Ubuntu-Development-Setup.md)
- [Windows 10](Installation/Alternatives/Windows-Development-Setup.md)
- [RHEL 8](Installation/Alternatives/RHEL-Development-Setup.md)
- [macOS](Installation/Alternatives/macOS-Development-Setup.md)

<span id="which-install-should-you-choose"></span>

## 应该选择哪种安装方式？

通过二进制软件包安装和从源码构建，都能获得功能完整、可用的 ROS 2 环境。选择哪种方式，取决于你打算如何使用 ROS 2。

**二进制软件包**面向一般用途，提供已经构建好的 ROS 2。这适合希望立即上手、直接使用现成 ROS 2 的用户。

Linux 用户可以选择两种二进制安装方式：

- 软件包（根据平台选择 deb 或 RPM）
- 二进制归档包

推荐通过软件包安装，因为它会自动安装必要的依赖，并随常规系统更新一起更新。不过，安装 deb 软件包需要 root 权限。如果没有 root 权限，二进制归档包是次优选择。

Windows 用户如果选择二进制安装，则只能使用二进制归档包（deb 软件包仅适用于 Ubuntu/Debian）。

**从源码构建**适合希望修改或明确排除 ROS 2 基础组件中某些部分的开发者。对于不提供二进制软件包的平台，也推荐采用这种方式。从源码构建还可以让你安装 ROS 2 的最新版本。

<span id="contributing-to-ros-2-core"></span>

### 想为 ROS 2 核心作贡献？

如果计划直接为 ROS 2 核心软件包作贡献，可以[从源码安装最新开发版本](Installation/Alternatives/Latest-Development-Setup.md)，其安装说明与 [Rolling 发行版](Releases.md#rolling-distribution)相同。

以下是本部分的其他安装相关文档：

- [其他安装方式](Installation/Alternatives.md)
- [维护源码工作副本](Installation/Maintaining-a-Source-Checkout.md)
- [使用预发布二进制软件包进行测试](Installation/Testing.md)
- [RMW 实现](Installation/RMW-Implementations.md)
- [ROS 2 镜像站](Installation/ROS-2-Mirrors.md)
