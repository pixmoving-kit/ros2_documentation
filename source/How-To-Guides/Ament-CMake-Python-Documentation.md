<span id="ament-cmake-python-user-documentation"></span>
# ament_cmake_python 用户文档

`ament_cmake_python` 为采用 `ament_cmake` 构建类型且包含 Python 代码的软件包提供 CMake 函数。更多信息请参阅 [ament_cmake 用户文档](Ament-CMake-Documentation.md)。

!!! note "说明"
    纯 Python 软件包在大多数情况下应使用 `ament_python` 构建类型。创建方法见[创建第一个 ROS 2 软件包](../Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md)。只有无法采用这种方式时，例如需要混合 C/C++ 和 Python 代码，才应使用 `ament_cmake_python`。

<span id="basics"></span>
## 基础知识

<span id="basic-project-outline"></span>
### 基本项目结构

名为 `my_project`、采用 `ament_cmake` 构建类型并使用 `ament_cmake_python` 的软件包，其结构如下：

```
.
└── my_project
    ├── CMakeLists.txt
    ├── package.xml
    └── my_project
        ├── __init__.py
        └── my_script.py
```

`__init__.py` 可以为空，但必须存在，才能[让 Python 将其所在目录视为软件包](https://docs.python.org/3/tutorial/modules.html#packages)。在 `CMakeLists.txt` 同级还可以设置 `src` 或 `include` 目录，用来存放 C/C++ 代码。

<span id="using-ament-cmake-python"></span>
### 使用 ament_cmake_python

软件包必须在 `package.xml` 中声明对 `ament_cmake_python` 的依赖：

```xml
<buildtool_depend>ament_cmake_python</buildtool_depend>
```

`CMakeLists.txt` 应包含：

```cmake
find_package(ament_cmake_python REQUIRED)
# ...
ament_python_install_package(${PROJECT_NAME})
```

`ament_python_install_package()` 的参数是与 `CMakeLists.txt` 同级、包含 Python 文件的目录名称。本例中是 `my_project`，也就是 `${PROJECT_NAME}`。

!!! warning "警告"
    在同一个 CMake 项目中调用 `rosidl_generate_interfaces` 和 `ament_python_install_package` 无法正常工作。详见此 [GitHub issue](https://github.com/ros2/rosidl_python/issues/141)。最佳实践是将消息生成单独放到另一个软件包中。

这样，只要另一个 Python 软件包正确声明了对 `my_project` 的依赖，就能将它作为普通 Python 模块使用：

```python
from my_project.my_script import my_function
```

这里假定 `my_script.py` 包含名为 `my_function()` 的函数。

<span id="using-ament-cmake-pytest"></span>
### 使用 ament_cmake_pytest

`ament_cmake_pytest` 用于让 `cmake` 发现测试。软件包必须在 `package.xml` 中将其声明为测试依赖：

```xml
<test_depend>ament_cmake_pytest</test_depend>
```

假设软件包结构如下，测试位于 `tests` 文件夹中：

```
.
├── CMakeLists.txt
├── my_project
│   └── my_script.py
├── package.xml
└── tests
    ├── test_a.py
    └── test_b.py
```

`CMakeLists.txt` 应包含：

```cmake
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

`ament_python` 支持自动发现测试，而 `ament_cmake_pytest` 必须逐个传入测试文件路径。可以按需缩短超时时间。

现在可以使用[标准 colcon 测试命令](../Tutorials/Intermediate/Testing/CLI.md)运行测试。
