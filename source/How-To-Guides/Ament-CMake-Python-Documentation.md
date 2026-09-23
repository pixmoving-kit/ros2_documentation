---
translation_status: machine_translated
source: How-To-Guides/Ament-CMake-Python-Documentation.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="ament-cmake-python-user-documentation"></span>

# ament_cmake_python 用户文档

`ament_cmake_python` 是一个为软件包提供 CMake 函数的软件包 `ament_cmake` 构建包含 Python 代码的类型。参见 [ament_cmake 用户文档](Ament-CMake-Documentation.md) 以获取更多信息。

> **说明**
>
> 纯 Python 软件包应使用 `ament_python` 在多数情况下构建类型。要创建 `ament_python` 软件包,见 [创建您的第一个 ROS 2 软件包](../Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md). `ament_cmake_python` 只能用于不可能使用的情况,如混合C/C++和Python代码时。

<span id="basics"></span>

## 基本情况

<span id="basic-project-outline"></span>

### 基本项目纲要

名为“我的项目”的一揽子方案的纲要 `ament_cmake` 用于构建类型 `ament_cmake_python` 看起来像:

``` default
.
└── my_project
    ├── CMakeLists.txt
    ├── package.xml
    └── my_project
        ├── __init__.py
        └── my_script.py
```

那个... `__init__.py` 文件可以是空的,但需要它 [使 Python 将包含此内容的目录作为软件包处理](https://docs.python.org/3/tutorial/modules.html#packages)。也可以有一个 `src` 或 时 间 `include` 与目录并列 `CMakeLists.txt` 含有 C/C++ 代码。

<span id="using-ament-cmake-python"></span>

### 使用ament_cmake_python

软件包必须声明依赖 `ament_cmake_python` 编号 `package.xml`.

``` xml
<buildtool_depend>ament_cmake_python</buildtool_depend>
```

那个... `CMakeLists.txt` 应包含:

``` cmake
find_package(ament_cmake_python REQUIRED)
# ...
ament_python_install_package(${PROJECT_NAME})
```

论点 `ament_python_install_package()` 名称与目录并列 `CMakeLists.txt` 包含 Python 文件。在此情况下,它是 `my_project`,或 `${PROJECT_NAME}`.

> **警告**
>
> 调用 `rosidl_generate_interfaces` 财务报告和财务报告 `ament_python_install_package` 。见此 [Github 问题](https://github.com/ros2/rosidl_python/issues/141) 。将信件生成分离成一个单独的软件包是最佳做法。

然后,另一个Python软件包 正确依赖于 `my_project` 可以将其作为普通的 Python 模块:

``` python
from my_project.my_script import my_function
```

假设 `my_script.py` 包含一个名为 `my_function()`.

<span id="using-ament-cmake-pytest"></span>

### 使用ament_cmake_pystems

套装 `ament_cmake_pytest` 用于使测试能够发现到 `cmake`。软件包必须声明测试依赖 `ament_cmake_pytest` 编号 `package.xml`.

``` xml
<test_depend>ament_cmake_pytest</test_depend>
```

说软件包有类似下面的文件结构, 测试在 `tests` 文件夹。

``` default
.
├── CMakeLists.txt
├── my_project
│   └── my_script.py
├── package.xml
└── tests
    ├── test_a.py
    └── test_b.py
```

那个... `CMakeLists.txt` 应包含:

``` cmake
if(BUILD_TESTING)
  find_package(ament_cmake_pytest REQUIRED)
  set(_pytest_tests
    tests/test_a.py
    tests/test_b.py
    # Add other test files here
  )
  foreach(_test_path ${_pytest_tests})
    get_filename_component(_test_name ${_test_path} NAME_WE)
    ament_add_pytest_test(${_test_name} ${_test_path}
      APPEND_ENV PYTHONPATH=${CMAKE_CURRENT_BINARY_DIR}
      TIMEOUT 60
      WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
    )
  endforeach()
endif()
```

与支持自动测试发现的ament_python的使用相比,ament_cmake_pytest必须随每个测试文件的路径一起调用。超时可以根据需要减少。

现在,你可以引用你的测试与 [标准 colcon 测试命令](../Tutorials/Intermediate/Testing/CLI.md).
