<span id="running-tests-in-ros-2-from-the-command-line"></span>

# 从命令行运行 ROS 2 测试

<span id="prerequisites"></span>

## 前提条件

需要准备好一个工作空间，其中的软件包包含测试。

<span id="build-and-run-your-tests"></span>

## 构建并运行测试

要编译并运行测试，在工作空间根目录执行 `colcon` 的 [test 子命令](https://colcon.readthedocs.io/en/released/reference/verb/test.html)即可。

```console
$ colcon test --ctest-args tests [package_selection_args]
```

其中，`package_selection_args` 是可选的软件包选择参数，用于限制 `colcon` 构建和运行哪些包。更多信息参见 [colcon 软件包选择参数文档](https://colcon.readthedocs.io/en/released/reference/package-selection-arguments.html)。

测试前通常不必[加载工作空间环境](../../Beginner-Client-Libraries/Colcon-Tutorial.md#colcon-tutorial-source-the-environment)。`colcon test` 会确保测试在正确的环境中运行，并能访问所需依赖等。

<span id="examine-test-results"></span>

## 查看测试结果

要查看结果，运行 `colcon` 的 [test-result 子命令](https://colcon.readthedocs.io/en/released/reference/verb/test-result.html)。

```console
$ colcon test-result --all
```

要查看具体哪些测试用例失败，请使用 `--verbose` 参数：

```console
$ colcon test-result --all --verbose
```

<span id="debugging-tests-with-gdb"></span>

## 使用 GDB 调试测试

使用 GDB 调试测试的详细说明，请参阅 [GDB 教程](../../../How-To-Guides/Getting-Backtraces-in-ROS-2.md)。
