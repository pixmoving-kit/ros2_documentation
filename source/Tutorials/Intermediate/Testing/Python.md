<span id="writing-basic-tests-with-python"></span>

# 使用 Python 编写基本测试

本教程假设你已经创建了一个[基本的 ament_python 包](../../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md#createpkg)，现在希望为它添加测试。

如果使用 ament_cmake_python，请参阅 [ament_cmake_python 文档](../../../How-To-Guides/Ament-CMake-Python-Documentation.md)，了解如何让测试被发现。测试内容及通过 `colcon` 调用测试的方法相同。

<span id="package-setup"></span>

## 配置软件包

<span id="setup-py"></span>

### setup.py

在 `setup.py` 的 `setup(...)` 调用中，必须将 `pytest` 声明为测试依赖：

```python
tests_require=['pytest'],
```

<span id="test-files-and-folders"></span>

### 测试文件与目录

测试代码应放在软件包根目录下名为 `tests` 的文件夹中。

需要运行的测试所在文件必须采用 `test_FOO.py` 的命名方式，其中 `FOO` 可以替换为任意名称。

<span id="example-package-layout"></span>

#### 软件包目录示例

```
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

现在可以编写所需的测试。[pytest 文档](https://docs.pytest.org)提供了丰富的资料。简单来说，可以编写名称以 `test_` 开头的函数，并在其中加入所需的断言。

```python
def test_math():
    assert 2 + 2 == 5   # This should fail for most mathematical systems
```

<span id="running-tests"></span>

## 运行测试

有关运行测试和查看结果的更多信息，请参阅[从命令行运行测试的教程](CLI.md)。

<span id="special-commands"></span>

## 特殊命令

除了[标准的 colcon 测试命令](CLI.md)，还可以使用 `--pytest-args` 从命令行向 `pytest` 框架传递参数。例如，可以指定要运行的测试函数名称。

Linux/macOS：

```console
$ colcon test --packages-select <name-of-pkg> --pytest-args -k name_of_the_test_function
```

Windows：

```console
$ colcon test --merge-install --packages-select <name-of-pkg> --pytest-args -k name_of_the_test_function
```

要在测试运行时查看 pytest 输出，请使用以下参数：

```console
$ colcon test --event-handlers console_cohesion+
```
