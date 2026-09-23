<span id="subsequent-releases"></span>

# 后续发布

本指南介绍如何为已经发布过的 ROS 软件包发布新版本。

<span id="be-part-of-the-release-team"></span>

## 加入发布团队

如果你尚未加入具有发布仓库写入权限的团队，请按照[加入发布团队](Release-Team-Repository.md#join-a-release-team)中的说明操作。

<span id="install-dependencies"></span>

## 安装依赖

参阅[安装发布所需的依赖](_Install-Dependencies.md)。

<span id="set-up-a-personal-access-token"></span>

## 设置个人访问令牌

参阅[配置个人访问令牌](_Personal-Access-Token.md)。

<span id="ensure-repositories-are-up-to-date"></span>

## 确保仓库为最新状态

参阅[更新仓库](_Ensure-Repositories-Are-Up-To-Date.md)。

<span id="updating-changelog"></span>

## 更新变更日志

为了方便用户和开发者，请保持变更日志简明且及时更新。

```console
$ catkin_generate_changelog
```

然后按照[整理变更日志](_Clean-Up-Changelog.md)中的说明操作。

<span id="bump-the-package-version"></span>

## 更新软件包版本号

参阅[更新软件包版本号](_Bump-Package-Version.md)。

<span id="bloom-release"></span>

## 使用 Bloom 发布

运行以下命令，将 `my_repo` 替换为存放软件包的仓库名称：

```console
$ bloom-release --rosdistro rolling my_repo
```

Bloom 会自动向 [rosdistro](https://github.com/ros/rosdistro) 仓库创建拉取请求。

!!! note "说明"

    默认情况下，Bloom 会发布源码仓库中的全部软件包。
    如果要阻止某些软件包发布到特定发行版，请在发布仓库的 `master` 分支中添加名为 `{DISTRO}.ignored` 的文件，其中 `{DISTRO}` 为发行版名称。
    在文件中逐行列出不应发布的软件包名称，每行一个。
    可以参考 [rosidl-release](https://github.com/ros2-gbp/rosidl-release) 仓库中的配置。

<span id="next-steps"></span>

## 后续步骤

参阅[发布后的后续步骤](_Next-Steps.md)。
