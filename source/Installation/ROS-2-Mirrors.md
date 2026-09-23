<span id="mirrors"></span>

# 镜像站

<span id="docs-mirrors"></span>

## 文档镜像

当[主站](http://docs.ros.org)不可用时，ROS 文档镜像可作为备用站点；对地理位置更靠近镜像站的用户来说，它们也可能提供更快的访问速度。

<span id="debian-ubuntu-apt-repository-mirrors"></span>

## Debian/Ubuntu（APT）软件源镜像

要使用这些镜像，请在 APT 配置中，将 ROS 官方软件源 URL 替换为下表中相应的地址。

<span id="asia"></span>

### 亚洲

| 镜像提供方 | 国家 | 地址 |
| --- | --- | --- |
| 清华大学（TUNA） | 中国 | <https://mirrors.tuna.tsinghua.edu.cn/ros2/ubuntu/> |
| 中国科学技术大学（USTC） | 中国 | <https://mirrors.ustc.edu.cn/ros2/ubuntu/> |
| 阿里云（Aliyun） | 中国 | <https://mirrors.aliyun.com/ros2/ubuntu/> |
| 齐鲁工业大学（QLU） | 中国 | <https://mirrors.qlu.edu.cn/ros2/ubuntu/> |
| 重庆大学（CQU） | 中国 | <https://mirrors.cqu.edu.cn/ros2/ubuntu/> |

<span id="europe"></span>

### 欧洲

| 镜像提供方 | 国家 | 地址 |
| --- | --- | --- |
| 代尔夫特理工大学 | 荷兰 | <http://ftp.tudelft.nl/ros2/ubuntu/> |

<span id="north-america"></span>

### 北美洲

| 镜像提供方 | 国家 | 地址 |
| --- | --- | --- |
| 马里兰大学（UMD） | 美国 | <http://mirror.umd.edu/packages.ros.org/ros2/ubuntu/> |
| nulled LLC | 美国 | <http://mirror.nulled.llc/ros2/ubuntu/> |

<span id="oceania"></span>

### 大洋洲

| 镜像提供方 | 国家 | 地址 |
| --- | --- | --- |
| AARNet | 澳大利亚 | <https://mirror.aarnet.edu.au/pub/ros2-packages/ubuntu/> |

<span id="south-america-and-africa"></span>

### 南美洲和非洲

这些地区目前没有经过官方验证的 ROS 2 镜像站。如果你在南美洲或非洲提供镜像服务，并希望将其列在这里，请参阅下方的镜像托管说明。

<span id="creating-a-mirror"></span>

## 创建镜像

如果你正在维护镜像，请加入 discourse.openrobotics.org 上的 [Mirrors 分类](https://discourse.openrobotics.org/c/infrastructure-project/infra-mirrors/)，以便反馈问题并及时获取更新。

<span id="using-a-mirror"></span>

### 使用镜像

要使用镜像，请在 `ros2-latest.list` 文件中，将 `packages.ros.org` 替换为镜像 URL：

```bash
# Example for TUNA mirror
sudo sed -i 's|http://packages.ros.org/ros2/ubuntu|https://mirrors.tuna.tsinghua.edu.cn/ros2/ubuntu|g' /etc/apt/sources.list.d/ros2-latest.list
sudo apt update
```

<span id="setting-up-a-mirror"></span>

## 搭建镜像

ROS 基础设施使用 `rsync` 分发软件包。要创建 ROS 2 软件源的本地镜像，请执行以下步骤：

1. **存储要求：** 确保至少有 500 GB 可用磁盘空间。
2. **同步命令：** 使用 `rsync` 从 OSUOSL 官方端点拉取数据：

    ```bash
    # Sync the main ROS 2 repository
    rsync -azv rsync.osuosl.org::ros2-main /your/local/path --delete
    ```

3. **维护：** 设置 `cron` 任务，每 6–12 小时同步一次。

<span id="adding-your-mirror-to-this-list"></span>

### 将镜像加入本列表

要加入官方列表，镜像必须满足以下要求：

- 支持 **HTTPS**。
- 至少每 24 小时同步一次。
- 提供用于接收基础设施告警的联系邮箱。

验证通过后，请为本页提交 Pull Request，或在 [Discourse 的 Mirrors 分类](https://discourse.openrobotics.org/c/infrastructure-project/infra-mirrors/)中发帖。

<span id="mirroring-docs-ros-org"></span>

## 镜像 docs.ros.org

镜像文档站点需要专门的配置，以避免搜索引擎索引分散。如果你希望提供区域性文档镜像，请在开始之前，通过 Discourse **联系基础设施团队**。
