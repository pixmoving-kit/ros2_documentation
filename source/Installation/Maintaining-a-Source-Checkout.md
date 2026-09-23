<span id="maintain-source-checkout"></span>
<span id="maintainingsource"></span>

# 维护源码工作副本

如果从源码安装了 ROS 2，那么从你检出代码到现在，源码可能已经发生了变化。要保持本地源码工作副本为最新状态，需要定期更新 `ros2.repos` 文件、下载最新源码，并重新构建工作空间。

<span id="update-your-repository-list"></span>

## 更新仓库列表

每个 ROS 2 发行版都包含一个 `ros2.repos` 文件，列出该发行版使用的仓库及其版本。

<span id="latest-ros-2-rolling-branches"></span>

### 最新的 ROS 2 Rolling 分支

如果希望检出 ROS 2 Rolling 的最新代码，可以运行以下命令，获取相应的仓库列表。

**Linux：**

```console
$ cd ~/ros2_rolling
$ mv -i ros2.repos ros2.repos.old
$ wget https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos
```

**macOS：**

```console
$ cd ~/ros2_rolling
$ mv -i ros2.repos ros2.repos.old
$ wget https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos
```

**Windows：**

使用 Windows 命令行：

```console
$ cd \dev\ros2_rolling
$ curl -sk https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos -o ros2.repos
```

或使用 PowerShell：

```console
$ cd \dev\ros2_rolling
$ curl https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos -o ros2.repos
```

<span id="update-your-repositories"></span>

## 更新仓库

在 [ros2.repos](https://raw.githubusercontent.com/ros2/ros2/rolling/ros2.repos) 文件中，每个仓库都有一个对应的 `version`，指向特定的提交哈希、标签或分支名称。这些版本可能指向新标签或新分支，而本地仓库副本尚未更新，因此无法识别它们。为此，应使用以下命令更新已经检出的仓库：

```console
$ vcs custom --args remote update
```

<span id="download-the-new-source-code"></span>

## 下载新源码

现在可以下载与新仓库列表对应的源码。

**Linux：**

```console
$ vcs import src < ros2.repos
$ vcs pull src
```

**macOS：**

```console
$ vcs import src < ros2.repos
$ vcs pull src
```

**Windows：**

在 Windows 命令行中运行：

```console
$ vcs import --input ros2.repos src
$ vcs pull src
```

或在 PowerShell 中运行：

```console
$ vcs import --input ros2.repos src
$ vcs pull src
```

<span id="rebuild-your-workspace"></span>

## 重新构建工作空间

工作空间现在已经包含最新源码。移除之前的安装，然后重新构建工作空间，例如：

```console
$ colcon build --symlink-install
```

<span id="inspect-your-source-checkout"></span>

## 检查源码工作副本

在开发过程中，工作空间可能已不再处于最初导入仓库列表时的状态。如果想了解工作空间内各个仓库的版本，可以使用以下命令导出相关信息。

**Linux：**

```console
$ cd ~/ros2_rolling
$ vcs export src > my_ros2.repos
```

**macOS：**

```console
$ cd ~/ros2_rolling
$ vcs export src > my_ros2.repos
```

**Windows：**

```console
$ cd \dev\ros2_rolling
$ vcs export src > my_ros2.repos
```

随后可以将 `my_ros2.repos` 文件分享给其他人，让他们复现你的工作空间中各个仓库的状态。
