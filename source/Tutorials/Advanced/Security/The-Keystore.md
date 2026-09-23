---
translation_status: machine_translated
source: Tutorials/Advanced/Security/The-Keystore.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="understanding-the-security-keystore"></span> <span id="the-keystore"></span>

# 了解安全密钥库

**目标：** 探索位于ROS 2 安全密钥库中的文件.

**教程级别：** 高级

**用时：** 15分钟

<span id="background"></span>

## 背景

那个... `sros2` 软件包可以用来创建 ROS 2 安全所必需的密钥、证书和政策。但是,安全配置非常灵活。对 ROS 2 安全密钥库的基本理解将允许与现有的公钥基础设施(PKI)进行整合,并管理符合组织政策的敏感关键材料。

<span id="security-artifact-locations"></span>

## 安全器械地点

在前一个教程中允许通信安全后, 让我们看看安全启用时创建的文件。 这些是允许加密的文件 。

那个... `sros2` 水电费(`ros2 security ...`将档案分为公共、私人和飞地关键材料。

ROS 使用环境变量定义的目录 `ROS_SECURITY_KEYSTORE` 作为密钥。 对于此教程, 我们使用目录 `~/sros2_demo/demo_keystore`.

<span id="public-key-materials"></span>

### 公钥材料

您可以在公共目录中找到三个加密证书 。 `~/sros2_demo/demo_keys/public`但是,身份和权限证书实际上只是证书管理局(CA)证书的链接.

在公用钥匙基础设施中, [证书权限](https://en.wikipedia.org/wiki/Certificate_authority) 作为信任锚:它验证参与者的身份和权限。对于ROS来说,这意味着所有参与ROS图的节点(可能延伸到整个单个机器人车队)。`ca.cert.pem`)在机器人上的适当位置上,所有ROS节点都可以使用同一证书管理局与其他节点建立相互信任.

尽管我们在教程中创建了飞行证书管理局,但在生产系统中,这应该按照预先设定的安全计划来完成。通常情况下,生产系统的证书管理局会被脱机创建,并在初始设置时放置在机器人上。它可能对于每个机器人都是独一无二的,或者在一组机器人之间共享,这些机器人都是为了互相信任。

DDS(和ROS,通过扩展)支持身份和许可信任链的分离,因此每个函数都有自己的证书权限. 在大多数情况下,ROS系统的安全计划不需要这些职责的分离,因此安全公用设施生成一个用于身份和权限的单一证书管理局.

使用 `openssl` 以文本显示此 x509 证书:

``` console
$ cd ~/sros2_demo/demo_keys/public
$ openssl x509 -in ca.cert.pem -text -noout
```

产出应该与下列产出相似:

``` default
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

有关此 CA 证书的一些事宜 :  
- 证书主题名称 `sros2CA` 是由该选项提供的默认值 `sros2` 公用事业。

- 此证书自创建时起有效期为十年

- 与所有证书一样, 此证书包含用于公用密钥加密的公钥

- 作为根证书管理局,这是 [自签名证书](https://en.wikipedia.org/wiki/Self-signed_certificate);即它使用自己的私人密钥签名。

由于这是一个公开证书,它可以根据需要免费复制,以便在整个ROS系统中建立信任.

<span id="private-key-materials"></span>

### 私钥材料

密钥目录中可以找到私人关键材料 `~/sros2_demo/demo_keys/private`。与 `public` 目录, 此选项包含一个证书授权密钥 `ca.key.pem` 和符号链接,用作身份和权限 CA 私人密钥。

> **警告**
>
> 保护这把私钥 并建立一个安全的备份!

这是与公共证书管理局相关的私人密钥, 它充当了您 ROS 系统中所有安全的锚。 您将使用它来修改 ROS 图中的加密政策和添加新的 ROS 参与者。 根据您的机器人的安全需要, 该密钥可以使用访问权限加以保护, 并锁定到另一个账户, 或者它可以完全从机器人上移到另一个系统或设备上。 如果文件丢失, 您将无法更改访问权限并为系统添加新的参与者 。 同样, 任何访问文件的用户或进程都有能力修改系统政策和参与者 。

此文件仅用于配置机器人,而不需要机器人运行,可以安全地储存在另一个系统或可移动介质中.

那个... `sros2` 公用事业的使用 [椭圆曲线加密](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography) 而不是用于改进安全和缩小密钥大小的RSA。使用以下命令来显示此椭圆曲线的私人密钥的细节 :

``` console
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

除了私钥本身外,请注意公钥已列出,并与证书管理局所列的公钥相符 `ca.cert.pem`.

<span id="domain-governance-policy"></span>

### 领域治理政策

在密钥托尔的飞地目录中找到域治理政策, `~/sros2_demo/demo_keys/enclaves`。该词 `enclave` 目录包含 XML 治理政策文件 `governance.xml`,以及授权CA作为 `governance.p7s`.

那个... `governance.p7s` 文件包含全域设置,例如如何处理未经认证的参与者,是否加密发现,以及访问专题的默认规则。

使用以下命令验证 [S/MIME 签名](https://en.wikipedia.org/wiki/S/MIME) 治理文件:

``` console
$ openssl smime -verify -in governance.p7s -CAfile ../public/permissions_ca.cert.pem
```

此命令将打印出 XML 文档, 最后一行将是 `Verification successful` 以显示文档由权限 CA 正确签名。

<span id="security-enclaves"></span>

### 安全飞地

安全进程( 通常为ROS 节点) 运行在一个安全飞地内。 在最简单的情况下, 所有进程可以合并到同一个飞地内, 所有进程都会使用相同的安全政策。 然而, 为了对不同的进程应用不同的政策, 进程在启动时可以使用不同的安全飞地。 有关安全飞地的更多细节, 请参见 。 [设计文件](https://design.ros2.org/articles/ros2_security_enclaves.html)。使用 ROS 参数指定了安全飞地 `--enclave` 当运行一个节点时。

**每个安全飞地需要六个文件** 以启用安全性。每个文件 **必须** 如下文所述,并按下文概述的那样。 [DDS 安全标准](https://www.omg.org/spec/DDS-SECURITY/1.1/About-DDS-SECURITY/)。为了避免同一文件的多份副本, `sros2` 公用事业为每个飞地与上述单一治理政策、身份CA和权限CA建立联系。

见以下6个文件: `listener` 三个是这个飞地特有的,而三个是这个ROS系统通用的:

> - `key.pem`,用于加密和解密此飞地的私钥
>
> - `cert.pem`,该飞地的公共证书;该证书由身份CA签署。
>
> - `permissions.p7s`,此飞地的权限; 此文件已经与权限 CA 签署
>
> - `governance.p7s`,链接到此域的已签名的安全政策文件
>
> - `identity_ca.cert.pem`,此域的身份CA的链接
>
> - `permissions_ca.cert.pem`,此域的许可 CA 链接

私密加密密钥 `key.pem` 此密钥将在此特定飞地内加密、解密和验证通信。如果密钥丢失或被盗,请撤销密钥,并为该飞地创建新身份。

文件 `permissions.xml` 已经在此目录中创建, 并可用于重新创建已签名的权限文件。 然而, 此文件不需要启用安全性, 因为 DDS 使用已签名的文件版本 。

<span id="take-the-quiz"></span>

## 参加测验吧!

请检查您能否回答关于 ROS 安全密钥的这些问题 。 从一个新的终端会话开始, 并启用上一个教程中创建的密钥密钥的安全性 :

``` console
$ export ROS_SECURITY_KEYSTORE=~/sros2_demo/demo_keystore
$ export ROS_SECURITY_ENABLE=true
$ export ROS_SECURITY_STRATEGY=Enforce

$ cd ~/sros2_demo/demo_keys/enclaves/talker_listener/listener
```

制作备份副本 `permissions.p7s` 开始之前。

##### 问题1

打开 `permissions.p7s` 在文本编辑器中。对 XML 内容(例如添加空格或空白行)进行可忽略不计的更改并保存文件。启动收听器节点 :

``` console
$ ros2 run demo_nodes_cpp listener --ros-args --enclave /talker_listener/listener
```

你想怎样?

你能启动说话器节点吗?

``` console
$ ros2 run demo_nodes_cpp talker --ros-args --enclave /talker_listener/talker
```

发动听者与发动说话者有何区别?

##### 答 题 1

收听器无法发射并投出错误。 当 `permissions.p7s` 文件被修改了 — 不管多么小 — 文件的签名无效。 当权限文件无效时, 节点将不会启用和执行安全性。

说话者会按预期开始,它使用 `permissions.p7s` 文件在不同的飞地中,文件仍然有效。

##### 问题2

哪个命令允许您检查修改后的签名是否 `permissions.p7s` 文件有效吗 ?

##### 答 问 2

检查一下 `permissions.p7s` 已经由权限 CA 使用 `openssl smime` 命令 :

``` console
$ openssl smime -verify -in permissions.p7s -CAfile permissions_ca.cert.pem
```

还原原件, 正确签名 `permissions.p7s` 文档,然后进入下一个教程。
