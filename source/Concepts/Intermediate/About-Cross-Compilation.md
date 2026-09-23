<span id="cross-compilation"></span>
# 交叉编译

<span id="overview"></span>
## 概述

Open Robotics 为多个平台提供了预编译的 ROS 2 软件包，但一些开发者仍出于以下原因使用[交叉编译](https://en.wikipedia.org/wiki/Cross_compiler)：

- 开发机器与目标系统不同。
- 针对特定的处理器核心架构优化构建，例如为 Raspberry Pi 3 构建时设置 `-mcpu=cortex-a53 -mfpu=neon-fp-armv8`。
- 目标文件系统不在 Open Robotics 发布的预构建镜像所支持的范围内。

<span id="how-does-it-work"></span>
## 工作原理

对简单软件（例如不依赖外部库的软件）进行交叉编译相对容易，只需使用交叉编译工具链替代本机工具链。

以下因素会使这一过程变得复杂：

- 待构建的软件必须支持目标架构。必须妥善隔离架构专用代码，并在构建时根据目标架构启用相应代码，例如汇编代码。
- 在对目标软件进行交叉编译之前，必须准备好它的所有依赖项（例如库），这些依赖可以是预编译的软件包，也可以是交叉编译的软件包。
- 使用构建工具（例如 colcon）构建软件栈而非独立软件时，构建工具应提供一种机制，让开发者能够在软件栈中各个软件所使用的底层构建系统上启用交叉编译。

<span id="alternatives"></span>
## 替代方案

交叉编译的一种替代方案是使用 `docker buildx` [构建多平台 Docker 镜像](https://github.com/docker/buildx#building-multi-platform-images)。
