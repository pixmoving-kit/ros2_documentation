<span id="setting-access-controls"></span>
<span id="access-controls"></span>
<span id="background"></span>
<span id="modify-permissions-xml"></span>
<span id="sign-the-policy-file"></span>
<span id="launch-the-node"></span>
<span id="use-the-templates"></span>

# 设置访问控制

**目标：** 限制节点可以使用的话题。

**教程级别：** 高级

**耗时：** 20 分钟

## 背景

权限设置十分灵活，可以控制 ROS 图中的多种行为。

本教程演示一种仅允许在默认 `chatter` 话题上发布消息的策略。例如，该策略可以阻止启动 listener 时重映射话题，或将同一安全隔离域用于其他用途。

要实施此策略，需要先更新 `permissions.xml` 文件并重新签名，再启动节点。可以手动修改权限文件，也可以使用 XML 模板。

### 修改 `permissions.xml`

首先备份权限文件，然后打开 `permissions.xml` 进行编辑：

```console
$ cd ~/sros2_demo/demo_keys/enclaves/talker_listener/talker
$ mv permissions.p7s permissions.p7s~
$ mv permissions.xml permissions.xml~
$ vi permissions.xml
```

我们将修改 `<publish>` 和 `<subscribe>` 的 `<allow_rule>`。此 XML 文件中的话题采用 DDS 命名格式，而不是 ROS 名称。有关 ROS 与 DDS 话题名称的映射，参阅[话题和服务名称设计文档](https://design.ros2.org/articles/topic_and_service_names.html#mapping-of-ros-2-topic-and-service-names-to-dds-concepts)。

把下面的 XML 内容粘贴到 `permissions.xml`，保存并退出文本编辑器。其中，ROS 话题 `chatter` 和 `rosout` 分别对应 DDS 话题 `rt/chatter` 和 `rt/rosout`：

```xml
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

此策略允许 talker 在 `chatter` 和 `rosout` 话题上发布消息。它还包含 talker 节点管理参数所需的发布和订阅权限，这是所有节点都需要的权限。发现机制的权限沿用原模板，没有改动。

### 为策略文件签名

下面的命令根据更新后的 XML 文件 `permissions.xml` 创建新的 S/MIME 签名策略文件 `permissions.p7s`。必须使用权限 CA 证书进行签名，**这需要访问权限 CA 的私钥**。如果私钥受到保护，可能还需要按照安全方案执行额外步骤，才能解锁和使用它。

```console
$ openssl smime -sign -text -in permissions.xml -out permissions.p7s \
  --signer permissions_ca.cert.pem \
  -inkey ~/sros2_demo/demo_keys/private/permissions_ca.key.pem
```

### 启动节点

更新权限后，可以使用与前面教程相同的命令成功启动节点：

```console
$ ros2 run demo_nodes_cpp talker --ros-args --enclave /talker_listener/talker
```

但是，尝试重映射 `chatter` 话题会导致节点无法启动。注意，这要求将 `ROS_SECURITY_STRATEGY` 设置为 `Enforce`。

```console
$ ros2 run demo_nodes_cpp talker --ros-args --enclave /talker_listener/talker \
  --remap chatter:=not_chatter
```

### 使用模板

安全策略很容易变得复杂难懂，因此 `sros2` 工具提供了从模板创建策略的功能。这里使用 `sros2` 仓库提供的[示例策略文件](https://github.com/ros2/sros2/blob/rolling/sros2/test/policies/sample.policy.xml#L1)，创建一份仅允许 `talker` 和 `listener` 使用 `chatter` 话题的策略。

首先下载包含示例策略文件的 `sros2` 仓库：

```console
$ git clone https://github.com/ros2/sros2.git /tmp/sros2
```

然后使用 `create_permission` 子命令并指定示例策略，生成 XML 权限文件：

```console
$ ros2 security create_permission demo_keystore \
  /talker_listener/talker \
  /tmp/sros2/sros2/test/policies/sample.policy.xml
$ ros2 security create_permission demo_keystore \
  /talker_listener/listener \
  /tmp/sros2/sros2/test/policies/sample.policy.xml
```

这些权限文件仅允许节点发布或订阅 `chatter` 话题，同时允许参数管理所需的通信。

按照前面的安全教程启用安全功能，在一个终端中运行 `talker` 演示程序：

```console
$ ros2 run demo_nodes_cpp talker --ros-args -e /talker_listener/talker
```

在另一个终端中以同样的方式运行 `listener`：

```console
$ ros2 run demo_nodes_py listener --ros-args -e /talker_listener/listener
```

此时，`talker` 和 `listener` 节点通过显式访问控制列表进行安全通信。不过，下面让 `listener` 订阅 `chatter` 之外话题的尝试会失败：

```console
$ ros2 run demo_nodes_py listener --ros-args --enclave /talker_listener/listener \
  --remap chatter:=not_chatter
```
