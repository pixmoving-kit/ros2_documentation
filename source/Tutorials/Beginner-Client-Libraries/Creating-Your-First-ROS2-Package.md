---
translation_status: machine_translated
source: Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="creating-a-package"></span> <span id="createpkg"></span>

# 创建软件包

**目标：** 使用 CMake 或 Python 创建新软件包,并运行其可执行文件 。

**教程级别：** 入门

**用时：** 15分钟

<span id="background"></span>

## 背景

<span id="what-is-a-ros-2-package"></span>

### 1是什么ROS 2 包车?

软件包是您 ROS 2 代码的组织单位。 如果您想要安装您的代码或与他人共享, 您需要用软件包来组织它。 有了软件包, 您可以释放自己的 ROS 2 工作, 并允许其他人轻松构建和使用它 。

ROS 2 中的软件包创建将 Ament 作为它的构建系统, 并将 colcon 作为它的构建工具。 您可以使用 CMake 或 Python 创建一个软件包, 尽管其他的构建类型确实存在 。

<span id="what-makes-up-a-ros-2-package"></span>

### 2是什么构成 ROS 2包?

ROS 2 Python 和 CMake 软件包各有各自的最低要求内容:

##### CMake

- `CMakeLists.txt` 描述如何在软件包内构建代码的文件

- `include/<package_name>` 包含软件包公共信头的目录

- `package.xml` 包含关于软件包的元信息的文件

- `src` 包含软件包源代码的目录

##### Python

- `package.xml` 包含关于软件包的元信息的文件

- `resource/<package_name>` 包的标记文件

- `setup.cfg` 当软件包有可执行文件时需要执行,所以 `ros2 run` 可以找到他们

- `setup.py` 包含如何安装软件包的指令

- `<package_name>` - 与您的软件包同名的目录,由ROS 2 工具用于查找您的软件包,包含 `__init__.py`

最简单的可能包可能有一个文件结构,其外观类似:

##### CMake

``` console
my_package/
     CMakeLists.txt
     include/my_package/
     package.xml
     src/
```

##### Python

``` console
my_package/
      package.xml
      resource/my_package
      setup.cfg
      setup.py
      my_package/
```

<span id="packages-in-a-workspace"></span>

### 工作空间中的3个软件包

单个工作空间可以包含您想要的众多软件包, 每一个软件包都包含在自己的文件夹中。 您也可以在一个工作空间( CMake, Python等) 中拥有不同构建类型的软件包。 您不能拥有嵌入软件包 。

最佳做法是: `src` 在工作空间中创建文件夹,并在其中创建软件包。这保持了工作空间的顶层“清理”。

一个无关紧要的工作空间可能看起来像:

``` console
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

你应该有一个ROS 2工作空间 在遵循指令后 [上一个教程](Creating-A-Workspace/Creating-A-Workspace.md)。您将在此工作空间创建您的软件包。

<span id="tasks"></span>

## 操作步骤

<span id="create-a-package"></span>

### 1 创建软件包

首先,我们... [源代码 ROS 2 安装](../Beginner-CLI-Tools/Configuring-ROS2-Environment.md).

让我们利用您在您创建的工作空间 [上一个教程](Creating-A-Workspace/Creating-A-Workspace.md#new-directory), `ros2_ws`为了你的新包裹

确定你身处 `src` 文件夹在运行软件包创建命令之前。

##### Linux

``` console
$ cd ~/ros2_ws/src
```

##### macOS

``` console
$ cd ~/ros2_ws/src
```

##### Windows

``` console
$ cd \ros2_ws\src
```

在ROS 2中创建新软件包的命令语法是:

##### CMake

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 <package_name>
```

##### Python

``` console
$ ros2 pkg create --build-type ament_python --license Apache-2.0 <package_name>
```

对于此教程, 您将使用可选参数 `--node-name` 它在软件包中创建了一个简单的 Hello World 类型可执行文件。

在终端中输入以下命令:

##### CMake

``` console
$ ros2 pkg create --build-type ament_cmake --license Apache-2.0 --node-name my_node my_package
```

##### Python

``` console
$ ros2 pkg create --build-type ament_python --license Apache-2.0 --node-name my_node my_package
```

您现在将会在工作空间内有一个新文件夹 `src` 调用目录 `my_package`.

运行命令后, 您的终端将返回消息 :

##### CMake

``` console
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

##### Python

``` console
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

您可以看到新软件包的自动生成文件 。

<span id="build-a-package"></span>

### 2 构建软件包

将软件包放入工作空间尤其有价值,因为您可以同时通过运行构建许多软件包 `colcon build` 在工作空间根中。否则,您必须单独构建每个软件包。

返回您工作空间的根 :

##### Linux

``` console
$ cd ~/ros2_ws
```

##### macOS

``` console
$ cd ~/ros2_ws
```

##### Windows

``` console
$ cd \ros2_ws
```

现在,你可以构建您的软件包:

##### Linux

``` console
$ colcon build
```

##### macOS

``` console
$ colcon build
```

##### Windows

``` console
$ colcon build --merge-install
```

Windows 不允许长路径, 所以 `merge-install` 将所有路径结合到 `install` 目录。

从上一个教程中回忆起,您还有 `ros_tutorials` 在您的软件包中 `ros2_ws`。你可能已经注意到运行 `colcon build` 还建造了 `turtlesim` 软件包。如果工作空间中只有几个软件包,但有很多软件包,那就没事了。 `colcon build` 需要很长的时间

只有建造 `my_package` 下次,您可以运行 :

``` console
$ colcon build --packages-select my_package
```

<span id="source-the-setup-file"></span>

### 3 来源设置文件

要使用您的新软件包和可执行文件, 首先打开一个新的终端并源代码为您的主要ROS 2 安装 。

然后,从里面 `ros2_ws` 目录,运行以下命令以源代码工作空间:

##### Linux

``` console
$ source install/local_setup.bash
```

##### macOS

``` console
$ . install/local_setup.bash
```

##### Windows

``` console
$ call install/local_setup.bat
```

您的工作空间已被添加到您的路径中, 您将可以使用您的新软件包的可执行文件 。

<span id="use-the-package"></span>

### 4 使用软件包

要运行您创建的可执行文件 。 `--node-name` 创建软件包时的参数,输入命令:

``` console
$ ros2 run my_package my_node
```

这将返回一个消息到您的终端:

##### CMake

``` console
hello world my_package package
```

##### Python

``` console
Hi from my_package.
```

<span id="examine-package-contents"></span>

### 5 审查一揽子内容

内部 `ros2_ws/src/my_package`,您将看到文件和文件夹 `ros2 pkg create` 自动生成 :

##### CMake

``` console
CMakeLists.txt  include  package.xml  src
```

`my_node.cpp` 内在的 `src` 目录。这是您所有自定义的 C++ 节点将来都会去的地方。

##### Python

``` console
my_package  package.xml  resource  setup.cfg  setup.py  test
```

`my_node.py` 内在的 `my_package` 目录。 您所有自定义的 Python 节点将来都会在这里运行 。

<span id="customize-package-xml"></span>

### 6 自定义软件包.xml

您可能在创建软件包后的返回信件中注意到字段 `description` 财务报告和财务报告 `license` 包含 `TODO` 备注。这是因为软件包描述和许可证声明不是自动设置的,而是如果想要发布软件包,则需要这样做。 `maintainer` 字段也可能需要填入。

从 `ros2_ws/src/my_package`打开 `package.xml` 使用您首选的文本编辑器 :

##### CMake

``` xml
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

##### Python

``` xml
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

输入您的姓名和电子邮件到 `maintainer` 线条,如果它没有自动为您服务。然后编辑 `description` 线条以概括软件包 :

``` xml
<description>Beginner client libraries tutorials practice package</description>
```

然后,更新 `license` 线条。您可以读取更多关于开源许可证的内容 [这儿](https://opensource.org/licenses/alphabetical)。由于这个包只用于实践,因此使用任何许可证都是安全的。我们将使用 `Apache License 2.0`:

``` xml
<license>Apache License 2.0</license>
```

编辑完成后, 不要忘记保存。

在牌照牌照下面,你会看到一些牌照名的结尾 `_depend`。这是你的 `package.xml` 将会列出它对其他软件包的依赖性, 以便Colcon 搜索 。 `my_package` 简单且没有任何依赖关系, 但您会看到此空间被使用在即将到来的教程中 。

##### CMake

你们现在都完了!

##### Python

那个... `setup.py` 文件包含的描述、维护者和许可字段与 `package.xml`,所以您也需要设置这些。它们需要在两个文件中精确匹配。版本和名称(`package_name`)还需要精确匹配,并且应当自动地将两者都包含在两个文件中.

打开 `setup.py` 与您首选的文本编辑器。

``` python
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

编辑 `maintainer`, `maintainer_email`,以及 `description` 要匹配的线条 `package.xml`.

别忘了保存文件。

<span id="summary"></span>

## 小结

您创建了一个包来组织您的代码, 并方便他人使用。

您的软件包被自动装入了必要的文件, 然后您使用colcon来构建它, 这样您就可以在本地环境中使用它的可执行文件 。

<span id="next-steps"></span>

## 后续步骤

接下来,让我们在软件包中添加一些有意义的内容。您将从一个简单的出版商/订阅商系统开始,您可以选择在其中任何一个中写入 [C++](Writing-A-Simple-Cpp-Publisher-And-Subscriber.md) 或 时 间 [Python](Writing-A-Simple-Py-Publisher-And-Subscriber.md).
