---
translation_status: machine_translated
source: How-To-Guides/Using-Variants.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="using-variants"></span>

# 使用变体

Metapackages并不直接提供软件,而是依赖一组其他相关的软件包为完整组的软件包提供方便的安装机制. <span id="id1"></span>[\[1\]](#id3) <span id="id2"></span>[\[2\]](#id4) 变体是用于常用的ROS包组的官方元件组列表.

<span id="id3"></span>

\[[1](#id1)\]

<https://wiki.debian.org/metapackage>

<span id="id4"></span>

\[[2](#id2)\]

<https://help.ubuntu.com/community/MetaPackages>

ROS 2中的不同变体在 [REP-2001号](https://reps.openrobotics.org/rep-2001/).

除了官方的变体外,可能还有针对特定机构或机器人的元包,如: [REP-108号报告](https://reps.openrobotics.org/rep-0108/#institution-specific).

<span id="adding-variants"></span>

## 添加变体

可通过向《公约》秘书处提供更新资料,提出对《公约》秘书处社区具有普遍用途的其他变式。 [REP-2001 通过牵引请求](https://github.com/openrobotics/reps/blob/main/_posts/rep-2001.md) 说明新变体中包含的软件包。机构和机器人特定变体可以由各自的维护者直接发布,不需要更新REP-2001。

<span id="creating-project-specific-variants"></span>

## 创建特定项目的变体

如果您正在创建 ROS 软件包, 以便在自己的项目中私下使用, 您可以使用正式的变体作为示例来创建您的项目的特有变体。 要做到这一点, 您只需要创建两个文件 :

1.  最小的变体软件包作为软件包创建,其中包含 `ament_cmake` 构建类型, a `buildtool_depend` 打开 `ament_cmake` 财务报告和财务报告 `exec_depend` 用于变量中要包含的每个软件包的条目。

    ``` xml
    <?xml version="1.0"?>
    <?xml-model href="http://download.ros.org/schema/package_format2.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
    <package format="2">
      <name>my_project_variant</name>
      <version>1.0.0</version>
      <description>A package to aggregate all packages in my_project.</description>
      <maintainer email="maintainer-email">Maintainer Name</maintainer>
      <license>Apache License 2.0</license>
      <!-- packages in my_project -->
      <exec_depend>my_project_msgs</exec_depend>
      <exec_depend>my_project_services</exec_depend>
      <exec_depend>my_project_examples</exec_depend>

      <export>
        <build_type>ament_cmake</build_type>
      </export>
    </package>
    ```

2.  最小的 ament_cmake 包包括一个 `CMakeLists.txt` 它将包.xml注册为用于ROS 2的Ament包.

    ``` cmake
    cmake_minimum_required(VERSION 3.5)

    project(my_project_variant NONE)
    find_package(ament_cmake REQUIRED)
    ament_package()
    ```

然后您可以与其他私有软件包一起构建和安装您的变体软件包。

<span id="creating-custom-variants-with-platform-specific-tools"></span>

### 创建带有特定平台工具的自定义变体

有些平台有创建基本软件包的工具,不需要完整的ROS构建农场环境或等效的基础设施。可以使用这些工具创建平台依赖的变体。这种方法并不包括支持ROS包装工具,而是依赖平台,但如果您正在创建现有软件包的集合,而不是公共和私人ROS软件包的组合,则生产所需的基础设施要少得多。例如,在Debian或Ubuntu系统上,您可以使用该软件包。 `equivs` Debian行政长官的手册载有: [关于元包的一节](https://www.debian.org/doc/manuals/debian-handbook/sect.building-first-package.en.html#id-1.18.5.2).
