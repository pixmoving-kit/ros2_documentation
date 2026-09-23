---
translation_status: machine_translated
source: How-To-Guides/Releasing/Subsequent-Releases.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="subsequent-releases"></span>

# 后续发布

本指南解释了如何发布之前已经发布的ROS套件的新版本.

<span id="be-part-of-the-release-team"></span>

## 加入释放队

如果您不是已写入发布存储器的发布团队成员, 请跟随 [加入释放小组](Release-Team-Repository.md#join-a-release-team).

<span id="install-dependencies"></span>

## 安装依赖关系

安装您将在即将到来的步骤中根据您的平台使用的工具 :

##### db(例如Ubuntu) (中文(简体) ).

``` console
$ sudo apt install python3-bloom python3-catkin-pkg
```

##### RPM(例如,RHEL)

``` console
$ sudo dnf install python3-bloom python3-catkin_pkg
```

##### 其他人员

``` console
$ pip3 install -U bloom catkin_pkg
```

确定您已经初始化 :

``` console
$ sudo rosdep init
$ rosdep update
```

请注意, `rosdep init` 命令如果在过去已经初始化,则可能失败;这可以安全地忽略。

<span id="set-up-a-personal-access-token"></span>

## 设置个人访问托肯

> **警告**
>
> 如果文件 `~/.config/bloom` 存在于您的计算机上, 您很可能已经这样做了, 所以您应该跳过此区域 。

在发布过程中,会执行多个需要密码认证的 HTTPS Git 操作。为了避免被反复要求密码, a [个人访问托肯( PAT)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) 如果您在您的 GitHub 账户上设置了多要素认证设置, 您将会被设置 。 **必须** 设置个人访问Token。

通过 :

1.  登录到 GitHub 并前往 [个人访问令牌](https://github.com/settings/tokens).

2.  单击 **生成新符号** 按钮。

3.  在下拉时,选择 **生成新符号( 经典)**

4.  设定 **说明** {\fn黑体\fs20\shad2\2aH82\3aH20\4aH33\fscx95\3cH592001\be1}对类似的东西 `Bloom token`.

5.  设定 **过期** 改为: **无过期**.

6.  勾选 `public_repo` 财务报告和财务报告 `workflow` 复选框。

7.  单击 **生成符号** 按钮。

在你创造了这个标志之后,你会回到 *个人访问令牌* 页面。 **复制字母符号** 以绿色突出。

将您的 GitHub 用户名和 PAT 保存到新文件 `~/.config/bloom`,格式如下:

``` text
{
   "github_user": "<your-github-username>",
   "oauth_token": "<token-you-created-for-bloom>"
}
```

在您的配置中配置 `~/.gitconfig` 您的 GitHub 账户和 PAT 用于所有释放寄存器 [ros2-gbp 缩写](https://github.com/ros2-gbp):

``` ini
[credential "https://github.com/ros2-gbp"]
    username = x-access-token
    helper = "!f() { test \"$1\" = get && echo \"password=<token-you-created-for-bloom>\"; }; f"
```

您可以额外使用不同的 GitHub 账户和 PATs 单个释放寄存器 :

``` ini
[credential "https://github.com/ros2-gbp/my_package-release.git"]
    username = x-access-token
    helper = "!f() { test \"$1\" = get && echo \"password=<other-token-you-created-for-bloom>\"; }; f"
```

<span id="ensure-repositories-are-up-to-date"></span>

## 确保储存库的更新

确保:

- 您的寄存器在 GitHub 这样的远程上 。

- 您的计算机上有一个存储器的克隆, 并且位于正确的分支上 。

- 远程寄存器和您的克隆都是最新的.

<span id="updating-changelog"></span>

## 更新更改日志

对于您的用户和开发者,请保持更改日志的简洁和最新.

``` console
$ catkin_generate_changelog
```

全部打开 `CHANGELOG.rst` 在编辑器中保存文件。你会看到 `catkin_generate_changelog` 已自动生成包含承诺信件中注释的下一节 :

``` rst
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Changelog for package your_package
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Forthcoming
-----------
* you can modify this commit message
* and this
```

清理承诺信息清单,以简洁地传达自上次发布以来对软件包的显著变化,以及 **输入所有 ChangeGELOG.rst 文件 。** 不修改 `Forthcoming` 头曰.

<span id="bump-the-package-version"></span>

## 弹出软件包版本

软件包的每次发行都必须有一个比之前的发行版本要高的独特版本号.

运行 :

``` console
$ catkin_prepare_release
```

实施下列措施:

1.  增加软件包版本 `package.xml`

2.  替换标题 `Forthcoming` 与 `version (date)` (e.g. `0.0.1 (2022-01-08)`) 内 `CHANGELOG.rst`

3.  执行这些修改

4.  创建标签( 例如) 。 `0.0.1`)

5.  将更改和标签推到您的远程仓库

> **说明**
>
> 默认情况下,包的补丁版本会递增,例如从 `0.0.0` 改为: `0.0.1`。要递增小版本或主要版本,请运行 `catkin_prepare_release --bump minor` 或 时 间 `catkin_prepare_release --bump major`。详细情况见 `catkin_prepare_release --help`.

> **说明**
>
> 如果您的寄存器有严格的合并规则类似 `Require a pull request before merging`中,您需要创建一个拉动请求,其中包含由 `catkin_prepare_release` 然后合并,因为您无法直接向分支推进。根据您的仓库的拉动请求合并设置(如壁球合并或重新定位合并),合并拉动请求可能会改变版本的 SHA 承诺。在这种情况下,您需要在合并后手动重置版本的承诺,以确保标记点为正确的承诺。

<span id="bloom-release"></span>

## Bloom 发布

运行以下命令, 替换 `my_repo` 并使用软件包中的仓库名称:

``` console
$ bloom-release --rosdistro rolling my_repo
```

Bloom会自动为您创建一个牵引请求 [rostro 维基月球](https://github.com/ros/rosdistro).

> **说明**
>
> 默认情况下, 将打开源寄存器中的所有软件包 。 要有选择地阻止特定软件包的发布 。 `rolling`,添加时 `rolling.ignored` 创建文件到 `master` 释放寄存器的分支。在每个文件中,列出包的名称,每行一个,以阻断包的释放。 [rosidl- 释放](https://github.com/ros2-gbp/rosidl-release) 仓库可作为此配置的有用参考。

<span id="next-steps"></span>

## 下一个步骤

一旦您提交拉动请求, 通常在一两天内, rodistro 的维护者之一将会审查并合并您的拉动请求 。 如果您的包构建成功, 您的包将在24- 48 小时内在 **横向测试** 存储器, 您可以在其中 [测试您的预释二进制](../../Installation/Testing.md).

发行版的发行管理器大约每两到四周会手动将 ROS 测试内容同步到主 ROS 仓库中。 这是当您的软件包真正可供 ROS 社区的其他用户使用的时候。 要获得下一次同步( sync) 即将到来时的更新, 请订阅 。 [开放机器人演讲的包装和释放管理类别](https://discourse.openrobotics.org/c/ros/release/16).
