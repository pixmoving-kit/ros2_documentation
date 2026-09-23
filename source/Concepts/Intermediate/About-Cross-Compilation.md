---
translation_status: machine_translated
source: Concepts/Intermediate/About-Cross-Compilation.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="cross-compilation"></span>

# 交叉编译

<span id="overview"></span>

## 概述

Open Robotics 为多个平台提供预建的ROS 2 套件,但一些开发者仍然依赖 [交叉汇编](https://en.wikipedia.org/wiki/Cross_compiler) 原因不同,例如:  
- 开发机与目标系统不匹配.

- 为特定核心建筑进行修饰(例如为Raspberry Pi3建造时设置-mcpu=cortex-a53-mfpu=neon-fp-armv8).

- 瞄准Open Robotics发布的预建图像所支持的文件系统以外的文件系统.

<span id="how-does-it-work"></span>

## 怎么会这样?

交叉编译的简单软件(例如,对外部库没有依赖性)相对简单,只需要使用交叉编译工具链而不是本土工具链.

有许多因素使这一进程更加复杂:  
- 正在构建的软件必须支持目标架构. 架构特定代码必须适当隔离,并在构建过程中根据目标架构启用,例子包括组装代码.

- 所有依赖性(如库)在使用它们的目标软件被交叉编译之前,必须作为预建或交叉编译的软件包存在.

- 在使用构建工具(如colcon)构建软件堆栈(相对于独立软件)时,预计构建工具提供了一个机制,使开发者能够对堆栈中每个软件片所使用的基础构建系统进行交叉汇编.

<span id="alternatives"></span>

## 其他安装方式

交叉汇编的替代办法是: [构建多平台的 Docker 图像](https://github.com/docker/buildx#building-multi-platform-images) 使用 `docker buildx`.
