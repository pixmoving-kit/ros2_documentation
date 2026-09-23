---
translation_status: machine_translated
source: How-To-Guides/Ament-CMake-Documentation.rst
---

<span id="ament-cmake-user-documentation"></span>

# ament_cmake 用户文档

`ament_cmake` 是ROS 2 中基于 CMake 软件包的构建系统( 特别是将用于大多数 C/ C++ 项目)。 它是一组增强 CMake 和为软件包作者添加方便功能的脚本。在使用前 `ament_cmake`了解这些基本知识很有帮助 [CMake](https://cmake.org/cmake/help/v3.8/)。可以找到一个正式的教程 [这儿](https://cmake.org/cmake/help/latest/guide/tutorial/index.html).

<span id="basics"></span>

## 基本情况

使用 CMake 基本大纲可制作 `ros2 pkg create <package_name>` 然后将构建信息收集到两个文件中: `package.xml` 页:1 `CMakeLists.txt`,该目录必须在同一目录中。 `package.xml` 必须在 CI 中安装所有依赖和一些元数据,以便 colcon 为您的软件包找到正确的构建顺序, 安装所需的依赖性, 并提供发布信息 `bloom`。该词 `CMakeLists.txt` 包含用于构建和包装可执行文件及库的命令,并将是本文件的主要焦点。

<span id="basic-project-outline"></span>

### 基本项目纲要

《公约》的基本纲要 `CMakeLists.txt` 包件中包含:

``` cmake
cmake_minimum_required(VERSION 3.8)
project(my_project)

ament_package()
```

论点 `project` 将是软件包名称,必须与软件包名称完全相同。 `package.xml`.

项目设置由下列人员完成: `ament_package()` 并且这个电话必须 准确一次每个包。 `ament_package()` 安装 `package.xml`,将软件包与 Ament 索引一起注册,并为 CMake 安装配置文件(以及可能的目标),以便其他软件包使用 `find_package`。自 `ament_package()` 收集了大量来自 `CMakeLists.txt` 这应该是你最后的电话了 `CMakeLists.txt`.

`ament_package` 还可以给出其他参数:

- `CONFIG_EXTRAS`: CMake 文件列表 (`.cmake` 或 时 间 `.cmake.in` 模板已扩大 `configure_file()`用于软件包客户的软件包。关于何时使用这些参数的例子,请参见 [添加资源](#adding-resources)。关于如何使用模板文件的更多信息,参见 [正式文件](https://cmake.org/cmake/help/v3.8/command/configure_file.html).

- `CONFIG_EXTRAS_POST`: 与 `CONFIG_EXTRAS`,但文件添加的顺序不同。 `CONFIG_EXTRAS` 文件包含在生成的文件之前 `ament_export_*` 调用文件从 `CONFIG_EXTRAS_POST` 其后列入。

而不是添加到 `ament_package`,也可以添加到变量中 `${PROJECT_NAME}_CONFIG_EXTRAS` 财务报告和财务报告 `${PROJECT_NAME}_CONFIG_EXTRAS_POST` 具有相同效果。唯一的区别是文件添加顺序,总顺序如下:

- 添加的文件由 `CONFIG_EXTRAS`

- 文件通过附加添加到 `${PROJECT_NAME}_CONFIG_EXTRAS`

- 文件通过附加添加到 `${PROJECT_NAME}_CONFIG_EXTRAS_POST`

- 添加的文件由 `CONFIG_EXTRAS_POST`

<span id="compiler-and-linker-options"></span>

### 编译器和链接选项

ROS 2 目标编译器符合 C++17 和 C99 标准。 未来可能会针对更新型的编译器,并被引用 。 [这儿](https://reps.openrobotics.org/rep-2000/)因此,习惯上设置相应的CMake旗号:

``` cmake
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()
```

为了保持代码的清洁,编译器应当为有问题的代码发出警告,这些警告应当被固定.

建议至少达到下列预警水平:

- 对于视觉工作室:默认值 `W1` 警告

- 对于海合会和Clang: `-Wall -Wextra -Wpedantic` 高度建议 `-Wshadow` 这是可取的

目前建议使用 `add_compile_options` 用于添加所有目标的这些选项。这可以避免将代码与基于目标的编译选项混为一谈,用于所有可执行文件、库和测试:

``` cmake
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()
```

<span id="finding-dependencies"></span>

### 查找依赖关系

多数 `ament_cmake` 项目将依赖其他软件包。在 CMake 中,通过调用 `find_package`。例如,如果您的软件包依赖于 `rclcpp`,然后是 `CMakeLists.txt` 文件应包含:

``` cmake
find_package(rclcpp REQUIRED)
```

> **说明**
>
> 永远没有必要 `find_package` 库,它不是明确需要的,而是明确需要的另一种依赖的依赖。如果是这种情况,请对照相应的软件包提交错误。

<span id="adding-targets"></span>

### 添加目标

在CMake术语中, `targets` 属于此工程将创建的文物。可以创建库或可执行文件,而单个项目可以包含零或其中多个。

##### 库

这些是用呼叫来创建的 `add_library`,其中应同时包含目标名称和为创建库而应当编译的源文件。

随着C/C++中信头文件与执行的分离,通常不需要添加信头文件作为参数到 `add_library`.

提议采用下列最佳做法:

- 将本库客户端应使用的所有信头(因此必须安装)放入该库的子目录中。 `include` 类似软件包命名的文件夹, 而所有其他文件( Q)`.c/.cpp` 和不应导出的标题文件) `src` 文件夹

- 仅 `.c/.cpp` 在调用时明确引用文件 `add_library`

- 在您的库中查找信头 `my_library` 通过

``` cmake
target_include_directories(my_library
  PUBLIC
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
    "$<INSTALL_INTERFACE:include/${PROJECT_NAME}>")
```

此添加文件夹中的所有文件 `${CMAKE_CURRENT_SOURCE_DIR}/include` 在构建时间和包含文件夹中的所有文件(与 `${CMAKE_INSTALL_DIR}`安装时。

`ros2 pkg create` 创建一个遵循这些规则的软件包布局。

> **说明**
>
> 由于Windows是官方支持的平台之一,要产生最大的影响,任何软件包都应该建立在Windows上. Windows库格式执行符号的能见度;也就是说,每个应该从客户端中使用的符号都必须由库明确输出(符号需要隐含导入).
>
> 由于海合会和Clang建筑一般不这样做,因此建议使用下列逻辑: [海合会维基文库](https://gcc.gnu.org/wiki/Visibility)。用它来制作一个名为 `my_library`:
>
> - 将链接中的逻辑复制到一个名为标题的文件 `visibility_control.hpp`.
>
> - 替换 `DLL` 由 `MY_LIBRARY` (举例来说,见可见度控制 [rviz_rendering](https://github.com/ros2/rviz/blob/ros2/rviz_rendering/include/rviz_rendering/visibility_control.hpp)).
>
> - 对您要导出的所有符号( 即类或函数) 使用宏“ MY\_ LIBRARY\_ PUBLIC ” 。
>
> - 在项目中 `CMakeLists.txt` 使用 :
>
>   ``` cmake
>   target_compile_definitions(my_library PRIVATE "MY_LIBRARY_BUILDING_LIBRARY")
>   ```
>
> 详情见 [Windows 文档中的 Windows 符号可见度](../The-ROS2-Project/Contributing/Windows-Tips-and-Tricks.md#windows-symbol-visibility).

##### 可执行文件

建立这些机制时,应呼吁 `add_executable`,其中应当同时包含目标名称和为创建可执行文件而应当编译的源文件。可执行文件也可能需要通过使用此软件包中创建的任何库链接到其中。 `target_link_libraries`.

由于客户端一般不使用可执行文件作为库,所以无需将标题文件放入库中 `include` 目录。

如果一个软件包同时有库和可执行文件,则确保将上面“库”和“可执行文件”的建议结合起来。

<span id="linking-to-dependencies"></span>

### 与依附关系的联系

将目标与依赖联系起来有两种方式。

第一个和推荐的方法是使用ament macro `ament_target_dependencies`。举例来说,假设我们想链接 `my_library` 相对线性代数库 Eigen3.

``` cmake
find_package(Eigen3 REQUIRED)
ament_target_dependencies(my_library PUBLIC Eigen3)
```

它包括必要的头和库以及项目正确发现的附属物.

第二种方式是使用 `target_link_libraries`.

现代 CMake 偏好只使用目标,导出并与之对接。 CMake 目标可能带有命名空间, 类似于 C++ 。 如果有命名空间目标可用, 则倾向于使用它们。 例如 , `Eigen3` 定义目标 `Eigen3::Eigen`.

以Eigen3为例,电话应该看起来像

``` cmake
target_link_libraries(my_library PUBLIC Eigen3::Eigen)
```

这还将包括必要的信头、库及其附属关系。请注意,这种附属关系必须是以前通过呼叫找到的。 `find_package`.

<span id="installing"></span>

### 安装

##### 库

在建设可重复使用的库时,需要将一些信息输出给下游包,以方便使用.

首先, 安装客户端应该可用的信头文件。 包含的目录是支持覆盖的自定义 。 `colcon`; 见 <https://colcon.readthedocs.io/en/released/user/overriding-packages.html#install-headers-to-a-unique-include-directory> 以获取更多信息。

``` cmake
install(
  DIRECTORY include/
  DESTINATION include/${PROJECT_NAME}
)
```

接下来,安装目标并创建导出目标(`export_${PROJECT_NAME}`) 查找此软件包将使用其他代码。请注意,您可以使用单个代码。 `install` 调用以安装项目中的全部库 。

``` cmake
install(
  TARGETS my_library
  EXPORT export_${PROJECT_NAME}
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
)

ament_export_targets(export_${PROJECT_NAME} HAS_LIBRARY_TARGET)
ament_export_dependencies(some_dependency)
```

以下是上面的片段中发生的情况:

- 那个... `ament_export_targets` 宏导出 CMake 的目标。 这对于允许您的库客户端使用 `target_link_libraries(client PRIVATE my_library::my_library)` 语法。如果导出集包含库,请添加选项 `HAS_LIBRARY_TARGET` 改为: `ament_export_targets`,将潜在的库添加到环境变量中。

- 那个... `ament_export_dependencies` 导出对下游软件包的依赖性 。 这样做是必要的, 这样库的用户不必调用 `find_package` 对于这些依赖性,太。

> **警告**
>
> 调用 `ament_export_targets`, `ament_export_dependencies`,或 CMake 子目录中的其他命令不会如预期的那样起作用。这是因为 CMake 子目录无法在母目录中设置必要的变量,因为 `ament_package` 被唤作.

> **说明**
>
> Windows DLL 被作为运行时的文物处理并安装到 `RUNTIME DESTINATION` 文件夹。因此,建议保留该文件夹。 `RUNTIME` 安装,即使开发基于Unix的系统的库。

- 那个... `EXPORT` 安装呼叫的标记需要额外的注意: 它为 CMake 文件安装 `my_library` 目标。它必须与参数完全相同。 `ament_export_targets`。确保其能够通过下列途径加以使用: `ament_target_dependencies`,它不应该与库名完全相同,而是应该有一个像 `export_` (如上所示)。

- 所有安装路径都相对 `CMAKE_INSTALL_PREFIX`中,通过colcon/ament已经正确设置。

还有两种功能可供使用,但对于基于目标的安装来说是多余的:

``` cmake
ament_export_include_directories("include/${PROJECT_NAME}")
ament_export_libraries(my_library)
```

第一个宏标记导出目录包括目录。第二个宏标记已安装库的位置(由 `HAS_LIBRARY_TARGET` 调用时的参数 `ament_export_targets`。 只有当下游项目不能或不想使用基于CMake目标依赖性时,才会使用这些数据。

有些宏可以针对非目标出口采取不同类型的论据,但由于现代 Make 推荐的方法是使用目标,所以我们不会在这里覆盖这些选项,这些选项的文档可以在源代码本身中找到.

##### 可执行文件

当安装可执行文件时, 以下 stanza *一定要跟紧我* 其余ROS工具来找到它:

``` cmake
install(TARGETS my_exe
    DESTINATION lib/${PROJECT_NAME})
```

如果一个软件包同时有库和可执行文件,则确保将上面“库”和“可执行文件”的建议结合起来。

<span id="linting-and-testing"></span>

## 定点和测试

为了将测试与用colcon建库区分开来,将所有调用到linters和测试以一个条件:

``` cmake
if(BUILD_TESTING)
  find_package(ament_cmake_gtest REQUIRED)
  ament_add_gtest(<tests>)
endif()
```

<span id="linting"></span>

### 林廷( L)

他们建议使用来自 [ament_lint_auto](https://github.com/ament/ament_lint/blob/rolling/ament_lint_auto/doc/index.rst#ament_lint_auto):

``` cmake
find_package(ament_lint_auto REQUIRED)
ament_lint_auto_find_test_dependencies()
```

这将运行在 `package.xml`。建议使用套件定义的插件集 `ament_lint_common`。其中包含的单个插件及其功能,可见于 [ament_lint_常见文件](https://github.com/ament/ament_lint/blob/rolling/ament_lint_common/doc/index.rst).

Ament 提供的 Linters 也可以单独添加, 而不是运行 `ament_lint_auto`。可找到一个实例,说明如何这样做。 [ament_cmake_lint_cmake 文档](https://github.com/ament/ament_lint/blob/rolling/ament_cmake_lint_cmake/doc/index.rst).

<span id="testing"></span>

### 测试

Ament 包含 CMake 宏以简化 GTests 的设置。 call:

``` cmake
find_package(ament_cmake_gtest)
ament_add_gtest(some_test <test_sources>)
```

添加 GTest 。然后,它就是一个可与其他库(如项目库)链接的常规目标。宏有其他参数:

- `APPEND_ENV`: 附加环境变量。 例如, 您可以调用 :

``` cmake
find_package(ament_cmake_gtest REQUIRED)
ament_add_gtest(some_test <test_sources>
  APPEND_ENV PATH=some/additional/path/for/testing/resources)
```

- `APPEND_LIBRARY_DIRS`: 附加库,以便可以在运行时被链接器找到。这可以通过设置环境变量实现,如 `PATH` 窗口和 `LD_LIBRARY_PATH` 在Linux上,但这使得呼叫平台具有特定性.

- `ENV`: 设置环境变量(语法与 `APPEND_ENV`).

- `TIMEOUT`: 在第二位设置测试超时。 GTestes 的默认值为 60 秒。 例如 :

``` cmake
ament_add_gtest(some_test <test_sources> TIMEOUT 120)
```

- `SKIP_TEST`: 跳过此测试( 在控制台输出中显示为“ 通过” ) 。

- `SKIP_LINKING_MAIN_LIBRARIES`:不要与GTest联系。

- `WORKING_DIRECTORY`:设置测试工作目录.

否则默认工作目录是 `CMAKE_CURRENT_BINARY_DIR`,在表格中描述。 [CMake 文档](https://cmake.org/cmake/help/latest/variable/CMAKE_CURRENT_BINARY_DIR.html).

同样,还有一个CMake宏来设置GTest,包括GMock:

``` cmake
find_package(ament_cmake_gmock REQUIRED)
ament_add_gmock(some_test <test_sources>)
```

其附加参数与 `ament_add_gtest`.

<span id="extending-ament"></span>

## 展 展 开

可以将额外的宏/功能登记到 `ament_cmake` 并用若干种方法加以扩展。

<span id="adding-a-function-macro-to-ament"></span>

### 将函数/宏添加到命令中

扩展 ament 通常意味着您想要给其他软件包提供一些功能。向客户端软件包提供宏的最佳方式是用 ament 来注册它 。

可以通过附加 `${PROJECT_NAME}_CONFIG_EXTRAS` 变量,用于 `ament_package()` 通过

``` cmake
list(APPEND ${PROJECT_NAME}_CONFIG_EXTRAS
  path/to/file.cmake"
  other/pathto/file.cmake"
)
```

或者,您可以直接将文件添加到 `ament_package()` 调用 :

``` cmake
ament_package(CONFIG_EXTRAS
  path/to/file.cmake
  other/pathto/file.cmake
)
```

<span id="adding-to-extension-points"></span>

### 添加到扩展点

除了可以用于其他软件包的功能的简单文件外, 您还可以添加扩展到 ament 中。 这些扩展是用定义扩展点的功能执行的脚本。 用于 lament 扩展的最常用的用例很可能是登记 rosidl 信件生成器 : 当写入生成器时, 您通常也想要使用您的生成器生成所有信件和服务, 而无需修改信件/ 服务定义包的代码 。 通过将生成器注册为扩展到 `rosidl_generate_interfaces`.

举例来说,

``` cmake
ament_register_extension(
  "rosidl_generate_interfaces"
  "rosidl_generator_cpp"
  "rosidl_generator_cpp_generate_interfaces.cmake")
```

输入宏 `rosidl_generator_cpp_generate_interfaces.cmake` 用于软件包 `rosidl_generator_cpp` 切换到扩展点 `rosidl_generate_interfaces`。当扩展点被执行时,这将触发脚本的执行。 `rosidl_generator_cpp_generate_interfaces.cmake` 在此。 特别是, 当函数作用时, 这将调用生成器 。 `rosidl_generate_interfaces` 被处死。

发电机最重要的扩展点,除此之外 `rosidl_generate_interfaces`时,是 `ament_package`,它将简单地执行脚本 `ament_package()` 。此扩展点在注册资源时有用(见下文)。

`ament_register_extension` 函数需要三个参数:

- `extension_point`: 扩展点的名称( 大部分时间是其中之一) `ament_package` 或 时 间 `rosidl_generate_interfaces`)

- `package_name`: 包含 CMake 文件的软件包名称(即文件被写入的项目的项目名称)

- `cmake_filename`: 扩展点运行时执行的 CMake 文件

> **说明**
>
> 可以以类似方式定义自定义扩展点。 `ament_package` 财务报告和财务报告 `rosidl_generate_interfaces`但是这根本不必要。

<span id="adding-extension-points"></span>

### 添加扩展点

极少数情况下,界定一个新的延伸点,即Ament,可能很有趣。

扩展点可以在宏中注册,以便在调用相应的宏时执行所有扩展。为此:

- 定义和记录扩展名(例如: `my_extension_point`),这就是传到该地名。 `ament_register_extension` 宏在使用扩展点时。

- 执行扩展调用时的宏/功能:

``` cmake
ament_execute_extensions(my_extension_point)
```

通过定义包含扩展点名称的变量并填充要执行的宏来定义扩展点的工作。在调用时 `ament_execute_extensions`,然后将变量中定义的脚本逐一执行。

<span id="adding-resources"></span> <span id="ament-cmake-doc-adding-resources"></span>

## 添加资源

特别是在开发允许插件的插件或软件包时,往往必须从另一个ROS软件包(如插件)中添加资源。实例可以是使用插件lib的工具的插件。

使用Ament指数(也称为“资源指数”)可以做到这一点。

<span id="the-ament-index-explained"></span>

### 说明的指数

有关设计和意图的详情,请参见: [这儿](https://github.com/ament/ament_cmake/blob/rolling/ament_cmake_core/doc/resource_index.md)

原则上,该索引载于秘书处内部的文件夹中。 [安装空间](https://colcon.readthedocs.io/en/released/user/what-is-a-workspace.html#install-artifacts)。它包含以不同类型资源命名的浅层子文件夹。在子文件夹中,每个提供所述资源的软件包都使用“标记文件”的名称来引用。文件可能包含获取资源所需的任何内容,例如,与资源安装目录的相对路径,也可能是空的。

举例来说, 考虑为 RViz 提供显示插件: 当在命名的工程中提供 RViz 插件时 `my_rviz_displays` 它将被插件lib读取,您将提供一个 `plugin_description.xml` 文件,它将被插件lib安装并用于加载插件。要做到这一点,插件_description.xml通过资源_index注册为资源

``` cmake
pluginlib_export_plugin_description_file(rviz_common plugins_description.xml)
```

运行时 `colcon build`,此选项将安装文件 `my_rviz_displays` 输入子文件夹 `rviz_common__pluginlib__plugin` 输入资源_index。 rviz\_ common 中的插件lib 工厂将会从所有命名的文件夹中收集信息 `rviz_common__pluginlib__plugin` 用于导出插件的软件包。插件lib工厂的标记文件包含一个安装文件夹相对路径 `plugins_description.xml` 文件( 以及作为标记文件名的库名称) 。 有了此信息, 插件lib 可以加载库, 并知道要从库中加载哪些插件 。 `plugin_description.xml` 文档。

作为第二个例子, 请考虑让自己的 RViz 插件使用您自定义的 meshes 的可能性。 Meshes 在启动时被加载, 这样插件所有者不必处理它, 但这意味着 RViz 必须知道 meshes 。 要做到这一点, RViz 提供了一个函数 :

``` cmake
register_rviz_ogre_media_exports(DIRECTORIES <my_dirs>)
```

它将目录注册为 ament 索引中的 ogre\_ media 资源。 简言之, 它安装一个以工程命名的文件, 将函数调用到一个子文件夹中 。 `rviz_ogre_media_exports`。该文件包含与宏中列出的目录相对的安装文件夹路径。启动时,RViz现在可以搜索所有名为文件夹的文件夹 `rviz_ogre_media_exports` 并载入所有提供的文件夹中的资源。这些搜索使用 `ament_index_cpp` (或 减) `ament_index_py` Python 软件包。

在以下各节中,我们将探讨如何将你们自己的资源添加到指数中,并提供这样做的最佳做法。

<span id="querying-the-ament-index"></span>

### 查询天体指数

如有必要,可以通过CMake查询资源指数。为此,有三个功能:

`ament_index_has_resource`: 如果资源存在下列参数,则获取资源前缀路径:

- `var`: 输出参数: 如果资源不存在, 或资源前缀路径存在, 用 FALSE 填充此变量

- `resource_type`:资源的类型(例如. `rviz_common__pluginlib__plugin`)

- `resource_name`: 在添加类型资源_类型资源(例如,)后,通常等于软件包名称的资源名称。 `rviz_default_plugins`)

`ament_index_get_resource`:获取特定资源的内容,即Ament索引中标记文件的内容.

- `var`: 输出参数:如果存在资源标记文件,则填充内容.

- `resource_type`:资源的类型(例如. `rviz_common__pluginlib__plugin`)

- `resource_name`: 在添加类型资源_类型资源(例如,)后,通常等于软件包名称的资源名称。 `rviz_default_plugins`)

- `PREFIX_PATH`: 搜索的前缀路径(通常是默认) `ament_index_get_prefix_path()` 将足够。

请注意: `ament_index_get_resource` 如果资源不存在, 则会丢出错误, 因此可能需要检查是否使用 `ament_index_has_resource`.

`ament_index_get_resources`: 从索引中获取所有特定类型的注册资源

- `var`: 输出参数:填充所有已注册资源类型软件包的名称列表

- `resource_type`:资源的类型(例如. `rviz_common__pluginlib__plugin`)

- `PREFIX_PATH`: 搜索的前缀路径(通常是默认) `ament_index_get_prefix_path()` 将足够。

<span id="adding-to-the-ament-index"></span>

### 添加到 adment 索引

定义资源需要两个比特的信息:

- 资源的名称必须是独一无二的,

- 标记文件的布局,它可以是任何东西,也可以是空的(例如,标记ROS 2 包的“包”资源就是如此)

对于RViz网格资源,相应的选择是:

- `rviz_ogre_media_exports` 作为资源的名称,

- 安装包含资源的所有文件夹的路径。 这将已经允许您为使用您的软件包中的相应资源而写入逻辑 。

为了方便用户为您的软件包注册资源,您应当进一步提供宏或函数,例如插件lib函数或 `rviz_ogre_media_exports` 函数。

要注册资源, 请使用 mention 函数 `ament_index_register_resource`。这将创建和安装资源_index中的标记文件。举例来说,相应的呼吁 `rviz_ogre_media_exports` 具体如下:

``` cmake
ament_index_register_resource(rviz_ogre_media_exports CONTENT ${OGRE_MEDIA_RESOURCE_FILE})
```

此安装一个名为 `${PROJECT_NAME}` 输入文件夹 `rviz_ogre_media_exports` 输入资源_索引,内容由变量提供 `${OGRE_MEDIA_RESOURCE_FILE}`。宏有若干参数可以使用:

- 第一个( 未命名) 参数是资源的名称, 相当于资源\_ 索引中文件夹的名称

- `CONTENT`: 标记文件作为字符串的内容。 这可以是相对路径列表等 。 `CONTENT` 无法一起使用 `CONTENT_FILE`.

- `CONTENT_FILE`: 用于创建标记文件的文件的路径。 文件可以是普通文件,也可以是模板文件,以扩展为 `configure_file()`. `CONTENT_FILE` 无法一起使用 `CONTENT`.

- `PACKAGE_NAME`: 数据包/文献导出资源的名称,相当于标记文件的名称。默认为 `${PROJECT_NAME}`.

- `AMENT_INDEX_BINARY_DIR`: 生成的 Ament 索引的基础路径。 除非真的有必要, 总是使用默认 `${CMAKE_BINARY_DIR}/ament_cmake_index`.

- `SKIP_INSTALL`: 跳过安装标记文件。

由于每个软件包只存在一个标记文件, 如果 CMake 函数/ macro 被同一个项目调用两次, 通常就是一个问题。 然而, 对于大型项目来说, 最好将注册资源的呼叫分开 。

因此,让宏观资源注册,例如: `register_rviz_ogre_media_exports.cmake` 只填充一些变量。真正的调用到 `ament_index_register_resource` 然后可以在一个扩展内添加到 `ament_package`。因为只有一次呼吁 `ament_package` 每个项目,总是只有一个地方可以登记资源。 `rviz_ogre_media_exports` 这相当于以下战略:

- 宏 `register_rviz_ogre_media_exports` 将文件夹列表附加到一个名为 `OGRE_MEDIA_RESOURCE_FILE`.

- 另一个宏叫做 `register_rviz_ogre_media_exports_hook` 电话 `ament_index_register_resource` 若为 `${OGRE_MEDIA_RESOURCE_FILE}` 是非空的。

- 那个... `register_rviz_ogre_media_exports_hook.cmake` 文件在第三个文件中注册为 ament 扩展名 `register_rviz_ogre_media_exports_hook-extras.cmake` 通过电话

``` cmake
ament_register_extension("ament_package" "rviz_rendering"
  "register_rviz_ogre_media_exports_hook.cmake")
```

- 档案 `register_rviz_ogre_media_exports.cmake` 财务报告和财务报告 `register_rviz_ogre_media_exports_hook-extra.cmake` 登记为 `CONFIG_EXTRA` 与 `ament_package()`.

<span id="setting-environment-variables"></span>

## 设置环境变量

`ament_cmake` 提供一种机制,用于自动设置ROS 2 工作空间的环境变量,用于配置:

- RAM的实施工作(建立旋风DDS、FastDDS等)

- Gazebo 模拟( 设置插件和资源的路径)

- 其它自定义的机器人设置配置

可通过下列方式加以执行: `ament_environment_hooks`,它允许软件包定义工作空间来源时设定的持久性环境变量。

<span id="about-environment-hooks"></span>

### 关于环境钩

环境钩是 ROS 2 软件包提供的 shell 脚本。 当工作空间中的设置文件来源于此时, 钩也是来源于此。 这些脚本允许您设置或扩展环境变量, 并需要手工修改 。 `setup.bash` 或 时 间 `setup.zsh` 文档。

这些环境钩子可以通过创建两种类型的脚本文件来实现:

- `.dsv.in` 文件:这些是指定预期环境变量变化的机器可读文件。Ament比传统的 shell 脚本更有效地处理这些文件,在设置环境时提高了性能。

- `.sh.in` 文件 : 这些是 Linux/macOS  shells 执行的 shell 脚本, 如 sh、 bash 和 zsh 。 当获取工作空间时, 它们会在运行时设置环境变量 。

这些文件由 `colcon` 生成最后的环境钩脚本。

实际执行情况 `ament_environment_hooks` 可在官方网站找到 [ament-cmake 存储器](https://github.com/ament/ament_cmake/tree/master/ament_cmake_core/cmake/environment_hooks).

<span id="defining-persistent-environment-variables-through-hooks"></span>

### 通过钩子定义持久性环境变量

本节提供了如何使用环境钩子来配置 FastDDS XML 配置文件用于您的 ROS 2 软件包的简单实例 。

建议在界定环境钩时采用的最佳做法是,将它们置于一个专门的 `hooks` 软件包工作空间内的目录。

在你的创造中 `hooks` 文件夹,创建 `my_package.sh.in` 现将有关事项通知如下:

``` bash
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
export RMW_FASTRTPS_USE_QOS_FROM_XML=1
export FASTRTPS_DEFAULT_PROFILES_FILE="$COLCON_CURRENT_PREFIX/my_dds_profile.xml"
```

在同一文件夹中创建 `my_package.dsv.in` 文件如下:

``` bash
set;RMW_IMPLEMENTATION;rmw_fastrtps_cpp
set;RMW_FASTRTPS_USE_QOS_FROM_XML;1
set;FASTRTPS_DEFAULT_PROFILES_FILE;my_dds_profile.xml
```

添加后,您可以使用 ment_environment_hooks 函数在您的 `CMakeLists.txt` 文件 :

``` bash
ament_environment_hooks(
  "${CMAKE_CURRENT_SOURCE_DIR}/hooks/my_package.dsv.in"
  "${CMAKE_CURRENT_SOURCE_DIR}/hooks/my_package.sh.in"
)
```

Gazebo 插件路径的另一个使用环境钩子的例子可见于官方 [ros_gz_project_template](https://github.com/gazebosim/ros_gz_project_template/tree/main/ros_gz_example_gazebo/hooks).

<span id="api-version-management"></span>

## API 版本管理

ROS 2 通过提供自动版本头生成 `ament_generate_version_header`,它为 API 版本和特性检测创建编译时间宏。这特别有助于维护基于库版本的后向兼容性和有条件的启用功能。

> **说明**
>
> 那个... `ament_generate_version_header` 功能只针对C,C++和其他基于C的语言。它生成带有预处理器宏的C/C++头文件,不适用于Python或其他非基于C的软件包。

<span id="understanding-auto-generated-version-macros"></span>

### 理解自动生成的版本宏

许多ROS 2 C/C++ 软件包(例如: `rclcpp`, `rcl`,以及 `rmw`)自动生成包含宏的版本头文件,以曝光库的版本信息。这些版本头文件来自 `package.xml` 使用 [ament_generate_version_header.cmake](https://github.com/ament/ament_cmake/blob/$%7BROS_DISTRO%7D/ament_cmake_gen_version_h/cmake/ament_generate_version_header.cmake) 脚本。

生成的版本宏遵循此命名惯例 :

- `<PACKAGE_NAME>_VERSION_MAJOR`: 主要版本编号

- `<PACKAGE_NAME>_VERSION_MINOR`: 小版本编号

- `<PACKAGE_NAME>_VERSION_PATCH`: 补丁版本编号

- `<PACKAGE_NAME>_VERSION`: 组合版为单整数(主要\* 10000+小\* 100+补丁)

- `<PACKAGE_NAME>_VERSION_STR`: 文本的字符串表示(例如, “1.2.3)

- `<PACKAGE_NAME>_VERSION_GTE(major, minor, patch)`: 宏以检查版本是否大于或等于指定的版本

举例来说, `rclcpp` 提供宏,例如:

- `RCLCPP_VERSION_MAJOR`

- `RCLCPP_VERSION_MINOR`

- `RCLCPP_VERSION_PATCH`

- `RCLCPP_VERSION`

- `RCLCPP_VERSION_STR`

- `RCLCPP_VERSION_GTE(major, minor, patch)`

<span id="generating-version-headers-for-your-package"></span>

### 生成您的软件包的版本头

要为自己的软件包生成版本头,请在您的软件包中添加以下内容: `CMakeLists.txt`:

``` cmake
find_package(ament_cmake_gen_version_h REQUIRED)
ament_generate_version_header(my_library)
```

生成标题文件 : `<build_dir>/my_library/version.h` ,可以包含在您的代码中:

``` cpp
#include "my_library/version.h"
```

版本信息自动从 `<version>` 标签在您的 `package.xml`.

默认情况下,生成的页眉文件将放置在构建目录中 `<package_name>/version.h`。您可以自定义输出位置 :

``` cmake
ament_generate_version_header(my_library HEADER_PATH "my_library/my_version.h")
```

<span id="using-version-macros-for-api-negotiation"></span>

### 使用版本宏进行 API 谈判

版本宏可以实现运行时间和编译时间特征检测,这对于跨越不同的ROS 2分布来写入便携式代码至关重要.

虽然ROS 2保证了ABI(应用二进制接口)在同一发行范围内的兼容性,但新的接口和特性可以回转,这意味着在单一发行范围内,根据安装的补丁发布方式,可能会有不同的API版本. 版本宏允许开发者在使用该版本之前检查是否有特定特性.

<span id="example-version-checking"></span>

#### 示例: 版本检查

``` cpp
#include "rclcpp/version.h"

// Check if new feature is available
#if RCLCPP_VERSION_GTE(28, 3, 0)
  use_new_api_with_feature();
#else
  use_old_api_without_feature();
#endif
```

<span id="best-practices"></span>

#### 最佳做法

- **在使用新特性前检查**:在使用旧版本库可能不具备的特性时,总是使用版本宏.

- **提供退步执行**: 在可能的情况下,为较旧的API版本提供替代执行,以保持后向兼容性.

- **文件版本要求**: 清晰地记录您软件包文档中特定功能所需的最低版本。

- **跨版本测试**:如果您的软件包需要支持多个ROS 2 分布,请对照最小支持的版本进行测试.

- **使用 GTE 宏**: 优先使用 `_VERSION_GTE(major, minor, patch)` 用于版本比较的宏,因为它提供了比手动比较单个版本组件更清洁,更可读的语法.
