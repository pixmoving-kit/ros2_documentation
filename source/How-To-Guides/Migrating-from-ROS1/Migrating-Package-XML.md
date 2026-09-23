---
translation_status: machine_translated
source: How-To-Guides/Migrating-from-ROS1/Migrating-Package-XML.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="migrating-your-package-xml-to-format-2"></span>

# 将 package.xml 迁移至格式 2

报告2要求 `package.xml` 至少要使用的文件 [格式 2](https://reps.openrobotics.org/rep-0140/)。本指南显示如何迁移 `package.xml` 从格式1到格式2。

如果说 `<package>` 标签在您的起始处 `package.xml` 看起来像下面的任何一个,然后它使用格式1,你必须迁移它。

``` xml
<package>
```

``` xml
<package format="1">
```

<span id="prerequisites"></span>

## 前提条件

您应该有一个工作 ROS 1 的安装。 这样您就可以检查转换的 `package.xml` 通过构建和测试软件包是有效的,因为ROS 1 支持所有 `package.xml` 格式版本。

<span id="migrate-from-format-1-to-2"></span>

## 从格式 1 移到 2

格式 1 和格式 2 如何指定依赖关系不同。请阅读 [REP-0140中的兼容性部分](https://reps.openrobotics.org/rep-0140/#compatibility) 以汇总差异。

<span id="add-format-attribute-to-package"></span>

### 添加 `format` 属性为 `<package>`

添加或设置 `format` 属性为 `2` 以表明 `package.xml` 使用格式2。

``` xml
<package format="2">
```

<span id="replace-run-depend"></span>

### 替换 `<run_depend>`

那个... `<run_depend>` 标记不再允许。如果您有这样的依赖性 :

``` xml
<run_depend>foo</run_depend>
```

然后用其中之一或两个标签替换:

``` xml
<build_export_depend>foo</build_export_depend>
<exec_depend>foo</exec_depend>
```

如果执行软件包中的某些内容时需要依赖,请使用 `<exec_depend>` 标签。如果依赖于您的软件包的软件包在构建时需要依赖性,则使用 `<build_export_depend>` 标记。如果不确定,则使用两个标记。

<span id="convert-some-build-depend-to-test-depend"></span>

### 转换一些 `<build_depend>` 改为: `<test_depend>`

格式 1 `<test_depend>` 声明在运行您的软件包测试时需要的依赖性。它仍然在格式2中这样做,但它还声明了构建您的软件包测试时需要的依赖性。

由于格式1中此标签的局限性, 您的软件包可能具有一个被指定为仅测试的依赖性 。 `<build_depend>` 像这样:

``` xml
<build_depend>testfoo</build_depend>
```

如果是,则改为a `<test_depend>`.

``` xml
<test_depend>testfoo</test_depend>
```

> **说明**
>
> 如果您正在使用 CMake , 请确保您的测试依赖性仅在一个内引用 `if(BUILD_TESTING)` 块 :
>
> ``` cmake
> if (BUILD_TESTING)
>     find_package(testfoo REQUIRED)
> endif()
> ```

<span id="begin-using-doc-depend"></span>

### 开始使用 `<doc_depend>`

使用新内容 `<doc_depend>` C++ 软件包可能具有此依赖性 :

``` xml
<doc_depend>doxygen</doc_depend>
```

而Python软件包可能会有这个:

``` xml
<doc_depend>python3-sphinx</doc_depend>
```

见 [记录 ROS 2 软件包的指南](../Documenting-a-ROS-2-Package.md) 以获取更多信息。

<span id="simplify-dependencies-with-depend"></span>

### 简化依附关系 `<depend>`

`<depend>` 是一个新标签,它使 `package.xml` 文档更为简洁。如果您是 `package.xml` 有这三种标记用于同一依赖性:

``` default
<build_depend>foo</build_depend>
<build_export_depend>foo</build_export_depend>
<exec_depend>foo</exec_depend>
```

然后用一个单数来代替它们。 `<depend>` 像这样:

``` xml
<depend>foo</depend>
```

<span id="test-your-new-package-xml"></span>

## 测试您的新 `package.xml`

构建和测试您通常使用的软件包 `catkin_make`, `cakin_make_isolated`,或 `catkin` 构建工具。如果一切都成功,那么您 `package.xml` 无效。
