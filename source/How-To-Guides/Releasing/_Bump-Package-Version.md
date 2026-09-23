---
translation_status: machine_translated
source: How-To-Guides/Releasing/_Bump-Package-Version.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

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
