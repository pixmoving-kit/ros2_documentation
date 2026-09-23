---
translation_status: machine_translated
source: The-ROS2-Project/Contributing/Quality-Guide.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="quality-guide-ensuring-code-quality"></span>

# 质量指南：确保代码质量

本页指导如何改进ROS 2软件包的软件质量,侧重于比该软件的质量做法部分更具体的领域。 [开发者指南](Developer-Guide.md).

以下各节旨在讨论ROS 2核心、应用和生态系统软件包以及核心客户端库C++和Python,所提出的解决方案是出于设计和实施方面的考虑,以改善质量属性,如“可靠性”、“安全性”、“持久性”、“决定因素”等,这些属性与非功能性要求有关。

<span id="static-code-analysis-as-part-of-the-ament-package-build"></span>

## 静态代码分析,作为Ament 包构建的一部分

**一. 背景情况**:

- 你开发了你的C++生产代码.

- 您创建了 ROS 2 软件包, 并使用 `ament`.

**问题**:

- 库级静态代码分析不是作为软件包构建程序的一部分运行的.

- 库级静态代码分析需要手动执行.

- 在构建新软件包版本之前忘记执行库级静态代码分析的风险.

**解决方案**:

- 使用集成能力 `ament` 执行静态代码分析,作为软件包构建程序的一部分。

**执行情况**:

- 插入到软件包中 `CMakeLists.txt` 文档。

``` bash
...
if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
  ...
endif()
...
```

- 插入该词 `ament_lint` 测试对包的依赖性 `package.xml` 文档。

``` bash
...
<package format="2">
  ...
  <test_depend>ament_lint_auto</test_depend>
  <test_depend>ament_lint_common</test_depend>
  ...
</package>
```

**实例**:

- `rclcpp`:

  - [rclcpp/rclcpp/CMakeLists.txt](https://github.com/ros2/rclcpp/blob/rolling/rclcpp/CMakeLists.txt)

  - [rclcpp/rclcpp/package.xml](https://github.com/ros2/rclcpp/blob/rolling/rclcpp/package.xml)

- `rclcpp_lifecycle`:

  - [rclcpp/rclcpp_lifecycle/CMakeLists.txt](https://github.com/ros2/rclcpp/blob/rolling/rclcpp_lifecycle/CMakeLists.txt)

  - [rclcpp/rclcpp_lifecycle/package.xml](https://github.com/ros2/rclcpp/blob/rolling/rclcpp_lifecycle/package.xml)

**产生背景**:

- 支持的静态代码分析工具 `ament` 作为软件包构建的一部分运行。

- 不支持的静态代码分析工具 `ament` 需要单独执行。

<span id="static-thread-safety-analysis-via-code-annotation"></span>

## 通过代码注释进行静态线条安全分析

**背景情况 :**

- 您正在开发/ 调试您的多条 C++ 生产代码

- 您访问 C++ 代码中多个线程的数据

**问题:**

- 数据竞赛和僵局会导致临界的bug.

**解决方案 :**

- 使用 Clang 的静态 [线索安全分析](https://clang.llvm.org/docs/ThreadSafetyAnalysis.html) 通过注释线程代码

**实施的背景:**

要启用 Thread 安全性分析, 代码必须附加注释, 让编译者更多地了解代码的语义。 这些注释是 Clang 特定属性 - 例如 。 `__attribute__(capability()))`。ROS 2 不直接使用这些属性,而是提供在使用其他编译器时被擦除的预处理器宏。

这些宏可见于 [rcpputils/thread_safety_annotations.hpp](https://github.com/ros2/rcpputils/blob/rolling/include/rcpputils/thread_safety_annotations.hpp)

线索安全分析文件显示  
线程安全分析可用于任何线程库,但它确实要求线程API以有适当说明的类和方法包裹.

我们决定让ROS 2开发者能够使用 `std::` 我们不想像上面建议的那样提供我们自己的包裹型号。

有三个 C++ 标准库需要了解

- GNU 标准库 `libstdc++` - 默认 Linux, 明确通过编译器选项 `-stdlib=libstdc++`

- LLVM 标准库 `libc++` (又称: `libcxx` ) - 在macOS上默认,由编译器选项明确设置 `-stdlib=libc++`

- Windows C++ 标准库 - 与此使用大小写无关

`libcxx` 说明其 `std::mutex` 财务报告和财务报告 `std::lock_guard` 线程安全分析的执行。当使用 GNU 时 `libstdc++` ,这些说明不在场,因此无法在非包装上使用线条安全分析 `std::` 类型。

*因此,直接使用线程安全分析* `std::` *类型,我们必须使用* `libcxx`

**执行:**

这里的代码迁移建议绝不是完整的 — 当写入( 或注释已有的) 线条代码时, 鼓励您使用尽可能多的说明, 以及您使用的逻辑。 然而, 这个步骤是一个很好的开始 !

- 软件包/目标启用分析

  当 C++ 编译器为 Clang 时,启用 `-Wthread-safety` 旗。下文中基于 CMake 的项目示例

  ``` cmake
  if(CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    add_compile_options(-Wthread-safety)   # for your whole package
    target_compile_options(${MY_TARGET} PUBLIC -Wthread-safety)  # for a single library or executable
  endif()
  ```

- 注释代码

  - 第1步 - 注释数据成员

    - 在任何地方找到那个 `std::mutex` 用于保护一些成员数据

    - 添加数据 `RCPPUTILS_TSA_GUARDED_BY(mutex_name)` 被 mutex 保护的数据注释

    ``` cpp
    class Foo {
    public:
      void incr(int amount) {
        std::lock_guard<std::mutex> lock(mutex_);
        bar += amount;
      }

      void get() const {
        return bar;
      }

    private:
      mutable std::mutex mutex_;
      int bar RCPPUTILS_TSA_GUARDED_BY(mutex_) = 0;
    };
    ```

  - 步骤2 - 修正警告

    - 在上述例子中, `Foo::get` 将生成编译器警告 。 要修复它, 在返回栏前锁定

    ``` cpp
    void get() const {
      std::lock_guard<std::mutex> lock(mutex_);
      return bar;
    }
    ```

  - 步骤3 - (可选但建议) 将现有代码重构为私人- Mutex 模式

    线程 C++ 代码中推荐的图案是永远保留您的 `mutex` 作为 `private:` 数据结构成员。这使得数据安全成为包含结构的关切,从结构用户身上卸下责任,并尽量减少受影响代码的表面积。

    将锁锁私密化可能需要重新思考您的数据界面。 这是一项伟大的工作, 这里有一些需要考虑的事情 。

    - 您可能想要为进行需要复杂锁定逻辑的分析提供专门的接口,例如,在一组被过滤的mutex保护的地图结构中计数成员,而不是将基本结构实际还给消费者

    - 考虑复制以避免在数据量小的地方阻塞。 这可以让其他线程继续访问共享数据, 这有可能导致总体性能的改善 。

  - 步骤4 - (可选) 启用负能力分析

    [负能力分析](https://clang.llvm.org/docs/ThreadSafetyAnalysis.html#negative-capabilities) 请指定“在调用此功能时不能扣住这个锁 ” 。 它可能揭示出其他说明无法发现的潜在僵局。

    - 在您指定的地方 `-Wthread-safety`,添加额外的旗帜 `-Wthread-safety-negative`

    - 在获得锁的任何函数上,使用 `RCPPUTILS_TSA_REQUIRES(!mutex)` 模式

- 如何进行分析

  - ROS CI 建农场的夜间工作与 `libcxx`,当线程安全分析产生警告时,通过标记“不稳定”来显示 ROS 2 堆芯中的任何问题。

  - 对于本地运行,您有以下选项, 全部等效

    - 使用折叠器 [crang-libcxx 混音符](https://github.com/colcon/colcon-mixin-repository/blob/master/clang-libcxx.mixin) (见E/CN.4/Sub.2/2000/L.10和Add.1和2。) [文档](https://github.com/colcon/colcon-mixin-repository/blob/master/README.md) 用于配置组合)

      ``` default
      colcon build --mixin clang-libcxx
      ```

    - 将编译器传递到 CMake

      ``` default
      colcon build --cmake-args -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++ -DCMAKE_CXX_FLAGS='-stdlib=libc++ -D_LIBCPP_ENABLE_THREAD_SAFETY_ANNOTATIONS' -DFORCE_BUILD_VENDOR_PKG=ON --no-warn-unused-cli
      ```

    - 覆盖系统编译器

      ``` default
      CC=clang CXX=clang++ colcon build --cmake-args -DCMAKE_CXX_FLAGS='-stdlib=libc++ -D_LIBCPP_ENABLE_THREAD_SAFETY_ANNOTATIONS' -DFORCE_BUILD_VENDOR_PKG=ON --no-warn-unused-cli
      ```

**结果背景 :**

- 潜在的僵局和种族条件将在编译时出现,使用Clang和Clang时出现。 `libcxx`

<span id="dynamic-analysis-data-races-deadlocks"></span>

## 动态分析( 数据竞速和僵局)

**背景情况 :**

- 您正在开发/ 调试您的多条 C++ 生产代码 。

- 您使用线程或 C++11 线程 + llvm libc++(如 ThreadSanitizer) 。

- 您不使用 Libc/libstdc++ 静态连接( 如果是 Thread Sanitizer ) 。

- 您不构建非位置独立的可执行文件( 如 Thread Sanitize) 。

**问题:**

- 数据竞赛和僵局会导致临界的bug.

- 利用静态分析(合理性:静态分析的限制)无法检测到数据种族和僵局.

- 数据竞赛和僵局在开发调试 / 测试时不得出现( 理由: 通常不是所有可能通过生产代码行使的控制路径) 。

**解决方案 :**

- 使用一个动态分析工具,该工具侧重于查找数据种族和僵局(这里是crange Thread Sanitize)。

**执行:**

- 使用选项编译生产代码并将其与 clang 链接 `-fsanitize=thread` (这些工具是生产代码)

- 如果在分析期间必须执行不同的生产代码,则考虑有条件的编译,例如: [线性卫生器 \_ has_feature(thread_sanitizer)](https://clang.llvm.org/docs/ThreadSanitizer.html#has-feature-thread-sanitizer).

- 在不考虑采用某些编码的情况下, [线性卫生器 \_/ \* 属性 \*/\_( 无\_ 卫生器 (“ 线性 ”) )](https://clang.llvm.org/docs/ThreadSanitizer.html#attribute-no-sanitize-thread).

- 如果某些文件不应被装入仪器,则考虑将文件或函数级别排除在外 [线性卫生者黑名单](https://clang.llvm.org/docs/ThreadSanitizer.html#ignorelist),更具体地说: [链式卫生剂 卫生剂 特殊案例列表](https://clang.llvm.org/docs/SanitizerSpecialCaseList.html) 或带有 [线性卫生器 no\_ sanitize (“ 线性”)](https://clang.llvm.org/docs/ThreadSanitizer.html#ignorelist) 并使用选项 `--fsanitize-blacklist`.

**结果背景:**

- 更有机会在应用之前发现数据竞赛和生产代码的僵局.

- 分析结果可能缺乏可靠性,工具处于β相阶段(如ThreadSanitizer).

- 由于生产代号仪表(维持仪器/非仪器生产代号的单独分支等)而导致的间接费用.

- 仪器编码需要每个线程更多的内存(如ThreadSanitizer).

- 仪器编码映射了很多虚拟地址空间(如ThreadSanitizer).
