<span id="release-track"></span>
# 发布轨道

<span id="what-is-a-track"></span> <span id="id1"></span>
## 什么是发布轨道？

首次发布软件包时，Bloom 要求用户输入配置信息。将这些配置保存在发布仓库中，可以避免在后续发布时重复输入不会改变的设置。

针对不同 ROS 发行版发布软件包时，部分配置会有所不同，因此 bloom 使用**发布轨道（release track）分别保存各发行版的发布配置**。按照惯例，轨道名称应与目标 ROS 发行版名称相同。

所有发布轨道配置都保存在发布仓库 `master` 分支的 `tracks.yaml` 中。

<span id="track-configurations"></span>
## 轨道配置项

下面结合 bloom 的提示，详细说明各项轨道配置。

<span id="release-repository-url"></span> <span id="id2"></span>
### 发布仓库 URL

这是发布仓库的 URL。如果仓库托管在 ros2-gbp，地址应采用 `https://github.com/ros2-gbp/my_repo-release.git` 的形式。

```bash
No reasonable default release repository url could be determined from previous releases.
Release repository url [press enter to abort]:
```

粘贴发布仓库 URL，然后按 Enter。

Bloom 还可能询问是否初始化新仓库，如下所示：

```bash
Freshly initialized git repository detected.
An initial empty commit is going to be made.
Continue [Y/n]?
```

直接按 Enter，接受默认的 yes 即可。

<span id="repository-name"></span> <span id="id3"></span>
### 仓库名称

仓库名称可以自行选择，但建议使用项目名称。

```bash
Repository Name:
   upstream
      Default value, leave this as upstream if you are unsure
   <name>
      Name of the repository (used in the archive name)
   ['upstream']:
```

输入项目名称，例如 `my_project`，然后按 Enter。

<span id="upstream-repository-uri"></span> <span id="id4"></span>
### 上游仓库 URI

**上游仓库**是保存源码的仓库，通常是 GitHub 或 GitLab 等 Git 托管服务上的项目 HTTPS 链接。

```bash
Upstream Repository URI:
   <uri>
      Any valid URI. This variable can be templated, for example an svn url
      can be templated as such: "https://svn.foo.com/foo/tags/foo-:{version}"
      where the :{version} token will be replaced with the version for this release.
   [None]:
```

务必**使用 HTTPS 地址**，例如 `https://github.com/my_organization/my_repo.git`，不要使用 SSH 地址。

<span id="upstream-vcs-type"></span> <span id="id5"></span>
### 上游版本控制系统类型

这是[上游仓库 URI](#upstream-repository-uri)对应的版本控制系统（VCS）类型。必须从 `svn`、`git`、`hg` 或 `tar` 中选择仓库使用的类型。

```bash
Upstream VCS Type:
   svn
      Upstream URI is a svn repository
   git
      Upstream URI is a git repository
   hg
      Upstream URI is a hg repository
   tar
      Upstream URI is a tarball
   ['git']:
```

大多数仓库使用 Git，但部分旧仓库可能使用 Mercurial（hg）或 SVN。

<span id="version"></span> <span id="id6"></span>
### 版本

这是所发布软件包的版本，例如 `1.0.3`。

```bash
Version:
   :{ask}
      This means that the user will be prompted for the version each release.
      This also means that the upstream devel will be ignored.
   :{auto}
      This means the version will be guessed from the devel branch.
      This means that the devel branch must be set, the devel branch must exist,
      and there must be a valid package.xml in the upstream devel branch.
   <version>
      This will be the version used.
      It must be updated for each new upstream version.
   [':{auto}']:
```

设为 `:{auto}` 时，会根据开发分支中的 `package.xml` 自动确定版本。这是默认且推荐的设置。

设为 `:{ask}` 时，每次使用 bloom 发布都会提示输入版本。

<span id="release-tag"></span> <span id="id7"></span>
### 发布标签

发布标签指定要从哪个标签或分支导入代码。

```bash
Release Tag:
   :{version}
      This means that the release tag will match the :{version} tag.
      This can be further templated, for example: "foo-:{version}" or "v:{version}"

      This can describe any vcs reference. For git that means {tag, branch, hash},
      for hg that means {tag, branch, hash}, for svn that means a revision number.
      For tar this value doubles as the sub directory (if the repository is
      in foo/ of the tar ball, putting foo here will cause the contents of
      foo/ to be imported to upstream instead of foo itself).
   :{ask}
      This means the user will be prompted for the release tag on each release.
   :{none}
      For svn and tar only you can set the release tag to :{none}, so that
      it is ignored.  For svn this means no revision number is used.
   [':{version}']:
```

设为 `:{version}` 时，发布标签会与版本标签一致。这是默认且推荐的设置。

另一种较少使用的方式是设为分支名，这样每次发布时都会从上游项目拉取该分支。

如果希望每次发布都提示输入一个不同的标签，可以设为 `:{ask}`。当上游项目频繁发布带标签的新版本，而你希望每次发布时引用新的标签时，这个选项很有用。

<span id="upstream-devel-branch"></span> <span id="id8"></span>
### 上游开发分支

上游开发分支是[上游仓库](#upstream-repository-uri)中的分支名称。如果每个 ROS 发行版使用独立分支，那么各发布轨道中的这一字段也会不同。当[版本](#version)设为 `:{auto}` 时，会用此分支来确定待发布软件包的版本。

```bash
Upstream Devel Branch:
   <vcs reference>
      Branch in upstream repository on which to search for the version.
      This is used only when version is set to ':{auto}'.
   [None]:
```

要从名为 `rolling` 的分支发布，请输入 `rolling`。如果保留为 `None`，则会从仓库的默认分支确定版本，不推荐这样做。

<span id="ros-distro"></span> <span id="id9"></span>
### ROS 发行版

这是准备将软件包发布到的 ROS 发行版。

```bash
ROS Distro:
   <ROS distro>
      This can be any valid ROS distro, e.g. indigo, kinetic, lunar, melodic
   ['indigo']:
```

如果计划向 ROS Rolling 发布，请输入 `rolling`。

<span id="patches-directory"></span> <span id="id10"></span>
### 补丁目录

这是保存发布所需额外补丁的目录。

```bash
Patches Directory:
   <path in bloom branch>
      This can be any valid relative path in the bloom branch. The contents
      of this folder will be overlaid onto the upstream branch after each
      import-upstream.  Additionally, any package.xml files found in the
      overlay will have the :{version} string replaced with the current
      version being released.
   :{none}
      Use this if you want to disable overlaying of files.
   [None]:
```

为发布添加额外补丁是一项很少使用的功能。对几乎所有软件包而言，都应保留默认值 `None`。

<span id="release-repository-push-url"></span> <span id="id11"></span>
### 发布仓库推送 URL

```bash
Release Repository Push URL:
   :{none}
      This indicates that the default release url should be used.
   <url>
      (optional) Used when pushing to remote release repositories. This is only
      needed when the release uri which is in the rosdistro file is not writable.
      This is useful, for example, when a releaser would like to use a ssh url
      to push rather than a https:// url.
   [None]:
```

大多数情况下，可以保留默认值。
