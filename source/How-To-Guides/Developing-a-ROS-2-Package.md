<span id="developing-a-ros-2-package"></span>
# 开发 ROS 2 软件包

本教程介绍如何创建第一个 ROS 2 应用程序，适合希望学习创建自定义 ROS 2 软件包的开发者，而不是仅使用已有软件包的用户。

<span id="prerequisites"></span>
## 前提条件

- [安装 ROS](../Installation.md)。
- [安装 colcon](https://colcon.readthedocs.io/en/released/user/installation.html)。
- 加载 ROS 2 安装环境，为工作空间做好准备。

<span id="creating-a-package"></span>
## 创建软件包

创建 ROS 2 软件包时，首先在工作空间（通常是 `~/ros2_ws/src`）中运行：

```console
$ ros2 pkg create --license Apache-2.0 <pkg-name> --dependencies [deps]
```

要针对特定客户端库创建软件包，分别使用以下命令。

**C++：**

```console
$ ros2 pkg create  --build-type ament_cmake --license Apache-2.0 <pkg-name> --dependencies [deps]
```

**Python：**

```console
$ ros2 pkg create  --build-type ament_python --license Apache-2.0 <pkg-name> --dependencies [deps]
```

随后可以更新 `package.xml`，填写依赖项、描述、作者等软件包信息。

<span id="c-packages"></span>
### C++ 软件包

通常使用 CMake 的 `add_executable()` 宏，配合以下命令创建节点可执行文件并链接依赖项：

```cmake
ament_target_dependencies(<executable-name> [dependencies])
```

要安装启动文件和节点，可以在文件末尾、`ament_package()` 宏之前调用 `install()`。

安装启动文件和节点的示例如下：

```cmake
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

ROS 2 遵循 Python 基于 `setuptools` 的标准模块分发流程。Python 软件包中的 `setup.py` 与 C++ 软件包中的 `CMakeLists.txt` 承担相应的配置作用。有关分发的详细信息，请参阅[官方文档](https://docs.python.org/3/distributing/index.html#distributing-index)。

ROS 2 软件包中应有一个如下形式的 `setup.cfg`：

```ini
[develop]
script_dir=$base/lib/<package-name>
[install]
install_scripts=$base/lib/<package-name>
```

以及如下形式的 `setup.py`：

```python
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
### 同时包含 C++ 和 Python 的软件包

编写同时包含 C++ 和 Python 代码的软件包时，不使用 `setup.py` 和 `setup.cfg`，而应使用 [ament_cmake_python](Ament-CMake-Python-Documentation.md)。
