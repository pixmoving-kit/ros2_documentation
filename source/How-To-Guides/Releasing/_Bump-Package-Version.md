软件包每次发布都必须使用唯一的版本号，而且版本号必须高于上一次发布。

运行：

```console
$ catkin_prepare_release
```

该命令将执行以下操作：

1. 更新 `package.xml` 中的软件包版本号。
2. 将 `CHANGELOG.rst` 中的 `Forthcoming` 标题替换为 `version (date)`，例如 `0.0.1 (2022-01-08)`。
3. 提交这些修改。
4. 创建标签，例如 `0.0.1`。
5. 将修改和标签推送到远程仓库。

!!! note "说明"

    默认递增补丁版本号，例如从 `0.0.0` 更新到 `0.0.1`。
    如果要递增次版本号或主版本号，请分别运行 `catkin_prepare_release --bump minor` 或 `catkin_prepare_release --bump major`。
    更多信息见 `catkin_prepare_release --help`。

!!! note "说明"

    如果仓库设置了严格的合并规则，例如 `Require a pull request before merging`，就不能直接向该分支推送修改，需要为 `catkin_prepare_release` 生成的修改和标签创建拉取请求，再进行合并。
    某些合并方式（例如压缩合并或变基合并）可能改变版本提交的 SHA。
    在这种情况下，需要在合并后手动重新为版本提交打标签，确保标签指向正确的提交。
