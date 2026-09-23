---
translation_status: machine_translated
source: The-ROS2-Project/Contributing/Code-Style-Language-Versions.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="code-style-and-language-versions"></span> <span id="codestyle"></span>

# 代码风格与语言版本

为了实现一个一致的外观产品,我们将(如果可能的话)对每种语言都遵循外部定义的样式准则。对于其他诸如软件包布局或文档布局,我们需要借鉴当前使用的流行风格,提出我们自己的准则。

此外,只要有可能,开发者应该使用综合工具,让他们检查这些指南是否在编辑中被遵循。例如,每个人都应该在其编辑器中安装一个PEP8检查器,以减少与风格有关的审查迭代。

此外,如果可能,软件包还应检查样式,作为单位测试的一部分,以帮助自动检测样式问题(见 [ament_lint_auto](https://github.com/ament/ament_lint/blob/rolling/ament_lint_auto/doc/index.rst)).

<span id="c"></span>

## C

<span id="standard"></span>

### 标准

我们将瞄准C99。

<span id="style"></span>

### 样式

我们用 [Python 的 PEP7 数据](https://www.python.org/dev/peps/pep-0007/) 我们的C风格指南,经过一些修改和补充:

- 我们将针对C99,因为我们不需要支持C89(如PEP7建议)

  - 理由:除其他外,它允许我们同时使用 `//` 财务报告和财务报告 `/* */` 样式注释

  - 理由:C99现在几乎无处不在

- C++ 样式 `//` 允许注释

- (可选)总是将字面放在比较操作符的左侧,例如. `0 == ret` 改为 `ret == 0`

  - 理由说明 : `ret == 0` 太容易变成 `ret = 0` 意外事故

  - 选项,因为使用时 `-Wall` 现代编译器会警告你

只有当我们不编写 Python 模块时,以下所有修改才适用 :

- 不使用 `Py_` 做为一切事物的前缀

  - 使用 CamelCase 版本的软件包名称或其他适当的前缀

- 有关文档字符串的内容不适用

我们可以使用 [第7页](https://github.com/mike-perdide/pep7) 用于样式检查的 Python 模块。 编辑器集成似乎很弱, 我们可能需要更详细地研究 C 的自动检查 。

<span id="id1"></span>

## C++

<span id="id2"></span>

### 标准

滚动目标C++17.

<span id="id3"></span>

### 样式

我们会使用 [Google C++ 样式指南](https://google.github.io/styleguide/cppguide.html),经过一些修改:

<span id="line-length"></span>

#### 行长

- 我们最长的行长度是100个字符.

<span id="file-extensions"></span>

#### 文件扩展名

- 页眉文件应当使用 `.hpp` 扩展名。

  - 原理:允许工具确定文件,C++或C的内容.

- 执行文件应使用 `.cpp` 扩展名。

  - 原理:允许工具确定文件,C++或C的内容.

<span id="variable-naming"></span>

#### 可变命名

- 对于全局变量,使用带有下划线的前缀的小写 `g_`

  - 理由:在整个项目中保持可变命名大小写的一致性

  - 理由:一看就容易分辨变量的范围

  - 语文之间的一致性

- **关于命名公约的说明**: ROS 2在多个命名领域偏离了Google C++样式指南:

  - Google 风格指南推荐 `kPascalCase` 对于常数(例如, `kDaysInAWeek`)

  - ROS 2项目目前使用混合 `snake_case`, `PascalCase`,以及 `UPPER_CASE` 命名公约

  - 这种偏差是出于历史原因并与现有的ROS代码库保持一致.

  - 就新项目而言,开发者应当遵循相关ROS 2软件包中的现有公约

  - 当出现疑问时,宁可与周边代码保持一致,也不要严格遵守Google风格

<span id="function-and-method-naming"></span>

#### 函数和方法命名

- Google 风格指南说 `CamelCase`,但 C++ std 库的样式为 `snake_case` 也允许

  - 理由: ROS 2 核心软件包目前使用 `snake_case`

    - 原因: 历史疏忽或个人偏好,

    - 不改变的理由:追溯性改变会太具有破坏性

  - 其他考虑:

    - `cpplint.py` 不检查此案件( 难于执行, 除了审查)

    - `snake_case` 能够使各语文之间更加一致

  - 具体指导:

    - 对已有项目,选择现有样式

    - 对新项目来说,要么可以接受,但建议更倾向于匹配相关的现有项目。

    - 最后决定总是开发者的自由裁量权

      - 函数指针、可调用类型等特殊情况可能需要弯曲规则

    - 注意类仍应使用 `CamelCase` 默认

<span id="access-control"></span>

#### 访问控制

- 要求所有阶级成员都私人化,因此需要进入者。

  - 理由:对用户 API 设计过度限制

  - 我们更喜欢私人会员, 只有在需要的时候才公开

  - 在选择允许成员直接访问之前,我们应该考虑使用访问器

  - 我们应该有很好的理由允许成员直接进入, 而不是因为它对我们来说是方便的。

<span id="exceptions"></span>

#### 例外

- 允许例外

  - 理由:这是一个新的代码库,所以遗留的论据不适用于我们

  - 理由:对于用户的 API 来说, C++ 的例外更为偏颇

  - 应明确避免销毁器的例外

- 我们应该考虑避免例外,如果我们打算将由此产生的API 包装在C中

  - 理由:这将更容易用C来包装

  - 理由:我们大多数在代码中的依赖性,我们打算在C中包起来,无论如何,不使用例外

<span id="function-like-objects"></span>

#### 类似函数的对象

- 对Lambda或 `std::function` 或 时 间 `std::bind`

<span id="boost"></span>

#### 脚步

- 除非绝对需要,否则应避免助推。

<span id="comments-and-doc-comments"></span>

#### 评论和文件评论

- 使用 `///` 财务报告和财务报告 `/** */` 备注 *文档* 目的和目标 `//` 注释和一般性评论的样式

  - 类和函数注释应当使用 `///` 财务报告和财务报告 `/** */` 样式注释

  - 理由:C/C++中为Doxygen和Sphinx推荐这些理由

  - 理由:混合 `/* */` 财务报告和财务报告 `//` 用于块评论包含注释的代码

  - 说明各类别和职能中代码或注释应如何使用 `//` 样式注释

<span id="pointer-syntax-alignment"></span>

#### 指向语法对齐

- 使用 `char * c;` 改为 `char* c;` 或 时 间 `char *c;` 由于这种情况, `char* c, *d, *e;`

<span id="class-privacy-keywords"></span>

#### 类隐私关键词

- 不要在前面放置一个空格 `public:`, `private:`,或 `protected:`,所有缩进的倍数为 2 更为一致。

  - 理由:大多数编辑员不喜欢不是(软的)标签大小的倍数的缩进

  - 之前使用零空格 `public:`, `private:`,或 `protected:`,或 2 个空格

  - 如果在前面使用2个空格,则用2个额外空格缩进其他类语句

  - 偏好零空间,即: `public:`, `private:`,或 `protected:` 在与类别相同的栏目中

<span id="nested-templates"></span>

#### 嵌入模板

- 永远不要在嵌入模板中添加空白

  - 偏爱 `set<list<string>>` (C++11特性)改为 `set<list<string> >` 或 时 间 `set< list<string> >`

<span id="always-use-braces"></span>

#### 总是使用括号

- 总是在后面使用牙套 `if`, `else`, `do`, `while`,以及 `for`,即使身体是单行。

  - 理由:视觉模糊和由于体内使用宏观因素而出现并发症的机会减少

<span id="open-versus-cuddled-braces"></span>

#### 打开 Versus 抱抱框

- 使用打开的括号用于 `function`, `class`, `enum`,以及 `struct` 定义,但抱住 `if`, `else`, `while`, `for`,等等... ) (中文(简体) ).

  - 例外:当 `if` (或 减) `while`) 条件足够长, 需要线圈, 然后使用一个开口的牙套( 即不要抱) 。

- 当函数调用不能匹配到一条线上时,请在开阔的括号内(而不是在参数之间)包裹,然后在下一行上以2-空格缩进开始。在后续线上继续2-空格缩进,以便进行更多的参数。 (注意: [谷歌风格指南](https://google.github.io/styleguide/cppguide.html#Function_Calls) 这一点在内部是矛盾的。 )

  - 也一样 `if` (并) `while`等),条件过长,不能合于一线.

<span id="examples"></span>

#### 实例

这没关系:

``` c++
int main(int argc, char **argv)
{
  if (condition) {
    return 0;
  } else {
    return 1;
  }
}

if (this && that || both) {
  ...
}

// Long condition; open brace
if (
  this && that || both && this && that || both && this && that || both && this && that)
{
  ...
}

// Short function call
call_func(foo, bar);

// Long function call; wrap at the open parenthesis
call_func(
  foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar,
  foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar, foo, bar);

// Very long function argument; separate it for readability
call_func(
  bang,
  fooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooo,
  bar, bat);
```

这是 **没有**  :

``` c++
int main(int argc, char **argv) {
  return 0;
}

if (this &&
    that ||
    both) {
  ...
}
```

使用开放式的括号而不是过度缩进,例如用于区分构造器代码和构造器初始器列表

这没关系:

``` c++
ReturnType LongClassName::ReallyReallyReallyLongFunctionName(
  Type par_name1,  // 2 space indent
  Type par_name2,
  Type par_name3)
{
  DoSomething();  // 2 space indent
  ...
}

MyClass::MyClass(int var)
: some_var_(var),
  some_other_var_(var + 1)
{
  ...
  DoSomething();
  ...
}
```

这是 **没有** 好吧,甚至怪异(谷歌的方式)吗?

``` c++
ReturnType LongClassName::ReallyReallyReallyLongFunctionName(
    Type par_name1,  // 4 space indent
    Type par_name2,
    Type par_name3) {
  DoSomething();  // 2 space indent
  ...
}

MyClass::MyClass(int var)
    : some_var_(var),             // 4 space indent
      some_other_var_(var + 1) {  // lined up
  ...
  DoSomething();
  ...
}
```

<span id="linters"></span>

#### 林特尔

我们用谷歌的组合来检查这些风格 [cpplint.py](https://github.com/google/styleguide) 财务报告和财务报告 [解密( U)](https://github.com/uncrustify/uncrustify).

我们提供带有自定义配置的命令行工具 :

- [ament_clang_format](https://github.com/ament/ament_lint/blob/rolling/ament_clang_format/doc/index.rst): [配置](https://github.com/ament/ament_lint/blob/rolling/ament_clang_format/ament_clang_format/configuration/.clang-format)

- [ament_cpplint](https://github.com/ament/ament_lint/blob/rolling/ament_cpplint/doc/index.rst)

- [ament_uncrustify](https://github.com/ament/ament_lint/blob/rolling/ament_uncrustify/doc/index.rst): [配置](https://github.com/ament/ament_lint/blob/rolling/ament_uncrustify/ament_uncrustify/configuration/ament_code_style.cfg)

一些格式化工具,如ament_uncrustify和ament_clang_format支持 `--reformat` 选项,以应用设置中的更改。

我们还运用其他工具来检测并消除尽可能多的警告。 以下是我们试图针对所有套件进行的额外工作的非详尽清单:

- 使用诸如编译器的旗子 `-Wall -Wextra -Wpedantic`

- 运行静态代码分析, 如 `cppcheck`,我们已融入其中 [ament_cppcheck](https://github.com/ament/ament_lint/blob/rolling/ament_cppcheck/doc/index.rst).

<span id="python"></span>

## Python

<span id="version"></span>

### 版本

我们将针对Python 3进行开发。

<span id="id4"></span>

### 样式

我们会使用 [PEP8准则](https://www.python.org/dev/peps/pep-0008/) 用于代码格式。

我们选择了以下更精确的规则 其中PEP 8留下了一些自由:

- [我们允许每行最多100个字符(第5段)](https://www.python.org/dev/peps/pep-0008/#maximum-line-length).

- [只要不需要逃跑,我们就会从双倍引用中选择单句](https://www.python.org/dev/peps/pep-0008/#string-quotes).

- [我们更喜欢挂缩进 继续行](https://www.python.org/dev/peps/pep-0008/#indentation).

- [我们更喜欢一线只进口一个的拆分](https://peps.python.org/pep-0008/#imports):

  ``` python
  # This is preferred
  from typing import Dict
  from typing import List

  # over these
  from typing import Dict, List
  from typing import (
    Dict,
    List,
  )
  ```

类似工具 `(ament_)pycodestyle` Python 包应用于单位测试和/或编辑器集成中,用于检查Python 代码样式.

线条中使用的 pycode 样式配置是 [这儿](https://github.com/ament/ament_lint/blob/rolling/ament_pycodestyle/ament_pycodestyle/configuration/ament_pycodestyle.ini).

与编辑的整合 :

- [原子](https://atom.io/packages/linter-pycodestyle)

- [emacs 数据](https://www.emacswiki.org/emacs/PythonProgrammingInEmacs)

- [下标文本](https://sublime.wbond.net/packages/SublimeLinter-flake8)

- [维姆( Vim)](https://github.com/nvie/vim-flake8)

<span id="cmake"></span>

## CMake

<span id="id5"></span>

### 版本

读取 [REP 2000 环境方案](https://reps.openrobotics.org/rep-2000/) 用于确定您应该支持的最小 CMake 版本。目前任何支持的 ROS distro 的最小版本是 **3.14.4** (ROS Humble on macOS) (英语).

<span id="id6"></span>

### 样式

由于没有现有的CMake风格指南,我们将定义我们自己的:

- 使用小写命令名称( E)`find_package`,没有 `FIND_PACKAGE`).

- 使用 `snake_case` 标识符(变量、函数、宏)。

- 使用空 `else()` 财务报告和财务报告 `end...()` 命令。

- 之前没有空格 `(`‘s.

- 使用缩进的两个空格, 不使用标签 。

- 对多行宏引用参数不要使用对齐缩进。只使用两个空格。

- 偏好函数与 `set(PARENT_SCOPE)` 切换到宏。

- 当使用宏前缀本地变量时 `_` 或合理的前缀。

<span id="markdown-restructured-text-docblocks"></span>

## 标记下/ 重新插入文本/ docblocks

以下关于格式文本的规则旨在增加可读性和版本性。

<span id="any-doc-type"></span>

### 任意 Doc 类型

- 每句必须从新线开始.

  - 理由:对于较长的段落,开头一个改动使diff无法读取,因为它贯穿整个段落。

- 每个句子可以被可选地包裹,以保持每行的短写.

- 线条不应该有任何后面的白色空格。

<span id="markdown-or-rst"></span>

### 标记下或 RST

- 每一节标题前应有一行空行,后应有一行空行。

  - 理由:在筛选文件时,它能加快获得对结构的概述.

- 代码块必须先行,然后由空行取代。

  - 理由: 白空间只在栅栏码块之前和之后直接具有显著意义。 遵循这些指示, 将确保正确和一致地突出工作。

- 一个代码块应该指定一个语法(例如. `bash`).

<span id="rst-only"></span>

### 仅限 RST

- 在按结构排列的文字中,标题应沿用 [狮身人面像样式指南](https://documentation-style-guide-sphinx.readthedocs.io/en/latest/style-guide.html#headings):

  - `#` 带有超线(仅一次,用于文档标题)

  - `*` 带有超线的

  - `=`

  - `-`

  - `^`

  - `"`

  - 理由: 连贯的层次结构在筛选文档时会加速获得关于筑巢级的构想.

<span id="markdown-only"></span>

### 仅标下

- 在Markdown中,标题应沿用以下表格中描述的ATX样式: [标记下语法文档](https://daringfireball.net/projects/markdown/syntax#header)

  - ATX 式头部使用 1-6 散列字符(`#`),在行首表示头级1-6。

  - 应使用散列标题和标题之间的空格( 例如 : `# Heading 1`),使视觉上更容易将它们分开.

  - ATX型偏好的理由来自: [Google Markdown 样式指南](https://github.com/google/styleguide/blob/gh-pages/docguide/style.md#atx-style-headings)

  - 理由:ATX风格头部更容易搜索和维护,使前两个头部关卡与其他关卡一致.
