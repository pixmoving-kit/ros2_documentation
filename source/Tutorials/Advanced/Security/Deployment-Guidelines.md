<span id="deployment-guidelines"></span>
<span id="background"></span>
<span id="prerequisites"></span>
<span id="general-guidelines"></span>
<span id="building-a-deployment-scenario"></span>
<span id="generating-the-docker-image"></span>
<span id="understanding-the-compose-file"></span>
<span id="running-the-example"></span>
<span id="examining-the-containers"></span>

# 部署指南

**目标：** 了解在生产系统中部署安全相关文件的最佳实践。

**教程级别：** 高级

**耗时：** 20 分钟

## 背景

典型部署场景通常需要将容器化应用或软件包分发到远程系统。部署启用安全功能的应用时，需要特别注意打包文件的敏感程度。

`sros2` 软件包遵循 [DDS Security 标准](https://www.omg.org/spec/DDS-SECURITY/1.1/About-DDS-SECURITY/)，提供一组高度模块化、灵活的工具，用于管理 ROS 2 环境中的安全功能。

遵循有关证书、密钥和目录组织方式的基本指导，是避免系统安全受到破坏的关键。这包括明确哪些内容需要保护，以及如何选择部署到远程生产系统的最小必要文件集，从而减少安全风险暴露。

## 前提条件

- 安装 Docker 及 Compose 插件。参阅 [Docker 安装](https://docs.docker.com/engine/install/)和 [Compose 插件](https://docs.docker.com/compose/install)的安装步骤。
- 建议具备 [ROS 2 安全设计](https://design.ros2.org/articles/ros2_dds_security.html)的基本知识。
- 建议完成前面的安全教程，特别是 [ROS 2 安全入门](Introducing-ros2-security.md)、[了解密钥库](The-Keystore.md)和[设置访问控制](Access-Controls.md)。

## 通用指导

ROS 2 使用 DDS Security 扩展，保障同一安全隔离域内的消息交换。隔离域中的签名文件和证书由受信任的[证书颁发机构（CA）](https://en.wikipedia.org/wiki/Certificate_authority)的私钥和证书生成。实际上，每个隔离域的身份认证和权限管理可以选用不同的 CA。这些 CA 文件保存在[密钥库](https://design.ros2.org/articles/ros2_security_enclaves.html)的 `private/` 和 `public/` 子目录中，目录结构如下：

```text
keystore
├── enclaves
│   └── ...
│       └── ...
├── private
│   └── ...
└── public
    └── ...
```

在典型生产系统部署中，创建和使用 CA 的良好实践是：

1. 在组织内部专用的系统中创建 CA。
2. 创建或修改所需的安全隔离域。注意，并非所有隔离域都应部署到所有目标设备；按应用分别建立隔离域，是实现职责分离的一种合理方式。
3. 初始化设备时，将 `public/` 和相应的 `enclaves/` 分发到不同的远程生产设备。
4. 将 `private/` 中的密钥和／或证书请求保留在组织内部并妥善保护。

如果丢失 `private/` 中的文件，就无法再更改访问权限、添加或修改安全配置。

此外，还可以考虑以下做法：

- 将 `enclaves/` 目录内容的权限设为只读。
- 如果生成隔离域私钥时提供了符合 PKCS#11 的 URI，可以使用[硬件安全模块（HSM）](https://en.wikipedia.org/wiki/Hardware_security_module)存储私钥。

下表归纳了密钥库各目录的推荐存放位置：

| 目录／位置 | 组织内部 | 目标设备 | 文件敏感程度 |
| --- | --- | --- | --- |
| public | ✓ | ✓ | 低 |
| private | ✓ | ✕ | 高 |
| enclaves | ✓ | ✓ | 中 |

## 构建部署场景

为了演示一个简单的部署场景，我们将在 `ros:<DISTRO>` 镜像的基础上构建新 Docker 镜像，并创建三个容器，用于：

- 在本地主机的共享卷中初始化密钥库。
- 模拟两台彼此安全通信的远程设备。

此例中，本地主机充当组织内部系统。首先创建工作目录：

```console
$ mkdir ~/security_gd_tutorial
$ cd ~/security_gd_tutorial
```

### 生成 Docker 镜像

构建新 Docker 镜像需要 Dockerfile。运行以下命令下载本教程使用的 Dockerfile：

```console
$ wget https://raw.githubusercontent.com/ros2/ros2_documentation/rolling/source/Tutorials/Advanced/Security/resources/deployment_gd/Dockerfile
```

然后构建镜像：

```console
$ docker build -t ros2_security/deployment_tutorial --build-arg ROS_DISTRO=rolling .
```

### 理解 Compose 文件

Compose 配置文件使用镜像创建作为服务运行的容器。本教程的配置定义了三个服务：

- *keystore-creator*：与前面的教程类似，在内部初始化密钥库目录树，创建 `enclaves/`、`public/` 和 `private/`。详情参阅 [ROS 2 安全隔离域](https://design.ros2.org/articles/ros2_security_enclaves.html)。`keystore` 目录配置为各容器之间的共享卷。
- *listener* 和 *talker*：模拟本教程中的远程设备。它们加载所需的安全环境变量，并从共享卷读取必要的密钥库文件。

下载 Compose YAML 配置文件：

```console
$ wget https://raw.githubusercontent.com/ros2/ros2_documentation/rolling/source/Tutorials/Advanced/Security/resources/deployment_gd/compose.deployment.yaml
```

## 运行示例

仍在 `~/security_gd_tutorial` 工作目录中，运行：

```console
$ docker compose -f compose.deployment.yaml up
```

应得到以下输出：

- *tutorial-listener-1*：`Found security directory: /keystore/enclaves/talker_listener/listener`
- *tutorial-talker-1*：`Found security directory: /keystore/enclaves/talker_listener/talker`
- *tutorial-listener-1*：`Publishing: 'Hello World: <number>'`
- *tutorial-talker-1*：`I heard: [Hello World: <number>]`

### 检查容器

保持模拟两台远程设备的容器运行，打开两个终端，分别进入容器。在第一个终端运行：

```console
$ docker exec -it tutorial-listener-1 bash
$ cd keystore
$ tree
```

在第二个终端运行：

```console
$ docker exec -it tutorial-talker-1 bash
$ cd keystore
$ tree
```

应得到类似以下的输出：

```bash
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

注意：

- `private/` 目录没有被复制到设备上，而是保留在本地主机（组织内部）。
- 每台已部署的设备仅包含自身应用所需的最小隔离域。

!!! note "说明"

    为简化演示，此隔离域使用同一个 CA 进行身份认证和权限管理。
