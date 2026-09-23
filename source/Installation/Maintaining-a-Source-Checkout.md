---
translation_status: machine_translated
source: Installation/Maintaining-a-Source-Checkout.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="maintain-source-checkout"></span> <span id="maintainingsource"></span>

# 维护源码工作副本

如果您从源头安装了 ROS 2, 自您检查时起, 源代码可能会有变化 。 要更新您的源代码检查, 您必须定期更新您的代码 。 `ros2.repos` 文件,下载最新来源,并重建工作空间。

<span id="update-your-repository-list"></span>

## 更新您的仓库列表

每个ROS 2 发布包括: `ros2.repos` 包含存储器列表及其版本的文件。

<span id="latest-ros-2-rolling-branches"></span>

### 最新版ROS 2 滚动分支

如果您想要查看ROS 2 Rolling的最新代码, 您可以通过运行获取相关的寄存器列表 :

##### Linux

``` console
$ cd ~/ros2_rolling
$ mv -i ros2.repos ros2.repos.old
$ wget https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos
```

##### macOS

``` console
$ cd ~/ros2_rolling
$ mv -i ros2.repos ros2.repos.old
$ wget https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos
```

##### Windows

使用 Windows 命令行接口 :

``` console
$ cd \dev\ros2_rolling
$ curl -sk https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos -o ros2.repos
```

或电壳曰:

``` console
$ cd \dev\ros2_rolling
$ curl https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos -o ros2.repos
```

<span id="update-your-repositories"></span>

## 更新您的寄存器

你会注意到,在 [ros2. repos 键](https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos) 文件,每个寄存器都有 `version` 这些版本可能指向您本地寄存器副本无法识别的已过期的新寄存器/空白。 因此, 您应该更新您已经用以下命令检查过的寄存器 :

``` console
$ vcs custom --args remote update
```

<span id="download-the-new-source-code"></span>

## 下载新源代码

您现在应该可以下载与新寄存器列表相关的源 :

##### Linux

``` console
$ vcs import src < ros2.repos
$ vcs pull src
```

##### macOS

``` console
$ vcs import src < ros2.repos
$ vcs pull src
```

##### Windows

在 Windows 命令行界面中 :

``` console
$ vcs import --input ros2.repos src
$ vcs pull src
```

或于权壳中:

``` console
$ vcs import --input ros2.repos src
$ vcs pull src
```

<span id="rebuild-your-workspace"></span>

## 重建工作空间

现在工作空间已经与最新来源更新, 移除您先前的安装并重建您的工作空间, 例如 :

``` console
$ colcon build --symlink-install
```

<span id="inspect-your-source-checkout"></span>

## 检查您的源检出

在开发过程中, 您可能偏离了您导入寄存器列表时的工作空间的原始状态。 如果您想要了解您工作空间中的寄存器的版本, 您可以使用以下命令导出此信息 :

##### Linux

``` console
$ cd ~/ros2_rolling
$ vcs export src > my_ros2.repos
```

##### macOS

``` console
$ cd ~/ros2_rolling
$ vcs export src > my_ros2.repos
```

##### Windows

``` console
$ cd \dev\ros2_rolling
$ vcs export src > my_ros2.repos
```

这个 `my_ros2.repos` 然后文件可以和其他人共享,以便复制您工作空间中寄存器的状态。
