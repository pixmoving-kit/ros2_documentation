---
translation_status: machine_translated
source: How-To-Guides/Developing-a-ROS-2-Package.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="developing-a-ros-2-package"></span>

# 开发 ROS 2 软件包

此教程将教你如何创建您的第一个 ROS 2 应用程序。 它针对想要在 ROS 2 中学习如何创建自定义包的开发者, 而不是想要使用 ROS 2 及其现有软件包的人 。

<span id="prerequisites"></span>

## 前提条件

- [安装ROS](../Installation.md)

- [安装 colcon](https://colcon.readthedocs.io/en/released/user/installation.html)

- 通过提供 ROS 2 安装来设置工作空间 。

<span id="creating-a-package"></span>

## 创建软件包

所有ROS 2 软件包从运行命令开始

``` console
$ ros2 pkg create --license Apache-2.0 <pkg-name> --dependencies [deps]
```

在您的工作空间( 通常是) `~/ros2_ws/src`).

要为特定客户端库创建软件包 :

##### C++

``` console
$ ros2 pkg create  --build-type ament_cmake --license Apache-2.0 <pkg-name> --dependencies [deps]
```

##### Python

``` console
$ ros2 pkg create  --build-type ament_python --license Apache-2.0 <pkg-name> --dependencies [deps]
```

然后,你可以更新 `package.xml` 包含您的软件包信息,例如依赖性、描述和作者身份。

<span id="c-packages"></span>

### C++ 软件包

你将主要使用 `add_executable()` CMake 宏随附

``` cmake
ament_target_dependencies(<executable-name> [dependencies])
```

以创建可执行的节点和链接依赖。

要安装您的发射文件和节点, 您可以使用 `install()` 宏放置在文件的末尾, 但放在文件的前面 `ament_package()` 宏 。

发射文件和节点的例子 :

``` cmake
# Install launch files
install(
  DIRECTORY launch
  DESTINATION share/${PROJECT_NAME}
)

# Install nodes
install(
  TARGETS [node-names]
  DESTINATION lib/${PROJECT_NAME}
)
```

<span id="python-packages"></span>

### Python 软件包

ROS 2 遵循 Python 使用的标准模块分配程序 `setuptools`对于 Python 软件包, `setup.py` 文件补充 C++ 软件包 `CMakeLists.txt`。关于分发的更多详情,请参见: [正式文件](https://docs.python.org/3/distributing/index.html#distributing-index).

在你的ROS2包里,你应该有一个 `setup.cfg` 文件看起来像 :

``` ini
[develop]
script_dir=$base/lib/<package-name>
[install]
install_scripts=$base/lib/<package-name>
```

备注a `setup.py` 看起来像文件的文件 :

``` python
import os
from glob import glob
from setuptools import setup

package_name = 'my_package'

setup(
    name=package_name,
    version='0.0.0',
    # Packages to export
    packages=[package_name],
    # Files we want to install, specifically launch files
    data_files=[
        # Install marker file in the package index
        ('share/ament_index/resource_index/packages', ['resource/' + package_name]),
        # Include our package.xml file
        (os.path.join('share', package_name), ['package.xml']),
        # Include all launch files.
        (os.path.join('share', package_name, 'launch'), glob('launch/*')),
    ],
    # This is important as well
    install_requires=['setuptools'],
    zip_safe=True,
    author='ROS 2 Developer',
    author_email='ros2@ros.com',
    maintainer='ROS 2 Developer',
    maintainer_email='ros2@ros.com',
    keywords=['foo', 'bar'],
    classifiers=[
        'Intended Audience :: Developers',
        'License :: TODO',
        'Programming Language :: Python',
        'Topic :: Software Development',
    ],
    description='My awesome package.',
    license='TODO',
    # Like the CMakeLists add_executable macro, you can add your python
    # scripts here.
    entry_points={
        'console_scripts': [
            'my_script = my_package.my_script:main'
        ],
    },
)
```

<span id="combined-c-and-python-packages"></span>

### 组合 C++ 和 Python 套件

当写一个同时带有 C++ 和 Python 代码的软件包时, `setup.py` 文档和 `setup.cfg` 文件未使用。 相反,使用 [ament_cmake_python](Ament-CMake-Python-Documentation.md).
