---
translation_status: machine_translated
source: Releases/Lyrical-Luth-Complete-Changelog.rst
---

<span id="ros-2-lyrical-luth-complete-changelog"></span>

# ROS 2 Lyrical Luth 完整变更日志

本页面是自上一期发布以来所有ROS 2核心包的完整更改列表.

<span id="action-msgs"></span>

## [action_msgs](https://github.com/ros2/rcl_interfaces/tree/lyrical/action_msgs/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#180](https://github.com/ros2/rcl_interfaces/issues/180))

- 贡献者:苔藓80

<span id="action-tutorials-cpp"></span>

## [action_tutorials_cpp](https://github.com/ros2/demos/tree/lyrical/action_tutorials/action_tutorials_cpp/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#781](https://github.com/ros2/demos//issues/781))

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714)) demo_nodes_cpp/CMakeLists.txt 需要cmake min 版本 3.12 其他模块 cmake 3.5 。它被提议与 3.12 版本标准化。它也修正 cmake \< 3.10 折旧警告

- 更新动作 cpp 演示支持设置内存( U)[\#709](https://github.com/ros2/demos/issues/709)) \* 更新动作 cpp 演示支持设置内存 \* 添加缺失头文件声明 \*

- 贡献者:许巴里,艾默森·克纳普,苔藓80

<span id="action-tutorials-py"></span>

## [action_tutorials_py](https://github.com/ros2/demos/tree/lyrical/action_tutorials/action_tutorials_py/CHANGELOG.rst)

- 动作_tutorys_py: 添加 ament_mypy 支持([\#775](https://github.com/ros2/demos//issues/775))

- 固定设置工具[\#733](https://github.com/ros2/demos/issues/733))

- 在动作_tutoris_py动作服务器中支持取消处理器 。 ()[\#727](https://github.com/ros2/demos/issues/727))

- 更新动作 python 演示支持设置内存([\#708](https://github.com/ros2/demos/issues/708)\* 更新动作 python 演示支持设置内存 \* 校正文档 QQ中的错误

- 贡献者:徐巴里、藤田丰也、莫希特、莫费特80

<span id="ament-clang-format"></span>

## [ament_clang_format](https://github.com/ament/ament_lint/tree/lyrical/ament_clang_format/CHANGELOG.rst)

- \[ament_mypy\] 修复 ament_cmake 软件包的配置和类型的切入点([\#574](https://github.com/ament/ament_lint/issues/574))

- 安装时丢弃设置工具( R)[\#566](https://github.com/ament/ament_lint/issues/566))

- 导出 mentlinters 的打字信息( Q)[\#553](https://github.com/ament/ament_lint/issues/553))

- 固定设置工具[\#547](https://github.com/ament/ament_lint/issues/547))

- 贡献者:Jochen Sprickerhof、Michael Carlstrom、苔藓80

<span id="ament-clang-tidy"></span>

## [ament_clang_tidy](https://github.com/ament/ament_lint/tree/lyrical/ament_clang_tidy/CHANGELOG.rst)

- \[ament_mypy\] 修复 ament_cmake 软件包的配置和类型的切入点([\#574](https://github.com/ament/ament_lint/issues/574))

- 安装时丢弃设置工具( R)[\#566](https://github.com/ament/ament_lint/issues/566))

- 导出 mentlinters 的打字信息( Q)[\#553](https://github.com/ament/ament_lint/issues/553))

- 固定设置工具[\#547](https://github.com/ament/ament_lint/issues/547))

- 贡献者:Jochen Sprickerhof、Michael Carlstrom、苔藓80

<span id="ament-cmake"></span>

## [ament_cmake](https://github.com/ament/ament_cmake/tree/lyrical/ament_cmake/CHANGELOG.rst)

- 删除已贬值的 ament\_ cmake_export_interfaces 软件包([\#581](https://github.com/ament/ament_cmake/issues/581))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗

<span id="ament-cmake-auto"></span>

## [ament_cmake_auto](https://github.com/ament/ament_cmake/tree/lyrical/ament_cmake_auto/CHANGELOG.rst)

- 在使用时不要出错_SCOPED_HEADER_INSTALL_DIR([\#596](https://github.com/ament/ament_cmake/issues/596))

- 撰稿人:蒂姆·克莱法斯

<span id="ament-cmake-clang-format"></span>

## [ament_cmake_clang_format](https://github.com/ament/ament_lint/tree/lyrical/ament_cmake_clang_format/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 允许通过 CMake 实现压倒性 clang 格式版本( S)[\#536](https://github.com/ament/ament_lint/issues/536))

- 贡献者:内森·维贝·诺伊费尔特、苔藓80

<span id="ament-cmake-clang-tidy"></span>

## [ament_cmake_clang_tidy](https://github.com/ament/ament_lint/tree/lyrical/ament_cmake_clang_tidy/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 贡献者:苔藓80

<span id="ament-cmake-copyright"></span>

## [ament_cmake_copyright](https://github.com/ament/ament_lint/tree/lyrical/ament_cmake_copyright/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 贡献者:苔藓80

<span id="ament-cmake-core"></span>

## [ament_cmake_core](https://github.com/ament/ament_cmake/tree/lyrical/ament_cmake_core/CHANGELOG.rst)

- 删除未使用的 AMENT_CMAKE_ENVIRONMENT_Generation 选项([\#354](https://github.com/ament/ament_cmake/issues/354))

- 地址 ament_lint_cmaking回归([\#604](https://github.com/ament/ament_cmake/issues/604))

- 从 ament_cmake_core (QUIET) 在链条中尊重 find_package(QUIET)[\#603](https://github.com/ament/ament_cmake/issues/603))

- perf: 使用 cmake\_ path 更快的正常路径执行( P)[\#586](https://github.com/ament/ament_cmake/issues/586))

- 贡献者:内森·布瓦德、斯科特·K·洛根、谢恩·洛雷茨

<span id="ament-cmake-cppcheck"></span>

## [ament_cmake_cppcheck](https://github.com/ament/ament_lint/tree/lyrical/ament_cmake_cppcheck/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 贡献者:苔藓80

<span id="ament-cmake-cpplint"></span>

## [ament_cmake_cpplint](https://github.com/ament/ament_lint/tree/lyrical/ament_cmake_cpplint/CHANGELOG.rst)

- 修正EXCLUDE一致性([\#481](https://github.com/ament/ament_lint/issues/481))

- cpplint: 上游 cpplint repo 的更新链接 ([\#538](https://github.com/ament/ament_lint/issues/538))

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 贡献者:Romain Reignier、Tom Moore、mossfet80

<span id="ament-cmake-export-targets"></span>

## [ament_cmake_export_targets](https://github.com/ament/ament_cmake/tree/lyrical/ament_cmake_export_targets/CHANGELOG.rst)

- 地址 ament_lint_cmaking回归([\#604](https://github.com/ament/ament_cmake/issues/604))

- 撰稿人:斯科特·K·洛根

<span id="ament-cmake-flake8"></span>

## [ament_cmake_flake8](https://github.com/ament/ament_lint/tree/lyrical/ament_cmake_flake8/CHANGELOG.rst)

- 修正EXCLUDE一致性([\#481](https://github.com/ament/ament_lint/issues/481))

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 贡献者:Tom Moore, mossfet80

<span id="ament-cmake-gen-version-h"></span>

## [ament_cmake_gen_version_h](https://github.com/ament/ament_cmake/tree/lyrical/ament_cmake_gen_version_h/CHANGELOG.rst)

- 地址 ament_lint_cmaking回归([\#604](https://github.com/ament/ament_cmake/issues/604))

- 更新 CMake 要求 (Y)[\#589](https://github.com/ament/ament_cmake/issues/589))

- 删除已贬值的函数 ament_cmake_gen_version_h ([\#582](https://github.com/ament/ament_cmake/issues/582))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、斯科特·K·洛根、苔藓80

<span id="ament-cmake-gmock"></span>

## [ament_cmake_gmock](https://github.com/ament/ament_cmake/tree/lyrical/ament_cmake_gmock/CHANGELOG.rst)

- 使用 libgtest- dev 和 libgmock- dev (英语)[\#622](https://github.com/ament/ament_cmake//issues/622))

- 撰稿人:谢恩·洛雷茨

<span id="ament-cmake-gtest"></span>

## [ament_cmake_gtest](https://github.com/ament/ament_cmake/tree/lyrical/ament_cmake_gtest/CHANGELOG.rst)

- 使用 libgtest- dev 和 libgmock- dev (英语)[\#622](https://github.com/ament/ament_cmake//issues/622))

- 撰稿人:谢恩·洛雷茨

<span id="ament-cmake-libraries"></span>

## [ament_cmake_libraries](https://github.com/ament/ament_cmake/tree/lyrical/ament_cmake_libraries/CHANGELOG.rst)

- 地址 ament_lint_cmaking回归([\#604](https://github.com/ament/ament_cmake/issues/604))

- 撰稿人:斯科特·K·洛根

<span id="ament-cmake-lint-cmake"></span>

## [ament_cmake_lint_cmake](https://github.com/ament/ament_lint/tree/lyrical/ament_cmake_lint_cmake/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 贡献者:苔藓80

<span id="ament-cmake-mypy"></span>

## [ament_cmake_mypy](https://github.com/ament/ament_lint/tree/lyrical/ament_cmake_mypy/CHANGELOG.rst)

- \[ament_mypy\] 添加 `--ament-strict` 用于更严格的类型检查的旗帜。 ()[\#573](https://github.com/ament/ament_lint/issues/573))

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 贡献者:迈克尔·卡尔斯特罗姆、苔藓80

<span id="ament-cmake-pclint"></span>

## [ament_cmake_pclint](https://github.com/ament/ament_lint/tree/lyrical/ament_cmake_pclint/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 贡献者:苔藓80

<span id="ament-cmake-pep257"></span>

## [ament_cmake_pep257](https://github.com/ament/ament_lint/tree/lyrical/ament_cmake_pep257/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 贡献者:苔藓80

<span id="ament-cmake-pycodestyle"></span>

## [ament_cmake_pycodestyle](https://github.com/ament/ament_lint/tree/lyrical/ament_cmake_pycodestyle/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 贡献者:苔藓80

<span id="ament-cmake-pyflakes"></span>

## [ament_cmake_pyflakes](https://github.com/ament/ament_lint/tree/lyrical/ament_cmake_pyflakes/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 贡献者:苔藓80

<span id="ament-cmake-python"></span>

## [ament_cmake_python](https://github.com/ament/ament_cmake/tree/lyrical/ament_cmake_python/CHANGELOG.rst)

- 特性: 允许在 `ament_python_install_package` ([\#587](https://github.com/ament/ament_cmake//issues/587))

- 添加缺失的依赖( E)[\#617](https://github.com/ament/ament_cmake/issues/617))

- 贡献者:纳达夫·埃尔卡贝茨、罗伯特·哈施克

<span id="ament-cmake-python-test"></span>

## [ament_cmake_python_test](https://github.com/ament/ament_cmake/tree/lyrical/ament_cmake_python_test/CHANGELOG.rst)

- 特性: 允许在 `ament_python_install_package` ([\#587](https://github.com/ament/ament_cmake//issues/587))

- 撰稿人:纳达夫·埃尔卡贝茨

<span id="ament-cmake-ros"></span>

## [ament_cmake_ros](https://github.com/ros2/ament_cmake_ros/tree/lyrical/ament_cmake_ros/CHANGELOG.rst)

- 固定cmake 折旧([\#47](https://github.com/ros2/ament_cmake_ros/issues/47))

- 贡献者:苔藓80

<span id="ament-cmake-ros-core"></span>

## [ament_cmake_ros_core](https://github.com/ros2/ament_cmake_ros/tree/lyrical/ament_cmake_ros_core/CHANGELOG.rst)

- 添加 `ament_ros_defaults` 目标([\#62](https://github.com/ros2/ament_cmake_ros/issues/62))

- 固定cmake 折旧([\#47](https://github.com/ros2/ament_cmake_ros/issues/47))

- 贡献者:迈克尔·卡尔斯特罗姆、苔藓80

<span id="ament-cmake-target-dependencies"></span>

## [ament_cmake_target_dependencies](https://github.com/ament/ament_cmake/tree/lyrical/ament_cmake_target_dependencies/CHANGELOG.rst)

- 重置“ 已撤销的已贬值函数 ament\_ cmake\_ target\_ dependenities(...) ” (“...”)[\#614](https://github.com/ament/ament_cmake/issues/614))

- 重置“已撤销的折旧函数 ament_cmake_target_dependenities”([\#585](https://github.com/ament/ament_cmake/issues/585))

- 删除已贬值的函数 ament_cmake_target_dependency([\#583](https://github.com/ament/ament_cmake/issues/583))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、谢恩·洛雷茨

<span id="ament-cmake-uncrustify"></span>

## [ament_cmake_uncrustify](https://github.com/ament/ament_lint/tree/lyrical/ament_cmake_uncrustify/CHANGELOG.rst)

- \[ament_cmake_uncrustify\] 添加ament_cmake_uncrustify_LANGUAGE变量(英语:[\#384](https://github.com/ament/ament_lint/issues/384))

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 贡献者: Abrar Rahman Protyasha, mossfet80

<span id="ament-cmake-vendor-package"></span>

## [ament_cmake_vendor_package](https://github.com/ament/ament_cmake/tree/lyrical/ament_cmake_vendor_package/CHANGELOG.rst)

- ament_vendor: 向外部项目添加变量([\#593](https://github.com/ament/ament_cmake/issues/593))

- 贡献者:西尔维奥·特拉韦萨罗

<span id="ament-cmake-xmllint"></span>

## [ament_cmake_xmllint](https://github.com/ament/ament_lint/tree/lyrical/ament_cmake_xmllint/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 贡献者:苔藓80

<span id="ament-copyright"></span>

## [ament_copyright](https://github.com/ament/ament_lint/tree/lyrical/ament_copyright/CHANGELOG.rst)

- \[ament_mypy\] 修复 ament_cmake 软件包的配置和类型的切入点([\#574](https://github.com/ament/ament_lint/issues/574))

- 安装时丢弃设置工具( R)[\#566](https://github.com/ament/ament_lint/issues/566))

- 删除 `importlib_metadata` ([\#564](https://github.com/ament/ament_lint/issues/564))

- 删除无效的许可模板 。 ()[\#209](https://github.com/ament/ament_lint/issues/209))

- 导出 mentlinters 的打字信息( Q)[\#553](https://github.com/ament/ament_lint/issues/553))

- 固定设置工具[\#547](https://github.com/ament/ament_lint/issues/547))

- 贡献者:乔亨·斯普里克霍夫、迈克尔·卡尔斯特罗姆、图利·福特、苔藓80

<span id="ament-cppcheck"></span>

## [ament_cppcheck](https://github.com/ament/ament_lint/tree/lyrical/ament_cppcheck/CHANGELOG.rst)

- \[ament_mypy\] 修复 ament_cmake 软件包的配置和类型的切入点([\#574](https://github.com/ament/ament_lint/issues/574))

- 安装时丢弃设置工具( R)[\#566](https://github.com/ament/ament_lint/issues/566))

- 导出 mentlinters 的打字信息( Q)[\#553](https://github.com/ament/ament_lint/issues/553))

- 固定设置工具[\#547](https://github.com/ament/ament_lint/issues/547))

- 贡献者:Jochen Sprickerhof、Michael Carlstrom、苔藓80

<span id="ament-cpplint"></span>

## [ament_cpplint](https://github.com/ament/ament_lint/tree/lyrical/ament_cpplint/CHANGELOG.rst)

- \[ament_mypy\] 修复 ament_cmake 软件包的配置和类型的切入点([\#574](https://github.com/ament/ament_lint/issues/574))

- 安装时丢弃设置工具( R)[\#566](https://github.com/ament/ament_lint/issues/566))

- 导出 mentlinters 的打字信息( Q)[\#553](https://github.com/ament/ament_lint/issues/553))

- 固定设置工具[\#547](https://github.com/ament/ament_lint/issues/547))

- cpplint: 上游 cpplint repo 的更新链接 ([\#538](https://github.com/ament/ament_lint/issues/538))

- 贡献者:Jochen Sprickerhof、迈克尔·卡尔斯特罗姆、Romain Reignier、mosfet80

<span id="ament-flake8"></span>

## [ament_flake8](https://github.com/ament/ament_lint/tree/lyrical/ament_flake8/CHANGELOG.rst)

- \[ament_mypy\] 添加 `--ament-strict` 用于更严格的类型检查的旗帜。 ()[\#573](https://github.com/ament/ament_lint/issues/573))

- 安装时丢弃设置工具( R)[\#566](https://github.com/ament/ament_lint/issues/566))

- 放弃对蟒蛇3-flake8-docstrings的依赖([\#513](https://github.com/ament/ament_lint/issues/513))

- 导出 mentlinters 的打字信息( Q)[\#553](https://github.com/ament/ament_lint/issues/553))

- 固定设置工具[\#547](https://github.com/ament/ament_lint/issues/547))

- 贡献者:乔亨·斯普里克霍夫,迈克尔·卡尔斯特罗姆,斯科特·K·洛根,苔藓80

<span id="ament-index-cpp"></span>

## [ament_index_cpp](https://github.com/ament/ament_index/tree/lyrical/ament_index_cpp/CHANGELOG.rst)

- 清理( E)[\#114](https://github.com/ament/ament_index/issues/114))

- 使用“ package_share\_ path” 和“ python” 一样( python)[\#112](https://github.com/ament/ament_index//issues/112))

- 添加自动生成的版本头( E)[\#105](https://github.com/ament/ament_index/issues/105))

- 将 API 扩展为使用 std: file system ()[\#104](https://github.com/ament/ament_index/issues/104))

- CMake 折旧( R)[\#102](https://github.com/ament/ament_index/issues/102))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、埃里克·卢詹、蒂姆·克莱法斯、莫斯费特80

<span id="ament-index-python"></span>

## [ament_index_python](https://github.com/ament/ament_index/tree/lyrical/ament_index_python/CHANGELOG.rst)

- 清理 ament_index_python (英语).[\#115](https://github.com/ament/ament_index/issues/115))

- 固定设置工具[\#101](https://github.com/ament/ament_index/issues/101))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,苔藓80

<span id="ament-lint"></span>

## [ament_lint](https://github.com/ament/ament_lint/tree/lyrical/ament_lint/CHANGELOG.rst)

- 安装时丢弃设置工具( R)[\#566](https://github.com/ament/ament_lint/issues/566))

- 导出 mentlinters 的打字信息( Q)[\#553](https://github.com/ament/ament_lint/issues/553))

- 固定设置工具[\#547](https://github.com/ament/ament_lint/issues/547))

- 贡献者:Jochen Sprickerhof、Michael Carlstrom、苔藓80

<span id="ament-lint-auto"></span>

## [ament_lint_auto](https://github.com/ament/ament_lint/tree/lyrical/ament_lint_auto/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 贡献者:苔藓80

<span id="ament-lint-cmake"></span>

## [ament_lint_cmake](https://github.com/ament/ament_lint/tree/lyrical/ament_lint_cmake/CHANGELOG.rst)

- 安装时丢弃设置工具( R)[\#566](https://github.com/ament/ament_lint/issues/566))

- 导出 mentlinters 的打字信息( Q)[\#553](https://github.com/ament/ament_lint/issues/553))

- 固定设置工具[\#547](https://github.com/ament/ament_lint/issues/547))

- 贡献者:Jochen Sprickerhof、Michael Carlstrom、苔藓80

<span id="ament-lint-common"></span>

## [ament_lint_common](https://github.com/ament/ament_lint/tree/lyrical/ament_lint_common/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#539](https://github.com/ament/ament_lint/issues/539))

- 贡献者:苔藓80

<span id="ament-mypy"></span>

## [ament_mypy](https://github.com/ament/ament_lint/tree/lyrical/ament_mypy/CHANGELOG.rst)

- 允许未使用的忽略( N)[\#575](https://github.com/ament/ament_lint/issues/575))

- \[ament_mypy\] 修复 ament_cmake 软件包的配置和类型的切入点([\#574](https://github.com/ament/ament_lint/issues/574))

- \[ament_mypy\] 添加 `--ament-strict` 用于更严格的类型检查的旗帜。 ()[\#573](https://github.com/ament/ament_lint/issues/573))

- 安装时丢弃设置工具( R)[\#566](https://github.com/ament/ament_lint/issues/566))

- 导出 mentlinters 的打字信息( Q)[\#553](https://github.com/ament/ament_lint/issues/553))

- 固定设置工具[\#547](https://github.com/ament/ament_lint/issues/547))

- 贡献者:Jochen Sprickerhof、Michael Carlstrom、苔藓80

<span id="ament-package"></span>

## [ament_package](https://github.com/ament/ament_package/tree/lyrical/CHANGELOG.rst)

- 成就:增加支持鱼类([\#164](https://github.com/ament/ament_package/issues/164))

- 修正片段8 (% 1)[\#163](https://github.com/ament/ament_package/issues/163))

- 删除不需要的音符( E)[\#161](https://github.com/ament/ament_package/issues/161))

- 固定设置工具[\#156](https://github.com/ament/ament_package/issues/156))

- 贡献者:迈克尔·卡尔斯特罗姆、斯佩克、苔藓80

<span id="ament-pclint"></span>

## [ament_pclint](https://github.com/ament/ament_lint/tree/lyrical/ament_pclint/CHANGELOG.rst)

- \[ament_mypy\] 修复 ament_cmake 软件包的配置和类型的切入点([\#574](https://github.com/ament/ament_lint/issues/574))

- 安装时丢弃设置工具( R)[\#566](https://github.com/ament/ament_lint/issues/566))

- 导出 mentlinters 的打字信息( Q)[\#553](https://github.com/ament/ament_lint/issues/553))

- 清理设置.py([\#552](https://github.com/ament/ament_lint/issues/552))

- 固定设置工具 折旧( )[\#551](https://github.com/ament/ament_lint/issues/551))

- 固定设置工具[\#547](https://github.com/ament/ament_lint/issues/547))

- 贡献者:Jochen Sprickerhof、Michael Carlstrom、苔藓80

<span id="ament-pep257"></span>

## [ament_pep257](https://github.com/ament/ament_lint/tree/lyrical/ament_pep257/CHANGELOG.rst)

- 如果无法导入, 请跳过 pydoctyle 测试([\#579](https://github.com/ament/ament_lint/issues/579))

- \[ament_mypy\] 修复 ament_cmake 软件包的配置和类型的切入点([\#574](https://github.com/ament/ament_lint/issues/574))

- 安装时丢弃设置工具( R)[\#566](https://github.com/ament/ament_lint/issues/566))

- 导出 mentlinters 的打字信息( Q)[\#553](https://github.com/ament/ament_lint/issues/553))

- 固定设置工具[\#547](https://github.com/ament/ament_lint/issues/547))

- 贡献者:乔亨·斯普里克霍夫,迈克尔·卡尔斯特罗姆,斯科特·K·洛根,苔藓80

<span id="ament-pycodestyle"></span>

## [ament_pycodestyle](https://github.com/ament/ament_lint/tree/lyrical/ament_pycodestyle/CHANGELOG.rst)

- \[ament_mypy\] 修复 ament_cmake 软件包的配置和类型的切入点([\#574](https://github.com/ament/ament_lint/issues/574))

- 安装时丢弃设置工具( R)[\#566](https://github.com/ament/ament_lint/issues/566))

- 导出 mentlinters 的打字信息( Q)[\#553](https://github.com/ament/ament_lint/issues/553))

- 固定设置工具[\#547](https://github.com/ament/ament_lint/issues/547))

- 贡献者:Jochen Sprickerhof、Michael Carlstrom、苔藓80

<span id="ament-pyflakes"></span>

## [ament_pyflakes](https://github.com/ament/ament_lint/tree/lyrical/ament_pyflakes/CHANGELOG.rst)

- \[ament_mypy\] 修复 ament_cmake 软件包的配置和类型的切入点([\#574](https://github.com/ament/ament_lint/issues/574))

- 安装时丢弃设置工具( R)[\#566](https://github.com/ament/ament_lint/issues/566))

- 导出 mentlinters 的打字信息( Q)[\#553](https://github.com/ament/ament_lint/issues/553))

- 固定设置工具[\#547](https://github.com/ament/ament_lint/issues/547))

- 贡献者:Jochen Sprickerhof、Michael Carlstrom、苔藓80

<span id="ament-uncrustify"></span>

## [ament_uncrustify](https://github.com/ament/ament_lint/tree/lyrical/ament_uncrustify/CHANGELOG.rst)

- \[ament_mypy\] 修复 ament_cmake 软件包的配置和类型的切入点([\#574](https://github.com/ament/ament_lint/issues/574))

- 安装时丢弃设置工具( R)[\#566](https://github.com/ament/ament_lint/issues/566))

- 重置“ 已卸载的未卸载的\_ vendor( 返回) ”[\#556](https://github.com/ament/ament_lint/issues/556))” ([\#561](https://github.com/ament/ament_lint/issues/561))

- 删除的未校验\_ vendor ()[\#556](https://github.com/ament/ament_lint/issues/556))

- 导出 mentlinters 的打字信息( Q)[\#553](https://github.com/ament/ament_lint/issues/553))

- 固定设置工具[\#547](https://github.com/ament/ament_lint/issues/547))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、约亨·斯普里克霍夫、迈克尔·卡尔斯特罗姆、迈克尔·奥尔洛夫、莫斯费特80

<span id="ament-xmllint"></span>

## [ament_xmllint](https://github.com/ament/ament_lint/tree/lyrical/ament_xmllint/CHANGELOG.rst)

- \[ament_mypy\] 添加 `--ament-strict` 用于更严格的类型检查的旗帜。 ()[\#573](https://github.com/ament/ament_lint/issues/573))

- xmllint: 通过 Python 获取外部计划([\#570](https://github.com/ament/ament_lint/issues/570))

- 安装时丢弃设置工具( R)[\#566](https://github.com/ament/ament_lint/issues/566))

- 导出 mentlinters 的打字信息( Q)[\#553](https://github.com/ament/ament_lint/issues/553))

- 固定设置工具[\#547](https://github.com/ament/ament_lint/issues/547))

- 贡献者:乔亨·斯普里克霍夫、迈克尔·卡尔斯特罗姆、迈克尔·卡罗尔、莫斯费特80

<span id="builtin-interfaces"></span>

## [builtin_interfaces](https://github.com/ros2/rcl_interfaces/tree/lyrical/builtin_interfaces/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#180](https://github.com/ros2/rcl_interfaces/issues/180))

- 添加时间消息和时间消息注释的信息( Q)[\#176](https://github.com/ros2/rcl_interfaces/issues/176))

- 贡献者:Jimmy McElwain,苔藓80

<span id="camera-calibration-parsers"></span>

## [camera_calibration_parsers](https://github.com/ros-perception/image_common/tree/lyrical/camera_calibration_parsers/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#396](https://github.com/ros-perception/image_common/issues/396))

- 删除相机_校准_parsers/ setup.py([\#393](https://github.com/ros-perception/image_common/issues/393))

- 将 BSD 许可证更新到 SPDX 标识符([\#389](https://github.com/ros-perception/image_common/issues/389))合著:亚历杭德罗·赫尔南德斯·科尔德罗 \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 修补 cmake 折旧( E)[\#367](https://github.com/ros-perception/image_common/issues/367))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#345](https://github.com/ros-perception/image_common/issues/345))

- 贡献者:爱默生·克纳普,加勒特·布朗,迈克尔·卡尔斯特罗姆,谢恩·洛雷茨,摩斯费特80

<span id="camera-info-manager"></span>

## [camera_info_manager](https://github.com/ros-perception/image_common/tree/lyrical/camera_info_manager/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#396](https://github.com/ros-perception/image_common/issues/396))

- 添加相机信息管理器单元测试( E)[\#358](https://github.com/ros-perception/image_common/issues/358))

- 使用 get\_ package_share\_ path ([\#391](https://github.com/ros-perception/image_common/issues/391))

- 将 BSD 许可证更新到 SPDX 标识符([\#389](https://github.com/ros-perception/image_common/issues/389))合著:亚历杭德罗·赫尔南德斯·科尔德罗 \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 更新已贬值的 ament_index_cpp API ([\#388](https://github.com/ros-perception/image_common/issues/388))

- 以 crange 修正编译错误 (S)[\#372](https://github.com/ros-perception/image_common/issues/372))

- 支持生命周期节点 - 节点界面( )[\#352](https://github.com/ros-perception/image_common/issues/352))

- 已贬值的rmw_qos\_ profile_t 支持rclcpp: QoS (中文(简体) ).[\#364](https://github.com/ros-perception/image_common/issues/364))

- 修补 cmake 折旧( E)[\#367](https://github.com/ros-perception/image_common/issues/367))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、埃默森·克纳普、加勒特·布朗、苔丝菲特80

<span id="camera-info-manager-py"></span>

## [camera_info_manager_py](https://github.com/ros-perception/image_common/tree/lyrical/camera_info_manager_py/CHANGELOG.rst)

- 将 BSD 许可证更新到 SPDX 标识符([\#389](https://github.com/ros-perception/image_common/issues/389))合著:亚历杭德罗·赫尔南德斯·科尔德罗 \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 清理错误标签的 BSD 许可证( Name[\#382](https://github.com/ros-perception/image_common/issues/382))

- 固定设置工具 折旧( )[\#366](https://github.com/ros-perception/image_common/issues/366))

- 修正相机Info扭曲系数和记录器([\#360](https://github.com/ros-perception/image_common/issues/360))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,加勒特·布朗,里克-v-E,摩斯费特80

<span id="class-loader"></span>

## [class_loader](https://github.com/ros/class_loader/tree/lyrical/CHANGELOG.rst)

- 将编译器错误修复为 crange ()[\#227](https://github.com/ros/class_loader/issues/227))

- 删除 ament_cmake_ros 依赖([\#226](https://github.com/ros/class_loader/issues/226)依赖的 ament_cmake_ros 包在 RTW 级包中转动拉动,该包不必要地重到应该为独立插件加载库的类。此承诺将删除 ament_cmake_ros 依赖性,并用一个明确的 shared 库类型替换为 plain ament_cmake,以保持依赖性最小化 。

- 改进( E)[\#225](https://github.com/ros/class_loader/issues/225))

- 清扫测试( E)[\#224](https://github.com/ros/class_loader/issues/224))

- 添加对向构造器传递参数的支持( E)[\#223](https://github.com/ros/class_loader//issues/223))

- 线索和地址 Sanitizer CI([\#198](https://github.com/ros/class_loader/issues/198))

- 更新 cmake 要求

- 删除 CODEOWINERS 和镜像滚动到主工作流程([\#215](https://github.com/ros/class_loader/issues/215))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、CY陈、泰勒·韦弗、苔藓·费特80、pum1k

<span id="common-interfaces"></span>

## [common_interfaces](https://github.com/ros2/common_interfaces/tree/lyrical/common_interfaces/CHANGELOG.rst)

- 修复 CMAKE 折旧[\#288](https://github.com/ros2/common_interfaces/issues/288))

- 删除已贬值的动作lib_msgs([\#280](https://github.com/ros2/common_interfaces/issues/280))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,苔藓80

<span id="composition"></span>

## [组成](https://github.com/ros2/demos/tree/lyrical/composition/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#781](https://github.com/ros2/demos//issues/781))

- 在 test_dlopen_composition.py.in 和 test_linktime_composition.py.in 中加入测试隔离(英语:[\#764](https://github.com/ros2/demos//issues/764))

- 切换到示例_界面( S)[\#674](https://github.com/ros2/demos/issues/674))

- Windows上链接时间构成的日志信息( E)[\#640](https://github.com/ros2/demos/issues/640))

- 共享库的正确名称及其位置( Q)[\#722](https://github.com/ros2/demos/issues/722)) ([\#726](https://github.com/ros2/demos/issues/726))

- 在发射测试中使用 EullRmwIsolation ()[\#724](https://github.com/ros2/demos/issues/724))

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 设定 Envars 以 rmw_zenoh_cpp 进行多播发现的测试([\#711](https://github.com/ros2/demos/issues/711))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#707](https://github.com/ros2/demos/issues/707))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,爱默生·克纳普,朱利安·埃诺赫,卢卡斯·温德兰,斯科特·克·洛根,谢恩·洛雷茨,注音\[bot\],mosfet80,yadundund

<span id="composition-interfaces"></span>

## [composition_interfaces](https://github.com/ros2/rcl_interfaces/tree/lyrical/composition_interfaces/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#180](https://github.com/ros2/rcl_interfaces/issues/180))

- 贡献者:苔藓80

<span id="console-bridge-vendor"></span>

## [console_bridge_vendor](https://github.com/ros2/console_bridge_vendor/tree/lyrical/CHANGELOG.rst)

- 在此更新 CMake 版本和控制台\_ 桥( C)[\#44](https://github.com/ros2/console_bridge_vendor/issues/44))

- 删除 CODEOWINERS 和镜像滚动到主工作流程 。 ([\#42](https://github.com/ros2/console_bridge_vendor/issues/42))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特

<span id="demo-nodes-cpp"></span>

## [demo_nodes_cpp](https://github.com/ros2/demos/tree/lyrical/demo_nodes_cpp/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#781](https://github.com/ros2/demos//issues/781))

- 在父节点下添加子日志, 日志级别不同 。 ()[\#772](https://github.com/ros2/demos//issues/772))

- 切换到示例_界面( S)[\#674](https://github.com/ros2/demos/issues/674))

- 更新订阅回调签名( N)[\#754](https://github.com/ros2/demos/issues/754))

- 在发射测试中使用 EullRmwIsolation ()[\#724](https://github.com/ros2/demos/issues/724))

- 在 Docs demo_nodes_cpp 中修正打字([\#715](https://github.com/ros2/demos/issues/715))

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 设定 Envars 以 rmw_zenoh_cpp 进行多播发现的测试([\#711](https://github.com/ros2/demos/issues/711))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、埃默森·克纳普、卡莱德·贾布尔、卢卡斯·温德兰、斯科特·K·洛根、藤田东茂雅、小型-1235、苔藓80

<span id="demo-nodes-cpp-native"></span>

## [demo_nodes_cpp_native](https://github.com/ros2/demos/tree/lyrical/demo_nodes_cpp_native/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#781](https://github.com/ros2/demos//issues/781))

- 切换到示例_界面( S)[\#674](https://github.com/ros2/demos/issues/674))

- 删除已过时的 TODO ([\#723](https://github.com/ros2/demos/issues/723))

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#707](https://github.com/ros2/demos/issues/707))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,爱默生·克纳普,卢卡斯·温德兰,谢恩·洛雷茨,摩斯费特80

<span id="demo-nodes-py"></span>

## [demo_nodes_py](https://github.com/ros2/demos/tree/lyrical/demo_nodes_py/CHANGELOG.rst)

- 在父节点下添加子日志, 日志级别不同 。 ()[\#772](https://github.com/ros2/demos//issues/772))

- 修复已贬值的 Rcutils Logger: warn () LoggerServiceNode 中的用法([\#773](https://github.com/ros2/demos//issues/773))

- 忽略 A005 (% 1)[\#771](https://github.com/ros2/demos//issues/771))

- 切换到示例_界面( S)[\#674](https://github.com/ros2/demos/issues/674))

- 固定设置工具[\#733](https://github.com/ros2/demos/issues/733))

- 从 yaml 文件恢复“ 修正” 固定加载参数行为 。 ()[\#656](https://github.com/ros2/demos/issues/656))” ([\#660](https://github.com/ros2/demos/issues/660))” ([\#661](https://github.com/ros2/demos/issues/661))

- 贡献者:许巴里,卢卡斯·温德兰,迈克尔·卡尔斯特罗姆,藤田丰也,苔丝菲特80

<span id="diagnostic-msgs"></span>

## [diagnostic_msgs](https://github.com/ros2/common_interfaces/tree/lyrical/diagnostic_msgs/CHANGELOG.rst)

- 修复 CMAKE 折旧[\#288](https://github.com/ros2/common_interfaces/issues/288))

- 贡献者:苔藓80

<span id="domain-coordinator"></span>

## [domain_coordinator](https://github.com/ros2/ament_cmake_ros/tree/lyrical/domain_coordinator/CHANGELOG.rst)

- 固定设置工具[\#49](https://github.com/ros2/ament_cmake_ros/issues/49))

- 贡献者:苔藓80

<span id="dummy-map-server"></span>

## [dummy_map_server](https://github.com/ros2/demos/tree/lyrical/dummy_robot/dummy_map_server/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#781](https://github.com/ros2/demos//issues/781))

- 清除已腐烂的 rclp:: spin_some (). (中文(简体) ).[\#734](https://github.com/ros2/demos/issues/734))

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714)) demo_nodes_cpp/CMakeLists.txt 需要cmake min 版本 3.12 其他模块 cmake 3.5 。它被提议与 3.12 版本标准化。它也修正 cmake \< 3.10 折旧警告

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#707](https://github.com/ros2/demos/issues/707))

- 贡献者:爱默生·克纳普,谢恩·洛雷茨,藤田友也,苔丝菲特80

<span id="dummy-robot-bringup"></span>

## [dummy_robot_bringup](https://github.com/ros2/demos/tree/lyrical/dummy_robot/dummy_robot_bringup/CHANGELOG.rst)

- 固定发射文件( A)[\#759](https://github.com/ros2/demos/issues/759))

- 为假人_robot_bringup添加了 README.md. ()[\#574](https://github.com/ros2/demos/issues/574))

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714)) demo_nodes_cpp/CMakeLists.txt 需要cmake min 版本 3.12 其他模块 cmake 3.5 。它被提议与 3.12 版本标准化。它也修正 cmake \< 3.10 折旧警告

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、加里·贝伊、苔藓80

<span id="dummy-sensors"></span>

## [dummy_sensors](https://github.com/ros2/demos/tree/lyrical/dummy_robot/dummy_sensors/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#781](https://github.com/ros2/demos//issues/781))

- 清除已腐烂的 rclp:: spin_some (). (中文(简体) ).[\#734](https://github.com/ros2/demos/issues/734))

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714)) demo_nodes_cpp/CMakeLists.txt 需要cmake min 版本 3.12 其他模块 cmake 3.5 。它被提议与 3.12 版本标准化。它也修正 cmake \< 3.10 折旧警告

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#707](https://github.com/ros2/demos/issues/707))

- 贡献者:爱默生·克纳普,谢恩·洛雷茨,藤田友也,苔丝菲特80

<span id="eigen3-cmake-module"></span>

## [eigen3_cmake_module](https://github.com/ros2/eigen3_cmake_module/tree/lyrical/CHANGELOG.rst)

- 固定cmake 折旧([\#10](https://github.com/ros2/eigen3_cmake_module/issues/10))

- 删除 CODEOWINERS 和镜像滚动到主工作流程 。 ([\#8](https://github.com/ros2/eigen3_cmake_module/issues/8))

- 贡献者:克里斯·拉朗谢特,苔藓80

<span id="example-interfaces"></span>

## [example_interfaces](https://github.com/ros2/example_interfaces/tree/lyrical/CHANGELOG.rst)

- 固定cmake 折旧([\#23](https://github.com/ros2/example_interfaces/issues/23))

- 删除.github/ISSUE_TEMPLATE.md(旧版模板)([\#21](https://github.com/ros2/example_interfaces/issues/21))

- 删除 CODEOWINERS 和镜像滚动到主工作流程 。 ([\#19](https://github.com/ros2/example_interfaces/issues/19))

- 贡献者:克里斯·拉朗塞特、藤田友也、苔藓80

<span id="examples-rclcpp-async-client"></span>

## [examples_rclcpp_async_client](https://github.com/ros2/examples/tree/lyrical/rclcpp/services/async_client/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#444](https://github.com/ros2/examples/issues/444))

- 修复 CMAKE 折旧[\#419](https://github.com/ros2/examples/issues/419))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 贡献者:爱默生·克纳普,谢恩·洛雷兹,苔藓80

<span id="examples-rclcpp-cbg-executor"></span>

## [examples_rclcpp_cbg_executor](https://github.com/ros2/examples/tree/lyrical/rclcpp/executors/cbg_executor/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#444](https://github.com/ros2/examples/issues/444))

- 修复 CMAKE 折旧[\#419](https://github.com/ros2/examples/issues/419))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 贡献者:爱默生·克纳普,谢恩·洛雷兹,苔藓80

<span id="examples-rclcpp-minimal-action-client"></span>

## [examples_rclcpp_minimal_action_client](https://github.com/ros2/examples/tree/lyrical/rclcpp/actions/minimal_action_client/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#444](https://github.com/ros2/examples/issues/444))

- 清除已腐烂的 rclp:: spin_some (). (中文(简体) ).[\#422](https://github.com/ros2/examples//issues/422))

- 修复 CMAKE 折旧[\#419](https://github.com/ros2/examples/issues/419))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 贡献者:爱默生·克纳普,谢恩·洛雷茨,藤田友也,苔丝菲特80

<span id="examples-rclcpp-minimal-action-server"></span>

## [examples_rclcpp_minimal_action_server](https://github.com/ros2/examples/tree/lyrical/rclcpp/actions/minimal_action_server/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#444](https://github.com/ros2/examples/issues/444))

- 添加 rclcpp 单目标动作服务器实例([\#429](https://github.com/ros2/examples/issues/429))

- 修复 CMAKE 折旧[\#419](https://github.com/ros2/examples/issues/419))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 贡献者:爱默生·克纳普,谢恩·洛雷茨,太加·阿拉伊,摩斯费特80

<span id="examples-rclcpp-minimal-client"></span>

## [examples_rclcpp_minimal_client](https://github.com/ros2/examples/tree/lyrical/rclcpp/services/minimal_client/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#444](https://github.com/ros2/examples/issues/444))

- 修复 CMAKE 折旧[\#419](https://github.com/ros2/examples/issues/419))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 贡献者:爱默生·克纳普,谢恩·洛雷兹,苔藓80

<span id="examples-rclcpp-minimal-composition"></span>

## [examples_rclcpp_minimal_composition](https://github.com/ros2/examples/tree/lyrical/rclcpp/composition/minimal_composition/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#444](https://github.com/ros2/examples/issues/444))

- 修复 CMAKE 折旧[\#419](https://github.com/ros2/examples/issues/419))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 贡献者:爱默生·克纳普,谢恩·洛雷兹,苔藓80

<span id="examples-rclcpp-minimal-publisher"></span>

## [examples_rclcpp_minimal_publisher](https://github.com/ros2/examples/tree/lyrical/rclcpp/topics/minimal_publisher/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#444](https://github.com/ros2/examples/issues/444))

- 改进最低限度的-publisher README,提供更清晰的结构和使用指导([\#434](https://github.com/ros2/examples/issues/434))

- 清除已腐烂的 rclp:: spin_some (). (中文(简体) ).[\#422](https://github.com/ros2/examples//issues/422))

- 修复 CMAKE 折旧[\#419](https://github.com/ros2/examples/issues/419))

- 等待 5 秒, 直到所有订阅都确认信件 。 ()[\#414](https://github.com/ros2/examples/issues/414))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 贡献者:爱默生·克纳普,谢恩·洛雷茨,藤田丰也,亚德涅什瓦尔·阿莫尔·萨哈雷,苔藓Fet80

<span id="examples-rclcpp-minimal-service"></span>

## [examples_rclcpp_minimal_service](https://github.com/ros2/examples/tree/lyrical/rclcpp/services/minimal_service/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#444](https://github.com/ros2/examples/issues/444))

- 修复 CMAKE 折旧[\#419](https://github.com/ros2/examples/issues/419))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 贡献者:爱默生·克纳普,谢恩·洛雷兹,苔藓80

<span id="examples-rclcpp-minimal-subscriber"></span>

## [examples_rclcpp_minimal_subscriber](https://github.com/ros2/examples/tree/lyrical/rclcpp/topics/minimal_subscriber/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#444](https://github.com/ros2/examples/issues/444))

- 修复 CMAKE 折旧[\#419](https://github.com/ros2/examples/issues/419))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 贡献者:爱默生·克纳普,谢恩·洛雷兹,苔藓80

<span id="examples-rclcpp-minimal-timer"></span>

## [examples_rclcpp_minimal_timer](https://github.com/ros2/examples/tree/lyrical/rclcpp/timers/minimal_timer/CHANGELOG.rst)

- 修复 CMAKE 折旧[\#419](https://github.com/ros2/examples/issues/419))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 贡献者:谢恩·洛雷茨、苔藓80

<span id="examples-rclcpp-multithreaded-executor"></span>

## [examples_rclcpp_multithreaded_executor](https://github.com/ros2/examples/tree/lyrical/rclcpp/executors/multithreaded_executor/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#444](https://github.com/ros2/examples/issues/444))

- 修复 CMAKE 折旧[\#419](https://github.com/ros2/examples/issues/419))

- 提高多线索执行器实例中报告线索标识的可读性([\#415](https://github.com/ros2/examples/issues/415))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 贡献者:爱默生·克纳普,何塞·法利亚,谢恩·洛雷茨,苔藓80

<span id="examples-rclcpp-wait-set"></span>

## [examples_rclcpp_wait_set](https://github.com/ros2/examples/tree/lyrical/rclcpp/wait_set/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#444](https://github.com/ros2/examples/issues/444))

- 修复 CMAKE 折旧[\#419](https://github.com/ros2/examples/issues/419))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 贡献者:爱默生·克纳普,谢恩·洛雷兹,苔藓80

<span id="examples-rclpy-executors"></span>

## [examples_rclpy_executors](https://github.com/ros2/examples/tree/lyrical/rclpy/executors/CHANGELOG.rst)

- 固定设置工具的折旧( E)[\#421](https://github.com/ros2/examples/issues/421))

- 贡献者:苔藓80

<span id="examples-rclpy-guard-conditions"></span>

## [examples_rclpy_guard_conditions](https://github.com/ros2/examples/tree/lyrical/rclpy/guard_conditions/CHANGELOG.rst)

- 固定设置工具的折旧( E)[\#421](https://github.com/ros2/examples/issues/421))

- 贡献者:苔藓80

<span id="examples-rclpy-minimal-action-client"></span>

## [examples_rclpy_minimal_action_client](https://github.com/ros2/examples/tree/lyrical/rclpy/actions/minimal_action_client/CHANGELOG.rst)

- 固定设置工具的折旧( E)[\#421](https://github.com/ros2/examples/issues/421))

- 贡献者:苔藓80

<span id="examples-rclpy-minimal-action-server"></span>

## [examples_rclpy_minimal_action_server](https://github.com/ros2/examples/tree/lyrical/rclpy/actions/minimal_action_server/CHANGELOG.rst)

- 固定设置工具的折旧( E)[\#421](https://github.com/ros2/examples/issues/421))

- 贡献者:苔藓80

<span id="examples-rclpy-minimal-client"></span>

## [examples_rclpy_minimal_client](https://github.com/ros2/examples/tree/lyrical/rclpy/services/minimal_client/CHANGELOG.rst)

- Flake8 修正([\#445](https://github.com/ros2/examples/issues/445))

- 固定设置工具的折旧( E)[\#421](https://github.com/ros2/examples/issues/421))

- 贡献者:迈克尔·卡尔斯特罗姆、苔藓80

<span id="examples-rclpy-minimal-publisher"></span>

## [examples_rclpy_minimal_publisher](https://github.com/ros2/examples/tree/lyrical/rclpy/topics/minimal_publisher/CHANGELOG.rst)

- 固定设置工具的折旧( E)[\#421](https://github.com/ros2/examples/issues/421))

- 将 Flake8 错误处理为例_rclpy_minimal_publisher ()[\#410](https://github.com/ros2/examples/issues/410))

- 添加发布器\_ 成员\_ 函数\_ with_wait\_ for\_ all_acked.py ()[\#407](https://github.com/ros2/examples/issues/407))

- 贡献者:藤田丰也,苔藓(mosfet80)

<span id="examples-rclpy-minimal-service"></span>

## [examples_rclpy_minimal_service](https://github.com/ros2/examples/tree/lyrical/rclpy/services/minimal_service/CHANGELOG.rst)

- Flake8 修正([\#445](https://github.com/ros2/examples/issues/445))

- 固定设置工具的折旧( E)[\#421](https://github.com/ros2/examples/issues/421))

- 贡献者:迈克尔·卡尔斯特罗姆、苔藓80

<span id="examples-rclpy-minimal-subscriber"></span>

## [examples_rclpy_minimal_subscriber](https://github.com/ros2/examples/tree/lyrical/rclpy/topics/minimal_subscriber/CHANGELOG.rst)

- Flake8 修正([\#445](https://github.com/ros2/examples/issues/445))

- 固定设置工具的折旧( E)[\#421](https://github.com/ros2/examples/issues/421))

- 贡献者:迈克尔·卡尔斯特罗姆、苔藓80

<span id="examples-rclpy-pointcloud-publisher"></span>

## [examples_rclpy_pointcloud_publisher](https://github.com/ros2/examples/tree/lyrical/rclpy/topics/pointcloud_publisher/CHANGELOG.rst)

- 固定设置工具的折旧( E)[\#421](https://github.com/ros2/examples/issues/421))

- 贡献者:苔藓80

<span id="examples-tf2-py"></span>

## [examples_tf2_py](https://github.com/ros2/geometry2/tree/lyrical/examples_tf2_py/CHANGELOG.rst)

- 修补字型( E)[\#921](https://github.com/ros2/geometry2/issues/921))

- 将conf.py文件现代化,只包括修改过的版权,eliminati... ()[\#865](https://github.com/ros2/geometry2/issues/865))

- 固定设置工具折价( S)[\#809](https://github.com/ros2/geometry2/issues/809))

- 贡献者:奥古斯特·拉兰德、R·肯特·詹姆斯、苔丝菲特80

<span id="foonathan-memory-vendor"></span>

## [foonathan_memory_vendor](https://github.com/eProsima/foonathan_memory_vendor/tree/master/CHANGELOG.rst)

- 向上游变化, 以串联修复建筑 (# 80)

- 上游更改为 eProsima 叉以避免补丁命令 (# 80)

- 上游更新以发布 0.7-4 (# 75)

- 删除安装器 CMake 补丁 (# 75)

- 改进安装 founathan_memory 的机制(# 67)

- 修正 ament_lint\_ cmake 错误 (# 68)

- 添加 FORCE\_ BUILD 选项到 cmake (# 69)

- 缩短新选项描述 (# 70)

<span id="geometry2"></span>

## [几何学2](https://github.com/ros2/geometry2/tree/lyrical/geometry2/CHANGELOG.rst)

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 贡献者:苔藓80

<span id="geometry-msgs"></span>

## [geometry_msgs](https://github.com/ros2/common_interfaces/tree/lyrical/geometry_msgs/CHANGELOG.rst)

- 澄清 `Inertia.msg` 表示关于质量中心的惯性([\#313](https://github.com/ros2/common_interfaces/issues/313))

- 修复 CMAKE 折旧[\#288](https://github.com/ros2/common_interfaces/issues/288))

- 删除已贬值的几何学\_ msgs/ Pose2d ([\#283](https://github.com/ros2/common_interfaces/issues/283))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、安德鲁·西明顿、莫斯费特80

<span id="gmock-vendor"></span>

## [gmock_vendor](https://github.com/ament/googletest/tree/lyrical/googlemock/CHANGELOG.rst)

- 堕落 gtest\_ vendor 和 gmock\_ vendor ([\#41](https://github.com/ament/googletest/issues/41))

- 撰稿人:谢恩·洛雷茨

<span id="gtest-vendor"></span>

## [gtest_vendor](https://github.com/ament/googletest/tree/lyrical/googletest/CHANGELOG.rst)

- 堕落 gtest\_ vendor 和 gmock\_ vendor ([\#41](https://github.com/ament/googletest/issues/41))

- 撰稿人:谢恩·洛雷茨

<span id="gz-cmake-vendor"></span>

## [gz_cmake_vendor](https://github.com/gazebo-release/gz_cmake_vendor/tree/lyrical/CHANGELOG.rst)

- 弹出版本为5.1.0([\#24](https://github.com/gazebo-release/gz_cmake_vendor/issues/24))

- 合并拉动请求 [\#23](https://github.com/gazebo-release/gz_cmake_vendor/issues/23) Bump版本为5.0.2 QQ

- 弹跳版本为5.0.1([\#20](https://github.com/gazebo-release/gz_cmake_vendor/issues/20))

- 弹出版本为5.0.0([\#19](https://github.com/gazebo-release/gz_cmake_vendor/issues/19))

- Jetty支持:撞到5.0.0,固定包名([\#16](https://github.com/gazebo-release/gz_cmake_vendor/issues/16)) \* Jetty 支持 : ump to 5. 0. 0, 固定软件包名称 主要版本编号从 Gazebo Jetty 的软件包名称中移除, 因此不需要额外的 cmake 配置文件 。 \* 添加选项 VENDOR\_ FROM\_ LIB\_ VCS\_ REF , 允许从指定的 vcs ref 而不是硬编码标签中进行销售 。 当此选项被设定为真时, 可以在 LIB\_ VCS\_ REF 变量中指定一个分支、 标记或承诺 。 如果 LIB\_ VCS_REF 中未指明, 将使用主服务器 。 \* 删除未使用的 cmake 配置模板 \* 使用小写符来修正 linter 投诉 \* 5. 0.0~ pre1 {}

- 弹跳版本为4.2.0([\#15](https://github.com/gazebo-release/gz_cmake_vendor/issues/15))

- 贡献者:Addiu Z. Taddese、Jose Luis Rivero、Steve Peters

<span id="gz-math-vendor"></span>

## [gz_math_vendor](https://github.com/gazebo-release/gz_math_vendor/tree/lyrical/CHANGELOG.rst)

- 弹出版本为 9.1.0 ()[\#20](https://github.com/gazebo-release/gz_math_vendor/issues/20))

- 弹出版本为9.0.0([\#17](https://github.com/gazebo-release/gz_math_vendor/issues/17))

- 设置 Jetty 包的 PYTHONPATH ()[\#14](https://github.com/gazebo-release/gz_math_vendor/issues/14)) \* 设置未转换包的 PYTHONPATH \* 跳跃到 9. 0.0-pre2 \* 将 PYTHONPATH 设置在单独的 dsv 文件 \* \* \*

- 跳转到9.0.0-前2([\#16](https://github.com/gazebo-release/gz_math_vendor/issues/16))

- Jetty 支持: 撞到9. 0. 0, 固定包名([\#12](https://github.com/gazebo-release/gz_math_vendor/issues/12)) \* Jetty 支持 : 缩到 9. 0, 固定包名 主要的版本编号已经从 Gazebo Jetty 的包名中删除, 因此不需要额外的 cmake 配置文件 。 \* 添加选项 VENDOR\_ FROM\_ LIB\_ VCS\_ REF , 允许从指定的 vcs ref 而不是硬编码标签中进行销售 。 当此选项被设定为真时, 可以在 LIB\_ VCS\_ REF 变量中指定一个分支、 标记或承诺 。 如果 LIB\_ VCS\_ REF 中未指明, 将使用 make 配置文件 。 \* 删除未使用的 cmake 配置文件 \* 使用小写符来修正 linter 投诉 \* 构建 python 绑定 \* 9. 0.0~ pre1\\\\\\ 。

- 弹跳版本改为8.2.0([\#11](https://github.com/gazebo-release/gz_math_vendor/issues/11))

- 贡献者:Addiu Z. Taddese、Ian Chen、Jose Luis Rivero、Steve Peters

<span id="gz-utils-vendor"></span>

## [gz_utils_vendor](https://github.com/gazebo-release/gz_utils_vendor/tree/lyrical/CHANGELOG.rst)

- 弹出版本为4.0.0([\#12](https://github.com/gazebo-release/gz_utils_vendor/issues/12))

- 为 Jetty 包件的 PYTHONPATH 添加 dsv([\#13](https://github.com/gazebo-release/gz_utils_vendor/issues/13))

- Jetty 支持: 凸起到 4. 0. 0, 固定包名 ([\#11](https://github.com/gazebo-release/gz_utils_vendor/issues/11)) \* Jetty 支持: 向 4. 0. 0, 固定软件包名称 主要版本编号已从 Gazebo Jetty 的软件包名称中删除, 因此不需要额外的 cmake 配置文件 。 \* 添加选项 VENDOR\_ FROM\_ LIB\_ VCS\_ REF , 允许从指定的 vcs ref 而不是硬码标签中进行销售。 当此选项被设定为真实时, 可以在 LIB\_ VCS\_ REF 变量中指定一个分支、 标记或承诺 。 如果 LIB\_ VCS\_ REF 中未指明, 将使用主版本 。 \* 删除未使用的 cmake 配置文件 \* 使用小写符处理 linter 投诉 \* 添加对 cli11\* 的依赖 。 4. 0.0~ pre1 \* 使用 CLI11 的供应商版本 。 \* 联合撰写:Addiu Z. Taddese \<[addisu@openrobotics.org](mailto:addisu%40openrobotics.org)\>

- 撰稿人:Addiu Z. Taddese、史蒂夫·彼得斯

<span id="image-common"></span>

## [image_common](https://github.com/ros-perception/image_common/tree/lyrical/image_common/CHANGELOG.rst)

- 将 BSD 许可证更新到 SPDX 标识符([\#389](https://github.com/ros-perception/image_common/issues/389))合著:亚历杭德罗·赫尔南德斯·科尔德罗 \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 修补 cmake 折旧( E)[\#367](https://github.com/ros-perception/image_common/issues/367))

- 贡献者:加勒特·布朗、苔藓80

<span id="image-tools"></span>

## [image_tools](https://github.com/ros2/demos/tree/lyrical/image_tools/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#781](https://github.com/ros2/demos//issues/781))

- 别用 `libopencv-dev` 用于执行([\#760](https://github.com/ros2/demos//issues/760))

- 切换到示例_界面( S)[\#674](https://github.com/ros2/demos/issues/674))

- 在发射测试中使用 EullRmwIsolation ()[\#724](https://github.com/ros2/demos/issues/724))

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 缩写图像\_ 工具/ CMakeLists. txt ()[\#712](https://github.com/ros2/demos/issues/712))

- 设定 Envars 以 rmw_zenoh_cpp 进行多播发现的测试([\#711](https://github.com/ros2/demos/issues/711))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,爱默生·克纳普,卢卡斯·温德兰,迈克尔·卡尔斯特罗姆,斯科特·克·洛根,摩斯费特80,雅敦德

<span id="image-transport"></span>

## [image_transport](https://github.com/ros-perception/image_common/tree/lyrical/image_transport/CHANGELOG.rst)

- 删除了 cang 警告( R)[\#399](https://github.com/ros-perception/image_common/issues/399))

- 包含消息类型 (E)[\#394](https://github.com/ros-perception/image_common/issues/394))

- 使用新的 ROSIDL 聚合 CMake 目标([\#396](https://github.com/ros-perception/image_common/issues/396))

- 将 BSD 许可证更新到 SPDX 标识符([\#389](https://github.com/ros-perception/image_common/issues/389))合著:亚历杭德罗·赫尔南德斯·科尔德罗 \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 在所有测试完成后, 适当关闭 Rclcpp 。 ()[\#384](https://github.com/ros-perception/image_common/issues/384))

- 修正 QoS 覆盖测试( Name[\#376](https://github.com/ros-perception/image_common/issues/376))

- 解决对 rclpp_生命周期的依赖性([\#373](https://github.com/ros-perception/image_common/issues/373))

- 以 crange 修正编译错误 (S)[\#372](https://github.com/ros-perception/image_common/issues/372))

- 支持生命周期节点 - 节点界面( )[\#352](https://github.com/ros-perception/image_common/issues/352))

- 固定条状结构([\#371](https://github.com/ros-perception/image_common/issues/371))

- 固定建筑([\#369](https://github.com/ros-perception/image_common/issues/369))

- 已贬值的rmw_qos\_ profile_t 支持rclcpp: QoS (中文(简体) ).[\#364](https://github.com/ros-perception/image_common/issues/364))

- 删除已折旧的代码( N)[\#356](https://github.com/ros-perception/image_common/issues/356))

- 修补 cmake 折旧( E)[\#367](https://github.com/ros-perception/image_common/issues/367))

- 定义插件的主题解析度( E)[\#365](https://github.com/ros-perception/image_common/issues/365))

- 删除窗口警告( E)[\#350](https://github.com/ros-perception/image_common/issues/350))

- 添加 `rclcpp::shutdown` ([\#347](https://github.com/ros-perception/image_common/issues/347))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#345](https://github.com/ros-perception/image_common/issues/345))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,亚历克斯·蒂什卡,爱默生·克纳普,加勒特·布朗,谢恩·洛雷茨,藤田富友亚,柳元,苔丝菲特80

<span id="image-transport-py"></span>

## [image_transport_py](https://github.com/ros-perception/image_common/tree/lyrical/image_transport_py/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#396](https://github.com/ros-perception/image_common/issues/396))

- 将 BSD 许可证更新到 SPDX 标识符([\#389](https://github.com/ros-perception/image_common/issues/389))合著:亚历杭德罗·赫尔南德斯·科尔德罗 \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 从 deb 或 pixi 使用 pybind11 ()[\#374](https://github.com/ros-perception/image_common/issues/374))

- 支持生命周期节点 - 节点界面( )[\#352](https://github.com/ros-perception/image_common/issues/352))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、埃默森·克纳普、加勒特·布朗

<span id="interactive-markers"></span>

## [interactive_markers](https://github.com/ros-visualization/interactive_markers/tree/lyrical/CHANGELOG.rst)

- 固定: 固定在 MSVC 2022 上编译 ([\#120](https://github.com/ros-visualization/interactive_markers/issues/120))

- 使用新的 ROSIDL 聚合 CMake 目标([\#119](https://github.com/ros-visualization/interactive_markers/issues/119))

- 清理错误标签的 BSD 许可证( Name[\#118](https://github.com/ros-visualization/interactive_markers/issues/118))

- 明确时间比较[\#105](https://github.com/ros-visualization/interactive_markers/issues/105))

- 固定cmake 折旧([\#113](https://github.com/ros-visualization/interactive_markers/issues/113))

- 贡献者:AiVerisimilitus,亚历杭德罗·埃尔南德斯·科尔德罗,爱默生·克纳普,雅诺施·麦克豪温斯基,苔丝菲特80

<span id="intra-process-demo"></span>

## [intra_process_demo](https://github.com/ros2/demos/tree/lyrical/intra_process_demo/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#781](https://github.com/ros2/demos//issues/781))

- 别用 `libopencv-dev` 用于执行([\#760](https://github.com/ros2/demos//issues/760))

- 切换到示例_界面( S)[\#674](https://github.com/ros2/demos/issues/674))

- 修饰图像\_ 管道\_ demo ([\#755](https://github.com/ros2/demos/issues/755))

- 在发射测试中使用 EullRmwIsolation ()[\#724](https://github.com/ros2/demos/issues/724))

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 设定 Envars 以 rmw_zenoh_cpp 进行多播发现的测试([\#711](https://github.com/ros2/demos/issues/711))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,爱默生·克纳普,卢卡斯·温德兰,迈克尔·卡尔斯特罗姆,斯科特·克·洛根,威廉·伍德尔,摩斯费特80

<span id="kdl-parser"></span>

## [kdl_parser](https://github.com/ros/kdl_parser/tree/lyrical/CHANGELOG.rst)

- 取消对kdl供应商的依赖([\#90](https://github.com/ros/kdl_parser//issues/90))

- 制作要求( E)[\#88](https://github.com/ros/kdl_parser/issues/88))

- 删除 kdl_parser_py. ()[\#89](https://github.com/ros/kdl_parser/issues/89))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、莫斯费特80

<span id="keyboard-handler"></span>

## [keyboard_handler](https://github.com/ros-tooling/keyboard_handler/tree/lyrical/keyboard_handler/CHANGELOG.rst)

- 固定cmake 折旧([\#55](https://github.com/ros-tooling/keyboard_handler/issues/55))

- 贡献者:苔藓80

<span id="laser-geometry"></span>

## [laser_geometry](https://github.com/ros-perception/laser_geometry/tree/lyrical/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#115](https://github.com/ros-perception/laser_geometry/issues/115))

- 在传感器_msgs::::msg::LaserScan msg中使用秒数在测试内([\#107](https://github.com/ros-perception/laser_geometry/issues/107))

- 使用 rclcpp 的构建器: 时间而不是转换 。 ()[\#91](https://github.com/ros-perception/laser_geometry/issues/91))

- 固定cmake 折旧([\#105](https://github.com/ros-perception/laser_geometry/issues/105))

- 删除 linux 主机的硬编码的 eigen3 头路徑([\#95](https://github.com/ros-perception/laser_geometry/issues/95))合著:亚历杭德罗·赫尔南德斯·科尔德罗 \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 贡献者:AiVerisimilite、Alejandro Hernández Cordero、Emerson Knapp、Lukas Schäper、苔藓80

<span id="launch"></span>

## [发射](https://github.com/ros2/launch/tree/lyrical/launch/CHANGELOG.rst)

- 校正类型( E)[\#961](https://github.com/ros2/launch/issues/961))

- 热补丁( E)[\#950](https://github.com/ros2/launch/issues/950))

- 宣布布尔发射参数( E)[\#944](https://github.com/ros2/launch/issues/944))

- 路径Join 替代的支持前端( P)[\#943](https://github.com/ros2/launch/issues/943))

- 测试用亚单元取代蟒([\#688](https://github.com/ros2/launch/issues/688))

- 范围发射文件 dir/path locals以包含发射文件([\#862](https://github.com/ros2/launch/issues/862))

- 在定时行动中抓住环境变量([\#728](https://github.com/ros2/launch/issues/728))

- 删除导入lib 元数据( R)[\#932](https://github.com/ros2/launch/issues/932))

- 修复间sphinx_映射格式( S)[\#921](https://github.com/ros2/launch/issues/921))

- 使目录搜索替代成为/操作员的路径替代([\#914](https://github.com/ros2/launch//issues/914))

- 曝光字符串Join 替换到前端( E)[\#857](https://github.com/ros2/launch//issues/857))

- 共同的替换逻辑( :[\#769](https://github.com/ros2/launch/issues/769))

- 使用 yaml 类型( E)[\#781](https://github.com/ros2/launch/issues/781))

- 切换 osrf_py 常用的依赖性到系统软件包([\#817](https://github.com/ros2/launch/issues/817))

- 在 xml 和 yaml 发射文件中修复全部/任何文件 ([\#906](https://github.com/ros2/launch/issues/906))

- 允许提供发射参数, 包括使用前端的 let( R)[\#848](https://github.com/ros2/launch//issues/848))

- 修复设置工具折旧( S)[\#898](https://github.com/ros2/launch/issues/898))

- 删除启动描述标记( R)[\#891](https://github.com/ros2/launch/issues/891))

- 确保安装 py.typed 文件([\#886](https://github.com/ros2/launch/issues/886))

- 根据用户设置使用自定义日志_文件名( Q)[\#861](https://github.com/ros2/launch/issues/861))

- 使用( E) `TimerAction` 与 `SetParameter` 从发射\_ ros 导致崩溃 ([\#879](https://github.com/ros2/launch/issues/879))

- 修补 `log\_*` 警告( E)[\#883](https://github.com/ros2/launch/issues/883))

- 更新 `launch` 打字([\#831](https://github.com/ros2/launch/issues/831))

- 允许替换路径, 而不是要求 cast to str ()[\#873](https://github.com/ros2/launch/issues/873))

- 添加一个 `/` 路径加入运算符 `PathJoinSubstitution` ([\#868](https://github.com/ros2/launch/issues/868))

- 其他日志执行 `getLevelNamesMapping` 固定( E)[\#866](https://github.com/ros2/launch/issues/866))

- 还原“添加其他日志执行( U)[\#858](https://github.com/ros2/launch/issues/858))” ([\#865](https://github.com/ros2/launch/issues/865)(b7b31c45b0eb350 deed282b88398d1ca0d5faf4)这种回归承诺.

- 添加其他日志执行( N)[\#858](https://github.com/ros2/launch/issues/858))

- 贡献者:奥古斯特·拉兰德,克里斯蒂安·鲁夫,克里斯托弗·贝达德,大卫·V·卢!!!!. 埃默森·克纳普,哈里森·陈,乔纳斯·奥托,肯吉·布拉梅尔德(TRACLabs),马特希斯·范德布尔格,迈克尔·卡尔斯特罗姆,斯科特·K·洛根,塞巴斯蒂安·哈维尔·达莱桑德罗·舍曼诺夫斯基,塔尼什克·乔达里,威尔,苔丝费特80

<span id="launch-pytest"></span>

## [launch_pytest](https://github.com/ros2/launch/tree/lyrical/launch_pytest/CHANGELOG.rst)

- 固定回归( E)[\#959](https://github.com/ros2/launch/issues/959))

- 固定 : 添加 pytest 兼容性的 get\_ launch\_ test_fixture_scope([\#949](https://github.com/ros2/launch/issues/949))

- 切换 osrf_py 常用的依赖性到系统软件包([\#817](https://github.com/ros2/launch/issues/817))

- 修复设置工具折旧( S)[\#898](https://github.com/ros2/launch/issues/898))

- 确保安装 py.typed 文件([\#886](https://github.com/ros2/launch/issues/886))

- 添加剩余 `py.typed` ([\#884](https://github.com/ros2/launch/issues/884))

- 允许替换路径, 而不是要求 cast to str ()[\#873](https://github.com/ros2/launch/issues/873))

- fix( launch\_ pytest): 防止在重新运行时重包装测试真假([\#855](https://github.com/ros2/launch/issues/855))

- 贡献者:克里斯托弗·贝达德,西松大介,大卫·雷瓦伊,爱默生·克纳普,迈克尔·卡尔斯特罗姆,斯科特·克·洛根,摩斯费特80

<span id="launch-ros"></span>

## [launch_ros](https://github.com/ros2/launch_ros/tree/lyrical/launch_ros/CHANGELOG.rst)

- 修正片段8 (% 1)[\#529](https://github.com/ros2/launch_ros//issues/529))

- 正确的打字符( R)[\#524](https://github.com/ros2/launch_ros//issues/524))

- 修正回归( E)[\#521](https://github.com/ros2/launch_ros//issues/521))

- 修复 rhel10 片断8 错误( Q)[\#515](https://github.com/ros2/launch_ros//issues/515))

- 与“平稳过渡”的兼容性 [ros2/rcl#1269](https://github.com/ros2/rcl/issues/1269) ([\#495](https://github.com/ros2/launch_ros/issues/495))

- 删除导入lib ([\#508](https://github.com/ros2/launch_ros/issues/508))

- 使 FindPackage 替换为获取运算符 / ()[\#494](https://github.com/ros2/launch_ros/issues/494))

- 曝光生命周期\_ 节点( E)[\#327](https://github.com/ros2/launch_ros/issues/327)) (附试) (.[\#482](https://github.com/ros2/launch_ros/issues/482))

- 在前端曝光可堆肥的\_ 生命周期\_ 节点( S)[\#480](https://github.com/ros2/launch_ros/issues/480))

- 切换 osrf_py 常用的依赖性到系统软件包([\#431](https://github.com/ros2/launch_ros/issues/431))

- 用于发射前端的 SetUssimTime([\#488](https://github.com/ros2/launch_ros/issues/488))

- 固定设置工具[\#475](https://github.com/ros2/launch_ros/issues/475))

- 改进错误的可读性类型( E)[\#469](https://github.com/ros2/launch_ros/issues/469))

- 修补: 装入可编解码失败, 无法正确解析通卡参数文件( L)[\#460](https://github.com/ros2/launch_ros/issues/460)) ([\#465](https://github.com/ros2/launch_ros/issues/465))

- 贡献者:奥古斯特·拉兰德,克里斯托弗·贝达德,爱默生·克纳普,埃姆雷·库鲁,贾斯珀·范布拉克尔,肯吉·布拉梅尔德,迈克尔·卡尔斯特罗姆,斯科特·K·洛根,苔丝菲特80

<span id="launch-testing"></span>

## [launch_testing](https://github.com/ros2/launch/tree/lyrical/launch_testing/CHANGELOG.rst)

- 校正类型( E)[\#961](https://github.com/ros2/launch/issues/961))

- 修复 Ubuntu26 的测试_io\_ tests ([\#960](https://github.com/ros2/launch/issues/960))

- 修正片段8 (% 1)[\#952](https://github.com/ros2/launch/issues/952))

- 切换 osrf_py 常用的依赖性到系统软件包([\#817](https://github.com/ros2/launch/issues/817))

- 修复设置工具折旧( S)[\#898](https://github.com/ros2/launch/issues/898))

- 确保安装 py.typed 文件([\#886](https://github.com/ros2/launch/issues/886))

- 添加剩余 `py.typed` ([\#884](https://github.com/ros2/launch/issues/884))

- 更新 `launch` 打字([\#831](https://github.com/ros2/launch/issues/831))

- 贡献者:奥古斯特·拉兰德,克里斯托弗·贝达德,迈克尔·卡尔斯特罗姆,斯科特·K·洛根,苔丝菲特80

<span id="launch-testing-ament-cmake"></span>

## [launch_testing_ament_cmake](https://github.com/ros2/launch/tree/lyrical/launch_testing_ament_cmake/CHANGELOG.rst)

- CMake 折旧( R)[\#899](https://github.com/ros2/launch/issues/899))

- 贡献者:苔藓80

<span id="launch-testing-examples"></span>

## [launch_testing_examples](https://github.com/ros2/examples/tree/lyrical/launch_testing/launch_testing_examples/CHANGELOG.rst)

- 使用 rmw_cyclonedds_cpp 改进测试完整性 ()[\#440](https://github.com/ros2/examples/issues/440))

- 固定设置工具的折旧( E)[\#421](https://github.com/ros2/examples/issues/421))

- 贡献者:藤田丰也,苔藓(mosfet80)

<span id="launch-testing-ros"></span>

## [launch_testing_ros](https://github.com/ros2/launch_ros/tree/lyrical/launch_testing_ros/CHANGELOG.rst)

- 在发射_测试_ros中添加测试隔离( Name[\#528](https://github.com/ros2/launch_ros//issues/528))

- 从 Flake8 中冲压多条进程警告 ().[\#520](https://github.com/ros2/launch_ros//issues/520))

- 正确的打字符( R)[\#524](https://github.com/ros2/launch_ros//issues/524))

- 修正在 WaitForTopics 中的关闭比赛( R) 启动\_ ros\_ testing shuting races ([\#511](https://github.com/ros2/launch_ros/issues/511))

- 提供选择,以注入服务质量简介([\#493](https://github.com/ros2/launch_ros/issues/493))

- 固定设置工具[\#475](https://github.com/ros2/launch_ros/issues/475))

- `WaitForTopics`: 等待建立出版商-订阅者连接( )[\#474](https://github.com/ros2/launch_ros/issues/474))

- 贡献者:奥古斯特·拉兰德,乔治·平陶迪,朱利安·伊诺克,迈克尔·卡罗尔,藤田友也,苔丝菲特80

<span id="launch-xml"></span>

## [launch_xml](https://github.com/ros2/launch/tree/lyrical/launch_xml/CHANGELOG.rst)

- 校正类型( E)[\#961](https://github.com/ros2/launch/issues/961))

- 路径Join 替代的支持前端( P)[\#943](https://github.com/ros2/launch/issues/943))

- 在定时行动中抓住环境变量([\#728](https://github.com/ros2/launch/issues/728))

- 曝光字符串Join 替换到前端( E)[\#857](https://github.com/ros2/launch//issues/857))

- 在 xml 和 yaml 发射文件中修复全部/任何文件 ([\#906](https://github.com/ros2/launch/issues/906))

- 允许提供发射参数, 包括使用前端的 let( R)[\#848](https://github.com/ros2/launch//issues/848))

- 修复设置工具折旧( S)[\#898](https://github.com/ros2/launch/issues/898))

- 确保安装 py.typed 文件([\#886](https://github.com/ros2/launch/issues/886))

- 添加剩余 `py.typed` ([\#884](https://github.com/ros2/launch/issues/884))

- 修补 `log\_*` 警告( E)[\#883](https://github.com/ros2/launch/issues/883))

- 允许替换路径, 而不是要求 cast to str ()[\#873](https://github.com/ros2/launch/issues/873))

- 其他日志执行 `getLevelNamesMapping` 固定( E)[\#866](https://github.com/ros2/launch/issues/866))

- 还原“添加其他日志执行( U)[\#858](https://github.com/ros2/launch/issues/858))” ([\#865](https://github.com/ros2/launch/issues/865)(b7b31c45b0eb350 deed282b88398d1ca0d5faf4)这种回归承诺.

- 添加其他日志执行( N)[\#858](https://github.com/ros2/launch/issues/858))

- 贡献者:奥古斯特·拉兰德、克里斯蒂安·鲁夫、克里斯托弗·贝达德、爱默生·克纳普、马蒂斯·范德布尔格、迈克尔·卡尔斯特罗姆、塞巴斯蒂安·哈维尔·达莱桑德罗·舍曼诺夫斯基、莫斯费特80

<span id="launch-yaml"></span>

## [launch_yaml](https://github.com/ros2/launch/tree/lyrical/launch_yaml/CHANGELOG.rst)

- 校正类型( E)[\#961](https://github.com/ros2/launch/issues/961))

- 路径Join 替代的支持前端( P)[\#943](https://github.com/ros2/launch/issues/943))

- 在定时行动中抓住环境变量([\#728](https://github.com/ros2/launch/issues/728))

- 曝光字符串Join 替换到前端( E)[\#857](https://github.com/ros2/launch//issues/857))

- 在 xml 和 yaml 发射文件中修复全部/任何文件 ([\#906](https://github.com/ros2/launch/issues/906))

- 允许提供发射参数, 包括使用前端的 let( R)[\#848](https://github.com/ros2/launch//issues/848))

- 修复设置工具折旧( S)[\#898](https://github.com/ros2/launch/issues/898))

- 确保安装 py.typed 文件([\#886](https://github.com/ros2/launch/issues/886))

- 添加剩余 `py.typed` ([\#884](https://github.com/ros2/launch/issues/884))

- 修补 `log\_*` 警告( E)[\#883](https://github.com/ros2/launch/issues/883))

- 其他日志执行 `getLevelNamesMapping` 固定( E)[\#866](https://github.com/ros2/launch/issues/866))

- 还原“添加其他日志执行( U)[\#858](https://github.com/ros2/launch/issues/858))” ([\#865](https://github.com/ros2/launch/issues/865)(b7b31c45b0eb350 deed282b88398d1ca0d5faf4)这种回归承诺.

- 添加其他日志执行( N)[\#858](https://github.com/ros2/launch/issues/858))

- 贡献者:奥古斯特·拉兰德、克里斯蒂安·鲁夫、克里斯托弗·贝达德、马蒂斯·范德布尔格、迈克尔·卡尔斯特罗姆、塞巴斯蒂安·哈维尔·达莱桑德罗·舍曼诺夫斯基、莫斯费特80

<span id="libstatistics-collector"></span>

## [libstatistics_collector](https://github.com/ros-tooling/libstatistics_collector/tree/lyrical/CHANGELOG.rst)

- 使用新的聚合rosidl目标,而不是 \_TARGETS([\#222](https://github.com/ros-tooling/libstatistics_collector/issues/222))

- 固定cmake 折旧([\#214](https://github.com/ros-tooling/libstatistics_collector/issues/214))

- 从0.3到0.4的横跨式轮椅/行动式轮椅

- 5.3.1至5.4.0的跳动编码cov/编码cov-动作

- 从5.1.2到5.3.1的跳动编码cov/编码cov-动作

- 5.0.7至5.1.2的Bump编码cov/编码cov-动作

- 4.6.0至5.0.7的跳跃密码cov/codecov-动作

- 贡献者:Alexis Tsogias, dependabot\[bot\],苔藓80

<span id="libyaml-vendor"></span>

## [libyaml_vendor](https://github.com/ros2/libyaml_vendor/tree/lyrical/CHANGELOG.rst)

- 将 ament_vendor 替换为 CMake 模块 ([\#67](https://github.com/ros2/libyaml_vendor/issues/67))

- 删除 CODEOWINERS 和镜像滚动到主工作流程 。 ([\#65](https://github.com/ros2/libyaml_vendor/issues/65))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特

<span id="lifecycle"></span>

## [寿命周期](https://github.com/ros2/demos/tree/lyrical/lifecycle/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#781](https://github.com/ros2/demos//issues/781))

- 切换到示例_界面( S)[\#674](https://github.com/ros2/demos/issues/674))

- r-simonelli/demos-寿命周期[\#750](https://github.com/ros2/demos/issues/750))

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#707](https://github.com/ros2/demos/issues/707))

- 贡献者:爱默生·克纳普,卢卡斯·温德兰,谢恩·洛雷茨,摩斯费特80,r-西莫内利

<span id="lifecycle-msgs"></span>

## [lifecycle_msgs](https://github.com/ros2/rcl_interfaces/tree/lyrical/lifecycle_msgs/CHANGELOG.rst)

- 使用内建的\_ 界面/ 过渡Event 印章( T)[\#185](https://github.com/ros2/rcl_interfaces/issues/185))

- 修补 cmake 折旧( E)[\#180](https://github.com/ros2/rcl_interfaces/issues/180))

- 贡献者:贾斯珀·范布拉克尔,苔藓80

<span id="lifecycle-py"></span>

## [lifecycle_py](https://github.com/ros2/demos/tree/lyrical/lifecycle_py/CHANGELOG.rst)

- 添加 `ament_mypy` 支持和类型提示 `lifecycle_py` ([\#778](https://github.com/ros2/demos/issues/778))

- 恢复生命周期\_ py 意外合并 - ament_mypy ()[\#777](https://github.com/ros2/demos//issues/777))

- 动作_tutorys_py: 添加 ament_mypy 支持([\#775](https://github.com/ros2/demos//issues/775))

- 切换到示例_界面( S)[\#674](https://github.com/ros2/demos/issues/674))

- 固定设置工具[\#733](https://github.com/ros2/demos/issues/733))

- 贡献者:卢卡斯·温德兰、莫希特·库马莱桑、莫希特、摩斯费特80

<span id="logging-demo"></span>

## [logging_demo](https://github.com/ros2/demos/tree/lyrical/logging_demo/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#781](https://github.com/ros2/demos//issues/781))

- 切换到示例_界面( S)[\#674](https://github.com/ros2/demos/issues/674))

- 在发射测试中使用 EullRmwIsolation ()[\#724](https://github.com/ros2/demos/issues/724))

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 设定 Envars 以 rmw_zenoh_cpp 进行多播发现的测试([\#711](https://github.com/ros2/demos/issues/711))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#707](https://github.com/ros2/demos/issues/707))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,埃默森·克纳普,卢卡斯·温德兰,斯科特·克·洛根,谢恩·洛雷茨,摩斯费特80

<span id="lttngpy"></span>

## [节点](https://github.com/ros2/ros2_tracing/tree/lyrical/lttngpy/CHANGELOG.rst)

- 在lttngpy中使用 \<lttng/lttng.h\> 并进行清理包括([\#222](https://github.com/ros2/ros2_tracing/issues/222))

- 允许创建快照会话( E)[\#195](https://github.com/ros2/ros2_tracing/issues/195))

- \[Fix\]编译失败([\#194](https://github.com/ros2/ros2_tracing/issues/194))

- 从 deb 或 pixi 使用 pybind11 ()[\#197](https://github.com/ros2/ros2_tracing/issues/197))

- 添加对运行时间开始追踪的支持( E)[\#191](https://github.com/ros2/ros2_tracing/issues/191))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、罗兰德、什拉万·德瓦、莫斯费特80

<span id="map-msgs"></span>

## [map_msgs](https://github.com/ros-planning/navigation_msgs/tree/lyrical/map_msgs/CHANGELOG.rst)

- 更改与维护者相关的电子邮件地址

- 固定 cmake 折旧

- 贡献者:David V. Lu、Steve Macenski、苔藓80

<span id="mcap-vendor"></span>

## [mcap_vendor](https://github.com/ros2/rosbag2/tree/lyrical/mcap_vendor/CHANGELOG.rst)

- 将 mcap 依赖性更新到 2. 1.3 版本 ([\#2355](https://github.com/ros2/rosbag2/issues/2355))

- 删除 lz4 供应商软件包( S)[\#2165](https://github.com/ros2/rosbag2/issues/2165))

- 替换 `zstd_vendor` 与 `zstd_cmake_module` ([\#2166](https://github.com/ros2/rosbag2/issues/2166))

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 后端端口缺失 `cstdint` 包括(a)[\#2008](https://github.com/ros2/rosbag2/issues/2008))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、戴维·安东尼、莫斯费特80

<span id="message-filters"></span>

## [message_filters](https://github.com/ros2/message_filters/tree/lyrical/CHANGELOG.rst)

- 避免在信件中指定矢量\_ filters 信号回调( S)[\#292](https://github.com/ros2/message_filters/issues/292)) ([\#293](https://github.com/ros2/message_filters/issues/293))

- 清理信头并删除死码( E)[\#284](https://github.com/ros2/message_filters/issues/284)) ([\#291](https://github.com/ros2/message_filters/issues/291))

- feat( python): 添加 Python 执行 InformationAligner (后端端口) [\#283](https://github.com/ros2/message_filters/issues/283)) ([\#286](https://github.com/ros2/message_filters/issues/286))

- C++20 样式 ([\#272](https://github.com/ros2/message_filters/issues/272))

- ([\#221](https://github.com/ros2/message_filters/issues/221)) 教学:增加DeltaFilter Python教学([\#277](https://github.com/ros2/message_filters/issues/277))

- DeltaFilter( C++): 添加 DeltaFilter 类。 添加测试( C)[\#273](https://github.com/ros2/message_filters/issues/273)) ([\#273](https://github.com/ros2/message_filters/issues/273))

- 删除已删除的已死亡代码

- 改进和增加测试覆盖面

- 使用新的 ROSIDL 聚合 CMake 目标

- 教程小修: 将 TODOs 替换为所需的其它教程的实际链接 。 将 Approximate- Tyme 教程重命名为 Approximate- Time([\#266](https://github.com/ros2/message_filters/issues/266))

- 教学:增加最新时间同步政策教学([\#266](https://github.com/ros2/message_filters/issues/266))

- 教程: 近似- 同步器: 标有正确语言标记的标签 CMake 代码块

- 教程: 为 Epsilon 时间同步策略添加 C++ 教程

- DeltaFilter( Python): 添加 Python 的 DeltaFilter 。 添加测试。 添加 Docstring 到过滤器和比较处理器中( Python) 。[\#252](https://github.com/ros2/message_filters/issues/252))

- 删除设置.py([\#257](https://github.com/ros2/message_filters/issues/257))

- ([\#246](https://github.com/ros2/message_filters/issues/246), [\#186](https://github.com/ros2/message_filters/issues/186)) 订阅者(Python):向_init_添加召回组,事件_召回,qos_overriding_options,原始和内容_filter_options参数到_init\_. ()[\#251](https://github.com/ros2/message_filters/issues/251))

- 添加从订阅者传递到节点的 kwarg. create\_ 订阅( S)[\#247](https://github.com/ros2/message_filters/issues/247)) 修复使用回调组的呼叫者

- 从基类获取话题名称以传播重映射( G)[\#68](https://github.com/ros2/message_filters/issues/68))

- [\#130](https://github.com/ros2/message_filters/issues/130) 为 cpp 添加简单的过滤器教程( E)[\#239](https://github.com/ros2/message_filters/issues/239))

- [\#200](https://github.com/ros2/message_filters/issues/200) 固定 cpp 和 python 之间的不一致 精确时间同步器 impl ()[\#238](https://github.com/ros2/message_filters/issues/238))

- 添加简单的过滤器教程( E)[\#226](https://github.com/ros2/message_filters/issues/226))

- 更新订阅回调签名( N)[\#222](https://github.com/ros2/message_filters/issues/222))

- 添加链导读 python([\#219](https://github.com/ros2/message_filters/issues/219))

- 更改 Python 订阅器类的函数签名( X)[\#220](https://github.com/ros2/message_filters/issues/220))

- 添加链过滤器的 Python 执行[\#213](https://github.com/ros2/message_filters/issues/213))

- C++ TimeSequencer () 修正不同时间源的比较[\#202](https://github.com/ros2/message_filters/issues/202))

- 对文件的一些修正[\#208](https://github.com/ros2/message_filters/issues/208))

- 创建 C++ 的链类教程( S)[\#203](https://github.com/ros2/message_filters/issues/203))

- 清除已腐烂的 rclp:: spin_some (). (中文(简体) ).[\#201](https://github.com/ros2/message_filters/issues/201))

- 添加“缓存( C++) ” 教程( C)[\#196](https://github.com/ros2/message_filters/issues/196))

- 缓存. hpp: 添加允许的\_ 无标题 ([\#195](https://github.com/ros2/message_filters/issues/195))

- 简化方法调用( N)[\#194](https://github.com/ros2/message_filters/issues/194))

- 修补缓存教程: 添加标签扩展名( T)[\#190](https://github.com/ros2/message_filters/issues/190))

- 为 Python 添加缓存过滤器的教程( T)[\#185](https://github.com/ros2/message_filters/issues/185))

- 固定cmake 折旧([\#182](https://github.com/ros2/message_filters/issues/182))

- B. 更新文件(续)[\#180](https://github.com/ros2/message_filters/issues/180))

- 已删除丢失的 pragma (% 1)[\#179](https://github.com/ros2/message_filters/issues/179))

- 删除的订户折旧( E)[\#177](https://github.com/ros2/message_filters/issues/177))

- 已删除的已贬值信头( E)[\#176](https://github.com/ros2/message_filters/issues/176))

- 使用警告而不是警告( E)[\#178](https://github.com/ros2/message_filters/issues/178))

- Docs - 删除 C++ 9 通道的执行限制([\#174](https://github.com/ros2/message_filters/issues/174))

- 贡献者:亚历杭德罗·赫尔南德斯·科尔德罗、亚历杭德罗·埃尔南德斯·科尔德罗、亚历克斯·斯皮策尔、埃默森·克纳普、埃尔温·L.、埃西波夫帕、约翰内斯·伯姆、迈克尔·卡尔斯特罗姆、帕特里克·龙卡廖洛、帕维尔·埃西波夫、塞缪尔·福·恩泽、藤田友雅、增能\[bot\]、小型-1235、苔藓80

<span id="mimick-vendor"></span>

## [mimick_vendor](https://github.com/ros2/mimick_vendor/tree/lyrical/CHANGELOG.rst)

- 删除 CODEOWINERS 和镜像滚动到主工作流程 。 ([\#40](https://github.com/ros2/mimick_vendor/issues/40))

- 撰稿人:克里斯·拉兰谢特

<span id="nav-msgs"></span>

## [nav_msgs](https://github.com/ros2/common_interfaces/tree/lyrical/nav_msgs/CHANGELOG.rst)

- 添加轨迹和轨迹[\#296](https://github.com/ros2/common_interfaces/issues/296))

- 修复 CMAKE 折旧[\#288](https://github.com/ros2/common_interfaces/issues/288))

- 贡献者:史蒂夫·马肯斯基,摩斯费特80

<span id="osrf-testing-tools-cpp"></span>

## [osrf_testing_tools_cpp](https://github.com/osrf/osrf_testing_tools_cpp/tree/lyrical/osrf_testing_tools_cpp/CHANGELOG.rst)

- 固定 cmake min 版本 ([\#96](https://github.com/osrf/osrf_testing_tools_cpp/issues/96))

- 固定cmake 折旧([\#94](https://github.com/osrf/osrf_testing_tools_cpp/issues/94))

- 贡献者:苔藓80

<span id="pendulum-control"></span>

## [pendulum_control](https://github.com/ros2/demos/tree/lyrical/pendulum_control/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#781](https://github.com/ros2/demos//issues/781))

- 更新订阅回调签名( N)[\#754](https://github.com/ros2/demos/issues/754))

- 清除已腐烂的 rclp:: spin_some (). (中文(简体) ).[\#734](https://github.com/ros2/demos/issues/734))

- 在发射测试中使用 EullRmwIsolation ()[\#724](https://github.com/ros2/demos/issues/724))

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 设定 Envars 以 rmw_zenoh_cpp 进行多播发现的测试([\#711](https://github.com/ros2/demos/issues/711))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#707](https://github.com/ros2/demos/issues/707))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,爱默生·克纳普,斯科特·K·洛根,谢恩·洛雷茨,藤田托莫亚,小型-1235,苔藓80

<span id="pendulum-msgs"></span>

## [pendulum_msgs](https://github.com/ros2/demos/tree/lyrical/pendulum_msgs/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 贡献者:苔藓80

<span id="performance-test-fixture"></span>

## [performance_test_fixture](https://github.com/ros2/performance_test_fixture/tree/lyrical/CHANGELOG.rst)

- 固定cmake 折旧([\#31](https://github.com/ros2/performance_test_fixture/issues/31))

- 删除 CODEOWINERS 和镜像滚动到主工作流程 。 ([\#28](https://github.com/ros2/performance_test_fixture/issues/28))

- 贡献者:克里斯·拉朗谢特,苔藓80

<span id="pluginlib"></span>

## [插件lib](https://github.com/ros/pluginlib/tree/lyrical/pluginlib/CHANGELOG.rst)

- 解决一些次要问题[\#292](https://github.com/ros/pluginlib/issues/292))

- 添加对向构造器传递参数的支持( E)[\#291](https://github.com/ros/pluginlib/issues/291))

- 导出包括( E)[\#290](https://github.com/ros/pluginlib/issues/290))

- 更新已贬值的 ament_index_cpp API ([\#289](https://github.com/ros/pluginlib/issues/289))

- refactor: 将 regex 替换为 find\_ last_of 以分割插件名称([\#271](https://github.com/ros/pluginlib/issues/271))

- 已删除的微小xml2\_ vendor 依赖性([\#274](https://github.com/ros/pluginlib/issues/274))

- 添加 ros2plugin ((% 1)[\#165](https://github.com/ros/pluginlib/issues/165))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、杰雷米·德雷、伊帕-菲兹、普姆1k

<span id="point-cloud-transport"></span>

## [point_cloud_transport](https://github.com/ros-perception/point_cloud_transport/tree/lyrical/point_cloud_transport/CHANGELOG.rst)

- 通过使用漏泄型单吨制成全球装载器来修复无源64上的退出坠机([\#157](https://github.com/ros-perception/point_cloud_transport/issues/157))

- 包含消息类型 (E)[\#152](https://github.com/ros-perception/point_cloud_transport/issues/152))

- 使用新的聚合rosidl目标,而不是 \_TARGETS([\#153](https://github.com/ros-perception/point_cloud_transport/issues/153))合著:亚历克西斯·佐吉亚斯 \<[a.tsogias@cellumation.com](mailto:a.tsogias%40cellumation.com)\>

- 改进( E)[\#150](https://github.com/ros-perception/point_cloud_transport/issues/150))

- 公布ROS的原始出版商和订阅商[\#146](https://github.com/ros-perception/point_cloud_transport/issues/146)) ([\#148](https://github.com/ros-perception/point_cloud_transport/issues/148))

- 修复塞族共和国的重复部件登记([\#142](https://github.com/ros-perception/point_cloud_transport/issues/142))

- 删除过时的注释( R)[\#138](https://github.com/ros-perception/point_cloud_transport/issues/138))

- 使用标准未签名的 int 来取代 uint 来进行 Windows 兼容性 。[\#134](https://github.com/ros-perception/point_cloud_transport/issues/134))

- 更新用户过滤器( U)[\#126](https://github.com/ros-perception/point_cloud_transport/issues/126))

- 简化节点接口 API mehotd 调用([\#129](https://github.com/ros-perception/point_cloud_transport/issues/129))

- 固定QOS 覆盖测试( QOS )[\#128](https://github.com/ros-perception/point_cloud_transport/issues/128))

- 折旧的rmw_qos\_ profile_t (英语).[\#125](https://github.com/ros-perception/point_cloud_transport/issues/125))

- Feat/Add 生命周期节点支持[\#109](https://github.com/ros-perception/point_cloud_transport/issues/109))

- 添加 `rclcpp::shutdown` ([\#110](https://github.com/ros-perception/point_cloud_transport/issues/110))

- 撰稿人:亚历杭德罗·埃尔南德斯·科尔德罗、阿列克西斯·佐吉亚斯、埃尔赛义德·埃尔谢伊、迈克尔·卡罗尔、西尔维奥·特拉韦萨罗、柳延元、注音\[bot\]、小型-1235

<span id="point-cloud-transport-py"></span>

## [point_cloud_transport_py](https://github.com/ros-perception/point_cloud_transport/tree/lyrical/point_cloud_transport_py/CHANGELOG.rst)

- 使用新的聚合rosidl目标,而不是 \_TARGETS([\#153](https://github.com/ros-perception/point_cloud_transport/issues/153))

- Python 改进( U)[\#151](https://github.com/ros-perception/point_cloud_transport/issues/151))

- 从 deb 或 pixi 使用 pybind11 ()[\#131](https://github.com/ros-perception/point_cloud_transport/issues/131))

- 简化节点接口 API mehotd 调用([\#129](https://github.com/ros-perception/point_cloud_transport/issues/129))

- Feat/Add 生命周期节点支持[\#109](https://github.com/ros-perception/point_cloud_transport/issues/109))

- 撰稿人:亚历杭德罗·埃尔南德斯·科尔德罗、阿列克西斯·佐吉亚斯、埃尔赛义德·埃尔谢伊赫

<span id="python-qt-binding"></span>

## [python_qt_binding](https://github.com/ros-visualization/python_qt_binding/tree/lyrical/CHANGELOG.rst)

- 在构建时选择 Qt 版本, 而不是安装时间( S)[\#161](https://github.com/ros-visualization/python_qt_binding/issues/161))

- 重新添加执行依赖于 python3 qt 绑定 rosdep 密钥([\#160](https://github.com/ros-visualization/python_qt_binding/issues/160))

- 从 package.xml 中删除 qt6- base- dev ()[\#159](https://github.com/ros-visualization/python_qt_binding/issues/159))

- 取决于 phython3-dev ([\#158](https://github.com/ros-visualization/python_qt_binding/issues/158))

- 使用 sip- 构建和 python3\_ add\_ library 为 Qt5/ Qt6 ()[\#157](https://github.com/ros-visualization/python_qt_binding/issues/157))

- 固定设置工具 折旧( )[\#151](https://github.com/ros-visualization/python_qt_binding/issues/151))

- 固定cmake 折旧([\#150](https://github.com/ros-visualization/python_qt_binding/issues/150))

- 删除镜像滚动到主要工作流程( R)[\#145](https://github.com/ros-visualization/python_qt_binding/issues/145))

- 删除 CODEOWINERS ()[\#144](https://github.com/ros-visualization/python_qt_binding/issues/144))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、谢恩·洛雷茨、莫斯费特80

<span id="qt-dotgraph"></span>

## [qt_dotgraph](https://github.com/ros-visualization/qt_gui_core/tree/lyrical/qt_dotgraph/CHANGELOG.rst)

- 更多 qt6 修正 ([\#334](https://github.com/ros-visualization/qt_gui_core/issues/334)) ([\#335](https://github.com/ros-visualization/qt_gui_core/issues/335)(摘自承诺的樱桃 62f29544c4061006f9c09c3dfa4b2895e8126e0) 合著:亚历杭德罗·埃尔南德斯·科尔德罗 \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 支持 qt6 (帮助 )[\#293](https://github.com/ros-visualization/qt_gui_core/issues/293))

- 在测试中坚持片段存在时忽略大小写( E)[\#314](https://github.com/ros-visualization/qt_gui_core/issues/314))

- 固定装置 工具贬值( U)[\#308](https://github.com/ros-visualization/qt_gui_core/issues/308))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,斯科特·K·洛根,注音\[bot\],苔藓80

<span id="qt-gui"></span>

## [qt_gui](https://github.com/ros-visualization/qt_gui_core/tree/lyrical/qt_gui/CHANGELOG.rst)

- 更多 qt6 修正 ([\#334](https://github.com/ros-visualization/qt_gui_core/issues/334)) ([\#335](https://github.com/ros-visualization/qt_gui_core/issues/335)(摘自承诺的樱桃 62f29544c4061006f9c09c3dfa4b2895e8126e0) 合著:亚历杭德罗·埃尔南德斯·科尔德罗 \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 支持 qt6 (帮助 )[\#293](https://github.com/ros-visualization/qt_gui_core/issues/293))

- 固定( qt\_ gui ): \_ buildingin\_ - \> 内建( )[\#315](https://github.com/ros-visualization/qt_gui_core/issues/315))

- 修理cmake 折旧([\#307](https://github.com/ros-visualization/qt_gui_core/issues/307))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、马蒂斯·范德布尔赫、注解\[bot\]、苔藓80

<span id="qt-gui-app"></span>

## [qt_gui_app](https://github.com/ros-visualization/qt_gui_core/tree/lyrical/qt_gui_app/CHANGELOG.rst)

- 修理cmake 折旧([\#307](https://github.com/ros-visualization/qt_gui_core/issues/307))

- 贡献者:苔藓80

<span id="qt-gui-core"></span>

## [qt_gui_core](https://github.com/ros-visualization/qt_gui_core/tree/lyrical/qt_gui_core/CHANGELOG.rst)

- 更新 qt_gui_core 到 package.xml 版本 2 ().[\#319](https://github.com/ros-visualization/qt_gui_core/issues/319))

- 修理cmake 折旧([\#307](https://github.com/ros-visualization/qt_gui_core/issues/307))

- 贡献者:克里斯·拉朗谢特,苔藓80

<span id="qt-gui-cpp"></span>

## [qt_gui_cpp](https://github.com/ros-visualization/qt_gui_core/tree/lyrical/qt_gui_cpp/CHANGELOG.rst)

- 更多 qt6 修正 ([\#334](https://github.com/ros-visualization/qt_gui_core/issues/334)) ([\#335](https://github.com/ros-visualization/qt_gui_core/issues/335)(摘自承诺的樱桃 62f29544c4061006f9c09c3dfa4b2895e8126e0) 合著:亚历杭德罗·埃尔南德斯·科尔德罗 \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- find_package(Qt...) 在下游软件包中[\#332](https://github.com/ros-visualization/qt_gui_core/issues/332))

- 在 package.xml 中导出 qt 依赖性( )[\#331](https://github.com/ros-visualization/qt_gui_core/issues/331))

- 使用 qt- base- dev / libqtwidgets ([\#330](https://github.com/ros-visualization/qt_gui_core/issues/330))

- 支持 qt6 (帮助 )[\#293](https://github.com/ros-visualization/qt_gui_core/issues/293))

- 使用新的聚合rosidl目标,而不是 \_TARGETS([\#325](https://github.com/ros-visualization/qt_gui_core/issues/325))

- 删除未调整的设置.py([\#323](https://github.com/ros-visualization/qt_gui_core/issues/323))

- 已删除的微小xml2\_ vendor 依赖性([\#309](https://github.com/ros-visualization/qt_gui_core/issues/309))

- 修理cmake 折旧([\#307](https://github.com/ros-visualization/qt_gui_core/issues/307))

- 已删除的已贬值信头( E)[\#305](https://github.com/ros-visualization/qt_gui_core/issues/305))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#302](https://github.com/ros-visualization/qt_gui_core/issues/302))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,亚历克西斯·措吉亚斯,迈克尔·卡尔斯特罗姆,谢恩·洛雷茨,注音\[bot\],苔藓80

<span id="qt-gui-py-common"></span>

## [qt_gui_py_common](https://github.com/ros-visualization/qt_gui_core/tree/lyrical/qt_gui_py_common/CHANGELOG.rst)

- 更多 qt6 修正 ([\#334](https://github.com/ros-visualization/qt_gui_core/issues/334)) ([\#335](https://github.com/ros-visualization/qt_gui_core/issues/335))

- 支持 qt6 (帮助 )[\#293](https://github.com/ros-visualization/qt_gui_core/issues/293))

- 删除未调整的设置.py([\#323](https://github.com/ros-visualization/qt_gui_core/issues/323))

- 修理cmake 折旧([\#307](https://github.com/ros-visualization/qt_gui_core/issues/307))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、迈克尔·卡尔斯特罗姆、莫吉利特、莫斯费特80

<span id="quality-of-service-demo-cpp"></span>

## [quality_of_service_demo_cpp](https://github.com/ros2/demos/tree/lyrical/quality_of_service_demo/rclcpp/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#781](https://github.com/ros2/demos//issues/781))

- 切换到示例_界面( S)[\#674](https://github.com/ros2/demos/issues/674))

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714)) demo_nodes_cpp/CMakeLists.txt 需要cmake min 版本 3.12 其他模块 cmake 3.5 。它被提议与 3.12 版本标准化。它也修正 cmake \< 3.10 折旧警告

- 贡献者:爱默生·克纳普、卢卡斯·温德兰、苔藓80

<span id="quality-of-service-demo-py"></span>

## [quality_of_service_demo_py](https://github.com/ros2/demos/tree/lyrical/quality_of_service_demo/rclpy/CHANGELOG.rst)

- 切换到示例_界面( S)[\#674](https://github.com/ros2/demos/issues/674))

- 固定设置工具[\#731](https://github.com/ros2/demos/issues/731))

- 贡献者:卢卡斯·温德兰、苔藓80

<span id="rcl"></span>

## [rcl (中文(简体) ).](https://github.com/ros2/rcl/tree/lyrical/rcl/CHANGELOG.rst)

- 功绩: 在 rcl_waitset 中添加了对实体重复使用的检查( )[\#1206](https://github.com/ros2/rcl/issues/1206))

- 保留 `rmw_create_node` 错误状态在 `rcl_node_init` 通过使用 `RCL_EXPECT_ERROR_IS_SET` ([\#1313](https://github.com/ros2/rcl/issues/1313))

- 删除 clang 警告( E)[\#1315](https://github.com/ros2/rcl/issues/1315))

- 添加 RCL_EXPECT\_ ERROR_IS_SET 宏 ([\#1312](https://github.com/ros2/rcl/issues/1312))

- 改进了 rcl ⁇ YZ_set_on_new ⁇ YZ_召回文件([\#1289](https://github.com/ros2/rcl/issues/1289))

- 添加 rcl\_ 订阅\_ options\_ set\_ acceptable_buffer\_ 后端, 并有适当的寿命管理 ()[\#1308](https://github.com/ros2/rcl/issues/1308))

- 添加到 rcl_take_loaded\_ message 的微量点([\#1300](https://github.com/ros2/rcl/issues/1300))

- 应用“使用新的聚合rosidl目标,而不是_TARGETS([\#1302](https://github.com/ros2/rcl/issues/1302)”放在一些残渣上。[\#1309](https://github.com/ros2/rcl/issues/1309))

- 删除对 RCL 层内容过滤支持的检查( R)[\#1304](https://github.com/ros2/rcl/issues/1304))

- 使用新的聚合rosidl目标,而不是 \_TARGETS([\#1302](https://github.com/ros2/rcl/issues/1302))

- 添加客户端库的 API 设置动作服务器目标到期回调( Q)[\#1295](https://github.com/ros2/rcl/issues/1295))

- Fujitatomoya/ 改进rcl测试图([\#1296](https://github.com/ros2/rcl/issues/1296))

- 添加订阅内容过滤支持检查( N)[\#1293](https://github.com/ros2/rcl/issues/1293))

- rcl_logb_执行软件包支持. ([\#1276](https://github.com/ros2/rcl/issues/1276))

- 从 enum 切换中删除默认值, 以便编译器警告 。 ()[\#1278](https://github.com/ros2/rcl/issues/1278))

- 添加客户端服务器信息( N)[\#1161](https://github.com/ros2/rcl/issues/1161))

- 修复 REP url 地点([\#1271](https://github.com/ros2/rcl/issues/1271))

- rcl_logb_locator_初始化 () 支持 。 ([\#1049](https://github.com/ros2/rcl/issues/1049))

- 校对:发生者-\>occurs,成功-\>成功([\#1259](https://github.com/ros2/rcl/issues/1259))

- 参考文件中的 " 中间软件 " ,而不是 " DS执行 " ([\#1260](https://github.com/ros2/rcl/issues/1260))

- 通过 rmw_test_fixture 切换到孤立测试([\#1251](https://github.com/ros2/rcl/issues/1251))

- 修补 Cmake 折旧( S)[\#1249](https://github.com/ros2/rcl/issues/1249))

- 将历史QoS 输入测试\_ info_by_topic([\#1242](https://github.com/ros2/rcl//issues/1242))

- 为订阅选项添加一个测试: " ungore_local_publications " ([\#1239](https://github.com/ros2/rcl//issues/1239))

- 删除不必要的测试 \_ with_localhost\_ only. ()[\#1238](https://github.com/ros2/rcl/issues/1238))

- 在 rCl 测试_timer_init\_ state 中地址内存泄漏( )[\#1236](https://github.com/ros2/rcl/issues/1236))

- 删除未使用的非默认\_ qos\_ profile ()[\#1233](https://github.com/ros2/rcl/issues/1233))

- 已删除未使用的职能( E)[\#1230](https://github.com/ros2/rcl/issues/1230))

- 删除 rcl_qos\_ profile_rosout\_ 默认 。 ()[\#1225](https://github.com/ros2/rcl/issues/1225))

- 从测试中删除 rmw_connext 。 ()[\#1226](https://github.com/ros2/rcl/issues/1226))

- 修补由新鲜的Clang发现的夹线指针([\#1222](https://github.com/ros2/rcl/issues/1222))

- 撰稿人:科马达阿基希科、亚历杭德罗·埃尔南德斯·科尔德罗、亚历山大·科尔尼延科、阿列克西斯·佐吉亚斯、巴里·徐、陈共青团、克里斯托弗·贝达德、爱默生·克纳普、雅诺施·麦克豪林斯基、李、马里奥·多明格斯·洛佩斯、迈克尔·奥尔洛夫、明珠、奥伦贝尔博士、鲁沙安克·萨海伊、赛义尔·基绍尔·科塔科塔、谢恩·洛雷茨、斯凯勒·梅德罗斯、蒂姆·克莱法斯、托莫亚·藤田、莫斯费特80、雅敦德

<span id="rcl-action"></span>

## [rcl_action](https://github.com/ros2/rcl/tree/lyrical/rcl_action/CHANGELOG.rst)

- fix( rcl\_ action): 使用 RMW 隔离进行交叉节点测试([\#1311](https://github.com/ros2/rcl/issues/1311))

- 添加2个配置动作客户端反馈订阅内容过滤器的接口( Name[\#1287](https://github.com/ros2/rcl/issues/1287))

- 应用“使用新的聚合rosidl目标,而不是_TARGETS([\#1302](https://github.com/ros2/rcl/issues/1302)”放在一些残渣上。[\#1309](https://github.com/ros2/rcl/issues/1309))

- 简化计时器取消错误记录( E)[\#1307](https://github.com/ros2/rcl/issues/1307))

- 固定 : 防止在过期的\_ Timer 中出现短时间无止境循环( )[\#1303](https://github.com/ros2/rcl/issues/1303))

- 添加客户端库的 API 设置动作服务器目标到期回调( Q)[\#1295](https://github.com/ros2/rcl/issues/1295))

- 支持 rcl\_ action_count_客户端和 rcl_action_count_servers ().[\#1294](https://github.com/ros2/rcl/issues/1294))

- 修复 REP url 地点([\#1271](https://github.com/ros2/rcl/issues/1271))

- 添加 rcl\_ action_object_handle_is_botable (). (中文(简体) ).[\#1257](https://github.com/ros2/rcl/issues/1257))

- 修补 Cmake 折旧( S)[\#1249](https://github.com/ros2/rcl/issues/1249))

- 贡献者:亚历克西斯·措吉亚斯,贝里·许,雅诺施·麦克豪林斯基,斯凯勒·梅德罗斯,蒂姆·克莱法斯,藤田富茂亚,威廉·伍德尔,袁远,苔丝菲特80

<span id="rcl-interfaces"></span>

## [rcl_interfaces](https://github.com/ros2/rcl_interfaces/tree/lyrical/rcl_interfaces/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#180](https://github.com/ros2/rcl_interfaces/issues/180))

- 贡献者:苔藓80

<span id="rcl-lifecycle"></span>

## [rcl_lifecycle](https://github.com/ros2/rcl/tree/lyrical/rcl_lifecycle/CHANGELOG.rst)

- 应用“使用新的聚合rosidl目标,而不是_TARGETS([\#1302](https://github.com/ros2/rcl/issues/1302)”放在一些残渣上。[\#1309](https://github.com/ros2/rcl/issues/1309))

- 过渡活动中的过渡(续)([\#1269](https://github.com/ros2/rcl/issues/1269))

- 修复 REP url 地点([\#1271](https://github.com/ros2/rcl/issues/1271))

- 修补 Cmake 折旧( S)[\#1249](https://github.com/ros2/rcl/issues/1249))

- 引入 rcl\_ lifecycle_get\_ transition_label_by_id () 。 ([\#1229](https://github.com/ros2/rcl/issues/1229))

- 贡献者:亚历克西斯·佐吉亚斯、贾斯珀·范布拉克尔、蒂姆·克莱法斯、藤田友也、苔藓80

<span id="rcl-logging-implementation"></span>

## [rcl_logging_implementation](https://github.com/ros2/rcl_logging/tree/lyrical/rcl_logging_implementation/CHANGELOG.rst)

- 更新 rcl_logb_执行架构图。 ([\#137](https://github.com/ros2/rcl_logging/issues/137))

- rcl 记录执行([\#135](https://github.com/ros2/rcl_logging/issues/135)) \* rcl_log\_ 执行包的第 1 稿 。 \* 添加 test_log\_ 执行来检查动态加载 。 \* 地址 副驾驶审查 。 \* 固定 : 在 CMakeLists.txt \* 中为 DLL 导出正确可见度宏 。 \* 添加可见度控, 使用 RCL_LOGGING\_ IMPLATION\_ DEFAULT_VISILITY 。 \* 在初始化时加载所有符号 。 \* 使用goto模式来消除清理重复 。 \* 添加 rmw_log\_ 执行的基本设计文件 。 \* 使用 RCPPUTIS\_ SCOPE_EXIT 而不是 goto 语句 。 \* 记录可见度宏是不正确的 。\* 记录符号会保留到 peocess 实际退出 。 \* 共同授权 by 。 : 徐巴里(Barry Xu) \<[barry.xu@sony.com](mailto:barry.xu%40sony.com)\>

- 贡献者:藤田友也

<span id="rcl-logging-interface"></span>

## [rcl_logging_interface](https://github.com/ros2/rcl_logging/tree/lyrical/rcl_logging_interface/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#133](https://github.com/ros2/rcl_logging/issues/133))

- 贡献者:苔藓80

<span id="rcl-logging-noop"></span>

## [rcl_logging_noop](https://github.com/ros2/rcl_logging/tree/lyrical/rcl_logging_noop/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#133](https://github.com/ros2/rcl_logging/issues/133))

- 清理 rcl_logb\_ noop 依赖性 。 ([\#132](https://github.com/ros2/rcl_logging/issues/132)它不应该建立-出口-依赖任何东西(因为任何下游都不应该与之挂钩),它的所有依赖都可以是私人的。

- 贡献者:克里斯·拉朗谢特,苔藓80

<span id="rcl-logging-spdlog"></span>

## [rcl_logging_spdlog](https://github.com/ros2/rcl_logging/tree/lyrical/rcl_logging_spdlog/CHANGELOG.rst)

- 功绩: 添加嵌入变量以配置冲洗间隔( E)[\#139](https://github.com/ros2/rcl_logging/issues/139))

- 修补 cmake 折旧( E)[\#133](https://github.com/ros2/rcl_logging/issues/133))

- 错误时清理覆盖的警告消息 。 ()[\#128](https://github.com/ros2/rcl_logging/issues/128))

- 贡献者:阿基勒·韦尔希、克里斯·拉兰塞特、苔丝菲特80

<span id="rcl-yaml-param-parser"></span>

## [rcl_yaml_param_parser](https://github.com/ros2/rcl/tree/lyrical/rcl_yaml_param_parser/CHANGELOG.rst)

- 删除 clang 警告( E)[\#1315](https://github.com/ros2/rcl/issues/1315))

- 固定( E)[\#1310](https://github.com/ros2/rcl/issues/1310))

- 使用 POSIX 本地端来解析 YAML 双倍 ()[\#1292](https://github.com/ros2/rcl/issues/1292))

- rcl_yaml_node_struct_打印循环插件修复. (简体中文).[\#1290](https://github.com/ros2/rcl//issues/1290))

- rcl_yaml_param_parser: 添加二进制标签支持以加载字节数组参数([\#1256](https://github.com/ros2/rcl//issues/1256))

- 验证添加_name_to_ns函数中的名称输入([\#1281](https://github.com/ros2/rcl/issues/1281))

- parse\_ key () 应使用 yaml\_ map_lvl\_ t 而不是 uint\_ 32 。 ()[\#1279](https://github.com/ros2/rcl/issues/1279))

- 从 enum 切换中删除默认值, 以便编译器警告 。 ()[\#1278](https://github.com/ros2/rcl/issues/1278))

- 添加 yaml 标签支持( R)[\#1275](https://github.com/ros2/rcl/issues/1275))合著:李刘 ⁇  \<[Lei.Liu.AP@sony.com](mailto:Lei.Liu.AP%40sony.com)\>

- 修复 REP url 地点([\#1271](https://github.com/ros2/rcl/issues/1271))

- Fix param 文件由于命令而用通配符解析失败( S)[\#1253](https://github.com/ros2/rcl/issues/1253))

- 修补 Cmake 折旧( S)[\#1249](https://github.com/ros2/rcl/issues/1249))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,贝里·徐,胡加尔31,迈克尔·卡尔斯特罗姆,罗曼·雷尼耶,蒂姆·克莱法斯,藤田托莫亚,苔丝菲特80

<span id="rclcpp"></span>

## [rclcpp](https://github.com/ros2/rclcpp/tree/lyrical/rclcpp/CHANGELOG.rst)

- 包含事件 CBG 执行器( E)[\#3137](https://github.com/ros2/rclcpp/issues/3137))

- 修复IPC订阅的专题统计( NAME OF TRANSLATORS)[\#3130](https://github.com/ros2/rclcpp/issues/3130))

- 固定: 固定的 MSVC 编译错误 ([\#3135](https://github.com/ros2/rclcpp/issues/3135))

- 成就: 添加了召回组事件执行器( E) :[\#3097](https://github.com/ros2/rclcpp/issues/3097))

- 纠正错误的依赖性( E)[\#3133](https://github.com/ros2/rclcpp/issues/3133))

- 功绩: 切换到 c++20 并删除由此产生的编译警告 ([\#3124](https://github.com/ros2/rclcpp/issues/3124))

- 固定: 汇编 MSVC 2022 ([\#3131](https://github.com/ros2/rclcpp/issues/3131))

- 删除测试时的警告( E)[\#3125](https://github.com/ros2/rclcpp/issues/3125))

- 功率: 通过节点选项添加每个节点日志级别支持( )[\#3092](https://github.com/ros2/rclcpp/issues/3092))

- 缺少参数值时改进错误消息( E)[\#3093](https://github.com/ros2/rclcpp/issues/3093))

- 在内部修正错误的内部清晰度 `RingBufferImplementation` ([\#3116](https://github.com/ros2/rclcpp/issues/3116))

- 在订阅选项Base中添加可接受 \_buffer_后端字段([\#3098](https://github.com/ros2/rclcpp/issues/3098))

- 删除关于已删除的静态SingleThreadExecutor( ) 的注释[\#3121](https://github.com/ros2/rclcpp/issues/3121))

- 添加的跟踪点( N)[\#3103](https://github.com/ros2/rclcpp/issues/3103))

- 在 take_shared_方法中添加 ConstRefCallback([\#3066](https://github.com/ros2/rclcpp/issues/3066))

- 将拼写“\${rcl_interfaces_TARGES}”改为rcl_interfaces:::rcl_interfaces([\#3112](https://github.com/ros2/rclcpp/issues/3112))

- 使用新的 ROSIDL 聚合 CMake 目标([\#3105](https://github.com/ros2/rclcpp/issues/3105))

- 在 TestAny 订阅召回中删除重复的测试大小写:is_serialized_message_callback([\#3104](https://github.com/ros2/rclcpp/issues/3104))

- 保持事件原状, 预示比赛。 ([\#3099](https://github.com/ros2/rclcpp/issues/3099))

- 在订阅中添加支持检查内容过滤功能( N)[\#3089](https://github.com/ros2/rclcpp/issues/3089))

- 服务公共API中的 Expose ServiceType([\#3088](https://github.com/ros2/rclcpp/issues/3088))

- perf: 优化出共享的_ptr 副本 ([\#3079](https://github.com/ros2/rclcpp/issues/3079))

- 在内容过滤测试中避免 stale 参数事件 。 ()[\#3085](https://github.com/ros2/rclcpp/issues/3085))

- 改进匹配的查询时间\_ any_publishers () ()[\#3084](https://github.com/ros2/rclcpp/issues/3084))

- 添加测试隔离( E)[\#3081](https://github.com/ros2/rclcpp/issues/3081))

- 重置“ 改进匹配的查询时间\_ any\_ publishers () 。 ()[\#3068](https://github.com/ros2/rclcpp/issues/3068))” ([\#3077](https://github.com/ros2/rclcpp/issues/3077))

- 改进匹配\_ any_publishers () 的查询时间。 ([\#3068](https://github.com/ros2/rclcpp/issues/3068))

- 固定 : 使用默认的 rcl 分配器, 如果分配器为 std : 分配器 ([\#3058](https://github.com/ros2/rclcpp/issues/3058))

- 修补:测试情况下的各种数据种族( E)[\#3057](https://github.com/ros2/rclcpp/issues/3057))

- 固定 : 在 Callback Group 中修补数据竞赛: 大小( ) ()[\#3056](https://github.com/ros2/rclcpp/issues/3056))

- 删除默认值: 这样编译器就可以检测缺失的大小写 。 ()[\#3048](https://github.com/ros2/rclcpp/issues/3048))

- 对内存策略中的 rcl 实体使用弱 \_ptr 。 ()[\#2988](https://github.com/ros2/rclcpp/issues/2988))

- 删除测试\_ static\_ executor_entry_collection.cpp (中文(简体) ).[\#3041](https://github.com/ros2/rclcpp/issues/3041))

- 包括可能丢弃例外的第1个旋转。 ([\#3042](https://github.com/ros2/rclcpp/issues/3042))

- 如果参数操作失败, 在所有者节点上打印警告消息 。 ()[\#3037](https://github.com/ros2/rclcpp/issues/3037))

- 在等待消息等待设置中固定上下文( E)[\#3030](https://github.com/ros2/rclcpp/issues/3030))

- 重置“按上下文排列的构造等待时间([\#3021](https://github.com/ros2/rclcpp/issues/3021))” ([\#3028](https://github.com/ros2/rclcpp/issues/3028))

- 构造等待设置, 以上下文通过( C)[\#3021](https://github.com/ros2/rclcpp/issues/3021))

- 改进主题终点信息构建器的稳健性([\#3013](https://github.com/ros2/rclcpp/issues/3013))

- 调值共享的\_ ptr \< MessageT \> 订阅回调签名( )[\#2975](https://github.com/ros2/rclcpp/issues/2975))

- 更新已贬值的 ament_index_cpp API ([\#3011](https://github.com/ros2/rclcpp/issues/3011))

- 统一节点接口: 添加获取\_ node_x_interface () 的康斯特版本( )[\#3006](https://github.com/ros2/rclcpp/issues/3006))

- 简化(简写)[\#2179](https://github.com/ros2/rclcpp/issues/2179))

- 参数EventHandler 支持内容过滤( S)[\#2971](https://github.com/ros2/rclcpp/issues/2971))

- 更新策略\_ name\_ from_kind_QQ test_qos ([\#2156](https://github.com/ros2/rclcpp/issues/2156))

- 添加禁用和启用订阅回调功能的能力( E)[\#2985](https://github.com/ros2/rclcpp/issues/2985))

- 通过 rmw_test_fixture 切换到孤立测试([\#2929](https://github.com/ros2/rclcpp/issues/2929))

- 从信号处理器中删除 I/O 。 ([\#2986](https://github.com/ros2/rclcpp/issues/2986))

- 正确的测试函数描述( E)[\#2970](https://github.com/ros2/rclcpp/issues/2970))

- 添加 : 获取客户端、 服务器信息( E)[\#2569](https://github.com/ros2/rclcpp/issues/2569))

- 修复 REP url 地点([\#2987](https://github.com/ros2/rclcpp/issues/2987))

- 在 test_memory_stratgy 中,在节点销毁前,手柄清晰。 ()[\#2969](https://github.com/ros2/rclcpp/issues/2969))

- 添加的静态断言主张自定义类型没有新的过载操作员([\#2954](https://github.com/ros2/rclcpp/issues/2954))

- 在上下文中而不是节点图中存储图表倾听器( Name[\#2952](https://github.com/ros2/rclcpp/issues/2952))

- 重新应用“ 如果上下文无效, 请从 rate. sleep( ) 中选择例外 。 ()[\#2956](https://github.com/ros2/rclcpp/issues/2956))” ([\#2963](https://github.com/ros2/rclcpp/issues/2963)) ([\#2964](https://github.com/ros2/rclcpp/issues/2964))

- 返回“ 如果上下文无效, 请从 rate. sleep( ) 中选择例外 。 ()[\#2956](https://github.com/ros2/rclcpp/issues/2956))” ([\#2963](https://github.com/ros2/rclcpp/issues/2963))

- 如果上下文无效, 请从 rate. sleep () 中抓取例外 。 ([\#2956](https://github.com/ros2/rclcpp/issues/2956))

- 更新时间文件([\#2955](https://github.com/ros2/rclcpp/issues/2955))

- 删除警告( R)[\#2949](https://github.com/ros2/rclcpp/issues/2949))

- 添加关于旋转到\_ 未来\_ 完成的问题的注释( )[\#2849](https://github.com/ros2/rclcpp/issues/2849))

- 解析 rclp::spin_some 和 rclp::spin_all ()[\#2848](https://github.com/ros2/rclcpp/issues/2848))

- 改进函数提取_类型_识别符( )[\#2923](https://github.com/ros2/rclcpp/issues/2923))

- 也允许隐性可转换的伐木机([\#2922](https://github.com/ros2/rclcpp/issues/2922))

- Fix: 改进参数_值\_ from( ) 的例外上下文[\#2917](https://github.com/ros2/rclcpp/issues/2917))

- 修补 `start_type_description_service` 电阻处理( 超时处理)[\#2897](https://github.com/ros2/rclcpp/issues/2897))

- 添加 qos 参数用于等待\_ for\_ message 函数([\#2903](https://github.com/ros2/rclcpp/issues/2903))

- Fujitatomoya/测试附件参数覆盖([\#2896](https://github.com/ros2/rclcpp/issues/2896))

- 曝光 `typesupport_helpers` Rosbag2型机车所需的API([\#2858](https://github.com/ros2/rclcpp/issues/2858))

- 删除关于现在已删除的静态SingleThreadedExector的注释([\#2893](https://github.com/ros2/rclcpp/issues/2893))

- 添加过载 `append_parameter_override` ([\#2891](https://github.com/ros2/rclcpp/issues/2891))

- 固定 : 如果在关闭回调中取消关闭回调, 不要陷入僵局( Name[\#2886](https://github.com/ros2/rclcpp/issues/2886))

- 手码登录. hpp (中文(简体) ).[\#2870](https://github.com/ros2/rclcpp/issues/2870))

- 节点图中的 TODO ()[\#2877](https://github.com/ros2/rclcpp/issues/2877))

- 固定测试_publisher\_ with_system_default_qos. (中文(简体) ).[\#2881](https://github.com/ros2/rclcpp/issues/2881))

- 修复 Rclcpp 中的内存泄漏: 序列化Message ()[\#2861](https://github.com/ros2/rclcpp/issues/2861))

- 删除警告测试\_ qos( Name[\#2859](https://github.com/ros2/rclcpp/issues/2859))

- 添加缺失的染色体包括( 包含)[\#2854](https://github.com/ros2/rclcpp/issues/2854))

- 获取_all_data_impl () 不正确处理空指针, 导致分割断层([\#2840](https://github.com/ros2/rclcpp/issues/2840))

- QoSinitialization: from_rmw 不验证无效的历史政策值,导致无声失败([\#2841](https://github.com/ros2/rclcpp/issues/2841))

- 从 NodeBase 界面中删除“ 注意” \_ 护卫\_ 条件 。 ([\#2839](https://github.com/ros2/rclcpp/issues/2839))

- 删除已拆解的静态SingleThreaded执行器([\#2835](https://github.com/ros2/rclcpp/issues/2835))

- 删除已贬值的弧形路径( E)[\#2834](https://github.com/ros2/rclcpp/issues/2834))

- 对适用的数组参数添加范围限制( E)[\#2828](https://github.com/ros2/rclcpp/issues/2828))

- 更新 RingBuffer 执行以清除内部数据。 ([\#2837](https://github.com/ros2/rclcpp/issues/2837))

- 删除已贬值的取消_sleep_或_wait ()[\#2836](https://github.com/ros2/rclcpp/issues/2836))

- 在文件/注释中将缺失的 's ' 添加到 'NodeParameters Interface ' ([\#2831](https://github.com/ros2/rclcpp/issues/2831))

- 下节点一致的行为和更新docstring. ().[\#2822](https://github.com/ros2/rclcpp/issues/2822))

- 抛出 std:: 如果参数Event 是 NULL , 则无效\_ 参数 。 ([\#2814](https://github.com/ros2/rclcpp/issues/2814))

- 删除了 clang 警告( R)[\#2823](https://github.com/ros2/rclcpp/issues/2823))

- 贡献者:阿尔韦托·索拉尼亚、亚历杭德罗·埃尔南德斯·科尔德罗、亚历克斯·杨斯、亚历克西斯·佐吉亚斯、安德里亚诺夫·罗曼、巴里·徐、陈共、克里斯·拉朗谢特、克里斯托弗·贝达德、达尼尔、埃默森·克纳普、伊拉里奥·阿佐利尼、伊沃·伊万诺夫、亚诺施·马科温斯基、朱利安·埃诺奇、李、卢卡斯·温德兰、莫里斯·亚历山大·普尔纳万、迈克尔·卡罗尔、迈克尔·奥尔洛夫、米歇尔·李格沃特、明朱、奥伦·贝尔、帕特里克·龙卡廖洛、彭旺、拉哈特·丹德、斯凯勒·梅德罗斯、斯里哈尔沙·甘塔、蒂姆·克莱法斯、托莫亚·富塔、亚·亚赫尔什瓦尔·萨克 哈雷,尤琴966,法比安希尔曼,杰伊,雅敦德

<span id="rclcpp-action"></span>

## [rclcpp_action](https://github.com/ros2/rclcpp/tree/lyrical/rclcpp_action/CHANGELOG.rst)

- 发布\_ feedback 只应对执行状态生效 。 ([\#3118](https://github.com/ros2/rclcpp/issues/3118))

- 支持为动作客户端配置反馈订阅内容过滤器( S)[\#3034](https://github.com/ros2/rclcpp/issues/3034))

- 使用新的 ROSIDL 聚合 CMake 目标([\#3105](https://github.com/ros2/rclcpp/issues/3105))

- 在使用事件执行器时, 固定动作目标的到期时间( Q) :[\#3018](https://github.com/ros2/rclcpp/issues/3018))

- perf: 优化出共享的_ptr 副本 ([\#3079](https://github.com/ros2/rclcpp/issues/3079))

- 删除默认值: 这样编译器就可以检测缺失的大小写 。 ()[\#3048](https://github.com/ros2/rclcpp/issues/3048))

- 更新用于服务器GoalHandle中目标取消的例外文件([\#3019](https://github.com/ros2/rclcpp/issues/3019))

- 修复 REP url 地点([\#2987](https://github.com/ros2/rclcpp/issues/2987))

- 它错过了移动器第二 锁定弱者。 ([\#2958](https://github.com/ros2/rclcpp/issues/2958))

- 尝试在取消服务器GoalHandle dtor 上的第 1 位前中止 。 ()[\#2953](https://github.com/ros2/rclcpp/issues/2953))

- 解析 rclp::spin_some 和 rclp::spin_all ()[\#2848](https://github.com/ros2/rclcpp/issues/2848))

- 固定cmake 折旧([\#2914](https://github.com/ros2/rclcpp/issues/2914))

- 将 std 替换为: 默认\_ random\_ engine 替换为 std: mt19937 (滚动) ([\#2843](https://github.com/ros2/rclcpp/issues/2843))

- 添加缺失的染色体包括( 包含)[\#2854](https://github.com/ros2/rclcpp/issues/2854))

- 贡献者:阿尔韦托·索拉格纳,亚历杭德罗·埃尔南德斯·科尔德罗,安德烈·科斯蒂内斯库,巴里·许,爱默生·克纳普,亚诺施·麦克豪林斯基,斯凯勒·梅德罗斯,蒂姆·克莱法斯,藤田友也,保托诺伊罗,摩斯费特80

<span id="rclcpp-components"></span>

## [rclcpp_components](https://github.com/ros2/rclcpp/tree/lyrical/rclcpp_components/CHANGELOG.rst)

- 重构组件容器 + 为 CBG 执行器添加选项( )[\#3134](https://github.com/ros2/rclcpp/issues/3134))

- 功率: 通过节点选项添加每个节点日志级别支持( )[\#3092](https://github.com/ros2/rclcpp/issues/3092))

- 使用新的 ROSIDL 聚合 CMake 目标([\#3105](https://github.com/ros2/rclcpp/issues/3105))

- 避免不必要地创建多路径执行器( E)[\#3090](https://github.com/ros2/rclcpp/issues/3090))

- 在子目录中注册的固定元件( S)[\#3064](https://github.com/ros2/rclcpp/issues/3064))

- 在 rclcpp_组件_register_node 中将库依赖性添加到可执行的节点([\#3047](https://github.com/ros2/rclcpp/issues/3047))

- 更新已贬值的 ament_index_cpp API ([\#3011](https://github.com/ros2/rclcpp/issues/3011))

- 修复 REP url 地点([\#2987](https://github.com/ros2/rclcpp/issues/2987))

- 清除 rclcpp\_ 组件中的依赖性 。 ()[\#2918](https://github.com/ros2/rclcpp/issues/2918))

- 固定cmake 折旧([\#2914](https://github.com/ros2/rclcpp/issues/2914))

- New PR: 添加组件_事件执行器的容器( E)[\#2885](https://github.com/ros2/rclcpp/issues/2885))

- 确定插件参数包括双冒号。 ()[\#2878](https://github.com/ros2/rclcpp/issues/2878))

- 在孤立的组件容器中按节点设置线程名称( Q)[\#2871](https://github.com/ros2/rclcpp/issues/2871))

- 添加缺失的染色体包括( 包含)[\#2854](https://github.com/ros2/rclcpp/issues/2854))

- 贡献者:亚当·阿波希安,亚历杭德罗·埃尔南德斯·科尔德罗,克里斯·拉朗谢特,爱默生·克纳普,米希尔·拉奥,彭旺,斯凯勒·梅德罗斯,蒂姆·克莱法斯,藤田友也,尤金·洪,苔藓·费特80,pum1k,独奏

<span id="rclcpp-lifecycle"></span>

## [rclcpp_lifecycle](https://github.com/ros2/rclcpp/tree/lyrical/rclcpp_lifecycle/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#3105](https://github.com/ros2/rclcpp/issues/3105))

- 与“平稳过渡”的兼容性 [ros2/rcl#1269](https://github.com/ros2/rcl/issues/1269) ([\#2967](https://github.com/ros2/rclcpp/issues/2967))

- 添加 : 获取客户端、 服务器信息( E)[\#2569](https://github.com/ros2/rclcpp/issues/2569))

- 修复 REP url 地点([\#2987](https://github.com/ros2/rclcpp/issues/2987))

- 添加 get_parameter_或超载返回值或选项([\#2973](https://github.com/ros2/rclcpp/issues/2973))

- 解析 rclp::spin_some 和 rclp::spin_all ()[\#2848](https://github.com/ros2/rclcpp/issues/2848))

- 更清晰的警告信息,旧信息缺乏信息,也许具有误导性([\#2927](https://github.com/ros2/rclcpp/issues/2927))

- 固定cmake 折旧([\#2914](https://github.com/ros2/rclcpp/issues/2914)cmake 版本 \< 然后3.10 贬值

- 添加缺失的染色体包括( 包含)[\#2854](https://github.com/ros2/rclcpp/issues/2854))

- 引入 rcl\_ lifecycle_get\_ transition_label_by_id () 。 ([\#2827](https://github.com/ros2/rclcpp/issues/2827))

- 贡献者:阿尔韦托·索拉尼亚、亚历杭德罗·埃尔南德斯·科尔德罗、埃默森·克纳普、贾斯珀·范布拉克尔、李、明朱、彼得·米特拉诺(AR)、蒂姆·克莱法斯、藤田丰也、郑奎、苔丝菲特80

<span id="rclpy"></span>

## [rclpy](https://github.com/ros2/rclpy/tree/lyrical/rclpy/CHANGELOG.rst)

- 特性: 自动节点( Q)[\#1620](https://github.com/ros2/rclpy/issues/1620))

- 重构: 已移动类型Description Service, Logging Service, Parameter Service 到 BaseNode ([\#1645](https://github.com/ros2/rclpy/issues/1645))

- 重构: 基点节点( R)[\#1637](https://github.com/ros2/rclpy/issues/1637))

- Bugfix: 执行器不宣传等待未来的任务的例外( )[\#1643](https://github.com/ros2/rclpy/issues/1643))

- 修正: 禁用片面执行器测试( F)[\#1648](https://github.com/ros2/rclpy/issues/1648)) ([\#1649](https://github.com/ros2/rclpy/issues/1649))

- 精简实体销毁([\#1629](https://github.com/ros2/rclpy/issues/1629))

- 在 rclpy 中添加可接受_buffer_后端作为订阅选项([\#1628](https://github.com/ros2/rclpy/issues/1628))

- 发布\_ feedback 只应对执行状态生效 。 ([\#1639](https://github.com/ros2/rclpy/issues/1639))

- 支持为动作客户端配置反馈订阅内容过滤器( S)[\#1633](https://github.com/ros2/rclpy/issues/1633))

- 修补 Flashy 测试\_ 多重线程\_ 执行器\_ 关闭线程 。 ()[\#1636](https://github.com/ros2/rclpy/issues/1636))

- 纠正违反行为([\#1635](https://github.com/ros2/rclpy/issues/1635))

- 固定测试\_ 执行器类型( F)[\#1632](https://github.com/ros2/rclpy/issues/1632))

- 重构: 基准时钟( R)[\#1627](https://github.com/ros2/rclpy/issues/1627))

- 修复未来的片段8([\#1634](https://github.com/ros2/rclpy/issues/1634))

- 使用新的 ROSIDL 聚合 CMake 目标([\#1630](https://github.com/ros2/rclpy/issues/1630))

- 更新参数类型提示( E)[\#1631](https://github.com/ros2/rclpy/issues/1631))

- 在订阅中添加支持检查内容过滤功能( N)[\#1618](https://github.com/ros2/rclpy/issues/1618))

- 要素:基础实体类别([\#1624](https://github.com/ros2/rclpy/issues/1624))

- 修复更多测试打字并删除未使用的类型别名( S)[\#1626](https://github.com/ros2/rclpy/issues/1626))

- 添加类型到 test_waitable ([\#1625](https://github.com/ros2/rclpy/issues/1625))

- 校正类型( E)[\#1619](https://github.com/ros2/rclpy/issues/1619))

- 修正错误的动作客户端/服务器召回类型提示( S)[\#1616](https://github.com/ros2/rclpy/issues/1616))

- 在内容过滤测试中避免 stale 参数事件 。 ()[\#1615](https://github.com/ros2/rclpy/issues/1615))

- 纠正侵犯行为([\#1614](https://github.com/ros2/rclpy/issues/1614))

- 打字后退修正( R)[\#1612](https://github.com/ros2/rclpy/issues/1612))

- CFT只支持rmw_fastrtps和rmw_connexts. (中文(简体) ).[\#1611](https://github.com/ros2/rclpy/issues/1611))

- 防止两度设定未来结果。 ([\#1599](https://github.com/ros2/rclpy/issues/1599))

- 在旋转前总结动作连接结构( Q)[\#1591](https://github.com/ros2/rclpy/issues/1591))

- 与“平稳过渡”的兼容性 [ros2/rcl#1269](https://github.com/ros2/rcl/issues/1269) ([\#1528](https://github.com/ros2/rclpy/issues/1528))

- 将无效的等待器从等待设置中丢弃( R)[\#1590](https://github.com/ros2/rclpy/issues/1590))

- 给一些时间用于测试\_ on_new_message_callback。 ([\#1585](https://github.com/ros2/rclpy/issues/1585))

- 如果参数操作失败, 在所有者节点上打印警告消息 。 ()[\#1584](https://github.com/ros2/rclpy/issues/1584))

- 将发行版本更新为10.0.4([\#1583](https://github.com/ros2/rclpy/issues/1583))

- 更新 `type_support.py` 以使用新信件抽象基类( Q)[\#1509](https://github.com/ros2/rclpy/issues/1509))

- 在多路径执行器( 希望) 中修正性能错误( MultiThreadedExecutor) (P) :[\#1547](https://github.com/ros2/rclpy/issues/1547))

- 曝光动作图作为节点类方法的函数 。 ()[\#1574](https://github.com/ros2/rclpy/issues/1574))

- 改进通配符解析并优化解析 YAML 段落的逻辑... ()[\#1571](https://github.com/ros2/rclpy/issues/1571))

- 改进处理 YAML 参数文件的兼容性( )[\#1548](https://github.com/ros2/rclpy/issues/1548))

- 固定参数解析未指定的目标节点( N)[\#1552](https://github.com/ros2/rclpy/issues/1552))

- 从 enum 切换中删除默认值, 以便编译器警告 。 ()[\#1566](https://github.com/ros2/rclpy/issues/1566))

- 尽可能使用无条件的等待。 ([\#1563](https://github.com/ros2/rclpy/issues/1563))

- 提高时钟准确度( Q)[\#1564](https://github.com/ros2/rclpy/issues/1564))

- 解决未来恢复同步任务的问题([\#1469](https://github.com/ros2/rclpy/issues/1469))

- 参数EventHandler 支持内容过滤( S)[\#1531](https://github.com/ros2/rclpy/issues/1531))

- 添加 : 获取客户端、 服务器信息( E)[\#1307](https://github.com/ros2/rclpy/issues/1307))

- 允许不执行召回的动作服务器( E)[\#1219](https://github.com/ros2/rclpy/issues/1219))

- 删除意外的拖曳( E)[\#1542](https://github.com/ros2/rclpy/issues/1542))

- 固定( 测试\_ events\_ executor): 在关闭前销毁所有节点( T)[\#1538](https://github.com/ros2/rclpy/issues/1538))

- 从 send_goal_async 中删除重复的未来处理( )[\#1532](https://github.com/ros2/rclpy/issues/1532))

- 删除未使用的“ param\_ type ” ([\#1524](https://github.com/ros2/rclpy/issues/1524))

- Fixes Action. QQSync 期货永远不会完成( Fixes Action.[\#1308](https://github.com/ros2/rclpy/issues/1308))

- 添加执行类的旋转状态 。 ()[\#1510](https://github.com/ros2/rclpy/issues/1510))

- 事件执行器: 处理服务及订阅的自动调用( E)[\#1478](https://github.com/ros2/rclpy/issues/1478))

- 添加锁定以保护多行执行器的期货( Q)[\#1477](https://github.com/ros2/rclpy/issues/1477))

- 添加内容过滤-专题接口( Q)[\#1506](https://github.com/ros2/rclpy/issues/1506))

- 从 gcc. 修正警告 ([\#1501](https://github.com/ros2/rclpy/issues/1501))

- 特性:在订阅、服务、客户端和定时器中曝光事件召回定时器([\#1496](https://github.com/ros2/rclpy/issues/1496))

- 特性: 添加执行器. create_future () ([\#1495](https://github.com/ros2/rclpy/issues/1495))

- 添加更多测试打字符( N)[\#1472](https://github.com/ros2/rclpy/issues/1472))

- 从 deb 或 pixi 使用 pybind11 ()[\#1497](https://github.com/ros2/rclpy/issues/1497))

- 如果 call_timer\_ with_info () 失败, 请不要执行计时器( )[\#1488](https://github.com/ros2/rclpy/issues/1488))

- 修补建立警告的磁盘 `operator==` pybind11的折旧[\#1483](https://github.com/ros2/rclpy/issues/1483))

- 清理 Rclpy 依赖性 。 ()[\#1482](https://github.com/ros2/rclpy/issues/1482))

- 特性:在订阅、出版、服务和客户端中添加logger_name属性([\#1471](https://github.com/ros2/rclpy/issues/1471))

- 更新 `test_node` 类型([\#1464](https://github.com/ros2/rclpy/issues/1464))

- 添加从时间获得日期时间. 日期时间的方法( T)[\#1443](https://github.com/ros2/rclpy/issues/1443))

- 添加( E) `MessageInfo.publisher_gid` ([\#1466](https://github.com/ros2/rclpy/issues/1466))

- 添加类型到 `test_action\_\*.py` ([\#1444](https://github.com/ros2/rclpy/issues/1444))

- 还原“持续时间、时钟和QoS Docs([\#1428](https://github.com/ros2/rclpy/issues/1428))” ([\#1447](https://github.com/ros2/rclpy/issues/1447))

- 删除所有已贬值的类和方法( E)[\#1456](https://github.com/ros2/rclpy/issues/1456))

- \[rclpy\] 固定自旋() 如果已经附加, 错误地从执行器中去除节点( )[\#1446](https://github.com/ros2/rclpy/issues/1446))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、阿隆·博伦施泰因、奥古斯特·拉兰德、巴里·徐、布拉德·马丁、布伦南·米勒-克卢格曼、布劳瓦·埃杰·索瓦、陈共青团、克里斯·拉朗谢、克里斯·拉朗谢、克拉拉·贝伦森、埃默森·克纳普、弗洛里安·瓦尔、贾斯珀·范布拉克尔、让·保罗、乔纳森、李、迈克尔·卡尔斯特罗姆、迈克尔·坦迪、明朱、纳达夫·埃尔卡贝茨、内森·维贝·诺伊费尔特、蒂姆·克莱法斯、托莫亚·藤田、玉元、姆希达尔戈赖

<span id="rcpputils"></span>

## [弧形图案](https://github.com/ros2/rcpputils/tree/lyrical/CHANGELOG.rst)

- 与 tl_预期有关的更新说明([\#229](https://github.com/ros2/rcpputils/issues/229))

- 扩大测试覆盖面([\#222](https://github.com/ros2/rcpputils/issues/222))

- 从作品中添加 BSD 和 CC0 许可证副本([\#223](https://github.com/ros2/rcpputils/issues/223))

- 使用 std: file system in find_library 并添加更多的测试([\#221](https://github.com/ros2/rcpputils/issues/221))

- 从 Clang 编译选项中删除 - 错误( W)[\#220](https://github.com/ros2/rcpputils/issues/220))

- 从 rcpputils 中删除不必要的依赖性 。 ()[\#216](https://github.com/ros2/rcpputils/issues/216)它不需要依赖蟒蛇测试。

- 固定cmake 折旧([\#214](https://github.com/ros2/rcpputils/issues/214))

- 添加线程命名工具( I)[\#213](https://github.com/ros2/rcpputils/issues/213))

- 删除已贬值的路径( N)[\#212](https://github.com/ros2/rcpputils/issues/212))

- 贡献者:亚当·阿波希安,亚历杭德罗·埃尔南德斯·科尔德罗,克里斯·拉兰谢特,图利·福特,威廉·伍德尔,苔丝菲特80

<span id="rcutils"></span>

## [rcutils 维基月球](https://github.com/ros2/rcutils/tree/lyrical/CHANGELOG.rst)

- 添加构建工具\_ 导出\_ 依赖 ament\_ cmake\_ ros\_ core( )[\#558](https://github.com/ros2/rcutils/issues/558))

- 修补: 覆盖参数文档中的类型( T)[\#557](https://github.com/ros2/rcutils/issues/557))

- 删除 ATOMIC_VAR_INIT ([\#556](https://github.com/ros2/rcutils/issues/556))

- 使用 `ament_set_default_language_standards` 从 `ament_cmake_core` ([\#548](https://github.com/ros2/rcutils/issues/548))

- 在宏中使用不寻常的变量名称以避免被覆盖( U)[\#551](https://github.com/ros2/rcutils/issues/551))

- 删除 `ament_export_link_flags()` 用于原子操作的[\#528](https://github.com/ros2/rcutils/issues/528))

- 在宏中使用不太常见的变量名称( E)[\#550](https://github.com/ros2/rcutils/issues/550))

- 缺少 std: get\_ time 的包含[\#549](https://github.com/ros2/rcutils/issues/549))

- 修正gcc 15.2.1 丢弃 " 压缩 " 限定词的警告([\#547](https://github.com/ros2/rcutils/issues/547))

- 禁用 Base64.c 中较老的 Windows SDKs 的警告 C5105 ()[\#544](https://github.com/ros2/rcutils/issues/544))

- 添加 {short_file_name} 作为日志格式选项 ([\#541](https://github.com/ros2/rcutils/issues/541))

- 添加基数64的编码和解码功能,同时进行测试( )[\#533](https://github.com/ros2/rcutils/issues/533))

- 删除默认值: 这样编译器就可以检测缺失的大小写 。 ()[\#534](https://github.com/ros2/rcutils/issues/534))

- 检查 SIZE\_ MAX 的阵列初始化 。 ()[\#527](https://github.com/ros2/rcutils/issues/527))

- 不以 rcutils\_ LIBRARIES 格式导出 dl ()[\#522](https://github.com/ros2/rcutils/issues/522))

- rcutils_logb_locator_初始化 () 支持 。 ([\#419](https://github.com/ros2/rcutils/issues/419))

- 导出 - 原子, 即使 BUILD\_ TESTING 已禁用 。 ()[\#516](https://github.com/ros2/rcutils/issues/516))

- 添加 rcutils\_ raw_stedy_time\_ 现在的无时钟方法([\#507](https://github.com/ros2/rcutils/issues/507))

- 重置“ Windows 使用 geenv_s 而不是 geenv 。 ()[\#499](https://github.com/ros2/rcutils/issues/499))” ([\#504](https://github.com/ros2/rcutils/issues/504)(b) 恢复承诺46ab4d4eeb55a2e9e880157b97f0a867d3a256c。

- 手码记录_macros.h([\#502](https://github.com/ros2/rcutils/issues/502))

- 执行rcutils_strnlen. ().[\#430](https://github.com/ros2/rcutils/issues/430))

- 对 Windows 使用 getenv_s 而不是 getenv 。 ()[\#499](https://github.com/ros2/rcutils/issues/499))

- 使木工快乐

- 在 test\_ process.cpp 中清除内存([\#495](https://github.com/ros2/rcutils/issues/495))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、安德烈·霍洛德尼、巴里·徐、克里斯·拉兰谢特、埃迪加里布、米格尔公司、西·基绍尔·科塔科塔、谢恩·洛雷茨、藤田托莫亚、托尼·纳贾尔

<span id="resource-retriever"></span>

## [resource_retriever](https://github.com/ros/resource_retriever/tree/lyrical/resource_retriever/CHANGELOG.rst)

- 删除了 python2 代码 ([\#121](https://github.com/ros/resource_retriever/issues/121))

- 删除资源\_ retriever/ setup.py([\#120](https://github.com/ros/resource_retriever/issues/120))

- 使用 get\_ package_share\_ path ([\#119](https://github.com/ros/resource_retriever/issues/119))

- 更新已贬值的 ament_index_cpp API ([\#118](https://github.com/ros/resource_retriever/issues/118))

- 已删除的 libcurl\_ vendor 软件包([\#116](https://github.com/ros/resource_retriever/issues/116))

- 删除已折旧的代码( N)[\#113](https://github.com/ros/resource_retriever/issues/113))

- 固定 clang 编译错误 (% 1)[\#112](https://github.com/ros/resource_retriever/issues/112))

- 删除窗口警告( R)[\#111](https://github.com/ros/resource_retriever/issues/111))

- 添加一个插件机制到资源回收器( E)[\#103](https://github.com/ros/resource_retriever/issues/103))

- 制服 MinCMakeVersion (英语:[\#108](https://github.com/ros/resource_retriever/issues/108))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、迈克尔·卡尔斯特罗姆、迈克尔·卡罗尔、莫斯费特80

<span id="resource-retriever-interfaces"></span>

## [resource_retriever_interfaces](https://github.com/ros2/resource_retriever_service/tree/lyrical/resource_retriever_interfaces/CHANGELOG.rst)

- 更新插件许可证( E)[\#17](https://github.com/ros2/resource_retriever_service/issues/17))

- 贡献者:斯托扬·盖达罗夫

<span id="resource-retriever-service"></span>

## [resource_retriever_service](https://github.com/ros2/resource_retriever_service/tree/lyrical/resource_retriever_service/CHANGELOG.rst)

- 更新插件许可证( E)[\#17](https://github.com/ros2/resource_retriever_service/issues/17))

- 贡献者:斯托扬·盖达罗夫

<span id="resource-retriever-service-plugin"></span>

## [resource_retriever_service_plugin](https://github.com/ros2/resource_retriever_service/tree/lyrical/resource_retriever_service_plugin/CHANGELOG.rst)

- 更新插件许可证( E)[\#17](https://github.com/ros2/resource_retriever_service/issues/17))

- 贡献者:斯托扬·盖达罗夫

<span id="rmw"></span>

## [rmw (英语).](https://github.com/ros2/rmw/tree/lyrical/rmw/CHANGELOG.rst)

- 查找(\_package ament_cmake_gtest) (中文(简体) ).[\#417](https://github.com/ros2/rmw/issues/417))

- 在 rmw_订阅_options_s 中添加可接受_buffer_后端字段([\#416](https://github.com/ros2/rmw/issues/416))

- 添加为\_ cft\_ 支持的字段为 rmw\_ 订阅\_ t ,用于内容过滤支持( C)[\#415](https://github.com/ros2/rmw/issues/415))

- 从 enum 切换中删除默认值, 以便编译器警告 。 ()[\#414](https://github.com/ros2/rmw/issues/414))

- 加:获取客户端、服务器信息( Q)[\#371](https://github.com/ros2/rmw//issues/371))

- 修复 REP url 地点([\#406](https://github.com/ros2/rmw//issues/406))

- 更新 Rmw API 文件的链接([\#405](https://github.com/ros2/rmw//issues/405))

- 在函数文件中,不要假定基于DS的执行([\#402](https://github.com/ros2/rmw//issues/402))

- 撰稿人:巴瑞·许,共青团陈,克里斯托弗·贝达德,李,明珠,谢恩·洛雷茨,蒂姆·克莱法斯,藤田友也.

<span id="rmw-connextdds"></span>

## [rmw_connextdds](https://github.com/ros2/rmw_connextdds/tree/lyrical/rmw_connextdds/CHANGELOG.rst)

- fix: MSVC 2022上的固定编译([\#225](https://github.com/ros2/rmw_connextdds/issues/225))

- 删除带有 enum 的切换器的默认值以启用编译器警告( E)[\#216](https://github.com/ros2/rmw_connextdds/issues/216))

- 添加: 获取客户端、 服务器信息( E)[\#154](https://github.com/ros2/rmw_connextdds/issues/154))

- 固定: 删除超浮点数 `buildtool_export_depend` ([\#206](https://github.com/ros2/rmw_connextdds/issues/206))

- 修补 cmake 折旧( E)[\#198](https://github.com/ros2/rmw_connextdds/issues/198))

- 贡献者:巴斯·扎姆斯特拉,雅诺施·麦克豪林斯基,李,明珠,藤田友也,苔丝菲特80

<span id="rmw-connextdds-common"></span>

## [rmw_connextdds_common](https://github.com/ros2/rmw_connextdds/tree/lyrical/rmw_connextdds_common/CHANGELOG.rst)

- 以现代 Connext DDS 修正 Windows 上的内容过滤( S)[\#226](https://github.com/ros2/rmw_connextdds/issues/226)) ([\#230](https://github.com/ros2/rmw_connextdds/issues/230))

- fix: MSVC 2022上的固定编译([\#225](https://github.com/ros2/rmw_connextdds/issues/225))

- 添加变量 `RMW_CONNEXT_USER_TOPICS_PUBLISH_MODE` 并折旧 `RMW_CONNEXT_USE_DEFAULT_PUBLISH_MODE` ([\#224](https://github.com/ros2/rmw_connextdds/issues/224))

- 将 Connext 从 7.3. 0 更新到 7. 7. 0 , 默认禁用监视库, 并使用同步发布模式([\#219](https://github.com/ros2/rmw_connextdds/issues/219))

- 启用属性 `dds.ros.demangle_topic_and_type_names` 宣布解题名为主题别名( Q)[\#221](https://github.com/ros2/rmw_connextdds/issues/221))

- 启用内容过滤标记( E)[\#223](https://github.com/ros2/rmw_connextdds/issues/223))

- 删除已贬值的担保属性并使用新的属性( E)[\#217](https://github.com/ros2/rmw_connextdds/issues/217))

- 删除带有 enum 的切换器的默认值以启用编译器警告( E)[\#216](https://github.com/ros2/rmw_connextdds/issues/216))

- 替换 `DDS_ContentFilter_register_filter` 与 `DDS_DomainParticipant_register_contentfilterI` ([\#215](https://github.com/ros2/rmw_connextdds/issues/215))

- 删除多余 `buildtool_export_depend` ([\#210](https://github.com/ros2/rmw_connextdds/issues/210))

- 添加: 获取客户端、 服务器信息( E)[\#154](https://github.com/ros2/rmw_connextdds/issues/154))

- \[rmw_connextds_common\]:删除 \<member_of_group\>rosidl_interface\_ packages (中文(简体) ).[\#202](https://github.com/ros2/rmw_connextdds/issues/202))

- 正确计算序列化密钥的大小( E) :[\#200](https://github.com/ros2/rmw_connextdds/issues/200))

- 修补 cmake 折旧( E)[\#198](https://github.com/ros2/rmw_connextdds/issues/198))

- 固定序列化最小样本大小回调([\#196](https://github.com/ros2/rmw_connextdds/issues/196))

- 删除警告( R)[\#187](https://github.com/ros2/rmw_connextdds/issues/187))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,巴瑞·许,克里斯·拉兰谢特,弗朗西斯科·加莱戈·萨利多,雅诺施·麦克豪林斯基,李,明珠,藤田友雅,增能\[bot\],苔藓80

<span id="rmw-connextddsmicro"></span>

## [rmw_connextddsmicro](https://github.com/ros2/rmw_connextdds/tree/lyrical/rmw_connextddsmicro/CHANGELOG.rst)

- fix: MSVC 2022上的固定编译([\#225](https://github.com/ros2/rmw_connextdds/issues/225))

- 删除带有 enum 的切换器的默认值以启用编译器警告( E)[\#216](https://github.com/ros2/rmw_connextdds/issues/216))

- 删除多余 `buildtool_export_depend` ([\#210](https://github.com/ros2/rmw_connextdds/issues/210))

- 添加: 获取客户端、 服务器信息( E)[\#154](https://github.com/ros2/rmw_connextdds/issues/154))

- 修补 cmake 折旧( E)[\#198](https://github.com/ros2/rmw_connextdds/issues/198))

- 贡献者:雅诺施·麦克豪温斯基,李,明珠,藤田丰也,苔丝菲特80

<span id="rmw-cyclonedds-cpp"></span>

## [rmw_cyclonedds_cpp](https://github.com/ros2/rmw_cyclonedds/tree/lyrical/rmw_cyclonedds_cpp/CHANGELOG.rst)

- 释放中未使用的变量警告构建( S)[\#580](https://github.com/ros2/rmw_cyclonedds/issues/580))

- 添加密钥支持并更新气旋 DDS 兼容性( C)[\#575](https://github.com/ros2/rmw_cyclonedds/issues/575))

- 明确禁用内容过滤支持( E)[\#574](https://github.com/ros2/rmw_cyclonedds/issues/574))

- 添加跟踪点到 `rmw_take_loan_int` ([\#566](https://github.com/ros2/rmw_cyclonedds/issues/566))

- 纠正警告 `may be used uninitialized` ([\#573](https://github.com/ros2/rmw_cyclonedds/issues/573))

- 改进信件类型支持性能( M)[\#562](https://github.com/ros2/rmw_cyclonedds/issues/562))

- 优化序列化性能 `dynamic_cast` 使用并用模板取代虚拟功能([\#553](https://github.com/ros2/rmw_cyclonedds/issues/553))

- 删除触发适当警告的默认值( R)[\#549](https://github.com/ros2/rmw_cyclonedds/issues/549))

- 添加 : 获取客户端、 服务器信息( E)[\#499](https://github.com/ros2/rmw_cyclonedds/issues/499))

- 请不要在 rmw propl 类型支持列表中包含 rosidl_typesupport\_%c, cpp} ([\#544](https://github.com/ros2/rmw_cyclonedds/issues/544))

- 更新 CMake 要求 (Y)[\#539](https://github.com/ros2/rmw_cyclonedds/issues/539))

- 撰稿人:布兰登·西蒙西奇,克里斯托弗·贝达德,雅诺施·麦克豪林斯基,李,明珠,奥伦·贝尔博士,谢恩·洛雷茨,藤田富茂亚,爱波阿松,苔藓80

<span id="rmw-dds-common"></span>

## [rmw_dds_common](https://github.com/ros2/rmw_dds_common/tree/lyrical/rmw_dds_common/CHANGELOG.rst)

- 如果没有出版商发现,请提供最佳的QoS供订阅。 ([\#84](https://github.com/ros2/rmw_dds_common/issues/84))

- 添加 get_clients_info_by_service 和 get_servers_info_by_service;引入服务实体Info,在图缓存中处理服务类型的散列([\#82](https://github.com/ros2/rmw_dds_common/issues/82))

- 删除不包含散列类型的已贬值的图形缓存方法( E)[\#83](https://github.com/ros2/rmw_dds_common/issues/83))

- 更新 cmake 要求 (% 1)[\#80](https://github.com/ros2/rmw_dds_common/issues/80))

- 取消已贬值的安保设施([\#79](https://github.com/ros2/rmw_dds_common/issues/79))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯托弗·贝达德、李、明珠、藤田友也、苔丝菲特80

<span id="rmw-fastrtps-cpp"></span>

## [rmw_fastrtps_cpp](https://github.com/ros2/rmw_fastrtps/tree/lyrical/rmw_fastrtps_cpp/CHANGELOG.rst)

- 清除 rosidl 的日志: 缓冲路径( )[\#886](https://github.com/ros2/rmw_fastrtps//issues/886)) ([\#887](https://github.com/ros2/rmw_fastrtps//issues/887))

- 更改缓冲感BUFBE: - \> bufbe. (后端端口) [\#880](https://github.com/ros2/rmw_fastrtps//issues/880)) ([\#884](https://github.com/ros2/rmw_fastrtps//issues/884))

- 在访问密钥时修补 UB (F)[\#879](https://github.com/ros2/rmw_fastrtps//issues/879)) ([\#882](https://github.com/ros2/rmw_fastrtps//issues/882))

- 在编译时删除警告 `lcang` ([\#876](https://github.com/ros2/rmw_fastrtps/issues/876))

- 添加对 rosidl 的支持: Buffer-aware per-endpoint pub/ sub ()[\#867](https://github.com/ros2/rmw_fastrtps/issues/867))

- 使用新的聚合rosidl目标,而不是 \_TARGETS([\#870](https://github.com/ros2/rmw_fastrtps/issues/870))

- 启用内容过滤标记( E)[\#869](https://github.com/ros2/rmw_fastrtps/issues/869))

- 修补: 删除超浮雕建材工具_export_dependent. ()[\#852](https://github.com/ros2/rmw_fastrtps/issues/852))

- 添加 : 获取客户端、 服务器信息( E)[\#771](https://github.com/ros2/rmw_fastrtps/issues/771))

- 请不要在 rmw propl 类型支持列表中包含 rosidl_typesupport\_%c, cpp} ([\#843](https://github.com/ros2/rmw_fastrtps/issues/843))

- 固定cmake 折旧([\#831](https://github.com/ros2/rmw_fastrtps/issues/831))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,亚历克西斯·佐吉亚斯,巴里·徐,CY陈,克里斯托弗·贝达德,李,明珠,藤田友也,增能\[bot\],苔藓80

<span id="rmw-fastrtps-dynamic-cpp"></span>

## [rmw_fastrtps_dynamic_cpp](https://github.com/ros2/rmw_fastrtps/tree/lyrical/rmw_fastrtps_dynamic_cpp/CHANGELOG.rst)

- 在访问密钥时修补 UB (F)[\#879](https://github.com/ros2/rmw_fastrtps//issues/879)) ([\#882](https://github.com/ros2/rmw_fastrtps//issues/882))

- 添加对 rosidl 的支持: Buffer-aware per-endpoint pub/ sub ()[\#867](https://github.com/ros2/rmw_fastrtps/issues/867))

- 使用新的聚合rosidl目标,而不是 \_TARGETS([\#870](https://github.com/ros2/rmw_fastrtps/issues/870))

- 修补: 删除超浮雕建材工具_export_dependent. ()[\#852](https://github.com/ros2/rmw_fastrtps/issues/852))

- 添加 : 获取客户端、 服务器信息( E)[\#771](https://github.com/ros2/rmw_fastrtps/issues/771))

- 请不要在 rmw propl 类型支持列表中包含 rosidl_typesupport\_%c, cpp} ([\#843](https://github.com/ros2/rmw_fastrtps/issues/843))

- 调整序列大小前检查剩余大小( E)[\#827](https://github.com/ros2/rmw_fastrtps/issues/827))

- 固定cmake 折旧([\#831](https://github.com/ros2/rmw_fastrtps/issues/831))

- 贡献者:亚历克西斯·措吉亚斯,CY陈,克里斯托弗·贝达德,李,米格尔公司,明珠,藤田丰也,增能\[bot\],苔藓80

<span id="rmw-fastrtps-shared-cpp"></span>

## [rmw_fastrtps_shared_cpp](https://github.com/ros2/rmw_fastrtps/tree/lyrical/rmw_fastrtps_shared_cpp/CHANGELOG.rst)

- 更改缓冲感BUFBE: - \> bufbe. (后端端口) [\#880](https://github.com/ros2/rmw_fastrtps//issues/880)) ([\#884](https://github.com/ros2/rmw_fastrtps//issues/884))

- 功绩: 设置收藏标题元素\_ 旗子 Try ConstructFail Action:: DISCARD 而不是 0 ()[\#875](https://github.com/ros2/rmw_fastrtps/issues/875))

- 添加对 rosidl 的支持: Buffer-aware per-endpoint pub/ sub ()[\#867](https://github.com/ros2/rmw_fastrtps/issues/867))

- 添加的 rmw\_ 采取跟踪点, 因为它不会被触发以获得成功 ([\#871](https://github.com/ros2/rmw_fastrtps/issues/871))

- 添加到借入的跟踪点( E)[\#868](https://github.com/ros2/rmw_fastrtps/issues/868))

- 修补: 删除超浮雕建材工具_export_dependent. ()[\#852](https://github.com/ros2/rmw_fastrtps/issues/852))

- 添加 : 获取客户端、 服务器信息( E)[\#771](https://github.com/ros2/rmw_fastrtps/issues/771))

- 参考文献 [\#23861](https://github.com/ros2/rmw_fastrtps/issues/23861)在类型对象构建中使用密钥注释( Q)[\#849](https://github.com/ros2/rmw_fastrtps/issues/849))

- 固定cmake 折旧([\#831](https://github.com/ros2/rmw_fastrtps/issues/831))

- 获取 `HistoryQoS` 在有发现时发现([\#829](https://github.com/ros2/rmw_fastrtps/issues/829))

- 请检查access-date=中的日期值 (帮助)[\#823](https://github.com/ros2/rmw_fastrtps/issues/823))

- 撰稿人:CY陈、Dasuke Nishimatsu、李、Mario Domínguez López、Miguel Company、Minju、Oren Bell、Oren Bell 博士、藤田友也、注音\[bot\]、mossfet80

<span id="rmw-implementation"></span>

## [rmw_implementation](https://github.com/ros2/rmw_implementation/tree/lyrical/rmw_implementation/CHANGELOG.rst)

- 添加 rmw_zenoh_cpp 作为构建依赖([\#273](https://github.com/ros2/rmw_implementation/issues/273))

- 更新已贬值的 ament_index_cpp API ([\#272](https://github.com/ros2/rmw_implementation/issues/272))

- 添加 rmw_get_clients_info_by_service, rmw_servers_clients_info_by_service(服务)[\#238](https://github.com/ros2/rmw_implementation/issues/238))

- 固定cmake 折旧([\#267](https://github.com/ros2/rmw_implementation/issues/267))

- 在 rmw priml 类型支持列表中解释 rosidl_typesupport\_%c, cpp} ([\#265](https://github.com/ros2/rmw_implementation/issues/265))

- 固定窗口警告( E)[\#254](https://github.com/ros2/rmw_implementation/issues/254))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯托弗·贝达德、李、明朱、托尼·纳贾尔、莫斯费特80

<span id="rmw-test-fixture"></span>

## [rmw_test_fixture](https://github.com/ros2/ament_cmake_ros/tree/lyrical/rmw_test_fixture/CHANGELOG.rst)

- 从 rmw\_ test_fixture 添加缺少的依赖性到 rmw ()[\#53](https://github.com/ros2/ament_cmake_ros/issues/53))

- 添加 find\_ package 调用([\#50](https://github.com/ros2/ament_cmake_ros/issues/50))

- 固定cmake 折旧([\#47](https://github.com/ros2/ament_cmake_ros/issues/47))

- 贡献者:马特·康迪诺、斯科特·K·洛根、苔丝菲特80

<span id="rmw-test-fixture-implementation"></span>

## [rmw_test_fixture_implementation](https://github.com/ros2/ament_cmake_ros/tree/lyrical/rmw_test_fixture_implementation/CHANGELOG.rst)

- Python 环境中的块信号在 rmw\_ test\_ fixture\_ 执行中重新装入( )[\#64](https://github.com/ros2/ament_cmake_ros/issues/64))

- 添加 `ament_ros_defaults` 目标([\#62](https://github.com/ros2/ament_cmake_ros/issues/62))

- 降低依赖组对测试固定装置的依赖([\#60](https://github.com/ros2/ament_cmake_ros/issues/60))

- 隔离完成后恢复ROS_DOMAIN_ID([\#58](https://github.com/ros2/ament_cmake_ros/issues/58))

- 默认为 c++17, 原因是在 std:: map 上使用了较新的方法 ([\#55](https://github.com/ros2/ament_cmake_ros/issues/55))

- 固定cmake 折旧([\#47](https://github.com/ros2/ament_cmake_ros/issues/47))

- 启动后,重新检查 RMW_IMPEMENTATION([\#46](https://github.com/ros2/ament_cmake_ros/issues/46))

- 在默认 RTW 隔离期间选择随机域ID( E)[\#39](https://github.com/ros2/ament_cmake_ros/issues/39))

- 忽略签名 *之后* 儿童过程已经孕育出来([\#45](https://github.com/ros2/ament_cmake_ros/issues/45))

- 为rmw_test_fixture_执行添加一些烟雾测试([\#42](https://github.com/ros2/ament_cmake_ros/issues/42))

- 明确复制所有环境变量( E)[\#43](https://github.com/ros2/ament_cmake_ros/issues/43))

- 为每个库拆分生成器表达式( E)[\#36](https://github.com/ros2/ament_cmake_ros/issues/36))

- 删除了 clang 警告( R)[\#34](https://github.com/ros2/ament_cmake_ros/issues/34))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,迈克尔·卡尔斯特罗姆,迈克尔·卡罗尔,斯科特·K·洛根,塔尼什克·乔达里,威廉·伍德尔,苔丝费特80

<span id="rmw-zenoh-cpp"></span>

## [rmw_zenoh_cpp](https://github.com/ros2/rmw_zenoh/tree/lyrical/rmw_zenoh_cpp/CHANGELOG.rst)

- Bump Zenoh 到 1. 8.0, 修补 Windows 关闭挂起, 并解决同步问题 。 `undeclare` ([\#964](https://github.com/ros2/rmw_zenoh/issues/964))

- 反向改变,以对抗锈蚀 = 1.75 和 折叠 zenoh 到 1.8.0 ([\#960](https://github.com/ros2/rmw_zenoh/issues/960))

- 在处理事件数据时不同时锁定两个锁,从而防止僵局([\#937](https://github.com/ros2/rmw_zenoh/issues/937))

- 弹出子纳至1.8.0([\#935](https://github.com/ros2/rmw_zenoh/issues/935))

- 明确设定 `false` 用于内容过滤功能[\#938](https://github.com/ros2/rmw_zenoh/issues/938))

- 将最后期限/寿命QoS事件添加到 `rmw_zenoh_cpp` ([\#934](https://github.com/ros2/rmw_zenoh/issues/934))

- 接着 `PackageNotFoundError` 在默认配置 URI 加载时防止崩溃([\#915](https://github.com/ros2/rmw_zenoh/issues/915))

- 分布 `reception_sequence_number` 财务报告和财务报告 `advertise_sequence_number` 特性( E)[\#920](https://github.com/ros2/rmw_zenoh/issues/920))

- 使用 `get_package_share_path` ([\#913](https://github.com/ros2/rmw_zenoh/issues/913))

- 处理尚未解决的 TODO 项目([\#896](https://github.com/ros2/rmw_zenoh/issues/896))

- 展 览 Zenoh 届会[\#865](https://github.com/ros2/rmw_zenoh/issues/865))

- 用错误的路径变量修正配置加载( R)[\#898](https://github.com/ros2/rmw_zenoh/issues/898))

- 修复构建二进制工作流程( S)[\#895](https://github.com/ros2/rmw_zenoh/issues/895))

- 修改会话中结束的行打开错误消息( S)[\#888](https://github.com/ros2/rmw_zenoh/issues/888))

- 更新已贬值 `ament_index_cpp` API (英语:[\#879](https://github.com/ros2/rmw_zenoh/issues/879))

- 删除 `default` 从 enum 切换到允许编译器警告( E)[\#871](https://github.com/ros2/rmw_zenoh/issues/871))

- 使用共享的 SHM 运输供应商, 而不是创建新实例( U)[\#857](https://github.com/ros2/rmw_zenoh/issues/857))

- 弹跳 `zenoh` 改为1.7.1 (中文(简体) ).[\#870](https://github.com/ros2/rmw_zenoh/issues/870))

- 添加 rmw_get_clients_info_by_service 和 rmw_get_servers_info_by_services (中文(简体) ).[\#679](https://github.com/ros2/rmw_zenoh/issues/679))

- 修复 REP url 地点([\#858](https://github.com/ros2/rmw_zenoh/issues/858))

- 隔离完成后恢复 ZENOH_CONFIG_OVERIDE([\#855](https://github.com/ros2/rmw_zenoh/issues/855))

- 在“触发”中修正打字符([\#844](https://github.com/ros2/rmw_zenoh/issues/844))

- 在 SHM 创建时的日志细节( 比例和门槛大小) ([\#835](https://github.com/ros2/rmw_zenoh/issues/835))

- 将 ZENOH_SHM_ALLOC_SIZE 的默认值改为 48 MiB([\#830](https://github.com/ros2/rmw_zenoh/issues/830))

- 配置: 将查询\_ 默认\_ 超时到 10min ()[\#820](https://github.com/ros2/rmw_zenoh/issues/820))

- 将编译为 clang([\#819](https://github.com/ros2/rmw_zenoh/issues/819))

- feat( logb): 在日志信件中添加上下文信息([\#809](https://github.com/ros2/rmw_zenoh/issues/809))

- 将配置与上游Zenoh对齐。 ([\#785](https://github.com/ros2/rmw_zenoh/issues/785))

- 固定: 与默认分配器一起发布时解决内存漏漏( S)[\#797](https://github.com/ros2/rmw_zenoh/issues/797))

- 传输时的循环序列化缓冲器([\#342](https://github.com/ros2/rmw_zenoh/issues/342))

- refactor:在回答时避免冗余的密钥表达式创建([\#732](https://github.com/ros2/rmw_zenoh/issues/732))

- 请不要在 rmw propl 类型支持列表中包含 rosidl_typesupport\_%c, cpp} ([\#748](https://github.com/ros2/rmw_zenoh/issues/748))

- 定义配置文件中的类型流到流量( T)[\#740](https://github.com/ros2/rmw_zenoh/issues/740))

- 在 C++ API 上共享内存([\#363](https://github.com/ros2/rmw_zenoh/issues/363))

- Bump Zenoh 到 v1.5.0 (中文(简体) ).[\#728](https://github.com/ros2/rmw_zenoh/issues/728))

- rmw_zenoh_cpp:包含 std 的算法: find_if ()[\#723](https://github.com/ros2/rmw_zenoh/issues/723))

- 使用 rfind 来避免服务类型在请求或回复中结束的问题([\#719](https://github.com/ros2/rmw_zenoh/issues/719))

- 删除出版商一侧的额外副本( N)[\#711](https://github.com/ros2/rmw_zenoh/issues/711))

- 避免与可变阴影模糊不清([\#706](https://github.com/ros2/rmw_zenoh/issues/706))

- 只配置与动作相关的服务的超时 `get_result` 到最大值。 ()[\#685](https://github.com/ros2/rmw_zenoh/issues/685))

- 使用 Zenoh Querier 来替换会话. get([\#694](https://github.com/ros2/rmw_zenoh/issues/694))

- 使用数据来避免可能忽略空向量( E)[\#667](https://github.com/ros2/rmw_zenoh/issues/667))

- 弹出Zenoh至1.4.0([\#652](https://github.com/ros2/rmw_zenoh/issues/652))

- fix(comment):纠正QoS不兼容([\#644](https://github.com/ros2/rmw_zenoh/issues/644))

- 固定 rmw\_ take_serialized\_ message 。 ([\#638](https://github.com/ros2/rmw_zenoh/issues/638))

- 更新 CMakeLists.txt (英语).[\#617](https://github.com/ros2/rmw_zenoh/issues/617))

- 贡献者:亚历杭德罗·赫尔南德斯·科尔德罗、亚历杭德罗·埃尔南德斯·科尔德罗、陈英国(共青团)、克里斯·拉朗谢特、克里斯托弗·贝达德、法赛尔·切马丹、菲利普、赫尔韦·奥德伦、扬·韦尔迈特、朱利安·埃诺赫、李、马哈茂德·马祖兹、明朱、尼古拉·巴诺维奇、斯科特·克·洛根、谢恩·洛雷兹、斯凯勒·梅德罗斯、史蒂文·帕尔马、蒂姆·克莱法斯、托莫亚·藤田、亚敦德、尤尤安·袁、约丹布伦德、密利达姆、莫斯费特80、雅敦德、黄哈特尔、奥伊斯坦·斯图雷

<span id="robot-state-publisher"></span>

## [robot_state_publisher](https://github.com/ros/robot_state_publisher/tree/lyrical/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#246](https://github.com/ros/robot_state_publisher/issues/246))

- 改进( E)[\#245](https://github.com/ros/robot_state_publisher/issues/245))

- 更新订阅回调签名( N)[\#241](https://github.com/ros/robot_state_publisher/issues/241))

- 添加函数从一个话题读取描述而非参数( Q) :[\#234](https://github.com/ros/robot_state_publisher/issues/234))

- 删除 tf2\_ ros 警告( R)[\#239](https://github.com/ros/robot_state_publisher/issues/239))

- 固定cmake 折旧([\#232](https://github.com/ros/robot_state_publisher/issues/232))

- 删除 tf2\_ ros 警告( R)[\#238](https://github.com/ros/robot_state_publisher/issues/238))

- 消除对orocos kdl供应商的依赖([\#237](https://github.com/ros/robot_state_publisher/issues/237))

- 已删除几何 2 中的警告 2 ([\#236](https://github.com/ros/robot_state_publisher/issues/236))

- 替换已折旧的 tf2\_ ros 信头([\#235](https://github.com/ros/robot_state_publisher/issues/235))

- 删除已贬值的命令行参数( E)[\#233](https://github.com/ros/robot_state_publisher/issues/233))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、埃默森·克纳普、肯吉·布拉梅尔德(TRACLabs)、莫里斯·亚历山大·普纳万、莫斯费特80

<span id="ros2action"></span>

## [ros2 动作](https://github.com/ros2/ros2cli/tree/lyrical/ros2action/CHANGELOG.rst)

- 修补 `flake8` ([\#1215](https://github.com/ros2/ros2cli/issues/1215))

- 添加超时参数到 `ros2 service call`, `ros2 action send_goal`, `ros2 component`, `ros2 lifecycle`,以及 `ros2 param` ([\#1185](https://github.com/ros2/ros2cli/issues/1185))

- 添加 osrf_pycommon 依赖测试_exec. ()[\#1120](https://github.com/ros2/ros2cli/issues/1120))

- 富士塔托莫亚/ 清除孤立的 ros2daemon([\#1098](https://github.com/ros2/ros2cli/issues/1098))

- 发射测试后恢复环境变量([\#1086](https://github.com/ros2/ros2cli/issues/1086))

- 使用rmw_test_fixture来隔离ros2cli测试([\#1062](https://github.com/ros2/ros2cli/issues/1062))

- 固定设置工具[\#1066](https://github.com/ros2/ros2cli/issues/1066))

- 固定 ros2 动作发送\_ 目标信号处理 。 ([\#1072](https://github.com/ros2/ros2cli/issues/1072))

- Fujitatomoya/ros2动作发送目标超时([\#1067](https://github.com/ros2/ros2cli/issues/1067))

- 确保安装 py.typed 文件([\#1058](https://github.com/ros2/ros2cli/issues/1058))

- 将检查从精确到部分匹配。 ([\#1055](https://github.com/ros2/ros2cli/issues/1055))

- 导出打字信息( E)[\#1041](https://github.com/ros2/ros2cli/issues/1041))

- 将 QoS 方法从 ros2topi.api 移动到 ros2cli.qos ()[\#1053](https://github.com/ros2/ros2cli/issues/1053))

- 从 ros2 动作信息中删除不必要的 " / " 。 ()[\#1049](https://github.com/ros2/ros2cli/issues/1049))

- 将 QoS 选项添加到 ros2service/ ros2 动作回声命令中 。 ()[\#1036](https://github.com/ros2/ros2cli/issues/1036))

- 允许 Zenoh 测试以多播方式运行([\#992](https://github.com/ros2/ros2cli/issues/992))

- 支持 " ros2 动作回声 " ([\#978](https://github.com/ros2/ros2cli/issues/978))

- 校正许可证内容( E)[\#979](https://github.com/ros2/ros2cli/issues/979))

- 贡献者:许巴里,克里斯托弗·贝达德,迈克尔·卡尔斯特罗姆,迈克尔·卡罗尔,斯科特·K·洛根,藤田丰也,苔丝菲特80

<span id="ros2bag"></span>

## [罗斯2袋](https://github.com/ros2/rosbag2/tree/lyrical/ros2bag/CHANGELOG.rst)

- 添加 `--repeat-all-transient-local` 用于自动瞬态本地主题检测的旗帜( C)[\#2391](https://github.com/ros2/rosbag2/issues/2391))

- 重复的瞬间本地主题:记录器、 CLI 和 Python 绑定([\#2387](https://github.com/ros2/rosbag2/issues/2387))

- 使用Rosbag2 Flake8([\#2329](https://github.com/ros2/rosbag2/issues/2329))

- 删除已贬值的参数和选项 `ros2bag` ([\#2328](https://github.com/ros2/rosbag2/issues/2328))

- 按拆分计数执行循环记录( E)`--max-bag-files`) ([\#2218](https://github.com/ros2/rosbag2/issues/2218))

- 改进 `ros2 bag convert` 剪切和添加碎片的性能 `--input-options` ([\#2325](https://github.com/ros2/rosbag2/issues/2325))

- 为记录器添加静态主题特性( E)[\#2319](https://github.com/ros2/rosbag2/issues/2319))

- 添加 `--max-cache-duration` 选项,用于设定时限的快照([\#2289](https://github.com/ros2/rosbag2/issues/2289))

- 添加 `rosbag2_storage_default_plugins` 改为: `exec_depend` 页:1 `ros2bag` ([\#2227](https://github.com/ros2/rosbag2/issues/2227))

- 添加 `input_serialization_format` 财务报告和财务报告 `output_serialization_format` 改为: `RecordOptions`,折旧 `rmw_serialization_format` ([\#2215](https://github.com/ros2/rosbag2/issues/2215))

- 将丢失的信息发布到 " 事件/信息_丢失 " 专题([\#2150](https://github.com/ros2/rosbag2/issues/2150))

- 向 Python 显示更多播放器和记录器 API,并改进信号处理([\#2062](https://github.com/ros2/rosbag2/issues/2062))

- 固定设置工具的折旧( E)[\#2087](https://github.com/ros2/rosbag2/issues/2087))

- 将 Python 播放器和记录器 API 重置为类([\#2047](https://github.com/ros2/rosbag2/issues/2047))

- 上游质量与Apex.AI第2部分相比有所变化([\#1924](https://github.com/ros2/rosbag2/issues/1924))

- 贡献者:克里斯托弗·贝达德,卢克·西,迈克尔·奥尔洛夫,藤田托莫亚,托尼·纳贾尔,摩斯费特80

<span id="ros2cli"></span>

## [罗斯2cli](https://github.com/ros2/ros2cli/tree/lyrical/ros2cli/CHANGELOG.rst)

- 添加 RMW 隔离固定器以允许发现 `rmw_zenoh_cpp` 测试([\#1216](https://github.com/ros2/ros2cli/issues/1216))

- 添加对鱼的支持( E)[\#1211](https://github.com/ros2/ros2cli/issues/1211))

- 修补 `flake8` ([\#1215](https://github.com/ros2/ros2cli/issues/1215))

- 修正未来的片段8回归( Q)[\#1196](https://github.com/ros2/ros2cli//issues/1196))

- 固定对动作图 API 的贬值警告。 ()[\#1188](https://github.com/ros2/ros2cli//issues/1188))

- 启用总是完整( E)[\#1190](https://github.com/ros2/ros2cli//issues/1190))

- 在 ros2cli 命令中添加基于 fzf 的交互式选择( )[\#1151](https://github.com/ros2/ros2cli//issues/1151))

- 检查无效的 ROS 发现配置和如果 ne... () 的打印警告[\#1178](https://github.com/ros2/ros2cli//issues/1178))

- 跳过历史和深度检查 rmw_connexteds 。 ()[\#1064](https://github.com/ros2/ros2cli/issues/1064))

- 删除导入lib 套件( R)[\#1117](https://github.com/ros2/ros2cli/issues/1117))

- 在服务-info动词中添加动词([\#916](https://github.com/ros2/ros2cli//issues/916))

- 在 ros2cli 中修复空 ROS_DOMAIN_ID 的处理([\#1112](https://github.com/ros2/ros2cli//issues/1112))

- 固定 : 同时抓取超时错误( T)[\#1092](https://github.com/ros2/ros2cli/issues/1092))

- \[ros2 doctor\] 添加动作报告([\#1076](https://github.com/ros2/ros2cli/issues/1076))

- 使用rmw_test_fixture来隔离ros2cli测试([\#1062](https://github.com/ros2/ros2cli/issues/1062))

- 固定设置工具[\#1066](https://github.com/ros2/ros2cli/issues/1066))

- 添加类似于主题报告的服务报告( E)[\#1059](https://github.com/ros2/ros2cli/issues/1059))

- 确保安装 py.typed 文件([\#1058](https://github.com/ros2/ros2cli/issues/1058))

- 导出打字信息( E)[\#1041](https://github.com/ros2/ros2cli/issues/1041))

- 将 QoS 方法从 ros2topi.api 移动到 ros2cli.qos ()[\#1053](https://github.com/ros2/ros2cli/issues/1053))

- 在测试_ros2cli_daemon中显示历史QoS([\#1040](https://github.com/ros2/ros2cli/issues/1040))

- 从 ros2cli 中删除添加\_ 子parsers. ()[\#1032](https://github.com/ros2/ros2cli/issues/1032))

- 允许 Zenoh 测试以多播方式运行([\#992](https://github.com/ros2/ros2cli/issues/992))

- 贡献者:克里斯托弗·贝达德,大卫·V·卢!!!,卡朱-布班贾,李,马里奥·多明格斯·洛佩斯,迈克尔·卡尔斯特罗姆,迈克尔·卡罗尔,明朱,SPeak,斯科特·K·洛根,藤田富友亚,托尼·纳杰尔,雄安·袁,苔藓80

<span id="ros2cli-common-extensions"></span>

## [ros2cli_common_extensions](https://github.com/ros2/ros2cli_common_extensions/tree/lyrical/ros2cli_common_extensions/CHANGELOG.rst)

- 在软件包.xml中添加对 ros2plugin 的依赖( )[\#13](https://github.com/ros2/ros2cli_common_extensions/issues/13))

- 更新 CMakeLists.txt (英语).[\#11](https://github.com/ros2/ros2cli_common_extensions/issues/11))

- 贡献者:莫里斯·亚历山大·普尔纳万,摩斯费特80

<span id="ros2cli-test-interfaces"></span>

## [ros2cli_test_interfaces](https://github.com/ros2/ros2cli/tree/lyrical/ros2cli_test_interfaces/CHANGELOG.rst)

- 固定cmake 折旧([\#1082](https://github.com/ros2/ros2cli/issues/1082))

- 贡献者:苔藓80

<span id="ros2component"></span>

## [ros2 组件](https://github.com/ros2/ros2cli/tree/lyrical/ros2component/CHANGELOG.rst)

- 修补 `flake8` ([\#1215](https://github.com/ros2/ros2cli/issues/1215))

- 添加超时参数到 `ros2 service call`, `ros2 action send_goal`, `ros2 component`, `ros2 lifecycle`,以及 `ros2 param` ([\#1185](https://github.com/ros2/ros2cli/issues/1185))

- 修正未来的片段8回归( Q)[\#1196](https://github.com/ros2/ros2cli//issues/1196))

- 固定设置工具[\#1066](https://github.com/ros2/ros2cli/issues/1066))

- 确保安装 py.typed 文件([\#1058](https://github.com/ros2/ros2cli/issues/1058))

- 导出打字信息( E)[\#1041](https://github.com/ros2/ros2cli/issues/1041))

- 贡献者:克里斯托弗·贝达德,迈克尔·卡尔斯特罗姆,藤田丰也,苔藓Fet80

<span id="ros2doctor"></span>

## [ros2 医生](https://github.com/ros2/ros2cli/tree/lyrical/ros2doctor/CHANGELOG.rst)

- 修正未来的片段8回归( Q)[\#1196](https://github.com/ros2/ros2cli//issues/1196))

- 删除导入lib 套件( R)[\#1117](https://github.com/ros2/ros2cli/issues/1117))

- Harden ros2博士系统呼叫. (中文(简体) ).[\#1118](https://github.com/ros2/ros2cli/issues/1118))

- 本地分析软件包时添加错误处理( E)[\#1108](https://github.com/ros2/ros2cli//issues/1108))

- 富士塔托莫亚/ 清除孤立的 ros2daemon([\#1098](https://github.com/ros2/ros2cli/issues/1098))

- \[罗斯2博士\]环境报告[\#1045](https://github.com/ros2/ros2cli/issues/1045))

- 发射测试后恢复环境变量([\#1086](https://github.com/ros2/ros2cli/issues/1086))

- 为 ros2 医生 – report 添加警告通知 。 ([\#1079](https://github.com/ros2/ros2cli/issues/1079))

- \[ros2 doctor\] 添加动作报告([\#1076](https://github.com/ros2/ros2cli/issues/1076))

- 使用rmw_test_fixture来隔离ros2cli测试([\#1062](https://github.com/ros2/ros2cli/issues/1062))

- 固定设置工具[\#1066](https://github.com/ros2/ros2cli/issues/1066))

- 添加类似于主题报告的服务报告( E)[\#1059](https://github.com/ros2/ros2cli/issues/1059))

- 确保安装 py.typed 文件([\#1058](https://github.com/ros2/ros2cli/issues/1058))

- 导出打字信息( E)[\#1041](https://github.com/ros2/ros2cli/issues/1041))

- 当旗帜为空时修复字符串界面标记 。 ()[\#1026](https://github.com/ros2/ros2cli/issues/1026))

- 允许 Zenoh 测试以多播方式运行([\#992](https://github.com/ros2/ros2cli/issues/992))

- 跳过 Zenoh 上的 QoS 兼容性测试( S)[\#985](https://github.com/ros2/ros2cli/issues/985))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,克里斯·拉朗谢特,克里斯托弗·贝达德,迈克尔·卡尔斯特罗姆,迈克尔·卡罗尔,斯科特·K·洛根,藤田托莫亚,微型-1235,苔藓80

<span id="ros2interface"></span>

## [ros2 接口](https://github.com/ros2/ros2cli/tree/lyrical/ros2interface/CHANGELOG.rst)

- 在 ros2cli 命令中添加基于 fzf 的交互式选择( )[\#1151](https://github.com/ros2/ros2cli//issues/1151))

- 固定设置工具[\#1066](https://github.com/ros2/ros2cli/issues/1066))

- 确保安装 py.typed 文件([\#1058](https://github.com/ros2/ros2cli/issues/1058))

- 导出打字信息( E)[\#1041](https://github.com/ros2/ros2cli/issues/1041))

- 贡献者:克里斯托弗·贝达德、迈克尔·卡尔斯特罗姆、托尼·纳贾尔、莫斯费特80

<span id="ros2launch"></span>

## [ros2 发射](https://github.com/ros2/launch_ros/tree/lyrical/ros2launch/CHANGELOG.rst)

- 正确的打字符( R)[\#524](https://github.com/ros2/launch_ros//issues/524))

- 固定设置工具[\#475](https://github.com/ros2/launch_ros/issues/475))

- 用户控制日志文件基名, 以 ros2 发布([\#461](https://github.com/ros2/launch_ros/issues/461))合著:凯瑟琳·斯科特 \<[katherineAScott@gmail.com](mailto:katherineAScott%40gmail.com)\>

- 贡献者:奥古斯特·拉兰德、塔尼什克·乔达里、苔藓80

<span id="ros2lifecycle"></span>

## [旋转二寿命周期](https://github.com/ros2/ros2cli/tree/lyrical/ros2lifecycle/CHANGELOG.rst)

- 添加超时参数到 `ros2 service call`, `ros2 action send_goal`, `ros2 component`, `ros2 lifecycle`,以及 `ros2 param` ([\#1185](https://github.com/ros2/ros2cli/issues/1185))

- ros2 界面输出每个节点的内容 。 ()[\#1163](https://github.com/ros2/ros2cli//issues/1163))

- 富士塔托莫亚/ 清除孤立的 ros2daemon([\#1098](https://github.com/ros2/ros2cli/issues/1098))

- 发射测试后恢复环境变量([\#1086](https://github.com/ros2/ros2cli/issues/1086))

- 使用rmw_test_fixture来隔离ros2cli测试([\#1062](https://github.com/ros2/ros2cli/issues/1062))

- 固定设置工具[\#1066](https://github.com/ros2/ros2cli/issues/1066))

- 确保安装 py.typed 文件([\#1058](https://github.com/ros2/ros2cli/issues/1058))

- 将检查从精确到部分匹配。 ([\#1055](https://github.com/ros2/ros2cli/issues/1055))

- 导出打字信息( E)[\#1041](https://github.com/ros2/ros2cli/issues/1041))

- 允许 Zenoh 测试以多播方式运行([\#992](https://github.com/ros2/ros2cli/issues/992))

- 贡献者:克里斯托弗·贝达德,迈克尔·卡尔斯特罗姆,迈克尔·卡罗尔,斯科特·K·洛根,藤田友也,苔丝菲特80

<span id="ros2lifecycle-test-fixtures"></span>

## [ros2lifecycle_test_fixtures](https://github.com/ros2/ros2cli/tree/lyrical/ros2lifecycle_test_fixtures/CHANGELOG.rst)

- 固定cmake 折旧([\#1082](https://github.com/ros2/ros2cli/issues/1082))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#973](https://github.com/ros2/ros2cli/issues/973))

- 贡献者:谢恩·洛雷茨、苔藓80

<span id="ros2multicast"></span>

## [ros2 倍数显示器](https://github.com/ros2/ros2cli/tree/lyrical/ros2multicast/CHANGELOG.rst)

- 固定设置工具[\#1066](https://github.com/ros2/ros2cli/issues/1066))

- 确保安装 py.typed 文件([\#1058](https://github.com/ros2/ros2cli/issues/1058))

- 导出打字信息( E)[\#1041](https://github.com/ros2/ros2cli/issues/1041))

- 贡献者:克里斯托弗·贝达德、迈克尔·卡尔斯特罗姆、苔藓80

<span id="ros2node"></span>

## [ros2 节点](https://github.com/ros2/ros2cli/tree/lyrical/ros2node/CHANGELOG.rst)

- 在 ros2cli 命令中添加基于 fzf 的交互式选择( )[\#1151](https://github.com/ros2/ros2cli//issues/1151))

- 富士塔托莫亚/ 清除孤立的 ros2daemon([\#1098](https://github.com/ros2/ros2cli/issues/1098))

- 发射测试后恢复环境变量([\#1086](https://github.com/ros2/ros2cli/issues/1086))

- 使用rmw_test_fixture来隔离ros2cli测试([\#1062](https://github.com/ros2/ros2cli/issues/1062))

- 固定设置工具[\#1066](https://github.com/ros2/ros2cli/issues/1066))

- 确保安装 py.typed 文件([\#1058](https://github.com/ros2/ros2cli/issues/1058))

- 导出打字信息( E)[\#1041](https://github.com/ros2/ros2cli/issues/1041))

- 允许 Zenoh 测试以多播方式运行([\#992](https://github.com/ros2/ros2cli/issues/992))

- 贡献者:克里斯托弗·贝达德,迈克尔·卡尔斯特罗姆,迈克尔·卡罗尔,斯科特·K·洛根,藤田丰也,托尼·纳贾尔,苔丝菲特80

<span id="ros2param"></span>

## [ros2 参数](https://github.com/ros2/ros2cli/tree/lyrical/ros2param/CHANGELOG.rst)

- 添加超时参数到 `ros2 service call`, `ros2 action send_goal`, `ros2 component`, `ros2 lifecycle`,以及 `ros2 param` ([\#1185](https://github.com/ros2/ros2cli/issues/1185))

- ros2 param set /node_name \<param1 值1 parm2 值2 \> 支持 。 ([\#1204](https://github.com/ros2/ros2cli//issues/1204))

- ros2 param 获得/node_name \<param1 parm2 param3... \> 支持 。 ([\#1203](https://github.com/ros2/ros2cli//issues/1203))

- 将每个节点的超时选项添加到 ros2 参数列表([\#1170](https://github.com/ros2/ros2cli//issues/1170))

- 修补巴什补全( E)[\#1182](https://github.com/ros2/ros2cli//issues/1182))

- 在 ros2cli 命令中添加基于 fzf 的交互式选择( )[\#1151](https://github.com/ros2/ros2cli//issues/1151))

- 在所有节点上支持“ros2 param get \<parameter\>”。[\#1174](https://github.com/ros2/ros2cli//issues/1174))

- 固定参数Name 完成器. ()[\#1172](https://github.com/ros2/ros2cli//issues/1172))

- 每次收到时输出节点参数([\#1162](https://github.com/ros2/ros2cli//issues/1162))

- 跳过测试\_ 动词\_ load\_ wildcard for rmw_connexts. ([\#1150](https://github.com/ros2/ros2cli/issues/1150))

- 富士塔托莫亚/ 清除孤立的 ros2daemon([\#1098](https://github.com/ros2/ros2cli/issues/1098))

- 发射测试后恢复环境变量([\#1086](https://github.com/ros2/ros2cli/issues/1086))

- 使用rmw_test_fixture来隔离ros2cli测试([\#1062](https://github.com/ros2/ros2cli/issues/1062))

- 固定设置工具[\#1066](https://github.com/ros2/ros2cli/issues/1066))

- 确保安装 py.typed 文件([\#1058](https://github.com/ros2/ros2cli/issues/1058))

- 将检查从精确到部分匹配。 ([\#1055](https://github.com/ros2/ros2cli/issues/1055))

- 导出打字信息( E)[\#1041](https://github.com/ros2/ros2cli/issues/1041))

- 修补打字错误。 ([\#1035](https://github.com/ros2/ros2cli/issues/1035))

- 抓取连接RefusedError, 以便它能够返回 DirectNode 。 ()[\#1014](https://github.com/ros2/ros2cli/issues/1014))

- 避免 TypeError 例外的测试失败。 ()[\#1016](https://github.com/ros2/ros2cli/issues/1016))

- 从 Yaml 文件修正加载参数行为( E)[\#864](https://github.com/ros2/ros2cli/issues/864))

- 允许 Zenoh 测试以多播方式运行([\#992](https://github.com/ros2/ros2cli/issues/992))

- 贡献者:许巴里、克里斯托弗·贝达德、迈克尔·卡尔斯特罗姆、迈克尔·卡罗尔、斯科特·K·洛根、大加·阿拉伊、藤田丰也、托尼·纳贾尔、苔丝菲特80

<span id="ros2pkg"></span>

## [ros2pkg (单位:千米)](https://github.com/ros2/ros2cli/tree/lyrical/ros2pkg/CHANGELOG.rst)

- 删除“ rclrs” 重复依赖([\#1197](https://github.com/ros2/ros2cli//issues/1197))

- 修正未来的片段8回归( Q)[\#1196](https://github.com/ros2/ros2cli//issues/1196))

- 添加原生 ROS2 Rust 套件创建能力([\#1107](https://github.com/ros2/ros2cli/issues/1107))

- 删除导入lib 套件( R)[\#1117](https://github.com/ros2/ros2cli/issues/1117))

- 添加 mypy( 单数) ([\#1109](https://github.com/ros2/ros2cli//issues/1109))

- 固定cmake 折旧([\#1082](https://github.com/ros2/ros2cli/issues/1082))

- 固定设置工具[\#1066](https://github.com/ros2/ros2cli/issues/1066))

- 确保安装 py.typed 文件([\#1058](https://github.com/ros2/ros2cli/issues/1058))

- 安装中减少锅炉板(供库使用的TARGETS)[\#1056](https://github.com/ros2/ros2cli/issues/1056))

- 导出打字信息( E)[\#1041](https://github.com/ros2/ros2cli/issues/1041))

- 使用现代 C++17 语法. ()[\#982](https://github.com/ros2/ros2cli/issues/982))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#973](https://github.com/ros2/ros2cli/issues/973))

- 尝试使用 git 环球用户. name 来维护者名( U)[\#968](https://github.com/ros2/ros2cli/issues/968))

- 更新最小 CMake 版本 CMakeLists.txt.em ()[\#969](https://github.com/ros2/ros2cli/issues/969))

- 贡献者:巴特洛米耶·施蒂克岑,克里斯托弗·贝达德,拉里·格泽利乌斯,迈克尔·卡尔斯特罗姆,帕蒂·帕特尔,塞巴斯蒂安·卡斯特罗,谢恩·洛雷茨,谢努尔,西尔维奥·特拉韦萨罗,苔丝费特80

<span id="ros2plugin"></span>

## [ros2plugin 缩写](https://github.com/ros/pluginlib/tree/lyrical/ros2plugin/CHANGELOG.rst)

- 执行软件包选项( E)[\#293](https://github.com/ros/pluginlib/issues/293))

- 无法解析插件时改进记录( E)[\#285](https://github.com/ros/pluginlib/issues/285))

- 添加 ros2plugin ((% 1)[\#165](https://github.com/ros/pluginlib/issues/165))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、杰雷米·德雷、小型-1235

<span id="ros2run"></span>

## [罗斯2运行](https://github.com/ros2/ros2cli/tree/lyrical/ros2run/CHANGELOG.rst)

- 修补巴什补全( E)[\#1182](https://github.com/ros2/ros2cli//issues/1182))

- 在 ros2cli 命令中添加基于 fzf 的交互式选择( )[\#1151](https://github.com/ros2/ros2cli//issues/1151))

- 固定设置工具[\#1066](https://github.com/ros2/ros2cli/issues/1066))

- 确保安装 py.typed 文件([\#1058](https://github.com/ros2/ros2cli/issues/1058))

- 导出打字信息( E)[\#1041](https://github.com/ros2/ros2cli/issues/1041))

- 将信号处理器 SIGIN/SIGTERM 添加到 ros2run ([\#899](https://github.com/ros2/ros2cli/issues/899))

- 贡献者:克里斯托弗·贝达德,迈克尔·卡尔斯特罗姆,藤田丰也,托尼·纳杰尔,苔丝菲特80

<span id="ros2service"></span>

## [罗斯2服务](https://github.com/ros2/ros2cli/tree/lyrical/ros2service/CHANGELOG.rst)

- 添加超时参数到 `ros2 service call`, `ros2 action send_goal`, `ros2 component`, `ros2 lifecycle`,以及 `ros2 param` ([\#1185](https://github.com/ros2/ros2cli/issues/1185))

- 修补巴什补全( E)[\#1182](https://github.com/ros2/ros2cli//issues/1182))

- 在 ros2cli 命令中添加基于 fzf 的交互式选择( )[\#1151](https://github.com/ros2/ros2cli//issues/1151))

- 在服务-info动词中添加动词([\#916](https://github.com/ros2/ros2cli//issues/916))

- 富士塔托莫亚/ 清除孤立的 ros2daemon([\#1098](https://github.com/ros2/ros2cli/issues/1098))

- 发射测试后恢复环境变量([\#1086](https://github.com/ros2/ros2cli/issues/1086))

- 使用rmw_test_fixture来隔离ros2cli测试([\#1062](https://github.com/ros2/ros2cli/issues/1062))

- 固定设置工具[\#1066](https://github.com/ros2/ros2cli/issues/1066))

- 确保安装 py.typed 文件([\#1058](https://github.com/ros2/ros2cli/issues/1058))

- 将检查从精确到部分匹配。 ([\#1055](https://github.com/ros2/ros2cli/issues/1055))

- 导出打字信息( E)[\#1041](https://github.com/ros2/ros2cli/issues/1041))

- 将 QoS 方法从 ros2topi.api 移动到 ros2cli.qos ()[\#1053](https://github.com/ros2/ros2cli/issues/1053))

- 将 QoS 选项添加到 ros2service/ ros2 动作回声命令中 。 ()[\#1036](https://github.com/ros2/ros2cli/issues/1036))

- 使用 `get_service` 输入 `ros2service call` ([\#994](https://github.com/ros2/ros2cli/issues/994))

- 允许 Zenoh 测试以多播方式运行([\#992](https://github.com/ros2/ros2cli/issues/992))

- 支持 QoS 选项 `ros2 service call` ([\#966](https://github.com/ros2/ros2cli/issues/966))

- 贡献者:克里斯托弗·贝达德,李,迈克尔·卡尔斯特罗姆,迈克尔·卡罗尔,明珠,斯科特·K·洛根,藤田托莫亚,托尼·纳杰尔,摩斯费特80

<span id="ros2test"></span>

## [ros2 测试](https://github.com/ros2/ros_testing/tree/lyrical/ros2test/CHANGELOG.rst)

- 固定设置工具[\#16](https://github.com/ros2/ros_testing/issues/16))

- 贡献者:苔藓80

<span id="ros2topic"></span>

## [ros2 专题](https://github.com/ros2/ros2cli/tree/lyrical/ros2topic/CHANGELOG.rst)

- 改进测试隔离并抑制Connext许可证噪声([\#1225](https://github.com/ros2/ros2cli/issues/1225))

- 在 ros2cli 命令中添加基于 fzf 的交互式选择( )[\#1151](https://github.com/ros2/ros2cli//issues/1151))

- 在屏幕刷新后的 " ros2 专题体重 " 中加上 " 全部/-a " 选项。 ([\#1130](https://github.com/ros2/ros2cli/issues/1130))

- 明确从内部函数返回。 ([\#1128](https://github.com/ros2/ros2cli/issues/1128))

- 支持“ros2专题体重”的多个专题。 ([\#1124](https://github.com/ros2/ros2cli/issues/1124))

- 在屏幕刷新后的 " ros2 专题hz " 中添加 " –all/-a " 选项。 ([\#1122](https://github.com/ros2/ros2cli/issues/1122))

- 富士塔托莫亚/ 清除孤立的 ros2daemon([\#1098](https://github.com/ros2/ros2cli/issues/1098))

- 在测试命令执行之前等待出版商。 ()[\#1094](https://github.com/ros2/ros2cli/issues/1094))

- 允许对一些剩余的ros2专题测试进行测试隔离([\#1087](https://github.com/ros2/ros2cli/issues/1087))

- 发射测试后恢复环境变量([\#1086](https://github.com/ros2/ros2cli/issues/1086))

- 使用rmw_test_fixture来隔离ros2cli测试([\#1062](https://github.com/ros2/ros2cli/issues/1062))

- 固定设置工具[\#1066](https://github.com/ros2/ros2cli/issues/1066))

- 确保安装 py.typed 文件([\#1058](https://github.com/ros2/ros2cli/issues/1058))

- 导出打字信息( E)[\#1041](https://github.com/ros2/ros2cli/issues/1041))

- 将 QoS 方法从 ros2topi.api 移动到 ros2cli.qos ()[\#1053](https://github.com/ros2/ros2cli/issues/1053))

- 自定义用于获取主题原型的补全查找器( C)[\#995](https://github.com/ros2/ros2cli/issues/995))

- 现在已记录并自动关键字( Q)[\#1008](https://github.com/ros2/ros2cli/issues/1008))

- 信息有条件的去序列化 `ros2 topic hz` ([\#1005](https://github.com/ros2/ros2cli/issues/1005))

- 启用 `ros2 topic echo` 包含数组字段的条目([\#996](https://github.com/ros2/ros2cli/issues/996))

- 允许 Zenoh 测试以多播方式运行([\#992](https://github.com/ros2/ros2cli/issues/992))

- 适应Zenoh的测试[\#988](https://github.com/ros2/ros2cli/issues/988))

- 调整主题 hz 和 bw 命令描述( )[\#987](https://github.com/ros2/ros2cli/issues/987))

- 添加对主题 QOS 的支持, 用于 ros2topic bw, 延迟和 hz ()[\#935](https://github.com/ros2/ros2cli/issues/935))

- 开始模拟,从1秒开始测试([\#975](https://github.com/ros2/ros2cli/issues/975))

- 支持 QoS 选项 `ros2 service call` ([\#966](https://github.com/ros2/ros2cli/issues/966))

- 支持 ros2 主题 pub yaml 文件输入([\#925](https://github.com/ros2/ros2cli/issues/925))

- 撰稿人:亚历杭德罗·埃尔南德斯·科尔德罗,安东尼·韦尔特,克里斯托弗·贝达德,法比安·汤姆森,弗洛伦西亚,科斯图布·汗德尔瓦尔,莱安德·斯蒂芬·杜萨,马丁·佩卡,迈克尔·卡尔斯特罗姆,迈克尔·卡罗尔,斯科特·克·洛根,藤田丰友,托尼·纳杰尔,摩斯费特80,诺穆穆

<span id="ros2trace"></span>

## [ros2 跟踪](https://github.com/ros2/ros2_tracing/tree/lyrical/ros2trace/CHANGELOG.rst)

- 忽略 A005 (帮助)[\#237](https://github.com/ros2/ros2_tracing/issues/237))

- 更新跟踪命令的 doc 字符串( Q)[\#213](https://github.com/ros2/ros2_tracing/issues/213))

- 允许创建快照会话( E)[\#195](https://github.com/ros2/ros2_tracing/issues/195))

- 添加对运行时间开始追踪的支持( E)[\#191](https://github.com/ros2/ros2_tracing/issues/191))

- 固定设置工具 折旧( )[\#189](https://github.com/ros2/ros2_tracing/issues/189))

- 微量工具中的神秘度报告的地址打字问题_发射([\#184](https://github.com/ros2/ros2_tracing/issues/184))

- 贡献者:克里斯托弗·贝达德、迈克尔·卡尔斯特伦、什拉万·德瓦、莫斯费特80

<span id="ros-environment"></span>

## [ros_environment](https://github.com/ros/ros_environment/tree/lyrical/CHANGELOG.rst)

- 将默认 ROS_DISTRO 从“ 滚动” 改为“ 语言 ”

- 固定cmake 折旧([\#42](https://github.com/ros/ros_environment/issues/42))

- 移除 CODEOWINERS. (中文(简体) ).[\#40](https://github.com/ros/ros_environment/issues/40))

- 贡献者:克里斯·拉兰谢特、谢恩·洛雷茨、苔藓80

<span id="ros-testing"></span>

## [ros_testing](https://github.com/ros2/ros_testing/tree/lyrical/ros_testing/CHANGELOG.rst)

- 固定cmake 折旧([\#17](https://github.com/ros2/ros_testing/issues/17))

- 贡献者:苔藓80

<span id="rosbag2"></span>

## [rosbag2](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2/CHANGELOG.rst)

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 贡献者:苔藓80

<span id="rosbag2-compression"></span>

## [rosbag2_compression](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_compression/CHANGELOG.rst)

- 在压缩过程中添加对空文件路径的校验( E)[\#2398](https://github.com/ros2/rosbag2/issues/2398))

- 关闭压缩刻录器中修正一个可能的种族条件( Q)[\#2362](https://github.com/ros2/rosbag2/issues/2362))

- 更新 Rosbag2 文件名格式到 `index+name+timestamp` ([\#2265](https://github.com/ros2/rosbag2/issues/2265))

- 按拆分计数执行循环记录( E)`--max-bag-files`) ([\#2218](https://github.com/ros2/rosbag2/issues/2218))

- 使编剧的结束( ) 和开放( ) API 调用( )[\#2229](https://github.com/ros2/rosbag2/issues/2229))

- 通过增加缓存大小来测试地址记录器的片段([\#2203](https://github.com/ros2/rosbag2/issues/2203))

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 添加信件损失统计回调和记录( E)[\#2039](https://github.com/ros2/rosbag2/issues/2039))

- 引入新的 `BaseWriteInterface` 方法( I) `write_messages` 财务报告和财务报告 `write_message` 以提供运行状态、 折旧旧写 API([\#2030](https://github.com/ros2/rosbag2/issues/2030))

- 错误修正 : `ros2 bag convert` 以压缩模式发送信件( E)[\#1975](https://github.com/ros2/rosbag2/issues/1975))

- 贡献者:佐藤大介、丹吉特·本、卢克·西、迈克尔·奥尔洛夫、莫斯费特80

<span id="rosbag2-compression-zstd"></span>

## [rosbag2_compression_zstd](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_compression_zstd/CHANGELOG.rst)

- 替换 `zstd_vendor` 与 `zstd_cmake_module` ([\#2166](https://github.com/ros2/rosbag2/issues/2166))

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,苔藓80

<span id="rosbag2-cpp"></span>

## [rosbag2_cpp](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_cpp/CHANGELOG.rst)

- 删除了 cang 警告( R)[\#2404](https://github.com/ros2/rosbag2/issues/2404))

- 执行 `transient-local topic` 重复 Writer API 和 拆分/ snapshot 集成([\#2386](https://github.com/ros2/rosbag2/issues/2386))

- 添加 `TransientLocalMessagesCache` 财务报告和财务报告 `RecordOptions` 用于重复瞬间-局部主题([\#2385](https://github.com/ros2/rosbag2/issues/2385))

- 使用新的 ROSIDL 聚合 CMake 目标([\#2384](https://github.com/ros2/rosbag2/issues/2384))

- 关闭压缩刻录器中修正一个可能的种族条件( Q)[\#2362](https://github.com/ros2/rosbag2/issues/2362))

- 纠正元数据中不正确的序列化格式( )[\#2372](https://github.com/ros2/rosbag2/issues/2372))

- 更新 Rosbag2 文件名格式到 `index+name+timestamp` ([\#2265](https://github.com/ros2/rosbag2/issues/2265))

- 支持相对包括本地信件定义中的IDL([\#2241](https://github.com/ros2/rosbag2/issues/2241))

- 按拆分计数执行循环记录( E)`--max-bag-files`) ([\#2218](https://github.com/ros2/rosbag2/issues/2218))

- 添加 `--max-cache-duration` 选项,用于设定时限的快照([\#2289](https://github.com/ros2/rosbag2/issues/2289))

- 工作杂乱 `bagsize_split_is_at_least_specified_size` 测试([\#2311](https://github.com/ros2/rosbag2/issues/2311))

- 将Apex.AI的上游小修补纳入其中[\#2240](https://github.com/ros2/rosbag2/issues/2240))

- 更新已贬值的 ament_index_cpp API([\#2268](https://github.com/ros2/rosbag2/issues/2268))

- 使编剧的结束( ) 和开放( ) API 调用( )[\#2229](https://github.com/ros2/rosbag2/issues/2229))

- 在将新信件推向信件缓存时添加无效检查( N)[\#2219](https://github.com/ros2/rosbag2/issues/2219))

- 通过增加缓存大小来测试地址记录器的片段([\#2203](https://github.com/ros2/rosbag2/issues/2203))

- 仅在调试日志中找到信件定义的逻辑推理( E)[\#2183](https://github.com/ros2/rosbag2/issues/2183))

- 改进 rosbag2_cpp 中的错误处理, 并取消检查和例外抛掷( )[\#2127](https://github.com/ros2/rosbag2/issues/2127))

- 在其中添加无指针检查 `Reader` 建筑师和建筑师 `open()` 方法 (单位:千美元)[\#2135](https://github.com/ros2/rosbag2/issues/2135))

- 使用 `rclcpp typesupport helpers` 输入 `rosbag2_cpp` ([\#2017](https://github.com/ros2/rosbag2/issues/2017))

- 修复召回, 不为 MESSAGES\_ LOST 事件调用( Name[\#2105](https://github.com/ros2/rosbag2/issues/2105))

- 改进记录器信件缓存的性能( E)[\#2104](https://github.com/ros2/rosbag2/issues/2104))

- 当包文件持续时间重叠时, 固定重索引持续时间错误( F)[\#2036](https://github.com/ros2/rosbag2/issues/2036))

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 在嵌入子目录中添加搜索信件定义的支持( N)[\#2055](https://github.com/ros2/rosbag2/issues/2055))

- 添加信件损失统计回调和记录( E)[\#2039](https://github.com/ros2/rosbag2/issues/2039))

- 使用缓存确定动作接口内置类型( U)[\#2052](https://github.com/ros2/rosbag2/issues/2052))

- 解决服务/行动信息定义问题([\#2041](https://github.com/ros2/rosbag2/issues/2041))

- 引入新的 `BaseWriteInterface` 方法( I) `write_messages` 财务报告和财务报告 `write_message` 以提供运行状态、 折旧旧写 API([\#2030](https://github.com/ros2/rosbag2/issues/2030))

- 通过避免零星的唤醒和在玩家启动时修正不正确的间隔,改善消息发布时间([\#2025](https://github.com/ros2/rosbag2/issues/2025))

- 上游质量与Apex.AI第2部分相比有所变化([\#1924](https://github.com/ros2/rosbag2/issues/1924))

- 在其中处理串行警告 `TimeControllerClock::wakeup()` ([\#1962](https://github.com/ros2/rosbag2/issues/1962))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,贝里·许,克里斯托弗·贝达德,朱伊·范夫利特,佐藤大介,爱默生·克纳普,亨特·L·艾伦,何塞·法利亚,卢克·西,迈克尔·奥尔洛夫,富士塔,托尼·纳杰尔,尤金·洪,摩斯费特80

<span id="rosbag2-examples-cpp"></span>

## [rosbag2_examples_cpp](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_examples/rosbag2_examples_cpp/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#2384](https://github.com/ros2/rosbag2/issues/2384))

- 更新订阅回调签名( N)[\#2225](https://github.com/ros2/rosbag2/issues/2225))

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 添加压缩袋文件的示例( E)[\#1956](https://github.com/ros2/rosbag2/issues/1956))

- 贡献者:爱默生·克纳普,马克西姆·弗勒里,小型-1235,苔藓80

<span id="rosbag2-examples-py"></span>

## [rosbag2_examples_py](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_examples/rosbag2_examples_py/CHANGELOG.rst)

- 修补设置工具以去除折旧 `tests_require` ([\#2092](https://github.com/ros2/rosbag2/issues/2092))

- 添加压缩袋文件的示例( E)[\#1956](https://github.com/ros2/rosbag2/issues/1956))

- 上游质量与Apex.AI第2部分相比有所变化([\#1924](https://github.com/ros2/rosbag2/issues/1924))

- 添加一个简单的例子, 显示如何将包转换到 csv 文件( Name[\#1974](https://github.com/ros2/rosbag2/issues/1974))

- 贡献者:克里斯托弗·贝达德、马克西姆·弗洛里、迈克尔·奥尔洛夫、莫斯费特80

<span id="rosbag2-interfaces"></span>

## [rosbag2_interfaces](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_interfaces/CHANGELOG.rst)

- 添加基于时间的 Resume 服务支持( E)[\#2357](https://github.com/ros2/rosbag2/issues/2357))

- 允许在录音时暂停/恢复服务呼叫( R)[\#2349](https://github.com/ros2/rosbag2/issues/2349))

- 执行延迟和基于时间的记录器和播放器服务,增加新的包分割模式([\#2330](https://github.com/ros2/rosbag2/issues/2330))

- 将错误返回代码添加到 `~/stop` 服务请求( E)[\#2312](https://github.com/ros2/rosbag2/issues/2312))

- 添加 Record, Stop, Start Discovery, Stop Discovery, 和 IsDiscovery Running 服务给 Recorder( 开始发现, 停止发现, 停止发现, 和 IsDiscovery Running 用于记录器( )[\#2248](https://github.com/ros2/rosbag2/issues/2248))

- 将丢失的信息发布到 " 事件/信息_丢失 " 专题([\#2150](https://github.com/ros2/rosbag2/issues/2150))

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 贡献者:迈克尔·奥尔洛夫、Carlos-apex、mosfet80

<span id="rosbag2-performance-benchmarking"></span>

## [rosbag2_performance_benchmarking](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_performance/rosbag2_performance_benchmarking/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#2384](https://github.com/ros2/rosbag2/issues/2384))

- 删除不必要的依赖性 `yaml_cpp_vendor` ([\#2353](https://github.com/ros2/rosbag2/issues/2353))

- 通过初始化修正警告 `number_of_threads` ([\#2121](https://github.com/ros2/rosbag2/issues/2121))

- 启用 `rosbag2_performance_benchmarking` 要默认构建的软件包( E)[\#2093](https://github.com/ros2/rosbag2/issues/2093))

- 确定业绩基准数据生成和环境变量处理([\#2078](https://github.com/ros2/rosbag2/issues/2078))

- 修复失败 `benchmark_launch` 调用时 `Process.wait()` 两次([\#2076](https://github.com/ros2/rosbag2/issues/2076))

- 纠正错误的结果来自 `prosbag2_performance_benchmarking` 用于高频专题[\#2077](https://github.com/ros2/rosbag2/issues/2077))

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 上游质量与Apex.AI第2部分相比有所变化([\#1924](https://github.com/ros2/rosbag2/issues/1924))

- 贡献者:克里斯·拉兰谢特,克里斯托弗·贝达德,克里斯托瓦尔·阿罗约,爱默生·克纳普,迈克尔·奥尔洛夫,苔丝菲特80

<span id="rosbag2-performance-benchmarking-msgs"></span>

## [rosbag2_performance_benchmarking_msgs](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_performance/rosbag2_performance_benchmarking_msgs/CHANGELOG.rst)

- 启用 `rosbag2_performance_benchmarking` 要默认构建的软件包( E)[\#2093](https://github.com/ros2/rosbag2/issues/2093))

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 贡献者:迈克尔·奥尔洛夫,摩斯费特80

<span id="rosbag2-py"></span>

## [rosbag2_py](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_py/CHANGELOG.rst)

- 添加 `--repeat-all-transient-local` 用于自动瞬态本地主题检测的旗帜( C)[\#2391](https://github.com/ros2/rosbag2/issues/2391))

- 重复的瞬间本地主题:记录器、 CLI 和 Python 绑定([\#2387](https://github.com/ros2/rosbag2/issues/2387))

- 按拆分计数执行循环记录( E)`--max-bag-files`) ([\#2218](https://github.com/ros2/rosbag2/issues/2218))

- 移动到 `build_depend` ([\#2332](https://github.com/ros2/rosbag2/issues/2332))

- 改进 `ros2 bag convert` 剪切和添加碎片的性能 `--input-options` ([\#2325](https://github.com/ros2/rosbag2/issues/2325))

- 为记录器添加静态主题特性( E)[\#2319](https://github.com/ros2/rosbag2/issues/2319))

- 添加 `--max-cache-duration` 选项,用于设定时限的快照([\#2289](https://github.com/ros2/rosbag2/issues/2289))

- 将Apex.AI的上游小修补纳入其中[\#2240](https://github.com/ros2/rosbag2/issues/2240))

- 添加 `input_serialization_format` 财务报告和财务报告 `output_serialization_format` 改为: `RecordOptions`,折旧 `rmw_serialization_format` ([\#2215](https://github.com/ros2/rosbag2/issues/2215))

- 从 deb 或 pixi 使用 pybind11 ()[\#2154](https://github.com/ros2/rosbag2/issues/2154))

- 将丢失的信息发布到 " 事件/信息_丢失 " 专题([\#2150](https://github.com/ros2/rosbag2/issues/2150))

- 确保由记录器在 `rosbag2_py` 测试([\#2132](https://github.com/ros2/rosbag2/issues/2132))

- 以 robag2_py 和 crang 拼接 Env vars 的 CMake 列表附件([\#2116](https://github.com/ros2/rosbag2/issues/2116))

- 为玩家的开始时间和回放时间添加公开的 API( E)[\#2095](https://github.com/ros2/rosbag2/issues/2095))

- 向 Python 显示更多播放器和记录器 API,并改进信号处理([\#2062](https://github.com/ros2/rosbag2/issues/2062))

- 添加 `send_timestamp` 用于读取序列化信件的 Python 接口([\#2061](https://github.com/ros2/rosbag2/issues/2061))

- 将 Python 播放器和记录器 API 重置为类([\#2047](https://github.com/ros2/rosbag2/issues/2047))

- 解决服务/行动信息定义问题([\#2041](https://github.com/ros2/rosbag2/issues/2041))

- 上游质量与Apex.AI第2部分相比有所变化([\#1924](https://github.com/ros2/rosbag2/issues/1924))

- 错误修正 : `ros2 bag convert` 以压缩模式发送信件( E)[\#1975](https://github.com/ros2/rosbag2/issues/1975))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、巴瑞·徐、克里斯托弗·贝达德、丹吉特·本、卢克·西、迈克尔·卡尔斯特罗姆、迈克尔·奥尔洛夫、奥姆·希瓦姆·韦尔马、托尼·纳贾尔

<span id="rosbag2-storage"></span>

## [rosbag2_storage](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_storage/CHANGELOG.rst)

- 添加 `TransientLocalMessagesCache` 财务报告和财务报告 `RecordOptions` 用于重复瞬间-局部主题([\#2385](https://github.com/ros2/rosbag2/issues/2385))

- 执行延迟和基于时间的记录器和播放器服务,增加新的包分割模式([\#2330](https://github.com/ros2/rosbag2/issues/2330))

- 按拆分计数执行循环记录( E)`--max-bag-files`) ([\#2218](https://github.com/ros2/rosbag2/issues/2218))

- 改进 `ros2 bag convert` 剪切和添加碎片的性能 `--input-options` ([\#2325](https://github.com/ros2/rosbag2/issues/2325))

- 添加 `--max-cache-duration` 选项,用于设定时限的快照([\#2289](https://github.com/ros2/rosbag2/issues/2289))

- 掷出 `YAML::Exception` 如果数据类型不匹配,则在转换时([\#2262](https://github.com/ros2/rosbag2/issues/2262))

- 在 YAML 解码器解码器中修复解码器和编码不匹配([\#2277](https://github.com/ros2/rosbag2/issues/2277))

- 将Apex.AI的上游小修补纳入其中[\#2240](https://github.com/ros2/rosbag2/issues/2240))

- 修复内存泄漏 `make_empty_serialized_message()` ([\#2253](https://github.com/ros2/rosbag2/issues/2253))

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 引入新的 `BaseWriteInterface` 方法( I) `write_messages` 财务报告和财务报告 `write_message` 以提供运行状态、 折旧旧写 API([\#2030](https://github.com/ros2/rosbag2/issues/2030))

- 修正未定义的行为 `rosbag2_storage` 财务报告和财务报告 `rosbag2_storage_sqlite3` 软件包( E)[\#1997](https://github.com/ros2/rosbag2/issues/1997))

- 使用 DDS 队列深度进行订阅,作为出版商的最大值( D)[\#1960](https://github.com/ros2/rosbag2/issues/1960))

- 贡献者:卢克·西、迈克尔·奥尔洛夫、藤田托莫亚、托尼·纳贾尔、Carlos-apex、mosfet80

<span id="rosbag2-storage-default-plugins"></span>

## [rosbag2_storage_default_plugins](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_storage_default_plugins/CHANGELOG.rst)

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 贡献者:苔藓80

<span id="rosbag2-storage-mcap"></span>

## [rosbag2_storage_mcap](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_storage_mcap/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#2384](https://github.com/ros2/rosbag2/issues/2384))

- 删除不必要的依赖性 `yaml_cpp_vendor` ([\#2353](https://github.com/ros2/rosbag2/issues/2353))

- 修正 MCAPStorage :: seek( 时间) 当时间戳匹配当前时间时推进( )[\#2157](https://github.com/ros2/rosbag2/issues/2157))

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 引入新的 `BaseWriteInterface` 方法( I) `write_messages` 财务报告和财务报告 `write_message` 以提供运行状态、 折旧旧写 API([\#2030](https://github.com/ros2/rosbag2/issues/2030))

- 更新 `index.ros.org/p/` 链接 `rosbag2_storage_mcap` ([\#2034](https://github.com/ros2/rosbag2/issues/2034))

- 贡献者:克里斯·拉兰谢特、克里斯托弗·贝达德、爱默生·克纳普、迈克尔·奥尔洛夫、莫斯费特80

<span id="rosbag2-storage-sqlite3"></span>

## [rosbag2_storage_sqlite3](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_storage_sqlite3/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#2384](https://github.com/ros2/rosbag2/issues/2384))

- 按拆分计数执行循环记录( E)`--max-bag-files`) ([\#2218](https://github.com/ros2/rosbag2/issues/2218))

- 添加 `--max-cache-duration` 选项,用于设定时限的快照([\#2289](https://github.com/ros2/rosbag2/issues/2289))

- 通过使用参数化的查询来修复脆弱字符串连接( Q)[\#2290](https://github.com/ros2/rosbag2/issues/2290))

- 删除 sqlite3\_ vendor ()[\#2164](https://github.com/ros2/rosbag2/issues/2164))

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 引入新的 `BaseWriteInterface` 方法( I) `write_messages` 财务报告和财务报告 `write_message` 以提供运行状态、 折旧旧写 API([\#2030](https://github.com/ros2/rosbag2/issues/2030))

- 修正未定义的行为 `rosbag2_storage` 财务报告和财务报告 `rosbag2_storage_sqlite3` 软件包( E)[\#1997](https://github.com/ros2/rosbag2/issues/1997))

- 上游质量与Apex.AI第2部分相比有所变化([\#1924](https://github.com/ros2/rosbag2/issues/1924))

- 撰稿人:亚历杭德罗·埃尔南德斯·科尔德罗,克里斯托弗·贝达德,爱默生·克纳普,卢克·西,迈克尔·奥尔洛夫,藤田丰也,苔丝菲特80

<span id="rosbag2-test-common"></span>

## [rosbag2_test_common](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_test_common/CHANGELOG.rst)

- 减少Rosbag2记录器末端到末端测试中的碎片([\#2370](https://github.com/ros2/rosbag2/issues/2370))

- 使用新的 ROSIDL 聚合 CMake 目标([\#2384](https://github.com/ros2/rosbag2/issues/2384))

- 更新 Rosbag2 文件名格式到 `index+name+timestamp` ([\#2265](https://github.com/ros2/rosbag2/issues/2265))

- 更新订阅回调签名( N)[\#2225](https://github.com/ros2/rosbag2/issues/2225))

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 通过等待执行器旋转来地址测试故障( Q)[\#2001](https://github.com/ros2/rosbag2/issues/2001))

- 上游质量与Apex.AI第2部分相比有所变化([\#1924](https://github.com/ros2/rosbag2/issues/1924))

- 使用 DDS 队列深度进行订阅,作为出版商的最大值( D)[\#1960](https://github.com/ros2/rosbag2/issues/1960))

- 贡献者:克里斯托弗·贝达德,佐藤大介,爱默生·克纳普,迈克尔·奥尔洛夫,小型-1235,苔藓80

<span id="rosbag2-test-msgdefs"></span>

## [rosbag2_test_msgdefs](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_test_msgdefs/CHANGELOG.rst)

- 支持相对包括本地信件定义中的IDL([\#2241](https://github.com/ros2/rosbag2/issues/2241))

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 在嵌入子目录中添加搜索信件定义的支持( N)[\#2055](https://github.com/ros2/rosbag2/issues/2055))

- 贡献者:亨特·L·艾伦、迈克尔·奥尔洛夫、莫斯费特80

<span id="rosbag2-tests"></span>

## [rosbag2_tests](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_tests/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#2384](https://github.com/ros2/rosbag2/issues/2384))

- 更新 Rosbag2 文件名格式到 `index+name+timestamp` ([\#2265](https://github.com/ros2/rosbag2/issues/2265))

- 为记录器添加静态主题特性( E)[\#2319](https://github.com/ros2/rosbag2/issues/2319))

- 添加 `--max-cache-duration` 选项,用于设定时限的快照([\#2289](https://github.com/ros2/rosbag2/issues/2289))

- 工作杂乱 `bagsize_split_is_at_least_specified_size` 测试([\#2311](https://github.com/ros2/rosbag2/issues/2311))

- 添加 `input_serialization_format` 财务报告和财务报告 `output_serialization_format` 改为: `RecordOptions`,折旧 `rmw_serialization_format` ([\#2215](https://github.com/ros2/rosbag2/issues/2215))

- 通过增加缓存大小来测试地址记录器的片段([\#2203](https://github.com/ros2/rosbag2/issues/2203))

- 使用 `rclcpp typesupport helpers` 输入 `rosbag2_cpp` ([\#2017](https://github.com/ros2/rosbag2/issues/2017))

- 向 Python 显示更多播放器和记录器 API,并改进信号处理([\#2062](https://github.com/ros2/rosbag2/issues/2062))

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 解决Rosbag2 玩家在调用停止 API 时的僵局([\#2057](https://github.com/ros2/rosbag2/issues/2057))

- 引入新的 `BaseWriteInterface` 方法( I) `write_messages` 财务报告和财务报告 `write_message` 以提供运行状态、 折旧旧写 API([\#2030](https://github.com/ros2/rosbag2/issues/2030))

- 通过等待执行器旋转来地址测试故障( Q)[\#2001](https://github.com/ros2/rosbag2/issues/2001))

- 上游质量与Apex.AI第2部分相比有所变化([\#1924](https://github.com/ros2/rosbag2/issues/1924))

- 贡献者:克里斯托弗·贝达德、佐藤大介、爱默生·克纳普、迈克尔·奥尔洛夫、莫斯费特80

<span id="rosbag2-transport"></span>

## [rosbag2_transport](https://github.com/ros2/rosbag2/tree/lyrical/rosbag2_transport/CHANGELOG.rst)

- 将 /bigobj 应用到所有 MSVC 构建于 rosbag2\_ transport () 中 。[\#2424](https://github.com/ros2/rosbag2/issues/2424)) ([\#2428](https://github.com/ros2/rosbag2/issues/2428))

- 固定: MSVC 2022和C++20 的Rosbag2_transport中的固定编译错误([\#2407](https://github.com/ros2/rosbag2/issues/2407))

- 当话题名称没有主斜线时, 修复 QoS 覆盖被忽略([\#2394](https://github.com/ros2/rosbag2/issues/2394))

- 在 RecorderImpl 中重构瞬态本地主题检测和记录([\#2395](https://github.com/ros2/rosbag2/issues/2395))

- 将比赛条件固定在 `RecordSrvsSimTimeTest` 正在等待时钟订阅器( N)[\#2396](https://github.com/ros2/rosbag2/issues/2396))

- 添加 `--repeat-all-transient-local` 用于自动瞬态本地主题检测的旗帜( C)[\#2391](https://github.com/ros2/rosbag2/issues/2391))

- 重复的瞬间本地主题:记录器、 CLI 和 Python 绑定([\#2387](https://github.com/ros2/rosbag2/issues/2387))

- 执行 `transient-local topic` 重复 Writer API 和 拆分/ snapshot 集成([\#2386](https://github.com/ros2/rosbag2/issues/2386))

- 添加 `TransientLocalMessagesCache` 财务报告和财务报告 `RecordOptions` 用于重复瞬间-局部主题([\#2385](https://github.com/ros2/rosbag2/issues/2385))

- 使用新的 ROSIDL 聚合 CMake 目标([\#2384](https://github.com/ros2/rosbag2/issues/2384))

- 地址闪烁度 `rosbag2_transport::test_record_services` 测试([\#2368](https://github.com/ros2/rosbag2/issues/2368))

- 添加基于时间的 Resume 服务支持( E)[\#2357](https://github.com/ros2/rosbag2/issues/2357))

- 地址 : `wait_for_playback_to_start()` 函数( C)[\#2344](https://github.com/ros2/rosbag2/issues/2344))

- 添加 `set_on_start_recording_callback()` 设置录制开始时的回调( N)[\#2340](https://github.com/ros2/rosbag2/issues/2340))

- 删除不必要的依赖性 `yaml_cpp_vendor` ([\#2353](https://github.com/ros2/rosbag2/issues/2353))

- 允许在录音时暂停/恢复服务呼叫( R)[\#2349](https://github.com/ros2/rosbag2/issues/2349))

- 执行延迟和基于时间的记录器和播放器服务,增加新的包分割模式([\#2330](https://github.com/ros2/rosbag2/issues/2330))

- 更新 Rosbag2 文件名格式到 `index+name+timestamp` ([\#2265](https://github.com/ros2/rosbag2/issues/2265))

- 解决可能存在的僵局 `seek(timestamp)` ([\#2345](https://github.com/ros2/rosbag2/issues/2345))

- 添加缺失的字段到 `RecordOptions` YAML 编码/解码功能,并包含一个编译时间保障([\#2334](https://github.com/ros2/rosbag2/issues/2334))

- 按拆分计数执行循环记录( E)`--max-bag-files`) ([\#2218](https://github.com/ros2/rosbag2/issues/2218))

- 改进 `ros2 bag convert` 剪切和添加碎片的性能 `--input-options` ([\#2325](https://github.com/ros2/rosbag2/issues/2325))

- 为记录器添加静态主题特性( E)[\#2319](https://github.com/ros2/rosbag2/issues/2319))

- 添加 `--max-cache-duration` 选项,用于设定时限的快照([\#2289](https://github.com/ros2/rosbag2/issues/2289))

- 修补碎片 `can_record_again_after_stop` 测试([\#2313](https://github.com/ros2/rosbag2/issues/2313))

- 将错误返回代码添加到 `~/stop` 服务请求( E)[\#2312](https://github.com/ros2/rosbag2/issues/2312))

- 添加 Record, Stop, Start Discovery, Stop Discovery, 和 IsDiscovery Running 服务给 Recorder( 开始发现, 停止发现, 停止发现, 和 IsDiscovery Running 用于记录器( )[\#2248](https://github.com/ros2/rosbag2/issues/2248))

- 对内部 Rosbag2 出版主题使用 QoS 覆盖设置([\#2286](https://github.com/ros2/rosbag2/issues/2286))

- 在 YAML 解码器解码器中修复解码器和编码不匹配([\#2277](https://github.com/ros2/rosbag2/issues/2277))

- 将Apex.AI的上游小修补纳入其中[\#2240](https://github.com/ros2/rosbag2/issues/2240))

- 修复 macOS 构建: 禁用线程安全说明 `locked_priority_queue.hpp` ([\#2245](https://github.com/ros2/rosbag2/issues/2245))

- Fix C++ 记录器在停止时失败( ) 然后记录( ) 被调用相同的袋名( Name[\#2224](https://github.com/ros2/rosbag2/issues/2224))

- 添加直接 API 用于 `rosbag2_transport::Recorder` ([\#2221](https://github.com/ros2/rosbag2/issues/2221))

- 添加 `input_serialization_format` 财务报告和财务报告 `output_serialization_format` 改为: `RecordOptions`,折旧 `rmw_serialization_format` ([\#2215](https://github.com/ros2/rosbag2/issues/2215))

- 在 rosbag2_运输测试中启用 RMW 通信隔离([\#2190](https://github.com/ros2/rosbag2/issues/2190))

- 为散列映射密钥添加主题名称和类型分隔符以避免碰撞( S)[\#2210](https://github.com/ros2/rosbag2/issues/2210))

- 添加缓存用于 `TopicFilter` 避免对发现造成性能负担([\#1486](https://github.com/ros2/rosbag2/issues/1486))

- 通过增加缓存大小来测试地址记录器的片段([\#2203](https://github.com/ros2/rosbag2/issues/2203))

- 通过改进发现逻辑,减少罗斯巴格2记录器的CPU高空发现([\#2201](https://github.com/ros2/rosbag2/issues/2201))

- 将数据种族固定在 `PlayerProgressBar` 使用原子变量([\#2194](https://github.com/ros2/rosbag2/issues/2194))

- 在测试中修正数据种族 `MockSequentialWriter` ([\#2192](https://github.com/ros2/rosbag2/issues/2192))

- 玩家现在尊重相同时间戳的原始消息顺序([\#2172](https://github.com/ros2/rosbag2/issues/2172))

- 按值计算返回玩家存储选项 `get_storage_options()` 避免引用不便([\#2181](https://github.com/ros2/rosbag2/issues/2181))

- 修复玩家不玩的时候 `read_ahead_queue_size` 等于 1 (E) (中文(简体) )[\#2174](https://github.com/ros2/rosbag2/issues/2174))

- 解决多种族条件和玩家的僵局([\#2171](https://github.com/ros2/rosbag2/issues/2171))

- 通过管理按时间顺序排列的信息顺序来修复多包重放停滞状态,并改进回放性能 `ReadersManager` ([\#2158](https://github.com/ros2/rosbag2/issues/2158))

- 修正 MCAPStorage :: seek( 时间) 当时间戳匹配当前时间时推进( )[\#2157](https://github.com/ros2/rosbag2/issues/2157))

- 将丢失的信息发布到 " 事件/信息_丢失 " 专题([\#2150](https://github.com/ros2/rosbag2/issues/2150))

- 添加 `RecorderEventNotifier` 类([\#2144](https://github.com/ros2/rosbag2/issues/2144))

- 在多包重放和更新过程中解决僵局 `wait_for_playback_to_start` ([\#2143](https://github.com/ros2/rosbag2/issues/2143))

- 使用 `rclcpp typesupport helpers` 输入 `rosbag2_cpp` ([\#2017](https://github.com/ros2/rosbag2/issues/2017))

- 修复召回, 不为 MESSAGES\_ LOST 事件调用( Name[\#2105](https://github.com/ros2/rosbag2/issues/2105))

- 为玩家的开始时间和回放时间添加公开的 API( E)[\#2095](https://github.com/ros2/rosbag2/issues/2095))

- 修复 CMAKE 折旧[\#2067](https://github.com/ros2/rosbag2/issues/2067))

- 解决Rosbag2 玩家在调用停止 API 时的僵局([\#2057](https://github.com/ros2/rosbag2/issues/2057))

- 添加信件损失统计回调和记录( E)[\#2039](https://github.com/ros2/rosbag2/issues/2039))

- 跳过片状 `can_record_again_after_stop` 测试([\#2031](https://github.com/ros2/rosbag2/issues/2031))

- 修补 `cout` 已禁用进度栏时输出( E)[\#2024](https://github.com/ros2/rosbag2/issues/2024))

- 通过避免零星的唤醒和在玩家启动时修正不正确的间隔,改善消息发布时间([\#2025](https://github.com/ros2/rosbag2/issues/2025))

- 修补 `playing_respects_delay` 测试碎片化( E)[\#2016](https://github.com/ros2/rosbag2/issues/2016))

- 通过等待执行器旋转来地址测试故障( Q)[\#2001](https://github.com/ros2/rosbag2/issues/2001))

- 避免发送不存在的取消请求( E)[\#2005](https://github.com/ros2/rosbag2/issues/2005))

- 在玩家_action_client.cpp中修正一个可能未初始化的警告([\#1969](https://github.com/ros2/rosbag2/issues/1969))

- 上游质量与Apex.AI第2部分相比有所变化([\#1924](https://github.com/ros2/rosbag2/issues/1924))

- 错误修正 : `ros2 bag convert` 以压缩模式发送信件( E)[\#1975](https://github.com/ros2/rosbag2/issues/1975))

- 使用 DDS 队列深度进行订阅,作为出版商的最大值( D)[\#1960](https://github.com/ros2/rosbag2/issues/1960))

- 贡献者:许巴里、克里斯、拉朗谢特、克里斯托弗、贝达德、佐藤大介、丹吉特、德鲁夫、帕特尔、埃默森·克纳普、雅诺施·麦克豪林斯基、卢克·西、迈克尔·卡罗尔、迈克尔·奥尔洛夫、萨希尔·拉赫马尼、斯科特·克洛根、谢恩·洛雷茨、藤田托莫亚、托尼·纳贾尔、巴兰博洛古尔、卡洛斯-阿普克斯、加热\[机器人\]、苔藓80

<span id="rosgraph-msgs"></span>

## [rosgraph_msgs](https://github.com/ros2/rcl_interfaces/tree/lyrical/rosgraph_msgs/CHANGELOG.rst)

- 实际构建新的图表描述信件( Q)[\#192](https://github.com/ros2/rcl_interfaces/issues/192)) ([\#198](https://github.com/ros2/rcl_interfaces/issues/198))

- 添加图形描述信件到 `rosgraph_msgs` ([\#188](https://github.com/ros2/rcl_interfaces/issues/188))

- 修补 cmake 折旧( E)[\#180](https://github.com/ros2/rcl_interfaces/issues/180))

- 贡献者: Emerson Knapp, 增能\[bot\], mossfet80

<span id="rosidl-adapter"></span>

## [rosidl_adapter](https://github.com/ros2/rosidl/tree/lyrical/rosidl_adapter/CHANGELOG.rst)

- 修正片段的未来回归 8 ([\#936](https://github.com/ros2/rosidl//issues/936))

- 修补 @ optional for string literals ()[\#905](https://github.com/ros2/rosidl/issues/905))

- 导出打字信息( E)[\#903](https://github.com/ros2/rosidl/issues/903))

- 添加可选解析( N)[\#883](https://github.com/ros2/rosidl/issues/883))

- 制式为 minersion ([\#849](https://github.com/ros2/rosidl/issues/849))

- 贡献者:迈克尔·卡尔斯特罗姆、苔藓80

<span id="rosidl-buffer"></span>

## [rosidl_buffer](https://github.com/ros2/rosidl/tree/lyrical/rosidl_buffer/CHANGELOG.rst)

- 在 rosidl 中添加缺失的 std:: vector 兼容的 API: Buffer ()[\#959](https://github.com/ros2/rosidl/issues/959))

- 弹出 rosidl_缓冲 min CMake 版本([\#956](https://github.com/ros2/rosidl/issues/956))

- 更新 rosidl cpp 路径以发出 rosidl: Buffer for uint8\[\] type ()[\#942](https://github.com/ros2/rosidl/issues/942))

- 向 c\_ helpers.cpp 添加 cstdint 包括 。 ([\#953](https://github.com/ros2/rosidl/issues/953))

- 为本地缓冲类型支持添加 rosidl_buffer 和 rosidl_buffer_后端([\#941](https://github.com/ros2/rosidl/issues/941))

- 贡献者:CY陈,谢恩·洛雷茨,苔藓80

<span id="rosidl-buffer-backend"></span>

## [rosidl_buffer_backend](https://github.com/ros2/rosidl/tree/lyrical/rosidl_buffer_backend/CHANGELOG.rst)

- 清晰返回 get_descriptor_type_support() ()[\#958](https://github.com/ros2/rosidl/issues/958))

- 弹出 rosidl_缓冲 min CMake 版本([\#956](https://github.com/ros2/rosidl/issues/956))

- 为本地缓冲类型支持添加 rosidl_buffer 和 rosidl_buffer_后端([\#941](https://github.com/ros2/rosidl/issues/941))

- 贡献者:陈共青团、谢恩·洛雷茨

<span id="rosidl-buffer-backend-registry"></span>

## [rosidl_buffer_backend_registry](https://github.com/ros2/rosidl/tree/lyrical/rosidl_buffer_backend_registry/CHANGELOG.rst)

- 添加缺失的 ament\_ cmake\_ gtest 读取( S)[\#960](https://github.com/ros2/rosidl/issues/960))

- 弹出 rosidl_缓冲 min CMake 版本([\#956](https://github.com/ros2/rosidl/issues/956))

- 添加 rosidl_buffer_后端\_ registry ()[\#944](https://github.com/ros2/rosidl/issues/944))

- 贡献者:CY陈、Scott K Logan、Shane Loretz

<span id="rosidl-buffer-py"></span>

## [rosidl_buffer_py](https://github.com/ros2/rosidl/tree/lyrical/rosidl_buffer_py/CHANGELOG.rst)

- 弹出 rosidl_缓冲 min CMake 版本([\#956](https://github.com/ros2/rosidl/issues/956))

- 修补 pybind11 旋翼键([\#955](https://github.com/ros2/rosidl/issues/955))

- 更新 rosidl cpp 路径以发出 rosidl: Buffer for uint8\[\] type ()[\#942](https://github.com/ros2/rosidl/issues/942))

- 贡献者:CY 陈、Christoph Fröhlich、Shane Loretz

<span id="rosidl-cli"></span>

## [rosidl_cli](https://github.com/ros2/rosidl/tree/lyrical/rosidl_cli/CHANGELOG.rst)

- 修正片段的未来回归 8 ([\#936](https://github.com/ros2/rosidl//issues/936))

- 删除导入lib-metadata ([\#917](https://github.com/ros2/rosidl/issues/917))

- 导出打字信息( E)[\#903](https://github.com/ros2/rosidl/issues/903))

- 固定设置工具[\#877](https://github.com/ros2/rosidl/issues/877))

- rosidl_cli: 添加类型描述支持([\#857](https://github.com/ros2/rosidl/issues/857))

- 贡献者:弗朗西斯科·罗西、迈克尔·卡尔斯特罗姆、苔藓80

<span id="rosidl-cmake"></span>

## [rosidl_cmake](https://github.com/ros2/rosidl/tree/lyrical/rosidl_cmake/CHANGELOG.rst)

- 安装接口文件到与 idl 相同的文件夹 (S)[\#935](https://github.com/ros2/rosidl/issues/935))

- 为 rosidl 生成的接口目标创建聚合目标([\#947](https://github.com/ros2/rosidl//issues/947))

- 添加 `rosidl_auto_generate_interfaces` 函数( C)[\#918](https://github.com/ros2/rosidl/issues/918))

- 导出打字信息( E)[\#903](https://github.com/ros2/rosidl/issues/903))

- 删除已贬值的 rosidl\_ target\_ 界面 。 ()[\#898](https://github.com/ros2/rosidl/issues/898))

- 固定 cmake \< 3.10 折旧([\#875](https://github.com/ros2/rosidl/issues/875))

- 贡献者:爱默生·克纳普,吉本小太郎,迈克尔·卡尔斯特罗姆,蒂姆·温特,藤田丰也,苔丝菲特80

<span id="rosidl-core-generators"></span>

## [rosidl_core_generators](https://github.com/ros2/rosidl_core/tree/lyrical/rosidl_core_generators/CHANGELOG.rst)

- 还原“Revert 添加的 rosidl_generator_rs( 添加的 rosidl_generator_rs)[\#7](https://github.com/ros2/rosidl_core/issues/7))” ([\#8](https://github.com/ros2/rosidl_core/issues/8))” ([\#9](https://github.com/ros2/rosidl_core/issues/9))

- 固定cmake 折旧([\#10](https://github.com/ros2/rosidl_core/issues/10))

- 贡献者:埃斯特韦·费尔南德斯、苔藓80

<span id="rosidl-core-runtime"></span>

## [rosidl_core_runtime](https://github.com/ros2/rosidl_core/tree/lyrical/rosidl_core_runtime/CHANGELOG.rst)

- 添加 rosidl_buffer_py 以建立_export_dependent 表示明确的组分辨率([\#14](https://github.com/ros2/rosidl_core/issues/14))

- 固定cmake 折旧([\#10](https://github.com/ros2/rosidl_core/issues/10))

- 贡献者:CY陈,苔藓80

<span id="rosidl-default-generators"></span>

## [rosidl_default_generators](https://github.com/ros2/rosidl_defaults/tree/lyrical/rosidl_default_generators/CHANGELOG.rst)

- 固定cmake 折旧([\#31](https://github.com/ros2/rosidl_defaults/issues/31))

- 贡献者:苔藓80

<span id="rosidl-default-runtime"></span>

## [rosidl_default_runtime](https://github.com/ros2/rosidl_defaults/tree/lyrical/rosidl_default_runtime/CHANGELOG.rst)

- 固定cmake 折旧([\#31](https://github.com/ros2/rosidl_defaults/issues/31))

- 贡献者:苔藓80

<span id="rosidl-dynamic-typesupport"></span>

## [rosidl_dynamic_typesupport](https://github.com/ros2/rosidl_dynamic_typesupport/tree/lyrical/CHANGELOG.rst)

- 不要自动启用动词制作文件([\#17](https://github.com/ros2/rosidl_dynamic_typesupport/issues/17))

- 撰稿人:克里斯·拉兰谢特

<span id="rosidl-dynamic-typesupport-fastrtps"></span>

## [rosidl_dynamic_typesupport_fastrtps](https://github.com/ros2/rosidl_dynamic_typesupport_fastrtps/tree/lyrical/CHANGELOG.rst)

- 合并拉动请求 [\#11](https://github.com/ros2/rosidl_dynamic_typesupport_fastrtps/issues/11) 从 mossfet80/patch-1 调用

- 不要自动启用动词制作文件 。 ([\#9](https://github.com/ros2/rosidl_dynamic_typesupport_fastrtps/issues/9))

- 贡献者:克里斯·拉朗谢特,苔藓80

<span id="rosidl-generator-c"></span>

## [rosidl_generator_c](https://github.com/ros2/rosidl/tree/lyrical/rosidl_generator_c/CHANGELOG.rst)

- 导出打字信息( E)[\#903](https://github.com/ros2/rosidl/issues/903))

- 制式为 minersion ([\#849](https://github.com/ros2/rosidl/issues/849))

- rosidl_cli: 添加类型描述支持([\#857](https://github.com/ros2/rosidl/issues/857))

- 贡献者:弗朗西斯科·罗西、迈克尔·卡尔斯特罗姆、苔藓80

<span id="rosidl-generator-cpp"></span>

## [rosidl_generator_cpp](https://github.com/ros2/rosidl/tree/lyrical/rosidl_generator_cpp/CHANGELOG.rst)

- 更新 rosidl cpp 路径以发出 rosidl: Buffer for uint8\[\] type ()[\#942](https://github.com/ros2/rosidl/issues/942))

- 使用 IWYU pragma 导出来避免生成信头的串联警告([\#902](https://github.com/ros2/rosidl//issues/902))

- rosidl_generator_cpp: 生成结构的缩写信息特征和 to_tuple_ref ()[\#928](https://github.com/ros2/rosidl//issues/928))

- 制作( M) `data_type` 财务报告和财务报告 `name` 特征连结( E)[\#929](https://github.com/ros2/rosidl//issues/929))

- 导出打字信息( E)[\#903](https://github.com/ros2/rosidl/issues/903))

- 添加静态(\_C) ([\#884](https://github.com/ros2/rosidl/issues/884))

- 制式为 minersion ([\#849](https://github.com/ros2/rosidl/issues/849))

- rosidl_cli: 添加类型描述支持([\#857](https://github.com/ros2/rosidl/issues/857))

- 添加缺失的 cstdint 包括 ([\#864](https://github.com/ros2/rosidl/issues/864))

- 已删除的折旧方法( E)[\#863](https://github.com/ros2/rosidl/issues/863))

- 贡献者:亚当·利珀,亚历杭德罗·埃尔南德斯·科尔德罗,共青团陈,弗朗西斯科·罗西,迈克尔·卡尔斯特罗姆,摩斯费特80,奥伊斯泰因·斯图尔

<span id="rosidl-generator-dds-idl"></span>

## [rosidl_generator_dds_idl](https://github.com/ros2/rosidl_dds/tree/lyrical/rosidl_generator_dds_idl/CHANGELOG.rst)

- 更新 cmake 版本要求( E)[\#64](https://github.com/ros2/rosidl_dds/issues/64))

- 贡献者:苔藓80

<span id="rosidl-generator-py"></span>

## [rosidl_generator_py](https://github.com/ros2/rosidl_python/tree/lyrical/rosidl_generator_py/CHANGELOG.rst)

- 类型提示使用绝对名称( N)[\#258](https://github.com/ros2/rosidl_python/issues/258))

- 特性: 为 ament_python_install_package 添加依赖标志([\#254](https://github.com/ros2/rosidl_python/issues/254))

- 添加对 rosidl 的支持: 在 rosidl Python 路径中为 rclpy () 添加支持[\#250](https://github.com/ros2/rosidl_python/issues/250))

- 在任务上列出(带有模板)的 Cast 顺序( )[\#249](https://github.com/ros2/rosidl_python/issues/249))

- 以0.19.0-进口单处理违反规定行为[\#248](https://github.com/ros2/rosidl_python/issues/248))

- 添加 DEPENDS_EXPLICT_ONLY 以删除隐含的依赖性([\#238](https://github.com/ros2/rosidl_python/issues/238))

- 使用基于容器的输入集合进行折旧([\#243](https://github.com/ros2/rosidl_python//issues/243))

- 使用新 BaseImpl 的更新( U)[\#241](https://github.com/ros2/rosidl_python/issues/241))

- 删除二次通话( E)[\#232](https://github.com/ros2/rosidl_python/issues/232))

- 来自基类的衍生信件( E)[\#230](https://github.com/ros2/rosidl_python/issues/230))

- 暂时删除 NORED 。[\#229](https://github.com/ros2/rosidl_python/issues/229))

- 静态打入信件、服务和动作[\#206](https://github.com/ros2/rosidl_python/issues/206))

- 贡献者:安东尼·韦尔特、CY陈、迈克尔·卡尔斯特罗姆、纳达夫·埃尔卡贝茨

<span id="rosidl-generator-rs"></span>

## [rosidl_generator_rs](https://github.com/ros2-rust/rosidl_rust/tree/main/rosidl_generator_rs/CHANGELOG.rst)

- 固定( rosidl\_ generator\_ rs\_ generate\_ interfaces): 移除全球 CMAKE\_ shaled\_ LINKER\_ FLAGS 变量的中毒(# 22)

- 更改软件包元数据以指向新的 ros- env 框 (# 21)

- 修复 Ubuntu 决断上的 TransientParse Error (# 20)

- 修补: 不将后缀移入字符串( P)[\#18](https://github.com/ros2-rust/rosidl_rust/issues/18))

- 固定 : 为 Python \< 3.9 (RHEL 8) 添加 str.removes afthix () 后端端口[\#17](https://github.com/ros2-rust/rosidl_rust/issues/17))

- 功绩: 相对模块路径分辨率( E)[\#12](https://github.com/ros2-rust/rosidl_rust/issues/12)\* 更改了所有生成的代码,以使用相对符号,而不是 `crate::` 。重修 rosidl_generator_rs 稍稍简单一点。 将实际模板与重用它们的文件分开 。 \* WIP 用于在所有结构、 成员和 idl 生成的常数中添加文档 。 \* 清除所有来自生成代码的表面警告 。

- 构建: 将 rosidl_runtime_rs 依赖性版本更新为 0.6 ()[\#14](https://github.com/ros2-rust/rosidl_rust/issues/14))

- 固定: 将 rosidl_runtime_rs 依赖性版本更新为 0.5 ()[\#11](https://github.com/ros2-rust/rosidl_rust/issues/11))

- Serde的固定使用( )[\#9](https://github.com/ros2-rust/rosidl_rust/issues/9)) \* 修补serde的使用 \* 包含serde用于服务 \* \*

- 更新最新版的 " 行动特征 " ([\#7](https://github.com/ros2-rust/rosidl_rust/issues/7)) \*最新版本的动作特征更新 \* 固定使用 serde \* \*

- 固定cmake 折旧([\#6](https://github.com/ros2-rust/rosidl_rust/issues/6)\* 修补 CMake 腐烂 cmake 版本 \< 然后 3.10 被腐烂 \* 更新 CMakeLists.txt

- 修饰:清理依赖性( E)[\#5](https://github.com/ros2-rust/rosidl_rust/issues/5))

- 修补: 添加缺失的依赖

- 清除更改日志。 删除 rosidl_runtime_rs 作为依赖

- 设置 python 可执行的 var 用于自定义的 CMake 命令([\#3](https://github.com/ros2-rust/rosidl_rust/issues/3))

- 贡献者:埃斯特韦·费尔南德斯,格雷,金伯利·N·麦圭尔,萨姆·普里韦特,谢恩·洛雷茨,西尔维奥·特拉韦萨罗,摩斯费特80

<span id="rosidl-generator-tests"></span>

## [rosidl_generator_tests](https://github.com/ros2/rosidl/tree/lyrical/rosidl_generator_tests/CHANGELOG.rst)

- 使用新的聚合rosidl目标,而不是 \_TARGETS([\#952](https://github.com/ros2/rosidl/issues/952))

- rosidl_generator_cpp: 生成结构的缩写信息特征和 to_tuple_ref ()[\#928](https://github.com/ros2/rosidl//issues/928))

- 制作( M) `data_type` 财务报告和财务报告 `name` 特征连结( E)[\#929](https://github.com/ros2/rosidl//issues/929))

- 固定 cmake \< 3.10 折旧([\#875](https://github.com/ros2/rosidl/issues/875))

- 贡献者:阿列克西斯·佐吉亚斯、迈克尔·卡尔斯特罗姆、莫斯费特80、艾斯泰因·斯图尔

<span id="rosidl-generator-type-description"></span>

## [rosidl_generator_type_description](https://github.com/ros2/rosidl/tree/lyrical/rosidl_generator_type_description/CHANGELOG.rst)

- 添加 DEPENDS_EXPLICT_ONLY 以删除隐含的依赖性([\#910](https://github.com/ros2/rosidl//issues/910))

- 添加缺少对 ament_cmake_pytest 的依赖([\#914](https://github.com/ros2/rosidl/issues/914))

- 导出打字信息( E)[\#903](https://github.com/ros2/rosidl/issues/903))

- 制式为 minersion ([\#849](https://github.com/ros2/rosidl/issues/849))

- rosidl_cli: 添加类型描述支持([\#857](https://github.com/ros2/rosidl/issues/857))

- 贡献者:安东尼·韦尔特,弗朗西斯科·罗西,迈克尔·卡尔斯特罗姆,斯科特·K·洛根,苔丝菲特80

<span id="rosidl-parser"></span>

## [rosidl_parser](https://github.com/ros2/rosidl/tree/lyrical/rosidl_parser/CHANGELOG.rst)

- 修正片段的未来回归 8 ([\#936](https://github.com/ros2/rosidl//issues/936))

- 添加可选解析( N)[\#883](https://github.com/ros2/rosidl/issues/883))

- 固定 cmake \< 3.10 折旧([\#875](https://github.com/ros2/rosidl/issues/875))

- 贡献者:迈克尔·卡尔斯特罗姆、苔藓80

<span id="rosidl-pycommon"></span>

## [rosidl_pycommon](https://github.com/ros2/rosidl/tree/lyrical/rosidl_pycommon/CHANGELOG.rst)

- 固定回归( E)[\#951](https://github.com/ros2/rosidl/issues/951))

- 修正片段的未来回归 8 ([\#936](https://github.com/ros2/rosidl//issues/936))

- 添加 BasicImpl (英语:[\#912](https://github.com/ros2/rosidl/issues/912))

- 导出打字信息( E)[\#903](https://github.com/ros2/rosidl/issues/903))

- 提供基础课程 `rosidl_pycommon` ([\#887](https://github.com/ros2/rosidl/issues/887))

- 固定设置工具 折旧( )[\#880](https://github.com/ros2/rosidl/issues/880))

- 贡献者:迈克尔·卡尔斯特罗姆、苔藓80

<span id="rosidl-runtime-c"></span>

## [rosidl_runtime_c](https://github.com/ros2/rosidl/tree/lyrical/rosidl_runtime_c/CHANGELOG.rst)

- 更新 rosidl cpp 路径以发出 rosidl: Buffer for uint8\[\] type ()[\#942](https://github.com/ros2/rosidl/issues/942))

- 修复类型支持文件中的复制/粘贴错误( Q)[\#906](https://github.com/ros2/rosidl/issues/906))

- 固定 cmake \< 3.10 折旧([\#875](https://github.com/ros2/rosidl/issues/875))

- 在 rosidl_runtime_c 中添加 ament_cmake_gtest 依赖性 ()[\#865](https://github.com/ros2/rosidl/issues/865))

- 贡献者:CY陈、Chris Lalancette、Christophe Bedard、苔藓80

<span id="rosidl-runtime-cpp"></span>

## [rosidl_runtime_cpp](https://github.com/ros2/rosidl/tree/lyrical/rosidl_runtime_cpp/CHANGELOG.rst)

- 更新 rosidl cpp 路径以发出 rosidl: Buffer for uint8\[\] type ()[\#942](https://github.com/ros2/rosidl/issues/942))

- rosidl_generator_cpp: 生成结构的缩写信息特征和 to_tuple_ref ()[\#928](https://github.com/ros2/rosidl//issues/928))

- 制作( M) `data_type` 财务报告和财务报告 `name` 特征连结( E)[\#929](https://github.com/ros2/rosidl//issues/929))

- 固定 cmake \< 3.10 折旧([\#875](https://github.com/ros2/rosidl/issues/875))

- 添加缺失的 cstdint 包括 ([\#864](https://github.com/ros2/rosidl/issues/864))

- 贡献者:CY陈、Michael Carlstrom、Mossfet80、 ⁇ ystein Sture

<span id="rosidl-runtime-py"></span>

## [rosidl_runtime_py](https://github.com/ros2/rosidl_runtime_py/tree/lyrical/CHANGELOG.rst)

- 添加对 rosidl 的支持: 在 rosidl Python 路径中为 rclpy () 添加支持[\#39](https://github.com/ros2/rosidl_runtime_py/issues/39))

- 修正片段8 (% 1)[\#40](https://github.com/ros2/rosidl_runtime_py/issues/40))

- 在软件包中添加 py.typed ([\#37](https://github.com/ros2/rosidl_runtime_py/issues/37))

- 固定设置工具[\#35](https://github.com/ros2/rosidl_runtime_py/issues/35))

- 贡献者:CY陈,迈克尔·卡尔斯特罗姆,弗拉基米尔·格茨,苔藓Fet80

<span id="rosidl-typesupport-c"></span>

## [rosidl_typesupport_c](https://github.com/ros2/rosidl_typesupport/tree/lyrical/rosidl_typesupport_c/CHANGELOG.rst)

- 添加 DEPENDS_EXPLICT_ONLY 以删除隐含的依赖性([\#168](https://github.com/ros2/rosidl_typesupport/issues/168))

- 撰稿人:安东尼·韦尔特

<span id="rosidl-typesupport-cpp"></span>

## [rosidl_typesupport_cpp](https://github.com/ros2/rosidl_typesupport/tree/lyrical/rosidl_typesupport_cpp/CHANGELOG.rst)

- 添加 DEPENDS_EXPLICT_ONLY 以删除隐含的依赖性([\#168](https://github.com/ros2/rosidl_typesupport/issues/168))

- 删除已贬值的rosidl_typesupport_cpp/type_support_map.h (中文(简体) ).[\#167](https://github.com/ros2/rosidl_typesupport/issues/167))

- 取消复制类型\_ 支持\_ map.h 头条( Name[\#81](https://github.com/ros2/rosidl_typesupport/issues/81))

- 撰稿人:安东尼·韦尔特、克里斯托弗·贝达德

<span id="rosidl-typesupport-fastrtps-c"></span>

## [rosidl_typesupport_fastrtps_c](https://github.com/ros2/rosidl_typesupport_fastrtps/tree/lyrical/rosidl_typesupport_fastrtps_c/CHANGELOG.rst)

- 更新 rosidl 类型支持支持 rosidl: 在嵌入的 unit8 \[\] ()[\#151](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/151)) ([\#152](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/152))

- 添加对 rosidl: 缓冲类型序列化的支持 ([\#144](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/144))

- 使用变量来控制共享/静态构建类型( N)[\#138](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/138))

- 切换 ament_index_python 和 rosidl_cli 到 exec_depend. ()[\#137](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/137))

- 删除已折旧的代码( N)[\#135](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/135))

- 固定cmake 折旧([\#134](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/134))

- 调整序列大小前检查剩余大小( E)[\#130](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/130))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,安东尼·韦尔特,CY陈,克里斯·拉兰谢特,杰伊·斯里达兰,米格尔公司,加热\[bot\],苔藓80

<span id="rosidl-typesupport-fastrtps-cpp"></span>

## [rosidl_typesupport_fastrtps_cpp](https://github.com/ros2/rosidl_typesupport_fastrtps/tree/lyrical/rosidl_typesupport_fastrtps_cpp/CHANGELOG.rst)

- 清理缓冲序列化函数中的日志( E)[\#153](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/153)) ([\#154](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/154))

- 更新 rosidl 类型支持支持 rosidl: 在嵌入的 unit8 \[\] ()[\#151](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/151)) ([\#152](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/152))

- 添加导出依赖关系的缺失构建依赖( E)[\#149](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/149))

- 添加对 rosidl: 缓冲类型序列化的支持 ([\#144](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/144))

- 使用变量来控制共享/静态构建类型( N)[\#138](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/138))

- 添加 DEPENDS_EXPLICT_ONLY 以删除隐含的依赖性([\#136](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/136))

- 切换 ament_index_python 和 rosidl_cli 到 exec_depend. ()[\#137](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/137))

- 删除已折旧的代码( N)[\#135](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/135))

- 固定cmake 折旧([\#134](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/134))

- 调整序列大小前检查剩余大小( E)[\#130](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/130))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,安东尼·韦尔特,CY陈,克里斯·拉兰谢特,杰伊·斯里达兰,米格尔公司,斯科特·K·洛根, 壮志\[bot\], mossfet80

<span id="rosidl-typesupport-interface"></span>

## [rosidl_typesupport_interface](https://github.com/ros2/rosidl/tree/lyrical/rosidl_typesupport_interface/CHANGELOG.rst)

- 固定 cmake \< 3.10 折旧([\#875](https://github.com/ros2/rosidl/issues/875))

- 贡献者:苔藓80

<span id="rosidl-typesupport-introspection-c"></span>

## [rosidl_typesupport_introspection_c](https://github.com/ros2/rosidl/tree/lyrical/rosidl_typesupport_introspection_c/CHANGELOG.rst)

- 更新 rosidl cpp 路径以发出 rosidl: Buffer for uint8\[\] type ()[\#942](https://github.com/ros2/rosidl/issues/942))

- 添加 DEPENDS_EXPLICT_ONLY 以删除隐含的依赖性([\#910](https://github.com/ros2/rosidl//issues/910))

- 导出打字信息( E)[\#903](https://github.com/ros2/rosidl/issues/903))

- 制式为 minersion ([\#849](https://github.com/ros2/rosidl/issues/849))

- rosidl_cli: 添加类型描述支持([\#857](https://github.com/ros2/rosidl/issues/857))

- 贡献者:安东尼·韦尔特,CY陈,弗朗西斯科·罗西,迈克尔·卡尔斯特罗姆,苔丝费特80

<span id="rosidl-typesupport-introspection-cpp"></span>

## [rosidl_typesupport_introspection_cpp](https://github.com/ros2/rosidl/tree/lyrical/rosidl_typesupport_introspection_cpp/CHANGELOG.rst)

- 更新 rosidl cpp 路径以发出 rosidl: Buffer for uint8\[\] type ()[\#942](https://github.com/ros2/rosidl/issues/942))

- 添加 DEPENDS_EXPLICT_ONLY 以删除隐含的依赖性([\#910](https://github.com/ros2/rosidl//issues/910))

- 导出打字信息( E)[\#903](https://github.com/ros2/rosidl/issues/903))

- 制式为 minersion ([\#849](https://github.com/ros2/rosidl/issues/849))

- rosidl_cli: 添加类型描述支持([\#857](https://github.com/ros2/rosidl/issues/857))

- 贡献者:安东尼·韦尔特,CY陈,弗朗西斯科·罗西,迈克尔·卡尔斯特罗姆,苔丝费特80

<span id="rosidl-typesupport-introspection-tests"></span>

## [rosidl_typesupport_introspection_tests](https://github.com/ros2/rosidl/tree/lyrical/rosidl_typesupport_introspection_tests/CHANGELOG.rst)

- 更新 rosidl cpp 路径以发出 rosidl: Buffer for uint8\[\] type ()[\#942](https://github.com/ros2/rosidl/issues/942))

- 固定 cmake \< 3.10 折旧([\#875](https://github.com/ros2/rosidl/issues/875))

- 禁用覆盖作业中的测试失败, 参见 [\#812](https://github.com/ros2/rosidl/issues/812) ([\#853](https://github.com/ros2/rosidl/issues/853))

- 贡献者:CY陈、Jorge J. Perez、mossfet80

<span id="rosidl-typesupport-tests"></span>

## [rosidl_typesupport_tests](https://github.com/ros2/rosidl_typesupport/tree/lyrical/rosidl_typesupport_tests/CHANGELOG.rst)

- 通过所有 Rmw\_ cycloneds\_ cpp. ()[\#171](https://github.com/ros2/rosidl_typesupport/issues/171))

- 贡献者:藤田友也

<span id="rpyutils"></span>

## [rpyutils (英语).](https://github.com/ros2/rpyutils/tree/lyrical/CHANGELOG.rst)

- 强制执行\_ mypy – 限制执行( )[\#22](https://github.com/ros2/rpyutils/issues/22))

- 固定设置工具[\#17](https://github.com/ros2/rpyutils/issues/17))

- 贡献者:迈克尔·卡尔斯特罗姆、苔藓80

<span id="rqt"></span>

## [rqt](https://github.com/ros-visualization/rqt/tree/lyrical/rqt/CHANGELOG.rst)

- 固定设置工具[\#334](https://github.com/ros-visualization/rqt/issues/334))

- 固定设置工具[\#329](https://github.com/ros-visualization/rqt/issues/329))

- 贡献者:苔藓80

<span id="rqt-action"></span>

## [rqt_action](https://github.com/ros-visualization/rqt_action/tree/lyrical/CHANGELOG.rst)

- 固定设置工具[\#19](https://github.com/ros-visualization/rqt_action/issues/19))

- 删除 CODEOWINERS 和镜像滚动到主工作流程([\#16](https://github.com/ros-visualization/rqt_action/issues/16))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,苔藓80

<span id="rqt-bag"></span>

## [rqt_bag](https://github.com/ros-visualization/rqt_bag/tree/lyrical/rqt_bag/CHANGELOG.rst)

- 解决 Qt6 问题( 后端) [\#207](https://github.com/ros-visualization/rqt_bag/issues/207)) ([\#208](https://github.com/ros-visualization/rqt_bag/issues/208))

- 支持 Qt6 (帮助 )[\#206](https://github.com/ros-visualization/rqt_bag/issues/206))

- 清理错误标签的 BSD 许可证( Name[\#205](https://github.com/ros-visualization/rqt_bag/issues/205))

- 更好地处理大袋文件([\#178](https://github.com/ros-visualization/rqt_bag/issues/178))

- 显示四角体的卷、 投子、 yaw 值( Q)[\#179](https://github.com/ros-visualization/rqt_bag/issues/179))

- 在设置.py中修正 flake8 错误([\#192](https://github.com/ros-visualization/rqt_bag/issues/192))

- 改进原始视图,以便更好地处理阵列和时间对象([\#173](https://github.com/ros-visualization/rqt_bag/issues/173))

- 绘图_视图: 初始信件的固定显示( P)[\#180](https://github.com/ros-visualization/rqt_bag/issues/180))

- 固定设置工具[\#185](https://github.com/ros-visualization/rqt_bag/issues/185))

- 固定时间表决议[\#175](https://github.com/ros-visualization/rqt_bag/issues/175))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,马丁·佩卡,迈克尔·卡尔斯特罗姆,注音\[bot\],mossfet80

<span id="rqt-bag-plugins"></span>

## [rqt_bag_plugins](https://github.com/ros-visualization/rqt_bag/tree/lyrical/rqt_bag_plugins/CHANGELOG.rst)

- 支持 Qt6 (帮助 )[\#206](https://github.com/ros-visualization/rqt_bag/issues/206))

- 显示四角体的卷、 投子、 yaw 值( Q)[\#179](https://github.com/ros-visualization/rqt_bag/issues/179))

- 在设置.py中修正 flake8 错误([\#192](https://github.com/ros-visualization/rqt_bag/issues/192))

- 固定图像帮助器和 PNG 编码压缩深度的添加支持( P)[\#176](https://github.com/ros-visualization/rqt_bag/issues/176))

- 改进图景( E)[\#174](https://github.com/ros-visualization/rqt_bag/issues/174))

- 绘图_视图: 初始信件的固定显示( P)[\#180](https://github.com/ros-visualization/rqt_bag/issues/180))

- 固定设置工具[\#185](https://github.com/ros-visualization/rqt_bag/issues/185))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、马丁·佩卡、迈克尔·卡尔斯特罗姆、莫斯费特80

<span id="rqt-console"></span>

## [rqt_console](https://github.com/ros-visualization/rqt_console/tree/lyrical/CHANGELOG.rst)

- 支持 Qt6 (帮助 )[\#58](https://github.com/ros-visualization/rqt_console/issues/58))

- 固定版权测试([\#57](https://github.com/ros-visualization/rqt_console/issues/57))

- 添加版权标题

- 固定片段8([\#56](https://github.com/ros-visualization/rqt_console/issues/56))

- 固定片段8

- 删除彩色产出测试文件中的剩余进口([\#55](https://github.com/ros-visualization/rqt_console/issues/55))

- 消除剩余进口

- 基本支持颜色和粗体/粗体使用 ANSI 逃生代码([\#54](https://github.com/ros-visualization/rqt_console/issues/54))

- 用手动 ansi 代码替换色马依赖性

- 在设置中添加复选框

- 支持更多颜色代码

- 颜色和粗体/粗体的基本支持

- 固定设置工具[\#50](https://github.com/ros-visualization/rqt_console/issues/50))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、阿尔内·希茨曼、彼得、莫斯费特80、彼得

<span id="rqt-graph"></span>

## [rqt_graph](https://github.com/ros-visualization/rqt_graph/tree/lyrical/CHANGELOG.rst)

- 在 qt6 (后端端口) 支持轮事件 [\#116](https://github.com/ros-visualization/rqt_graph/issues/116)) ([\#117](https://github.com/ros-visualization/rqt_graph/issues/117))

- 修补: 断绝的依赖性( E)[\#115](https://github.com/ros-visualization/rqt_graph//issues/115))

- 支持 Qt6 (帮助 )[\#114](https://github.com/ros-visualization/rqt_graph//issues/114))

- 添加类型不兼容的警告( E)[\#105](https://github.com/ros-visualization/rqt_graph/issues/105))

- 删除 rqt_图脚本 。 ()[\#66](https://github.com/ros-visualization/rqt_graph/issues/66))

- 固定设置工具[\#107](https://github.com/ros-visualization/rqt_graph/issues/107))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,克里斯·拉朗谢特,约纳斯·奥托,马修·福伦, megify\[bot\], mosfet80

<span id="rqt-gui"></span>

## [rqt_gui](https://github.com/ros-visualization/rqt/tree/lyrical/rqt_gui/CHANGELOG.rst)

- 固定装置 工具贬值( U)[\#322](https://github.com/ros-visualization/rqt/issues/322))

- 贡献者:苔藓80

<span id="rqt-gui-cpp"></span>

## [rqt_gui_cpp](https://github.com/ros-visualization/rqt/tree/lyrical/rqt_gui_cpp/CHANGELOG.rst)

- 清理信头( E)[\#347](https://github.com/ros-visualization/rqt/issues/347)) ([\#350](https://github.com/ros-visualization/rqt/issues/350))

- 使用 qt- base- dev / libqtwidgets ([\#345](https://github.com/ros-visualization/rqt/issues/345))

- 修补:包括未列出的(fetid)h[\#341](https://github.com/ros-visualization/rqt/issues/341))

- 支持 Qt6 (帮助 )[\#339](https://github.com/ros-visualization/rqt/issues/339))

- 删除的已贬值信头( E)[\#340](https://github.com/ros-visualization/rqt/issues/340))

- 使用 qt6 作为来自 rosdep 的默认依赖( )[\#337](https://github.com/ros-visualization/rqt/issues/337))

- 以 qt6 编译的固定[\#321](https://github.com/ros-visualization/rqt/issues/321))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、西松大介、谢恩·洛雷兹、增能\[bot\]、苔藓80

<span id="rqt-gui-py"></span>

## [rqt_gui_py](https://github.com/ros-visualization/rqt/tree/lyrical/rqt_gui_py/CHANGELOG.rst)

- 支持 Qt6 (帮助 )[\#339](https://github.com/ros-visualization/rqt/issues/339))

- 固定装置 工具贬值( U)[\#322](https://github.com/ros-visualization/rqt/issues/322))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,苔藓80

<span id="rqt-msg"></span>

## [rqt_msg](https://github.com/ros-visualization/rqt_msg/tree/lyrical/CHANGELOG.rst)

- 支持 Qt6 (帮助 )[\#27](https://github.com/ros-visualization/rqt_msg/issues/27))

- 固定设置工具[\#23](https://github.com/ros-visualization/rqt_msg/issues/23))

- 删除 CODEOWINERS ()[\#20](https://github.com/ros-visualization/rqt_msg/issues/20))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,苔藓80

<span id="rqt-plot"></span>

## [rqt_plot](https://github.com/ros-visualization/rqt_plot/tree/lyrical/CHANGELOG.rst)

- 使用 qt- base- dev / libqtwidgets ([\#128](https://github.com/ros-visualization/rqt_plot/issues/128))

- 支持 Qt6 (帮助 )[\#127](https://github.com/ros-visualization/rqt_plot/issues/127))

- 固定设置工具[\#123](https://github.com/ros-visualization/rqt_plot/issues/123))

- 新增缺失的测试依赖性( E)[\#118](https://github.com/ros-visualization/rqt_plot/issues/118))

- 用于显示常数曲线的修饰( E)[\#114](https://github.com/ros-visualization/rqt_plot/issues/114))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、马丁·佩卡、谢恩·洛雷茨、莫斯费特80

<span id="rqt-publisher"></span>

## [rqt_publisher](https://github.com/ros-visualization/rqt_publisher/tree/lyrical/CHANGELOG.rst)

- 固定 Flake8 (后端端口) [\#57](https://github.com/ros-visualization/rqt_publisher/issues/57)) ([\#58](https://github.com/ros-visualization/rqt_publisher/issues/58))

- 支持 Qt6 (帮助 )[\#56](https://github.com/ros-visualization/rqt_publisher/issues/56))

- 固定设置工具[\#52](https://github.com/ros-visualization/rqt_publisher/issues/52))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,注音\[bot\],mosfet80

<span id="rqt-py-common"></span>

## [rqt_py_common](https://github.com/ros-visualization/rqt/tree/lyrical/rqt_py_common/CHANGELOG.rst)

- 清理信头( E)[\#347](https://github.com/ros-visualization/rqt/issues/347)) ([\#350](https://github.com/ros-visualization/rqt/issues/350))

- 使用 qt- base- dev / libqtwidgets ([\#345](https://github.com/ros-visualization/rqt/issues/345))

- 支持 Qt6 (帮助 )[\#339](https://github.com/ros-visualization/rqt/issues/339))

- 以 qt6 编译的固定[\#321](https://github.com/ros-visualization/rqt/issues/321))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,谢恩·洛雷茨,注音\[bot\],苔藓80

<span id="rqt-py-console"></span>

## [rqt_py_console](https://github.com/ros-visualization/rqt_py_console/tree/lyrical/CHANGELOG.rst)

- 添加 Qt6 兼容性 ([\#25](https://github.com/ros-visualization/rqt_py_console/issues/25))合著:亚历杭德罗·赫尔南德斯·科尔德罗 \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 固定设置工具[\#21](https://github.com/ros-visualization/rqt_py_console/issues/21))

- 贡献者:谢恩·洛雷茨、苔藓80

<span id="rqt-reconfigure"></span>

## [rqt_reconfigure](https://github.com/ros-visualization/rqt_reconfigure/tree/lyrical/CHANGELOG.rst)

- 支持 Qt6 (帮助 )[\#158](https://github.com/ros-visualization/rqt_reconfigure/issues/158))

- 如果双值或限制是无限的,则哈登行为([\#161](https://github.com/ros-visualization/rqt_reconfigure/issues/161))

- 如果范围超过 int32 , 缩放整数编辑器( E)[\#160](https://github.com/ros-visualization/rqt_reconfigure/issues/160))

- 忽略 A005 对于未来的片段 8 ()[\#159](https://github.com/ros-visualization/rqt_reconfigure/issues/159))

- 清理错误标签的 BSD 许可证( Name[\#157](https://github.com/ros-visualization/rqt_reconfigure/issues/157))

- 固定设置工具 折旧( )[\#153](https://github.com/ros-visualization/rqt_reconfigure/issues/153))

- 如果更新远程失败, 请在本地反映失败( I)[\#144](https://github.com/ros-visualization/rqt_reconfigure/issues/144))

- 删除 CODEOWINERS ()[\#147](https://github.com/ros-visualization/rqt_reconfigure/issues/147))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,克里斯托夫·弗勒赫利希,乔纳森·塞林,迈克尔·卡尔斯特罗姆,摩斯费特80

<span id="rqt-service-caller"></span>

## [rqt_service_caller](https://github.com/ros-visualization/rqt_service_caller/tree/lyrical/CHANGELOG.rst)

- 支持 Qt6 (帮助 )[\#38](https://github.com/ros-visualization/rqt_service_caller/issues/38))

- 固定设置工具[\#33](https://github.com/ros-visualization/rqt_service_caller/issues/33))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,苔藓80

<span id="rqt-shell"></span>

## [rqt_shell](https://github.com/ros-visualization/rqt_shell/tree/lyrical/CHANGELOG.rst)

- 使木工快乐

- 固定设置工具折旧( E)[\#26](https://github.com/ros-visualization/rqt_shell/issues/26))

- 贡献者:亚历杭德罗·赫尔南德斯·科尔德罗,苔藓80

<span id="rqt-srv"></span>

## [rqt_srv](https://github.com/ros-visualization/rqt_srv/tree/lyrical/CHANGELOG.rst)

- 固定设置工具[\#16](https://github.com/ros-visualization/rqt_srv/issues/16))

- 删除 CODEOWINERS ()[\#13](https://github.com/ros-visualization/rqt_srv/issues/13))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,苔藓80

<span id="rqt-topic"></span>

## [rqt_topic](https://github.com/ros-visualization/rqt_topic/tree/lyrical/CHANGELOG.rst)

- 支持 Qt6 (帮助 )[\#67](https://github.com/ros-visualization/rqt_topic/issues/67))

- 添加 Qt6 兼容性 ([\#66](https://github.com/ros-visualization/rqt_topic/issues/66))

- Pydantic v2 compat 测试中预计发生微调错误([\#65](https://github.com/ros-visualization/rqt_topic/issues/65))

- 从 ros2 主题回声使用选择_qos() ()[\#55](https://github.com/ros-visualization/rqt_topic/issues/55))

- 启用 Flake8 ([\#58](https://github.com/ros-visualization/rqt_topic/issues/58))

- rqt_主题的开源重写( R)[\#47](https://github.com/ros-visualization/rqt_topic/issues/47))合著:埃文·弗林 \<[evan.flynn@apex.ai](mailto:evan.flynn%40apex.ai)\> 由亚历杭德罗·赫尔南德斯·科尔德罗合著[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 固定设置工具[\#57](https://github.com/ros-visualization/rqt_topic/issues/57))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、埃文·弗林、罗曼·雷尼尔、斯科特·K·洛根、谢恩·洛雷茨、莫斯费特80

<span id="rti-connext-dds-cmake-module"></span>

## [rti_connext_dds_cmake_module](https://github.com/ros2/rmw_connextdds/tree/lyrical/rti_connext_dds_cmake_module/CHANGELOG.rst)

- 将 Connext 从 7.3. 0 更新到 7. 7. 0 , 默认禁用监视库, 并使用同步发布模式([\#219](https://github.com/ros2/rmw_connextdds/issues/219))

- 修补 cmake 折旧( E)[\#198](https://github.com/ros2/rmw_connextdds/issues/198))

- 贡献者:弗朗西斯科·加莱戈·萨利多(Francisco Gallego Salido),苔藓(mosfet80)

<span id="rttest"></span>

## [测试](https://github.com/ros2/realtime_support/tree/lyrical/rttest/CHANGELOG.rst)

- 清理并删除已删除的已死代码( E)[\#141](https://github.com/ros2/realtime_support/issues/141)) ([\#144](https://github.com/ros2/realtime_support/issues/144))

- 修补 cmake 折旧( E)[\#134](https://github.com/ros2/realtime_support/issues/134))

- 贡献者:加热\[bot\],苔藓(mosfet80)

<span id="rviz2"></span>

## [rviz2 (中文(简体) ).](https://github.com/ros2/rviz/tree/lyrical/rviz2/CHANGELOG.rst)

- 使用按平台选择 Qt5 或 Qt6 的 rosdep 密钥([\#1720](https://github.com/ros2/rviz/issues/1720))

- 使用新的 ROSIDL 聚合 CMake 目标([\#1688](https://github.com/ros2/rviz/issues/1688))

- 安装 Qt5 和 Qt6 时将 Qt 版本分辨率固定下来 - CMake 默认为升解, 需要 Qt6 时将找到 Qt5 (Rolling, L- Turtle, 及 external) 。 ([\#1689](https://github.com/ros2/rviz/issues/1689))

- 使用 qt6 作为来自 rosdep 的默认依赖( )[\#1635](https://github.com/ros2/rviz/issues/1635))

- 清除已腐烂的 rclp::spin_some() ()[\#1567](https://github.com/ros2/rviz/issues/1567))

- 功绩:既支持qt5,也支持qt6 ([\#1187](https://github.com/ros2/rviz/issues/1187))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、西松大介、爱默生·克纳普、内森·布鲁克斯、谢恩·洛雷茨

<span id="rviz-common"></span>

## [rviz_common](https://github.com/ros2/rviz/tree/lyrical/rviz_common/CHANGELOG.rst)

- 使用按平台选择 Qt5 或 Qt6 的 rosdep 密钥([\#1720](https://github.com/ros2/rviz/issues/1720))

- 压缩图像显示( E)[\#1288](https://github.com/ros2/rviz//issues/1288))

- fix: MSVC 2022上的固定编译([\#1706](https://github.com/ros2/rviz//issues/1706))

- 删除了 Qt6 警告 ([\#1704](https://github.com/ros2/rviz/issues/1704))

- 固定调节为RHEL([\#1703](https://github.com/ros2/rviz/issues/1703))

- 删除警告( E)[\#1693](https://github.com/ros2/rviz/issues/1693))

- 链接对 `GTest::gmock` 目标([\#1699](https://github.com/ros2/rviz/issues/1699))

- 减少 `QFile` 抚养([\#1652](https://github.com/ros2/rviz/issues/1652))

- 使用新的 ROSIDL 聚合 CMake 目标([\#1688](https://github.com/ros2/rviz/issues/1688))

- 安装 Qt5 和 Qt6 时将 Qt 版本分辨率固定下来 - CMake 默认为升解, 需要 Qt6 时将找到 Qt5 (Rolling, L- Turtle, 及 external) 。 ([\#1689](https://github.com/ros2/rviz/issues/1689))

- 在 rviz\_ common 中进行清理 ([\#1686](https://github.com/ros2/rviz/issues/1686))

- 在“配置”中添加浅拷贝和深拷贝测试([\#1682](https://github.com/ros2/rviz/issues/1682))

- 构建 rviz\_ common 的性能优化( S)[\#1677](https://github.com/ros2/rviz/issues/1677))

- 使用 get\_ package_share\_ path ([\#1671](https://github.com/ros2/rviz/issues/1671))

- 设置属性TreeWidget 中的隐藏回归( S)[\#1667](https://github.com/ros2/rviz/issues/1667))

- 添加新可视化时添加主题名称过滤( E)[\#1662](https://github.com/ros2/rviz/issues/1662))

- 使用 QTimer: singleShot 中的 QPointer 来防止无使用([\#1657](https://github.com/ros2/rviz/issues/1657))

- 由于软件包路径不正确而未加载插件 (F)[\#1651](https://github.com/ros2/rviz/issues/1651))

- 更新已贬值的 ament_index_cpp API ([\#1647](https://github.com/ros2/rviz/issues/1647))

- 在没有工具的情况下修复崩溃( F)[\#1639](https://github.com/ros2/rviz/issues/1639))

- 使用 qt6 作为来自 rosdep 的默认依赖( )[\#1635](https://github.com/ros2/rviz/issues/1635))

- Pointcloud2 显示集 QoS 以尽最大努力([\#1621](https://github.com/ros2/rviz/issues/1621))

- 清理折旧代码( E) :[\#1619](https://github.com/ros2/rviz//issues/1619))

- 移除了对 Yaml- cpp 低于 0.5 的支持 ([\#1605](https://github.com/ros2/rviz//issues/1605))

- 删除重复的转发类声明( E)[\#1602](https://github.com/ros2/rviz//issues/1602))

- 在可视化管理器中解析 TODO([\#1603](https://github.com/ros2/rviz//issues/1603))

- 在组合框中修正错误的 Qt 信号连接( S)[\#1596](https://github.com/ros2/rviz/issues/1596))

- 已删除的微小xml2\_ vendor 依赖性([\#1591](https://github.com/ros2/rviz/issues/1591))

- 将 QRegExp 替换为 QRegularExpression 支持 Qt6 ()[\#1592](https://github.com/ros2/rviz/issues/1592))

- 修复崩溃( E)[\#1587](https://github.com/ros2/rviz/issues/1587))

- 添加更改文件模式的选项( E)[\#1537](https://github.com/ros2/rviz/issues/1537))

- 已删除 tf2 中的折旧警告( U) :[\#1585](https://github.com/ros2/rviz/issues/1585))

- 默认插件中的 Std chrono 更新( S)[\#1579](https://github.com/ros2/rviz/issues/1579))

- 已删除的折旧( U)[\#1556](https://github.com/ros2/rviz/issues/1556))

- rviz 通用 ros 服务财产([\#1548](https://github.com/ros2/rviz/issues/1548))

- 添加 ros 动作属性( E)[\#1549](https://github.com/ros2/rviz/issues/1549))

- 折叠更新(浮点,浮点)方法并提供更新(std:chrono::duration,std:chrono::duration)替换. (std:chrono::duration).[\#1533](https://github.com/ros2/rviz//issues/1533))

- 替换已折旧的 tf2\_ ros 信头([\#1529](https://github.com/ros2/rviz/issues/1529))

- 推迟隐藏属性,直到插入模型完成( )[\#1508](https://github.com/ros2/rviz/issues/1508))

- 不要隐藏模型中以外的属性行( Q)[\#1507](https://github.com/ros2/rviz/issues/1507))

- 删除冗余检查( E)[\#1506](https://github.com/ros2/rviz/issues/1506))

- 固定面板删除( E)[\#1037](https://github.com/ros2/rviz/issues/1037))

- 配置: mapGetBool 当值\_ 输出为无效时, 会导致分割错( mapGetBool) 。[\#1471](https://github.com/ros2/rviz/issues/1471))

- 功绩:既支持qt5,也支持qt6 ([\#1187](https://github.com/ros2/rviz/issues/1187))

- 资源不可用时的固定崩溃( E)[\#1455](https://github.com/ros2/rviz/issues/1455))

- 正在使用新的资源检索器 apis 进行中([\#1262](https://github.com/ros2/rviz/issues/1262))

- 添加跟踪对象函数失败到处理 Null 指针, 当无效程序通过时导致崩溃([\#1375](https://github.com/ros2/rviz/issues/1375))

- 添加测试以检查缺少密钥时的映射GetString([\#1361](https://github.com/ros2/rviz/issues/1361))

- 统一样式: parseFloat 失败, 无法正确处理无效的浮点格式( S) :[\#1360](https://github.com/ros2/rviz/issues/1360))

- 在VisualizerApp中修正潜在的 Null 指针删除: getRender Window () 防止崩溃([\#1359](https://github.com/ros2/rviz/issues/1359))

- 扩展对类型适应的支持( REP 2007) rviz\_ 常见的 TF 过滤显示( )[\#1346](https://github.com/ros2/rviz/issues/1346))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、西松大介、大卫·V·卢!!!,爱默生·克纳普、亚诺施·马霍温斯基、约书亚·苏普拉特曼、马克·约翰逊、马特乌什·查克、马特奥·普林西格、马修·福伦、迈克尔·卡罗尔、内森·布鲁克斯、奥斯莫阿尔07、帕特里克·龙卡廖洛、谢恩·洛雷兹、小1235、内尔森、特0k0shi。

<span id="rviz-default-plugins"></span>

## [rviz_default_plugins](https://github.com/ros2/rviz/tree/lyrical/rviz_default_plugins/CHANGELOG.rst)

- 使用按平台选择 Qt5 或 Qt6 的 rosdep 密钥([\#1720](https://github.com/ros2/rviz/issues/1720))

- 压缩图像显示( E)[\#1288](https://github.com/ros2/rviz//issues/1288))

- 删除了 Qt6 警告 ([\#1704](https://github.com/ros2/rviz/issues/1704))

- 切换 rviz 服务资源检索器以使用新的 repo 代码([\#1698](https://github.com/ros2/rviz/issues/1698))

- 链接对 `GTest::gmock` 目标([\#1699](https://github.com/ros2/rviz/issues/1699))

- 改进通用标记( E)[\#1687](https://github.com/ros2/rviz/issues/1687))

- 减少 `QFile` 抚养([\#1652](https://github.com/ros2/rviz/issues/1652))

- 使用新的 ROSIDL 聚合 CMake 目标([\#1688](https://github.com/ros2/rviz/issues/1688))

- 安装 Qt5 和 Qt6 时将 Qt 版本分辨率固定下来 - CMake 默认为升解, 需要 Qt6 时将找到 Qt5 (Rolling, L- Turtle, 及 external) 。 ([\#1689](https://github.com/ros2/rviz/issues/1689))

- 删除测试固定装置的冗余编译( E)[\#1673](https://github.com/ros2/rviz/issues/1673))

- 更新已贬值的 ament_index_cpp API ([\#1647](https://github.com/ros2/rviz/issues/1647))

- 将相机Info 专题属性添加到深度CloudDisplay([\#1643](https://github.com/ros2/rviz/issues/1643))

- 使用 qt6 作为来自 rosdep 的默认依赖( )[\#1635](https://github.com/ros2/rviz/issues/1635))

- Pointcloud2 显示集 QoS 以尽最大努力([\#1621](https://github.com/ros2/rviz/issues/1621))

- 修正 XYOrbitView 控制器中的翻译问题( S)[\#1630](https://github.com/ros2/rviz/issues/1630))

- 超过16384尺寸限制([\#1622](https://github.com/ros2/rviz/issues/1622))

- 已删除的 TODO ()[\#1604](https://github.com/ros2/rviz//issues/1604))

- 第1593号固定问题[\#1598](https://github.com/ros2/rviz/issues/1598))

- 删除 tf2 警告( R)[\#1586](https://github.com/ros2/rviz/issues/1586))

- 已删除 tf2 中的折旧警告( U) :[\#1585](https://github.com/ros2/rviz/issues/1585))

- 默认插件中的 Std chrono 更新( S)[\#1579](https://github.com/ros2/rviz/issues/1579))

- 将点云2 显示除以 0 ()[\#1581](https://github.com/ros2/rviz/issues/1581))

- 添加对 ffmpeg_image_transport 和 point_cloud_transport 的支持([\#1568](https://github.com/ros2/rviz/issues/1568))

- 扩展点云2显示的信息过滤器显示( E)[\#1566](https://github.com/ros2/rviz/issues/1566))

- 支持图像传输生命周期( I)[\#1472](https://github.com/ros2/rviz//issues/1472))

- 从 rviz 配置文件中修复初始PoseTool 的 QoS 配置加载([\#1544](https://github.com/ros2/rviz//issues/1544))

- 将 rmw_qos\_ profile_t 替换为 rclcpp:: QoS ()[\#1525](https://github.com/ros2/rviz/issues/1525))

- 替换已折旧的 tf2\_ ros 信头([\#1529](https://github.com/ros2/rviz/issues/1529))

- 已折旧的固定设备包括([\#1530](https://github.com/ros2/rviz/issues/1530))

- 点\_ cloud\_ transport 更新 API 调用([\#1526](https://github.com/ros2/rviz/issues/1526))

- 更好地处理缺失的运输插件( E)[\#1488](https://github.com/ros2/rviz/issues/1488))

- 点\_ cloud_transport: rmw_qos_profile_t上的固定折旧警告(简体中文)[\#1491](https://github.com/ros2/rviz/issues/1491))

- 添加符号可见度宏以制作\*Palette 公共功能([\#1492](https://github.com/ros2/rviz/issues/1492))

- 修复/rviz/get_资源([\#1487](https://github.com/ros2/rviz/issues/1487))

- 删除的点\_ cloud\_ transport 折价 ()[\#1474](https://github.com/ros2/rviz/issues/1474))

- 框架视图控制器: 删除警告( E)[\#1470](https://github.com/ros2/rviz/issues/1470))

- 以 qt6 修复编译( S)[\#1475](https://github.com/ros2/rviz/issues/1475))

- 与 Quarternion 角距离 问题 ([\#1473](https://github.com/ros2/rviz/issues/1473))

- 点刻本 : 如果禁用, 忽略已发送的信件( Q)[\#1036](https://github.com/ros2/rviz/issues/1036))

- 从 resouce 检索器中删除了未使用的信头( Q)[\#1463](https://github.com/ros2/rviz/issues/1463))

- 功绩:既支持qt5,也支持qt6 ([\#1187](https://github.com/ros2/rviz/issues/1187))

- \[rviz_default_plugins\] 添加缺少的导出依赖性([\#1461](https://github.com/ros2/rviz/issues/1461))

- 后传框架对齐相机( S)[\#1453](https://github.com/ros2/rviz/issues/1453))

- 更改了标记显示器, 允许移动命名空间的能见度( C)[\#1402](https://github.com/ros2/rviz/issues/1402))

- 不使用 \$ ⁇ t5 Widgets_INCLUDE_DIRS } 来避免创建不可移动的 CMake 配置文件([\#1450](https://github.com/ros2/rviz/issues/1450))

- PointCloudDisplay: 修正衰变时间 0 保存多于上一封信件([\#1400](https://github.com/ros2/rviz/issues/1400))

- 正在使用新的资源检索器 apis 进行中([\#1262](https://github.com/ros2/rviz/issues/1262))

- 包含 chrono ([\#1353](https://github.com/ros2/rviz/issues/1353))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、阿列克西斯·措吉亚斯、安东尼奥·布兰迪、尼希马松大介、埃沙·库马尔、埃默森·克纳普、菲利克斯·埃克纳(弗纳)、格奥尔格·弗利克、纪尧姆·多伊西、哈里森·陈、布拉梅尔德(TRACLabs)、竹内康介、伦纳特·雷希尔、马克·约翰逊、马修·福伦、迈克尔·卡罗尔、内森·布鲁克斯、谢恩·洛雷茨、西尔维奥·特拉韦萨罗、斯特凡·法比安、斯托扬·盖达罗夫、摩斯费特80

<span id="rviz-ogre-vendor"></span>

## [rviz_ogre_vendor](https://github.com/ros2/rviz/tree/lyrical/rviz_ogre_vendor/CHANGELOG.rst)

- 添加补丁以删除 `binary_function` ([\#1691](https://github.com/ros2/rviz/issues/1691))

- 对 rviz\_ ogre\_ vendor 的 ump CMake 版本并抑制警告( )[\#1684](https://github.com/ros2/rviz/issues/1684))

- 删除Windows上的销售商免费类型和zlib([\#1636](https://github.com/ros2/rviz/issues/1636))

- 添加 RVIZ_OGRE_VENDOR_MANGLE_NAME_OF_LIBRARIES_USED_BY_RVIZ 选项来进一步管理 rviz 使用的 ogre 库([\#1493](https://github.com/ros2/rviz/issues/1493))

- 添加缺少的食人怪供应商包的 glew 依赖性( Q)[\#1350](https://github.com/ros2/rviz/issues/1350))

- 贡献者:Dhruv Patel、Michael Carroll、Shane Loretz、Silvio Traversaro、Stefan Fabian

<span id="rviz-rendering"></span>

## [rviz_rendering](https://github.com/ros2/rviz/tree/lyrical/rviz_rendering/CHANGELOG.rst)

- 使用按平台选择 Qt5 或 Qt6 的 rosdep 密钥([\#1720](https://github.com/ros2/rviz/issues/1720))

- 用于 Ubuntu 26 ([\#1694](https://github.com/ros2/rviz/issues/1694))

- 安装 Qt5 和 Qt6 时将 Qt 版本分辨率固定下来 - CMake 默认为升解, 需要 Qt6 时将找到 Qt5 (Rolling, L- Turtle, 及 external) 。 ([\#1689](https://github.com/ros2/rviz/issues/1689))

- 更新已贬值的 ament_index_cpp API ([\#1647](https://github.com/ros2/rviz/issues/1647))

- 使用 qt6 作为来自 rosdep 的默认依赖( )[\#1635](https://github.com/ros2/rviz/issues/1635))

- 删除未使用的文件( E)[\#1600](https://github.com/ros2/rviz//issues/1600))

- 已删除的垃圾供应商套件( U)[\#1574](https://github.com/ros2/rviz/issues/1574))

- 从 ROS1 RViz 更新 OGRE 网格文件([\#1536](https://github.com/ros2/rviz//issues/1536)) ([\#1559](https://github.com/ros2/rviz//issues/1559))

- 添加资源Exists 检查在加载纹理前装入 EmbededTexture ()[\#1542](https://github.com/ros2/rviz//issues/1542))

- 将几何学指定为资源组“rviz_relanding”([\#1502](https://github.com/ros2/rviz/issues/1502))

- 删除窗口警告( R)[\#1486](https://github.com/ros2/rviz/issues/1486))

- 处理 glTF Y- Up 框架关于网格负载的常规([\#1482](https://github.com/ros2/rviz/issues/1482))

- 从 resouce 检索器中删除了未使用的信头( Q)[\#1463](https://github.com/ros2/rviz/issues/1463))

- 功绩:既支持qt5,也支持qt6 ([\#1187](https://github.com/ros2/rviz/issues/1187))

- 扳手 : setForce Cororor 和 set Torque Coror 夹子值 ([\#1437](https://github.com/ros2/rviz/issues/1437))

- 缺少 null 指针 检查三角形 Polygon 建構器 铅到崩溃 ([\#1434](https://github.com/ros2/rviz/issues/1434))

- BillboardLine:: addPoint () 超过最大点时不会丢出例外( per\_ line limit) ([\#1436](https://github.com/ros2/rviz/issues/1436))

- 构造器 ScrewVisual :: ScrewVisual 不处理无指针,导致崩溃([\#1435](https://github.com/ros2/rviz/issues/1435))

- 删除了 Windows 警告( E)[\#1413](https://github.com/ros2/rviz/issues/1413))

- 处理拆分中的空字符串时发生内存访问错误 。[\#1412](https://github.com/ros2/rviz/issues/1412))

- 参数EventsFilter 构造器中的无手努尔指针造成的崩溃([\#1411](https://github.com/ros2/rviz/issues/1411))

- 移动Text 构造器不会验证无效字符高度, 默认的倒置缺失( Name[\#1398](https://github.com/ros2/rviz/issues/1398))

- 无效参数处理: 共变视觉构造器( Covariance Visual):[\#1396](https://github.com/ros2/rviz/issues/1396))

- 尝试视觉中无效参数缺乏有效性检查: 努力视觉构造器( E)[\#1395](https://github.com/ros2/rviz/issues/1395))

- 网格类构造器不处理 Null 指针, 导致程序崩溃( M)[\#1394](https://github.com/ros2/rviz/issues/1394))

- 在 MovebleText 中崩溃 :: update () 当标题由于未初始化的资源使用而成为空字符串时( )[\#1393](https://github.com/ros2/rviz/issues/1393))

- 正在使用新的资源检索器 apis 进行中([\#1262](https://github.com/ros2/rviz/issues/1262))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、西松大介、约翰·TGZ、迈克尔·卡尔斯特罗姆、迈克尔·卡罗尔、米歇尔·伊达尔戈、内森·布鲁克斯、谢恩·洛雷茨、马提亚斯88、莫吉利特\[标 、苔藓80

<span id="rviz-rendering-tests"></span>

## [rviz_rendering_tests](https://github.com/ros2/rviz/tree/lyrical/rviz_rendering_tests/CHANGELOG.rst)

- 使用按平台选择 Qt5 或 Qt6 的 rosdep 密钥([\#1720](https://github.com/ros2/rviz/issues/1720))

- 安装 Qt5 和 Qt6 时将 Qt 版本分辨率固定下来 - CMake 默认为升解, 需要 Qt6 时将找到 Qt5 (Rolling, L- Turtle, 及 external) 。 ([\#1689](https://github.com/ros2/rviz/issues/1689))

- 更新已贬值的 ament_index_cpp API ([\#1647](https://github.com/ros2/rviz/issues/1647))

- 使用 qt6 作为来自 rosdep 的默认依赖( )[\#1635](https://github.com/ros2/rviz/issues/1635))

- 功绩:既支持qt5,也支持qt6 ([\#1187](https://github.com/ros2/rviz/issues/1187))

- 正在使用新的资源检索器 apis 进行中([\#1262](https://github.com/ros2/rviz/issues/1262))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、西松大介、迈克尔·卡罗尔、内森·布鲁克斯、谢恩·洛雷茨

<span id="rviz-visual-testing-framework"></span>

## [rviz_visual_testing_framework](https://github.com/ros2/rviz/tree/lyrical/rviz_visual_testing_framework/CHANGELOG.rst)

- 使用按平台选择 Qt5 或 Qt6 的 rosdep 密钥([\#1720](https://github.com/ros2/rviz/issues/1720))

- 使用新的 ROSIDL 聚合 CMake 目标([\#1688](https://github.com/ros2/rviz/issues/1688))

- 安装 Qt5 和 Qt6 时将 Qt 版本分辨率固定下来 - CMake 默认为升解, 需要 Qt6 时将找到 Qt5 (Rolling, L- Turtle, 及 external) 。 ([\#1689](https://github.com/ros2/rviz/issues/1689))

- 使用 get\_ package_share\_ path ([\#1671](https://github.com/ros2/rviz/issues/1671))

- 更新 ament_index_cpp API ([\#1649](https://github.com/ros2/rviz/issues/1649))

- 使用 qt6 作为来自 rosdep 的默认依赖( )[\#1635](https://github.com/ros2/rviz/issues/1635))

- 已删除 tf2 中的折旧警告( U) :[\#1585](https://github.com/ros2/rviz/issues/1585))

- 替换已折旧的 tf2\_ ros 信头([\#1529](https://github.com/ros2/rviz/issues/1529))

- 功绩:既支持qt5,也支持qt6 ([\#1187](https://github.com/ros2/rviz/issues/1187))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、西松大介、爱默生·克纳普、内森·布鲁克斯、谢恩·洛雷茨

<span id="sensor-msgs"></span>

## [sensor_msgs](https://github.com/ros2/common_interfaces/tree/lyrical/sensor_msgs/CHANGELOG.rst)

- \[ADD\] 缺失点字段类型条目([\#301](https://github.com/ros2/common_interfaces/issues/301))

- 更新点\_ cloud2_iterator.hpp (中文(简体) ).[\#298](https://github.com/ros2/common_interfaces/issues/298))

- 修复 CMAKE 折旧[\#288](https://github.com/ros2/common_interfaces/issues/288))

- 增强传感器_msgs:image_encodings中的NV12和NV21支持([\#264](https://github.com/ros2/common_interfaces/issues/264))

- 贡献者:亚当·李珀,赵尧成,苔丝菲特80,沃特科

<span id="sensor-msgs-py"></span>

## [sensor_msgs_py](https://github.com/ros2/common_interfaces/tree/lyrical/sensor_msgs_py/CHANGELOG.rst)

- 使用结构化 NumPy point.dtype. item size 作为创建\_ cloud 中的默认点\_ step( )[\#295](https://github.com/ros2/common_interfaces/issues/295))

- 固定设置工具 折旧( )[\#293](https://github.com/ros2/common_interfaces/issues/293))

- 贡献者:苔藓80, xndcn

<span id="service-msgs"></span>

## [service_msgs](https://github.com/ros2/rcl_interfaces/tree/lyrical/service_msgs/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#180](https://github.com/ros2/rcl_interfaces/issues/180))

- 贡献者:苔藓80

<span id="shape-msgs"></span>

## [shape_msgs](https://github.com/ros2/common_interfaces/tree/lyrical/shape_msgs/CHANGELOG.rst)

- 修复 CMAKE 折旧[\#288](https://github.com/ros2/common_interfaces/issues/288))

- 贡献者:苔藓80

<span id="spdlog-vendor"></span>

## [spdlog_vendor](https://github.com/ros2/spdlog_vendor/tree/lyrical/CHANGELOG.rst)

- 删除 CODEOWINERS 和 镜像滚动到主机 。 ([\#38](https://github.com/ros2/spdlog_vendor/issues/38))

- 撰稿人:克里斯·拉兰谢特

<span id="sros2"></span>

## [斜线2](https://github.com/ros2/sros2/tree/lyrical/sros2/CHANGELOG.rst)

- 修补 `load_file_into_BIO: File could not be found, opened or is empty` Windows上的错误 ([\#386](https://github.com/ros2/sros2/issues/386)) ([\#387](https://github.com/ros2/sros2/issues/387))

- 缺少python3-pytest-time的测试依赖性。 ([\#377](https://github.com/ros2/sros2/issues/377))

- 清理孤立的 ros2 守护进程进行测试。 ()[\#375](https://github.com/ros2/sros2/issues/375))

- 删除导入lib( R)[\#368](https://github.com/ros2/sros2/issues/368))

- 时区意识到日期时间 + 删除黑客 [\#209](https://github.com/ros2/sros2/issues/209) ([\#300](https://github.com/ros2/sros2/issues/300))

- 固定设置工具[\#357](https://github.com/ros2/sros2/issues/357))

- 使用rmw_test_fixture来隔离ros2cli测试([\#356](https://github.com/ros2/sros2/issues/356))

- 更新公用设施,以通过非EC.SECP256R1级实例([\#352](https://github.com/ros2/sros2/issues/352))

- 压制多纹警告。 ()[\#346](https://github.com/ros2/sros2/issues/346))

- 切换为获取_rmw_附加_env([\#339](https://github.com/ros2/sros2/issues/339))

- 修正 github- workflow mypy 错误([\#336](https://github.com/ros2/sros2/issues/336))

- 贡献者:迈克尔·卡尔斯特罗姆,米卡埃尔·阿尔盖达斯,斯科特·K·洛根,藤田丰也,cdisco,加热\[bot\],mosfet80,yadund

<span id="sros2-cmake"></span>

## [sros2_cmake](https://github.com/ros2/sros2/tree/lyrical/sros2_cmake/CHANGELOG.rst)

- 更新 CMakeLists.txt (英语).[\#344](https://github.com/ros2/sros2/issues/344))

- 贡献者:苔藓80

<span id="statistics-msgs"></span>

## [statistics_msgs](https://github.com/ros2/rcl_interfaces/tree/lyrical/statistics_msgs/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#180](https://github.com/ros2/rcl_interfaces/issues/180))

- 贡献者:苔藓80

<span id="std-msgs"></span>

## [std_msgs](https://github.com/ros2/common_interfaces/tree/lyrical/std_msgs/CHANGELOG.rst)

- 修复 CMAKE 折旧[\#288](https://github.com/ros2/common_interfaces/issues/288))

- 贡献者:苔藓80

<span id="std-srvs"></span>

## [std_srvs](https://github.com/ros2/common_interfaces/tree/lyrical/std_srvs/CHANGELOG.rst)

- 修复 CMAKE 折旧[\#288](https://github.com/ros2/common_interfaces/issues/288))

- 贡献者:苔藓80

<span id="stereo-msgs"></span>

## [stereo_msgs](https://github.com/ros2/common_interfaces/tree/lyrical/stereo_msgs/CHANGELOG.rst)

- 修复 CMAKE 折旧[\#288](https://github.com/ros2/common_interfaces/issues/288))

- 贡献者:苔藓80

<span id="tango-icons-vendor"></span>

## [tango_icons_vendor](https://github.com/ros-visualization/tango_icons_vendor/tree/lyrical/CHANGELOG.rst)

- 固定cmake 折旧([\#15](https://github.com/ros-visualization/tango_icons_vendor/issues/15))

- 删除镜像滚动到主工作流程( R)[\#12](https://github.com/ros-visualization/tango_icons_vendor/issues/12))

- 删除 CODEOWINERS ()[\#11](https://github.com/ros-visualization/tango_icons_vendor/issues/11))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、莫斯费特80

<span id="test-cli"></span>

## [test_cli](https://github.com/ros2/system_tests/tree/lyrical/test_cli/CHANGELOG.rst)

- 固定 CMAKE 折旧 ([\#572](https://github.com/ros2/system_tests/issues/572))

- 贡献者:苔藓80

<span id="test-cli-remapping"></span>

## [test_cli_remapping](https://github.com/ros2/system_tests/tree/lyrical/test_cli_remapping/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#587](https://github.com/ros2/system_tests/issues/587))

- 固定 CMAKE 折旧 ([\#572](https://github.com/ros2/system_tests/issues/572))

- 贡献者: Emerson Knapp, mossfet80

<span id="test-communication"></span>

## [test_communication](https://github.com/ros2/system_tests/tree/lyrical/test_communication/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#587](https://github.com/ros2/system_tests/issues/587))

- 禁用 CircloDDS 和 FastRTPS WSTring 的互操作性检查 ()[\#586](https://github.com/ros2/system_tests/issues/586))

- 修正索引( E)[\#585](https://github.com/ros2/system_tests/issues/585))

- 更新订阅回调签名( N)[\#575](https://github.com/ros2/system_tests/issues/575))

- 清除已腐烂的 rclp:: spin_some (). (中文(简体) ).[\#574](https://github.com/ros2/system_tests//issues/574))

- 固定 CMAKE 折旧 ([\#572](https://github.com/ros2/system_tests/issues/572))

- 在发射测试中使用 EullRmwIsolation ()[\#571](https://github.com/ros2/system_tests/issues/571))

- 切换到孤立的测试固定宏( S)[\#571](https://github.com/ros2/system_tests/issues/571))

- 添加密钥类型的测试( E)[\#568](https://github.com/ros2/system_tests/issues/568))

- 删除使用ament_target_依赖性([\#566](https://github.com/ros2/system_tests/issues/566))

- 与 Zenoh 一起跳过所有多版本的 pub/ sub 测试([\#560](https://github.com/ros2/system_tests/issues/560))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,爱默生·克纳普,弗朗西斯科·加列戈·萨利多,雅诺施·麦克豪林斯基,迈克尔·卡尔斯特罗姆,斯科特·克·洛根,谢恩·洛雷茨,藤田托莫亚,微型-1235,苔藓80,雅敦德

<span id="test-interface-files"></span>

## [test_interface_files](https://github.com/ros2/test_interface_files/tree/lyrical/CHANGELOG.rst)

- 更新 CMakeLists.txt (英语).[\#26](https://github.com/ros2/test_interface_files/issues/26))

- 删除 CODEOWINERS 和镜像滚动到主工作流程 。 ([\#23](https://github.com/ros2/test_interface_files/issues/23))

- 贡献者:克里斯·拉朗谢特,苔藓80

<span id="test-launch-ros"></span>

## [test_launch_ros](https://github.com/ros2/launch_ros/tree/lyrical/test_launch_ros/CHANGELOG.rst)

- 添加新组件容器重构件的测试( E)[\#536](https://github.com/ros2/launch_ros/issues/536))

- 从 Flake8 中冲压多条进程警告 ().[\#520](https://github.com/ros2/launch_ros//issues/520))

- 正确的打字符( R)[\#524](https://github.com/ros2/launch_ros//issues/524))

- 将 PYTHONBUFFERED 设置为 1 以避免因缓冲器丢失而吊销([\#519](https://github.com/ros2/launch_ros//issues/519))

- 使 FindPackage 替换为获取运算符 / ()[\#494](https://github.com/ros2/launch_ros/issues/494))

- 曝光生命周期\_ 节点( E)[\#327](https://github.com/ros2/launch_ros/issues/327)) (附试) (.[\#482](https://github.com/ros2/launch_ros/issues/482))

- 切换 osrf_py 常用的依赖性到系统软件包([\#431](https://github.com/ros2/launch_ros/issues/431))

- 用于发射前端的 SetUssimTime([\#488](https://github.com/ros2/launch_ros/issues/488))

- 固定设置工具[\#475](https://github.com/ros2/launch_ros/issues/475))

- 修补: 装入可编解码失败, 无法正确解析通卡参数文件( L)[\#460](https://github.com/ros2/launch_ros/issues/460)) ([\#465](https://github.com/ros2/launch_ros/issues/465))

- 贡献者:奥古斯特·拉兰德,克里斯托弗·贝达德,克拉拉·贝伦森,爱默生·克纳普,埃姆雷·库鲁,贾斯珀·范布拉克尔,斯科特·K·洛根,斯凯勒·梅德罗斯,藤田丰茂雅,苔丝菲特80

<span id="test-launch-testing"></span>

## [test_launch_testing](https://github.com/ros2/launch/tree/lyrical/test_launch_testing/CHANGELOG.rst)

- CMake 折旧( R)[\#899](https://github.com/ros2/launch/issues/899))

- 允许替换路径, 而不是要求 cast to str ()[\#873](https://github.com/ros2/launch/issues/873))

- 贡献者: Emerson Knapp, mossfet80

<span id="test-msgs"></span>

## [test_msgs](https://github.com/ros2/rcl_interfaces/tree/lyrical/test_msgs/CHANGELOG.rst)

- 添加 `ament_cmake_mypy` 改为: `test_msgs` ([\#187](https://github.com/ros2/rcl_interfaces/issues/187))

- 修补 cmake 折旧( E)[\#180](https://github.com/ros2/rcl_interfaces/issues/180))

- 贡献者:迈克尔·卡尔斯特罗姆、苔藓80

<span id="test-osrf-testing-tools-cpp"></span>

## [test_osrf_testing_tools_cpp](https://github.com/osrf/osrf_testing_tools_cpp/tree/lyrical/test_osrf_testing_tools_cpp/CHANGELOG.rst)

- 固定cmake 折旧([\#94](https://github.com/osrf/osrf_testing_tools_cpp/issues/94))

- 贡献者:苔藓80

<span id="test-quality-of-service"></span>

## [test_quality_of_service](https://github.com/ros2/system_tests/tree/lyrical/test_quality_of_service/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#587](https://github.com/ros2/system_tests/issues/587))

- 固定 CMAKE 折旧 ([\#572](https://github.com/ros2/system_tests/issues/572))

- 切换到孤立的测试固定宏( S)[\#571](https://github.com/ros2/system_tests/issues/571))

- 使用rmw_event_type_is_支持跳过测试([\#563](https://github.com/ros2/system_tests/issues/563))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,爱默生·克纳普,斯科特·K·洛根,摩斯费特80

<span id="test-rclcpp"></span>

## [test_rclcpp](https://github.com/ros2/system_tests/tree/lyrical/test_rclcpp/CHANGELOG.rst)

- 在 test_rclcpp 中添加测试隔离([\#583](https://github.com/ros2/system_tests/issues/583))

- 信息信息来自另一个线程的延迟信号处理器。 ([\#576](https://github.com/ros2/system_tests/issues/576))

- 清除已腐烂的 rclp:: spin_some (). (中文(简体) ).[\#574](https://github.com/ros2/system_tests//issues/574))

- 固定 CMAKE 折旧 ([\#572](https://github.com/ros2/system_tests/issues/572))

- 在发射测试中使用 EullRmwIsolation ()[\#571](https://github.com/ros2/system_tests/issues/571))

- 确保测试核实所有产卵节点的存在([\#558](https://github.com/ros2/system_tests/issues/558))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,朱琳·埃诺赫,斯科特·K·洛根,藤田友也,柳延元,苔丝芬80

<span id="test-rmw-implementation"></span>

## [test_rmw_implementation](https://github.com/ros2/rmw_implementation/tree/lyrical/test_rmw_implementation/CHANGELOG.rst)

- 使用新的聚合rosidl目标,而不是 \_TARGETS([\#276](https://github.com/ros2/rmw_implementation/issues/276))

- 测试_rmw\_ 执行: 添加测试隔离 ([\#275](https://github.com/ros2/rmw_implementation/issues/275))

- 添加 rmw_get_clients_info_by_service, rmw_servers_clients_info_by_service(服务)[\#238](https://github.com/ros2/rmw_implementation/issues/238))

- 固定cmake 折旧([\#267](https://github.com/ros2/rmw_implementation/issues/267))

- 测试无效序列长度的去序列化失败( Q)[\#261](https://github.com/ros2/rmw_implementation/issues/261))

- 添加忽略\_ 本地\_ 出版物\_ 序列化测试 。 ([\#255](https://github.com/ros2/rmw_implementation/issues/255))

- 贡献者:亚历克西斯·佐吉亚斯、朱利安·埃诺赫、李、米格尔公司、明珠、藤田友也、苔藓80

<span id="test-ros2trace"></span>

## [test_ros2trace](https://github.com/ros2/ros2_tracing/tree/lyrical/test_ros2trace/CHANGELOG.rst)

- 暂时跳过测试\_ ros2trace 的跟踪测试( S)[\#218](https://github.com/ros2/ros2_tracing/issues/218))

- 允许创建快照会话( E)[\#195](https://github.com/ros2/ros2_tracing/issues/195))

- 只检查测试进程事件在测试 \_ 运行时间\_ 无法进行( )[\#193](https://github.com/ros2/ros2_tracing/issues/193))

- 添加运行时间追踪选择退出机制( E)[\#185](https://github.com/ros2/ros2_tracing/issues/185))

- 固定设置工具 折旧( )[\#189](https://github.com/ros2/ros2_tracing/issues/189))

- 在测试_ros2trace 测试中使用超时([\#174](https://github.com/ros2/ros2_tracing/issues/174))

- 贡献者:克里斯托弗·贝达德、米歇尔·伊达尔戈、什拉万·德瓦、莫斯费特80

<span id="test-security"></span>

## [test_security](https://github.com/ros2/system_tests/tree/lyrical/test_security/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#587](https://github.com/ros2/system_tests/issues/587))

- 固定 CMAKE 折旧 ([\#572](https://github.com/ros2/system_tests/issues/572))

- 贡献者: Emerson Knapp, mossfet80

<span id="test-tf2"></span>

## [test_tf2](https://github.com/ros2/geometry2/tree/lyrical/test_tf2/CHANGELOG.rst)

- 修补字型( E)[\#921](https://github.com/ros2/geometry2/issues/921))

- 使用新的 ROSIDL 聚合 CMake 目标([\#907](https://github.com/ros2/geometry2/issues/907))

- 用于eigen-acel及其测试的Msg([\#887](https://github.com/ros2/geometry2/issues/887))

- 将作者标记移动到文件摘要( N)[\#870](https://github.com/ros2/geometry2/issues/870))

- 将conf.py文件现代化,只包括修改过的版权,eliminati... ()[\#865](https://github.com/ros2/geometry2/issues/865))

- 添加节点界面 API 设计( E)[\#714](https://github.com/ros2/geometry2/issues/714))

- 将 tf2_ros C 改为 C++ 标题([\#805](https://github.com/ros2/geometry2/issues/805))

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 添加 `rclcpp::shutdown` ([\#762](https://github.com/ros2/geometry2/issues/762))

- 贡献者:阿利雷扎·莫艾耶迪,奥古斯特·拉兰德,爱默生·克纳普,加里·塞尔文,卢卡斯·温德兰,R·肯特·詹姆斯,柳延元,苔丝菲特80

<span id="test-tracetools"></span>

## [test_tracetools](https://github.com/ros2/ros2_tracing/tree/lyrical/test_tracetools/CHANGELOG.rst)

- 固定配制: MSVC 2022 ([\#243](https://github.com/ros2/ros2_tracing/issues/243))

- 使用新的 ROSIDL 聚合 CMake 目标([\#238](https://github.com/ros2/ros2_tracing/issues/238))

- 支持 Eclipse Trace Compass 的 ROS 2 插件所使用的复杂信件流注释的痕量点([\#233](https://github.com/ros2/ros2_tracing/issues/233))

- 更新订阅回调签名( N)[\#217](https://github.com/ros2/ros2_tracing/issues/217))

- 添加运行时间追踪选择退出机制( E)[\#185](https://github.com/ros2/ros2_tracing/issues/185))

- 更新 CMakeLists.txt (英语).[\#176](https://github.com/ros2/ros2_tracing/issues/176))

- 贡献者:爱默生·克纳普,雅诺施·麦克豪林斯基,米歇尔·伊达尔戈,拉斐尔·范·肯彭,小型-1235,苔藓80

<span id="test-tracetools-launch"></span>

## [test_tracetools_launch](https://github.com/ros2/ros2_tracing/tree/lyrical/test_tracetools_launch/CHANGELOG.rst)

- 允许创建快照会话( E)[\#195](https://github.com/ros2/ros2_tracing/issues/195))

- 固定设置工具 折旧( )[\#189](https://github.com/ros2/ros2_tracing/issues/189))

- 使可替代 xml 和 yaml 发射文件的痕量动作参数([\#188](https://github.com/ros2/ros2_tracing/issues/188))

- 使可替换的痕量动作参数([\#187](https://github.com/ros2/ros2_tracing/issues/187))

- 微量工具中的神秘度报告的地址打字问题_发射([\#184](https://github.com/ros2/ros2_tracing/issues/184))

- 贡献者:克里斯托弗·贝达德、什拉万·德瓦、莫斯费特80

<span id="tf2"></span>

## [tf2](https://github.com/ros2/geometry2/tree/lyrical/tf2/CHANGELOG.rst)

- 添加静态缓存测试( Q)[\#920](https://github.com/ros2/geometry2/issues/920))

- 以干净的基于索引的迭代取代,并避免以零除法([\#901](https://github.com/ros2/geometry2/issues/901))

- 修补字型( E)[\#921](https://github.com/ros2/geometry2/issues/921))

- 修复静态缓存 :: getData () 在空缓存中返回真值( T)[\#908](https://github.com/ros2/geometry2/issues/908))

- 使用新的 ROSIDL 聚合 CMake 目标([\#907](https://github.com/ros2/geometry2/issues/907))

- 在 tf2 中修正 CPP 样式([\#902](https://github.com/ros2/geometry2/issues/902))

- 本地变量 tf2 不再阴影 tf2: ()[\#903](https://github.com/ros2/geometry2/issues/903))

- 将 char\* 替换为 std: 字符串([\#904](https://github.com/ros2/geometry2/issues/904))

- 在缓冲点( C) 中修复误导推断时间( Q)[\#832](https://github.com/ros2/geometry2/issues/832)) ([\#896](https://github.com/ros2/geometry2/issues/896))

- 从旋转直接添加到条形四角体的静态函数([\#881](https://github.com/ros2/geometry2/issues/881))

- 在 tf2 中曝光 Doxygen 输出, 显示前 Doxygen 头页同样为 README. md () 。[\#871](https://github.com/ros2/geometry2/issues/871))

- 将作者标记移动到文件摘要( N)[\#870](https://github.com/ros2/geometry2/issues/870))

- 将conf.py文件现代化,只包括修改过的版权,eliminati... ()[\#865](https://github.com/ros2/geometry2/issues/865))

- 纠正 tf2 中的各种文档错误 ([\#857](https://github.com/ros2/geometry2/issues/857))

- 在 rosdoc2 中禁用 TAGFILES , 将命名空间 tf2 文档分离为软件包([\#856](https://github.com/ros2/geometry2/issues/856))

- 修复 REP url 地点([\#847](https://github.com/ros2/geometry2/issues/847))

- 增加零quternion正常化的明确处理方式([\#839](https://github.com/ros2/geometry2/issues/839))

- 清理 TF2 依赖性( E)[\#843](https://github.com/ros2/geometry2/issues/843))

- 向docs.ros.org添加 tf2 文档([\#671](https://github.com/ros2/geometry2/issues/671))

- 添加 RPY 之四建构器( E)[\#806](https://github.com/ros2/geometry2/issues/806))

- 默认初始化 TransformStorage 的帧\_ id\_ 和 child\_ frame\_ id\_ 用 UINT32\_ MAX ()[\#783](https://github.com/ros2/geometry2/issues/783))

- 删除已贬值的页眉 tf2([\#789](https://github.com/ros2/geometry2/issues/789))

- 添加isnan 支持( R)[\#780](https://github.com/ros2/geometry2/issues/780))

- 从Sec () 处理极端大值或小值时的函数( E) 时间跨度问题( E)[\#785](https://github.com/ros2/geometry2/issues/785))

- 在取消待变换请求时, 请不要弹出回调控( N)[\#779](https://github.com/ros2/geometry2/issues/779))

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、阿利雷扎·莫艾耶迪、安德烈亚斯、奥古斯特·拉兰德、克里斯·拉兰谢特、埃默森·克纳普、马尔库斯·巴德尔、迈克尔·卡尔斯特罗姆、帕维尔·古曾费尔德、R·肯特·詹姆斯、塞利姆·阿尔曼、西蒙·尤斯纳、蒂姆·克莱法斯、蒂莫·罗赫林、克拉姆克、摩斯费特80

<span id="tf2-bullet"></span>

## [tf2_bullet](https://github.com/ros2/geometry2/tree/lyrical/tf2_bullet/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#907](https://github.com/ros2/geometry2/issues/907))

- 将作者标记移动到文件摘要( N)[\#870](https://github.com/ros2/geometry2/issues/870))

- 将conf.py文件现代化,只包括修改过的版权,eliminati... ()[\#865](https://github.com/ros2/geometry2/issues/865))

- 在 rosdoc2 中禁用 TAGFILES , 将命名空间 tf2 文档分离为软件包([\#856](https://github.com/ros2/geometry2/issues/856))

- 设定 Cmake 政策 CMP0144 ([\#819](https://github.com/ros2/geometry2/issues/819))

- 将 tf2_ros C 改为 C++ 标题([\#805](https://github.com/ros2/geometry2/issues/805))

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 贡献者:克里斯托瓦尔·阿罗约、埃默森·克纳普、加里·塞尔文、R·肯特·詹姆斯、苔丝菲特80

<span id="tf2-eigen"></span>

## [tf2_eigen](https://github.com/ros2/geometry2/tree/lyrical/tf2_eigen/CHANGELOG.rst)

- 修补字型( E)[\#921](https://github.com/ros2/geometry2/issues/921))

- 使用新的 ROSIDL 聚合 CMake 目标([\#907](https://github.com/ros2/geometry2/issues/907))

- 用于eigen-acel及其测试的Msg([\#887](https://github.com/ros2/geometry2/issues/887))

- 将作者标记移动到文件摘要( N)[\#870](https://github.com/ros2/geometry2/issues/870))

- 将conf.py文件现代化,只包括修改过的版权,eliminati... ()[\#865](https://github.com/ros2/geometry2/issues/865))

- 从 Accel 转换为 Eigen 的 Msg 添加([\#844](https://github.com/ros2/geometry2/issues/844))

- 在 rosdoc2 中禁用 TAGFILES , 将命名空间 tf2 文档分离为软件包([\#856](https://github.com/ros2/geometry2/issues/856))

- 将 tf2_ros C 改为 C++ 标题([\#805](https://github.com/ros2/geometry2/issues/805))

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 贡献者:阿里雷扎·莫艾耶迪,奥古斯特·拉兰德,爱默生·克纳普,加里·塞尔文,R·肯特·詹姆斯,摩斯费特80

<span id="tf2-eigen-kdl"></span>

## [tf2_eigen_kdl](https://github.com/ros2/geometry2/tree/lyrical/tf2_eigen_kdl/CHANGELOG.rst)

- 将conf.py文件现代化,只包括修改过的版权,eliminati... ()[\#865](https://github.com/ros2/geometry2/issues/865))

- 在 rosdoc2 中禁用 TAGFILES , 将命名空间 tf2 文档分离为软件包([\#856](https://github.com/ros2/geometry2/issues/856))

- 清理错误标签的 BSD 许可证( Name[\#855](https://github.com/ros2/geometry2/issues/855))

- 消除对orocos kdl供应商的依赖([\#826](https://github.com/ros2/geometry2/issues/826))

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、R·肯特·詹姆斯、mossfet80

<span id="tf2-geometry-msgs"></span>

## [tf2_geometry_msgs](https://github.com/ros2/geometry2/tree/lyrical/tf2_geometry_msgs/CHANGELOG.rst)

- 修补字型( E)[\#921](https://github.com/ros2/geometry2/issues/921))

- 固定 : 变速器的变形 变形后添加输入矢量( )[\#909](https://github.com/ros2/geometry2/issues/909))

- 使用新的 ROSIDL 聚合 CMake 目标([\#907](https://github.com/ros2/geometry2/issues/907))

- 从输入复制 child_frame_id([\#889](https://github.com/ros2/geometry2/issues/889))

- 将作者标记移动到文件摘要( N)[\#870](https://github.com/ros2/geometry2/issues/870))

- 将conf.py文件现代化,只包括修改过的版权,eliminati... ()[\#865](https://github.com/ros2/geometry2/issues/865))

- 消除对orocos kdl供应商的依赖([\#826](https://github.com/ros2/geometry2/issues/826))

- 将 tf2_ros C 改为 C++ 标题([\#805](https://github.com/ros2/geometry2/issues/805))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,奥古斯特·拉兰德,爱默生·克纳普,加里·塞尔文,R·肯特·詹姆斯,扬尼克·梅因肯,克拉姆克

<span id="tf2-kdl"></span>

## [tf2_kdl](https://github.com/ros2/geometry2/tree/lyrical/tf2_kdl/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#907](https://github.com/ros2/geometry2/issues/907))

- 将作者标记移动到文件摘要( N)[\#870](https://github.com/ros2/geometry2/issues/870))

- tf2_kdl 的文档修补( T)[\#869](https://github.com/ros2/geometry2/issues/869))

- 将conf.py文件现代化,只包括修改过的版权,eliminati... ()[\#865](https://github.com/ros2/geometry2/issues/865))

- 在 rosdoc2 中禁用 TAGFILES , 将命名空间 tf2 文档分离为软件包([\#856](https://github.com/ros2/geometry2/issues/856))

- 消除对orocos kdl供应商的依赖([\#826](https://github.com/ros2/geometry2/issues/826))

- 将 tf2_ros C 改为 C++ 标题([\#805](https://github.com/ros2/geometry2/issues/805))

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 修复外部文件映射( R)[\#757](https://github.com/ros2/geometry2/issues/757))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,爱默生·克纳普,埃马纽埃尔,加里·塞尔文,R·肯特·詹姆斯,摩斯费特80

<span id="tf2-msgs"></span>

## [tf2_msgs](https://github.com/ros2/geometry2/tree/lyrical/tf2_msgs/CHANGELOG.rst)

- 修补字型( E)[\#921](https://github.com/ros2/geometry2/issues/921))

- 将conf.py文件现代化,只包括修改过的版权,eliminati... ()[\#865](https://github.com/ros2/geometry2/issues/865))

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 贡献者:奥古斯特·拉兰德、R·肯特·詹姆斯、苔丝菲特80

<span id="tf2-py"></span>

## [tf2_py](https://github.com/ros2/geometry2/tree/lyrical/tf2_py/CHANGELOG.rst)

- 修补字型( E)[\#921](https://github.com/ros2/geometry2/issues/921))

- 使用新的 ROSIDL 聚合 CMake 目标([\#907](https://github.com/ros2/geometry2/issues/907))

- 将conf.py文件现代化,只包括修改过的版权,eliminati... ()[\#865](https://github.com/ros2/geometry2/issues/865))

- 清理 TF2 依赖性( E)[\#843](https://github.com/ros2/geometry2/issues/843))

- 贡献者:奥古斯特·拉兰德,克里斯·拉兰谢特,爱默生·克纳普,R·肯特·詹姆斯

<span id="tf2-ros"></span>

## [tf2_ros](https://github.com/ros2/geometry2/tree/lyrical/tf2_ros/CHANGELOG.rst)

- 修补字型( E)[\#921](https://github.com/ros2/geometry2/issues/921))

- 使用新的 ROSIDL 聚合 CMake 目标([\#907](https://github.com/ros2/geometry2/issues/907))

- 将作者标记移动到文件摘要( N)[\#870](https://github.com/ros2/geometry2/issues/870))

- 将conf.py文件现代化,只包括修改过的版权,eliminati... ()[\#865](https://github.com/ros2/geometry2/issues/865))

- 在 rosdoc2 中禁用 TAGFILES , 将命名空间 tf2 文档分离为软件包([\#856](https://github.com/ros2/geometry2/issues/856))

- 从 tf2_ros 信件\_ filter 防止日志垃圾邮件( )[\#851](https://github.com/ros2/geometry2/issues/851))

- 更新的 tf2_echo 带有一些其他特性([\#802](https://github.com/ros2/geometry2/issues/802)) ([\#840](https://github.com/ros2/geometry2/issues/840))

- 将 std 替换为: sleep\_ for rclpp : hour: sleep\_ for ()[\#835](https://github.com/ros2/geometry2/issues/835))

- 删除的 rclcpp: spin\_ some( 节点) ([\#824](https://github.com/ros2/geometry2/issues/824))

- 添加节点界面 API 设计( E)[\#714](https://github.com/ros2/geometry2/issues/714))

- ger 摆脱已腐烂的 rclcpp:: spin_some (). (中文(简体) ).[\#821](https://github.com/ros2/geometry2/issues/821))

- 在信件\_ filter\_ test 中确保变量被视为挥发性( )[\#812](https://github.com/ros2/geometry2/issues/812))

- 将 tf2_ros C 改为 C++ 标题([\#805](https://github.com/ros2/geometry2/issues/805))

- 修正信件过滤目标框架字符串( Q)[\#803](https://github.com/ros2/geometry2/issues/803))

- 删除折旧警告( E)[\#790](https://github.com/ros2/geometry2/issues/790))

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 添加 `rclcpp::shutdown` ([\#762](https://github.com/ros2/geometry2/issues/762))

- 修复外部文件映射( R)[\#757](https://github.com/ros2/geometry2/issues/757))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,奥古斯特·拉兰德,爱默生·克纳普,埃马纽埃尔,加里·塞尔文,卢卡斯·温德兰,米尔科·费拉蒂,R·肯特·詹姆斯,谢尔盖·佐博夫,藤田友友也,柳延元,增能\[bot\],小1235,苔藓80

<span id="tf2-ros-py"></span>

## [tf2_ros_py](https://github.com/ros2/geometry2/tree/lyrical/tf2_ros_py/CHANGELOG.rst)

- 修补字型( E)[\#921](https://github.com/ros2/geometry2/issues/921))

- Flake8 修正([\#919](https://github.com/ros2/geometry2/issues/919))

- 当静态_只=真时防止属性错误( Q)[\#906](https://github.com/ros2/geometry2/issues/906))

- 在缓冲.py中固定打字([\#905](https://github.com/ros2/geometry2/issues/905))

- 提高听众和广播员测试的力度([\#894](https://github.com/ros2/geometry2/issues/894))

- 将conf.py文件现代化,只包括修改过的版权,eliminati... ()[\#865](https://github.com/ros2/geometry2/issues/865))

- 在 rosdoc2 中禁用 TAGFILES , 将命名空间 tf2 文档分离为软件包([\#856](https://github.com/ros2/geometry2/issues/856))

- 清理 TF2 依赖性( E)[\#843](https://github.com/ros2/geometry2/issues/843))

- Static TransformPublisher的C++和Python执行的固定不一致性([\#820](https://github.com/ros2/geometry2/issues/820))

- 修正折旧警告( E)[\#804](https://github.com/ros2/geometry2/issues/804))

- 删除折旧警告( E)[\#790](https://github.com/ros2/geometry2/issues/790))

- 修复外部文件映射( R)[\#757](https://github.com/ros2/geometry2/issues/757))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,奥古斯特·拉兰德,克里斯·拉朗谢特,多米尼克,埃马纽埃尔,迈克尔·卡尔斯特罗姆,迈克尔·卡罗尔,R·肯特·詹姆斯,摩斯费特80

<span id="tf2-sensor-msgs"></span>

## [tf2_sensor_msgs](https://github.com/ros2/geometry2/tree/lyrical/tf2_sensor_msgs/CHANGELOG.rst)

- 修补字型( E)[\#921](https://github.com/ros2/geometry2/issues/921))

- 使用新的 ROSIDL 聚合 CMake 目标([\#907](https://github.com/ros2/geometry2/issues/907))

- 将conf.py文件现代化,只包括修改过的版权,eliminati... ()[\#865](https://github.com/ros2/geometry2/issues/865))

- 以 tf2_sensor\_ msgs 的版权解决了 TODO ()[\#836](https://github.com/ros2/geometry2/issues/836))

- 消除对orocos kdl供应商的依赖([\#826](https://github.com/ros2/geometry2/issues/826))

- 添加 imu 和mag 支持到 `tf2_sensor_msgs` ([\#800](https://github.com/ros2/geometry2/issues/800)) ([\#813](https://github.com/ros2/geometry2/issues/813))

- 将 tf2_ros C 改为 C++ 标题([\#805](https://github.com/ros2/geometry2/issues/805))

- 在其中添加普通旋转 `PointCloud2` `doTransform` ([\#792](https://github.com/ros2/geometry2/issues/792))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、奥古斯特·拉兰德、埃默森·克纳普、加里·塞尔文、帕特里克·龙卡廖洛、R·肯特·詹姆斯

<span id="tf2-tools"></span>

## [tf2_tools](https://github.com/ros2/geometry2/tree/lyrical/tf2_tools/CHANGELOG.rst)

- 将conf.py文件现代化,只包括修改过的版权,eliminati... ()[\#865](https://github.com/ros2/geometry2/issues/865))

- 固定设置工具折价( S)[\#809](https://github.com/ros2/geometry2/issues/809))

- 贡献者:R Kent James, mossfet80

<span id="tlsf"></span>

## [tlsf 转换为](https://github.com/ros2/tlsf/tree/lyrical/tlsf/CHANGELOG.rst)

- 更新 cmake 要求([\#18](https://github.com/ros2/tlsf/issues/18))

- 贡献者:苔藓80

<span id="tlsf-cpp"></span>

## [tlsf_cpp](https://github.com/ros2/realtime_support/tree/lyrical/tlsf_cpp/CHANGELOG.rst)

- 清理并删除已删除的已死代码( E)[\#141](https://github.com/ros2/realtime_support/issues/141)) ([\#144](https://github.com/ros2/realtime_support/issues/144))

- 修补: 删除了分配器记忆策略( 后端端口) [\#140](https://github.com/ros2/realtime_support/issues/140)) ([\#142](https://github.com/ros2/realtime_support/issues/142))

- 删除折旧警告( E)[\#139](https://github.com/ros2/realtime_support/issues/139))

- 使用新的 ROSIDL 聚合 CMake 目标([\#137](https://github.com/ros2/realtime_support/issues/137))

- tlsf_cpp: 添加测试隔离( )[\#136](https://github.com/ros2/realtime_support/issues/136))

- 更新订阅回调签名( N)[\#135](https://github.com/ros2/realtime_support/issues/135))

- 修补 cmake 折旧( E)[\#134](https://github.com/ros2/realtime_support/issues/134))

- 在测试退出前明确关闭上下文([\#129](https://github.com/ros2/realtime_support/issues/129))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,爱默生·克纳普,朱琳·埃诺赫,注音\[bot\],小型-1235,苔藓80,雅敦德

<span id="topic-monitor"></span>

## [topic_monitor](https://github.com/ros2/demos/tree/lyrical/topic_monitor/CHANGELOG.rst)

- 添加 mypy 配置( R)[\#776](https://github.com/ros2/demos//issues/776))

- 切换到示例_界面( S)[\#674](https://github.com/ros2/demos/issues/674))

- 固定设置工具[\#733](https://github.com/ros2/demos/issues/733))

- 更新 README.md (英语).[\#718](https://github.com/ros2/demos/issues/718)) ([\#719](https://github.com/ros2/demos/issues/719))

- 贡献者: 丹·马斯卡林哈斯、卢卡斯·温德兰、注解\[bot\]、苔藓80

<span id="topic-statistics-demo"></span>

## [topic_statistics_demo](https://github.com/ros2/demos/tree/lyrical/topic_statistics_demo/CHANGELOG.rst)

- 使用新的 ROSIDL 聚合 CMake 目标([\#781](https://github.com/ros2/demos//issues/781))

- 切换到示例_界面( S)[\#674](https://github.com/ros2/demos/issues/674))

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 贡献者:爱默生·克纳普、卢卡斯·温德兰、苔藓80

<span id="tracetools"></span>

## [跟踪工具](https://github.com/ros2/ros2_tracing/tree/lyrical/tracetools/CHANGELOG.rst)

- 支持 Eclipse Trace Compass 的 ROS 2 插件所使用的复杂信件流注释的痕量点([\#233](https://github.com/ros2/ros2_tracing/issues/233))

- 删除警告( R)[\#225](https://github.com/ros2/ros2_tracing/issues/225))

- 添加运行时间追踪选择退出机制( E)[\#185](https://github.com/ros2/ros2_tracing/issues/185))

- 通过在宏中使用适当的函数原型来修正 Clang 警告( S)[\#179](https://github.com/ros2/ros2_tracing/issues/179))

- 更新 CMakeLists.txt (英语).[\#176](https://github.com/ros2/ros2_tracing/issues/176))

- 删除了 cang 警告( R)[\#168](https://github.com/ros2/ros2_tracing/issues/168))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、米歇尔·伊达尔戈、拉斐尔·范凯姆彭、什拉万·德瓦、莫斯费特80

<span id="tracetools-launch"></span>

## [tracetools_launch](https://github.com/ros2/ros2_tracing/tree/lyrical/tracetools_launch/CHANGELOG.rst)

- 微量工具\_ 启动: 非字符串动作参数使用 parse_if\_ 替代( )[\#234](https://github.com/ros2/ros2_tracing/issues/234))

- 添加快照模式的启动文件示例( E)[\#206](https://github.com/ros2/ros2_tracing/issues/206))

- 允许创建快照会话( E)[\#195](https://github.com/ros2/ros2_tracing/issues/195))

- 添加带有预配置双会话的启动文件( N)[\#196](https://github.com/ros2/ros2_tracing/issues/196))

- 添加对运行时间开始追踪的支持( E)[\#191](https://github.com/ros2/ros2_tracing/issues/191))

- 固定设置工具 折旧( )[\#189](https://github.com/ros2/ros2_tracing/issues/189))

- 使可替代 xml 和 yaml 发射文件的痕量动作参数([\#188](https://github.com/ros2/ros2_tracing/issues/188))

- 使可替换的痕量动作参数([\#187](https://github.com/ros2/ros2_tracing/issues/187))

- 微量工具中的神秘度报告的地址打字问题_发射([\#184](https://github.com/ros2/ros2_tracing/issues/184))

- 贡献者:克里斯托弗·贝达德、萨尔塔克·巴加、什拉万·德瓦、莫斯费特80

<span id="tracetools-read"></span>

## [tracetools_read](https://github.com/ros2/ros2_tracing/tree/lyrical/tracetools_read/CHANGELOG.rst)

- 在读取 bailltrace1 Python API 的微量数据时绕断层工作([\#246](https://github.com/ros2/ros2_tracing/issues/246))

- 忽略 A005 (帮助)[\#237](https://github.com/ros2/ros2_tracing/issues/237))

- 固定设置工具 折旧( )[\#189](https://github.com/ros2/ros2_tracing/issues/189))

- 微量工具中的神秘度报告的地址打字问题_发射([\#184](https://github.com/ros2/ros2_tracing/issues/184))

- 贡献者:克里斯托弗·贝达德、迈克尔·卡尔斯特罗姆、苔藓80

<span id="tracetools-test"></span>

## [tracetools_test](https://github.com/ros2/ros2_tracing/tree/lyrical/tracetools_test/CHANGELOG.rst)

- 在 TraceTestCase上设定默认值,以避免在QQ8.2.0 pytest上出现错误([\#236](https://github.com/ros2/ros2_tracing/issues/236))

- 只检查测试进程事件在测试 \_ 运行时间\_ 无法进行( )[\#193](https://github.com/ros2/ros2_tracing/issues/193))

- 固定设置工具 折旧( )[\#189](https://github.com/ros2/ros2_tracing/issues/189))

- 微量工具中的神秘度报告的地址打字问题_发射([\#184](https://github.com/ros2/ros2_tracing/issues/184))

- 贡献者:克里斯托弗·贝达德、克拉拉·贝伦森、莫斯费特80

<span id="tracetools-trace"></span>

## [tracetools_trace](https://github.com/ros2/ros2_tracing/tree/lyrical/tracetools_trace/CHANGELOG.rst)

- 支持 Eclipse Trace Compass 的 ROS 2 插件所使用的复杂信件流注释的痕量点([\#233](https://github.com/ros2/ros2_tracing/issues/233))

- 忽略 A005 (帮助)[\#237](https://github.com/ros2/ros2_tracing/issues/237))

- 添加 exec_depend on procps 到 tracktools\_ trace for ps 命令 ([\#227](https://github.com/ros2/ros2_tracing/issues/227))

- 处理SIGTERM和优雅地停止交互跟踪模式的跟踪([\#219](https://github.com/ros2/ros2_tracing/issues/219))

- 对快照会话使用覆盖模式( E)[\#210](https://github.com/ros2/ros2_tracing/issues/210))

- 允许创建快照会话( E)[\#195](https://github.com/ros2/ros2_tracing/issues/195))

- 添加对运行时间开始追踪的支持( E)[\#191](https://github.com/ros2/ros2_tracing/issues/191))

- 固定设置工具 折旧( )[\#189](https://github.com/ros2/ros2_tracing/issues/189))

- 微量工具中的神秘度报告的地址打字问题_发射([\#184](https://github.com/ros2/ros2_tracing/issues/184))

- 如果内核可能偏执于“perf:thread:”上下文字段([\#173](https://github.com/ros2/ros2_tracing/issues/173))

- 在 ros2 微量输出中修复复数化([\#169](https://github.com/ros2/ros2_tracing/issues/169))

- 撰稿人:克里斯托弗·贝达德,迈克尔·卡尔斯特罗姆,拉斐尔·范·肯彭,舍拉万·德瓦,摩斯费特80

<span id="trajectory-msgs"></span>

## [trajectory_msgs](https://github.com/ros2/common_interfaces/tree/lyrical/trajectory_msgs/CHANGELOG.rst)

- 修复 CMAKE 折旧[\#288](https://github.com/ros2/common_interfaces/issues/288))

- 贡献者:苔藓80

<span id="turtlesim"></span>

## [乌龟](https://github.com/ros/ros_tutorials/tree/lyrical/turtlesim/CHANGELOG.rst)

- 为 Lyrical Luth 添加图标([\#196](https://github.com/ros/ros_tutorials/issues/196)) ([\#197](https://github.com/ros/ros_tutorials/issues/197))

- 使用按平台选择 Qt5 或 Qt6 的 rosdep 密钥([\#195](https://github.com/ros/ros_tutorials/issues/195))

- 使用新的 ROSIDL 聚合 CMake 目标([\#194](https://github.com/ros/ros_tutorials/issues/194))

- 使用 get\_ package_share\_ path ([\#193](https://github.com/ros/ros_tutorials/issues/193))

- 修补装入龟图像的臭虫( E)[\#192](https://github.com/ros/ros_tutorials//issues/192))

- 更新已贬值的 ament_index_cpp API ([\#190](https://github.com/ros/ros_tutorials/issues/190))

- 使用 qt6 作为来自 rosdep 的默认依赖( )[\#189](https://github.com/ros/ros_tutorials/issues/189))

- 清除已腐烂的 rclp::spin_some() ()[\#183](https://github.com/ros/ros_tutorials/issues/183))

- 支持 Qt6 (帮助 )[\#170](https://github.com/ros/ros_tutorials/issues/170))

- 为 Kilted Kaiju 添加图标([\#180](https://github.com/ros/ros_tutorials/issues/180))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,爱默生·克纳普,斯科特·K·洛根,谢恩·洛雷茨,dcconner,加热\[bot\]

<span id="turtlesim-msgs"></span>

## [turtlesim_msgs](https://github.com/ros/ros_tutorials/tree/lyrical/turtlesim_msgs/CHANGELOG.rst)

- 固定 : 在 CMakeLists.txt 中互换动作和信件文件组名称 (S)[\#186](https://github.com/ros/ros_tutorials/issues/186)) ([\#187](https://github.com/ros/ros_tutorials/issues/187))

- 固定cmake 折旧([\#182](https://github.com/ros/ros_tutorials/issues/182))

- 贡献者:加热\[bot\],苔藓(mosfet80)

<span id="type-description-interfaces"></span>

## [type_description_interfaces](https://github.com/ros2/rcl_interfaces/tree/lyrical/type_description_interfaces/CHANGELOG.rst)

- 修补 cmake 折旧( E)[\#180](https://github.com/ros2/rcl_interfaces/issues/180))

- 贡献者:苔藓80

<span id="uncrustify-vendor"></span>

## [uncrustify_vendor](https://github.com/ament/uncrustify_vendor/tree/lyrical/CHANGELOG.rst)

- 删除 CODEOWINERS 和镜像滚动到主工作流程 。 ([\#38](https://github.com/ament/uncrustify_vendor/issues/38))

- 撰稿人:克里斯·拉兰谢特

<span id="unique-identifier-msgs"></span>

## [unique_identifier_msgs](https://github.com/ros2/unique_identifier_msgs/tree/lyrical/CHANGELOG.rst)

- 固定cmake 折旧([\#33](https://github.com/ros2/unique_identifier_msgs/issues/33))

- 删除 CODEOWINERS 和镜像滚动到主工作流程 。 ([\#31](https://github.com/ros2/unique_identifier_msgs/issues/31))

- 贡献者:克里斯·拉朗谢特,苔藓80

<span id="urdf"></span>

## [乌尔德夫](https://github.com/ros2/urdf/tree/lyrical/urdf/CHANGELOG.rst)

- 删除 `urdf_world/types.h` 折旧[\#54](https://github.com/ros2/urdf/issues/54))

- 修复 CMAKE 折旧[\#48](https://github.com/ros2/urdf/issues/48))

- 已删除的微小xml2\_ vendor 依赖性([\#47](https://github.com/ros2/urdf/issues/47))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,苔藓80

<span id="urdf-parser-plugin"></span>

## [urdf_parser_plugin](https://github.com/ros2/urdf/tree/lyrical/urdf_parser_plugin/CHANGELOG.rst)

- 删除 `urdf_world/types.h` 折旧[\#54](https://github.com/ros2/urdf/issues/54))

- 修复 CMAKE 折旧[\#48](https://github.com/ros2/urdf/issues/48)cmake 版本 \< 然后3.10 贬值

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,苔藓80

<span id="urdfdom"></span>

## [土 产](https://github.com/ros/urdfdom/tree/lyrical/CHANGELOG.rst)

- 支持URDF规格 1.2 \* 扩展加速、减速和杂耍限制的解析范围。 `limit` 标记( R)[\#212](https://github.com/ros/urdfdom/issues/212)\* 更新联合限值和安全限值的默认限值([\#249](https://github.com/ros/urdfdom/issues/249)\* 在几何数据中添加无效的数据检查([\#242](https://github.com/ros/urdfdom/issues/242)\* 需要urdfdom_headers 3.0.0 (中文(简体) ).[\#257](https://github.com/ros/urdfdom/issues/257))

- 使用 URDF_MAJOR_VERSION 进行覆盖([\#248](https://github.com/ros/urdfdom/issues/248))

- 将“加速、减速和自带限制从 `limit` 标记( R)[\#212](https://github.com/ros/urdfdom/issues/212)”这是一次突破性的改变,将在6.0.0中发布。

- 防止CI迅速失败,使所有建筑都能够完成([\#254](https://github.com/ros/urdfdom/issues/254))

- 删除 `urdf_world/types.h` 折旧[\#251](https://github.com/ros/urdfdom/issues/251))

- 扩展加速度、减速和混蛋限制的解析范围 `limit` 标记( R)[\#212](https://github.com/ros/urdfdom/issues/212))

- ROS 2 CI: 从源头建立 urdfdom_headers ([\#246](https://github.com/ros/urdfdom/issues/246))

- 禁用系统工作流程,因为 `urdfdom_headers` 无法在 Ubuntu 24.04 上获取([\#240](https://github.com/ros/urdfdom/issues/240))

- 通过更新 Ubuntu 版本和退出动作来修正 ROS 2 CI 工作流程([\#239](https://github.com/ros/urdfdom/issues/239))

- 支持URDF规格 1.1 \* 加入支持胶囊几何类型([\#238](https://github.com/ros/urdfdom/issues/238)\* 添加关于版本的文档 \* 需要urdfdom_headers的2.1.0版本 由 Steve Peters 共同编写 \<[scpeters@openrobotics.org](mailto:scpeters%40openrobotics.org)\> \* URDF 1.1中的支持之四 (单位:千美元)[\#235](https://github.com/ros/urdfdom/issues/235))合著:纪尧姆·多伊西 \<[doisyg@users.noreply.github.com](mailto:doisyg%40users.noreply.github.com)\>

- 在URDF 剖析器记录中修复多个格式字符串的弱点([\#243](https://github.com/ros/urdfdom/issues/243)用户控制的 URDF 内容在多个呼叫站点被直接传递到 CONSOLE\_ BRIDGE_logError () , 允许打印f 风格的格式字符串解释 。 现在所有受影响的日志路径都使用明确的“ %s” 格式描述符, 以确保输入被视为数据, 并防止信息披露或未定义的行为 。

- 更多日志格式字符串修正( E)[\#244](https://github.com/ros/urdfdom/issues/244)\* 记录时添加明显的“%s”格式字符串 \* 使用 %s 格式字符串而不是字符串添加

- 从 package.xml 读取 CMake 版本 ([\#236](https://github.com/ros/urdfdom/issues/236)) \* 使用 regex来匹配版本字符串. 根据Chris Lalancette 的建议. \* 需要 cmake 最小版本 3.10 由 Chris Lalancette 共同编写: \<[clalancette@gmail.com](mailto:clalancette%40gmail.com)\>

- 还原“urdf中的Quarnion(PR123新尝试)(# 231)” (中文(简体) ).[\#231](https://github.com/ros/urdfdom/issues/231))

- urdf中的Quaternion(PR123新尝试) (英语).[\#194](https://github.com/ros/urdfdom/issues/194))

- 已删除的微小xml2\_ vendor 依赖性([\#225](https://github.com/ros/urdfdom/issues/225))

- 放松urdfdom_headers的版本兼容性. ().[\#222](https://github.com/ros/urdfdom/issues/222))

- 删除已折旧的代码( N)[\#217](https://github.com/ros/urdfdom/issues/217))

- 删除 ROS 1 工作流程并更新 ROS 2 ()[\#218](https://github.com/ros/urdfdom/issues/218))

- 改进URDF xsd 规格([\#200](https://github.com/ros/urdfdom/issues/200))

- 更新 ros2. yaml (中文(简体) ).[\#214](https://github.com/ros/urdfdom/issues/214))

- 修补: 缺少信头( E)[\#216](https://github.com/ros/urdfdom/issues/216))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、阿明·亚、克里斯·拉朗谢特、弗洛伦西亚、纪尧姆·多伊西、何塞·路易斯·里韦罗、皮埃尔·巴利夫、赛·基绍尔·科塔科塔、史蒂夫·彼得斯、莫斯费特80

<span id="urdfdom-headers"></span>

## [urdfdom_headers](https://github.com/ros/urdfdom_headers/tree/lyrical/CHANGELOG.rst)

- 更新下限、上限、努力度和速度默认联合限制([\#95](https://github.com/ros/urdfdom_headers/issues/95))

- 清理模型界面共享软件的宣告( E)[\#99](https://github.com/ros/urdfdom_headers/issues/99))

- 扩展 `JointLimits` 分类包括加速度、减速和插件限制([\#83](https://github.com/ros/urdfdom_headers/issues/83))

- 重置“ 扩展联合限制等级以包括加速度、 减速和杂件限制( Q) ”[\#83](https://github.com/ros/urdfdom_headers/issues/83)”这是一个突破性的改变,将在3.0.0中发布。

- 清理模型界面共享软件的宣告( E)[\#99](https://github.com/ros/urdfdom_headers/issues/99))

- 恢复对 modelInterface 共享Pts 的清理([\#33](https://github.com/ros/urdfdom_headers/issues/33))

- 恢复对 CMAKE_INSTALL%DIR 路径是相对的假设的修复([\#90](https://github.com/ros/urdfdom_headers/issues/90)) ([\#97](https://github.com/ros/urdfdom_headers/issues/97))

- 清理模型界面共享软件的宣告( E)[\#33](https://github.com/ros/urdfdom_headers/issues/33))

- 修正假设 CMAKE_INSTALL =%DIR 路径是相对的([\#90](https://github.com/ros/urdfdom_headers/issues/90))

- 扩展 `JointLimits` 分类包括加速度、减速和插件限制([\#83](https://github.com/ros/urdfdom_headers/issues/83))

- 添加对胶囊几何类型的支持( E)[\#94](https://github.com/ros/urdfdom_headers/issues/94))

- 2.0.2

- 从 package.xml 读取 CMake 版本 ([\#92](https://github.com/ros/urdfdom_headers/issues/92))使用 regex 来匹配版本字符串。 [ros/urdfdom#236](https://github.com/ros/urdfdom/issues/236).

- urdf中的之四(PR 51新尝试) + tump版本([\#77](https://github.com/ros/urdfdom_headers/issues/77))

- 固定cmake 折旧([\#89](https://github.com/ros/urdfdom_headers/issues/89)cmake 版本 \< 然后3.10 贬值

- 2.0.0

- 从 package.xml 中删除所有依赖性 ()[\#88](https://github.com/ros/urdfdom_headers/issues/88))这个软件包没有任何信头依赖关系,所以我们这里不需要任何信头。

- 固定套件.xml 代用软件包([\#87](https://github.com/ros/urdfdom_headers/issues/87))

- 从发布寄存器中添加 package.xml 文件([\#85](https://github.com/ros/urdfdom_headers/issues/85))

- 删除标题, 执行被折旧和删除( Q)[\#86](https://github.com/ros/urdfdom_headers/issues/86))

- 移除 CODEOWINERS. (中文(简体) ).[\#81](https://github.com/ros/urdfdom_headers/issues/81)它已经过时,不再达到预定目的。

- 贡献者:阿拉夫·古普塔、亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉朗谢特、纪尧姆·多伊西、豪尔赫·佩雷斯、卢西安·莫雷、米哈尔·索伊卡、罗伯特·哈施克、赛伊·基绍尔·科塔科塔、史蒂夫·彼得斯、苔丝费特80

<span id="visualization-msgs"></span>

## [visualization_msgs](https://github.com/ros2/common_interfaces/tree/lyrical/visualization_msgs/CHANGELOG.rst)

- 修复 CMAKE 折旧[\#288](https://github.com/ros2/common_interfaces/issues/288))

- 贡献者:苔藓80

<span id="yaml-cpp-vendor"></span>

## [yaml_cpp_vendor](https://github.com/ros2/yaml_cpp_vendor/tree/lyrical/CHANGELOG.rst)

- 将 ament_vendor 替换为 CMake 模块 ([\#56](https://github.com/ros2/yaml_cpp_vendor/issues/56))

- 删除 CODEOWINERS 和镜像滚动到主工作流程 。 ([\#52](https://github.com/ros2/yaml_cpp_vendor/issues/52))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特

<span id="zenoh-cpp-vendor"></span>

## [zenoh_cpp_vendor](https://github.com/ros2/rmw_zenoh/tree/lyrical/zenoh_cpp_vendor/CHANGELOG.rst)

- 使用 Zenoh-cpp 481b71b 在 C++20 模式下使用 MSVC 2022 固定构建([\#969](https://github.com/ros2/rmw_zenoh/issues/969))

- Bump Zenoh 到 1. 8.0, 修补 Windows 关闭挂起, 并解决同步问题 。 `undeclare` ([\#964](https://github.com/ros2/rmw_zenoh/issues/964))

- 反向改变,以对抗锈蚀 = 1.75 和 折叠 zenoh 到 1.8.0 ([\#960](https://github.com/ros2/rmw_zenoh/issues/960))

- 由于 Windows 测试失败, 将 Cargo.lock 的补丁与 Zenoh 的新承诺还原([\#959](https://github.com/ros2/rmw_zenoh/issues/959))

- 更新 Cargo.lock 并添加新的 Zenoh 承诺( Q)[\#957](https://github.com/ros2/rmw_zenoh/issues/957))

- 建构对 `rust >= 1.75` ROS 语言([\#945](https://github.com/ros2/rmw_zenoh/issues/945))

- 弹出子纳至1.8.0([\#935](https://github.com/ros2/rmw_zenoh/issues/935))

- 允许使用在座的非复仇 Zenoh (如果有的话)[\#908](https://github.com/ros2/rmw_zenoh/issues/908))

- 弹跳 `zenoh` 改为1.7.1 (中文(简体) ).[\#870](https://github.com/ros2/rmw_zenoh/issues/870))

- 修复 REP url 地点([\#858](https://github.com/ros2/rmw_zenoh/issues/858))

- ⁇ ( ⁇ )至1.6.2( ⁇ )[\#842](https://github.com/ros2/rmw_zenoh/issues/842))

- 弹出Zenoh改为1.5.1([\#774](https://github.com/ros2/rmw_zenoh/issues/774))

- Bump Zenoh 到 v1.5.0 (中文(简体) ).[\#728](https://github.com/ros2/rmw_zenoh/issues/728))

- 更改 zenoh-c 特性,以使用默认 + 共享- 模拟 + 运输\_ 串联( serial)[\#692](https://github.com/ros2/rmw_zenoh/issues/692))

- 弹出Zenoh至1.4.0([\#652](https://github.com/ros2/rmw_zenoh/issues/652))

- 固定: 将生锈工具链钉到v1.75.0([\#602](https://github.com/ros2/rmw_zenoh/issues/602))

- 固定值:用右键将zenoh撞到v1.3.2([\#607](https://github.com/ros2/rmw_zenoh/issues/607))

- 撰稿人:陈英国(共青团),朱琳·艾诺赫,谢恩·洛雷茨,蒂姆·克莱法斯,亚敦恩德,袁玉 ⁇ ,艾斯坦·斯图雷.

<span id="zenoh-security-tools"></span>

## [zenoh_security_tools](https://github.com/ros2/rmw_zenoh/tree/lyrical/zenoh_security_tools/CHANGELOG.rst)

- 处理尚未解决的 TODO 项目([\#896](https://github.com/ros2/rmw_zenoh/issues/896))

- 已删除的微小xml2\_ vendor 依赖性([\#829](https://github.com/ros2/rmw_zenoh/issues/829))

- 在 zenoh_security_tools 中修正命令 README ()[\#814](https://github.com/ros2/rmw_zenoh/issues/814))

- 重置“ fix: handle missed 飞地\_ dir 参数 for zenoh_security_tools (#...) ” ([\#802](https://github.com/ros2/rmw_zenoh/issues/802))

- 校正 zenoh_security_tools README 中的描述错误([\#789](https://github.com/ros2/rmw_zenoh/issues/789))

- 修补: 处理缺少的飞地\_ dir 参数 zenoh_security_tools ()[\#788](https://github.com/ros2/rmw_zenoh/issues/788))

- SROS:为 TRANSIENT_LOCAL 酒吧/子( 定义) 添加 ACL 规则 [\#753](https://github.com/ros2/rmw_zenoh/issues/753)) ([\#779](https://github.com/ros2/rmw_zenoh/issues/779))

- 修复 Zenoh_security_tools 中的飞地路径处理( )[\#770](https://github.com/ros2/rmw_zenoh/issues/770))

- 更新 CMakeLists.txt (英语).[\#617](https://github.com/ros2/rmw_zenoh/issues/617))

- 在 Windows 上修正警告( E)[\#615](https://github.com/ros2/rmw_zenoh/issues/615))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,巴瑞·徐,克里斯托弗·贝达德,朱利安·埃诺赫,藤田富美亚,亚杜南德,苔藓80
