<span id="creating-a-package"></span> <span id="createpkg"></span>
# 创建软件包

**目标：** 使用 CMake 或 Python 创建新软件包，并运行其中的可执行程序。

**教程级别：** 初学者

**预计用时：** 15 分钟

<span id="background"></span>
## 背景

<span id="what-is-a-ros-2-package"></span>
### 1 什么是 ROS 2 软件包？

软件包是组织 ROS 2 代码的基本单位。如果希望安装自己的代码，或与他人分享，就需要将代码组织成软件包。这样，你就能发布 ROS 2 开发成果，让其他人方便地构建和使用。

ROS 2 使用 ament 作为构建系统，使用 colcon 作为构建工具。官方支持通过 CMake 或 Python 创建软件包，此外也存在其他构建类型。

<span id="what-makes-up-a-ros-2-package"></span>
### 2 ROS 2 软件包由什么组成？

ROS 2 的 Python 和 CMake 软件包各有其最低必需内容。

**CMake 软件包**

- `CMakeLists.txt`：描述如何构建包内代码。
- `include/<package_name>`：存放软件包的公共头文件。
- `package.xml`：包含软件包的元信息。
- `src`：存放软件包的源代码。

**Python 软件包**

- `package.xml`：包含软件包的元信息。
- `resource/<package_name>`：软件包的标记文件。
- `setup.cfg`：软件包包含可执行程序时需要此文件，让 `ros2 run` 能找到它们。
- `setup.py`：说明如何安装软件包。
- `<package_name>`：与软件包同名的目录，包含 `__init__.py`，ROS 2 工具用它查找软件包。

最简单的软件包结构如下。

**CMake**

```console
my_package/
     CMakeLists.txt
     include/my_package/
     package.xml
     src/
```

**Python**

```console
my_package/
      package.xml
      resource/my_package
      setup.cfg
      setup.py
      my_package/
```

<span id="packages-in-a-workspace"></span>
### 3 工作空间中的软件包

一个工作空间可以包含任意多个软件包，每个包位于独立文件夹中。同一工作空间也可以同时包含不同构建类型的软件包，例如 CMake 和 Python。软件包不能相互嵌套。

推荐在工作空间内创建 `src` 文件夹，并将软件包放在其中，以保持工作空间顶层整洁。

简单的工作空间可能具有以下结构：

```console
workspace_folder/
    src/
      cpp_package_1/
          CMakeLists.txt
          include/cpp_package_1/
          package.xml
          src/

      py_package_1/
          package.xml
          resource/py_package_1
          setup.cfg
          setup.py
          py_package_1/
      ...
      cpp_package_n/
          CMakeLists.txt
          include/cpp_package_n/
          package.xml
          src/
```

<span id="prerequisites"></span>
## 前提条件

按照[上一篇教程](Creating-A-Workspace/Creating-A-Workspace.md)操作后，你应该已经有了一个 ROS 2 工作空间。本教程将在其中创建软件包。

<span id="tasks"></span>
## 操作步骤

<span id="create-a-package"></span>
### 1 创建软件包

首先[加载 ROS 2 安装环境](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md)。

在[上一篇教程](Creating-A-Workspace/Creating-A-Workspace.md#new-directory)创建的 `ros2_ws` 工作空间中创建新软件包。运行创建命令前，确保进入 `src` 目录。

**Linux**

```console
$ cd ~/ros2_ws/src
```

**macOS**

```console
$ cd ~/ros2_ws/src
```

**Windows**

```console
$ cd \ros2_ws\src
```

ROS 2 创建新软件包的命令语法如下。

**CMake**

```console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 <package_name>
```

**Python**

```console
$ ros2 pkg create --build-type ament_python --license Apache-2.0 <package_name>
```

本教程使用可选参数 `--node-name`，在软件包中创建一个简单的 Hello World 可执行程序。

在终端输入对应命令。

**CMake**

```console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 --node-name my_node my_package
```

**Python**

```console
$ ros2 pkg create --build-type ament_python --license Apache-2.0 --node-name my_node my_package
```

此时，工作空间的 `src` 下会出现名为 `my_package` 的新文件夹。

命令执行后，终端会输出以下消息。

**CMake**

```console
going to create a new package
package name: my_package
destination directory: /home/user/ros2_ws/src
package format: 3
version: 0.0.0
description: TODO: Package description
maintainer: ['<name> <email>']
licenses: ['TODO: License declaration']
build type: ament_cmake
dependencies: []
node_name: my_node
creating folder ./my_package
creating ./my_package/package.xml
creating source and include folder
creating folder ./my_package/src
creating folder ./my_package/include/my_package
creating ./my_package/CMakeLists.txt
creating ./my_package/src/my_node.cpp
```

**Python**

```console
going to create a new package
package name: my_package
destination directory: /home/user/ros2_ws/src
package format: 3
version: 0.0.0
description: TODO: Package description
maintainer: ['<name> <email>']
licenses: ['TODO: License declaration']
build type: ament_python
dependencies: []
node_name: my_node
creating folder ./my_package
creating ./my_package/package.xml
creating source folder
creating folder ./my_package/my_package
creating ./my_package/setup.py
creating ./my_package/setup.cfg
creating folder ./my_package/resource
creating ./my_package/resource/my_package
creating ./my_package/my_package/__init__.py
creating folder ./my_package/test
creating ./my_package/test/test_copyright.py
creating ./my_package/test/test_flake8.py
creating ./my_package/test/test_pep257.py
creating ./my_package/my_package/my_node.py
```

这些消息列出了为新软件包自动生成的文件。

<span id="build-a-package"></span>
### 2 构建软件包

将软件包放在工作空间中有一个明显好处：在工作空间根目录运行一次 `colcon build`，就可以同时构建多个软件包，无需逐个构建。

返回工作空间根目录。

**Linux**

```console
$ cd ~/ros2_ws
```

**macOS**

```console
$ cd ~/ros2_ws
```

**Windows**

```console
$ cd \ros2_ws
```

现在构建软件包。

**Linux**

```console
$ colcon build
```

**macOS**

```console
$ colcon build
```

**Windows**

```console
$ colcon build --merge-install
```

Windows 存在路径长度限制，因此 `merge-install` 将各软件包合并安装到 `install` 目录。

上一教程还在 `ros2_ws` 中放入了 `ros_tutorials` 的软件包，所以运行 `colcon build` 时也会构建 `turtlesim`。工作空间中只有少量软件包时，这没什么问题；包较多时，完整构建就可能耗费很长时间。

下次只构建 `my_package` 时，可以运行：

```console
$ colcon build --packages-select my_package
```

<span id="source-the-setup-file"></span>
### 3 加载环境设置文件

要使用新软件包及其可执行程序，先打开新终端，加载主 ROS 2 安装环境。

然后在 `ros2_ws` 目录中运行对应命令，加载工作空间环境。

**Linux**

```console
$ source install/local_setup.bash
```

**macOS**

```console
$ . install/local_setup.bash
```

**Windows**

```console
$ call install/local_setup.bat
```

工作空间加入搜索路径后，就可以使用新软件包的可执行程序了。

<span id="use-the-package"></span>
### 4 使用软件包

运行创建软件包时通过 `--node-name` 生成的可执行程序：

```console
$ ros2 run my_package my_node
```

终端会显示以下消息。

**CMake**

```console
hello world my_package package
```

**Python**

```console
Hi from my_package.
```

<span id="examine-package-contents"></span>
### 5 查看软件包内容

在 `ros2_ws/src/my_package` 中，可以看到 `ros2 pkg create` 自动生成的文件和目录。

**CMake**

```console
CMakeLists.txt  include  package.xml  src
```

`my_node.cpp` 位于 `src` 目录，今后编写的 C++ 节点也放在这里。

**Python**

```console
my_package  package.xml  resource  setup.cfg  setup.py  test
```

`my_node.py` 位于 `my_package` 目录，今后编写的 Python 节点也放在这里。

<span id="customize-package-xml"></span>
### 6 自定义 package.xml

创建软件包后的输出中，`description` 和 `license` 字段可能包含 `TODO` 提示。软件包说明和许可证声明不会自动补充完整，而发布软件包时必须提供它们。`maintainer` 字段也可能需要填写。

用文本编辑器打开 `ros2_ws/src/my_package/package.xml`。

**CMake**

```xml
<?xml version="1.0"?>
<?xml-model
   href="http://download.ros.org/schema/package_format3.xsd"
   schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="3">
 <name>my_package</name>
 <version>0.0.0</version>
 <description>TODO: Package description</description>
 <maintainer email="user@todo.todo">user</maintainer>
 <license>TODO: License declaration</license>

 <buildtool_depend>ament_cmake</buildtool_depend>

 <test_depend>ament_lint_auto</test_depend>
 <test_depend>ament_lint_common</test_depend>

 <export>
   <build_type>ament_cmake</build_type>
 </export>
</package>
```

**Python**

```xml
<?xml version="1.0"?>
<?xml-model
   href="http://download.ros.org/schema/package_format3.xsd"
   schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="3">
 <name>my_package</name>
 <version>0.0.0</version>
 <description>TODO: Package description</description>
 <maintainer email="user@todo.todo">user</maintainer>
 <license>TODO: License declaration</license>

 <test_depend>ament_copyright</test_depend>
 <test_depend>ament_flake8</test_depend>
 <test_depend>ament_pep257</test_depend>
 <test_depend>python3-pytest</test_depend>

 <export>
   <build_type>ament_python</build_type>
 </export>
</package>
```

如果 `maintainer` 尚未自动填写，请输入你的姓名和电子邮箱。然后修改 `description`，简要说明软件包用途：

```xml
<description>Beginner client libraries tutorials practice package</description>
```

接着更新 `license`。有关开源许可证的说明，请参阅[开源许可证列表](https://opensource.org/licenses/alphabetical)。这里的软件包仅用于练习，可以选择任意许可证。本教程使用 `Apache License 2.0`：

```xml
<license>Apache License 2.0</license>
```

编辑完成后记得保存。

许可证标签下方有一些以 `_depend` 结尾的标签。`package.xml` 在这里声明对其他软件包的依赖，供 colcon 查找。`my_package` 很简单，没有其他软件包依赖；后续教程会使用这部分配置。

**CMake**

目前的配置已经完成。

**Python**

`setup.py` 中也包含与 `package.xml` 对应的说明、维护者和许可证字段，需要一起设置，确保两个文件中的值完全一致。版本和名称（`package_name`）也必须完全一致，这两项应该已自动填写。

用文本编辑器打开 `setup.py`：

```python
from setuptools import setup

package_name = 'my_py_pkg'

setup(
 name=package_name,
 version='0.0.0',
 packages=[package_name],
 data_files=[
     ('share/ament_index/resource_index/packages',
             ['resource/' + package_name]),
     ('share/' + package_name, ['package.xml']),
   ],
 install_requires=['setuptools'],
 zip_safe=True,
 maintainer='TODO',
 maintainer_email='TODO',
 description='TODO: Package description',
 license='TODO: License declaration',
 tests_require=['pytest'],
 entry_points={
     'console_scripts': [
             'my_node = my_py_pkg.my_node:main'
     ],
   },
)
```

修改 `maintainer`、`maintainer_email` 和 `description`，使其与 `package.xml` 一致，然后保存文件。

<span id="summary"></span>
## 小结

你已经创建了一个软件包，用于组织代码，并方便其他人使用。

创建命令自动生成了必需文件，随后通过 colcon 构建软件包，使其中的可执行程序能够在本地环境运行。

<span id="next-steps"></span>
## 后续步骤

接下来为软件包添加实际功能，从一个简单的发布者/订阅者系统开始。你可以选择使用 [C++](Writing-A-Simple-Cpp-Publisher-And-Subscriber.md) 或 [Python](Writing-A-Simple-Py-Publisher-And-Subscriber.md) 编写。
