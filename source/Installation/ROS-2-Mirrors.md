---
translation_status: machine_translated
source: Installation/ROS-2-Mirrors.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="mirrors"></span>

# 镜像站点

<span id="docs-mirrors"></span>

## 多克镜像

ROS Docs 的镜像在 [主站点](http://docs.ros.org) 无法使用,而且可以更快地向地理上接近镜像的用户提供访问机会。

<span id="debian-ubuntu-apt-repository-mirrors"></span>

## Debian/Ubuntu (APT) 存储镜

要使用这些镜像, 请用您 APT 配置中列出的 ROS 官方寄存器 URL 替换 。

<span id="asia"></span>

### 亚洲

|  |  |  |
|----|----|----|
| 清华大学(TUNA) | 中国 | <https://mirrors.tuna.tsinghua.edu.cn/ros2/ubuntu/> |
| USTC 美国电信公司 | 中国 | <https://mirrors.ustc.edu.cn/ros2/ubuntu/> |
| 阿里巴巴云(亚利云). | 中国 | <https://mirrors.aliyun.com/ros2/ubuntu/> |
| 齐鲁科技大学(QLU) | 中国 | <https://mirrors.qlu.edu.cn/ros2/ubuntu/> |
| 重庆大学(CQU) | 中国 | <https://mirrors.cqu.edu.cn/ros2/ubuntu/> |

<span id="europe"></span>

### 欧洲

|               |      |                                      |
|---------------|------|--------------------------------------|
| Delft技术大学 | 荷兰 | <http://ftp.tudelft.nl/ros2/ubuntu/> |

<span id="north-america"></span>

### 北美

|  |  |  |
|----|----|----|
| 马里兰大学(大学) | 美国 | <http://mirror.umd.edu/packages.ros.org/ros2/ubuntu/> |
| 已取消的专卖中心 | 美国 | <http://mirror.nulled.llc/ros2/ubuntu/> |

<span id="oceania"></span>

### 大洋洲

|          |          |                                                          |
|----------|----------|----------------------------------------------------------|
| 亚洲网络 | 澳大利亚 | <https://mirror.aarnet.edu.au/pub/ros2-packages/ubuntu/> |

<span id="south-america-and-africa"></span>

### 南美洲和非洲

目前这些地区没有经过官方核实的ROS 2 镜像。 如果您在南美洲或非洲托管一个镜像, 并想在此列出, 请参见 **正在托管镜像** 下节。

<span id="creating-a-mirror"></span>

## 创建镜像

请加入镜像类。 openrobotics. <https://discourse.openrobotics.org/c/infrastructure-project/infra-mirrors/> 用于反馈和及时更新。

<span id="using-a-mirror"></span>

### 使用镜像

要使用镜子,请替换 `packages.ros.org` 在您的镜像 URL 中 `ros2-latest.list` 文件 :

``` bash
# Example for TUNA mirror
sudo sed -i 's|http://packages.ros.org/ros2/ubuntu|https://mirrors.tuna.tsinghua.edu.cn/ros2/ubuntu|g' /etc/apt/sources.list.d/ros2-latest.list
sudo apt update
```

<span id="setting-up-a-mirror"></span>

## 建立镜像

ROS基础设施使用 `rsync` 要创建 ROS 2 存储器的本地镜像 :

1.  **存储要求 :** 确保您至少有500GB可用的磁盘空间 。

2.  **同步命令 :** 使用 `rsync` 从官方的 OSUOSL 端点拉出:

``` bash
# Sync the main ROS 2 repository
rsync -azv rsync.osuosl.org::ros2-main /your/local/path --delete
```

3.  **维持:** 设置一个 `cron` 任务每6-12小时同步一次。

<span id="adding-your-mirror-to-this-list"></span>

### 将您的镜像添加到此列表

要正式上市,您的镜像必须符合以下要求: 1.

- 支助 **HTTPS (韩语)**.

- 至少每24小时同步一次.

- 为基础设施提醒提供联系电子邮件 。

校对后,请针对此页面或帖子打开“拉”请求。 [镜像对话](https://discourse.openrobotics.org/c/infrastructure-project/infra-mirrors/).

<span id="mirroring-docs-ros-org"></span>

## 镜像 docs.ros.org

镜像文档网站需要特定的配置来防止搜索引擎破碎。如果您想托管文档的区域镜像,请 **与基础设施小组联系** 在诉讼前通过演讲。
