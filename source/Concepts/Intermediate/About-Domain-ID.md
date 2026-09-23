<span id="the-ros-domain-id"></span>
# ROS_DOMAIN_ID

<span id="overview"></span>
## 概述

如其他章节所述，ROS 2 默认使用 DDS 中间件进行通信。在 DDS 中，让不同逻辑网络共享同一物理网络的主要机制称为域 ID（Domain ID）。同一域中的 ROS 2 节点可以自由地相互发现并发送消息，不同域中的节点则不能。所有 ROS 2 节点默认使用域 ID 0。为避免同一网络中运行 ROS 2 的不同计算机组相互干扰，应为每组设置不同的域 ID。

<span id="choosing-a-domain-id-short-version"></span>
## 选择域 ID：简要说明

下文将解释 ROS 2 中可用域 ID 范围的推导过程。如果不需要了解背景，只想选择一个安全的值，请选择 **0 到 101** 之间的域 ID，包含两个端点。

<span id="choosing-a-domain-id-long-version"></span>
## 选择域 ID：详细说明

DDS 使用域 ID 来计算发现和通信所需的 UDP 端口。端口计算方式的详细说明见[这篇文章](https://community.rti.com/content/forum-topic/statically-configure-firewall-let-omg-dds-traffic-through)。回顾网络基础知识，UDP 端口号是一个[无符号 16 位整数](https://en.wikipedia.org/wiki/User_Datagram_Protocol#Ports)，因此能分配的最大端口号为 65535。代入上述文章中的公式计算可知，可分配的最大域 ID 为 232，最小值为 0。

<span id="platform-specific-constraints"></span>
### 平台特定的限制

为获得最佳兼容性，选择域 ID 时还应遵守一些与平台有关的限制。尤其应避免选择会占用操作系统[临时端口范围](https://en.wikipedia.org/wiki/Ephemeral_port)的域 ID，以免 ROS 2 节点使用的端口与计算机上的其他网络服务发生冲突。

各平台的临时端口情况如下。

#### Linux

默认情况下，Linux 内核将 32768–60999 用作临时端口范围。因此，域 ID 0–101 和 215–232 可以安全使用，不会与临时端口冲突。Linux 可以通过修改 `/proc/sys/net/ipv4/ip_local_port_range` 中的值来自定义临时端口范围。如果使用了自定义范围，上述数值可能需要相应调整。

#### macOS

默认情况下，macOS 的临时端口范围是 49152–65535。因此，域 ID 0–166 可以安全使用，不会与临时端口冲突。macOS 可以通过设置 `net.inet.ip.portrange.first` 和 `net.inet.ip.portrange.last` 的 sysctl 值来自定义临时端口范围。如果使用了自定义范围，上述数值可能需要相应调整。

#### Windows

默认情况下，Windows 的临时端口范围是 49152–65535。因此，域 ID 0–166 可以安全使用，不会与临时端口冲突。Windows 可以[使用 netsh](https://docs.microsoft.com/en-us/troubleshoot/windows-server/networking/default-dynamic-port-range-tcpip-chang) 配置临时端口范围。如果使用了自定义范围，上述数值可能需要相应调整。

<span id="participant-constraints"></span>
### 参与者数量限制

计算机上每运行一个 ROS 2 进程，就会创建一个 DDS“参与者”（participant）。每个 DDS 参与者会占用两个端口，因此在一台计算机上运行超过 120 个 ROS 2 进程时，端口分配可能越界到其他域 ID 的端口范围或临时端口范围。

以域 ID 1 和 2 为例，可以理解其原因：

- 域 ID 1 使用端口 7650 和 7651 进行组播。
- 域 ID 2 使用端口 7900 和 7901 进行组播。
- 在域 ID 1 中创建第 1 个进程（参与者编号 0）时，使用端口 7660 和 7661 进行单播。
- 在域 ID 1 中创建第 120 个进程（参与者编号 119）时，使用端口 7898 和 7899 进行单播。
- 在域 ID 1 中创建第 121 个进程（参与者编号 120）时，使用端口 7900 和 7901 进行单播，与域 ID 2 的端口重叠。

如果可以确定该计算机在任一时刻只使用一个域 ID，且域 ID 足够小，就可以安全地创建更多 ROS 2 进程。

当选择的域 ID 接近平台允许范围的上限时，还需要考虑额外限制。例如，一台 Linux 计算机使用域 ID 101：

- 编号为 0 的 ROS 2 进程会连接到端口 32650、32651、32660 和 32661。
- 编号为 1 的 ROS 2 进程会连接到端口 32650、32651、32662 和 32663。
- 编号为 53 的 ROS 2 进程会连接到端口 32650、32651、32766 和 32767。
- 编号为 54 的 ROS 2 进程会连接到端口 32650、32651、32768 和 32769，进入临时端口范围。

因此，在 Linux 上使用域 ID 101 时，最多应创建 54 个进程。同样，由于最大端口号为 65535，在 Linux 上使用域 ID 232 时，最多应创建 63 个进程。

macOS 和 Windows 的情况类似，但具体数值不同。在这两个平台上，选择域 ID 166（范围上限）时，一台计算机上最多可以创建 120 个 ROS 2 进程，而不进入临时端口范围。

<span id="domain-id-to-udp-port-calculator"></span>
### 域 ID 到 UDP 端口的计算器

<table>
  <tr>
    <td style="text-align: right; vertical-align: middle;"><label>域 ID：</label></td>
    <td><input type="number" min="0" max="232" size="3" class="display" value="0" id="domainID" onChange="calculate(this.value)"/></td>
  </tr>
  <tr>
    <td style="text-align: right; vertical-align: middle;"><label>参与者 ID：</label></td>
    <td><input type="number" min="0" size="3" class="display" value="0" id="participantID" onChange="calculate(this.value)"/></td>
  </tr>
</table>
<hr/>
<table>
  <tr>
    <td style="text-align: right; vertical-align: middle;"><label>发现组播端口：</label></td>
    <td><input type="text" size="5" class="discoveryMulticastPort" disabled/></td>
  </tr>
  <tr>
    <td style="text-align: right; vertical-align: middle;"><label>用户组播端口：</label></td>
    <td><input type="text" size="5" class="userMulticastPort" disabled/></td>
  </tr>
  <tr>
    <td style="text-align: right; vertical-align: middle;"><label>发现单播端口：</label></td>
    <td><input type="text" size="5" class="discoveryUnicastPort" disabled/></td>
  </tr>
  <tr>
    <td style="text-align: right; vertical-align: middle;"><label>用户单播端口：</label></td>
    <td><input type="text" size="5" class="userUnicastPort" disabled/></td>
  </tr>
</table>
<br/>
<br/>

<script type="text/javascript">
  window.addEventListener('load', (event) => {
     calculate(event);
  });
  const discoveryMcastPort = document.querySelector('.discoveryMulticastPort');
  const userMcastPort = document.querySelector('.userMulticastPort');
  const discoveryUnicastPort = document.querySelector('.discoveryUnicastPort');
  const userUnicastPort = document.querySelector('.userUnicastPort');

  const domainID = document.getElementById('domainID');
  const participantID = document.getElementById('participantID');

  // calculate function
  function calculate(event) {
    const d0 = 0;
    const d2 = 1;
    const d1 = 10;
    const d3 = 11;
    const PB = 7400;
    const DG = 250;
    const PG = 2;

    discoveryMcastPort.value = PB + (DG * domainID.value) + d0;
    userMcastPort.value = PB + (DG * domainID.value) + d2;
    discoveryUnicastPort.value = PB + (DG * domainID.value) + d1 + (PG * participantID.value);
    userUnicastPort.value = PB + (DG * domainID.value) + d3 + (PG * participantID.value);
  }
</script>
