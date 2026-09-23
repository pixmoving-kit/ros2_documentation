---
translation_status: machine_translated
source: Tutorials/Intermediate/Testing/Cpp.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="writing-basic-tests-with-c-with-gtest"></span>

# 使用 C++ 和 GTest 编写基础测试

开始点:我们假设你有一个 [基本备忘\_ 制作软件包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md#createpkg) 已经设置了, 您想要加入一些测试 。

在这个教程中,我们将使用 [测试](https://google.github.io/googletest/primer.html).

<span id="package-setup"></span>

## 软件包设置

<span id="source-code"></span>

### 源代码

我们从一个文件开始, `test/tutorial_test.cpp`

``` c++
#include <gtest/gtest.h>

TEST(package_name, a_first_test)
{
  ASSERT_EQ(4, 2 + 2);
}

int main(int argc, char ** argv)
{
  testing::InitGoogleTest(&argc, argv);
  return RUN_ALL_TESTS();
}
```

<span id="package-xml"></span>

### package.xml

添加以下一行为 `package.xml`

``` c++
<test_depend>ament_cmake_gtest</test_depend>
```

<span id="cmakelists-txt"></span>

### CMakeLists.txt (中文(简体) ).

``` cmake
if(BUILD_TESTING)
  find_package(ament_cmake_gtest REQUIRED)
  ament_add_gtest(${PROJECT_NAME}_tutorial_test test/tutorial_test.cpp)
  target_include_directories(${PROJECT_NAME}_tutorial_test PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
  )
  # target_link_libraries(${PROJECT_NAME}_tutorial_test name_of_local_library)
endif()
```

测试代码被包裹在 `if/endif` 块,以避免在可能时进行建筑测试。 `ament_add_gtest` 函数非常类似 `add_executable` 因此,你需要打电话 `target_include_directories` 财务报告和财务报告 `target_link_libraries` 和平常一样 `target_link_libraries` 调用被显示为注释,因为 `name_of_local_library` 是一个占位符, 解析它并替换 `name_of_local_library` 与您实际目标名称连接 `add_library()` 只有在测试依赖于此软件包中构建的库时,才会拨打。

<span id="running-tests"></span>

## 运行测试

见 [关于如何从命令行运行测试的教程](CLI.md) 关于测试运行和检查测试结果的更多信息。
