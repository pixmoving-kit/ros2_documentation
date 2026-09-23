---
translation_status: machine_translated
source: Releases/lyrical/supported-platforms.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="lyrical-luth-supported-platforms"></span>

# Lyrical Luth 支持的平台

ROS Lyrical 支持以下平台: [平台支持级别](../../The-ROS2-Project/Platform-Support-Tiers.md):

| 建筑 | 乌本图决战(26.04). | 乌本图·诺贝尔\*(24.04) | Windows 11 (VS2022) (英语). | 莱尔 10 | macOS | 德比安·特里克西\* (13) | OpenEmbed / Yocto 项目 |
|----|----|----|----|----|----|----|----|
| amd64 (中文(简体) ). | 第1级 \[d\]\[a\] | 第3级 | 第1级 \[a\] | 第二级\[d\]\[a\] | 第3级 | 第3级 | 第3级 |
| 军火64 | 第1级 \[d\]\[a\] | 第3级 |  |  |  | 第3级 | 第3级 |
| 臂弹32 | 第3级 | 第3级 |  |  |  | 第3级 | 第3级 |

- `*` 早期 OL / 每名 [平台EOL政策](../../The-ROS2-Project/Platform-EOL-Policy.md)  
  - 乌本图·诺贝尔得到支持,直到 `2029-06-01`

  - Debian Trixie 被支持到 `2028-08-09`

- `[d]` 您可以在此平台上安装 ROS Lyrical , 使用发行专用的包( Debian, RPM 等) 。

- `[a]` 您可以通过下载包含预建的包包的归档来安装 ROS Lyrical [ROS 拼写 ros2. repos 文件](https://github.com/ros2/ros2/blob/lyrical/ros2.repos)

要在任何三级平台上使用 ROS Lyrical, 你必须从源头构建 ROS Lyrical.

<span id="minimum-language-requirements"></span>

## 最低语文要求

- [C++20](https://discourse.openrobotics.org/t/ros-2-lyrical-c-version/52551)

- C17

- Python 3.12 - 3.14 (英语).

<span id="dependency-requirements"></span>

## 扶养要求

<table class="docutils align-default">
<thead>
<tr class="row-odd">
<th class="head"></th>
<th colspan="2" class="head"><p>所需支助</p></th>
<th colspan="5" class="head"><p>建议的支助</p></th>
</tr>
</thead>
<tbody>
<tr class="row-even">
<td><p>软件包</p></td>
<td><p>乌本图决断</p></td>
<td><p>窗口 11</p></td>
<td><p>乌邦图诺布尔</p></td>
<td><p>莱尔 10</p></td>
<td><p>马科斯***</p></td>
<td><p>德比安·特里克西(Debian Trixie)</p></td>
<td><p>打开嵌入</p></td>
</tr>
<tr class="row-odd">
<td><p>CMake</p></td>
<td><p>4.2.3</p></td>
<td><p>3.28.3</p></td>
<td><p>3.28.3</p></td>
<td><p>3.30.5</p></td>
<td><p>4.3.2</p></td>
<td><p>3.31.6</p></td>
<td><p>4.3.2</p></td>
</tr>
<tr class="row-even">
<td><p>爱咪</p></td>
<td><p>4.2.1</p></td>
<td><p>3.3.4</p></td>
<td><p>3.3.4</p></td>
<td><p>4.2.1</p></td>
<td></td>
<td><p>3.3.4</p></td>
<td><p>3.3.2</p></td>
</tr>
<tr class="row-odd">
<td><p>Gazebo</p></td>
<td><p>杰蒂* (简体中文).</p></td>
<td><p>N/A</p></td>
<td><p>N/A</p></td>
<td><p>N/A</p></td>
<td><p>杰蒂* (简体中文).</p></td>
<td><p>杰蒂* (简体中文).</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>数字</p></td>
<td><p>2.3.4</p></td>
<td><p>1.26.4</p></td>
<td><p>1.26.4</p></td>
<td><p>1.26.4</p></td>
<td><p>2.4.5</p></td>
<td><p>2.2.4</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-odd">
<td><p>食人鱼</p></td>
<td><p>1.12.10</p></td>
<td><p>1.12.10</p></td>
<td><p>1.12.10</p></td>
<td><p>N/A</p></td>
<td></td>
<td><p>1.12.10</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>打开CV</p></td>
<td><p>4.10.0</p></td>
<td><p>4.10.0</p></td>
<td><p>4.6.0</p></td>
<td><p>4.10.0</p></td>
<td><p>4.13.10</p></td>
<td><p>4.10.0</p></td>
<td><p>4.13.10</p></td>
</tr>
<tr class="row-odd">
<td><p>打开SSL</p></td>
<td><p>3.5.5</p></td>
<td><p><code class="docutils literal notranslate">&gt;=3.4</code></p></td>
<td><p>3.0.13</p></td>
<td><p>3.5.1</p></td>
<td><p>3.6.2</p></td>
<td><p>3.5.6</p></td>
<td><p>3.5.6</p></td>
</tr>
<tr class="row-even">
<td><p>Python</p></td>
<td><p>3.14.3</p></td>
<td><p>3.12.3</p></td>
<td><p>3.12.3</p></td>
<td><p>3.12.12</p></td>
<td><p>3.14.5</p></td>
<td><p>3.13.5</p></td>
<td><p>3.14.4</p></td>
</tr>
<tr class="row-odd">
<td><p>Qt 键</p></td>
<td><p>6.10.2</p></td>
<td><p><code class="docutils literal notranslate">&gt;=6</code></p></td>
<td><p>5.15.13</p></td>
<td><p>6.10.1</p></td>
<td><p>6.11.1</p></td>
<td><p>6.8.2</p></td>
<td><p>N/A</p></td>
</tr>
<tr class="row-even">
<td><p>个人计算机L</p></td>
<td><p>1.15.1</p></td>
<td><p>N/A</p></td>
<td><p>1.14.0</p></td>
<td><p>1.15.0*</p></td>
<td><p>1.15.1</p></td>
<td><p>1.15.0</p></td>
<td><p>6.12.0</p></td>
</tr>
</tbody>
</table>

" \* " 是指这不是上游版本(可在官方操作系统寄存器上查阅),而是OSRF或社区分发的包(自定义寄存器上建立和分发的包)。

" \*\*"是指依赖可能看到多个版本的改变,因为依赖使用一个包管理器,在没有稳定的API的情况下不断更新依赖.

此文档只抓取 ROS 发行的首次发布时的版本, 并且不会随着依赖关系向前移动而更新, 因此这些版本是低水印 。

<span id="middleware-implementation-support"></span>

## 中件执行支持

ROS Lyrical中默认的中间软件是 **rmw_fastrtps_cpp**.

<table class="docutils align-default">
<thead>
<tr class="row-odd">
<th class="head"><p>中间软件</p></th>
<th class="head"><p>乌本图决断</p></th>
<th class="head"><p>窗口 11</p></th>
<th class="head"><p>乌邦图诺布尔</p></th>
<th class="head"><p>莱尔 10</p></th>
<th class="head"><p>macOS</p></th>
<th class="head"><p>德比安·特里克西(Debian Trixie)</p></th>
<th class="head"><p>打开嵌入</p></th>
</tr>
</thead>
<tbody>
<tr class="row-even">
<td><p>Connext DDS</p></td>
<td colspan="2"><p>7.7.0</p></td>
<td colspan="2"><p>N/A</p></td>
<td><p>7.7.0</p></td>
<td colspan="2"><p>N/A</p></td>
</tr>
<tr class="row-odd">
<td><p>Cyclone DDS</p></td>
<td colspan="7"><p>11.0.x</p></td>
</tr>
<tr class="row-even">
<td><p>快速数据交换系统</p></td>
<td colspan="7"><p>3.6.x</p></td>
</tr>
<tr class="row-odd">
<td><p>Zenoh</p></td>
<td colspan="7"><p>1.8.0</p></td>
</tr>
</tbody>
</table>

| 中间软件库 | 中件提供者 | 支助级别 | 建筑 |
|----|----|----|----|
| rmw_fastrtps_cpp | eProsima Fast-DDS 软件 | 第1级 | 所有建筑 |
| rmw_connextdds | RTI 连接 | 第1级 | 除arm64外的所有建筑 |
| rmw_cyclonedds_cpp | Eclipse Cyclone DDS | 第1级 | 所有建筑 |
| rmw_zenoh_cpp | Eclipse Zenoh (英语). | 第1级 | 所有建筑 |
| rmw_fastrtps_dynamic_cpp | eProsima Fast-DDS 软件 | 第二级 | 所有建筑 |

Middleware执行支持取决于平台支持级别。例如,一个Tier 2 平台上的第一级中间软件执行将只获得第二级支持。
