---
translation_status: machine_translated
source: How-To-Guides/Releasing/_Personal-Access-Token.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

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
