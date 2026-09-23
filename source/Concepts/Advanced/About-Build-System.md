---
translation_status: machine_translated
source: Concepts/Advanced/About-Build-System.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="the-build-system"></span>

# 构建系统

构建系统是允许开发者根据需要构建ROS 2代码的原因. ROS 2在很大程度上依赖于将代码划分为包,每个包包含一个表列文件(professional file).`package.xml`。这个显示文件包含关于软件包的必要元数据,包括它对其他软件包的依赖性。这个显示文件是元构建工具发挥作用所需的。

ROS 2构建系统由3个主要概念组成.

<span id="build-tool"></span>

## 构建工具

这是控制单一软件包的编译和测试的软件。在ROS 2中,通常是C++的CMake,以及Python的设置工具,但支持其他构建工具。

<span id="build-helpers"></span>

## 构建助手

这些是连接到构建工具以提高开发者经验的帮助器功能。 ROS 2 包通常依赖于 `ament` 用于此的系列软件包。 `ament` 由几个重要的储存库组成,这些储存库都在 [GitHub 组织](https://github.com/ament).

<span id="the-ament-package-package"></span>

### 那个... `ament_package` 软件包

定位于( E) [GitHub](https://github.com/) 现时 [ament/ament_package](https://github.com/ament/ament_package),此寄存器包含一个单 [ament Python 软件包](#term-ament-Python-package) 提供各种公用设施 [邮包](#term-ament-package),例如环境钩的模板。

全部人员 [邮包](#term-ament-package) 必须包含单个 [package.xml](#term-package.xml) 文件位于软件包的根部,而不管其基本构建系统是什么。 [package.xml](#term-package.xml) “明显”文件载有处理和操作下列文件所需的信息: [软件包](../../Glossary.md#term-package)。这个 [软件包](../../Glossary.md#term-package) 信息包括诸如 [软件包](../../Glossary.md#term-package)其名称是全球独有的,也是软件包的依赖性。 [package.xml](#term-package.xml) 文件还充当显示位置的标记文件 [软件包](../../Glossary.md#term-package) 在文件系统中。

分析 [package.xml](#term-package.xml) 文件由 `catkin_pkg` (如ROS 1),同时定位的功能 [软件包](../../Glossary.md#term-package) 通过搜索文件系统查找这些 [package.xml](#term-package.xml) 文件由构建工具提供,例如: `colcon`.

<span id="term-package.xml"></span>package.xml  
标为根的软件包显示文件 [软件包](../../Glossary.md#term-package) 并包含关于 [软件包](../../Glossary.md#term-package) 包括名称、版本、描述、维护者、许可证、依赖性等等。 [区域方案](../../Glossary.md#term-REP) [127](https://reps.openrobotics.org/rep-0127/) 财务报告和财务报告 [140](https://reps.openrobotics.org/rep-0140/),并有可能在今后作进一步修改 [区域方案](../../Glossary.md#term-REP).

所以,随时随地 [软件包](../../Glossary.md#term-package) 称为 [邮包](#term-ament-package),这意味着它是一个单一的软件单元(源代码、构建文件、测试、文档和其他资源),使用一种软件进行描述。 [package.xml](#term-package.xml) 显示文件 。

<span id="term-ament-package"></span>邮包  
任意 [软件包](../../Glossary.md#term-package) 包含一个 [package.xml](#term-package.xml) 并遵循以下各项的包装准则: `ament`,无论基础建筑系统如何.

自该术语以来 [邮包](#term-ament-package) 是构建系统不可知论, 可能有不同的类型 [邮包](#term-ament-package), e.g. [ament CMake 软件包](#term-ament-CMake-package), [ament Python 软件包](#term-ament-Python-package), 等 (简体中文).

以下是您可能在此软件堆栈中遇到的常见软件包类型列表 :

<span id="term-CMake-package"></span>CMake 软件包  
任意 [软件包](../../Glossary.md#term-package) 包含一个简单的 CMake 工程和一个 [package.xml](#term-package.xml) 显示文件 。

<span id="term-ament-CMake-package"></span>ament CMake 软件包  
A [CMake 软件包](#term-CMake-package) 也沿用 `ament` 包装准则。

<span id="term-Python-package"></span>Python 软件包  
任意 [软件包](../../Glossary.md#term-package) 包含一个 [设置工具](https://pypi.org/project/setuptools/) 基于 Python 工程和 a [package.xml](#term-package.xml) 显示文件 。

<span id="term-ament-Python-package"></span>ament Python 软件包  
A [Python 软件包](#term-Python-package) 也沿用 `ament` 包装准则。

<span id="the-ament-cmake-repository"></span>

### 那个... `ament_cmake` 存储器

定位于( E) [GitHub](https://github.com/) 现时 [ament/ament_cmake](https://github.com/ament/ament_cmake),该寄存器中包含许多“ament CMake”和纯CMake软件包,这些软件包在CMake中提供了创建“ament CMake”软件包所需的基础设施。 `ament` 使用 CMake 构建的软件包。 [软件包](../../Glossary.md#term-package) 在这个寄存器中,提供必要的CMake函数/宏和CMake模块,以促进创建更多的“ament CMake”(或 `ament_cmake`() 软件包。这类软件包与 `<build_type>ament_cmake</build_type>` 标记在 `<export>` 标记 [package.xml](#term-package.xml) 文档。

那个... [软件包](../../Glossary.md#term-package) 在这个存储库中,是极其模块化的,但有一个单一的“瓶颈” [软件包](../../Glossary.md#term-package) 调用 `ament_cmake`任何人都可以依赖 `ament_cmake` [软件包](../../Glossary.md#term-package) 以获取全部汇总函数 [软件包](../../Glossary.md#term-package) 中。此处列出 [软件包](../../Glossary.md#term-package) 在储存库中加上一个简短的描述:

- `ament_cmake`

  - 所有其他总计 [软件包](../../Glossary.md#term-package) 在此寄存器中, 用户只需依赖此选项

- `ament_cmake_auto`

  - 提供方便 CMake 函数,可以自动处理写入 a 的许多乏味部分 [软件包](../../Glossary.md#term-package)’s `CMakeLists.txt` 文件

- `ament_cmake_core`

  - 提供所有内置核心概念 `ament`,例如环境钩子,资源索引,符号链接安装等

- `ament_cmake_gmock`

  - 添加用于进行基于gmock的单元测试的方便功能

- `ament_cmake_gtest`

  - 添加基于 gtest 的自动测试的便利功能

- `ament_cmake_nose`

  - 添加用于进行鼻检测的方便功能 Python 自动化测试

- `ament_cmake_python`

  - 提供 CMake 函数用于 [软件包](../../Glossary.md#term-package) 包含 Python 代码

  - 见 [ament_cmake_python 用户文档](../../How-To-Guides/Ament-CMake-Python-Documentation.md)

- `ament_cmake_test`

  - 在一个单一目标下,使用 [测试](https://cmake.org/Wiki/CMake/Testing_With_CTest)

那个... `ament_cmake_core` [软件包](../../Glossary.md#term-package) 包含许多 CMake 的基础设施, 从而可以清理在两者之间传递信息 [软件包](../../Glossary.md#term-package) 使用常规接口。 [软件包](../../Glossary.md#term-package) 拥有更多与其它连接的构建界面 [软件包](../../Glossary.md#term-package),促进其再利用,并鼓励在不同的建筑系统中订立公约。 [软件包](../../Glossary.md#term-package)例如,它提供了一个标准途径,用于通过目录、库、定义和相互依存关系。 [软件包](../../Glossary.md#term-package) 这样,这种信息的消费者就能够以传统的方式获取这种信息。

那个... `ament_cmake_core` [软件包](../../Glossary.md#term-package) 此外,还提供了《公约》的特征。 `ament` 构建像符号链接安装这样的系统, 它允许您象征性地将来自源空间或构建空间的文件链接到安装空间中, 而不是复制它们。 这样您就可以一次编辑 Python 代码和配置文件等非生成资源, 而无需重运行安装步骤使其生效 。 此特性基本上取代了“ 解码空间 ” 。 `catkin` 因为它有大部分优点 很少有复杂或缺点。

提供的另一个特征 `ament_cmake_core` 是那个 [软件包](../../Glossary.md#term-package) 资源索引,这是一种方法 [软件包](../../Glossary.md#term-package) 以表示它们包含某种类型的资源。此特性的设计使得回答简单问题的效率要高得多,例如: [软件包](../../Glossary.md#term-package) 位于此前缀( 例如) 。 `/usr/local`) 因为它只要求您在此前缀下以单个可能的位置列出文件。 您可以在其中读取更多关于此特性的内容 。 [设计文件](https://github.com/ament/ament_cmake/blob/rolling/ament_cmake_core/doc/resource_index.md) 用于资源索引。

喜欢 `catkin`, `ament_cmake_core` 也提供环境设置文件以及 [软件包](../../Glossary.md#term-package) 特定的环境钩。 环境设置文件, 经常命名类似的东西 。 `setup.bash`,是一个地方 [软件包](../../Glossary.md#term-package) 开发者将定义为利用环境而需要的环境变化 [软件包](../../Glossary.md#term-package)。开发者可以使用“环境钩”来做到这一点,它基本上是一个任意的 shell 代码的位点,可以设置或修改环境变量,定义 shell 函数,设置自动补全规则等. 例如,这是ROS 1 如何设置 `ROS_DISTRO` 无环境变量 `catkin` 了解ROS的发行情况

<span id="the-ament-lint-repository"></span>

### 那个... `ament_lint` 存储器

定位于( E) [GitHub](https://github.com/) 现时 [ament/ament_lint](https://github.com/ament/ament_lint),此寄存器提供了多个 [软件包](../../Glossary.md#term-package) 以方便和一致的方式提供衬里和测试服务。 [软件包](../../Glossary.md#term-package) 以支持 C++ 样式 `uncrustify`,静态 C++ 编码检查 `cppcheck`,检查源代码中的版权, Python 样式会使用 `pep8`等。帮助软件包清单在未来可能会增加。

<span id="meta-build-tool"></span>

## Meta 构建工具

这是一个软件,它知道如何在地形上订购一组软件包,并按正确的依赖顺序构建或测试它们。这个软件将调用“构建工具”来完成汇编、测试和安装软件包的实际工作。

在ROS 2中,这个工具命名为 [colcon](https://colcon.readthedocs.io/en/released/) 用于此目的。
