<span id="id1"></span>
<span id="id2"></span>
<span id="id3"></span>
<span id="id4"></span>

<span id="using-variants"></span>
# 使用变体

元软件包本身不直接提供软件，而是依赖一组相关软件包，以便一次安装整组软件包。参见 [Debian 的元软件包说明](https://wiki.debian.org/metapackage)和 [Ubuntu 的元软件包说明](https://help.ubuntu.com/community/MetaPackages)。变体（variant）是针对常用 ROS 软件包组合提供的一组官方元软件包。

ROS 2 的各个变体在 [REP-2001](https://reps.openrobotics.org/rep-2001/) 中定义。

除了官方变体，还可以存在针对特定机构或机器人的元软件包，见 [REP-108](https://reps.openrobotics.org/rep-0108/#institution-specific)。

<span id="adding-variants"></span>
## 添加变体

如果一个新变体对 ROS 社区具有普遍用途，可以[通过拉取请求更新 REP-2001](https://github.com/openrobotics/reps/blob/main/_posts/rep-2001.md)，在提案中说明新变体包含哪些软件包。特定机构或机器人使用的变体可由各自维护者直接发布，无须更新 REP-2001。

<span id="creating-project-specific-variants"></span>
## 创建项目专用变体

如果正在为自己的项目创建私有 ROS 软件包，可以参照官方变体创建项目专用变体。只需创建两个文件：

1. 最小变体软件包采用 `ament_cmake` 构建类型，使用 `buildtool_depend` 声明对 `ament_cmake` 的依赖，并为要纳入变体的每个软件包添加一项 `exec_depend`。

```xml
<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/schema/package_format2.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="2">
  <name>my_project_variant</name>
  <version>1.0.0</version>
  <description>A package to aggregate all packages in my_project.</description>
  <maintainer email="maintainer-email">Maintainer Name</maintainer>
  <license>Apache License 2.0</license>
  <!-- packages in my_project -->
  <exec_depend>my_project_msgs</exec_depend>
  <exec_depend>my_project_services</exec_depend>
  <exec_depend>my_project_examples</exec_depend>

  <export>
    <build_type>ament_cmake</build_type>
  </export>
</package>
```

2. 最小 `ament_cmake` 软件包还需要一个 `CMakeLists.txt`，将 `package.xml` 注册为 ROS 2 中可用的 ament 软件包。

```cmake
cmake_minimum_required(VERSION 3.5)

project(my_project_variant NONE)
find_package(ament_cmake REQUIRED)
ament_package()
```

随后就可以与其他私有软件包一起构建、安装这个变体软件包。

<span id="creating-custom-variants-with-platform-specific-tools"></span>
### 使用平台专用工具创建自定义变体

一些平台提供了创建基础软件包的工具，无须完整的 ROS 构建农场环境或同等基础设施。这些工具也可以用于创建平台相关的变体。

这种方式不支持 ROS 打包工具，而且依赖特定平台；不过，如果只是将已有软件包组合起来，而不是混合使用公共和私有 ROS 软件包，所需的基础设施就少得多。例如，在 Debian 或 Ubuntu 上可以使用 `equivs` 工具。Debian 管理员手册提供了[关于元软件包的章节](https://www.debian.org/doc/manuals/debian-handbook/sect.building-first-package.en.html#id-1.18.5.2)。
