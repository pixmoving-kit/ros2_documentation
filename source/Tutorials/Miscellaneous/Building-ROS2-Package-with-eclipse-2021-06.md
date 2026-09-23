---
translation_status: machine_translated
source: Tutorials/Miscellaneous/Building-ROS2-Package-with-eclipse-2021-06.rst
---

<span id="building-a-package-with-eclipse-2021-06"></span>

# 使用 Eclipse 2021-06 构建软件包

您无法创建带有日蚀的ROS 2 软件包, 您需要使用命令行工具创建该软件包 。 [创建软件包](../Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.md) 教学。

在您创建了您的项目后, 您可以编辑源代码, 并用日蚀构建它 。

我们开始日食 并选择日食工作空间。

[![eclipse_work_dir](images/eclipse_work_dir.png)](images/eclipse_work_dir.png)

我们创建 C++ 项目

[![eclipse_create_c++\_project](images/eclipse_create_c%2B%2B_project.png)](images/eclipse_create_c%2B%2B_project.png) [![eclipse_c++\_project_select_type](images/eclipse_c%2B%2B_project_select_type.png)](images/eclipse_c%2B%2B_project_select_type.png)

我们看到,我们得到了C++包括。

[![eclipse_c++\_project_includes](images/eclipse_c%2B%2B_project_includes.png)](images/eclipse_c%2B%2B_project_includes.png)

我们现在输入了ROS 2项目 密码还在旧地方

[![eclipse_import_project](images/eclipse_import_project.png)](images/eclipse_import_project.png) [![eclipse_import_filesystem](images/eclipse_import_filesystem.png)](images/eclipse_import_filesystem.png) [![eclipse_import_select_my_package](images/eclipse_import_select_my_package.png)](images/eclipse_import_select_my_package.png)

我们在源代码中看到,C++包括了解决了,但没有解决ROS 2的.

[![eclipse_c++\_wo_ros_includes](images/eclipse_c%2B%2B_wo_ros_includes.png)](images/eclipse_c%2B%2B_wo_ros_includes.png) [![eclipse_c++\_path_and_symbols](images/eclipse_c%2B%2B_path_and_symbols.png)](images/eclipse_c%2B%2B_path_and_symbols.png) [![eclipse_c++\_add_directory_path](images/eclipse_c%2B%2B_add_directory_path.png)](images/eclipse_c%2B%2B_add_directory_path.png)

我们现在看到,《规则2》也得到了解决。

[![eclipse_c++\_indexer_ok](images/eclipse_c%2B%2B_indexer_ok.png)](images/eclipse_c%2B%2B_indexer_ok.png)

添加构建器 colcon, 这样我们就可以用右键点击项目和“ 构建工程” 来构建 。

[![eclipse_c++\_properties_builders](images/eclipse_c%2B%2B_properties_builders.png)](images/eclipse_c%2B%2B_properties_builders.png) [![eclipse_c++\_builder_main](images/eclipse_c%2B%2B_builder_main.png)](images/eclipse_c%2B%2B_builder_main.png)

有了PYTHONPATH,你也可以建造python项目.

[![eclipse_c++\_builder_env](images/eclipse_c%2B%2B_builder_env.png)](images/eclipse_c%2B%2B_builder_env.png) [![eclipse_c++\_properties_builders_with_colcon](images/eclipse_c%2B%2B_properties_builders_with_colcon.png)](images/eclipse_c%2B%2B_properties_builders_with_colcon.png)

右键点击项目并选择“ 建设工程 ” 。

[![eclipse_c++\_build_project_with_colcon](images/eclipse_c%2B%2B_build_project_with_colcon.png)](images/eclipse_c%2B%2B_build_project_with_colcon.png)
