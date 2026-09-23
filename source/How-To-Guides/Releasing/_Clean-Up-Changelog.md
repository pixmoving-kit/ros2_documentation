在编辑器中打开所有 `CHANGELOG.rst` 文件。
你会看到 `catkin_generate_changelog` 根据提交说明自动生成了一个 `Forthcoming` 小节：

```rst
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Changelog for package your_package
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Forthcoming
-----------
* you can modify this commit message
* and this
```

整理这些提交说明，简明描述软件包自上次发布以来的重要变化，然后**提交所有 `CHANGELOG.rst` 文件**。
不要修改 `Forthcoming` 标题。
