<span id="understanding-the-security-keystore"></span>
<span id="the-keystore"></span>
<span id="background"></span>
<span id="security-artifact-locations"></span>
<span id="public-key-materials"></span>
<span id="private-key-materials"></span>
<span id="domain-governance-policy"></span>
<span id="security-enclaves"></span>
<span id="take-the-quiz"></span>

# 了解安全密钥库

**目标：** 探索 ROS 2 安全密钥库中的文件。

**教程级别：** 高级

**耗时：** 15 分钟

## 背景

`sros2` 软件包可用于创建启用 ROS 2 安全功能所需的密钥、证书和策略。安全配置非常灵活。了解 ROS 2 安全密钥库的基本结构，有助于将其集成到现有的公钥基础设施（PKI），并按照组织的规定管理敏感密钥资料。

## 安全文件的位置

上一教程已启用通信安全，现在查看启用安全功能时创建的文件。正是这些文件让加密成为可能。

`sros2` 工具（`ros2 security ...`）将文件分为公开资料、私密资料和隔离域密钥资料。

ROS 使用环境变量 `ROS_SECURITY_KEYSTORE` 指定的目录作为密钥库。本教程使用 `~/sros2_demo/demo_keystore`。

### 公开密钥资料

在 `~/sros2_demo/demo_keys/public` 目录中有三个加密证书，但身份认证证书和权限证书实际上只是指向证书颁发机构（CA）证书的链接。

在公钥基础设施中，[证书颁发机构](https://en.wikipedia.org/wiki/Certificate_authority)充当信任锚，验证参与者的身份和权限。对于 ROS，这些参与者就是 ROS 图中的所有节点，范围可能覆盖整个机器人群。将 CA 证书 `ca.cert.pem` 放在机器人上的适当位置后，所有 ROS 节点就能与使用同一 CA 的其他节点建立相互信任。

虽然教程中即时创建 CA，但生产系统应按照预先制定的安全方案执行此操作。通常，生产系统的 CA 会离线创建，并在机器人初始化时放到机器人上。每台机器人可以使用独立 CA，也可以由一组需要彼此信任的机器人共享同一个 CA。

DDS（以及基于 DDS 的 ROS）支持将身份和权限的信任链分离，为两种功能分别设置 CA。多数 ROS 系统的安全方案不要求将两项职责分离，因此安全工具只生成一个同时用于身份认证和权限管理的 CA。

使用 `openssl` 以文本方式查看这个 X.509 证书：

```console
$ cd ~/sros2_demo/demo_keys/public
$ openssl x509 -in ca.cert.pem -text -noout
```

输出应类似如下：

```text
Certificate:
  Data:
      Version: 3 (0x2)
      Serial Number:
          02:8e:9a:24:ea:10:55:cb:e6:ea:e8:7a:c0:5f:58:6d:37:42:78:aa
      Signature Algorithm: ecdsa-with-SHA256
      Issuer: CN = sros2CA
      Validity
          Not Before: Jun  1 16:57:37 2021 GMT
          Not After : May 31 16:57:37 2031 GMT
      Subject: CN = sros2CA
      Subject Public Key Info:
          Public Key Algorithm: id-ecPublicKey
              Public-Key: (256 bit)
              pub:
                  04:71:e9:37:d7:32:ba:b8:a0:97:66:da:9f:e3:c4:
                  08:4f:7a:13:59:24:c6:cf:6a:f7:95:c5:cd:82:c0:
                  7f:7f:e3:90:dd:7b:0f:77:d1:ee:0e:af:68:7c:76:
                  a9:ca:60:d7:1e:2c:01:d7:bc:7e:e3:86:2a:9f:38:
                  dc:ed:39:c5:32
              ASN1 OID: prime256v1
              NIST CURVE: P-256
      X509v3 extensions:
          X509v3 Basic Constraints: critical
              CA:TRUE, pathlen:1
  Signature Algorithm: ecdsa-with-SHA256
       30:45:02:21:00:d4:fc:d8:45:ff:a4:51:49:98:4c:f0:c4:3f:
       e0:e7:33:19:8e:31:3c:d0:43:e7:e9:8f:36:f0:90:18:ed:d7:
       7d:02:20:30:84:f7:04:33:87:bb:4f:d3:8b:95:61:48:df:83:
       4b:e5:92:b3:e6:ee:3c:d5:cf:30:43:09:04:71:bd:dd:7c
```

关于此 CA 证书，需要注意：

- 证书主体名称 `sros2CA` 是 `sros2` 工具提供的默认值。
- 证书自创建起有效期为十年。
- 与所有证书一样，它包含用于公私钥加密的公钥。
- 作为根 CA，它是[自签名证书](https://en.wikipedia.org/wiki/Self-signed_certificate)，即使用自己的私钥签名。

这是公开证书，可以根据需要自由复制，以便在整个 ROS 系统中建立信任。

### 私密密钥资料

私密密钥资料位于 `~/sros2_demo/demo_keys/private`。与 `public` 目录类似，这里包含一个 CA 密钥 `ca.key.pem`，以及指向它的符号链接，分别用于身份 CA 和权限 CA 的私钥。

!!! warning "警告"

    务必保护此私钥，并为它创建安全备份！

此私钥对应于公开 CA，而该 CA 是 ROS 系统整体安全的信任锚。修改 ROS 图的加密策略和添加新的 ROS 参与者时都会用到它。根据机器人的安全要求，可以用访问权限保护该密钥，将它限制为其他账户可访问，也可以完全移出机器人，存储在其他系统或设备上。丢失此文件后，就无法更改访问权限或向系统添加新参与者。同样，任何能够访问此文件的用户或进程，都有能力修改系统策略和参与者。

只有配置机器人时才需要此文件，运行机器人并不需要它。因此，可以放心地将其离线保存在其他系统或可移动介质上。

`sros2` 工具使用[椭圆曲线密码学](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography)而非 RSA，以提高安全性并缩小密钥尺寸。使用以下命令查看椭圆曲线私钥的详细信息：

```console
$ cd ~/sros2_demo/demo_keys/private
$ openssl ec -in ca.key.pem -text -noout
read EC key
Private-Key: (256 bit)
priv:
    93:da:76:b9:e3:91:ab:e9:42:76:f2:38:f1:9d:94:
    90:5e:b5:96:7b:7f:71:ee:13:1b:d4:a0:f9:48:fb:
    ae:77
pub:
    04:71:e9:37:d7:32:ba:b8:a0:97:66:da:9f:e3:c4:
    08:4f:7a:13:59:24:c6:cf:6a:f7:95:c5:cd:82:c0:
    7f:7f:e3:90:dd:7b:0f:77:d1:ee:0e:af:68:7c:76:
    a9:ca:60:d7:1e:2c:01:d7:bc:7e:e3:86:2a:9f:38:
    dc:ed:39:c5:32
ASN1 OID: prime256v1
NIST CURVE: P-256
```

除了私钥本身，输出还列出了公钥；它与 CA 证书 `ca.cert.pem` 中的公钥相同。

### 域治理策略

域治理策略位于密钥库的隔离域目录 `~/sros2_demo/demo_keys/enclaves`。`enclave` 目录包含 XML 治理策略文档 `governance.xml`，以及经过权限 CA 签名的副本 `governance.p7s`。

`governance.p7s` 包含作用于整个域的设置，例如如何处理未经认证的参与者、是否加密发现过程，以及话题访问的默认规则。

用以下命令验证治理文件的 [S/MIME 签名](https://en.wikipedia.org/wiki/S/MIME)：

```console
$ openssl smime -verify -in governance.p7s -CAfile ../public/permissions_ca.cert.pem
```

该命令会输出 XML 文档，最后一行是 `Verification successful`，表示文档已由权限 CA 正确签名。

### 安全隔离域

安全进程（通常是 ROS 节点）在安全隔离域中运行。最简单的情况是把所有进程放入同一个隔离域，它们就会使用同一安全策略。要为不同进程应用不同策略，可以在启动时指定不同的隔离域。详情参阅[设计文档](https://design.ros2.org/articles/ros2_security_enclaves.html)。运行节点时，使用 ROS 参数 `--enclave` 指定安全隔离域。

**每个安全隔离域都需要六个文件**才能启用安全功能。每个文件**必须**按照下面的名称命名，相关要求见 [DDS Security 标准](https://www.omg.org/spec/DDS-SECURITY/1.1/About-DDS-SECURITY/)。为了避免重复保存相同文件，`sros2` 为各隔离域创建链接，指向上述唯一的治理策略、身份 CA 和权限 CA。

查看 `listener` 隔离域中的以下六个文件。其中三个专用于此隔离域，另外三个是 ROS 系统共用的文件：

- `key.pem`：此隔离域用于加密和解密的私钥。
- `cert.pem`：此隔离域的公开证书，由身份 CA 签名。
- `permissions.p7s`：此隔离域的权限文件，由权限 CA 签名。
- `governance.p7s`：指向此域已签名安全策略文件的链接。
- `identity_ca.cert.pem`：指向此域身份 CA 的链接。
- `permissions_ca.cert.pem`：指向此域权限 CA 的链接。

私有加密密钥 `key.pem` 应按安全方案妥善保护。它用于对此隔离域内的通信进行加密、解密和验证。如果密钥丢失或被盗，应撤销密钥，并为此隔离域创建新身份。

目录中还创建了 `permissions.xml`，可用于重新生成已签名的权限文件。但启用安全功能不需要此文件，因为 DDS 使用的是其已签名版本。

## 测一测

试着回答以下关于 ROS 安全密钥库的问题。打开新终端，使用上一教程创建的密钥库启用安全功能：

```console
$ export ROS_SECURITY_KEYSTORE=~/sros2_demo/demo_keystore
$ export ROS_SECURITY_ENABLE=true
$ export ROS_SECURITY_STRATEGY=Enforce

$ cd ~/sros2_demo/demo_keys/enclaves/talker_listener/listener
```

开始前先备份 `permissions.p7s`。

### 问题 1

用文本编辑器打开 `permissions.p7s`，对 XML 内容做一个微小改动（例如添加空格或空行），然后保存。启动 listener：

```console
$ ros2 run demo_nodes_cpp listener --ros-args --enclave /talker_listener/listener
```

你预期会发生什么？能否启动 talker 节点？

```console
$ ros2 run demo_nodes_cpp talker --ros-args --enclave /talker_listener/talker
```

启动 listener 和启动 talker 有何不同？

### 答案 1

listener 无法启动，并会报错。只要修改了 `permissions.p7s`，无论改动多么微小，文件签名都会失效。启用并强制执行安全功能时，如果权限文件无效，节点就不会启动。

talker 会正常启动。它使用另一个隔离域中的 `permissions.p7s`，该文件仍然有效。

### 问题 2

可以用什么命令检查修改后的 `permissions.p7s` 签名是否有效？

### 答案 2

使用 `openssl smime` 检查 `permissions.p7s` 是否由权限 CA 正确签名：

```console
$ openssl smime -verify -in permissions.p7s -CAfile permissions_ca.cert.pem
```

进入下一教程前，恢复原来已正确签名的 `permissions.p7s` 文件。
