<span id="writing-basic-tests-with-c-with-gtest"></span>

# 使用 GTest 编写基本 C++ 测试

本教程假设你已经创建了一个[基本的 ament_cmake 包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md#createpkg)，现在希望为它添加测试。

本教程使用 [gtest](https://google.github.io/googletest/primer.html)。

<span id="package-setup"></span>

## 配置软件包

<span id="source-code"></span>

### 源代码

首先，将以下代码放入 `test/tutorial_test.cpp` 文件：

```c++
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

在 `package.xml` 中添加以下行：

```c++
<test_depend>ament_cmake_gtest</test_depend>
```

<span id="cmakelists-txt"></span>

### CMakeLists.txt

```cmake
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

测试代码放在 `if/endif` 块中，以便在不需要测试时避免构建它们。`ament_add_gtest` 的工作方式与 `add_executable` 类似，因此仍需像平常一样调用 `target_include_directories` 和 `target_link_libraries`。

示例将 `target_link_libraries` 注释掉，是因为 `name_of_local_library` 只是占位符。只有当测试依赖本包构建的库时，才应取消注释，并将其替换为 `add_library()` 调用中的实际目标名称。

<span id="running-tests"></span>

## 运行测试

有关运行测试和查看结果的更多信息，请参阅[从命令行运行测试的教程](CLI.md)。
