---
translation_status: machine_translated
source: Tutorials/Advanced/Ament-Lint-For-Clean-Code.rst
---

<span id="ament-lint-cli-utilities"></span>

# Ament Lint 命令行工具

**目标：** 学会如何使用 `ament_lint` 以及查明和解决代码质量问题的相关工具。

**教程级别：** 高级

**用时：** 10分钟

<span id="background"></span>

## 背景

那个... `ament` CLI工具的家族是用ROS 2. Ament工具用于软件开发的Python工具,可以从任何构建系统中使用,但这些工具的一个子集,即: `ament_cmake` 工具,专门设计为基于 CMake 的开发 。 收集了 CLI 程序的 Ament 船可以帮助用户编写符合 ROS 2 编码标准的代码。 使用这些工具可以大大提高开发速度, 帮助用户编写符合 ROS 应用程序和核心代码 [ROS项目的编码标准](../../The-ROS2-Project/Contributing/Code-Style-Language-Versions.md)我们建议ROS开发人员熟悉这些工具,并在提交拉动请求之前使用这些工具。

<span id="prerequisites"></span>

## 前提条件

你应该有 `ament` 作为常规 ROS 2 设置的一部分而安装的软件包。

如果需要安装ROS 2,请查看 [安装指令](../../Installation.md).

<span id="ament-lint-cli-tools"></span>

## Ament Lint CLI 工具软件

所有嵌入工具都使用相似的 CLI 模式。 它们使用目录、目录列表、文件或文件列表, 分析输入文件并生成报告。 所有嵌入工具都有以下内置选项 。 **通过使用所建工具,可以找到一个特定阳性工具的最新和准确的文档。** `--help` **函数。**

- `-h, --help` - 显示帮助消息并退出。内置帮助信息通常拥有工具最准确和最新的文档。

- `--exclude [filename ...]` - 将文件名排除在分析之外,包括通配符。

- `--xunit-file XUNIT_FILE` - 生成一个 [x单位](https://xunit.net/) 符合 XML 文件。 这些文件最常被IDEs和CI用于自动接收测试结果。

<span id="ament-copyright"></span>

### 1 `ament_copyright`

那个... `ament_copyright` CLI 可用于检查和更新ROS源代码中的版权声明。该工具也可以用于检查您源代码中是否有适当的软件许可证、版权年和版权持有者。 `ament_copyright` 工具相对于其名称的目录工作,并将子目录行走,并检查目录中的每个源文件。您可以使用 `ament_copyright` 检查您的 ROS 软件包、 ROS 工作空间、 目录或单个源文件,只需移动到相应的根目录并调用命令即可。 `ament_copyright` 也可以用来对缺失的源代码文件自动应用版权和许可证.

<span id="ament-copyright-arguments"></span>

#### 1.1 `ament_copyright` 参数

默认 `ament_copyright` 将它被调用的目录,包括子目录和返回一个列出所有缺失版权通知的文件的报表。该程序使用一个单一的可选参数,该参数是报告需要扫描的目录的列表。例如,如果您想要扫描版权通知的纯源和头文件,您可以拨打: `ament_copyright ./src ./include`.

<span id="ament-copyright-options"></span>

#### 1.2 `ament_copyright` 选项

`ament_copyright` 支持以下选项:

- `--add-missing COPYRIGHT_NAME LICENSE` - 利用已通过的版权持有人和许可证,添加缺失的版权通知和许可证信息。 `LICENSE` 转到此选项的许可证名称。 `ament_copyright --list-licenses`

- `--add-copyright-year` - 在现有版权通知中增加本年度内容。

- `--list-copyright-names` - 列出已知版权持有者的姓名。

- `--list-licenses` - 列出已知许可证的名称。

- `--verbose` - 显示所有文件, 而不是只显示有错误/ 修改的文件 。

<span id="ament-copyright-example"></span>

#### 1.3 `ament_copyright` 示例

检查您的ROS 软件包是否有适当的版权和许可文件 `ament_copyright` 没有参数。使用 `--verbose` 选项将列出所有已检查的文件。

``` console
$ ament_copyright --verbose
my_package/src/new_file.cpp: could not find copyright notice
my_package/src/old_file.cpp: copyright=Open Source Robotics Foundation, Inc. (2023), license=apache2
my_package/include/new_file.h: could not find copyright notice
my_package/include/old_file.h: copyright=Open Source Robotics Foundation, Inc. (2023), license=apache2
```

<span id="ament-cppcheck"></span>

### 2 `ament_cppcheck`

那个... `ament_cppcheck` 命令行工具可用于对C++源代码文件进行静态分析. [静态分析](https://en.wikipedia.org/wiki/Static_program_analysis) 是自动审查源代码文件的过程,用于处理在编译后常常会产生问题的图案。 [cppp 检查](https://github.com/danmar/cppcheck),用于: `ament_cppcheck`,可以相当慢。 `ament_cppcheck` 可能在某些系统中被禁用。要启用它,只需设置 `AMENT_CPPCHECK_ALLOW_SLOW_VERSIONS` 环境变量。

<span id="ament-cppcheck-arguments"></span>

#### 2.1 `ament_cppcheck` 参数

默认 `ament_cppcheck` 将它被调用的目录,包括子目录并返回一个在源代码文件中列出所有潜在问题的报表。该程序使用一个单一的可选参数,该参数是报告需要扫描的目录列表。例如,如果您想要扫描最近修改的文件,您可以调用 `ament_cppcheck ./src/my_cpp_file.cpp`.

<span id="ament-cppcheck-options"></span>

#### 2.2 `ament_cppcheck` 选项

`ament_cppcheck` 支持以下选项:

- `--libraries [LIBRARIES ...]` - 除了 C 和 C++ 的标准库之外, 还要加载库配置。 每个库被传递到 cppcheck 作为 {library}\_ library\_ name\_}

- `--include_dirs [INCLUDE_DIRS ...]` - 包含正在检查的 C/C++ 文件的目录。 每一个目录都会被传递到 cppcheck 以 \`- I \< 包含\_ dir\_%%%% (默认: 无)

- `--cppcheck-version` - 拿到cppcheck版本,打印出来,然后退出。

<span id="ament-cppcheck-example"></span>

#### 2.3 `ament_cppcheck` 示例

在命名文件中创建以下简单的 C++ 程序 `example.cpp`.

``` cpp
int main()
{
    char a[10];
    a[10] = 0;
    return 0;
}
```

此简单程序访问所分配数组界限外的一部分内存。 运行中 `ament_cppcheck` 在带有文件的目录中,将得出以下结果:

``` console
$ ament_cppcheck
[example.cpp:4]: (error: arrayIndexOutOfBounds) Array 'a[10]' accessed at index 10, which is out of bounds.
```

<span id="ament-cpplint"></span>

### 3 `ament_cpplint`

`ament_cpplint` 用于检查您的 C++ 代码与 [Google 风格会议](https://google.github.io/styleguide/cppguide.html) 使用 [缩写( C)](https://github.com/cpplint/cpplint?tab=readme-ov-file). `ament_cpplint` 将对所有 C++ 头和源文件扫描当前目录和子目录,并将 CppLint 应用程序应用到文件中并返回结果。此时 `ament_cpplint` 无法自动解决它发现的问题, 如果您想要自动修正格式化问题, 请参见 `ament_uncrustify`.

<span id="ament-cpplint-arguments"></span>

#### 3.1 `ament_cpplint` 参数

程序使用一个单一的可选参数,该参数是报告需要扫描的目录列表。例如,如果您想要扫描版权通知的纯源和页眉文件,您可以拨打: `ament_copyright ./src ./include`.

<span id="ament-cpplint-options"></span>

#### 3.2 `ament_cpplint` 选项

- `--filters FILTER,FILTER,...` - 要应用的分类过滤器的逗号分隔列表。

- `--linelength N` - 最大线长( 默认: 100)。

- `--root ROOT` - Cpplint的根基选项。

<span id="ament-cpplint-example"></span>

#### 3.3 `ament_cpplint` 示例

让我们创建一个简单的 C++ 程序 `example.cpp`。我们将增加几行违反编码标准的代码:

``` cpp
int main()
{
  int a = 10;
  int b = 10;
  int c = 0;/*<trailing whitespace>*/
  if( a == b)  {/*<tab>*/      c=a;}/*<trailing whitespace>*/
  return 0;
}
```

应用 `ament_cpplint` 至此文件将产生以下错误:

``` console
example.cpp:0:  No copyright message found.  You should have a line: "Copyright [year] <Copyright Owner>"  [legal/copyright] [5]
example.cpp:6:  Line ends in whitespace.  Consider deleting these extra spaces.  [whitespace/end_of_line] [4]
example.cpp:6:  Tab found; better to use spaces  [whitespace/tab] [1]
example.cpp:6:  Line ends in whitespace.  Consider deleting these extra spaces.  [whitespace/end_of_line] [4]
example.cpp:6:  Missing spaces around =  [whitespace/operators] [4]
```

<span id="ament-flake8"></span>

### 4 `ament_flake8`

[花纹8](https://pypi.org/project/flake8/) 是一个 Linting 和样式执行的 Python 工具。 `ament_flake8` 命令行工具可用于快速执行 Python 源代码文件的林化 [花纹8](https://pypi.org/project/flake8/)。此工具将帮助您定位 ROS Python 程序中的小错误和样式问题, 如跟踪白空格、 代码线条过长、 函数参数空格差, 以及更多问题 ! 但是请注意, `flake8` 财务报告和财务报告 `ament_flake8` 无法自动重构代码以解决这些问题。

<span id="ament-flake8-arguments"></span>

#### 4.1 `ament_flake8` 参数

程序需要一个单一的可选参数,该参数是一个目录列表,应该扫描报告。例如,如果您想要扫描您工作空间中一个软件包,您可以拨打 `ament_flake8` 直接在软件包的工作目录中或传递到目录的路径 。

<span id="ament-flake8-options"></span>

#### 4.2 `ament_flake8` 选项

- `--config path` - 所使用的配置文件。 默认配置文件可以在您的安装站点软件包目录中找到。 我们不建议更改默认设置 。

- `--linelength N` - 手动设定最大线长。

<span id="ament-flake8-example"></span>

#### 4.3 `ament_flake8` 示例

在命名的文件中创建以下简单的 Python 程序 `example.py`.

``` python
def uglyPythonFunction(a,b,  c):
    if a != b:
        print("A does not match b")
    thisIsAVariableNameThatIsWayTooLongLongLong = 2
    extra_long =(thisIsAVariableNameThatIsWayTooLongLongLong*thisIsAVariableNameThatIsWayTooLongLongLong )
    return(c)
```

应用 `ament_flake8` 到此文件将导致以下错误。

``` console
example.py:1:25: E231 missing whitespace after ','
def uglyPythonFunction(a,b,  c):

example.py:5:5: F841 local variable 'extra_long' is assigned to but never used
    extra_long =(thisIsAVariableNameThatIsWayTooLongLongLong*thisIsAVariableNameThatIsWayTooLongLongLong )
    ^

example.py:5:17: E225 missing whitespace around operator
    extra_long =(thisIsAVariableNameThatIsWayTooLongLongLong*thisIsAVariableNameThatIsWayTooLongLongLong )
                ^

example.py:5:100: E501 line too long (106 > 99 characters)
    extra_long =(thisIsAVariableNameThatIsWayTooLongLongLong*thisIsAVariableNameThatIsWayTooLongLongLong )
                                                                                                   ^

example.py:5:105: E202 whitespace before ')'
    extra_long =(thisIsAVariableNameThatIsWayTooLongLongLong*thisIsAVariableNameThatIsWayTooLongLongLong )
                                                                                                        ^

1     E202 whitespace before ')'
1     E225 missing whitespace around operator
1     E231 missing whitespace after ','
1     E501 line too long (106 > 99 characters)
1     F841 local variable 'extra_long' is assigned to but never used

1 files checked
5 errors

'E'-type errors: 4
'F'-type errors: 1

Checked files:

* example.py
```

<span id="ament-uncrustify"></span>

### 5 `ament_uncrustify`

[解密( U)](https://github.com/uncrustify/uncrustify) 是一个 C++ 折叠工具, 类似于 `ament_cpplint`,这有它的优势,它可以 **自动修正** 此工具将帮助您定位并修正您的 C++ ROS 程序中的小错误和样式问题, 如跟踪白空格、 代码线太长、 空格函数参数差, 还有更多 !

<span id="ament-uncrustify-arguments"></span>

#### 5.1 `ament_uncrustify` 参数

程序需要一个单一的可选参数,该参数是一个目录列表,应该扫描报告。例如,如果您想要扫描您工作空间中一个软件包,您可以拨打 `ament_uncrustify` 直接在软件包的工作目录中或传递到目录的路径 。

<span id="ament-uncrustify-options"></span>

#### 5.2 `ament_uncrustify` 选项

- `-c CFG` - 如果您喜欢使用自己的设置, Uncrustify 应使用的配置文件。 我们建议您坚持默认设置

- `--linelength N` - 最大线程。

- `--language` - {C,C++,CPP} 中的一种, 传递给解密为 `-l <language>` 以强制使用特定语言,而不是根据文件扩展名选择。

- `--reformat` - 重构文件,即纠正遇到的格式错误。 **我们建议您在运行时使用此选项** `ament_uncrustify` **因为它会节省你很多时间!**

<span id="ament-uncrustify-example"></span>

#### 5.3 `ament_uncrustify` 示例

让我们回到名为 C++ 的简单程序 `example.cpp`.

``` cpp
int main()
{
     int a = 10;
     int b = 10;
     int c = 0;<trailing whitespace>
     if( a == b)<trailing whitespace>{
  <tab>      c=a;}<trailing whitespace>
     return 0;
 }
```

应用 `ament_uncrustify example.cpp` 到此文件将输出以下输出。

``` diff
--- example.cpp
+++ example.cpp.uncrustify
@@ -1,9 +1,10 @@
-  int main()
-  {
-       int a = 10;
-       int b = 10;
-       int c = 0;<trailing whitespace>
-       if( a == b)<trailing whitespace>{
- <tab>       c=a;}<trailing whitespace>
-       return 0;
-   }
+int main()
+{
+  int a = 10;
+  int b = 10;
+  int c = 0;
+  if (a == b) {
+    c = a;
+  }
+  return 0;
+}
1 files with code style divergence
```

要将这些更改应用到文件中, 我们可以运行 `ament_uncrustify` 与 `--reformat` 旗帜。 **如果指定此选项, 解禁将会在原地进行必要的更改, 节省我们很多时间, 尤其是当与更大的代码库合作时!**

<span id="other-ament-tools-of-note"></span>

### 6 其他值得注意的工具

ROS桌面全舰拥有少数值得注意的Ament开发工具,其中几个工具列举如下.

- `ament_lint_cmake` - 对照样式惯例检查CMake文件。

- `ament_xmllint` - 检查 XML 标记,例如 XML 发射文件,使用 xmllint 。

- `ament_pep257` - 检查 Python 的字串与样式常规在 [PEP 257](https://peps.python.org/pep-0257/).

Ament非常可扩展,鼓励ROS用户建造和使用能使其更有成效的Ament工具。您可以通过使用该工具来寻找其他社区贡献的Ament lint工具。 `apt search` 或通过 [在ROS索引上查找注释](https://index.ros.org/?pkgs=ament&search_packages=true).
