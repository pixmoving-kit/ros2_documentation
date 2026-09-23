---
translation_status: machine_translated
source: Tutorials/Advanced/Security/Access-Controls.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="setting-access-controls"></span> <span id="access-controls"></span>

# 设置访问控制

**目标：** 限制节点可以使用的主题 。

**教程级别：** 高级

**用时：** 20分钟

<span id="background"></span>

## 背景

权限相当灵活,可用于控制ROS图内的许多行为.

对于此教程, 我们演示一个只允许在默认情况下发布消息的政策 。 `chatter` 例如,这将防止在发射听众时或为另一个目的使用同一安全飞地时重新绘制专题图。

为了执行这项政策,我们需要更新 `permissions.xml` 在启动节点之前,文件可以重新设计。这可以通过手动修改权限文件或者使用XML模板来实现。

<span id="modify-permissions-xml"></span>

### 修改 `permissions.xml`

从您权限文件的备份开始,然后打开 `permissions.xml` 供编辑 :

``` console
$ cd ~/sros2_demo/demo_keys/enclaves/talker_listener/talker
$ mv permissions.p7s permissions.p7s~
$ mv permissions.xml permissions.xml~
$ vi permissions.xml
```

我们将修改 `<allow_rule>` (单位:千美元) `<publish>` 财务报告和财务报告 `<subscribe>`。此 XML 文件的主题使用 DDS 命名格式,而不是 ROS 名称。在 ROS 和 DDS 之间查找绘图主题名称的细节 。 [主题和服务名称设计文件](https://design.ros2.org/articles/topic_and_service_names.html#mapping-of-ros-2-topic-and-service-names-to-dds-concepts).

粘贴到以下 XML 内容 `permissions.xml`中,保存文件并退出文本编辑器。这里显示 `chatter` 财务报告和财务报告 `rosout` ROS 主题重新命名为 DDS `rt/chatter` 财务报告和财务报告 `rt/rosout` 专题,分别:

``` xml
<dds xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://www.omg.org/spec/DDS-SECURITY/20170901/omg_shared_ca_permissions.xsd">
  <permissions>
    <grant name="/talker_listener/talker">
      <subject_name>CN=/talker_listener/talker</subject_name>
      <validity>
        <not_before>2021-06-01T16:57:53</not_before>
        <not_after>2031-05-31T16:57:53</not_after>
      </validity>
      <allow_rule>
        <domains>
          <id>0</id>
        </domains>
        <publish>
          <topics>
            <topic>rt/chatter</topic>
            <topic>rt/rosout</topic>
            <topic>rt/parameter_events</topic>
            <topic>*/talker/*</topic>
          </topics>
        </publish>
        <subscribe>
          <topics>
            <topic>rt/parameter_events</topic>
            <topic>*/talker/*</topic>
          </topics>
        </subscribe>
      </allow_rule>
      <allow_rule>
        <domains>
          <id>0</id>
        </domains>
        <publish>
          <topics>
            <topic>ros_discovery_info</topic>
          </topics>
        </publish>
        <subscribe>
          <topics>
            <topic>ros_discovery_info</topic>
          </topics>
        </subscribe>
      </allow_rule>
      <default>DENY</default>
    </grant>
  </permissions>
</dds>
```

此政策允许谈话者在 `chatter` 页:1 `rosout` 主题。 还包括发布和订阅谈话者节点管理参数所需的权限( 对所有节点的要求) 。 发现权限与原始模板保持不变 。

<span id="sign-the-policy-file"></span>

### 签名策略文件

此下一个命令创建了新的 S/MIME 签名的政策文件 `permissions.p7s` 从更新的 XML 文件 `permissions.xml`。文件必须在权限 CA 证书上签名, **它需要访问权限 CA 私人密钥**。如果私人密钥已经保护,可能需要根据您的安全计划采取更多步骤来解锁并使用。

``` console
$ openssl smime -sign -text -in permissions.xml -out permissions.p7s \
  --signer permissions_ca.cert.pem \
  -inkey ~/sros2_demo/demo_keys/private/permissions_ca.key.pem
```

<span id="launch-the-node"></span>

### 启动节点

在更新的权限到位后, 我们可以使用先前教程中使用的相同命令成功启动节点 :

``` console
$ ros2 run demo_nodes_cpp talker --ros-args --enclave /talker_listener/talker
```

然而,试图重新绘制 `chatter` 主题防止节点发射(注意这需要: `ROS_SECURITY_STRATEGY` 设置为 `Enforce`).

``` console
$ ros2 run demo_nodes_cpp talker --ros-args --enclave /talker_listener/talker \
  --remap chatter:=not_chatter
```

<span id="use-the-templates"></span>

### 使用模板

安全政策可能很快变得混乱,因此, `sros2` 公用设施从模板中添加创建策略的能力。 [政策文件样本](https://github.com/ros2/sros2/blob/rolling/sros2/test/policies/sample.policy.xml#L1) B. 《公约》第14条 `sros2` 让我们为两个数据库创建一个政策。 `talker` 页:1 `listener` 仅使用 `chatter` 主题。

开始下载 `sros2` 带有政策文件样本的仓库 :

``` console
$ git clone https://github.com/ros2/sros2.git /tmp/sros2
```

那就用那个 `create_permission` 动词同时指向生成 XML 权限文件的样本策略 :

``` console
$ ros2 security create_permission demo_keystore \
  /talker_listener/talker \
  /tmp/sros2/sros2/test/policies/sample.policy.xml
$ ros2 security create_permission demo_keystore \
  /talker_listener/listener \
  /tmp/sros2/sros2/test/policies/sample.policy.xml
```

这些权限文件只允许节点发布或订阅 `chatter` ,并允许参数所需的通信。

在一个有安全设备的终端中,像以前的安全辅导系统一样,运行 `talker` 演示程序 :

``` console
$ ros2 run demo_nodes_cpp talker --ros-args -e /talker_listener/talker
```

在另一个终端 做同样的 `listener` 程序 :

``` console
$ ros2 run demo_nodes_py listener --ros-args -e /talker_listener/listener
```

此时此刻,你的 `talker` 财务报告和财务报告 `listener` 节点将安全地使用明确的访问控制列表进行通信。 `listener` 用于订阅非主题的节点 `chatter` 将失败 :

``` console
$ ros2 run demo_nodes_py listener --ros-args --enclave /talker_listener/listener \
  --remap chatter:=not_chatter
```
