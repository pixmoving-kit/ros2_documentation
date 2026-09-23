---
translation_status: machine_translated
source: The-ROS2-Project/Contributing/Source-Control-Best-Practices.rst
---

<span id="source-control-best-practices"></span>

# 版本控制最佳实践

<span id="introduction"></span>

## 导言

本页着重介绍在为ROS 2项目作出贡献时通常遇到的源控制因素,并非要替换Git文档,而是侧重于ROS特定建议和酌情提及外部资源。

<span id="avoid-committing-generated-and-temporary-files"></span>

## 避免承诺生成的文件和临时文件

ROS 2工作空间生成一般不应致力于源控的建筑文物.

<span id="sources-of-generated-temporary-and-backup-files"></span>

### 生成、临时和备份文件的来源

<span id="colcon-workspace-artifacts"></span>

#### Colcon 工作空间文物

常见的工作空间文物包括:

``` text
build/
install/
log/
```

那个... `build/`, `install/`,以及 `log/` 目录由 `colcon build` 作为ROS 2 工作空间的一部分。这些目录通常是在工作空间层面创建的,而不是在单个寄存器内部创建的。

如果这些目录出现在寄存器内( 例如不小心调用) `colcon build` 在错误的目录中,它们不应被承诺用于源控。

<span id="python-generated-files"></span>

#### Python 生成的文件

Python 生成若干临时文件,包括 `__pycache__/` 目录和文件结束于 `*.pyc`, `*.pyo`,以及 `*.pyd` 当您运行此程序时, 这些文件都不应该被送入存储器 。

<span id="ide-configuration-files"></span>

#### IDE 配置文件

编辑器和IDE经常生成项目专用或用户专用的配置文件.

常见的例子包括:

``` text
.vscode/
.idea/
```

许多ROS 2 寄存器忽略了编辑器特定文件, 例如 `.vscode/` 财务报告和财务报告 `.idea/`. 贡献者应避免承担个人编辑配置,除非存储器明确要求这样做。

<span id="editor-temporary-and-backup-files"></span>

#### 编辑器临时文件和备份文件

Vim等一些编辑器创建了以结束的备份文件 `~` (例如, `test.py~`)和临时文件,以结束 `*.swp`, `*.swo`,并且类似。这些不应该被投入到存储器中。

<span id="operating-system-files"></span>

#### 操作系统文件

操作系统可能生成元数据文件,例如 `.DS_Store` (大型操作系统)和 `Thumbs.db` (Windows). 这些文件永远不应被送入寄存器.

执行更改前, 请检查任务中包含的文件 。 确保生成的文件和临时文件不被意外添加 。

<span id="gitignore-placement-options"></span>

## Gitignore 放置选项

<span id="repository-gitignore"></span>

### 仓库 `.gitignore`

在决定文件是否属于特定寄存器时使用以下缩略语规则 `.gitignore` 或全局范围 `.gitignore`.

如果文件将出现在每个开发者的计算机上, 请放在仓库中 `.gitignore`.

<span id="global-gitignore"></span>

### 全球 `.gitignore`

全球gitignore可以帮助防止意外执行与寄存器无关的文件,如操作系统元数据文件或个人编辑器配置.

Git 支持配置全局排除文件 `core.excludesfile` 配置选项。

例如:

``` console
$ git config --global core.excludesfile ~/.gitignore_global
```

全球吉提格诺尔有助于排除开发者机器或编辑器所特有的文件,这些文件并不打算投入到任何寄存器中。

<span id="credential-helpers"></span>

## 证书助手

在贡献ROS 2寄存器时,投存器会经常与Git主机服务如GitHub进行交互.

SSH 密钥和 Git 证书助手可以简化认证。 推动更改或与远程寄存器交互时, 避免重复输入证书 。

与其在此重复设置指令,请参考GitHub和Git官方文档中推荐的配置步骤.

<span id="developing-on-shared-robots"></span>

## 开发共享机器人

在共享的机器人或其他远程系统上开发时, 永远不要复制您的私人 SSH 密钥, 也永远不要运行远程 SSH 代理或证书助手, 可以缓存您的密钥并提供给他人。 相反, 在您的私人机器上使用本地 SSH 代理并使用 。 `ssh -A` 。使用转发的 SSH 代理使您的 SSH 密钥在远程机器上可用, 也使您不再重复输入您的 SSH 密钥密码句。 如果所有开发者都作为同一用户登录, 可能会有其他人重新使用您转发的密钥, 所以尝试限制在某个 `ssh -A` shell 到最小值。 请参考官方 Git SSH 文档以获取设置指令 。

如果远程机器被信任, 您也可以设置一个( 仅读) 的密钥或部署符号, 以便它能够访问您的私人重置器。 如果您需要从此机器中驱动, 请使用上面描述的 SSH 代理转发 。

<span id="additional-resources"></span>

## 追加资源

- [Git 忽略文档](https://git-scm.com/docs/gitignore)

- [GitHub gitignore 模板](https://github.com/github/gitignore)

- [GitHub SSH 文档](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

- [Git 证书存储文档](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage)

示例ROS 2 `.gitignore` 文件包括:

- [rclcpp](https://github.com/ros2/rclcpp/blob/rolling/.gitignore) (最小例子)

- [rviz 维兹](https://github.com/ros2/rviz/blob/rolling/.gitignore) (合理实例)

- [message_filters](https://github.com/ros2/message_filters/blob/rolling/.gitignore) (广例).
