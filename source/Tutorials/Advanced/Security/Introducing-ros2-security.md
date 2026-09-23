---
translation_status: machine_translated
source: Tutorials/Advanced/Security/Introducing-ros2-security.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="setting-up-security"></span> <span id="ros-2-security-tutorials"></span><span id="sros2"></span>

# 配置安全机制

**目标：** 设置安全性 `sros2`.

**教程级别：** 高级

**用时：** 15分钟

<span id="background"></span>

## 背景

那个... `sros2` 软件包提供了在 DDS- Security 之上使用 ROS 2 的工具和指令。安全特性已经测试过跨平台(Linux, macOS, 和 Windows)以及不同语言(C++和Python) 。 SROS2 的设计与任何安全的中间软件一起工作,尽管并非所有中间软件都是开源的,支持也因使用中的 ROS 分布而异。

<span id="installation"></span>

## 安装

通常在使用下列设备安装后即可获得担保: [ROS 2 安装指南](../../../Installation.md) 页:1 [配置指南](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)然而,如果您打算从源头安装或切换中间软件执行,请考虑下列说明:

<span id="installing-from-source"></span>

### 从源头安装

在从源头安装之前, 您需要安装最近版本的 Opensl( 1. 0.2 g 或稍后) :

##### Linux

``` console
$ sudo apt update
$ sudo apt install libssl-dev
```

##### 麦克OS

``` console
$ brew install openssl
```

您需要让 OpenSSL 在您的库路径上运行 DDS- Security 演示。 运行以下命令, 并考虑添加到您的库中 `~/.bash_profile`:

``` console
$ export DYLD_LIBRARY_PATH=`brew --prefix openssl`/lib:$DYLD_LIBRARY_PATH
$ export OPENSSL_ROOT_DIR=`brew --prefix openssl`
```

##### Windows

如果您没有安装 OpenSSL, 请跟随 [这些指示](../../../Installation/Windows-Install-Binary.md#windows-install-binary-installing-prerequisites)

Fast DDS 需要额外的 CMake 旗来构建安全插件, 因此需要修改collcon 引用以通过 :

``` console
$ colcon build --symlink-install --cmake-args -DSECURITY=ON --packages-select fastrtps rmw_fastrtps_cpp rmw_fastrtps_dynamic_cpp rmw_fastrtps_shared_cpp
```

<span id="selecting-an-alternate-middleware"></span>

### 选择一个替代的中间软件

如果您选择不使用默认的中间软件执行, 请确定 [更改您的 RMW 执行](../../../Installation/RMW-Implementations.md) 开始之前

ROS 2 允许您在运行时更改 RMW 执行 。 [B. 如何与多项《保护所有移徙工人及其家庭成员权利国际公约》的实施合作](../../../How-To-Guides/Working-with-multiple-RMW-implementations.md) 探索不同的中间软件执行。

请注意,供应商之间的安全通信没有支持。

<span id="run-the-demo"></span>

## 运行演示

<span id="create-a-folder-for-the-security-files"></span>

### 1) 为安全文件创建文件夹

> 开始创建文件夹来存储此演示所需的全部文件 :
>
> ##### Linux
>
> ``` console
> $ mkdir ~/sros2_demo
> ```
>
> ##### 麦克OS
>
> ``` console
> $ mkdir ~/sros2_demo
> ```
>
> ##### Windows
>
> ``` console
> $ md C:\dev\ros2\sros2_demo
> ```

<span id="generate-a-keystore"></span>

### 2) 生成密钥

使用该 `sros2` 创建密钥tore 的公用设备。 密钥tore 中的文件将被用于为 ROS 2 图中的所有参与者提供安全保障 。

##### Linux

``` console
$ cd ~/sros2_demo
$ ros2 security create_keystore demo_keystore
```

##### 麦克OS

``` console
$ cd ~/sros2_demo
$ ros2 security create_keystore demo_keystore
```

##### Windows

``` console
$ cd sros2_demo
$ ros2 security create_keystore demo_keystore
```

<span id="generate-keys-and-certificates"></span>

### 3) 生成密钥和证书

键盘创建后, 用安全性为每个节点创建密钥和证书。 对于我们的演示, 包括说话者和收听者节点。 此命令使用 `create_enclave` 特性,在下一期教学中将更详细地涵盖。

##### Linux

``` console
$ ros2 security create_enclave demo_keystore /talker_listener/talker
$ ros2 security create_enclave demo_keystore /talker_listener/listener
```

##### 麦克OS

``` console
$ ros2 security create_enclave demo_keystore /talker_listener/talker
$ ros2 security create_enclave demo_keystore /talker_listener/listener
```

##### Windows

``` console
$ ros2 security create_enclave demo_keystore /talker_listener/talker
$ ros2 security create_enclave demo_keystore /talker_listener/listener
```

若为: `unable to write 'random state'` 然后设置环境变量 `RANDFILE`.

``` console
$ set RANDFILE=C:\dev\ros2\sros2_demo\.rnd
```

然后重运行以上命令.

<span id="configure-environment-variables"></span>

### 4) 配置环境变量

3个环境变量使中间软件能够定位加密材料,并实现(可能执行)安全。 [ROS 2 DDS-安全整合设计文件](https://design.ros2.org/articles/ros2_dds_security.html).

##### Linux

``` console
$ export ROS_SECURITY_KEYSTORE=~/sros2_demo/demo_keystore
$ export ROS_SECURITY_ENABLE=true
$ export ROS_SECURITY_STRATEGY=Enforce
```

##### 麦克OS

``` console
$ export ROS_SECURITY_KEYSTORE=~/sros2_demo/demo_keystore
$ export ROS_SECURITY_ENABLE=true
$ export ROS_SECURITY_STRATEGY=Enforce
```

##### Windows

``` console
$ set ROS_SECURITY_KEYSTORE=%cd%/demo_keystore
$ set ROS_SECURITY_ENABLE=true
$ set ROS_SECURITY_STRATEGY=Enforce
```

这些变量需要在用于演示的每个终端中定义。 为了方便, 您可以将它们添加到您的启动环境中 。

<span id="run-the-talker-listener-demo"></span>

### 5) 运行 `talker/listener` 演示

启动演讲者节点开始演示。

``` console
$ ros2 run demo_nodes_cpp talker --ros-args --enclave /talker_listener/talker
```

在另一个终端, 做同样的启动 `listener` 节点。 此终端中的环境变量必须如上面第4步所描述的那样被正确设定 。

``` console
$ ros2 run demo_nodes_py listener --ros-args --enclave /talker_listener/listener
```

这些节点将使用认证和加密进行通信 ! 如果您查看了数据包内容( 例如, 使用 `tcpdump` 或 时 间 `Wireshark` 中,您可以看到信件是加密的。

注意: 您可以任意在 C++ (demo_nodes_cpp) 和 Python (demo_nodes_py) 套件之间切换.

这些节点能够通信,因为我们为他们创建了适当的密钥和证书.

两个节点随你用时运行 `ros2cli` 并回答下面的问题。

<span id="use-ros2cli-with-security"></span>

### 6) 使用 `ros2cli` 有安全保障的

要使用 `ros2cli` 使用 ROS 2 安全网络, 您需要为它提供覆盖飞地 `ROS_SECURITY_ENCLAVE_OVERRIDE` 环境变量。打开另一个终端并设置以下环境变量。

##### Linux

``` console
$ export ROS_SECURITY_KEYSTORE=~/sros2_demo/demo_keystore
$ export ROS_SECURITY_ENABLE=true
$ export ROS_SECURITY_STRATEGY=Enforce
$ export ROS_SECURITY_ENCLAVE_OVERRIDE=/talker_listener/listener
```

##### 麦克OS

``` console
$ export ROS_SECURITY_KEYSTORE=~/sros2_demo/demo_keystore
$ export ROS_SECURITY_ENABLE=true
$ export ROS_SECURITY_STRATEGY=Enforce
$ export ROS_SECURITY_ENCLAVE_OVERRIDE=/talker_listener/listener
```

##### Windows

``` console
$ set ROS_SECURITY_KEYSTORE=%cd%/demo_keystore
$ set ROS_SECURITY_ENABLE=true
$ set ROS_SECURITY_STRATEGY=Enforce
$ set ROS_SECURITY_ENCLAVE_OVERRIDE=/talker_listener/listener
```

现在你可以使用 `ros2cli` 与ROS 2安全网络通信。

``` console
$ ros2 node list --no-daemon --spin-time 3
[INFO] [1733862009.410918416] [rcl]: Found security directory: /root/ros2_ws/colcon_ws/demo_keystore/enclaves/talker_listener/talker
/listener
/talker
```

``` console
$ ros2 topic list --no-daemon --spin-time 3
[INFO] [1733861998.562163611] [rcl]: Found security directory: /root/ros2_ws/colcon_ws/demo_keystore/enclaves/talker_listener/talker
/chatter
/parameter_events
/rosout
```

> **说明**
>
> 避免使用ros2守护进程,因为它可能没有安全飞地,并且应当给在ROS 2安全网络中发现的时间留出足够的时间.

<span id="take-the-quiz"></span>

## 带上查兹!

##### 问题1

打开另一个终端会话, 但是 **不要这样** 设置环境变量, 以便不启用安全。 启动听众。 您期望发生什么 ?

##### 答 题 1

听者启动但没有收到任何消息。所有流量都是加密的,没有安全性,听者不会收到任何信息。

##### 问题2

停止收听器, 设置环境变量 `ROS_SECURITY_ENABLE` 改为: `true` 重新开始收听器,这次你期望得到什么结果?

##### 答 问 2

听者仍然在发射,但没有收到消息。虽然安全已经启用,但是由于ROS无法定位关键文件,所以它没有被正确配置。听者发射,但是由于安全性没有执行,所以处于非安全状态,这意味着虽然正确配置的谈话者发送加密消息,但这个听者无法解密它们。

##### 问题3

停止收听器并设定 `ROS_SECURITY_STRATEGY` 改为: `Enforce`现在怎么办?

##### 答 问 3

收听器无法启动。 安全已经启用, 并且正在强制实施 。 由于它仍然没有适当的配置, 错误会被抛出, 而不是以非安全模式发射 。

<span id="learn-more"></span>

## 学着点!

你准备好和ROS安全局合作了吗? [安全龟机器人2 Demo](https://github.com/ros-swg/turtlebot3_demo). 你会发现ROS 2 的安全性能和复杂的执行, 准备尝试你自己的定制方案。 请在这里创建拉动请求和问题, 以便我们继续改善ROS的安全支持 。
