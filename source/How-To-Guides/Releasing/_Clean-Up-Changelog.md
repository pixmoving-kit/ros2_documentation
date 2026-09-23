---
translation_status: machine_translated
source: How-To-Guides/Releasing/_Clean-Up-Changelog.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

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
