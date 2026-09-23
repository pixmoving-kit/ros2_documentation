<span id="the-build-system"></span>

# 构建系统

构建系统让开发者能够按需构建 ROS 2 代码。ROS 2 广泛采用软件包来组织代码，每个软件包都包含一个清单文件（`package.xml`）。清单记录软件包的关键元数据，包括它对其他软件包的依赖关系，是元构建工具正常工作的必要条件。

ROS 2 构建系统包含三个主要概念。

<span id="build-tool"></span>

## 构建工具

构建工具负责控制单个软件包的编译和测试。在 ROS 2 中，C++ 通常使用 CMake，Python 通常使用 setuptools，也支持其他构建工具。

<span id="build-helpers"></span>

## 构建辅助工具

构建辅助工具提供与构建工具集成的辅助函数，改善开发体验。ROS 2 软件包通常依赖 `ament` 系列软件包来实现这些功能。`ament` 由几个重要仓库组成，均位于 [ament GitHub 组织](https://github.com/ament)中。

<span id="the-ament-package-package"></span>

### `ament_package` 软件包

[ament/ament_package](https://github.com/ament/ament_package) 仓库包含一个 [ament Python 软件包](#term-ament-Python-package)，为 ament 软件包提供多种实用工具，例如环境钩子的模板。

无论底层采用哪种构建系统，所有 ament 软件包都必须在软件包根目录下包含一个 `package.xml` 文件。这个清单文件提供处理和使用软件包所需的信息，包括全局唯一的软件包名称以及依赖关系等。它还充当标记文件，用于确定软件包在文件系统中的位置。

与 ROS 1 一样，`package.xml` 文件由 `catkin_pkg` 解析；`colcon` 等构建工具则通过在文件系统中搜索这些文件来查找软件包。

<span id="term-package.xml"></span>

**package.xml**：软件包清单文件，标识软件包的根目录，并记录名称、版本、描述、维护者、许可证、依赖关系等元数据。清单使用机器可读的 XML 格式，其内容由 [REP 127](https://reps.openrobotics.org/rep-0127/) 和 [REP 140](https://reps.openrobotics.org/rep-0140/) 定义，未来的 REP 可能会进一步修改这些规定。

因此，称某个软件包为 *ament 软件包*，意味着它是一个通过 `package.xml` 清单描述的独立软件单元，包含源代码、构建文件、测试、文档及其他资源。

<span id="term-ament-package"></span>

**ament 软件包**：包含 `package.xml` 并遵循 `ament` 软件包规范的任何软件包，与底层构建系统无关。

由于 ament 软件包这一概念不依赖特定构建系统，因此可以分为 ament CMake 软件包、ament Python 软件包等不同类型。本软件栈中常见的软件包类型如下。

<span id="term-CMake-package"></span>

**CMake 软件包**：包含普通 CMake 项目和 `package.xml` 清单文件的软件包。

<span id="term-ament-CMake-package"></span>

**ament CMake 软件包**：同时遵循 `ament` 软件包规范的 CMake 软件包。

<span id="term-Python-package"></span>

**Python 软件包**：包含基于 [setuptools](https://pypi.org/project/setuptools/) 的 Python 项目和 `package.xml` 清单文件的软件包。

<span id="term-ament-Python-package"></span>

**ament Python 软件包**：同时遵循 `ament` 软件包规范的 Python 软件包。

<span id="the-ament-cmake-repository"></span>

### `ament_cmake` 仓库

[ament/ament_cmake](https://github.com/ament/ament_cmake) 仓库包含许多 ament CMake 软件包和纯 CMake 软件包，提供创建 ament CMake 软件包所需的 CMake 基础设施。这里的 ament CMake 软件包，是指使用 CMake 构建的 `ament` 软件包。因此，这个仓库提供必要的 CMake 函数、宏和模块，便于创建更多 ament CMake（或 `ament_cmake`）软件包。这类软件包通过 `package.xml` 的 `<export>` 标签中的 `<build_type>ament_cmake</build_type>` 标签标识。

仓库中的软件包高度模块化，同时提供一个名为 `ament_cmake` 的统一入口软件包。只需依赖它，即可使用仓库中各个软件包汇总提供的功能。主要软件包如下：

| 软件包 | 功能 |
| --- | --- |
| `ament_cmake` | 汇总仓库中其他软件包的功能，用户只需依赖此包。 |
| `ament_cmake_auto` | 提供便捷的 CMake 函数，自动处理编写 `CMakeLists.txt` 时的许多繁琐工作。 |
| `ament_cmake_core` | 提供 `ament` 的核心机制，如环境钩子、资源索引和符号链接安装。 |
| `ament_cmake_gmock` | 提供编写基于 gmock 的单元测试的便捷函数。 |
| `ament_cmake_gtest` | 提供编写基于 gtest 的自动化测试的便捷函数。 |
| `ament_cmake_nose` | 提供编写基于 nosetests 的 Python 自动化测试的便捷函数。 |
| `ament_cmake_python` | 为包含 Python 代码的软件包提供 CMake 函数，参见 [ament_cmake_python 用户文档](../../How-To-Guides/Ament-CMake-Python-Documentation.md)。 |
| `ament_cmake_test` | 使用 [CTest](https://cmake.org/Wiki/CMake/Testing_With_CTest)，将 gtest、nosetests 等不同测试汇总到同一个目标下。 |

`ament_cmake_core` 提供大量 CMake 基础设施，使软件包能够通过约定的接口清晰地传递信息。这降低了软件包之间构建接口的耦合，促进代码复用，也使不同软件包的构建系统遵循共同约定。例如，它提供传递头文件目录、库、定义和依赖关系的标准方式，让使用这些信息的软件包可以用一致的方法获取它们。

`ament_cmake_core` 还提供符号链接安装等 `ament` 构建系统功能：将源空间或构建空间中的文件以符号链接的形式放入安装空间，而不是复制文件。这样，只需安装一次，之后编辑 Python 代码、配置文件等非生成资源就能直接生效，无需再次执行安装步骤。这个功能实际上替代了 `catkin` 的开发空间（devel space），保留了其中大部分优点，同时减少了复杂性和缺点。

软件包资源索引是 `ament_cmake_core` 的另一项功能，使软件包能够声明自己包含某类资源。它的设计使一些简单查询更加高效，例如查询某个前缀（如 `/usr/local`）下有哪些软件包时，只需列出该前缀下一个固定位置的文件。详情参见[资源索引设计文档](https://github.com/ament/ament_cmake/blob/rolling/ament_cmake_core/doc/resource_index.md)。

与 `catkin` 一样，`ament_cmake_core` 还提供环境设置文件和软件包专用的环境钩子。环境设置文件通常名为 `setup.bash` 等，供开发者定义使用软件包所需的环境修改。开发者通过“环境钩子”实现这些修改；环境钩子本质上是一段 shell 代码，可以设置或修改环境变量、定义 shell 函数、设置自动补全规则等。例如，ROS 1 正是通过这个机制设置 `ROS_DISTRO` 环境变量，而无需让 `catkin` 了解 ROS 发行版。

<span id="the-ament-lint-repository"></span>

### `ament_lint` 仓库

[ament/ament_lint](https://github.com/ament/ament_lint) 仓库包含多个软件包，以便捷且一致的方式提供代码检查和测试服务。目前支持使用 `uncrustify` 检查 C++ 代码风格、使用 `cppcheck` 进行 C++ 静态检查、检查源代码中的版权声明，以及使用 `pep8` 检查 Python 代码风格等。将来可能会加入更多辅助软件包。

<span id="meta-build-tool"></span>

## 元构建工具

元构建工具能够根据依赖关系对一组软件包进行拓扑排序，再按照正确的顺序构建或测试它们。它会调用各软件包的构建工具，完成实际的编译、测试和安装工作。

ROS 2 使用的元构建工具是 [colcon](https://colcon.readthedocs.io/en/released/)。
