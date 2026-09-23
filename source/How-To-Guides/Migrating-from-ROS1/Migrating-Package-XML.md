<span id="migrating-your-package-xml-to-format-2"></span>
# 将 package.xml 迁移到格式 2

ROS 2 要求 `package.xml` 至少使用[格式 2](https://reps.openrobotics.org/rep-0140/)。本指南介绍如何将 `package.xml` 从格式 1 迁移到格式 2。

如果文件开头的 `<package>` 标签如下所示，就表示它使用格式 1，必须迁移：

```xml
<package>
```

```xml
<package format="1">
```

<span id="prerequisites"></span>
## 前提条件

应已安装能够正常工作的 ROS 1。由于 ROS 1 支持所有 `package.xml` 格式版本，可以通过构建和测试软件包，验证转换后的文件是否有效。

<span id="migrate-from-format-1-to-2"></span>
## 从格式 1 迁移到格式 2

格式 1 和格式 2 指定依赖项的方式有所不同，差异概述见 [REP-0140 的兼容性章节](https://reps.openrobotics.org/rep-0140/#compatibility)。

<span id="add-format-attribute-to-package"></span>
### 为 `<package>` 添加 format 属性

添加 `format` 属性并将其设为 `2`，或将已有属性改为 `2`，表明 `package.xml` 使用格式 2：

```xml
<package format="2">
```

<span id="replace-run-depend"></span>
### 替换 `<run_depend>`

格式 2 不再允许使用 `<run_depend>`。如果有以下依赖声明：

```xml
<run_depend>foo</run_depend>
```

请将其替换为以下一个或两个标签：

```xml
<build_export_depend>foo</build_export_depend>
<exec_depend>foo</exec_depend>
```

如果运行软件包中的内容时需要该依赖，请使用 `<exec_depend>`。如果其他依赖此软件包的包在构建时也需要该依赖，请使用 `<build_export_depend>`。不确定时，可以同时使用两个标签。

<span id="convert-some-build-depend-to-test-depend"></span>
### 将部分 `<build_depend>` 改为 `<test_depend>`

在格式 1 中，`<test_depend>` 声明运行软件包测试所需的依赖。在格式 2 中，它还用于声明构建测试所需的依赖。

由于格式 1 的限制，软件包可能通过 `<build_depend>` 声明仅用于测试的依赖，例如：

```xml
<build_depend>testfoo</build_depend>
```

此时应将其改为 `<test_depend>`：

```xml
<test_depend>testfoo</test_depend>
```

!!! note "说明"
    如果使用 CMake，确保只在 `if(BUILD_TESTING)` 块中引用测试依赖：

    ```cmake
    if (BUILD_TESTING)
        find_package(testfoo REQUIRED)
    endif()
    ```

<span id="begin-using-doc-depend"></span>
### 使用 `<doc_depend>`

使用新增的 `<doc_depend>` 标签，声明构建软件包文档所需的依赖。例如，C++ 软件包可能有以下依赖：

```xml
<doc_depend>doxygen</doc_depend>
```

Python 软件包则可能有：

```xml
<doc_depend>python3-sphinx</doc_depend>
```

更多信息见[为 ROS 2 软件包编写文档的指南](../Documenting-a-ROS-2-Package.md)。

<span id="simplify-dependencies-with-depend"></span>
### 使用 `<depend>` 简化依赖声明

新增的 `<depend>` 标签可以使 `package.xml` 更简洁。如果同一个依赖使用了以下三个标签：

```xml
<build_depend>foo</build_depend>
<build_export_depend>foo</build_export_depend>
<exec_depend>foo</exec_depend>
```

可以将它们替换为一个 `<depend>`：

```xml
<depend>foo</depend>
```

<span id="test-your-new-package-xml"></span>
## 测试新的 package.xml

像平时一样，使用 `catkin_make`、`cakin_make_isolated` 或 `catkin` 构建工具构建并测试软件包。如果全部成功，就说明新的 `package.xml` 有效。
