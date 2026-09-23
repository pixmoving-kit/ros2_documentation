---
translation_status: machine_translated
source: The-ROS2-Project/Contributing/Windows-Tips-and-Tricks.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="windows-tips-and-tricks"></span>

# Windows 使用技巧

ROS 2 支持Windows 10作为一级平台,这意味着所有进入ROS 2核心的代码都必须支持Windows. 对于Linux或其他Unix类系统上的传统开发,在Windows上开发可能有点困难,此文档旨在列出其中的一些差异.

<span id="maximum-path-length"></span>

## 最大路径长度

默认情况下, Windows 有 [最大路径长度](https://docs.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation) 在260个字符中。实际上,其中4个字符总是被驱动字母、冒号、初始反斜线和最后的NULL字符所使用。这意味着只有256个字符可供 *合计* 这对ROS 2有两种实际后果:

- 一些ROS 2 的内部路径名称相当长。 因此, 我们总是建议您在ROS 2 目录的根部使用一个简短的路径名称, 如 : `C:\dev`.

- 在从源头构建 ROS 2 时, 默认孤立的 colcon 构建模式可以生成非常长的路径名称 。 要避免这些非常长的路径名称, 请使用 `--merge-install` 当在 Windows 上建立时。

**说明**: 改变 Windows 以拥有更长的最大路径长度是可能的。见 [本条](https://docs.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=cmd#enable-long-paths-in-windows-10-version-1607-and-later) 以获取更多信息。

<span id="symbol-visibility"></span> <span id="windows-symbol-visibility"></span>

## 符号可见度

Microsoft Visual C++ 编译器( MSVC) 仅在明确导出时才会从动态链接库( DLL) 中曝光符号。 clang 和 gcc 编译器可选择同样操作, 但默认关闭 。 因此, 当先前在 Linux 上建的库建在 Windows 上时, 其他库可能无法解决外部符号。 下面是常见错误消息的例子, 可能是由于符号未曝光而导致的 :

``` console
error C2448: '__attribute__': function-style initializer appears to be a function definition
'visibility': identifier not found
```

``` console
CMake Error at C:/ws_ros2/install/random_numbers/share/random_numbers/cmake/ament_cmake_export_libraries-extras.cmake:48 (message):
   Package 'random_numbers' exports the library 'random_numbers' which
   couldn't be found
```

符号可见度也会影响二进制加载。 如果您发现一个可混凝土节点没有运行, 或者Qt Visualizer 无法工作, 则托管进程可能无法从二进制中找到一个预期的符号导出 。 要在 Windows 上判断这一点, Windows 开发工具包括一个名为 Gflags 的程序, 以启用各种选项。 其中一个选项叫做 *装入器抓图* 您可以在调试时检测负载失败。 请访问 Microsoft 文档了解更多信息 [标记](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/setting-and-clearing-image-file-flags) 财务报告和财务报告 [装货机抓取](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/show-loader-snaps).

Windows上导出符号的两个解决方案是可见度控制信头和 `WINDOWS_EXPORT_ALL_SYMBOLS` 属性。微软建议ROS开发者使用可见度控制信头来控制二进制中的符号导出。可见度控制信头提供了对符号导出宏的更多控制,并提供其他好处,包括较小的二进制大小和缩短链接时间。

<span id="visibility-control-headers"></span>

### 可见度控制信头

可见控制信头的目的是定义每个共享库的宏,该宏将正确宣布符号为 dllimport 或 dllexport。这是根据库是被消耗还是被建设本身来决定的。宏中的逻辑还考虑到编译器,并包括选择适当的语法的逻辑。 [海湾合作委员会可见度文件](https://gcc.gnu.org/wiki/Visibility) 包括逐步向库中添加明确符号的可见度的指示,“使质量最高的代码在二进制大小、负载时间和链接时间上得到最大减少”。 `visibility_control.h` 可以放置在 `includes` 下面的例子显示每个库的文件夹。下面的例子显示如何为一个库添加可见度控制标题。 `my_lib` 名为类的库 `example_class`。在包含的文件夹中添加一个可见标题,用于库。锅炉板逻辑与宏中使用的库名一起使用,使其在项目中独有。在另一个库中, `MY_LIB` 将替换为库名。

``` c++
#ifndef MY_LIB__VISIBILITY_CONTROL_H_
#define MY_LIB__VISIBILITY_CONTROL_H_
#if defined _WIN32 || defined __CYGWIN__
#ifdef __GNUC__
   #define MY_LIB_EXPORT __attribute__ ((dllexport))
   #define MY_LIB_IMPORT __attribute__ ((dllimport))
#else
   #define MY_LIB_EXPORT __declspec(dllexport)
   #define MY_LIB_IMPORT __declspec(dllimport)
#endif
#ifdef MY_LIB_BUILDING_LIBRARY
   #define MY_LIB_PUBLIC MY_LIB_EXPORT
#else
   #define MY_LIB_PUBLIC MY_LIB_IMPORT
#endif
#define MY_LIB_PUBLIC_TYPE MY_LIB_PUBLIC
#define MY_LIB_LOCAL
#else
 // Linux visibility settings
#define MY_LIB_PUBLIC_TYPE
#endif
#endif  // MY_LIB__VISIBILITY_CONTROL_H_
```

关于这个标题的完整示例,请参见: [rviz_rendering](https://github.com/ros2/rviz/blob/ros2/rviz_rendering/include/rviz_rendering/visibility_control.hpp).

要使用宏,请添加 `MY_LIB_PUBLIC` 在外部库中需要可见的符号之前。例如:

``` c++
Class MY_LIB_PUBLIC example_class {}

MY_LIB_PUBLIC void example_function (){}
```

要以正确的导出符号构建您的库, 您需要添加以下内容到您的 CMakeLists. txt 文件 :

``` cmake
target_compile_definitions(${PROJECT_NAME}
  PRIVATE "MY_LIB_BUILDING_LIBRARY")
```

<span id="windows-export-all-symbols-target-property"></span>

### WINDOWS\_ 出口\_ ALL\_ SYMBOLS 目标属性

CMake 执行 `WINDOWS_EXPORT_ALL_SYMBOLS` 在 Windows 上属性,它会导致函数符号自动导出。可以在 [WINDOWS_EXPORT_ALL_SYMBOLS 制作文档](https://cmake.org/cmake/help/latest/prop_tgt/WINDOWS_EXPORT_ALL_SYMBOLS.html)。可以通过在 CMakeLists 文件中添加以下内容来实施属性 :

``` cmake
set_target_properties(${LIB_NAME} PROPERTIES WINDOWS_EXPORT_ALL_SYMBOLS TRUE)
```

如果 CMakeLists 文件中有多个库, 您需要拨打 `set_target_properties` 分别放在每个上面。

请注意, Windows 上的二进制只能导出 65 536 个符号。如果一个二进制导出的符号超过此值,则会出错,并且应该使用可见度_控制头。对于全局数据符号,此方法有例外。例如,像下面那样的全局静态数据成员。

``` c++
class Example_class
{
public:
static const int Global_data_num;
```

在这种情况下, dllimprort/ dllexport必须明确适用。 使用以下条款中描述的生成\_ export\_ header即可做到 : [使用新的 CMake 导出全部特性在 Windows 上创建 dlls 而不拆分光谱 ()](https://blog.kitware.com/create-dlls-on-windows-without-declspec-using-new-cmake-export-all-feature/).

最后,必须至少将导出符号的页眉文件包含在其中之一。 `.cpp` 在软件包中的文件,这样宏就可以被扩展并放置到产生的二进制中。否则,符号仍然无法调用。

<span id="debug-builds"></span>

## 调试构建

在 Windows 上以调试模式构建时, 有几个非常重要的事情会改变。 第一个是所有的 DLL 都得到 `_d` 自动附加到库名中。所以如果该库被调用 `libfoo.dll`,在调试模式下,它将是 `libfoo_d.dll`。Windows上的动态链接器也知道要查找该形式的库,所以没有这些库,它不会找到这些库。 `_d` 前缀。此外,Windows在调试模式下打开了一整套编译时间和运行时间的检查,这些检查比Release所构建的检查要严格得多。由于这些原因,运行一个Windows调试构建并测试许多调试请求是一个好主意。

<span id="forward-slash-vs-back-slash"></span>

## 前锋对后锋

在Windows中,默认路径分隔符是反斜线(`\`这与前斜线不同。`/`) 用于 Linux 和 macOS 。 Windows API 大多可以作为路径分隔符处理,但这并非普遍真实。例如, `cmd.exe` shell 只能在使用反斜线字符时进行制表补全,而不是前斜线。对于Windows上的最大兼容性,总是应该使用反斜线作为Windows上的路径分隔符.

<span id="patching-vendored-packages"></span>

## 补丁供应商包

在 ROS 2 中销售包时,往往需要应用补丁来修复错误,添加特性等。 `ExternalProject_add` 调用以添加 `PATCH` 命令,使用 `patch` 可执行文件。不幸的是, `patch` 由巧克力交付的可执行文件需要管理员访问权限运行 。 工作环绕是使用 `git apply-patch` 当对外部项目应用补丁时。

`git apply-patch` 因此,外部项目应始终使用该数据库。 `GIT` 获取项目的方法,然后使用 `PATCH_COMMAND` 引用 `git apply-patch`.

以上所有例子的用法类似:

``` cmake
ExternalProject_Add(mylibrary-${version}
  GIT_REPOSITORY https://github.com/lib/mylibrary.git
  GIT_TAG ${version}
  GIT_CONFIG advice.detachedHead=false
  # Suppress git update due to https://gitlab.kitware.com/cmake/cmake/-/issues/16419
  # See https://github.com/ament/uncrustify_vendor/pull/22 for details
  UPDATE_COMMAND ""
  TIMEOUT 600
  CMAKE_ARGS
    -DCMAKE_INSTALL_PREFIX=${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}_install
    ${extra_cmake_args}
    -Wno-dev
  PATCH_COMMAND
    ${CMAKE_COMMAND} -E chdir <SOURCE_DIR> git apply -p1 --ignore-space-change --whitespace=nowarn ${CMAKE_CURRENT_SOURCE_DIR}/install-patch.diff
)
```

<span id="windows-slow-timers-slowness-in-general"></span>

## Windows 慢时器( 一般慢时)

在Windows上运行的软件一般比在Linux上运行的软件慢得多。这是由于一些因素造成的,从默认时间切片(根据数据,每20 ms) [文档](https://docs.microsoft.com/en-us/windows/win32/procthread/multitasking)),到运行的抗病毒和抗疟疾过程的数量,到运行的背景过程的数量。由于所有这些原因,测试应当 *永远* 期待 Windows 上的紧凑时间。 所有测试都应该有慷慨的超时, 并且只能期望事件最终发生( 这也会防止测试在 Linux 上成为 Flakey ) 。

<span id="shells"></span>

## 贝壳

Windows上有两个主命令行贝壳: 贵重的 `cmd.exe`和PowerShell. - 说吧 - 和Powershell.

`cmd.exe` 是命令 shell, 最密切地模仿旧 DOS shell, 尽管它的能力大大增强。 它完全基于文本, 并且只理解 DOS/ Windows `batch` 文档。

PowerShell 是微软为大多数新应用程序推荐的基于对象的更新 shell。它理解 `ps1` 用于配置的文件。

ROS 2 支持两者 `cmd.exe` 和PowerShell,所以任何改变(特别是诸如: `ament` 或 时 间 `colcon`应在两个方面进行测试。
