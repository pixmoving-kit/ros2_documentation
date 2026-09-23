---
translation_status: machine_translated
source: How-To-Guides/Releasing/Release-Track.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="release-track"></span>

# 发布轨道

<span id="what-is-a-track"></span> <span id="id1"></span>

## 什么是轨迹?

Bloom 要求用户在首次发布软件包时输入配置信息。 将此类配置存储在发布存储库中是有益的, 这样我们不必手动输入不会为后续发布而更改的配置 。

由于一些配置在为不同的ROS分配放出包时会有所不同,所以开花使用 **释放用于存储释放配置的音轨** 您应该按常规创建与您所释放的 ROS Distro 名称相同的音轨 。

所有放行轨道配置都存储在 `tracks.yaml` 在您发布存储器的主分支上。

<span id="track-configurations"></span>

## 音轨配置

音轨配置与开花时的提示有更详细的解释.

<span id="release-repository-url"></span> <span id="id2"></span>

### 释放仓库url

这是您放行仓库的url, 应该是形式 `https://github.com/ros2-gbp/my_repo-release.git` 如果您的发布寄存器被托管在 ros2- gbp 上。

``` bash
No reasonable default release repository url could be determined from previous releases.
Release repository url [press enter to abort]:
```

粘贴您的发布仓库 URL 并按 Enter 键。

Bloom 可能会另外询问您关于新寄存器初始化的问题, 如下:

``` bash
Freshly initialized git repository detected.
An initial empty commit is going to be made.
Continue [Y/n]?
```

只需按 Enter 即可接受默认为是 。

<span id="repository-name"></span> <span id="id3"></span>

### 仓库名称

寄存器的名称是微不足道的, 但建议将此设置为您的项目名称 。

``` bash
Repository Name:
   upstream
      Default value, leave this as upstream if you are unsure
   <name>
      Name of the repository (used in the archive name)
   ['upstream']:
```

键入您的工程名称( 例如 ) 。 `my_project`按 Enter 键。

<span id="upstream-repository-uri"></span> <span id="id4"></span>

### 上游存储器 URI

那个... **上游储存库** 是您源代码所在的寄存器。这最有可能是一个在 GitHub 或 GitLab 这样的 git 主机服务中托管您的项目的 https 链接。

``` bash
Upstream Repository URI:
   <uri>
      Any valid URI. This variable can be templated, for example an svn url
      can be templated as such: "https://svn.foo.com/foo/tags/foo-:{version}"
      where the :{version} token will be replaced with the version for this release.
   [None]:
```

一定要保证你 **使用 https 地址** (e.g. `https://github.com/my_organization/my_repo.git`而不是shsh地址。

<span id="upstream-vcs-type"></span> <span id="id5"></span>

### 上游 VCS 类型

这是 [上游存储器 URI](#id4)版本控制系统( VCS) 类型。 您必须指定您寄存器使用的 vc 类型, 从 `svn`, `git`, `hg` 或 时 间 `tar`.

``` bash
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

大多数寄存器将使用git,但一些遗留的寄存器可能使用hg或svn.

<span id="version"></span> <span id="id6"></span>

### 版本

这是您正在释放的软件包的版本 。 (例如 。) `1.0.3`)

``` bash
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

设为 `:{auto}` (默认,以及推荐的设置)会自动从devel分支的软件包.xml中确定版本.

设为 `:{ask}` 每次你开花发布时 都会迅速要求版本

<span id="release-tag"></span> <span id="id7"></span>

### 释放标签

释放标记是指您要导入代码的标记或分支 。

``` bash
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

设为 `:{version}` (默认,以及推荐的设置)将使发行标记与版本标记匹配.

一个不太常见的设置是将此设置为分支名称,在从上游项目释放时总是拉入该分支.

或者,如果每次发布时要被提示输入不同的标签,请输入 `:{ask}`. `:{ask}` 如果上游工程经常有标签发布, 且每次发布时您都想要参考新标签,

<span id="upstream-devel-branch"></span> <span id="id8"></span>

### 上游发展处

上游弯曲分支是您所在的分支的名称 [上游储存库](#upstream-repository-uri)。如果您为每个ROS分布使用单独的分支,则每个发布轨道的此字段将不同。用于确定您在发布时发布的软件包的版本。 [版本](#version) 设置为 `:{auto}`.

``` bash
Upstream Devel Branch:
   <vcs reference>
      Branch in upstream repository on which to search for the version.
      This is used only when version is set to ':{auto}'.
   [None]:
```

从一个叫"从树枝中释放"的 `rolling`输入 `rolling`。离开这个作为 `None` 将导致从您的寄存器的默认分支中确定版本( 不推荐此选项) 。

<span id="ros-distro"></span> <span id="id9"></span>

### ROS 地铁

这是您计划放入软件包的分布 。

``` bash
ROS Distro:
   <ROS distro>
      This can be any valid ROS distro, e.g. indigo, kinetic, lunar, melodic
   ['indigo']:
```

如果你计划放入ROS 滚动,请输入 `rolling`.

<span id="patches-directory"></span> <span id="id10"></span>

### 补丁目录

这是任何额外的补丁所在的目录 。

``` bash
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

添加额外的补丁到发布中是很少使用的功能。 对于几乎所有的软件包来说, 这应该留作默认值 `None`.

<span id="release-repository-push-url"></span> <span id="id11"></span>

### 释放仓库 Push URL

``` bash
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

在多数情况下,可留作默认值。
