---
translation_status: machine_translated
source: Tutorials/Intermediate/Testing/CLI.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="running-tests-in-ros-2-from-the-command-line"></span>

# 从命令行运行 ROS 2 测试

<span id="prerequisites"></span>

## 前提条件

您需要一个工作区设置, 包含有测试的软件包 。

<span id="build-and-run-your-tests"></span>

## 构建和运行您的测试

要编译和运行测试,只需运行 [测试](https://colcon.readthedocs.io/en/released/reference/verb/test.html) 动词来自 `colcon` 在您工作空间的根部。

``` console
$ colcon test --ctest-args tests [package_selection_args]
```

何处 `package_selection_args` 是可选的软件包选择参数 `colcon` 以限制构建和运行哪些软件包。在 [关于软件包选择参数的 Colcon 文档](https://colcon.readthedocs.io/en/released/reference/package-selection-arguments.html)

[搜索工作空间](../../Beginner-Client-Libraries/Colcon-Tutorial.md#colcon-tutorial-source-the-environment) 在试验之前,不应作必要试验。 `colcon test` 确保测试与适当的环境同时进行,能够接触其依赖性等。

<span id="examine-test-results"></span>

## 检查测试结果

要看到结果,只需运行 [测试结果](https://colcon.readthedocs.io/en/released/reference/verb/test-result.html) 动词来自 `colcon`.

``` console
$ colcon test-result --all
```

要看到失败的准确测试案例,请使用 `--verbose` 旗帜 :

``` console
$ colcon test-result --all --verbose
```

<span id="debugging-tests-with-gdb"></span>

## 使用 GDB 调试测试

关于使用 GDB 调试测试的详细指南,请参见 [GDB 教程](../../../How-To-Guides/Getting-Backtraces-in-ROS-2.md).
