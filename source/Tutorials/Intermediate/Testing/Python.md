---
translation_status: machine_translated
source: Tutorials/Intermediate/Testing/Python.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="writing-basic-tests-with-python"></span>

# 使用 Python 编写基础测试

开始点:我们假设你有一个 [基本动因\_ python 套件](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md#createpkg) 已经设置了, 您想要加入一些测试 。

如果您使用ament_cmake_python,请参考 [备注_cmake_python 文档](../../../How-To-Guides/Ament-CMake-Python-Documentation.md) 测试内容和引用 `colcon` 保持不变。

<span id="package-setup"></span>

## 软件包设置

<span id="setup-py"></span>

### setup.py

 `setup.py` 必须有测试依赖 `pytest` 在呼吁中 `setup(...)`:

``` python
tests_require=['pytest'],
```

<span id="test-files-and-folders"></span>

### 测试文件和文件夹

您的测试代码需要输入一个名为文件夹的文件夹 `tests` 在您的包的根部。

包含要运行的测试的任何文件必须具有模式 `test_FOO.py` 地点 `FOO` 可以用任何东西来代替。

<span id="example-package-layout"></span>

#### 示例包布局 :

``` default
awesome_ros_package/
  awesome_ros_package/
      __init__.py
      fozzie.py
  package.xml
  setup.cfg
  setup.py
  tests/
      test_init.py
      test_copyright.py
      test_fozzie.py
```

<span id="test-contents"></span>

## 测试内容

您现在可以写出您内心的测试内容。 [用于测试的充足资源](https://docs.pytest.org),但简言之,你可以与 `test_` 前缀并包含任何您想要的断言语句 。

``` python
def test_math():
    assert 2 + 2 == 5   # This should fail for most mathematical systems
```

<span id="running-tests"></span>

## 运行测试

见 [关于如何从命令行运行测试的教程](CLI.md) 关于测试运行和检查测试结果的更多信息。

<span id="special-commands"></span>

## 特别命令

超越 [标准 colcon 测试命令](CLI.md) 参数,也可以指定参数。 `pytest` 框架,从命令行 `--pytest-args` 标记。例如,您可以指定要运行的函数的名称

##### Linux/macOS

``` console
$ colcon test --packages-select <name-of-pkg> --pytest-args -k name_of_the_test_function
```

##### Windows

``` console
$ colcon test --merge-install --packages-select <name-of-pkg> --pytest-args -k name_of_the_test_function
```

要在进行测试时看到 pytest 输出, 请使用这些标记 :

``` console
$ colcon test --event-handlers console_cohesion+
```
