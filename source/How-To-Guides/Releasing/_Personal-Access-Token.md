!!! warning "注意"

    如果计算机上已经存在 `~/.config/bloom` 文件，你可能已经完成过这项配置，可以跳过本节。

发布过程中会执行多次需要密码认证的 HTTPS Git 操作。
为了避免反复输入密码，需要设置[个人访问令牌（PAT）](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)。
如果 GitHub 账户已启用多重身份验证，则**必须**设置个人访问令牌。

创建令牌的步骤如下：

1. 登录 GitHub，打开 [Personal access tokens](https://github.com/settings/tokens) 页面。
2. 点击 **Generate new token**。
3. 在下拉菜单中选择 **Generate new token (classic)**。
4. 在 **Note** 中填写说明，例如 `Bloom token`。
5. 将 **Expiration** 设置为 **No expiration**。
6. 勾选 `public_repo` 和 `workflow`。
7. 点击 **Generate token**。

创建成功后，页面会返回 *Personal access tokens*。
**复制以绿色高亮显示的字母数字令牌。**

新建 `~/.config/bloom` 文件，按以下格式保存 GitHub 用户名和 PAT：

```text
{
   "github_user": "<your-github-username>",
   "oauth_token": "<token-you-created-for-bloom>"
}
```

在 `~/.gitconfig` 中进行以下配置，使 [ros2-gbp](https://github.com/ros2-gbp) 下的所有发布仓库都使用该 GitHub 账户及 PAT：

```ini
[credential "https://github.com/ros2-gbp"]
    username = x-access-token
    helper = "!f() { test \"$1\" = get && echo \"password=<token-you-created-for-bloom>\"; }; f"
```

也可以为不同的发布仓库分别使用不同的 GitHub 账户和 PAT：

```ini
[credential "https://github.com/ros2-gbp/my_package-release.git"]
    username = x-access-token
    helper = "!f() { test \"$1\" = get && echo \"password=<other-token-you-created-for-bloom>\"; }; f"
```
