---
translation_status: machine_translated
source: Tutorials/Advanced/Security/Deployment-Guidelines.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="deployment-guidelines"></span>

# 部署指南

**目标：** 了解在生产系统中部署安全文物的最佳做法。

**教程级别：** 高级

**用时：** 20分钟

<span id="background"></span>

## 背景

典型的部署方案往往涉及将集装箱化的应用程序或包件运入远程系统,在部署安全功能应用程序时应特别注意,要求用户对包件文件的敏感性提出理由。

遵守 [DDS 安全标准](https://www.omg.org/spec/DDS-SECURITY/1.1/About-DDS-SECURITY/),则 `sros2` 成套软件以模块化和灵活化的方式,为在ROS 2环境下管理安保提供一套公用事业。

关于如何组织不同的证书、钥匙和目录的基本核心准则仍然是避免损害系统安全的关键因素,其中包括保护意识和选择在远程生产系统上部署的最低限度必要文件的标准,以尽量减少安全风险。

<span id="prerequisites"></span>

## 前提条件

- 带有编曲插件的嵌入器安装 。 请参考详细可见的安装步骤 。 [嵌入器安装](https://docs.docker.com/engine/install/) 财务报告和财务报告 [编译插件](https://docs.docker.com/compose/install).

- (建议) [ROS 2 安保设计](https://design.ros2.org/articles/ros2_dds_security.html).

- (建议)以前的安全辅导完成情况。特别是:

  > - [配置安全机制](Introducing-ros2-security.md)
  >
  > - [了解安全密钥库](The-Keystore.md)
  >
  > - [设置访问控制](Access-Controls.md)

<span id="general-guidelines"></span>

## 一般准则

ROS 2 利用 DDS 安全扩展来保证同一飞地内信件交换的安全。飞地内不同的签名文件和证书来自一个飞地的私人密钥和证书 [证书管理权限( CA)](https://en.wikipedia.org/wiki/Certificate_authority) 信任的实体。事实上,每个飞地可以选择两个不同的 CA 身份和权限。这些 CA 的文物存放在内部 。 `private/` 财务报告和财务报告 `public/` a 的子目录 [键盘](https://design.ros2.org/articles/ros2_security_enclaves.html) 带有以下文件夹结构:

``` text
keystore
├── enclaves
│   └── ...
│       └── ...
├── private
│   └── ...
└── public
    └── ...
```

在生产系统的典型部署方面,设立和使用某一证书管理局的一个良好做法是:

1.  在组织系统内创建,仅供内部使用。

2.  生成/修改所希望的飞地,同时铭记:

    > - 并非所有生成的飞地都应部署在所有目标装置上.
    >
    > - 合理的处理方式是每份申请有一个飞地,从而能够区分各种关切。

3.  船舶 `public/` 与相应的 `enclaves/` 在设置时进入不同的远程生产设备。

4.  维护和保护 `private/` 组织中的密钥和/或认证请求。

必须指出,如果 `private/` 文件丢失, 无法再更改访问权限、 添加或修改安全配置 。

此外,还可考虑下列其他做法:

- 给予只读权限 `enclaves/` 目录内容。

- 如果为生成飞地的私人密钥提供了符合PKCS#11的URI,则 a [硬件安全模块(HSM)](https://en.wikipedia.org/wiki/Hardware_security_module) 可以用来储存它们。

下表概述了以往与Keystore目录相关的声明,并标明了建议的位置:

| 目录/ 位置 | 组织 | 目标设备 | 材料敏感性 |
|------------|------|----------|------------|
| 公开       | ✓    | ✓        | 低级       |
| 私营       | ✓    | ✕        | 高级       |
| 飞地       | ✓    | ✓        | 中型       |

<span id="building-a-deployment-scenario"></span>

## B. 建立部署设想

为了说明一个简单的部署方案,将在由下列人员提供的插头图像之上建立一个新的插头图像: `ros:<DISTRO>`从图像开始,将创建三个容器,目的是:

- 在本地主机共享的音量中初始化密钥托 。

- 模拟两个部署的远程设备,它们以安全的方式相互相互作用。

在这个例子中,当地东道主充当该组织的系统。让我们首先创建一个工作空间文件夹:

``` console
$ mkdir ~/security_gd_tutorial
$ cd ~/security_gd_tutorial
```

<span id="generating-the-docker-image"></span>

### 生成 Docker 图像

要构建一个新的嵌入器图像, 需要一个嵌入器文件。 要下载为此教程提议的嵌入器文件, 请运行 :

``` console
$ wget https://raw.githubusercontent.com/ros2/ros2_documentation/rolling/source/Tutorials/Advanced/Security/resources/deployment_gd/Dockerfile
```

现在,用命令构建嵌入器图像 :

``` console
$ docker build -t ros2_security/deployment_tutorial --build-arg ROS_DISTRO=rolling .
```

<span id="understanding-the-compose-file"></span>

### 理解编曲文件

编曲配置文件需要一个图像来创建容器作为服务。在这个教程中,在配置中定义了三个服务:

- *密钥生成器*: 类似于以前的教程, 它在内部初始化一个新的密钥托尔树目录。 这将创建 *enclaves/* *public/* 财务报告和财务报告 *private/*,在下文中对此作了更详细的解释。 [ROS 2 安全飞地](https://design.ros2.org/articles/ros2_security_enclaves.html)。该词 `keystore` 目录被配置为跨容器共享的卷。

- *监听器* 财务报告和财务报告 *说话者*: 担任此教程中的远程设备角色。 需要 `Security` 环境变量来自共享的音量以及必要的密钥文件。

编曲配置 Yaml 文件可以下载到 :

``` console
$ wget https://raw.githubusercontent.com/ros2/ros2_documentation/rolling/source/Tutorials/Advanced/Security/resources/deployment_gd/compose.deployment.yaml
```

<span id="running-the-example"></span>

## 运行示例

在同一工作目录中 `~/security_gd_tutorial`,以开始示例运行 :

``` console
$ docker compose -f compose.deployment.yaml up
```

这将产生以下产出:

- *教程收听器 - 1*: `Found security directory: /keystore/enclaves/talker_listener/listener`

- *导读器 - 1*: `Found security directory: /keystore/enclaves/talker_listener/talker`

- *教程收听器 - 1*: `Publishing: 'Hello World: <number>'`

- *导读器 - 1*: `I heard: [Hello World: <number>]`

<span id="examining-the-containers"></span>

### 检查集装箱

在运行模拟此教程的两个远程设备的容器时, 请通过打开两个不同的终端来连接每个设备。 在第一个终端中, 运行 :

``` console
$ docker exec -it tutorial-listener-1 bash
$ cd keystore
$ tree
```

在第二航站楼,运行:

``` console
$ docker exec -it tutorial-talker-1 bash
$ cd keystore
$ tree
```

应获得与下文所述产出类似的产出:

``` bash
# Terminal 1
keystore
 ├── enclaves
 │   ├── governance.p7s
 │   ├── governance.xml
 │   └── talker_listener
 │       └── listener
 │           ├── cert.pem
 │           ├── governance.p7s
 │           ├── identity_ca.cert.pem
 │           ├── key.pem
 │           ├── permissions_ca.cert.pem
 │           ├── permissions.p7s
 │           └── permissions.xml
 └── public
     ├── ca.cert.pem
     ├── identity_ca.cert.pem
     └── permissions_ca.cert.pem

# Terminal 2
keystore
 ├── enclaves
 │   ├── governance.p7s
 │   ├── governance.xml
 │   └── talker_listener
 │       └── talker
 │           ├── cert.pem
 │           ├── governance.p7s
 │           ├── identity_ca.cert.pem
 │           ├── key.pem
 │           ├── permissions_ca.cert.pem
 │           ├── permissions.p7s
 │           └── permissions.xml
 └── public
     ├── ca.cert.pem
     ├── identity_ca.cert.pem
     └── permissions_ca.cert.pem
```

注意:

- *private/* 文件夹不移动,而是留在本地主机(组织)中。

- 所部署的装置中的每一装置都装有其应用所需的最低飞地。

> **说明**
>
> 为了简单起见,同一CA在这个飞地内用于身份和权限.
