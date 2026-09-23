<span id="testing-your-code-with-the-ros-build-farm"></span>

# 使用 ROS 构建农场测试代码

[ROS 2 构建农场](https://build.ros2.org/)功能强大。除了生成二进制包，它还可以在拉取请求（PR）合并之前编译 ROS 软件包并运行全部测试，从而检查 PR。

需要满足四个前提条件：

- GitHub 用户 [@ros-pull-request-builder](https://github.com/ros-pull-request-builder)具有仓库访问权限。
- GitHub 仓库已配置 webhook。
- [软件包已被 rosdistro 收录](../../../How-To-Guides/Releasing/Index-Your-Packages.md)。
- `test_pull_requests` 标志设为 true。

<span id="github-access"></span>

## GitHub 访问权限

可以在 GitHub 组织层面授予 PR Builder 权限，也可以只授予其单个 GitHub 仓库的访问权限。

<span id="github-organization"></span>

### GitHub 组织

1. 打开 `https://github.com/orgs/%YOUR_ORG%/people`，将 `%YOUR_ORG%` 替换为相应组织名称。
2. 点击 `Invite Member`，输入 `ros-pull-request-builder`。

<span id="github-repository"></span>

### GitHub 仓库

1. 打开 `https://github.com/%YOUR_ORG%/%YOUR_REPO%/settings/access`，将占位符替换为相应组织和仓库名称。
2. 点击 `Add people`，输入 `ros-pull-request-builder`。
3. 为其选择 `Admin` 或 `Write` 角色，具体区别见下一节。

<span id="webhooks"></span>

## Webhook

如果向 `ros-pull-request-builder` 授予完整管理员权限，它会自动配置 webhook。

也可以只授予 **write** 权限，手动配置 webhook，从而不必提供完整管理员权限。

1. 打开 `https://github.com/%YOUR_ORG%/%YOUR_REPO%/settings/hooks/new`。
2. 将 Payload URL 设置为 `https://build.ros2.org/ghprbhook/`。
3. 勾选 `Let me select individual events.`、`Issue comments` 和 `Pull requests`。

<span id="test-pull-requests"></span>

## test_pull_requests

对于每个希望启用 PR 测试的 ROS 发行版，都必须在 [rosdistro](https://github.com/ros/rosdistro/) 对应部分启用 `test_pull_requests` 标志。

- **方法 1**：运行 [bloom](../../../How-To-Guides/Releasing/Releasing-a-Package.md) 时选择启用 PR 测试。
- **方法 2**：**谨慎地**手动编辑 rosdistro 仓库中的相应文件，然后提交新的 PR。参见[示例](https://github.com/ros/rosdistro/blob/3c295f76b0755989e9ed526c0b5f28a5f6a94da3/rolling/distribution.yaml#L4708)及 [REP 143 文档](http://docs.ros.org/en/independent/api/rep/html/rep-0143.html#distribution-file)。

注意，添加 PR 后，通常要等到 Jenkins 夜间重新配置时才会创建相应任务。
