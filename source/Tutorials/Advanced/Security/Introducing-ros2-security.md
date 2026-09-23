<span id="setting-up-security"></span>
<span id="ros-2-security-tutorials"></span>
<span id="sros2"></span>
<span id="background"></span>
<span id="installation"></span>
<span id="installing-from-source"></span>
<span id="selecting-an-alternate-middleware"></span>
<span id="run-the-demo"></span>
<span id="create-a-folder-for-the-security-files"></span>
<span id="generate-a-keystore"></span>
<span id="generate-keys-and-certificates"></span>
<span id="configure-environment-variables"></span>
<span id="run-the-talker-listener-demo"></span>
<span id="use-ros2cli-with-security"></span>
<span id="take-the-quiz"></span>
<span id="learn-more"></span>

# 设置安全功能

**目标：** 使用 `sros2` 设置安全功能。

**教程级别：** 高级

**耗时：** 15 分钟

## 背景

`sros2` 软件包提供在 DDS-Security 基础上使用 ROS 2 所需的工具和说明。安全功能已经过跨平台（Linux、macOS、Windows）和跨语言（C++、Python）测试。SROS2 的设计支持任何具备安全功能的中间件，但并非所有中间件都开源，支持情况也取决于所用的 ROS 发行版。

## 安装

通常按照 [ROS 2 安装指南](../../../Installation.md)和[配置指南](../../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)完成安装后，即可使用安全功能。如果打算从源码安装或切换中间件实现，请注意以下事项。

### 从源码安装

从源码安装前，需要安装较新的 OpenSSL（1.0.2g 或更高版本）。

#### Linux

```console
$ sudo apt update
$ sudo apt install libssl-dev
```

#### macOS

```console
$ brew install openssl
```

运行 DDS-Security 演示时，OpenSSL 必须位于库搜索路径中。运行以下命令，并考虑将它们添加到 `~/.bash_profile`：

```console
$ export DYLD_LIBRARY_PATH=`brew --prefix openssl`/lib:$DYLD_LIBRARY_PATH
$ export OPENSSL_ROOT_DIR=`brew --prefix openssl`
```

#### Windows

如果尚未安装 OpenSSL，请按照[安装前提条件](../../../Installation/Windows-Install-Binary.md#windows-install-binary-installing-prerequisites)中的说明操作。

Fast DDS 构建安全插件需要额外的 CMake 选项，因此必须调整 colcon 命令以传入该选项：

```console
$ colcon build --symlink-install --cmake-args -DSECURITY=ON --packages-select fastrtps rmw_fastrtps_cpp rmw_fastrtps_dynamic_cpp rmw_fastrtps_shared_cpp
```

### 选择其他中间件

如果不使用默认中间件实现，请先[更改 RMW 实现](../../../Installation/RMW-Implementations.md)，再继续。

ROS 2 允许在运行时切换 RMW 实现。有关尝试不同中间件的说明，请参阅[使用多个 RMW 实现](../../../How-To-Guides/Working-with-multiple-RMW-implementations.md)。

注意，不支持不同供应商中间件之间的安全通信。

## 运行演示

### 1 创建安全文件目录

首先创建一个目录，保存本演示需要的全部文件。

#### Linux

```console
$ mkdir ~/sros2_demo
```

#### macOS

```console
$ mkdir ~/sros2_demo
```

#### Windows

```console
$ md C:\dev\ros2\sros2_demo
```

### 2 生成密钥库

使用 `sros2` 工具创建密钥库。密钥库中的文件用于为 ROS 2 图中的所有参与者启用安全功能。

#### Linux

```console
$ cd ~/sros2_demo
$ ros2 security create_keystore demo_keystore
```

#### macOS

```console
$ cd ~/sros2_demo
$ ros2 security create_keystore demo_keystore
```

#### Windows

```console
$ cd sros2_demo
$ ros2 security create_keystore demo_keystore
```

### 3 生成密钥和证书

创建密钥库后，为每个需要启用安全功能的节点创建密钥和证书。本演示包括 talker 和 listener 节点。这里使用 `create_enclave` 功能，下一教程将详细介绍。

#### Linux

```console
$ ros2 security create_enclave demo_keystore /talker_listener/talker
$ ros2 security create_enclave demo_keystore /talker_listener/listener
```

#### macOS

```console
$ ros2 security create_enclave demo_keystore /talker_listener/talker
$ ros2 security create_enclave demo_keystore /talker_listener/listener
```

#### Windows

```console
$ ros2 security create_enclave demo_keystore /talker_listener/talker
$ ros2 security create_enclave demo_keystore /talker_listener/listener
```

在 Windows 上，如果出现 `unable to write 'random state'`，请设置环境变量 `RANDFILE`：

```console
$ set RANDFILE=C:\dev\ros2\sros2_demo\.rnd
```

然后重新运行上述命令。

### 4 配置环境变量

三个环境变量让中间件能够找到加密资料并启用（以及按需强制执行）安全功能。这些变量及其他安全相关环境变量的说明，见 [ROS 2 DDS-Security 集成设计文档](https://design.ros2.org/articles/ros2_dds_security.html)。

#### Linux

```console
$ export ROS_SECURITY_KEYSTORE=~/sros2_demo/demo_keystore
$ export ROS_SECURITY_ENABLE=true
$ export ROS_SECURITY_STRATEGY=Enforce
```

#### macOS

```console
$ export ROS_SECURITY_KEYSTORE=~/sros2_demo/demo_keystore
$ export ROS_SECURITY_ENABLE=true
$ export ROS_SECURITY_STRATEGY=Enforce
```

#### Windows

```console
$ set ROS_SECURITY_KEYSTORE=%cd%/demo_keystore
$ set ROS_SECURITY_ENABLE=true
$ set ROS_SECURITY_STRATEGY=Enforce
```

每个用于演示的终端都需要定义这些变量。为方便起见，可以把它们加入启动环境。

### 5 运行 `talker/listener` 演示

首先启动 talker 节点：

```console
$ ros2 run demo_nodes_cpp talker --ros-args --enclave /talker_listener/talker
```

在另一个终端中，以相同方式启动 `listener` 节点。该终端必须按照步骤 4 正确设置环境变量。

```console
$ ros2 run demo_nodes_py listener --ros-args --enclave /talker_listener/listener
```

现在，这些节点通过身份认证和加密进行通信！查看数据包内容（例如使用后续教程介绍的 `tcpdump` 或 `Wireshark`）即可看到消息已被加密。

注意，可以任意切换使用 C++ 软件包 `demo_nodes_cpp` 和 Python 软件包 `demo_nodes_py`。

这些节点能够通信，是因为我们为它们创建了合适的密钥和证书。

保持两个节点运行，继续使用 `ros2cli` 并回答下面的问题。

### 6 在启用安全功能时使用 `ros2cli`

要使用 `ros2cli` 与受保护的 ROS 2 网络交互，需要通过环境变量 `ROS_SECURITY_ENCLAVE_OVERRIDE` 指定覆盖使用的隔离域。打开另一个终端，设置以下环境变量：

#### Linux

```console
$ export ROS_SECURITY_KEYSTORE=~/sros2_demo/demo_keystore
$ export ROS_SECURITY_ENABLE=true
$ export ROS_SECURITY_STRATEGY=Enforce
$ export ROS_SECURITY_ENCLAVE_OVERRIDE=/talker_listener/listener
```

#### macOS

```console
$ export ROS_SECURITY_KEYSTORE=~/sros2_demo/demo_keystore
$ export ROS_SECURITY_ENABLE=true
$ export ROS_SECURITY_STRATEGY=Enforce
$ export ROS_SECURITY_ENCLAVE_OVERRIDE=/talker_listener/listener
```

#### Windows

```console
$ set ROS_SECURITY_KEYSTORE=%cd%/demo_keystore
$ set ROS_SECURITY_ENABLE=true
$ set ROS_SECURITY_STRATEGY=Enforce
$ set ROS_SECURITY_ENCLAVE_OVERRIDE=/talker_listener/listener
```

现在即可使用 `ros2cli` 与受保护的 ROS 2 网络通信：

```console
$ ros2 node list --no-daemon --spin-time 3
[INFO] [1733862009.410918416] [rcl]: Found security directory: /root/ros2_ws/colcon_ws/demo_keystore/enclaves/talker_listener/talker
/listener
/talker
```

```console
$ ros2 topic list --no-daemon --spin-time 3
[INFO] [1733861998.562163611] [rcl]: Found security directory: /root/ros2_ws/colcon_ws/demo_keystore/enclaves/talker_listener/talker
/chatter
/parameter_events
/rosout
```

!!! note "说明"

    避免使用 ros2 daemon，因为它可能没有配置安全隔离域；同时，应为受保护 ROS 2 网络中的发现过程留出足够时间。

## 测一测

### 问题 1

打开另一个终端，但**不要**设置环境变量，使安全功能保持关闭。启动 listener。你预期会发生什么？

### 答案 1

listener 能够启动，但收不到任何消息。所有流量都已加密，未启用安全功能的 listener 无法接收它们。

### 问题 2

停止 listener，将环境变量 `ROS_SECURITY_ENABLE` 设为 `true`，然后重新启动 listener。这次你预期会发生什么？

### 答案 2

listener 仍然能够启动，但收不到消息。虽然启用了安全功能，但配置不正确，ROS 无法找到密钥文件。由于没有强制执行安全策略，listener 会以非安全模式启动。因此，尽管正确配置的 talker 正在发送加密消息，此 listener 仍无法解密它们。

### 问题 3

停止 listener，将 `ROS_SECURITY_STRATEGY` 设为 `Enforce`。现在会发生什么？

### 答案 3

listener 无法启动。安全功能已启用且被强制执行，但配置仍不正确，因此会报错，而不是以非安全模式启动。

## 深入了解

准备进一步了解 ROS 安全功能了吗？请查看 [Secure Turtlebot2 演示](https://github.com/ros-swg/turtlebot3_demo)。其中提供了一个可运行且较复杂的 ROS 2 安全实现，可用于尝试你自己的场景。欢迎在该仓库提交拉取请求和问题，帮助持续改进 ROS 的安全支持！
