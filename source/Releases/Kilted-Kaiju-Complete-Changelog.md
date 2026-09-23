---
translation_status: machine_translated
source: Releases/Kilted-Kaiju-Complete-Changelog.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="ros-2-kilted-kaiju-complete-changelog"></span>

# ROS 2 Kilted Kaiju 完整变更日志

本页面是自上一期发布以来所有ROS 2核心包的完整更改列表.

<span id="action-msgs"></span>

## [action_msgs](https://github.com/ros2/rcl_interfaces/tree/kilted/action_msgs/CHANGELOG.rst)

- 添加缺失的构建\_ 导出\_ 依赖 rosidl\_ core_runtime ()[\#165](https://github.com/ros2/rcl_interfaces/issues/165))

- 撰稿人:斯科特·K·洛根

<span id="action-tutorials-cpp"></span>

## [action_tutorials_cpp](https://github.com/ros2/demos/tree/kilted/action_tutorials/action_tutorials_cpp/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714)) demo_nodes_cpp/CMakeLists.txt 需要cmake min 版本 3.12 其他模块 cmake 3.5 。它被提议与 3.12 版本标准化。它也修正 cmake \< 3.10 折旧警告

- 更新动作 cpp 演示支持设置内存( U)[\#709](https://github.com/ros2/demos/issues/709)) \* 更新动作 cpp 演示支持设置内存 \* 添加缺失头文件声明 \*

- 删除动作_tutoris_接口 。 ()[\#701](https://github.com/ros2/demos/issues/701))

- 删除过时的注释( R)[\#699](https://github.com/ros2/demos/issues/699))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、许巴里、克里斯·拉朗谢特、莫斯费特80

<span id="action-tutorials-py"></span>

## [action_tutorials_py](https://github.com/ros2/demos/tree/kilted/action_tutorials/action_tutorials_py/CHANGELOG.rst)

- 更新动作 python 演示支持设置内存([\#708](https://github.com/ros2/demos/issues/708)\* 更新动作 python 演示支持设置内存 \* 校正文档 QQ中的错误

- 在所有的 Ament_python 包中添加 test_xmllint.py 。 ([\#704](https://github.com/ros2/demos/issues/704))

- 删除动作_tutoris_接口 。 ()[\#701](https://github.com/ros2/demos/issues/701))

- 更改所有演示使用新的 rclpy 上下文管理器 。 ()[\#694](https://github.com/ros2/demos/issues/694))

- 撰稿人: 许巴里、克里斯·拉兰谢特

<span id="ament-clang-format"></span>

## [ament_clang_format](https://github.com/ament/ament_lint/tree/kilted/ament_clang_format/CHANGELOG.rst)

- 对所有可以使用的软件包添加 ament_xmllint 测试 。 ([\#508](https://github.com/ament/ament_lint/issues/508))

- 撰稿人:克里斯·拉兰谢特

<span id="ament-clang-tidy"></span>

## [ament_clang_tidy](https://github.com/ament/ament_lint/tree/kilted/ament_clang_tidy/CHANGELOG.rst)

- 对所有可以使用的软件包添加 ament_xmllint 测试 。 ([\#508](https://github.com/ament/ament_lint/issues/508))

- ament_clang_tidy - 在配置中指定警告错误时的固定报告([\#397](https://github.com/ament/ament_lint/issues/397))

- 贡献者:克里斯·拉朗谢特、马特·康迪诺

<span id="ament-cmake-auto"></span>

## [ament_cmake_auto](https://github.com/ament/ament_cmake/tree/kilted/ament_cmake_auto/CHANGELOG.rst)

- 通过 ament_auto\_ package 安装的 固定信头目标( )[\#540](https://github.com/ament/ament_cmake/issues/540))

- 添加 ament_auto_dependent_on_packages 以替换 ament_target_dependents ([\#571](https://github.com/ament/ament_cmake/issues/571))

- 某些 cmake_parse\_ 参数调用中更具体的前缀( )[\#523](https://github.com/ament/ament_cmake/issues/523))

- 贡献者:凯文·埃格尔,吉本小太郎,谢恩·洛雷茨

<span id="ament-cmake-core"></span>

## [ament_cmake_core](https://github.com/ament/ament_cmake/tree/kilted/ament_cmake_core/CHANGELOG.rst)

- 在同步链接安装时创建目的目录( C)[\#569](https://github.com/ament/ament_cmake/issues/569))

- 同步安装时支持生成器表达式( FILES) ([\#560](https://github.com/ament/ament_cmake/issues/560))

- 总是将TARGETQLINKER,SONAMEFILE 链接到库([\#535](https://github.com/ament/ament_cmake/issues/535))

- 在 macOS 上修复已版本的 libs 安装的 symlink ()[\#558](https://github.com/ament/ament_cmake/issues/558))

- 某些 cmake_parse\_ 参数调用中更具体的前缀( )[\#523](https://github.com/ament/ament_cmake/issues/523))

- 贡献者:埃兹拉·布鲁克斯、凯文·埃格、斯科特·K·洛根

<span id="ament-cmake-gen-version-h"></span>

## [ament_cmake_gen_version_h](https://github.com/ament/ament_cmake/tree/kilted/ament_cmake_gen_version_h/CHANGELOG.rst)

- 添加 Ament_generate_version_header 目标的全部目标 。 ()[\#526](https://github.com/ament/ament_cmake/issues/526))

- 撰稿人:克里斯·拉兰谢特

<span id="ament-cmake-gtest"></span>

## [ament_cmake_gtest](https://github.com/ament/ament_cmake/tree/kilted/ament_cmake_gtest/CHANGELOG.rst)

- 设置搜索路径参数,然后附加( E)[\#543](https://github.com/ament/ament_cmake/issues/543))

- 贡献者:威尔

<span id="ament-cmake-pytest"></span>

## [ament_cmake_pytest](https://github.com/ament/ament_cmake/tree/kilted/ament_cmake_pytest/CHANGELOG.rst)

- 引用 pytest 时不要写 Python 字节代码([\#533](https://github.com/ament/ament_cmake/issues/533))

- 撰稿人:斯科特·K·洛根

<span id="ament-cmake-ros"></span>

## [ament_cmake_ros](https://github.com/ros2/ament_cmake_ros/tree/kilted/ament_cmake_ros/CHANGELOG.rst)

- 添加 ament_add_ros_isolated\_%gmock, gtest\_%test 宏([\#29](https://github.com/ros2/ament_cmake_ros/issues/29))

- 从“域协调员”切换到“rmw\_ test_fixture”([\#28](https://github.com/ros2/ament_cmake_ros/issues/28))

- 添加 ament_add_ros_isolate_测试函数([\#27](https://github.com/ros2/ament_cmake_ros/issues/27))

- 将 ament_cmake_ros 的通用部件拆分为 \_core 包([\#20](https://github.com/ros2/ament_cmake_ros/issues/20))

- 撰稿人:斯科特·K·洛根

<span id="ament-cmake-ros-core"></span>

## [ament_cmake_ros_core](https://github.com/ros2/ament_cmake_ros/tree/kilted/ament_cmake_ros_core/CHANGELOG.rst)

- 添加缺失的构建\_ 导出\_ 依赖 ament\_ cmake\_ librarys ()[\#37](https://github.com/ros2/ament_cmake_ros/issues/37))

- 将 ament_cmake_ros 的通用部件拆分为 \_core 包([\#20](https://github.com/ros2/ament_cmake_ros/issues/20))

- 撰稿人:斯科特·K·洛根

<span id="ament-cmake-target-dependencies"></span>

## [ament_cmake_target_dependencies](https://github.com/ament/ament_cmake/tree/kilted/ament_cmake_target_dependencies/CHANGELOG.rst)

- 折旧ament_target_依赖性() ([\#572](https://github.com/ament/ament_cmake/issues/572))

- 撰稿人:谢恩·洛雷茨

<span id="ament-cmake-vendor-package"></span>

## [ament_cmake_vendor_package](https://github.com/ament/ament_cmake/tree/kilted/ament_cmake_vendor_package/CHANGELOG.rst)

- 从 ament_cmake_vendor\_ package 中添加明确的 git 依赖性([\#554](https://github.com/ament/ament_cmake/issues/554))

- 撰稿人:斯科特·K·洛根

<span id="ament-copyright"></span>

## [ament_copyright](https://github.com/ament/ament_lint/tree/kilted/ament_copyright/CHANGELOG.rst)

- 大力改进右侧的性能。 ([\#515](https://github.com/ament/ament_lint/issues/515))

- 修补搜索路径\_ copyright\_ information. ()[\#491](https://github.com/ament/ament_lint/issues/491))

- 撰稿人:克里斯·拉兰谢特

<span id="ament-cppcheck"></span>

## [ament_cppcheck](https://github.com/ament/ament_lint/tree/kilted/ament_cppcheck/CHANGELOG.rst)

- 对所有可以使用的软件包添加 ament_xmllint 测试 。 ([\#508](https://github.com/ament/ament_lint/issues/508))

- 撰稿人:克里斯·拉兰谢特

<span id="ament-cpplint"></span>

## [ament_cpplint](https://github.com/ament/ament_lint/tree/kilted/ament_cpplint/CHANGELOG.rst)

- 启用 cpplint 的静态模式( N)[\#532](https://github.com/ament/ament_lint/issues/532))

- 对所有可以使用的软件包添加 ament_xmllint 测试 。 ([\#508](https://github.com/ament/ament_lint/issues/508))

- 贡献者:克里斯·拉兰塞特、尼尔斯-克里斯蒂安·伊塞克

<span id="ament-flake8"></span>

## [ament_flake8](https://github.com/ament/ament_lint/tree/kilted/ament_flake8/CHANGELOG.rst)

- 添加其余的 Flake8 插件作为依赖性 。 ()[\#503](https://github.com/ament/ament_lint/issues/503))

- 撰稿人:克里斯·拉兰谢特

<span id="ament-index-python"></span>

## [ament_index_python](https://github.com/ament/ament_index/tree/kilted/ament_index_python/CHANGELOG.rst)

- 将 py.typed 添加到软件包\_ data ([\#100](https://github.com/ament/ament_index/issues/100))

- 将 test_xmllint 添加到 ament_index_python 中 ().[\#96](https://github.com/ament/ament_index/issues/96))

- 添加 ment_mypy 单元测试和导出类型([\#95](https://github.com/ament/ament_index/issues/95))

- 贡献者:克里斯·拉朗谢特、迈克尔·卡尔斯特罗姆

<span id="ament-lint-auto"></span>

## [ament_lint_auto](https://github.com/ament/ament_lint/tree/kilted/ament_lint_auto/CHANGELOG.rst)

- 添加 AMENT_LINT_AUTO_EXCLUDE 的文件([\#524](https://github.com/ament/ament_lint/issues/524))

- 贡献者:亚历山大·雷曼

<span id="ament-lint-cmake"></span>

## [ament_lint_cmake](https://github.com/ament/ament_lint/tree/kilted/ament_lint_cmake/CHANGELOG.rst)

- 对所有可以使用的软件包添加 ament_xmllint 测试 。 ([\#508](https://github.com/ament/ament_lint/issues/508))

- 撰稿人:克里斯·拉兰谢特

<span id="ament-mypy"></span>

## [ament_mypy](https://github.com/ament/ament_lint/tree/kilted/ament_mypy/CHANGELOG.rst)

- 通过去掉后缀来修正Windows递归() ()[\#530](https://github.com/ament/ament_lint/issues/530))

- 导出打字信息( E)[\#487](https://github.com/ament/ament_lint/issues/487))

- 添加类型树根的支持( N)[\#516](https://github.com/ament/ament_lint/issues/516))

- 对所有可以使用的软件包添加 ament_xmllint 测试 。 ([\#508](https://github.com/ament/ament_lint/issues/508))

- 贡献者:克里斯·拉朗谢特、迈克尔·卡尔斯特罗姆

<span id="ament-package"></span>

## [ament_package](https://github.com/ament/ament_package/tree/kilted/CHANGELOG.rst)

- 简化去除前导和后导分离器([\#152](https://github.com/ament/ament_package/issues/152))

- 删除 CODEOWINERS 和 镜像滚动到主机 。 ([\#149](https://github.com/ament/ament_package/issues/149))

- 总是考虑.dsv文件,即使不存在 shell 特定脚本([\#147](https://github.com/ament/ament_package/issues/147))

- 贡献者:Addiu Z. Taddese、Chris Lalancette、Rob Woolley

<span id="ament-pclint"></span>

## [ament_pclint](https://github.com/ament/ament_lint/tree/kilted/ament_pclint/CHANGELOG.rst)

- 对所有可以使用的软件包添加 ament_xmllint 测试 。 ([\#508](https://github.com/ament/ament_lint/issues/508))

- 撰稿人:克里斯·拉兰谢特

<span id="ament-pycodestyle"></span>

## [ament_pycodestyle](https://github.com/ament/ament_lint/tree/kilted/ament_pycodestyle/CHANGELOG.rst)

- 对所有可以使用的软件包添加 ament_xmllint 测试 。 ([\#508](https://github.com/ament/ament_lint/issues/508))

- 撰稿人:克里斯·拉兰谢特

<span id="ament-pyflakes"></span>

## [ament_pyflakes](https://github.com/ament/ament_lint/tree/kilted/ament_pyflakes/CHANGELOG.rst)

- 对所有可以使用的软件包添加 ament_xmllint 测试 。 ([\#508](https://github.com/ament/ament_lint/issues/508))

- 撰稿人:克里斯·拉兰谢特

<span id="ament-uncrustify"></span>

## [ament_uncrustify](https://github.com/ament/ament_lint/tree/kilted/ament_uncrustify/CHANGELOG.rst)

- 对所有可以使用的软件包添加 ament_xmllint 测试 。 ([\#508](https://github.com/ament/ament_lint/issues/508))

- 撰稿人:克里斯·拉兰谢特

<span id="ament-xmllint"></span>

## [ament_xmllint](https://github.com/ament/ament_lint/tree/kilted/ament_xmllint/CHANGELOG.rst)

- 对所有可以使用的软件包添加 ament_xmllint 测试 。 ([\#508](https://github.com/ament/ament_lint/issues/508))

- 撰稿人:克里斯·拉兰谢特

<span id="builtin-interfaces"></span>

## [builtin_interfaces](https://github.com/ros2/rcl_interfaces/tree/kilted/builtin_interfaces/CHANGELOG.rst)

- 添加缺失的构建\_ 导出\_ 依赖 rosidl\_ core_runtime ()[\#165](https://github.com/ros2/rcl_interfaces/issues/165))

- 撰稿人:斯科特·K·洛根

<span id="camera-calibration-parsers"></span>

## [camera_calibration_parsers](https://github.com/ros-perception/image_common/tree/kilted/camera_calibration_parsers/CHANGELOG.rst)

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#345](https://github.com/ros-perception/image_common/issues/345))

- 添加到相机\_ 校准\_ parsers 中的常见插件( Name[\#317](https://github.com/ros-perception/image_common/issues/317))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、谢恩·洛雷茨

<span id="camera-info-manager"></span>

## [camera_info_manager](https://github.com/ros-perception/image_common/tree/kilted/camera_info_manager/CHANGELOG.rst)

- 在 CameraInfoManager 中将可选命名空间添加到/set_camera_info 服务( )[\#324](https://github.com/ros-perception/image_common/issues/324))

- 添加到相机信息管理器中的常见测试( E)[\#318](https://github.com/ros-perception/image_common/issues/318))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、扬·赫纳斯

<span id="camera-info-manager-py"></span>

## [camera_info_manager_py](https://github.com/ros-perception/image_common/tree/kilted/camera_info_manager_py/CHANGELOG.rst)

- 正在清理相机\_ info_manager_py. ()[\#340](https://github.com/ros-perception/image_common/issues/340))

- 添加 `camera_info_manager_py` ([\#335](https://github.com/ros-perception/image_common/issues/335))

- 要与图像同步的弹出包版本\_ common

- 罗斯2号([\#2](https://github.com/clearpathrobotics/camera_info_manager_py/issues/2)) \* 运行魔法转换器 \* Ament_python 套件 \* 修复一些导入 \* 删除 cpp 相机信息管理器的引用 。 禁用测试 \* Linting \* 完全取消旧测试 \* 添加 lint 测试 \* 最终测试 \* 从依赖中移除 pep257

- 更改日志

- 添加心肺复苏维护器

- 释放到甲型和甲型

- 只有在测试启用时才使用Rostest,这要归功于Lukas Bulwahn.

- 移动寄存器到 ros- 感知 。

- 向构造器添加命名空间参数,这样一个驱动程序就可以处理多个相机. 增强感谢Martin Llofriu.

- 设定单位测试条件 `CATKIN_ENABLE_TESTING`.

- 释放给格鲁维和赫洛

- 设置无效校正, 即使 URL 无效 (# 7) 。

- 释放给格鲁维和赫洛

- 转换为猫金。

- 删除 roslib 依赖 。

- 释放给格鲁维和赫洛

- 初始 Python 相机\_ info_manager 向 Fuerte 发布 。

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·伊韦拉赫-布雷顿、克里斯·拉朗谢特、杰克·奥金、何塞·马斯特兰热洛、卢卡斯·沃尔特、卢卡斯·布尔瓦亨、马丁·佩卡、迈克尔·霍斯马尔、姆洛夫里乌

<span id="composition"></span>

## [组成](https://github.com/ros2/demos/tree/kilted/composition/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 设定 Envars 以 rmw_zenoh_cpp 进行多播发现的测试([\#711](https://github.com/ros2/demos/issues/711))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#707](https://github.com/ros2/demos/issues/707))

- 修补成分注释中的类型( E) :[\#703](https://github.com/ros2/demos/issues/703))

- 将滚动分支的“jazzy”改为“滚动”。[\#687](https://github.com/ros2/demos/issues/687))

- \[组合\]在校验部分添加发射动作控制台输出([\#677](https://github.com/ros2/demos/issues/677))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、克里斯托弗·贝达德、米卡埃尔·阿尔盖达斯、谢恩·洛雷茨、莫斯费特80

<span id="demo-nodes-cpp"></span>

## [demo_nodes_cpp](https://github.com/ros2/demos/tree/kilted/demo_nodes_cpp/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 设定 Envars 以 rmw_zenoh_cpp 进行多播发现的测试([\#711](https://github.com/ros2/demos/issues/711))

- \[demo_nodes_cpp\] 一些可读和可执行的名称固定([\#678](https://github.com/ros2/demos/issues/678))

- 以优化方式构建时修正 gcc 警告 。 ()[\#672](https://github.com/ros2/demos/issues/672))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、米卡埃尔·阿尔盖达斯、苔藓80

<span id="demo-nodes-cpp-native"></span>

## [demo_nodes_cpp_native](https://github.com/ros2/demos/tree/kilted/demo_nodes_cpp_native/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#707](https://github.com/ros2/demos/issues/707))

- 贡献者:谢恩·洛雷茨、苔藓80

<span id="demo-nodes-py"></span>

## [demo_nodes_py](https://github.com/ros2/demos/tree/kilted/demo_nodes_py/CHANGELOG.rst)

- 从 yaml 文件恢复“ 修正” 固定加载参数行为 。 ()[\#656](https://github.com/ros2/demos/issues/656))” ([\#660](https://github.com/ros2/demos/issues/660))” ([\#661](https://github.com/ros2/demos/issues/661))

- 在所有的 Ament_python 包中添加 test_xmllint.py 。 ([\#704](https://github.com/ros2/demos/issues/704))

- 更改所有演示使用新的 rclpy 上下文管理器 。 ()[\#694](https://github.com/ros2/demos/issues/694))

- 撰稿人:克里斯·拉兰塞特、藤田友也

<span id="domain-coordinator"></span>

## [domain_coordinator](https://github.com/ros2/ament_cmake_ros/tree/kilted/domain_coordinator/CHANGELOG.rst)

- 添加 test_xmllint 到域名_协调员 。 ()[\#17](https://github.com/ros2/ament_cmake_ros/issues/17))

- 撰稿人:克里斯·拉兰谢特

<span id="dummy-map-server"></span>

## [dummy_map_server](https://github.com/ros2/demos/tree/kilted/dummy_robot/dummy_map_server/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714)) demo_nodes_cpp/CMakeLists.txt 需要cmake min 版本 3.12 其他模块 cmake 3.5 。它被提议与 3.12 版本标准化。它也修正 cmake \< 3.10 折旧警告

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#707](https://github.com/ros2/demos/issues/707))

- 贡献者:谢恩·洛雷茨、苔藓80

<span id="dummy-robot-bringup"></span>

## [dummy_robot_bringup](https://github.com/ros2/demos/tree/kilted/dummy_robot/dummy_robot_bringup/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714)) demo_nodes_cpp/CMakeLists.txt 需要cmake min 版本 3.12 其他模块 cmake 3.5 。它被提议与 3.12 版本标准化。它也修正 cmake \< 3.10 折旧警告

- 贡献者:苔藓80

<span id="dummy-sensors"></span>

## [dummy_sensors](https://github.com/ros2/demos/tree/kilted/dummy_robot/dummy_sensors/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714)) demo_nodes_cpp/CMakeLists.txt 需要cmake min 版本 3.12 其他模块 cmake 3.5 。它被提议与 3.12 版本标准化。它也修正 cmake \< 3.10 折旧警告

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#707](https://github.com/ros2/demos/issues/707))

- 更新假人\_ 感应器读取器以响应正确的话题( S)[\#675](https://github.com/ros2/demos/issues/675))

- 贡献者:谢恩·洛雷茨、jmackay2、苔藓80

<span id="examples-rclcpp-async-client"></span>

## [examples_rclcpp_async_client](https://github.com/ros2/examples/tree/kilted/rclcpp/services/async_client/CHANGELOG.rst)

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 撰稿人:谢恩·洛雷茨

<span id="examples-rclcpp-cbg-executor"></span>

## [examples_rclcpp_cbg_executor](https://github.com/ros2/examples/tree/kilted/rclcpp/executors/cbg_executor/CHANGELOG.rst)

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 撰稿人:谢恩·洛雷茨

<span id="examples-rclcpp-minimal-action-client"></span>

## [examples_rclcpp_minimal_action_client](https://github.com/ros2/examples/tree/kilted/rclcpp/actions/minimal_action_client/CHANGELOG.rst)

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 删除过时的注释( R)[\#388](https://github.com/ros2/examples/issues/388))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、谢恩·洛雷茨

<span id="examples-rclcpp-minimal-action-server"></span>

## [examples_rclcpp_minimal_action_server](https://github.com/ros2/examples/tree/kilted/rclcpp/actions/minimal_action_server/CHANGELOG.rst)

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 删除过时的注释( R)[\#388](https://github.com/ros2/examples/issues/388))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、谢恩·洛雷茨

<span id="examples-rclcpp-minimal-client"></span>

## [examples_rclcpp_minimal_client](https://github.com/ros2/examples/tree/kilted/rclcpp/services/minimal_client/CHANGELOG.rst)

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 撰稿人:谢恩·洛雷茨

<span id="examples-rclcpp-minimal-composition"></span>

## [examples_rclcpp_minimal_composition](https://github.com/ros2/examples/tree/kilted/rclcpp/composition/minimal_composition/CHANGELOG.rst)

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 撰稿人:谢恩·洛雷茨

<span id="examples-rclcpp-minimal-publisher"></span>

## [examples_rclcpp_minimal_publisher](https://github.com/ros2/examples/tree/kilted/rclcpp/topics/minimal_publisher/CHANGELOG.rst)

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 撰稿人:谢恩·洛雷茨

<span id="examples-rclcpp-minimal-service"></span>

## [examples_rclcpp_minimal_service](https://github.com/ros2/examples/tree/kilted/rclcpp/services/minimal_service/CHANGELOG.rst)

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 撰稿人:谢恩·洛雷茨

<span id="examples-rclcpp-minimal-subscriber"></span>

## [examples_rclcpp_minimal_subscriber](https://github.com/ros2/examples/tree/kilted/rclcpp/topics/minimal_subscriber/CHANGELOG.rst)

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 撰稿人:谢恩·洛雷茨

<span id="examples-rclcpp-minimal-timer"></span>

## [examples_rclcpp_minimal_timer](https://github.com/ros2/examples/tree/kilted/rclcpp/timers/minimal_timer/CHANGELOG.rst)

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 撰稿人:谢恩·洛雷茨

<span id="examples-rclcpp-multithreaded-executor"></span>

## [examples_rclcpp_multithreaded_executor](https://github.com/ros2/examples/tree/kilted/rclcpp/executors/multithreaded_executor/CHANGELOG.rst)

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 撰稿人:谢恩·洛雷茨

<span id="examples-rclcpp-wait-set"></span>

## [examples_rclcpp_wait_set](https://github.com/ros2/examples/tree/kilted/rclcpp/wait_set/CHANGELOG.rst)

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#404](https://github.com/ros2/examples/issues/404))

- 撰稿人:谢恩·洛雷茨

<span id="examples-rclpy-executors"></span>

## [examples_rclpy_executors](https://github.com/ros2/examples/tree/kilted/rclpy/executors/CHANGELOG.rst)

- 在 ament_xmllint 中为 ament_python 包添加. ([\#397](https://github.com/ros2/examples/issues/397))

- 切换到在任何地方使用rclpy上下文管理器 。 ()[\#389](https://github.com/ros2/examples/issues/389))

- 更新所有 Python 实例中的关机处理 。 ()[\#379](https://github.com/ros2/examples/issues/379))

- 撰稿人:克里斯·拉兰谢特

<span id="examples-rclpy-guard-conditions"></span>

## [examples_rclpy_guard_conditions](https://github.com/ros2/examples/tree/kilted/rclpy/guard_conditions/CHANGELOG.rst)

- 在 ament_xmllint 中为 ament_python 包添加. ([\#397](https://github.com/ros2/examples/issues/397))

- 切换到在任何地方使用rclpy上下文管理器 。 ()[\#389](https://github.com/ros2/examples/issues/389))

- 更新所有 Python 实例中的关机处理 。 ()[\#379](https://github.com/ros2/examples/issues/379))

- 撰稿人:克里斯·拉兰谢特

<span id="examples-rclpy-minimal-action-client"></span>

## [examples_rclpy_minimal_action_client](https://github.com/ros2/examples/tree/kilted/rclpy/actions/minimal_action_client/CHANGELOG.rst)

- 在 ament_xmllint 中为 ament_python 包添加. ([\#397](https://github.com/ros2/examples/issues/397))

- 切换到在任何地方使用rclpy上下文管理器 。 ()[\#389](https://github.com/ros2/examples/issues/389))

- 更新所有 Python 实例中的关机处理 。 ()[\#379](https://github.com/ros2/examples/issues/379))

- 撰稿人:克里斯·拉兰谢特

<span id="examples-rclpy-minimal-action-server"></span>

## [examples_rclpy_minimal_action_server](https://github.com/ros2/examples/tree/kilted/rclpy/actions/minimal_action_server/CHANGELOG.rst)

- 在 ament_xmllint 中为 ament_python 包添加. ([\#397](https://github.com/ros2/examples/issues/397))

- 切换到在任何地方使用rclpy上下文管理器 。 ()[\#389](https://github.com/ros2/examples/issues/389))

- 在 Python 单目标动作服务器实例上添加守护( P)[\#380](https://github.com/ros2/examples/issues/380))

- 更新所有 Python 实例中的关机处理 。 ()[\#379](https://github.com/ros2/examples/issues/379))

- 贡献者:克里斯·拉兰谢特、鲁迪克·劳伦斯

<span id="examples-rclpy-minimal-client"></span>

## [examples_rclpy_minimal_client](https://github.com/ros2/examples/tree/kilted/rclpy/services/minimal_client/CHANGELOG.rst)

- 在 ament_xmllint 中为 ament_python 包添加. ([\#397](https://github.com/ros2/examples/issues/397))

- 切换到在任何地方使用rclpy上下文管理器 。 ()[\#389](https://github.com/ros2/examples/issues/389))

- 在客户端\_ async\_ callback 中使用单个执行器实例旋转 。 ([\#382](https://github.com/ros2/examples/issues/382))

- 更新所有 Python 实例中的关机处理 。 ()[\#379](https://github.com/ros2/examples/issues/379))

- 撰稿人:克里斯·拉兰谢特

<span id="examples-rclpy-minimal-publisher"></span>

## [examples_rclpy_minimal_publisher](https://github.com/ros2/examples/tree/kilted/rclpy/topics/minimal_publisher/CHANGELOG.rst)

- 将 Flake8 错误处理为例_rclpy_minimal_publisher ()[\#410](https://github.com/ros2/examples/issues/410))

- 添加发布器\_ 成员\_ 函数\_ with_wait\_ for\_ all_acked.py ()[\#407](https://github.com/ros2/examples/issues/407))

- 在 ament_xmllint 中为 ament_python 包添加. ([\#397](https://github.com/ros2/examples/issues/397))

- 切换到在任何地方使用rclpy上下文管理器 。 ()[\#389](https://github.com/ros2/examples/issues/389))

- 更新所有 Python 实例中的关机处理 。 ()[\#379](https://github.com/ros2/examples/issues/379))

- 撰稿人:克里斯·拉兰塞特、藤田友也

<span id="examples-rclpy-minimal-service"></span>

## [examples_rclpy_minimal_service](https://github.com/ros2/examples/tree/kilted/rclpy/services/minimal_service/CHANGELOG.rst)

- 在 ament_xmllint 中为 ament_python 包添加. ([\#397](https://github.com/ros2/examples/issues/397))

- 切换到在任何地方使用rclpy上下文管理器 。 ()[\#389](https://github.com/ros2/examples/issues/389))

- 更新所有 Python 实例中的关机处理 。 ()[\#379](https://github.com/ros2/examples/issues/379))

- 撰稿人:克里斯·拉兰谢特

<span id="examples-rclpy-minimal-subscriber"></span>

## [examples_rclpy_minimal_subscriber](https://github.com/ros2/examples/tree/kilted/rclpy/topics/minimal_subscriber/CHANGELOG.rst)

- 在 ament_xmllint 中为 ament_python 包添加. ([\#397](https://github.com/ros2/examples/issues/397))

- 切换到在任何地方使用rclpy上下文管理器 。 ()[\#389](https://github.com/ros2/examples/issues/389))

- 更新所有 Python 实例中的关机处理 。 ()[\#379](https://github.com/ros2/examples/issues/379))

- 撰稿人:克里斯·拉兰谢特

<span id="examples-rclpy-pointcloud-publisher"></span>

## [examples_rclpy_pointcloud_publisher](https://github.com/ros2/examples/tree/kilted/rclpy/topics/pointcloud_publisher/CHANGELOG.rst)

- 在 ament_xmllint 中为 ament_python 包添加. ([\#397](https://github.com/ros2/examples/issues/397))

- 切换到在任何地方使用rclpy上下文管理器 。 ()[\#389](https://github.com/ros2/examples/issues/389))

- 更新所有 Python 实例中的关机处理 。 ()[\#379](https://github.com/ros2/examples/issues/379))

- 撰稿人:克里斯·拉兰谢特

<span id="examples-tf2-py"></span>

## [examples_tf2_py](https://github.com/ros2/geometry2/tree/kilted/examples_tf2_py/CHANGELOG.rst)

- 在测试_xmllint中添加几何2 python 包。 ([\#725](https://github.com/ros2/geometry2/issues/725))

- 切换为 python 示例使用上下文管理器 。 ()[\#700](https://github.com/ros2/geometry2/issues/700)那样我们就能确保永远清理干净,但使用更少的代码这样做.

- 撰稿人:克里斯·拉兰谢特

<span id="foonathan-memory-vendor"></span>

## [foonathan_memory_vendor](https://github.com/eProsima/foonathan_memory_vendor/tree/master/CHANGELOG.rst)

- 改进安装 founathan_memory 的机制(# 67)

<span id="geometry2"></span>

## [几何学2](https://github.com/ros2/geometry2/tree/kilted/geometry2/CHANGELOG.rst)

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 贡献者:苔藓80

<span id="geometry-msgs"></span>

## [geometry_msgs](https://github.com/ros2/common_interfaces/tree/kilted/geometry_msgs/CHANGELOG.rst)

- 完全清除 Pose 标定的阵列([\#270](https://github.com/ros2/common_interfaces/issues/270))

- 将几何\_ msgs/ PoseStamped Array 移动到 nav\_ msgs/ 目标([\#269](https://github.com/ros2/common_interfaces/issues/269))

- 添加 PoseStamped 阵列([\#262](https://github.com/ros2/common_interfaces/issues/262))

- 贡献者:Tony Najjar、Tully Foote

<span id="gmock-vendor"></span>

## [gmock_vendor](https://github.com/ament/googletest/tree/kilted/googlemock/CHANGELOG.rst)

- 弹出最小CMake版本为3.15([\#31](https://github.com/ament/googletest/issues/31))

- 贡献者:苔藓80

<span id="google-benchmark-vendor"></span>

## [google_benchmark_vendor](https://github.com/ament/google_benchmark_vendor/tree/kilted/CHANGELOG.rst)

- 弹出最小CMake版本为3.10([\#35](https://github.com/ament/google_benchmark_vendor/issues/35))

- 删除 CODEOWINERS 和镜像滚动到主工作流程 。 ([\#31](https://github.com/ament/google_benchmark_vendor/issues/31))

- 贡献者:克里斯·拉朗谢特,苔藓80

<span id="gtest-vendor"></span>

## [gtest_vendor](https://github.com/ament/googletest/tree/kilted/googletest/CHANGELOG.rst)

- 弹出最小CMake版本为3.15([\#33](https://github.com/ament/googletest/issues/33))

- 贡献者:苔藓80

<span id="gz-cmake-vendor"></span>

## [gz_cmake_vendor](https://github.com/gazebo-release/gz_cmake_vendor/tree/kilted/CHANGELOG.rst)

- 弹跳版本改为4.1.1([\#13](https://github.com/gazebo-release/gz_cmake_vendor/issues/13))

- 弹出版本为4.1.0([\#11](https://github.com/gazebo-release/gz_cmake_vendor/issues/11))

- 弹出版本为4.0.0([\#10](https://github.com/gazebo-release/gz_cmake_vendor/issues/10))

- 修复搜索时使用的cmake-配置( S)[\#8](https://github.com/gazebo-release/gz_cmake_vendor/issues/8)提供的cmake-config 如果有的话,实际上没有工作 `` ` find_package(gz_cmake_vendor) find_package(gz-cmake) ` `` 这是因为配置文件试图为不存在的目标创建别名。 例如, gz- cmake4: gz- cmake4 不是由 gz- cmake 导出 。

- 删除 BUILD_DOCS 参数 。 ()[\#9](https://github.com/gazebo-release/gz_cmake_vendor/issues/9)它显然在较新的Gazebo被贬值。

- 应用预释后缀并删除补丁( Q)[\#7](https://github.com/gazebo-release/gz_cmake_vendor/issues/7))

- 升级到离子体

- 贡献者:Addiu Z. Taddese、Chris Lalancette、Steve Peters

<span id="gz-math-vendor"></span>

## [gz_math_vendor](https://github.com/gazebo-release/gz_math_vendor/tree/kilted/CHANGELOG.rst)

- 弹出版本改为8.1.1([\#10](https://github.com/gazebo-release/gz_math_vendor/issues/10))

- 弹跳版本改为8.1.0([\#8](https://github.com/gazebo-release/gz_math_vendor/issues/8)\* 这是一次重新发行,因为7号供应商包的版本实际上没有撞上。

- 弹跳版本改为8.1.0([\#7](https://github.com/gazebo-release/gz_math_vendor/issues/7))

- 弹出版本为8.0.0([\#5](https://github.com/gazebo-release/gz_math_vendor/issues/5))

- 应用预释后缀( E)[\#4](https://github.com/gazebo-release/gz_math_vendor/issues/4))

- 升级到离子体

- 将供应商软件包版本更新到7.5.0

- 贡献者:Addiu Z. Taddese、Carlos Agüero、Michael Carroll

<span id="gz-utils-vendor"></span>

## [gz_utils_vendor](https://github.com/gazebo-release/gz_utils_vendor/tree/kilted/CHANGELOG.rst)

- 弹出版本改为3.1.1([\#10](https://github.com/gazebo-release/gz_utils_vendor/issues/10))

- 弹出版本改为 3.1.0 ([\#8](https://github.com/gazebo-release/gz_utils_vendor/issues/8))

- 弹跳版本为 3. 0. 0 ([\#7](https://github.com/gazebo-release/gz_utils_vendor/issues/7))

- 在依赖 spdlog\_ vendor 时添加 ()[\#6](https://github.com/gazebo-release/gz_utils_vendor/issues/6)\* 在依赖spdlog_vendor时添加。 这样, 在建立Windows 时, 在尝试构建此供应商软件包之前, 将适当设置用于spdlog 的路径 。 \* 也去掉对spdlog 的依赖性 。 这是因为我们只是依赖供应商软件包来在必要时为我们提供这种依赖性 。\\\\ \\\\\\\\

- 删除 BUILD_DOCS 参数 。 ()[\#5](https://github.com/gazebo-release/gz_utils_vendor/issues/5)它显然在较新的Gazebo被贬值。

- 应用预释后缀( E)[\#4](https://github.com/gazebo-release/gz_utils_vendor/issues/4))

- 升级到离子体

- 贡献者:Addiu Z. Taddese、Carlos Agüero、Chris Lalancette、Michael Carroll

<span id="image-tools"></span>

## [image_tools](https://github.com/ros2/demos/tree/kilted/image_tools/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 缩写图像\_ 工具/ CMakeLists. txt ()[\#712](https://github.com/ros2/demos/issues/712))

- 设定 Envars 以 rmw_zenoh_cpp 进行多播发现的测试([\#711](https://github.com/ros2/demos/issues/711))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、莫斯费特80、亚敦德

<span id="image-transport"></span>

## [image_transport](https://github.com/ros-perception/image_common/tree/kilted/image_transport/CHANGELOG.rst)

- 删除窗口警告( E)[\#350](https://github.com/ros-perception/image_common/issues/350))

- 添加 `rclcpp::shutdown` ([\#347](https://github.com/ros-perception/image_common/issues/347))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#345](https://github.com/ros-perception/image_common/issues/345))

- 成就: python 绑定图像\_ 传输和发布( )[\#323](https://github.com/ros-perception/image_common/issues/323))合著:亚历杭德罗·埃尔南德斯·科尔德罗 \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 在创建针对运输的专题之前, 将重映射应用到基础主题( R)[\#326](https://github.com/ros-perception/image_common/issues/326))

- 添加懒惰订阅到重印器( N)[\#325](https://github.com/ros-perception/image_common/issues/325))

- 固定节点名称( N)[\#321](https://github.com/ros-perception/image_common/issues/321))

- 已更新的已折旧信件过滤信头( E)[\#320](https://github.com/ros-perception/image_common/issues/320))

- 删除过时的注释( R)[\#319](https://github.com/ros-perception/image_common/issues/319))

- 准备 qos 折旧 ([\#315](https://github.com/ros-perception/image_common/issues/315))

- 删除警告( R)[\#312](https://github.com/ros-perception/image_common/issues/312))

- 支持零拷贝的流程内出版([\#306](https://github.com/ros-perception/image_common/issues/306))

- 添加缺失的子和阴极选项( E)[\#308](https://github.com/ros-perception/image_common/issues/308)共同撰写:Angsa部署小组 \<[team@angsa-robotics.com](mailto:team%40angsa-robotics.com)\>

- 撰稿人:亚历杭德罗·埃尔南德斯·科尔德罗、布瓦·埃伊·索瓦、费尔迪·塔马斯、卢卡斯·温德兰、米哈尔·索伊卡、谢恩·洛雷茨、托尼·纳贾尔、袁远雄

<span id="image-transport-py"></span>

## [image_transport_py](https://github.com/ros-perception/image_common/tree/kilted/image_transport_py/CHANGELOG.rst)

- 在 python3-dev 构建依赖中添加([\#334](https://github.com/ros-perception/image_common/issues/334))

- 成就: python 绑定图像\_ 传输和发布( )[\#323](https://github.com/ros-perception/image_common/issues/323))合著:亚历杭德罗·埃尔南德斯·科尔德罗 \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 撰稿人:克里斯·拉兰谢特、费尔迪·塔马斯

<span id="interactive-markers"></span>

## [interactive_markers](https://github.com/ros-visualization/interactive_markers/tree/kilted/CHANGELOG.rst)

- 折旧 tf2 C 信头( C)[\#109](https://github.com/ros-visualization/interactive_markers/issues/109))

- 删除 CODEOWINERS 和镜像滚动到主工作流程([\#110](https://github.com/ros-visualization/interactive_markers/issues/110))

- 使用未贬值的 API (E)[\#108](https://github.com/ros-visualization/interactive_markers/issues/108))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、卢卡斯·温德兰

<span id="intra-process-demo"></span>

## [intra_process_demo](https://github.com/ros2/demos/tree/kilted/intra_process_demo/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 设定 Envars 以 rmw_zenoh_cpp 进行多播发现的测试([\#711](https://github.com/ros2/demos/issues/711))

- 已删除打开cv3的预编译器检查([\#695](https://github.com/ros2/demos/issues/695))

- \[ intra\_ process_demo\] 在 README.md 修复中可执行名称([\#690](https://github.com/ros2/demos/issues/690))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、特鲁尚特·阿德斯哈拉、莫斯费特80

<span id="kdl-parser"></span>

## [kdl_parser](https://github.com/ros/kdl_parser/tree/kilted/kdl_parser/CHANGELOG.rst)

- 更新 urdf 模型头( S)[\#85](https://github.com/ros/kdl_parser/issues/85))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗

<span id="laser-geometry"></span>

## [laser_geometry](https://github.com/ros-perception/laser_geometry/tree/kilted/CHANGELOG.rst)

- 折旧 tf2 C 信头( C)[\#98](https://github.com/ros-perception/laser_geometry/issues/98))

- 删除 CODEOWINERS 和镜像滚动到主工作流程([\#100](https://github.com/ros-perception/laser_geometry/issues/100))

- 停止使用 python\_ cmake_模块. ()[\#93](https://github.com/ros-perception/laser_geometry/issues/93))

- 添加的常见线条( N)[\#96](https://github.com/ros-perception/laser_geometry/issues/96))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、卢卡斯·温德兰

<span id="launch"></span>

## [发射](https://github.com/ros2/launch/tree/kilted/launch/CHANGELOG.rst)

- 向定时行动实体提供发射配置复制件([\#836](https://github.com/ros2/launch/issues/836))

- 允许对 PathJoin 替换中的每个路径组件进行调色( P)[\#838](https://github.com/ros2/launch/issues/838))

- 添加 StringJoin 替换( R)[\#843](https://github.com/ros2/launch/issues/843))

- 添加缺失的发射测试(\_D)[\#850](https://github.com/ros2/launch/issues/850))

- 架构 doc 中的文档替换调值( C)[\#845](https://github.com/ros2/launch/issues/845))

- 更新文件以使用合适的 RST 字元( Q)[\#837](https://github.com/ros2/launch/issues/837))

- 固定函数参数缩进( F)[\#833](https://github.com/ros2/launch/issues/833))

- 添加 ForEach 动作以重复实体使用迭代特定值([\#802](https://github.com/ros2/launch/issues/802))

- 创建 py.typed ()[\#828](https://github.com/ros2/launch/issues/828))

- 通过在例外中添加文件位置来改进错误报告( E)[\#823](https://github.com/ros2/launch/issues/823))

- 增加涉及E标记的替换边框的试验范围([\#824](https://github.com/ros2/launch/issues/824))

- 清理发射的依赖性。 ([\#819](https://github.com/ros2/launch/issues/819))

- 修复 " 设置 " 类型( E)[\#813](https://github.com/ros2/launch/issues/813))

- 在所有 ment_python 包中添加 test_xmllint 。 ([\#804](https://github.com/ros2/launch/issues/804))

- 修改备注中的打字符( E)[\#783](https://github.com/ros2/launch/issues/783))

- 贡献者:克里斯·拉兰谢特、克里斯蒂安·鲁夫、克里斯托弗·贝达德、迈克尔·卡尔斯特罗姆、罗兰·阿森诺、达尼尔克兰斯顿

<span id="launch-pytest"></span>

## [launch_pytest](https://github.com/ros2/launch/tree/kilted/launch_pytest/CHANGELOG.rst)

- 清理发射的依赖性。 ([\#819](https://github.com/ros2/launch/issues/819))

- 在所有 ment_python 包中添加 test_xmllint 。 ([\#804](https://github.com/ros2/launch/issues/804))

- 切换到使用rclpy上下文管理器 。 ()[\#787](https://github.com/ros2/launch/issues/787))

- 撰稿人:克里斯·拉兰谢特

<span id="launch-ros"></span>

## [launch_ros](https://github.com/ros2/launch_ros/tree/kilted/launch_ros/CHANGELOG.rst)

- 取消自领导斜线事项以来的斜线剥离([\#456](https://github.com/ros2/launch_ros/issues/456))

- 修复生命周期节点自动启动问题 [\#445](https://github.com/ros2/launch_ros/issues/445) ([\#449](https://github.com/ros2/launch_ros/issues/449))

- 将 docstring 标记下码块改为 RST ()[\#450](https://github.com/ros2/launch_ros/issues/450))

- 自动启动生命周期节点和实例启动文件演示( E)[\#430](https://github.com/ros2/launch_ros/issues/430))

- 为 str 类型添加 YAML 倾斜器代表符,以保留引号 。 ()[\#436](https://github.com/ros2/launch_ros/issues/436))

- 模拟发射组件导致 rosdoc2 失败 Python API([\#425](https://github.com/ros2/launch_ros/issues/425))

- 在 ment_python 包中添加 ament_xmllint 。 ([\#423](https://github.com/ros2/launch_ros/issues/423))

- 在设置中修正url.py([\#413](https://github.com/ros2/launch_ros/issues/413))

- 贡献者:克里斯·拉兰谢特,克里斯托弗·贝达德,奥利维亚/F.F.,R·肯特·詹姆斯,史蒂夫·马肯斯基,藤田友也,魏HU

<span id="launch-testing"></span>

## [launch_testing](https://github.com/ros2/launch/tree/kilted/launch_testing/CHANGELOG.rst)

- 固定函数参数缩进( F)[\#833](https://github.com/ros2/launch/issues/833))

- 清理发射的依赖性。 ([\#819](https://github.com/ros2/launch/issues/819))

- 在所有 ment_python 包中添加 test_xmllint 。 ([\#804](https://github.com/ros2/launch/issues/804))

- 添加机制,使依赖群体无法工作([\#775](https://github.com/ros2/launch/issues/775))

- 贡献者:克里斯·拉兰谢特、克里斯托弗·贝达德、斯科特·K·洛根

<span id="launch-testing-ament-cmake"></span>

## [launch_testing_ament_cmake](https://github.com/ros2/launch/tree/kilted/launch_testing_ament_cmake/CHANGELOG.rst)

- 添加 CMake 参数以覆盖启动_测试模块([\#854](https://github.com/ros2/launch/issues/854))

- 停止使用 python\_ cmake_模块. ()[\#760](https://github.com/ros2/launch/issues/760))

- 使用发射测试时不要写 Python 字节代码([\#785](https://github.com/ros2/launch/issues/785))

- 贡献者:克里斯·拉兰谢特、斯科特·K·洛根

<span id="launch-testing-examples"></span>

## [launch_testing_examples](https://github.com/ros2/examples/tree/kilted/launch_testing/launch_testing_examples/CHANGELOG.rst)

- 添加 test_xmllint.py. (中文(简体) ).[\#401](https://github.com/ros2/examples/issues/401))

- 撰稿人:克里斯·拉兰谢特

<span id="launch-testing-ros"></span>

## [launch_testing_ros](https://github.com/ros2/launch_ros/tree/kilted/launch_testing_ros/CHANGELOG.rst)

- `WaitForTopics`:让用户在启动订阅者后注入一个要执行的触发函数([\#356](https://github.com/ros2/launch_ros/issues/356))

- 添加启动 rmw\_ test\_ fixture 的启用Rmwsolation 动作 ()[\#459](https://github.com/ros2/launch_ros/issues/459))

- 固定函数参数缩进( F)[\#446](https://github.com/ros2/launch_ros/issues/446))

- 在 ment_python 包中添加 ament_xmllint 。 ([\#423](https://github.com/ros2/launch_ros/issues/423))

- 切换到使用 rclpy.init 上下文管理器 。 ()[\#402](https://github.com/ros2/launch_ros/issues/402))

- 贡献者:克里斯·拉朗谢特、克里斯托弗·贝达德、乔治·平陶迪、斯科特·K·洛根

<span id="launch-xml"></span>

## [launch_xml](https://github.com/ros2/launch/tree/kilted/launch_xml/CHANGELOG.rst)

- 添加 ForEach 动作以重复实体使用迭代特定值([\#802](https://github.com/ros2/launch/issues/802))

- 在启动时停止加载扩展( X), yaml) 测试 。 ([\#820](https://github.com/ros2/launch/issues/820))

- 清理发射的依赖性。 ([\#819](https://github.com/ros2/launch/issues/819))

- 在所有 ment_python 包中添加 test_xmllint 。 ([\#804](https://github.com/ros2/launch/issues/804))

- 贡献者:克里斯·拉朗谢特、克里斯托弗·贝达德

<span id="launch-yaml"></span>

## [launch_yaml](https://github.com/ros2/launch/tree/kilted/launch_yaml/CHANGELOG.rst)

- 添加 ForEach 动作以重复实体使用迭代特定值([\#802](https://github.com/ros2/launch/issues/802))

- 在启动时停止加载扩展( X), yaml) 测试 。 ([\#820](https://github.com/ros2/launch/issues/820))

- 清理发射的依赖性。 ([\#819](https://github.com/ros2/launch/issues/819))

- 在所有 ment_python 包中添加 test_xmllint 。 ([\#804](https://github.com/ros2/launch/issues/804))

- 贡献者:克里斯·拉朗谢特、克里斯托弗·贝达德

<span id="libcurl-vendor"></span>

## [libcurl_vendor](https://github.com/ros/resource_retriever/tree/kilted/libcurl_vendor/CHANGELOG.rst)

- 制服 MinCMakeVersion (英语:[\#108](https://github.com/ros/resource_retriever/issues/108))

- 在 Windows 卷曲搜索路径中添加“ lib ” 。 ([\#96](https://github.com/ros/resource_retriever/issues/96)在 CMake 3.3 中,一个承诺使 CMake 中的 find\_ package 模块具有兼容性模式,可以自动在一个 \< prefix \>/lib 子目录中搜索软件包。在 CMake 3.6 中,所有平台都恢复了这种兼容性模式。 *除外* Windows。这意味着,自CMake 3.3以来,我们实际上没有按照 CMake 中指定的路径使用过 `curl_DIR`, 但我们却无意中依赖了这种倒置行为。 在 CMake 3.28 中, 兼容模式也被Windows 移除, 这意味着我们现在无法在下游软件包中找到\_ package( curl) (像资源回收器) 。 在“ lib” 目录中添加应该一直存在的内容来解决这个问题。 我会注意到这一点 。 *仅限* 影响我们的 Windows 构建, 因为此代码处于 if( WIN32) 块中 。

- 贡献者:克里斯·拉朗谢特,苔藓80

<span id="liblz4-vendor"></span>

## [liblz4_vendor](https://github.com/ros2/rosbag2/tree/kilted/liblz4_vendor/CHANGELOG.rst)

- 在库中添加来自 Windows 上的 conda 的 lz4 前缀。 ()[\#1846](https://github.com/ros2/rosbag2/issues/1846))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特

<span id="libstatistics-collector"></span>

## [libstatistics_collector](https://github.com/ros-tooling/libstatistics_collector/tree/kilted/CHANGELOG.rst)

- 4.5.0至4.6.0的跳动编码cov/编码cov-动作

- 修正移动参数统计:: max_默认值( M)[\#201](https://github.com/ros-tooling/libstatistics_collector/issues/201))

- 已删除的已贬值类( N) :[\#200](https://github.com/ros-tooling/libstatistics_collector/issues/200))

- 固定: 添加空注( )[\#194](https://github.com/ros-tooling/libstatistics_collector/issues/194))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、西松大介、杰弗里·许、托巴博特\[方

<span id="libyaml-vendor"></span>

## [libyaml_vendor](https://github.com/ros2/libyaml_vendor/tree/kilted/CHANGELOG.rst)

- 如果没有设置, 只设置 CRT\_ SECURE\_ NO\_ WARNINGS 。 ([\#64](https://github.com/ros2/libyaml_vendor/issues/64))

- 撰稿人:克里斯·拉兰谢特

<span id="lifecycle"></span>

## [寿命周期](https://github.com/ros2/demos/tree/kilted/lifecycle/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#707](https://github.com/ros2/demos/issues/707))

- 贡献者:谢恩·洛雷茨、苔藓80

<span id="lifecycle-py"></span>

## [lifecycle_py](https://github.com/ros2/demos/tree/kilted/lifecycle_py/CHANGELOG.rst)

- 在所有的 Ament_python 包中添加 test_xmllint.py 。 ([\#704](https://github.com/ros2/demos/issues/704))

- 更改所有演示使用新的 rclpy 上下文管理器 。 ()[\#694](https://github.com/ros2/demos/issues/694))

- 撰稿人:克里斯·拉兰谢特

<span id="logging-demo"></span>

## [logging_demo](https://github.com/ros2/demos/tree/kilted/logging_demo/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 设定 Envars 以 rmw_zenoh_cpp 进行多播发现的测试([\#711](https://github.com/ros2/demos/issues/711))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#707](https://github.com/ros2/demos/issues/707))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、谢恩·洛雷茨、苔藓80

<span id="lttngpy"></span>

## [节点](https://github.com/ros2/ros2_tracing/tree/kilted/lttngpy/CHANGELOG.rst)

- 从 pybind11_add_模块中删除SHALED ()[\#154](https://github.com/ros2/ros2_tracing/issues/154))

- 添加 python3- dev 构建\_ 依赖到lttngpy. ()[\#146](https://github.com/ros2/ros2_tracing/issues/146))

- 不要试图建立在 BSD 之上([\#142](https://github.com/ros2/ros2_tracing/issues/142))

- 允许启用调用 `ros2 trace` 或跟踪动作( E)[\#137](https://github.com/ros2/ros2_tracing/issues/137))

- 删除 python\_ cmake_模块使用 。 ([\#91](https://github.com/ros2/ros2_tracing/issues/91))

- 将缺少的 pkg- config 依赖添加到lttngpy ()[\#130](https://github.com/ros2/ros2_tracing/issues/130))

- 贡献者:克里斯·拉朗谢特、克里斯托弗·贝达、内森·维贝·诺伊费尔特、斯科特·K·洛根、西尔维奥·特拉韦萨罗

<span id="mcap-vendor"></span>

## [mcap_vendor](https://github.com/ros2/rosbag2/tree/kilted/mcap_vendor/CHANGELOG.rst)

- 更新 mcap (英语).[\#1774](https://github.com/ros2/rosbag2/issues/1774)将 mcap cpp 更新到最后一个版本

- 将 mcap- releases-cpp- 更新到 CMakeLists.txt (中文(简体) ).[\#1612](https://github.com/ros2/rosbag2/issues/1612))

- 贡献者:苔藓80

<span id="message-filters"></span>

## [message_filters](https://github.com/ros2/message_filters/tree/kilted/CHANGELOG.rst)

- 删除窗口警告( R)[\#171](https://github.com/ros2/message_filters/issues/171))

- 使用 rclcpp 的节点界面执行更多通用用户([\#113](https://github.com/ros2/message_filters/issues/113))

- 地物/时间测序器python([\#156](https://github.com/ros2/message_filters/issues/156))

- 将同步\_ 到达\_ 时旗添加到近似TimeSynchronizer ()[\#166](https://github.com/ros2/message_filters/issues/166))

- 修补:添加 `rclcpp::shutdown` ([\#167](https://github.com/ros2/message_filters/issues/167))

- 修正类型:缓存. getLast Time - \> 缓存. getLast Time (Cache.[\#165](https://github.com/ros2/message_filters//issues/165))

- 在主题之间添加时间冲抵, 介于近似正弦态器之间( Q)[\#154](https://github.com/ros2/message_filters/issues/154))

- 删除 CODEOWINERS 和镜像滚动到主工作流程 。 ([\#158](https://github.com/ros2/message_filters/issues/158))

- 更新的 Python 文档([\#150](https://github.com/ros2/message_filters/issues/150))

- 添加输入对齐器过滤器( E)[\#148](https://github.com/ros2/message_filters/issues/148))

- 停止使用 python\_ cmake_模块. ()[\#114](https://github.com/ros2/message_filters/issues/114))

- 修改折旧电文中的措辞。 ([\#144](https://github.com/ros2/message_filters/issues/144))

- 将一些简化和调试应用到精确时间同步策略( Q)[\#142](https://github.com/ros2/message_filters/issues/142))

- 用于 [\#93](https://github.com/ros2/message_filters/issues/93) ([\#143](https://github.com/ros2/message_filters/issues/143))

- 获取空缓存的周围间隔时出现错误fix/segfault([\#116](https://github.com/ros2/message_filters/issues/116))

- 迁移到 C++11  variadic 模板([\#93](https://github.com/ros2/message_filters/issues/93))

- \[LastestTimeSync\]在混凝土供应之前,Synchronizeris开始时修复崩溃. (Synchronizeris).[\#137](https://github.com/ros2/message_filters/issues/137))

- 在 Windwos 上修正 cppch 警告([\#138](https://github.com/ros2/message_filters/issues/138))

- 正在添加 ament_lint_常见([\#120](https://github.com/ros2/message_filters/issues/120))

- 折旧所有 C 信头( E)[\#135](https://github.com/ros2/message_filters/issues/135))

- 清理( E)[\#134](https://github.com/ros2/message_filters/issues/134))

- 在 README.md 中固定索引.rst 的链接([\#133](https://github.com/ros2/message_filters/issues/133))

- 还原“添加明确的建筑师([\#129](https://github.com/ros2/message_filters/issues/129))” ([\#132](https://github.com/ros2/message_filters/issues/132))

- 固定: 倒计时 时间使用错误的时钟( Q)[\#118](https://github.com/ros2/message_filters/issues/118))

- 添加显式构造器( E)[\#129](https://github.com/ros2/message_filters/issues/129))

- 在订阅者中被折旧的qos\_ profile( ưμ㼯A)[\#127](https://github.com/ros2/message_filters/issues/127))

- 正在添加 cpplint (% 1)[\#125](https://github.com/ros2/message_filters/issues/125))

- 从 Wiki 移动 Docs ()[\#119](https://github.com/ros2/message_filters/issues/119))

- 正在添加林特\_ cmake (% 1)[\#126](https://github.com/ros2/message_filters/issues/126))

- 添加未验证的更改( N)[\#124](https://github.com/ros2/message_filters/issues/124))

- 添加版权线条( E)[\#122](https://github.com/ros2/message_filters/issues/122))

- 撰稿人:亚历杭德罗·埃尔南德斯·科德罗、克里斯·拉朗塞特、克里斯·韦赫特、克莱门特·丘平、多米尼克、丹尼斯博士、伊万·洛佩斯·布罗塞尼奥、卡尔维克、卢卡斯·温德兰、马蒂亚斯·霍洛赫、米哈尔·斯塔尼亚斯泽克、鲁斯、赛义夫·西季克、萨沙·阿诺德、于延元

<span id="mimick-vendor"></span>

## [mimick_vendor](https://github.com/ros2/mimick_vendor/tree/kilted/CHANGELOG.rst)

- 更新散列以修正窗口故障( U)[\#39](https://github.com/ros2/mimick_vendor/issues/39))

- 更新包含 DT_GNU_HASH 的承诺([\#37](https://github.com/ros2/mimick_vendor/issues/37))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特

<span id="nav-msgs"></span>

## [nav_msgs](https://github.com/ros2/common_interfaces/tree/kilted/nav_msgs/CHANGELOG.rst)

- 将几何\_ msgs/ PoseStamped Array 移动到 nav\_ msgs/ 目标([\#269](https://github.com/ros2/common_interfaces/issues/269))

- 贡献者: Tully Fote

<span id="orocos-kdl-vendor"></span>

## [orocos_kdl_vendor](https://github.com/ros2/orocos_kdl_vendor/tree/kilted/orocos_kdl_vendor/CHANGELOG.rst)

- 使用相同的cmake版本([\#36](https://github.com/ros2/orocos_kdl_vendor/issues/36))

- 解决与更新的 CMake 的兼容性问题 ([\#35](https://github.com/ros2/orocos_kdl_vendor/issues/35))

- 固定 : 添加 cxx\_ 标准以避免 c++ 检查错误 ([\#30](https://github.com/ros2/orocos_kdl_vendor/issues/30))

- 确保Orocos_kdl_vendor不会意外发现自己。 ()[\#27](https://github.com/ros2/orocos_kdl_vendor/issues/27))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、霍马洛佐阿十世、艾斯泰因·斯图尔

<span id="osrf-pycommon"></span>

## [osrf_pycommon](https://github.com/osrf/osrf_pycommon/tree/master/CHANGELOG.rst)

- 合并拉动请求 [\#103](https://github.com/osrf/osrf_pycommon/issues/103) 从克里斯托弗贝达/克里斯托弗贝达/ 固定-typo-on-each-动词

- 用设置对齐 stdeb 依赖性. py ()[\#101](https://github.com/osrf/osrf_pycommon/issues/101)4b2f3a8e4969f33dced1dc2db2296230e7a55b1d的后续行动

- 在已发布的 deb 版本中添加 QQ 上游后缀([\#102](https://github.com/osrf/osrf_pycommon/issues/102))使用debian版本的后缀,在字母顺序上落在后面,Apt似乎会给我们的软件包带来偏好。如果一个用户启用了分发由OSRF或ROS创建的软件包的寄存器,那么他们很可能希望使用这些软件包,而不是其平台所包装的软件包。

- 上传覆盖结果到编码cov ([\#100](https://github.com/osrf/osrf_pycommon/issues/100))

- 更新ci.yaml (中文(简体) ).[\#96](https://github.com/osrf/osrf_pycommon/issues/96)) 固定节点.js \<20 拆解 共同作者:斯科特·K·洛根 \<[logans@cottsay.net](mailto:logans%40cottsay.net)\>

- 更新的 python 版本 ([\#97](https://github.com/osrf/osrf_pycommon/issues/97)) Python 3.7版本自2023年6月27日起不再支持,共同编剧:Scott K Logan \<[logans@cottsay.net](mailto:logans%40cottsay.net)\>

- 运行测试时解决未解决的资源警告( E)[\#99](https://github.com/osrf/osrf_pycommon/issues/99))

- 更新发布 deb 平台( Q)[\#95](https://github.com/osrf/osrf_pycommon/issues/95)添加: \*Ubuntu Noble(24.04 LTS预发版) \*Debian Trixie(测试版) 降档: \*Debian Bullseye(旧时稳定版) 保留: \*Debian Bookworm(稳定版) \*Ubuntu C焦(20.04 LTS) \*Ubuntu Jammy(22.04 LTS)

- 移除 CODEOWINERS. (中文(简体) ).[\#98](https://github.com/osrf/osrf_pycommon/issues/98)它已经过时,不再达到预定目的。

- 贡献者:克里斯·拉兰谢特,克里斯托弗·贝达德,斯科特·K·洛根,史蒂文! Ragnarök, mossfet80

<span id="osrf-testing-tools-cpp"></span>

## [osrf_testing_tools_cpp](https://github.com/osrf/osrf_testing_tools_cpp/tree/kilted/osrf_testing_tools_cpp/CHANGELOG.rst)

- 更新 CMakeLists.txt (英语).[\#85](https://github.com/osrf/osrf_testing_tools_cpp/issues/85))

- 贡献者:苔藓80

<span id="pendulum-control"></span>

## [pendulum_control](https://github.com/ros2/demos/tree/kilted/pendulum_control/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 设定 Envars 以 rmw_zenoh_cpp 进行多播发现的测试([\#711](https://github.com/ros2/demos/issues/711))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#707](https://github.com/ros2/demos/issues/707))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、谢恩·洛雷茨、苔藓80

<span id="pendulum-msgs"></span>

## [pendulum_msgs](https://github.com/ros2/demos/tree/kilted/pendulum_msgs/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 贡献者:苔藓80

<span id="performance-test-fixture"></span>

## [performance_test_fixture](https://github.com/ros2/performance_test_fixture/tree/kilted/CHANGELOG.rst)

- 在Ubuntu Noble上建构时确定警告。 ([\#26](https://github.com/ros2/performance_test_fixture/issues/26))

- 撰稿人:克里斯·拉兰谢特

<span id="pluginlib"></span>

## [插件lib](https://github.com/ros/pluginlib/tree/kilted/CHANGELOG.rst)

- 重清理插件lib. (S)[\#265](https://github.com/ros/pluginlib/issues/265))

- 删除 CODEOWINERS 和镜像滚动到主工作流程([\#268](https://github.com/ros/pluginlib/issues/268))

- 修正小拼写错误( E)[\#260](https://github.com/ros/pluginlib/issues/260))

- 删除的折旧方法( E)[\#256](https://github.com/ros/pluginlib/issues/256))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,克里斯·拉兰谢特,大卫·V·卢!

<span id="point-cloud-transport"></span>

## [point_cloud_transport](https://github.com/ros-perception/point_cloud_transport/tree/kilted/point_cloud_transport/CHANGELOG.rst)

- 添加 `rclcpp::shutdown` ([\#110](https://github.com/ros-perception/point_cloud_transport/issues/110))

- 已更新的已折旧信件过滤信头( E)[\#94](https://github.com/ros-perception/point_cloud_transport/issues/94))

- 删除警告( R)[\#89](https://github.com/ros-perception/point_cloud_transport/issues/89))

- 重排器: qos 覆盖 pub 和 sub ([\#88](https://github.com/ros-perception/point_cloud_transport/issues/88))

- 停止使用ament_target_dependency. (中文(简体) ).[\#86](https://github.com/ros-perception/point_cloud_transport/issues/86)) 我们正慢慢地放弃使用它,所以不要在这里使用它。 当我们在这里的时候, 注意一些能让这个软件包变得更容易的事情: 1. 插件lib绝对是这个软件包的公共依赖。 因此, 我们只能依赖公共输出, 而且我们不需要将它连接到每个测试中。 但这也意味着我们不需要一些在加载器\_ fwds.hpp 中的前置声明, 因为我们可以直接通过头文件。 2. republish.hpp 根本不需要存在。 这是因为它只是一个头文件, 但执行是可执行的文件。 因此, 任何下游都无法使用它。 因此, 只需删除文件, 直接将声明放入 cpp 文件中。

- 撰稿人:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、袁汝庸

<span id="point-cloud-transport-py"></span>

## [point_cloud_transport_py](https://github.com/ros-perception/point_cloud_transport/tree/kilted/point_cloud_transport_py/CHANGELOG.rst)

- 在对蟒蛇3-dev的依赖中添加([\#103](https://github.com/ros-perception/point_cloud_transport/issues/103))

- 删除 python\_ cmake\_ 模块的使用 。 ()[\#63](https://github.com/ros-perception/point_cloud_transport/issues/63))

- 删除额外的分号( E)[\#98](https://github.com/ros-perception/point_cloud_transport/issues/98))

- 撰稿人: 克里斯·拉兰谢特,马努

<span id="python-orocos-kdl-vendor"></span>

## [python_orocos_kdl_vendor](https://github.com/ros2/orocos_kdl_vendor/tree/kilted/python_orocos_kdl_vendor/CHANGELOG.rst)

- 固定 : 使用获取内容\_ make 可用以修正 CMP0169 ([\#32](https://github.com/ros2/orocos_kdl_vendor/issues/32))

- 删除 python_cmake_模块的使用( Name[\#26](https://github.com/ros2/orocos_kdl_vendor/issues/26))

- 贡献者:克里斯·拉兰谢特、Homalozoa X

<span id="python-qt-binding"></span>

## [python_qt_binding](https://github.com/ros-visualization/python_qt_binding/tree/kilted/CHANGELOG.rst)

- 跳过运行 Windows 调试的测试 。 ()[\#142](https://github.com/ros-visualization/python_qt_binding/issues/142))

- 撰稿人:克里斯·拉兰谢特

<span id="qt-dotgraph"></span>

## [qt_dotgraph](https://github.com/ros-visualization/qt_gui_core/tree/kilted/qt_dotgraph/CHANGELOG.rst)

- 将 qt\_ dotgraph 转换为纯 Python 软件包 。 ([\#300](https://github.com/ros-visualization/qt_gui_core/issues/300))

- 清理qt_dotgraph,使测试更坚固. ().[\#296](https://github.com/ros-visualization/qt_gui_core/issues/296))

- 跳过运行 Windows 调试的测试 。 ()[\#292](https://github.com/ros-visualization/qt_gui_core/issues/292))

- 撰稿人:克里斯·拉兰谢特

<span id="qt-gui-cpp"></span>

## [qt_gui_cpp](https://github.com/ros-visualization/qt_gui_core/tree/kilted/qt_gui_cpp/CHANGELOG.rst)

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#302](https://github.com/ros-visualization/qt_gui_core/issues/302))

- 添加常见的 linters 并让它们快乐于 qt\_ gui\_ cpp ()[\#295](https://github.com/ros-visualization/qt_gui_core/issues/295))

- 已折旧的 h 信头( U)[\#294](https://github.com/ros-visualization/qt_gui_core/issues/294))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、谢恩·洛雷茨

<span id="quality-of-service-demo-cpp"></span>

## [quality_of_service_demo_cpp](https://github.com/ros2/demos/tree/kilted/quality_of_service_demo/rclcpp/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714)) demo_nodes_cpp/CMakeLists.txt 需要cmake min 版本 3.12 其他模块 cmake 3.5 。它被提议与 3.12 版本标准化。它也修正 cmake \< 3.10 折旧警告

- 贡献者:苔藓80

<span id="quality-of-service-demo-py"></span>

## [quality_of_service_demo_py](https://github.com/ros2/demos/tree/kilted/quality_of_service_demo/rclpy/CHANGELOG.rst)

- 在所有的 Ament_python 包中添加 test_xmllint.py 。 ([\#704](https://github.com/ros2/demos/issues/704))

- 更改所有演示使用新的 rclpy 上下文管理器 。 ()[\#694](https://github.com/ros2/demos/issues/694))

- 撰稿人:克里斯·拉兰谢特

<span id="rcl"></span>

## [rcl (中文(简体) ).](https://github.com/ros2/rcl/tree/kilted/rcl/CHANGELOG.rst)

- 设定 Envars 以 rmw_zenoh_cpp 进行多播发现的测试([\#1218](https://github.com/ros2/rcl/issues/1218))

- 修正信件头中的打字,包括文件([\#1219](https://github.com/ros2/rcl/issues/1219))

- 使用 rmw_event_type\_ is_支持([\#1214](https://github.com/ros2/rcl/issues/1214))

- 执行中无需增加公共符号能见度宏。 ([\#1213](https://github.com/ros2/rcl/issues/1213))

- 添加新接口以启用 intrapsection 用于动作([\#1207](https://github.com/ros2/rcl/issues/1207))

- 使用 FASTDDS_DEFAULT_PROFILES_FILE 代替。 ([\#1211](https://github.com/ros2/rcl/issues/1211))

- 提醒定时器测试期不要错过周期。 ([\#1209](https://github.com/ros2/rcl/issues/1209))

- fix( rcl\_ action): 允许在初始化期间将计时器传递给动作([\#1201](https://github.com/ros2/rcl/issues/1201)\* 固定( timer):在跳转调用时使用 popl 指针 界面描述没有明确指出一个 rcl_timer\_ t 可能不会被复制到周围。 因此, 用户可以这样做 。 在调用调用中使用已知的永远不变的指针, 我们避免了塞格法尔错误, 即使“ 用户” 决定复制 rcl_timer\_ t 周围 。

- 将 qos\_ profile_rosout\_ 默认值移动到 rmw ()[\#1195](https://github.com/ros2/rcl/issues/1195))

- 更新 rcl_wait_set_init 的示例用法,以通过正确的参数([\#1204](https://github.com/ros2/rcl/issues/1204))

- 清除许多 rcl ⁇ action 中的错误处理,\_lifecycle} 代码路径( )[\#1202](https://github.com/ros2/rcl/issues/1202)\* 缩短测试\_ action\_ server 设置中的延迟。 与其在设置 10 个进球之间等待 250 ms( 至少 2.5 秒) , 只需等待 100 ms , 就可以将它减少为 1 秒 。 \* 测试\_ action\_ server. cpp \* 在 rcl_node_type_cache\_ register_type () 中重新设置错误, 也就是说, 如果 rcutils\_ hash\_ map\_ set () 失败, 它会设置自己的错误, 从而压倒它会导致警告打印 。 如果安装了它, 请在设置 之前先清除它 。 \* 只需在 rcl_timer_init2 中避免对清除的警告 。 \* 记录它。 返回 rcl\_ node_type_cache_register_type 的值。 否则, 我们设置了错误, 但将 RCL\_ RET\_ OK 返回到上层, 这是奇数 。 \* 取消完全不必要的返回值翻译 。 这个生成的代码将 RCL 错误翻译为 RCL 错误, 这没有多大意义 。 \* 使用 rcl\_ timer\_ init2 功能启动禁用计时器 。 而不是启用它, 然后立即取消它 。 \* 不要覆盖 rcl\_ action_goal\_ handle_get_info() 的错误, 它已经设置了错误, 因此 rcl\_ action\_ server_goal\_ sicostages() 不应该再次设置它 。 在检查动作有效性时先设置新错误, 这样我们就可以避免错误路径中的丑陋警告 。 \* 在 rcl\_ 订阅\_ init 中移动选项的复制。 这样, 当我们在“ 失败” 情况下进行清理时, 选项实际存在并且是有效的 。 \* 这样做可以避免清理过程中的丑陋警告 。 \* 请确定设置错误时, rcl\_ action\_ get\_ service\_ name 与 动作\_ 客户端的生成代码匹配 。 \* 重置 RUPITLS\_ FAULT\_ INJECTION 测试中的错误 。 那样, 以后的失败就不会打印出丑陋错误字符串 。 \* 请确定返回 \_ rcl_parse_resource\_ match中的错误 。 也就是说, 如果 rcl\_ lexer\_ lookahead2 的话 。 \_expect() 返回一个错误, 我们应该将错误传递到更高层, 而不是忽略它 。 \* 不要用 rcl\_ validate\_ enclave\_ name\_ name\_ name\_ namedate\_ namespace\_ 大小设置错误 。 \* 添加注释, rmw\_ validate\_ namespace\_ 设置错误 。 \* 一定要在 rcl\_ node\_ type\_ cache\_ init 中重置错误 。 否则我们会得到警告, 要从 rcutils\_ hash\_ map_init 中重写错误 。 \* 在 rcl_publisher\_ is_valid 中有条件设置错误消息。 只有当 rcl_context\_ is\_ valid 不设置错误时才会这样做 。 \* 不要从 rclll上重写错误 。 \_node_get_logger_name。 它已经设置了故障大小写中的错误。 \* 在测试网络流端点时, 请确定重置错误 。 这是因为一些 RMW 执行可能不支持此特性, 从而设置错误 。 \* 请在 rcl\_ expand_topic_name 中重置错误 。 这样我们就可以为上层设置更多有用的错误 。 \* 清理.c 错误处理 。 特别是, 在进入错误处理路径时, 请不要覆盖错误, 这会清除警告 。 \* 请在 rcl\_ life 循环测试中重置错误 。 这样我们就不会在后续测试中获得丑陋的“ 重叠” 警告 。 @%%%%%%%%%%%% 。

- 使事件跳过更笼统。 ()[\#1197](https://github.com/ros2/rcl/issues/1197))

- 重清理测试_事件. cpp. (中文(简体) ).[\#1196](https://github.com/ros2/rcl/issues/1196))

- 清理测试_graph.cpp. (中文(简体) ).[\#1193](https://github.com/ros2/rcl/issues/1193))

- 期望在测试_图中至少有两个节点存在([\#1192](https://github.com/ros2/rcl/issues/1192))

- 将 RCL_RET_ACTION_xxxx升级为 40XX. (中文(简体) ).[\#1191](https://github.com/ros2/rcl/issues/1191))

- 修复 NULL 分配器和 racy 条件。 ([\#1188](https://github.com/ros2/rcl/issues/1188))

- 正确初始化类型散列计算中使用的字符数组 。 ()[\#1182](https://github.com/ros2/rcl/issues/1182))

- 超时增加([\#1181](https://github.com/ros2/rcl/issues/1181))

- 在 rmw_zenoh 上跳过一些事件测试([\#1180](https://github.com/ros2/rcl/issues/1180))

- doc:rcl_logb_spdlog是默认的杂音. ()[\#1177](https://github.com/ros2/rcl/issues/1177))

- 更新 rcl_wait 的文档 wait.h ([\#1176](https://github.com/ros2/rcl/issues/1176))

- 改变目标到期的起始时间([\#1121](https://github.com/ros2/rcl/issues/1121))

- 删除已贬值的本地主机\_% 1[\#1169](https://github.com/ros2/rcl/issues/1169))

- 在 rcl_validate_enclave_name\_ with_size() doc 中修正打字符([\#1168](https://github.com/ros2/rcl/issues/1168))

- 删除已贬值的 rcl_init_timer () ()[\#1167](https://github.com/ros2/rcl/issues/1167))

- 清理测试\_ 计数\_ 匹配测试 处理非 DDS  RMWs ()[\#1164](https://github.com/ros2/rcl/issues/1164)\* 在 测试\_ counter\_ counter\_ matched中设置一个类方法。 这样我们就可以在每次引用中传递更少的参数, 并允许我们在类中隐藏更多的执行 。 \* 在 测试\_ counter\_ matched中将“ ops” 重新命名为“ opts ” 。 它只是更好地反映这些结构是什么 。 \* 在测试\_ counter\_ counter\_ matched中, 清理带有范围_exit a exit\_ counter\_ matched 的酒吧/ subs 。 这只是确保它们总是被清理, 即使我们提早退出, 请注意我们特别这样做。 *没有* 用于测试\_ counter\_ matched\_ 函数, 因为清除是故意与其他测试互换的 。 \* 请检查 QoS 层是否兼容 。 有些 CMW 的兼容性可能与 DDS 不同, 所以请检查 RTW 层, 以查看我们期望的出版商和订阅数量 。

- 添加机制,使依赖群体无法工作([\#1151](https://github.com/ros2/rcl/issues/1151))

- 重映射\_ 缩写: 小打字符( E)[\#1158](https://github.com/ros2/rcl/issues/1158))

- 修复 Rmw\_ clocalneds 时间戳测试。 ([\#1156](https://github.com/ros2/rcl/issues/1156))

- 在使用Mimick的测试中添加“ mimick” 标签([\#1152](https://github.com/ros2/rcl/issues/1152))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、巴瑞·许、克里斯·拉朗谢特、克里斯托弗·贝达德、菲利克斯·彭兹林、G.A. v. Hoorn、Janosch Machowinski、斯科特·K·洛根、藤田丰也、亚都、雅敦德

<span id="rcl-action"></span>

## [rcl_action](https://github.com/ros2/rcl/tree/kilted/rcl_action/CHANGELOG.rst)

- 设定 Envars 以 rmw_zenoh_cpp 进行多播发现的测试([\#1218](https://github.com/ros2/rcl/issues/1218))

- 执行中无需增加公共符号能见度宏。 ([\#1213](https://github.com/ros2/rcl/issues/1213))

- 固定“ rcl\_ action\_ server\_ confition\_ action_introduction” : 不一致的 dll链接 。 ([\#1212](https://github.com/ros2/rcl/issues/1212))

- 添加新接口以启用 intrapsection 用于动作([\#1207](https://github.com/ros2/rcl/issues/1207))

- fix( rcl\_ action): 允许在初始化期间将计时器传递给动作([\#1201](https://github.com/ros2/rcl/issues/1201)\* 固定( timer):在跳转调用时使用 popl 指针 界面描述没有明确指出一个 rcl_timer\_ t 可能不会被复制到周围。 因此, 用户可以这样做 。 在调用调用中使用已知的永远不变的指针, 我们避免了塞格法尔错误, 即使“ 用户” 决定复制 rcl_timer\_ t 周围 。

- 为动作名称添加重映射解析度( N)[\#1170](https://github.com/ros2/rcl/issues/1170)\* 为动作名称添加了重映射解析度 \* Fix cpplint/uncrustify \* 在大小写解析度失败时简化了返回的错误代码。 \* 重命名了动作_name 字段以重映射\_ action_name name. \* 删除了不必要的解析\_ action_name stack 变量 \* 为动作名称重映射添加了测试度 \* 添加了使用本地参数重映射动作名称的测试度 {}

- 清除许多 rcl ⁇ action 中的错误处理,\_lifecycle} 代码路径( )[\#1202](https://github.com/ros2/rcl/issues/1202)\* 缩短测试\_ action\_ server 设置中的延迟。 与其在设置 10 个进球之间等待 250 ms( 至少 2.5 秒) , 只需等待 100 ms , 就可以将它减少为 1 秒 。 \* 测试\_ action\_ server. cpp \* 在 rcl_node_type_cache\_ register_type () 中重新设置错误, 也就是说, 如果 rcutils\_ hash\_ map\_ set () 失败, 它会设置自己的错误, 从而压倒它会导致警告打印 。 如果安装了它, 请在设置 之前先清除它 。 \* 只需在 rcl_timer_init2 中避免对清除的警告 。 \* 记录它。 返回 rcl\_ node_type_cache_register_type 的值。 否则, 我们设置了错误, 但将 RCL\_ RET\_ OK 返回到上层, 这是奇数 。 \* 取消完全不必要的返回值翻译 。 这个生成的代码将 RCL 错误翻译为 RCL 错误, 这没有多大意义 。 \* 使用 rcl\_ timer\_ init2 功能启动禁用计时器 。 而不是启用它, 然后立即取消它 。 \* 不要覆盖 rcl\_ action_goal\_ handle_get_info() 的错误, 它已经设置了错误, 因此 rcl\_ action\_ server_goal\_ sicostages() 不应该再次设置它 。 在检查动作有效性时先设置新错误, 这样我们就可以避免错误路径中的丑陋警告 。 \* 在 rcl\_ 订阅\_ init 中移动选项的复制。 这样, 当我们在“ 失败” 情况下进行清理时, 选项实际存在并且是有效的 。 \* 这样做可以避免清理过程中的丑陋警告 。 \* 请确定设置错误时, rcl\_ action\_ get\_ service\_ name 与 动作\_ 客户端的生成代码匹配 。 \* 重置 RUPITLS\_ FAULT\_ INJECTION 测试中的错误 。 那样, 以后的失败就不会打印出丑陋错误字符串 。 \* 请确定返回 \_ rcl_parse_resource\_ match中的错误 。 也就是说, 如果 rcl\_ lexer\_ lookahead2 的话 。 \_expect() 返回一个错误, 我们应该将错误传递到更高层, 而不是忽略它 。 \* 不要用 rcl\_ validate\_ enclave\_ name\_ name\_ name\_ namedate\_ namespace\_ 大小设置错误 。 \* 添加注释, rmw\_ validate\_ namespace\_ 设置错误 。 \* 一定要在 rcl\_ node\_ type\_ cache\_ init 中重置错误 。 否则我们会得到警告, 要从 rcutils\_ hash\_ map_init 中重写错误 。 \* 在 rcl_publisher\_ is_valid 中有条件设置错误消息。 只有当 rcl_context\_ is\_ valid 不设置错误时才会这样做 。 \* 不要从 rclll上重写错误 。 \_node_get_logger_name。 它已经设置了故障大小写中的错误。 \* 在测试网络流端点时, 请确定重置错误 。 这是因为一些 RMW 执行可能不支持此特性, 从而设置错误 。 \* 请在 rcl\_ expand_topic_name 中重置错误 。 这样我们就可以为上层设置更多有用的错误 。 \* 清理.c 错误处理 。 特别是, 在进入错误处理路径时, 请不要覆盖错误, 这会清除警告 。 \* 请在 rcl\_ life 循环测试中重置错误 。 这样我们就不会在后续测试中获得丑陋的“ 重叠” 警告 。 @%%%%%%%%%%%% 。

- 清理测试_graph.cpp. (中文(简体) ).[\#1193](https://github.com/ros2/rcl/issues/1193))

- 期望在测试_图中至少有两个节点存在([\#1192](https://github.com/ros2/rcl/issues/1192))

- 将 RCL_RET_ACTION_xxxx升级为 40XX. (中文(简体) ).[\#1191](https://github.com/ros2/rcl/issues/1191))

- 修复 NULL 分配器和 racy 条件。 ([\#1188](https://github.com/ros2/rcl/issues/1188))

- 超时增加([\#1181](https://github.com/ros2/rcl/issues/1181))

- 改变目标到期的起始时间([\#1121](https://github.com/ros2/rcl/issues/1121))

- 增加测试\_ 动作\_ 交互超时 。 ()[\#1172](https://github.com/ros2/rcl/issues/1172)虽然我不能在当地重现这个问题,但我怀疑只要等一秒钟实体准备就绪是不够的,特别是在Windows、Connext以及我们与其他测试平行运行时。 因此,在所有测试中增加rcl_wait()的超时时间,这有望足以使测试永远通过。

- 停止多次编译 rcl\_ 动作测试 。 ()[\#1165](https://github.com/ros2/rcl/issues/1165))我们不需要为每部RMW编译一次测试;我们只需编译一次,然后使用RMW_IMPLEMTION环境变量来运行不同RMW的测试。这可以加快编译速度。

- 撰稿人:亚历杭德罗·埃尔南德斯·科尔德罗,贝里·许,克里斯·拉朗谢特,亚诺施·麦克豪林斯基,贾斯图斯·布劳恩,藤田丰也,亚都,雅敦德

<span id="rcl-lifecycle"></span>

## [rcl_lifecycle](https://github.com/ros2/rcl/tree/kilted/rcl_lifecycle/CHANGELOG.rst)

- 添加 rcl\_ print\_ transition_map 。 ([\#1217](https://github.com/ros2/rcl/issues/1217))

- 在 rcl_生命周期中启用测试隔离( S)[\#1216](https://github.com/ros2/rcl/issues/1216))

- 清除许多 rcl ⁇ action 中的错误处理,\_lifecycle} 代码路径( )[\#1202](https://github.com/ros2/rcl/issues/1202)\* 缩短测试\_ action\_ server 设置中的延迟。 与其在设置 10 个进球之间等待 250 ms( 至少 2.5 秒) , 只需等待 100 ms , 就可以将它减少为 1 秒 。 \* 测试\_ action\_ server. cpp \* 在 rcl_node_type_cache\_ register_type () 中重新设置错误, 也就是说, 如果 rcutils\_ hash\_ map\_ set () 失败, 它会设置自己的错误, 从而压倒它会导致警告打印 。 如果安装了它, 请在设置 之前先清除它 。 \* 只需在 rcl_timer_init2 中避免对清除的警告 。 \* 记录它。 返回 rcl\_ node_type_cache_register_type 的值。 否则, 我们设置了错误, 但将 RCL\_ RET\_ OK 返回到上层, 这是奇数 。 \* 取消完全不必要的返回值翻译 。 这个生成的代码将 RCL 错误翻译为 RCL 错误, 这没有多大意义 。 \* 使用 rcl\_ timer\_ init2 功能启动禁用计时器 。 而不是启用它, 然后立即取消它 。 \* 不要覆盖 rcl\_ action_goal\_ handle_get_info() 的错误, 它已经设置了错误, 因此 rcl\_ action\_ server_goal\_ sicostages() 不应该再次设置它 。 在检查动作有效性时先设置新错误, 这样我们就可以避免错误路径中的丑陋警告 。 \* 在 rcl\_ 订阅\_ init 中移动选项的复制。 这样, 当我们在“ 失败” 情况下进行清理时, 选项实际存在并且是有效的 。 \* 这样做可以避免清理过程中的丑陋警告 。 \* 请确定设置错误时, rcl\_ action\_ get\_ service\_ name 与 动作\_ 客户端的生成代码匹配 。 \* 重置 RUPITLS\_ FAULT\_ INJECTION 测试中的错误 。 那样, 以后的失败就不会打印出丑陋错误字符串 。 \* 请确定返回 \_ rcl_parse_resource\_ match中的错误 。 也就是说, 如果 rcl\_ lexer\_ lookahead2 的话 。 \_expect() 返回一个错误, 我们应该将错误传递到更高层, 而不是忽略它 。 \* 不要用 rcl\_ validate\_ enclave\_ name\_ name\_ name\_ namedate\_ namespace\_ 大小设置错误 。 \* 添加注释, rmw\_ validate\_ namespace\_ 设置错误 。 \* 一定要在 rcl\_ node\_ type\_ cache\_ init 中重置错误 。 否则我们会得到警告, 要从 rcutils\_ hash\_ map_init 中重写错误 。 \* 在 rcl_publisher\_ is_valid 中有条件设置错误消息。 只有当 rcl_context\_ is\_ valid 不设置错误时才会这样做 。 \* 不要从 rclll上重写错误 。 \_node_get_logger_name。 它已经设置了故障大小写中的错误。 \* 在测试网络流端点时, 请确定重置错误 。 这是因为一些 RMW 执行可能不支持此特性, 从而设置错误 。 \* 请在 rcl\_ expand_topic_name 中重置错误 。 这样我们就可以为上层设置更多有用的错误 。 \* 清理.c 错误处理 。 特别是, 在进入错误处理路径时, 请不要覆盖错误, 这会清除警告 。 \* 请在 rcl\_ life 循环测试中重置错误 。 这样我们就不会在后续测试中获得丑陋的“ 重叠” 警告 。 @%%%%%%%%%%%% 。

- 修复 NULL 分配器和 racy 条件。 ([\#1188](https://github.com/ros2/rcl/issues/1188))

- 在 rcl_lifecycle_com_interface_t doc 中修复打字([\#1174](https://github.com/ros2/rcl/issues/1174))

- 在 test_rcl_lifecyc循环中修复内存泄漏. ().[\#1173](https://github.com/ros2/rcl/issues/1173)) 这可能是由于不良合并的结果。 但实际上我们正迫使 srv_change_state com_interface 成为无效的, 但却忘记尽早省去旧的指针。 因此, 在去“ fini ” 之前, 我们永远无法恢复旧的指针, 记忆会漏掉。 通过更早的记忆来修复这个指针 。

- 贡献者:克里斯·拉朗塞特、克里斯托弗·贝达德、斯科特·K·洛根、藤田友也

<span id="rcl-logging-noop"></span>

## [rcl_logging_noop](https://github.com/ros2/rcl_logging/tree/kilted/rcl_logging_noop/CHANGELOG.rst)

- rcl_logb_interface 仅是建构环境的有效路径 。 ([\#122](https://github.com/ros2/rcl_logging/issues/122))

- 读取 EADME 更新和一些清理。 ([\#120](https://github.com/ros2/rcl_logging/issues/120))

- 贡献者:藤田友也

<span id="rcl-logging-spdlog"></span>

## [rcl_logging_spdlog](https://github.com/ros2/rcl_logging/tree/kilted/rcl_logging_spdlog/CHANGELOG.rst)

- rcl_logb_interface 仅是建构环境的有效路径 。 ([\#122](https://github.com/ros2/rcl_logging/issues/122))

- 读取 EADME 更新和一些清理。 ([\#120](https://github.com/ros2/rcl_logging/issues/120))

- 最新贬值的API([\#117](https://github.com/ros2/rcl_logging/issues/117))

- 撰稿人:亚历杭德罗·埃尔南德斯·科尔德罗、藤田友也

<span id="rcl-yaml-param-parser"></span>

## [rcl_yaml_param_parser](https://github.com/ros2/rcl/tree/kilted/rcl_yaml_param_parser/CHANGELOG.rst)

- rcl_yaml_param_parser 测试中错误路径后清除错误 。 ([\#1203](https://github.com/ros2/rcl/issues/1203)这消除了试验中丑陋的“重叠”警告。

- 在使用Mimick的测试中添加“ mimick” 标签([\#1152](https://github.com/ros2/rcl/issues/1152))

- 贡献者:克里斯·拉兰谢特、斯科特·K·洛根

<span id="rclcpp"></span>

## [rclcpp](https://github.com/ros2/rclcpp/tree/kilted/rclcpp/CHANGELOG.rst)

- 修补一个种族条件( E) :[\#2819](https://github.com/ros2/rclcpp/issues/2819))

- 删除序列化模块中的冗余类型支持检查( E)[\#2808](https://github.com/ros2/rclcpp/issues/2808))

- 删除 get_typesupport\_ handle 执行 。 ([\#2806](https://github.com/ros2/rclcpp/issues/2806))

- 使用节点Paremeter Interface 而不是/parameter_event 来更新“ 使用\_ sim_time ” ([\#2378](https://github.com/ros2/rclcpp/issues/2378))

- 删除取消\_ clock_executor_promise\_. ([\#2797](https://github.com/ros2/rclcpp/issues/2797))

- 仅当 QoS 超过参数时, 才允许参数递归更新 。 ([\#2742](https://github.com/ros2/rclcpp/issues/2742))

- 从密码库中删除了后面的白空间 。 ()[\#2791](https://github.com/ros2/rclcpp/issues/2791))

- 扩展剂量弦 `get_rmw_qos_profile()` ([\#2787](https://github.com/ros2/rclcpp/issues/2787))

- 设定 Envars 以 rmw_zenoh_cpp 进行多播发现的测试([\#2776](https://github.com/ros2/rclcpp/issues/2776))

- 修补: clang 的编译符( T)[\#2775](https://github.com/ros2/rclcpp/issues/2775))

- 添加配置\_ introduction 的例外 doc 。 ()[\#2773](https://github.com/ros2/rclcpp/issues/2773))

- 功绩: 添加时钟候机器和时钟条件变异器( C)[\#2691](https://github.com/ros2/rclcpp/issues/2691))

- doc: 添加了警告, 不直接与 RCL\_ ROS\_ TIME 同步时钟([\#2768](https://github.com/ros2/rclcpp/issues/2768))

- 使用 rmw\_ event\_ type\_ is\_ 支持在测试\_ qos\_ event ()[\#2761](https://github.com/ros2/rclcpp/issues/2761))

- 支持动作类型支持助手( S)[\#2750](https://github.com/ros2/rclcpp/issues/2750))

- 对可移植性使用可能未使用的属性。 ()[\#2758](https://github.com/ros2/rclcpp/issues/2758))

- 执行者强的参考定律( U)[\#2745](https://github.com/ros2/rclcpp/issues/2745))

- 清理 <https://github.com/ros2/rclcpp/pull/2683> ([\#2714](https://github.com/ros2/rclcpp/issues/2714))

- 在 doc 区域中修复用于 get\_ service_typesupport_handle () 的打字功能[\#2751](https://github.com/ros2/rclcpp/issues/2751))

- 测试大小写和修复 <https://github.com/ros2/rclcpp/issues/2652> ([\#2713](https://github.com/ros2/rclcpp/issues/2713))

- 固定( 计时器): 在执行器线程终止后删除节点( T)[\#2737](https://github.com/ros2/rclcpp/issues/2737))

- 更新 spin_xxx 方法的 doc 部分 。 ([\#2730](https://github.com/ros2/rclcpp/issues/2730))

- 固定: rclpp 使用的曝光计时器: 等待( P)[\#2699](https://github.com/ros2/rclcpp/issues/2699))

- 使用 rmw_qos\_ profile_rosout\_ 默认而不是 rcl. ()[\#2663](https://github.com/ros2/rclcpp/issues/2663))

- 固定( 执行者): 仅添加了 beeing 后未执行的固定实体([\#2724](https://github.com/ros2/rclcpp/issues/2724))

- fix:使循环条件与描述一致([\#2726](https://github.com/ros2/rclcpp/issues/2726))

- 从 rCl 收集日志消息, 并重置 。 ()[\#2720](https://github.com/ros2/rclcpp/issues/2720))

- 修复临时的本地IPC出版物([\#2708](https://github.com/ros2/rclcpp/issues/2708))

- 将实际的QoS从rmw应用到IPC出版社。 ([\#2707](https://github.com/ros2/rclcpp/issues/2707))

- 在议题名称中添加对IPC问题的记录([\#2706](https://github.com/ros2/rclcpp/issues/2706))

- 固定 TestTime Source.ROS_time_valid_attach_detach. (中文(简体) ).[\#2700](https://github.com/ros2/rclcpp/issues/2700))

- 更新文件串 `rclcpp::Node::now()` ([\#2696](https://github.com/ros2/rclcpp/issues/2696))

- 在 rmw_connexts 上重新启用可执行器测试. ([\#2693](https://github.com/ros2/rclcpp/issues/2693))

- 在 Windows 上修正警告 。 ()[\#2692](https://github.com/ros2/rclcpp/issues/2692))

- 与 Connext 一起运行测试的 Omnibus 修正 ()[\#2684](https://github.com/ros2/rclcpp/issues/2684))

- 修补( 执行器): 如果在 rmw\_ wait 期间删除调用组, 则会修复 segfault ()[\#2683](https://github.com/ros2/rclcpp/issues/2683))

- 接受自定义的贷款分配符。 ([\#2672](https://github.com/ros2/rclcpp/issues/2672))

- 在“贷款医疗”的医生部分中修补几处字型。 ([\#2676](https://github.com/ros2/rclcpp/issues/2676))

- 在 AnyServiceCallback 中确保触发回调_端端的跟踪点([\#2670](https://github.com/ros2/rclcpp/issues/2670))

- 更正通用_客户端.hpp中的错误注释([\#2662](https://github.com/ros2/rclcpp/issues/2662))

- 修补节点可选指派操作员( E)[\#2656](https://github.com/ros2/rclcpp/issues/2656))

- 明确为统计出版社设置QoS History Keep_ALL。 ([\#2650](https://github.com/ros2/rclcpp/issues/2650))

- 以 rmw_zenoh_cpp 修正测试 \_ intra\_ process_manager.cpp ([\#2653](https://github.com/ros2/rclcpp/issues/2653))

- zenoh 中的固定测试_事件\_ 执行器( E)[\#2643](https://github.com/ros2/rclcpp/issues/2643))

- rmw_fastrtps支持服务事件 gid 独有性测试. ().[\#2638](https://github.com/ros2/rclcpp/issues/2638))

- 如果事件召回不支持, 而不是通过例外, 则打印警告 。 ()[\#2648](https://github.com/ros2/rclcpp/issues/2648))

- 执行对 async\_ send\_ request 服务通用客户端的回调支持( S)[\#2614](https://github.com/ros2/rclcpp/issues/2614))

- 固定试验qos rmw zenoh([\#2639](https://github.com/ros2/rclcpp/issues/2639))

- 验证单一服务事件的客户端 gid 独有性 。 ()[\#2636](https://github.com/ros2/rclcpp/issues/2636))

- 在测试\_ qos\_ event 中跳过一些测试, 用 rmw\_ zenoh 支持的事件类型运行其它测试 ()[\#2626](https://github.com/ros2/rclcpp/issues/2626))

- 在试验中援引上下文的破坏器之前,先关闭上下文([\#2633](https://github.com/ros2/rclcpp/issues/2633))

- 跳过 rmw zenoh 内容过滤测试([\#2627](https://github.com/ros2/rclcpp/issues/2627))

- 使用无效的ServiciceTypeError来获取通用客户端中不可用的服务类型( P)[\#2629](https://github.com/ros2/rclcpp/issues/2629))

- 实施通用服务( U)[\#2617](https://github.com/ros2/rclcpp/issues/2617))

- 修补事件执行器热点错误并添加单位测试( E)[\#2591](https://github.com/ros2/rclcpp/issues/2591))

- 在测试_执行器中删除不必要的 gtest-skip ()[\#2600](https://github.com/ros2/rclcpp/issues/2600))

- 在服务测试代码中校正节点名称( N)[\#2615](https://github.com/ros2/rclcpp/issues/2615))

- 参数Value 到_string () 函数的次要命名修正( )[\#2609](https://github.com/ros2/rclcpp/issues/2609))

- 删除了 clang 警告( R)[\#2605](https://github.com/ros2/rclcpp/issues/2605))

- 解决文件中的几个问题。 ([\#2608](https://github.com/ros2/rclcpp/issues/2608))

- 解析静态单线程执行器( Q)[\#2598](https://github.com/ros2/rclcpp/issues/2598))

- 在 Doc 中定义参数EventHandler 类的固定名称([\#2604](https://github.com/ros2/rclcpp/issues/2604))

- 订阅者\_ 统计\_ 收集者\_ 应当使用 mutex 保护 。 ()[\#2592](https://github.com/ros2/rclcpp/issues/2592))

- 修正事件执行器在计时器寿命周期中的错误( E)[\#2586](https://github.com/ros2/rclcpp/issues/2586))

- 固定 rclcpp/ test/rclcpp/CMakeLists.txt 以检查正确的目标存在性([\#2596](https://github.com/ros2/rclcpp/issues/2596))

- 如果登录配置失败, 在登录时关闭上下文( I)[\#2594](https://github.com/ros2/rclcpp/issues/2594))

- 使更多等待 API 抽象([\#2593](https://github.com/ros2/rclcpp/issues/2593))

- 只编译一次测试。 ([\#2590](https://github.com/ros2/rclcpp/issues/2590))

- 更新的 rcpputils 路径 API ()[\#2579](https://github.com/ros2/rclcpp/issues/2579))

- 让订阅者_触发_to_接收_消息测试更加可靠. ().[\#2584](https://github.com/ros2/rclcpp/issues/2584)) \* 使订阅者\_ 触发\_ 接收\_ 消息测试更加可靠。 在当前代码中, 我们在定时器内创建订阅和出版商, 立即发布, 并期望订阅立即获得。 但可能当发布呼吁发生时, 出版商和订阅者之间甚至还没有发现这种情况。 要让这个更可靠, 创建订阅和出版 *在此之前* 这至少可以让100毫秒的发现发生。 这可能不足以让这在所有平台上变得可靠,但是在我的本地测试中,这非常有帮助。 在这样的变化之前,我可以让这10次失败一次,在变化之后,我运行了100次,没有失败。

- 是否让事件执行器使用更常用的代码( E)[\#2570](https://github.com/ros2/rclcpp/issues/2570)\* 移动可等待设置到它自己的功能 \* 移动 mutex 锁以获取\_ 实体工具 \* 在事件执行器中使用实体\_ 需要\_ 重建\_ 原子布尔 \* 删除重复的 set\_ on\_ ready\_ call back for report\_ waitable \* 使用 mutex 而不是使用新的 递归 mutex \* 使用 current_collection\_ 成员在事件执行器中 \* 延迟添加可等待获取的通知 \* 推迟清除当前收藏 \* 通用通知可等待和收集 \* 通用 添加/ remove node/cbg 方法 \* 固定linter 错误 \*

- 已删除的折旧方法和类别([\#2575](https://github.com/ros2/rclcpp/issues/2575))

- 旋转取消后释放实体的所有权( E)[\#2556](https://github.com/ros2/rclcpp/issues/2556)\*旋转取消后释放实体所有权 \* 移动释放动作到不同旋转函数的每个退出点 \* 移动等待\_ 结果\_.reset() 设置旋转到假 \* 更新测试代码 \* 移动测试代码到测试\_ executors.cpp {}

- 更进一步分割测试_executors.cpp. ().[\#2572](https://github.com/ros2/rclcpp/issues/2572)) 这是因为Windows调试器编译太大,所以分成较小的位数。即使这样,文件也太大了;这很可能是因为我们在这里使用TYPED_TEST,它每个测试例产生多个符号。为了处理这个问题,在不进一步拆分文件的情况下,在编译Windows调试器时也会在/bigobj旗中添加这个符号。

- 避免在事件执行器收藏中添加两次可等待的通知( S)[\#2564](https://github.com/ros2/rclcpp/issues/2564)\* 避免在事件执行实体收藏中添加两次可等待的通知 \* 删除冗余的 mutex 锁 \* \*

- 删除测试中包含的不必要的 msg([\#2566](https://github.com/ros2/rclcpp/issues/2566))

- 修复函数文件中的复制- 粘贴错误( E)[\#2565](https://github.com/ros2/rclcpp/issues/2565))

- 在函数 Doc 中修补类型( N)[\#2563](https://github.com/ros2/rclcpp/issues/2563))

- 添加创建两个内容过滤话题的测试, 主题名称相同( Name[\#2546](https://github.com/ros2/rclcpp/issues/2546)) ([\#2549](https://github.com/ros2/rclcpp/issues/2549))

- 添加执行器选项的 priml 指针( E)[\#2523](https://github.com/ros2/rclcpp/issues/2523))

- 修复执行器: spin_all() 回归修正( S)[\#2517](https://github.com/ros2/rclcpp/issues/2517))

- 在使用Mimick的测试中添加“ mimick” 标签([\#2516](https://github.com/ros2/rclcpp/issues/2516))

- 贡献者:阿卜希谢克·卡希亚普、阿尔韦托·索拉尼亚、亚历杭德罗·埃尔南德斯·科尔德罗、亚历克西斯·波乔莫夫斯基、巴里·徐、克里斯·拉朗谢特、克里斯托弗·贝达德、希辛-伊、雅诺施·麦克豪温斯基、杰弗里·许、康、莱安德尔·斯蒂芬·杜萨、帕特里克·龙卡廖洛、佩德罗·德阿泽雷多、罗曼·DESILE、斯科特·K·洛根、史蒂夫·马肯斯基、塔尼什克·乔达里、托莫亚·藤田、威廉·伍德尔、尤昂元、杰马肖温斯基

<span id="rclcpp-action"></span>

## [rclcpp_action](https://github.com/ros2/rclcpp/tree/kilted/rclcpp_action/CHANGELOG.rst)

- 使用 std: recursive_mutex 来请求动作 。 ()[\#2798](https://github.com/ros2/rclcpp/issues/2798))

- 删除警告( E)[\#2790](https://github.com/ros2/rclcpp/issues/2790))

- Harden rclcpp\_ action:: convert (). (中文(简体) ).[\#2786](https://github.com/ros2/rclcpp/issues/2786))

- 添加新的接口, 以允许对动作进行回顾( N)[\#2743](https://github.com/ros2/rclcpp/issues/2743))

- 对可移植性使用可能未使用的属性。 ()[\#2758](https://github.com/ros2/rclcpp/issues/2758))

- 固定: rclpp 使用的曝光计时器: 等待( P)[\#2699](https://github.com/ros2/rclcpp/issues/2699))

- 从 rCl 收集日志消息, 并重置 。 ()[\#2720](https://github.com/ros2/rclcpp/issues/2720))

- 制作建材工具依赖性( M)[\#2689](https://github.com/ros2/rclcpp/issues/2689))

- 修正服务器中的文档类型_goal_handle.hpp([\#2669](https://github.com/ros2/rclcpp/issues/2669))

- 在 rclcpp\_ action 上增加 cppcheck 的超时度 。 ()[\#2640](https://github.com/ros2/rclcpp/issues/2640))

- 将智能指针宏定义添加到动作服务器和客户端基础类中( S)[\#2631](https://github.com/ros2/rclcpp/issues/2631))

- 在函数 Doc 中修补类型( N)[\#2563](https://github.com/ros2/rclcpp/issues/2563))

- 在使用Mimick的测试中添加“ mimick” 标签([\#2516](https://github.com/ros2/rclcpp/issues/2516))

- 撰稿人:阿尔韦托·索拉尼亚、亚历杭德罗·埃尔南德斯·科尔德罗、巴瑞·许、克里斯·拉兰谢特、克里斯托弗·贝达德、亚诺施·麦克豪林斯基、内森·维贝·诺伊费尔特、斯科特·K·洛根、藤田丰也、YR

<span id="rclcpp-components"></span>

## [rclcpp_components](https://github.com/ros2/rclcpp/tree/kilted/rclcpp_components/CHANGELOG.rst)

- 从密码库中删除了后面的白空间 。 ()[\#2791](https://github.com/ros2/rclcpp/issues/2791))

- 将 NO\_ UNDEFINED\_ SYMBOLS 添加到 rclpp\_ 组件\_ register_node CMake 宏([\#2746](https://github.com/ros2/rclcpp/issues/2746)) ([\#2764](https://github.com/ros2/rclcpp/issues/2764))

- 对可移植性使用可能未使用的属性。 ()[\#2758](https://github.com/ros2/rclcpp/issues/2758))

- 组件管理器应该忽略未知的额外参数... (bas.)[\#2723](https://github.com/ros2/rclcpp/issues/2723))

- 添加其它明显布尔附加参数的解析, 并丢弃不支持的参数( Q)[\#2685](https://github.com/ros2/rclcpp/issues/2685))

- 在试验中援引上下文的破坏器之前,先关闭上下文([\#2633](https://github.com/ros2/rclcpp/issues/2633))

- 在 rclpp_组件基准_组件中修正类型( )[\#2602](https://github.com/ros2/rclcpp/issues/2602))

- 更新的 rcpputils 路径 API ()[\#2579](https://github.com/ros2/rclcpp/issues/2579))

- 从组件_manager.hpp中删除已贬值的API([\#2585](https://github.com/ros2/rclcpp/issues/2585))

- 贡献者:阿尔韦托·索拉格纳、亚历杭德罗·埃尔南德斯·科尔德罗、克里斯托弗·贝达德、约纳斯·奥托、莱安德·斯蒂芬·德苏扎、藤田丰也、rcp1

<span id="rclcpp-lifecycle"></span>

## [rclcpp_lifecycle](https://github.com/ros2/rclcpp/tree/kilted/rclcpp_lifecycle/CHANGELOG.rst)

- 在试图改变状态之前, 应当先拉动有效的过渡 。 ()[\#2774](https://github.com/ros2/rclcpp/issues/2774))

- 对可移植性使用可能未使用的属性。 ()[\#2758](https://github.com/ros2/rclcpp/issues/2758))

- 从 rCl 收集日志消息, 并重置 。 ()[\#2720](https://github.com/ros2/rclcpp/issues/2720))

- 更新文件串 `rclcpp::Node::now()` ([\#2696](https://github.com/ros2/rclcpp/issues/2696))

- 在 rclpp_lishcycle 中修复错误消息: 状态: 重置( ) ()[\#2647](https://github.com/ros2/rclcpp/issues/2647))

- 在试验中援引上下文的破坏器之前,先关闭上下文([\#2633](https://github.com/ros2/rclcpp/issues/2633))

- 寿命周期节点错误修补并添加测试大小写( N)[\#2562](https://github.com/ros2/rclcpp/issues/2562))

- 在rclpp_life循环中正确测试 get_service_names\_ and_types_by_node ([\#2599](https://github.com/ros2/rclcpp/issues/2599))

- 已删除的折旧方法和类别([\#2575](https://github.com/ros2/rclcpp/issues/2575))

- 确定RHEL-9的生命周期测试。 ([\#2583](https://github.com/ros2/rclcpp/issues/2583)\* 修复 RHEL-9 上的生命周期测试。 完整的解释在评论中, 但基本上由于 RHEL 不支持嘲弄_utils:: inject_on\_ return, 我们必须将某些测试分开,以确保一个过程中的资源不会碰撞。 共同作者: Alejandro Hernández Cordero \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 在寿命周期断裂器中恢复呼叫关闭( N)[\#2557](https://github.com/ros2/rclcpp/issues/2557))

- 仅在有效上下文下关闭 dtor 的寿命周期节点 。 ()[\#2545](https://github.com/ros2/rclcpp/issues/2545))

- 在生命周期节点dtor中调用关闭,以避免设备处于未知状态(2nd)([\#2528](https://github.com/ros2/rclcpp/issues/2528))

- rclcpp::shutdown 不应在生命周期节点dtor之前被调用. ()[\#2527](https://github.com/ros2/rclcpp/issues/2527))

- 还原“在寿命周期节点调用关闭,以避免将设备留在un... ()[\#2450](https://github.com/ros2/rclcpp/issues/2450))” ([\#2522](https://github.com/ros2/rclcpp/issues/2522))

- 在使用Mimick的测试中添加“ mimick” 标签([\#2516](https://github.com/ros2/rclcpp/issues/2516))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、克里斯托弗·贝达德、帕特里克·龙卡廖洛、斯科特·K·洛根、藤田托莫亚

<span id="rclpy"></span>

## [rclpy](https://github.com/ros2/rclpy/tree/kilted/rclpy/CHANGELOG.rst)

- 更新参数类型( E)[\#1441](https://github.com/ros2/rclpy/issues/1441))

- 添加 TypeError 字符串参数以更好的清晰度([\#1442](https://github.com/ros2/rclpy/issues/1442))

- 从 Yaml 文件修正加载参数行为 。 ()[\#1193](https://github.com/ros2/rclpy/issues/1193))

- 更新 `lifecycle` 类型([\#1440](https://github.com/ros2/rclpy/issues/1440))

- 更新 \_rclpy_pybind11.pyi 顺序并添加事件执行器([\#1436](https://github.com/ros2/rclpy/issues/1436))

- 更新时钟类型( U)[\#1433](https://github.com/ros2/rclpy/issues/1433))

- 引入事件执行器( E)[\#1391](https://github.com/ros2/rclpy/issues/1391))

- 持续时间、时钟和 QoS 文档([\#1428](https://github.com/ros2/rclpy/issues/1428))

- 添加配置\_ introduction 的例外 doc 。 ()[\#1434](https://github.com/ros2/rclpy/issues/1434))

- 修复任务构建器类型错误( F)[\#1431](https://github.com/ros2/rclpy/issues/1431))

- 添加新接口以启用 intrapsection 用于动作([\#1413](https://github.com/ros2/rclpy/issues/1413))

- 在注册时检查参数召回签名 。 ()[\#1425](https://github.com/ros2/rclpy/issues/1425))

- 固定函数参数缩进( F)[\#1426](https://github.com/ros2/rclpy/issues/1426))

- 最新服务和行动议定书(续)[\#1409](https://github.com/ros2/rclpy/issues/1409))

- 删除 `SHARED` 从 `pybind11_add_module` ([\#1305](https://github.com/ros2/rclpy/issues/1305))

- 一旦执行前被接受,就公布行动目标状态。 ([\#1228](https://github.com/ros2/rclpy/issues/1228))

- 添加缺失的依赖关系, 以便 rosdoc2 显示节点([\#1408](https://github.com/ros2/rclpy/issues/1408))

- 向节点添加 QoS 配置/ 深度支持 。 ()[\#1376](https://github.com/ros2/rclpy/issues/1376))

- 各种打字修正( E)[\#1402](https://github.com/ros2/rclpy/issues/1402))

- 添加类型到使用 rhel roscli 修复的动作中([\#1361](https://github.com/ros2/rclpy/issues/1361))

- 检查任务( 未来) 是否被取消 。 (F)[\#1377](https://github.com/ros2/rclpy/issues/1377))

- 执行者类型( E)[\#1370](https://github.com/ros2/rclpy/issues/1370))

- 事件\_ handler.py 类型([\#1340](https://github.com/ros2/rclpy/issues/1340))

- 添加操作员超载支持 `Duration` ([\#1387](https://github.com/ros2/rclpy/issues/1387))

- 服务/客户执行类型([\#1384](https://github.com/ros2/rclpy/issues/1384))

- 避免生命周期节点过渡例外([\#1319](https://github.com/ros2/rclpy/issues/1319))

- 客户端: 调用生成超时 Error 例外, 当它超时 。 ([\#1271](https://github.com/ros2/rclpy/issues/1271))

- 在 python3-dev 中添加构建依赖性。 ()[\#1380](https://github.com/ros2/rclpy/issues/1380))

- 调用 rcl\_ shutdown 时修改比赛条件([\#1353](https://github.com/ros2/rclpy/issues/1353))

- 使用@ depressed 来标记类型检查器的 API 。 ([\#1350](https://github.com/ros2/rclpy/issues/1350))

- 输入( I)[\#1358](https://github.com/ros2/rclpy/issues/1358))

- 避免冗余 完成对未来的回调, 同时反复调用 spin\_ 直到\_ future\_ profile ()[\#1374](https://github.com/ros2/rclpy/issues/1374))

- 清洁qos zenoh试验([\#1369](https://github.com/ros2/rclpy/issues/1369))

- 调整警告消息, 请求的目标已经过期 。 ()[\#1363](https://github.com/ros2/rclpy/issues/1363))

- 将类型添加到寿命周期对象( E)[\#1338](https://github.com/ros2/rclpy/issues/1338))

- 删除 python\_ cmake\_ 模块使用( I)[\#1220](https://github.com/ros2/rclpy/issues/1220))

- TestClient.test_service_timestamps 持续失败 ().[\#1364](https://github.com/ros2/rclpy/issues/1364))

- 将“ 添加类型到 Action 服务器和 Action 客户端( O)[\#1349](https://github.com/ros2/rclpy/issues/1349))” ([\#1359](https://github.com/ros2/rclpy/issues/1359))

- 重置“ 执行器类型( E) :[\#1345](https://github.com/ros2/rclpy/issues/1345))” ([\#1360](https://github.com/ros2/rclpy/issues/1360))

- 删除模拟_compat ([\#1357](https://github.com/ros2/rclpy/issues/1357))

- 执行者类型( E)[\#1345](https://github.com/ros2/rclpy/issues/1345))

- 添加类型到 Action 服务器和 Action 客户端([\#1349](https://github.com/ros2/rclpy/issues/1349))

- 移除 OpenSplice DDS 发行的 TODO 。 ([\#1354](https://github.com/ros2/rclpy/issues/1354))

- 添加类型到参数_客户端.py([\#1348](https://github.com/ros2/rclpy/issues/1348))

- 将类型添加到节点( N) ([\#1346](https://github.com/ros2/rclpy/issues/1346))

- 将类型添加到信号.py([\#1344](https://github.com/ros2/rclpy/issues/1344))

- 修复旋转到\_ 未来\_ 完成内部回调( N)[\#1316](https://github.com/ros2/rclpy/issues/1316))

- 添加类型( E)[\#1339](https://github.com/ros2/rclpy/issues/1339))

- 添加类型以等待\_ message.py ,并将 Handles 移动到 type stubs ()[\#1325](https://github.com/ros2/rclpy/issues/1325))

- 添加类型到可等待的.py ([\#1328](https://github.com/ros2/rclpy/issues/1328))

- 将rclpyHandle 替换为类型 tubs ([\#1326](https://github.com/ros2/rclpy/issues/1326))

- 修正时间减值( E)[\#1312](https://github.com/ros2/rclpy/issues/1312))

- 添加类型到 TypeDescription Service 。 ()[\#1329](https://github.com/ros2/rclpy/issues/1329))

- 导入周期 Handle 不持续周期 Type ([\#1332](https://github.com/ros2/rclpy/issues/1332))

- 创建出版商Handle并更新出版商.py([\#1310](https://github.com/ros2/rclpy/issues/1310))

- 订阅类型[\#1281](https://github.com/ros2/rclpy/issues/1281))

- 添加类型到 qos.py ([\#1255](https://github.com/ros2/rclpy/issues/1255))

- 稍有改进([\#1330](https://github.com/ros2/rclpy/issues/1330))

- 上下文后初始化信号处理器( I)[\#1331](https://github.com/ros2/rclpy/issues/1331))

- 在多路径执行器中关闭 ThreadPool 执行器 。 ()[\#1309](https://github.com/ros2/rclpy/issues/1309))

- 通用服务和客户端[\#1275](https://github.com/ros2/rclpy/issues/1275))

- 添加类型到参数服务( N)[\#1262](https://github.com/ros2/rclpy/issues/1262))

- 将类型添加到定时器.py([\#1260](https://github.com/ros2/rclpy/issues/1260))

- 添加类型到 rcutils_logger.py ()[\#1249](https://github.com/ros2/rclpy/issues/1249))

- 添加类型到主题\_ endpoint\_ info.oy ()[\#1253](https://github.com/ros2/rclpy/issues/1253))

- 添加类型到参数.py. ()[\#1246](https://github.com/ros2/rclpy/issues/1246))

- 警卫条件类型。 ([\#1252](https://github.com/ros2/rclpy/issues/1252))

- 添加类型到 callback\_ groups.py ([\#1251](https://github.com/ros2/rclpy/issues/1251))

- 公用事业.py类型。 ([\#1250](https://github.com/ros2/rclpy/issues/1250))

- 将结果_超时从15分钟减少到10秒。 ([\#1171](https://github.com/ros2/rclpy/issues/1171))

- 将 TimerInfo 添加到 Timer 回调中 。 ()[\#1292](https://github.com/ros2/rclpy/issues/1292))

- 添加类型到任务.py([\#1254](https://github.com/ros2/rclpy/issues/1254))

- 在获取寿命周期过渡时修补一个错误的错误 。 ()[\#1321](https://github.com/ros2/rclpy/issues/1321))

- 使用多个 rclpy.init 上下文管理器时修复错误 。 ()[\#1314](https://github.com/ros2/rclpy/issues/1314))

- 执行者按 FIFO 顺序执行任务 。 ()[\#1304](https://github.com/ros2/rclpy/issues/1304))

- 添加顶级尝试\_ shutdown 方法 。 ()[\#1302](https://github.com/ros2/rclpy/issues/1302))

- 让rclpy初始化上下文管理者意识到. ().[\#1298](https://github.com/ros2/rclpy/issues/1298))

- 说明适当销毁和创建速率、计时器和警卫条件的指令[\#1286](https://github.com/ros2/rclpy/issues/1286))

- 使定时器具有上下文意识。 ([\#1296](https://github.com/ros2/rclpy/issues/1296))

- 使服务留置上下文意识。 ()[\#1295](https://github.com/ros2/rclpy/issues/1295))

- 使服务服务器上下文管理者意识到。 ()[\#1294](https://github.com/ros2/rclpy/issues/1294))

- 使节点上下文管理者意识到。 ()[\#1293](https://github.com/ros2/rclpy/issues/1293))

- 使订阅者了解上下文管理。 ([\#1291](https://github.com/ros2/rclpy/issues/1291))

- 使出版商了解上下文管理。 ([\#1289](https://github.com/ros2/rclpy/issues/1289))

- (实体数量)[\#1285](https://github.com/ros2/rclpy/issues/1285))

- 信件使用通用词( E)[\#1239](https://github.com/ros2/rclpy/issues/1239))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、阿尔霍·查克拉瓦蒂、巴里·徐、布拉德·马丁、克里斯·拉兰谢特、克里斯托弗·贝达德、埃利安·NEPPEL、乔纳森、马蒂斯·范德布尔格、迈克尔·卡尔斯特罗姆、纳达夫·埃尔卡贝茨、肯特·詹姆斯、谢恩·洛雷茨、托莫亚·藤田、沃尔夫·沃尔普雷希特、扎希·卡基什

<span id="rcpputils"></span>

## [弧形图案](https://github.com/ros2/rcpputils/tree/kilted/CHANGELOG.rst)

- 切换到 ament_cmake_ros_core 软件包([\#211](https://github.com/ros2/rcpputils/issues/211))

- 添加 Marco 以禁用折旧警告( Q)[\#210](https://github.com/ros2/rcpputils/issues/210))

- 添加的缺失包括( E)[\#207](https://github.com/ros2/rcpputils/issues/207))

- 丢出例外时清除 rcutils 错误 。 ()[\#206](https://github.com/ros2/rcpputils/issues/206))

- 删除 CODEOWINERS 和镜像滚动到主工作流程 。 ([\#204](https://github.com/ros2/rcpputils/issues/204))

- 将内存漏出修复为去除_all () 。 ([\#201](https://github.com/ros2/rcpputils/issues/201))

- 禁止因贬值而导致的叮当错误( X)[\#199](https://github.com/ros2/rcpputils/issues/199))

- 已折旧路径类( X)[\#196](https://github.com/ros2/rcpputils/issues/196))

- 以新创建\_ 临时\_ 指令替换创建\_ temp\_ directory ()[\#198](https://github.com/ros2/rcpputils/issues/198)\* 用新创建_临时_目录替换创建_temp_目录 - 新添加的 `create_temporary_directory(..)` 使用 std:: filesystem:: path 且没有特定平台代码。 - 同时已贬值 `create_temp_directory(..)` 财务报告和财务报告 `temp_directory_path`

- 删除的已贬值头获取_env.hpp([\#195](https://github.com/ros2/rcpputils/issues/195))

- 删除滚动的平均值累积器折叠页眉( E)[\#194](https://github.com/ros2/rcpputils/issues/194))

- 删除已贬值的夹子方法( E)[\#193](https://github.com/ros2/rcpputils/issues/193))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、亚诺施·麦克豪温斯基、迈克尔·卡罗尔、迈克尔·奥尔洛夫、藤田托莫亚

<span id="rcutils"></span>

## [rcutils 维基月球](https://github.com/ros2/rcutils/tree/kilted/CHANGELOG.rst)

- 在 Windows (Windows) 上处理启动_进程参数中的空格([\#494](https://github.com/ros2/rcutils/issues/494))

- 添加用于引用子进程的功能( U)[\#491](https://github.com/ros2/rcutils/issues/491)) ([\#492](https://github.com/ros2/rcutils/issues/492))

- 添加调制字符串的 rcutils_join 函数([\#490](https://github.com/ros2/rcutils/issues/490))

- 切换到 ament_cmake_ros_core 软件包([\#489](https://github.com/ros2/rcutils/issues/489))

- 在 rcutils 中清理错误处理 。 ()[\#485](https://github.com/ros2/rcutils/issues/485))

- 删除 CODEOWINERS 和镜像滚动到主工作流程 。 ([\#483](https://github.com/ros2/rcutils/issues/483))

- 将设置分配器固定为 NULL 。 ()[\#478](https://github.com/ros2/rcutils/issues/478))

- 在指定覆盖时添加新的 API 设置 envar ()[\#473](https://github.com/ros2/rcutils/issues/473))

- 完全取消对 CLASNAME 的不必要的使用 。 ()[\#471](https://github.com/ros2/rcutils/issues/471))

- 加载 dll 由 MINGW 使用 lib 前缀构建 ([\#470](https://github.com/ros2/rcutils/issues/470))

- 添加明维支持( E)[\#468](https://github.com/ros2/rcutils/issues/468))

- 在 Windows 上修复文件系统迭代([\#469](https://github.com/ros2/rcutils/issues/469))

- 在使用Mimick的测试中添加“ mimick” 标签([\#466](https://github.com/ros2/rcutils/issues/466))

- 贡献者:亚历杭德罗·埃尔南德斯·科德罗、克里斯·拉兰谢特、费利克斯·弗·许、迈克尔·卡罗尔、斯科特·克·洛根、亚杜

<span id="resource-retriever"></span>

## [resource_retriever](https://github.com/ros/resource_retriever/tree/kilted/resource_retriever/CHANGELOG.rst)

- 固定 clang 编译错误 (% 1)[\#112](https://github.com/ros/resource_retriever/issues/112))

- 删除窗口警告( R)[\#111](https://github.com/ros/resource_retriever/issues/111))

- 添加一个插件机制到资源回收器( E)[\#103](https://github.com/ros/resource_retriever/issues/103))

- 制服 MinCMakeVersion (英语:[\#108](https://github.com/ros/resource_retriever/issues/108))

- 停止使用 python\_ cmake_模块. ()[\#94](https://github.com/ros/resource_retriever/issues/94))

- 允许空格( E)[\#100](https://github.com/ros/resource_retriever/issues/100))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、迈克尔·卡罗尔、莫斯费特80

<span id="rmw"></span>

## [rmw (英语).](https://github.com/ros2/rmw/tree/kilted/rmw/CHANGELOG.rst)

- 切换到 ament_cmake_ros_core 软件包([\#397](https://github.com/ros2/rmw/issues/397))

- 添加的 rmw\_ event\_ type\_ is\_ 支持([\#395](https://github.com/ros2/rmw/issues/395))

- 添加飞地选项函数。 ([\#393](https://github.com/ros2/rmw/issues/393))

- 医生部分的几条字型修正。 ([\#391](https://github.com/ros2/rmw/issues/391))

- 更新 cmake 版本 ([\#389](https://github.com/ros2/rmw/issues/389))

- 获取_零_初始化_xxx函数返回零初始化结构 。 ()[\#380](https://github.com/ros2/rmw/issues/380)) \* get_0_初始化_xxx函数返回零初始化结构. \* 在rm_event_type_t. \* 添加一个注释和更多对rmw_event_type的测试. \_X.

- 从 rcl 移动 qos\_ profile_rosout_默认值 ()[\#381](https://github.com/ros2/rmw/issues/381))

- 在错误路径上修复丑陋的覆盖警告信息 。 ()[\#387](https://github.com/ros2/rmw/issues/387))这主要与在测试的适当时间拨打rm_reset_error()有关,但我们也改变一个分配器的测试,以正确检查一个有效的分配器.

- 修正 rmw_validate_namespace { with\_ six} 处理错误 。 ([\#386](https://github.com/ros2/rmw/issues/386)\* Fix rmw_validate_namespace {with_size} 处理错误。它应该总是设置错误,即使在无效的参数上也是如此。

- 在 rmw_take_response () doc 中固定参数名([\#384](https://github.com/ros2/rmw/issues/384))

- 以静态值初始化 NULL 支架 。 ()[\#378](https://github.com/ros2/rmw/issues/378))

- 删除 rmw_localhost_仅限_t. ([\#376](https://github.com/ros2/rmw/issues/376))

- 使用 RMW_DURINT_UNSPIED 校对:Soup[\#375](https://github.com/ros2/rmw/issues/375))

- 以 rmw_validate\\ 与\_ size () doc 修正类型([\#374](https://github.com/ros2/rmw/issues/374))

- 删除已贬值的rmw_node_assert_livality() ()[\#373](https://github.com/ros2/rmw/issues/373))

- 添加明维支持( E)[\#370](https://github.com/ros2/rmw/issues/370))

- 小类型修补( E)[\#368](https://github.com/ros2/rmw/issues/368))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、克里斯托弗·贝达德、费利克斯·弗·许、G.A. v. Hoorn、迈克尔·卡罗尔、藤田友雅

<span id="rmw-connextdds"></span>

## [rmw_connextdds](https://github.com/ros2/rmw_connextdds/tree/kilted/rmw_connextdds/CHANGELOG.rst)

- 切换构建工具到 ament_cmake 软件包([\#183](https://github.com/ros2/rmw_connextdds/issues/183))

- 导出现代 CMake 目标( Q)[\#179](https://github.com/ros2/rmw_connextdds/issues/179))

- 添加的 rmw\_ event\_ type\_ is\_ 支持([\#173](https://github.com/ros2/rmw_connextdds/issues/173))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、斯科特·K·洛根、谢恩·洛雷茨

<span id="rmw-connextdds-common"></span>

## [rmw_connextdds_common](https://github.com/ros2/rmw_connextdds/tree/kilted/rmw_connextdds_common/CHANGELOG.rst)

- 地址 cpplit 和 gcc 警告 。 ([\#184](https://github.com/ros2/rmw_connextdds/issues/184))

- 支持专题实例( E)[\#178](https://github.com/ros2/rmw_connextdds/issues/178))

- 切换构建工具到 ament_cmake 软件包([\#183](https://github.com/ros2/rmw_connextdds/issues/183))

- 发现的种族条件缓解措施[\#174](https://github.com/ros2/rmw_connextdds/issues/174))

- 添加的 rmw\_ event\_ type\_ is\_ 支持([\#173](https://github.com/ros2/rmw_connextdds/issues/173))

- 使用 rmw_enclave_options_xxxx APIs 代替 。 ([\#172](https://github.com/ros2/rmw_connextdds/issues/172))

- 固定安全证书错误消息格式 。 ()[\#171](https://github.com/ros2/rmw_connextdds/issues/171))

- 使用 rmw_security_common (英语)[\#167](https://github.com/ros2/rmw_connextdds/issues/167))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#169](https://github.com/ros2/rmw_connextdds/issues/169))

- 在 rmw_event_type_t 中引入 ROW_EVENT_TYPE_MAX ()[\#162](https://github.com/ros2/rmw_connextdds/issues/162))

- 仪器客户端/端对端请求/答复跟踪服务([\#163](https://github.com/ros2/rmw_connextdds/issues/163))

- 修饰 : “ 未解析散列类型” 信件过于垃圾邮件( ros2- 50) ([\#149](https://github.com/ros2/rmw_connextdds/issues/149))

- 删除 rmw_localhost_仅限_t. ([\#156](https://github.com/ros2/rmw_connextdds/issues/156))

- Make rmw_service_server_is_可用返回 ROW_RET_INVALID_ArgUMENT ()[\#150](https://github.com/ros2/rmw_connextdds/issues/150))

- 在 rmw_create_node 中使用 rmw_namespace_validation_results_string () 创建_node ()[\#151](https://github.com/ros2/rmw_connextdds/issues/151))

- 让 rmw_destroy_wait_set return ROW_RET_INVALID_ArgUMENT( 重置返回 )[\#152](https://github.com/ros2/rmw_connextdds/issues/152))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯托弗·贝达德、弗朗西斯科·加列戈·萨利多、斯科特·K·洛根、谢恩·洛雷茨、塔克索·鲁比奥·RTI、藤田友也

<span id="rmw-connextddsmicro"></span>

## [rmw_connextddsmicro](https://github.com/ros2/rmw_connextdds/tree/kilted/rmw_connextddsmicro/CHANGELOG.rst)

- 将软件包 rmw_connextdsmicro 标记为贬值([\#182](https://github.com/ros2/rmw_connextdds/issues/182))

- 切换构建工具到 ament_cmake 软件包([\#183](https://github.com/ros2/rmw_connextdds/issues/183))

- 添加的 rmw\_ event\_ type\_ is\_ 支持([\#173](https://github.com/ros2/rmw_connextdds/issues/173))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、弗朗西斯科·加莱戈·萨利多、斯科特·K·洛根

<span id="rmw-cyclonedds-cpp"></span>

## [rmw_cyclonedds_cpp](https://github.com/ros2/rmw_cyclonedds/tree/kilted/rmw_cyclonedds_cpp/CHANGELOG.rst)

- 切换到 ament_cmake_ros_core 软件包([\#538](https://github.com/ros2/rmw_cyclonedds/issues/538))

- 添加的 rmw\_ event\_ type\_ is\_ 支持([\#532](https://github.com/ros2/rmw_cyclonedds/issues/532))

- 使用 rmw_enclave_options_xxxx APIs 代替 。 ([\#531](https://github.com/ros2/rmw_cyclonedds/issues/531))

- 使用 rmw_security\_ 常见 ([\#529](https://github.com/ros2/rmw_cyclonedds/issues/529))

- 在 rmw_event_type_t 中引入 ROW_EVENT_TYPE_MAX ()[\#518](https://github.com/ros2/rmw_cyclonedds/issues/518))

- 在设定新错误前重置错误 。 ()[\#526](https://github.com/ros2/rmw_cyclonedds/issues/526))

- 仪器客户端/端对端请求/答复跟踪服务([\#521](https://github.com/ros2/rmw_cyclonedds/issues/521))

- 丢弃浮点128的支持( D). ([\#522](https://github.com/ros2/rmw_cyclonedds/issues/522))

- 使用 RMW_GID_STORAGE_SIZE 客户端_服务_id_t. ([\#515](https://github.com/ros2/rmw_cyclonedds/issues/515))

- 删除 rmw_localhost_仅限_t. ([\#508](https://github.com/ros2/rmw_cyclonedds/issues/508))

- 解决触发警卫条件的问题。 ([\#504](https://github.com/ros2/rmw_cyclonedds/issues/504)) 当一个警戒条件生效时, 我们必须记住要增加trig_idx, 这样我们才能看下一个触发器。 否则, 我们可以进入一个被触发的成员跳过的情况 。

- Make rmw_service_server_is_可用返回 ROW_RET_INVALID_ArgUMENT ()[\#496](https://github.com/ros2/rmw_cyclonedds/issues/496))

- 在 rmw_create_node 中使用 rmw_namespace_validation_results_string () 创建_node ()[\#497](https://github.com/ros2/rmw_cyclonedds/issues/497))

- 让 rmw_destroy_wait_set return ROW_RET_INVALID_ArgUMENT( 重置返回 )[\#498](https://github.com/ros2/rmw_cyclonedds/issues/498))

- 将接收到的\_ Timestamp to system_chour: : now() in message_info ()[\#491](https://github.com/ros2/rmw_cyclonedds/issues/491))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、克里斯托弗·贝达德、埃里克·博阿松、乔·斯皮德、何塞·托马斯·洛伦特、迈克尔·奥尔洛夫、斯科特·K·洛根、托莫亚·藤田

<span id="rmw-dds-common"></span>

## [rmw_dds_common](https://github.com/ros2/rmw_dds_common/tree/kilted/rmw_dds_common/CHANGELOG.rst)

- 折旧的担保方法[\#77](https://github.com/ros2/rmw_dds_common/issues/77))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗

<span id="rmw-fastrtps-cpp"></span>

## [rmw_fastrtps_cpp](https://github.com/ros2/rmw_fastrtps/tree/kilted/rmw_fastrtps_cpp/CHANGELOG.rst)

- 地址 RHEL 警告和缺失包括. ([\#819](https://github.com/ros2/rmw_fastrtps/issues/819))

- 支持专题实例( E)[\#753](https://github.com/ros2/rmw_fastrtps/issues/753))

- 切换到 ament_cmake_ros_core 软件包([\#818](https://github.com/ros2/rmw_fastrtps/issues/818))

- 添加的 rmw\_ event\_ type\_ is\_ 支持([\#809](https://github.com/ros2/rmw_fastrtps/issues/809))

- 使用 rmw_enclave_options_xxxx APIs 代替 。 ([\#808](https://github.com/ros2/rmw_fastrtps/issues/808))

- 添加FASTRTPS_DEFAULT_PROFILES_FILE 的折旧警告([\#806](https://github.com/ros2/rmw_fastrtps/issues/806))

- 导出现代 CMake 目标( Q)[\#805](https://github.com/ros2/rmw_fastrtps/issues/805))

- 对照 Fast DDS 3.0 构建的更改 ([\#776](https://github.com/ros2/rmw_fastrtps/issues/776))

- 修正 rmw\_ fastrps 中的一些覆盖错误 ()[\#799](https://github.com/ros2/rmw_fastrtps/issues/799))

- 仪器客户端/端对端请求/答复跟踪服务([\#787](https://github.com/ros2/rmw_fastrtps/issues/787))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、卡洛斯·埃斯皮诺萨·库尔托、克里斯·拉兰谢特、克里斯托弗·贝达德、米格尔公司、斯科特·K·洛根、谢恩·洛雷茨、藤田友雅

<span id="rmw-fastrtps-dynamic-cpp"></span>

## [rmw_fastrtps_dynamic_cpp](https://github.com/ros2/rmw_fastrtps/tree/kilted/rmw_fastrtps_dynamic_cpp/CHANGELOG.rst)

- 地址 RHEL 警告和缺失包括. ([\#819](https://github.com/ros2/rmw_fastrtps/issues/819))

- 支持专题实例( E)[\#753](https://github.com/ros2/rmw_fastrtps/issues/753))

- 切换到 ament_cmake_ros_core 软件包([\#818](https://github.com/ros2/rmw_fastrtps/issues/818))

- 让 rmw_fastrtps_动态_cpp 导出现代 CMake 目标([\#814](https://github.com/ros2/rmw_fastrtps/issues/814))

- 添加的 rmw\_ event\_ type\_ is\_ 支持([\#809](https://github.com/ros2/rmw_fastrtps/issues/809))

- 使用 rmw_enclave_options_xxxx APIs 代替 。 ([\#808](https://github.com/ros2/rmw_fastrtps/issues/808))

- 添加FASTRTPS_DEFAULT_PROFILES_FILE 的折旧警告([\#806](https://github.com/ros2/rmw_fastrtps/issues/806))

- 对照 Fast DDS 3.0 构建的更改 ([\#776](https://github.com/ros2/rmw_fastrtps/issues/776))

- 修正 rmw\_ fastrps 中的一些覆盖错误 ()[\#799](https://github.com/ros2/rmw_fastrtps/issues/799))

- 仪器客户端/端对端请求/答复跟踪服务([\#787](https://github.com/ros2/rmw_fastrtps/issues/787))

- 在rmw_fastrtps_动态_cpp中添加追踪仪器([\#772](https://github.com/ros2/rmw_fastrtps/issues/772))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、卡洛斯·埃斯皮诺萨·库尔托、克里斯·拉兰谢特、克里斯托弗·贝达德、米格尔公司、斯科特·K·洛根、谢恩·洛雷茨、藤田友雅

<span id="rmw-fastrtps-shared-cpp"></span>

## [rmw_fastrtps_shared_cpp](https://github.com/ros2/rmw_fastrtps/tree/kilted/rmw_fastrtps_shared_cpp/CHANGELOG.rst)

- 地址 RHEL 警告和缺失包括. ([\#819](https://github.com/ros2/rmw_fastrtps/issues/819))

- 支持专题实例( E)[\#753](https://github.com/ros2/rmw_fastrtps/issues/753))

- 切换到 ament_cmake_ros_core 软件包([\#818](https://github.com/ros2/rmw_fastrtps/issues/818))

- 添加的 rmw\_ event\_ type\_ is\_ 支持([\#809](https://github.com/ros2/rmw_fastrtps/issues/809))

- 使用 rmw_enclave_options_xxxx APIs 代替 。 ([\#808](https://github.com/ros2/rmw_fastrtps/issues/808))

- 添加FASTRTPS_DEFAULT_PROFILES_FILE 的折旧警告([\#806](https://github.com/ros2/rmw_fastrtps/issues/806))

- 使用 rmw_security_common (英语)[\#803](https://github.com/ros2/rmw_fastrtps/issues/803))

- 在 rmw_event_type_t 中引入 ROW_EVENT_TYPE_MAX ()[\#785](https://github.com/ros2/rmw_fastrtps/issues/785))

- 对照 Fast DDS 3.0 构建的更改 ([\#776](https://github.com/ros2/rmw_fastrtps/issues/776))

- 在rmw_fastrtps_shared_cpp中清理一个测试([\#794](https://github.com/ros2/rmw_fastrtps/issues/794))

- 仪器客户端/端对端请求/答复跟踪服务([\#787](https://github.com/ros2/rmw_fastrtps/issues/787))

- 丢弃浮点128的支持( D). ([\#788](https://github.com/ros2/rmw_fastrtps/issues/788))

- 继续参考 `DomainParticipantFactory` ([\#770](https://github.com/ros2/rmw_fastrtps/issues/770))

- 使用客户端的读取器 Guid 来进行服务回顾事件 gid([\#781](https://github.com/ros2/rmw_fastrtps/issues/781))

- 还原“服务内视事件的唯一客户端 GID 。 ()[\#779](https://github.com/ros2/rmw_fastrtps/issues/779))” ([\#780](https://github.com/ros2/rmw_fastrtps/issues/780))

- 服务内观事件的唯一客户端 GID 。 ([\#779](https://github.com/ros2/rmw_fastrtps/issues/779))

- 删除 rmw_localhost_仅限_t. ([\#773](https://github.com/ros2/rmw_fastrtps/issues/773))

- Make rmw_service_server_is_可用返回 ROW_RET_INVALID_ArgUMENT ()[\#763](https://github.com/ros2/rmw_fastrtps/issues/763))

- 在 rmw_create_node 中使用 rmw_namespace_validation_results_string () 创建_node ()[\#765](https://github.com/ros2/rmw_fastrtps/issues/765))

- 让 rmw_destroy_wait_set return ROW_RET_INVALID_ArgUMENT( 重置返回 )[\#766](https://github.com/ros2/rmw_fastrtps/issues/766))

- 创建内容过滤话题时使用独有的拼接名称( U)[\#762](https://github.com/ros2/rmw_fastrtps/issues/762))

- 添加对数据表示的支持( E)[\#756](https://github.com/ros2/rmw_fastrtps/issues/756))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、卡洛斯·埃斯皮诺萨·库尔托、克里斯·拉朗谢特、克里斯托弗·贝达德、豪尔赫·佩雷斯、马里奥·多明格斯·洛佩斯、米格尔公司、斯科特·K·洛根、藤田托莫亚

<span id="rmw-implementation"></span>

## [rmw_implementation](https://github.com/ros2/rmw_implementation/tree/kilted/rmw_implementation/CHANGELOG.rst)

- 添加的 rmw\_ event\_ type\_ is\_ 支持([\#250](https://github.com/ros2/rmw_implementation/issues/250))

- 请在 Rmw\_ 执行中找到\_ package(rmw) 。 ([\#242](https://github.com/ros2/rmw_implementation/issues/242))

- 添加机制,使依赖群体无法工作([\#229](https://github.com/ros2/rmw_implementation/issues/229))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、斯科特·K·洛根

<span id="rmw-implementation-cmake"></span>

## [rmw_implementation_cmake](https://github.com/ros2/rmw/tree/kilted/rmw_implementation_cmake/CHANGELOG.rst)

- 更新 cmake 版本 ([\#389](https://github.com/ros2/rmw/issues/389))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗

<span id="rmw-security-common"></span>

## [rmw_security_common](https://github.com/ros2/rmw/tree/kilted/rmw_security_common/CHANGELOG.rst)

- 导出 rmw 依赖性 (% 1)[\#400](https://github.com/ros2/rmw/issues/400))

- 已添加 rmw_security_common ([\#388](https://github.com/ros2/rmw/issues/388))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、亚敦农德

<span id="rmw-test-fixture"></span>

## [rmw_test_fixture](https://github.com/ros2/ament_cmake_ros/tree/kilted/rmw_test_fixture/CHANGELOG.rst)

- 以 rmw\_ test_fixture 解决窗口警告([\#22](https://github.com/ros2/ament_cmake_ros/issues/22))

- 添加rmw_test_fixture 用于支持 RMW-同位素测试([\#21](https://github.com/ros2/ament_cmake_ros/issues/21))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、斯科特·K·洛根

<span id="rmw-test-fixture-implementation"></span>

## [rmw_test_fixture_implementation](https://github.com/ros2/ament_cmake_ros/tree/kilted/rmw_test_fixture_implementation/CHANGELOG.rst)

- 不要将ROS_AUTOMATIC_DISCOVERY_RANGE设置在rmw_test_fixture中([\#33](https://github.com/ros2/ament_cmake_ros/issues/33))

- 在 Windows 上修正 rmw\_ test_fixture DLL 导入([\#32](https://github.com/ros2/ament_cmake_ros/issues/32))

- 定义 rmw\_ test_fixture_默认端口锁定的范围([\#31](https://github.com/ros2/ament_cmake_ros/issues/31))

- 停止装入 RMW 以装入测试固定器([\#30](https://github.com/ros2/ament_cmake_ros/issues/30))

- 根据域名协调员添加“ 默认” rmw\_ test\_ fixture ()[\#26](https://github.com/ros2/ament_cmake_ros/issues/26))

- 安装 run_rmw_isolated 可执行文件到 lib 子目录( )[\#25](https://github.com/ros2/ament_cmake_ros/issues/25))

- 在 Windows 上的运行中忽略 Ctrl-C ()[\#24](https://github.com/ros2/ament_cmake_ros/issues/24))

- 以 rmw\_ test_fixture 解决窗口警告([\#22](https://github.com/ros2/ament_cmake_ros/issues/22))

- 添加rmw_test_fixture 用于支持 RMW-同位素测试([\#21](https://github.com/ros2/ament_cmake_ros/issues/21))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、斯科特·K·洛根

<span id="rmw-zenoh-cpp"></span>

## [rmw_zenoh_cpp](https://github.com/ros2/rmw_zenoh/tree/kilted/rmw_zenoh_cpp/CHANGELOG.rst)

- 更改附件\_ helpers.cpp中的序列化格式([\#601](https://github.com/ros2/rmw_zenoh/issues/601))

- Bump Zenoh to v1.3.2, 并用 HeartbeatSporadic 提高e2e的可靠性([\#591](https://github.com/ros2/rmw_zenoh/issues/591))

- 执行 rmw\_ test_fixture 启动 Zenoh 路由器([\#583](https://github.com/ros2/rmw_zenoh/issues/583))

- 添加质量声明( E)[\#483](https://github.com/ros2/rmw_zenoh/issues/483))

- 如果注册前有变化, 触发 qos 事件召回([\#587](https://github.com/ros2/rmw_zenoh/issues/587))

- 设置等待 \_ set- \> 触发的旗帜为假( S)[\#575](https://github.com/ros2/rmw_zenoh/issues/575))

- 后添加空格 `id` 符号在 `rmw_zenohd` 日志字符串( R)[\#576](https://github.com/ros2/rmw_zenoh/issues/576))

- 使用 `std::unique_lock` 在 Windows 上正确解锁([\#570](https://github.com/ros2/rmw_zenoh/issues/570))

- 切换到 std:: map for TopicTypeMap ()[\#546](https://github.com/ros2/rmw_zenoh/issues/546))

- 支持 zenoh 配置覆盖( R)[\#551](https://github.com/ros2/rmw_zenoh/issues/551))

- 将配置与最新的Zenoh对齐。 ()[\#556](https://github.com/ros2/rmw_zenoh/issues/556))

- 代码中添加的文档注释( Q)[\#540](https://github.com/ros2/rmw_zenoh/issues/540))

- 修补: 在获取前解锁鼠标( P) :[\#537](https://github.com/ros2/rmw_zenoh/issues/537))

- 订阅在条件 \_ 可变通知前使用 wait\_ set_lock 选项([\#528](https://github.com/ros2/rmw_zenoh/issues/528))

- 将默认耐久性切换为挥发性( S)[\#521](https://github.com/ros2/rmw_zenoh/issues/521))

- 添加的 rmw\_ event\_ type\_ is\_ 支持([\#502](https://github.com/ros2/rmw_zenoh/issues/502))

- 固定窗口警告( E)[\#500](https://github.com/ros2/rmw_zenoh/issues/500))

- 配置: 调制一些用于 ROS 的值, 特别是大量节点( \> 200) ([\#509](https://github.com/ros2/rmw_zenoh/issues/509))

- 订阅选项中的荣誉忽略\_ 本地出版物( O)[\#508](https://github.com/ros2/rmw_zenoh/issues/508))

- bump zenoh-cpp to 2a127bb, zenoh-c to 3540a3c, zenoh to f735bf5 (中文(简体) ).[\#503](https://github.com/ros2/rmw_zenoh/issues/503))

- 更新事件状态时固定当前\_ counter\_ change的计算([\#504](https://github.com/ros2/rmw_zenoh/issues/504))

- 修复无效参数的检查( V)[\#497](https://github.com/ros2/rmw_zenoh/issues/497))

- 如果 qos 包含未知的设置, 则无法创建实体([\#494](https://github.com/ros2/rmw_zenoh/issues/494))

- 使用 rmw_enclave_options_xxxx APIs 代替 。 ([\#491](https://github.com/ros2/rmw_zenoh/issues/491))

- 启用 Zenoh UDP 传输 (S)[\#486](https://github.com/ros2/rmw_zenoh/issues/486))

- 固定 : 使用默认的销毁器, 自动放下 zenoh 回复/ 清空, 从而发送最后的信号 ([\#473](https://github.com/ros2/rmw_zenoh/issues/473))

- 介绍先进出版商和订阅商([\#368](https://github.com/ros2/rmw_zenoh/issues/368))

- 如果话题\_ 名称不在话题\_ 地图中, 则切换为调试日志( T)[\#454](https://github.com/ros2/rmw_zenoh/issues/454))

- Bump Zenoh以3bbf6af(1.2.1+少数犯罪)([\#456](https://github.com/ros2/rmw_zenoh/issues/456))

- Bump Zenoh 犯罪 id e4ea6f0 (1.2.0 + 少数犯罪)[\#446](https://github.com/ros2/rmw_zenoh/issues/446))

- 告知用户,在路由器启动之前,对等方不会发现并相互沟通([\#440](https://github.com/ros2/rmw_zenoh/issues/440))

- 在 rmw_serialized_message_rescription() 后清除错误([\#435](https://github.com/ros2/rmw_zenoh/issues/435))

- 修补 `ZENOH_ROUTER_CHECK_ATTEMPTS` 不被尊重的,[\#427](https://github.com/ros2/rmw_zenoh/issues/427))

- 固定 : 使用默认的销毁器来丢弃成员 `Payload` ([\#419](https://github.com/ros2/rmw_zenoh/issues/419))

- 删除 `gid_hash\_` 从 `AttachmentData` ([\#416](https://github.com/ros2/rmw_zenoh/issues/416))

- 以 Zenoh 中的默认配置同步配置 。 ()[\#396](https://github.com/ros2/rmw_zenoh/issues/396))

- 修补:在访问会话前检查上下文的有效性( P)[\#403](https://github.com/ros2/rmw_zenoh/issues/403))

- 纠正不打字( W)[\#400](https://github.com/ros2/rmw_zenoh/issues/400))

- 基于Zenoh的ROS 2的替代中间软件.

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、亚历克斯·戴伊、伯恩德·普罗默、陈英国(共青团)、克里斯·拉兰谢特、克里斯托弗·贝达德、西哈特·阿尔蒂帕尔马克、埃斯特韦·费尔南德斯、佛朗哥·西波隆、杰弗里·比格斯、汉斯-马丁、胡加尔31、詹姆斯·蒙、朱利安·埃诺赫、卢卡·科米纳迪、马哈茂德·马祖兹、摩根·奎格利、内特·柯尼希、帕特里克·龙卡廖洛、斯科特·克洛根、希万·维杰、蒂姆·克莱法斯、托莫亚·富吉塔、亚都恩德、尤安·袁、梅迪德拉贡、雅敦德、黄哈特尔

<span id="robot-state-publisher"></span>

## [robot_state_publisher](https://github.com/ros/robot_state_publisher/tree/kilted/CHANGELOG.rst)

- 使用 `emplace()` 与 `std::map` ([\#231](https://github.com/ros/robot_state_publisher/issues/231))

- 删除 CODEOWINERS 和镜像滚动到主工作流程([\#229](https://github.com/ros/robot_state_publisher/issues/229))

- 更新 urdf 模型头( S)[\#223](https://github.com/ros/robot_state_publisher/issues/223))

- 撰稿人:亚历杭德罗·埃尔南德斯·科尔德罗、帕特里克·龙卡廖洛

<span id="ros2action"></span>

## [ros2 动作](https://github.com/ros2/ros2cli/tree/kilted/ros2action/CHANGELOG.rst)

- 允许 Zenoh 测试以多播方式运行([\#992](https://github.com/ros2/ros2cli/issues/992))

- 支持 " ros2 动作回声 " ([\#978](https://github.com/ros2/ros2cli/issues/978))

- 校正许可证内容( E)[\#979](https://github.com/ros2/ros2cli/issues/979))

- 保持将时间戳自动放入服务的一致性,行动称为象形文字出版。 ([\#961](https://github.com/ros2/ros2cli/issues/961))

- ros2action: 添加 SIGINT 处理器来管理取消请求 。 ([\#956](https://github.com/ros2/ros2cli/issues/956))

- 节点名称打印错误用 ros2 动作信息修复 。 ()[\#926](https://github.com/ros2/ros2cli/issues/926))

- 切换到使用 rclpy.init 上下文管理器. ()[\#918](https://github.com/ros2/ros2cli/issues/918))

- 支持“ ros2 动作找到” 。 ([\#917](https://github.com/ros2/ros2cli/issues/917))

- 撰稿人:Barry Xu, Chris Lalancette, Michael Carroll, Sukhvansh Jain, Tomoya Fujita

<span id="ros2bag"></span>

## [罗斯2袋](https://github.com/ros2/rosbag2/tree/kilted/ros2bag/CHANGELOG.rst)

- 添加动作重播功能( N)[\#1955](https://github.com/ros2/rosbag2/issues/1955))

- 实施记录和显示记录行动特征的信息的行动([\#1939](https://github.com/ros2/rosbag2/issues/1939))

- 修复 Windows 上的失败测试\_ record_qos\_ profiles ()[\#1949](https://github.com/ros2/rosbag2/issues/1949))

- ROS2 袋装游戏的进展栏([\#1836](https://github.com/ros2/rosbag2/issues/1836))

- 更新 CLI 播放元动词( M) ([\#1906](https://github.com/ros2/rosbag2/issues/1906))

- 将 test_xmllint.py 添加到 python 包中 。 ([\#1879](https://github.com/ros2/rosbag2/issues/1879))

- 增加基于出版时间戳的重播支持( Q)[\#1876](https://github.com/ros2/rosbag2/issues/1876))

- 延迟结束后公布时钟, 并禁用下个循环上的延迟( Name[\#1861](https://github.com/ros2/rosbag2/issues/1861))

- 支持重放多个袋( R)[\#1848](https://github.com/ros2/rosbag2/issues/1848))

- 将 rclpy.qos.QoS\* 政策重命名为 rclpy.qos.\* 政策 ([\#1832](https://github.com/ros2/rosbag2/issues/1832))

- 在“ros2 袋信息”指令中添加“-sort” CLI选项([\#1804](https://github.com/ros2/rosbag2/issues/1804))

- 添加 cli 选项压缩线程优先级( T)[\#1768](https://github.com/ros2/rosbag2/issues/1768))

- 将大小贡献的计算添加到信息动词中( E)[\#1726](https://github.com/ros2/rosbag2/issues/1726))

- 固定( start- off) : 允许指定一个初始偏移 0 ([\#1682](https://github.com/ros2/rosbag2/issues/1682))

- 指定时点选项时不包括已记录的/时点话题([\#1646](https://github.com/ros2/rosbag2/issues/1646))

- 清洗罗巴格2记录器 CLI 参数校验码([\#1633](https://github.com/ros2/rosbag2/issues/1633))

- 添加 -log- 级别到 ros2 袋播放和记录( )[\#1625](https://github.com/ros2/rosbag2/issues/1625))

- 为“ ros2 袋记录” 添加可选的 QQ专题的 CLI 参数([\#1632](https://github.com/ros2/rosbag2/issues/1632))

- 贡献者:亚历杭德罗·埃尔南德斯·科德罗、巴瑞·徐、克里斯·拉朗谢特、克里斯托弗·贝达德、竹内康介、迈克尔·奥尔洛夫、尼古拉·洛伊、帕特里克·龙卡廖洛、赖因·阿佩尔多伦、罗马、萨诺罗纳斯

<span id="ros2cli"></span>

## [罗斯2cli](https://github.com/ros2/ros2cli/tree/kilted/ros2cli/CHANGELOG.rst)

- 允许 Zenoh 测试以多播方式运行([\#992](https://github.com/ros2/ros2cli/issues/992))

- 重命名测试 {Direct}.py 测试 。[\#959](https://github.com/ros2/ros2cli/issues/959))

- 将删除前缀替换为字符串切换。 ()[\#953](https://github.com/ros2/ros2cli/issues/953))

- 修复 ROS2 守护进程中的不稳定性 。 ([\#947](https://github.com/ros2/ros2cli/issues/947))

- 减少对蟒3-pkg资源的依赖([\#946](https://github.com/ros2/ros2cli/issues/946))

- 节点战略支持节点名称参数 。 ()[\#941](https://github.com/ros2/ros2cli/issues/941))

- 切换到使用 rclpy.init 上下文管理器. ()[\#920](https://github.com/ros2/ros2cli/issues/920))

- 撰稿人:克里斯·拉兰谢特、迈克尔·卡罗尔、斯科特·K·洛根、藤田友也

<span id="ros2doctor"></span>

## [ros2 医生](https://github.com/ros2/ros2cli/tree/kilted/ros2doctor/CHANGELOG.rst)

- 允许 Zenoh 测试以多播方式运行([\#992](https://github.com/ros2/ros2cli/issues/992))

- 跳过 Zenoh 上的 QoS 兼容性测试( S)[\#985](https://github.com/ros2/ros2cli/issues/985))

- 新的旗帜和代码更新以供其使用( U)[\#942](https://github.com/ros2/ros2cli/issues/942))

- 切换到使用 rclpy.init 上下文管理器. ()[\#918](https://github.com/ros2/ros2cli/issues/918))

- 校正我们如何获得 ros2 Doctor 的网络信息 。 ([\#910](https://github.com/ros2/ros2cli/issues/910))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、安赫尔·洛加、克里斯·拉朗谢特、迈克尔·卡罗尔

<span id="ros2launch"></span>

## [ros2 发射](https://github.com/ros2/launch_ros/tree/kilted/ros2launch/CHANGELOG.rst)

- 在 ment_python 包中添加 ament_xmllint 。 ([\#423](https://github.com/ros2/launch_ros/issues/423))

- 在设置中修正url.py([\#413](https://github.com/ros2/launch_ros/issues/413))

- 添加机制,使依赖群体无法工作([\#397](https://github.com/ros2/launch_ros/issues/397))

- 贡献者:克里斯·拉兰谢特、斯科特·K·洛根、魏·胡

<span id="ros2lifecycle"></span>

## [旋转二寿命周期](https://github.com/ros2/ros2cli/tree/kilted/ros2lifecycle/CHANGELOG.rst)

- 允许 Zenoh 测试以多播方式运行([\#992](https://github.com/ros2/ros2cli/issues/992))

- 撰稿人:迈克尔·卡罗尔

<span id="ros2lifecycle-test-fixtures"></span>

## [ros2lifecycle_test_fixtures](https://github.com/ros2/ros2cli/tree/kilted/ros2lifecycle_test_fixtures/CHANGELOG.rst)

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#973](https://github.com/ros2/ros2cli/issues/973))

- 撰稿人:谢恩·洛雷茨

<span id="ros2node"></span>

## [ros2 节点](https://github.com/ros2/ros2cli/tree/kilted/ros2node/CHANGELOG.rst)

- 允许 Zenoh 测试以多播方式运行([\#992](https://github.com/ros2/ros2cli/issues/992))

- ros2node需要完全合格的节点名称. ().[\#923](https://github.com/ros2/ros2cli/issues/923))

- 切换到使用 rclpy.init 上下文管理器. ()[\#918](https://github.com/ros2/ros2cli/issues/918))

- 贡献者:克里斯·拉兰谢特、迈克尔·卡罗尔、藤田友也

<span id="ros2param"></span>

## [ros2 参数](https://github.com/ros2/ros2cli/tree/kilted/ros2param/CHANGELOG.rst)

- 从 Yaml 文件修正加载参数行为( E)[\#864](https://github.com/ros2/ros2cli/issues/864))

- 允许 Zenoh 测试以多播方式运行([\#992](https://github.com/ros2/ros2cli/issues/992))

- 罗斯2param垃圾堆装命令的化妆品修复。 ([\#933](https://github.com/ros2/ros2cli/issues/933))

- 切换到使用 rclpy.init 上下文管理器. ()[\#918](https://github.com/ros2/ros2cli/issues/918))

- 贡献者:克里斯·拉兰谢特、迈克尔·卡罗尔、藤田友也

<span id="ros2pkg"></span>

## [ros2pkg (单位:千米)](https://github.com/ros2/ros2cli/tree/kilted/ros2pkg/CHANGELOG.rst)

- 使用现代 C++17 语法. ()[\#982](https://github.com/ros2/ros2cli/issues/982))

- 使用目标_link_librarys 而不是 ament_target_dependenities ()[\#973](https://github.com/ros2/ros2cli/issues/973))

- 尝试使用 git 环球用户. name 来维护者名( U)[\#968](https://github.com/ros2/ros2cli/issues/968))

- 更新最小 CMake 版本 CMakeLists.txt.em ()[\#969](https://github.com/ros2/ros2cli/issues/969))

- 默认情况下在 Ament_python 包中添加 ament_xmllint 测试 。 ([\#957](https://github.com/ros2/ros2cli/issues/957))

- 减少对蟒3-pkg资源的依赖([\#946](https://github.com/ros2/ros2cli/issues/946))

- 支持嵌入4和嵌入3([\#921](https://github.com/ros2/ros2cli/issues/921))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、拉里·格泽利乌斯、斯科特·K·洛根、塞巴斯蒂安·卡斯特罗、谢恩·洛雷茨、谢努尔

<span id="ros2run"></span>

## [罗斯2运行](https://github.com/ros2/ros2cli/tree/kilted/ros2run/CHANGELOG.rst)

- 将信号处理器 SIGIN/SIGTERM 添加到 ros2run ([\#899](https://github.com/ros2/ros2cli/issues/899))

- 贡献者:藤田友也

<span id="ros2service"></span>

## [罗斯2服务](https://github.com/ros2/ros2cli/tree/kilted/ros2service/CHANGELOG.rst)

- 使用 `get_service` 输入 `ros2service call` ([\#994](https://github.com/ros2/ros2cli/issues/994))

- 允许 Zenoh 测试以多播方式运行([\#992](https://github.com/ros2/ros2cli/issues/992))

- 支持 QoS 选项 `ros2 service call` ([\#966](https://github.com/ros2/ros2cli/issues/966))

- 保持将时间戳自动放入服务的一致性,行动称为象形文字出版。 ([\#961](https://github.com/ros2/ros2cli/issues/961))

- 切换到使用 rclpy.init 上下文管理器. ()[\#920](https://github.com/ros2/ros2cli/issues/920))

- 切换到使用 rclpy.init 上下文管理器. ()[\#918](https://github.com/ros2/ros2cli/issues/918))

- 撰稿人:克里斯·拉兰谢特、迈克尔·卡尔斯特罗姆、迈克尔·卡罗尔、苏赫万什·贾因、藤田丰也

<span id="ros2test"></span>

## [ros2 测试](https://github.com/ros2/ros_testing/tree/kilted/ros2test/CHANGELOG.rst)

- 在 test_xmllint 中添加为 ros2 test 。 ([\#13](https://github.com/ros2/ros_testing/issues/13))

- 撰稿人:克里斯·拉兰谢特

<span id="ros2topic"></span>

## [ros2 专题](https://github.com/ros2/ros2cli/tree/kilted/ros2topic/CHANGELOG.rst)

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

- 支持 ROS2 专题回声中的多个字段([\#964](https://github.com/ros2/ros2cli/issues/964))

- 节点战略支持节点名称参数 。 ()[\#941](https://github.com/ros2/ros2cli/issues/941))

- 功能( echo - clear ): 添加 - clear 选项以回声([\#819](https://github.com/ros2/ros2cli/issues/819))

- 通过 ros2 主题 hz 支持多个主题 ([\#929](https://github.com/ros2/ros2cli/issues/929))

- 移除 OpenSplice DDS 发行的 TODO 。 ([\#928](https://github.com/ros2/ros2cli/issues/928))

- 切换到使用 rclpy.init 上下文管理器. ()[\#918](https://github.com/ros2/ros2cli/issues/918))

- 撰稿人:亚历杭德罗·埃尔南德斯·科尔德罗、安东尼·韦尔特、克里斯·拉朗谢特、法比安·汤姆森、弗洛伦西亚、纪尧姆·贝祖塞博克、科斯图布·汗德尔瓦尔、莱安德·斯蒂芬·德苏扎、马丁·佩卡、迈克尔·卡罗尔、桑塔克莱、藤田友雅

<span id="ros2trace"></span>

## [ros2 跟踪](https://github.com/ros2/ros2_tracing/tree/kilted/ros2trace/CHANGELOG.rst)

- 用于追踪工具的展览类型([\#153](https://github.com/ros2/ros2_tracing/issues/153))

- 贡献者:迈克尔·卡尔斯特罗姆

<span id="ros-environment"></span>

## [ros_environment](https://github.com/ros/ros_environment/tree/kilted/CHANGELOG.rst)

- 更新 Kilted Kaiju 的 ROS_DISTRO( 缩写)[\#41](https://github.com/ros/ros_environment/issues/41))

- 移除 CODEOWINERS. (中文(简体) ).[\#40](https://github.com/ros/ros_environment/issues/40))

- 贡献者:克里斯·拉兰谢特、斯科特·K·洛根

<span id="rosbag2"></span>

## [rosbag2](https://github.com/ros2/rosbag2/tree/kilted/rosbag2/CHANGELOG.rst)

- 支持重放多个袋( R)[\#1848](https://github.com/ros2/rosbag2/issues/1848))

- 贡献者:克里斯托弗·贝达德

<span id="rosbag2-compression"></span>

## [rosbag2_compression](https://github.com/ros2/rosbag2/tree/kilted/rosbag2_compression/CHANGELOG.rst)

- 错误fix: 在首次保存前, 用新文件更新元数据\_ info( I)[\#1843](https://github.com/ros2/rosbag2/issues/1843))

- 每次触发时, 将快照写入新文件( M)[\#1842](https://github.com/ros2/rosbag2/issues/1842))

- 添加 cli 选项压缩线程优先级( T)[\#1768](https://github.com/ros2/rosbag2/issues/1768))

- bag_split 事件调用错误fix 提前调用文件压缩( P)[\#1643](https://github.com/ros2/rosbag2/issues/1643))

- 返回的修补 `open_succeeds_twice` 财务报告和财务报告 `minimal_writer_example` 测试([\#1667](https://github.com/ros2/rosbag2/issues/1667))

- 关闭后无法再次打开写入器的错误修正( B)[\#1599](https://github.com/ros2/rosbag2/issues/1599))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、迈克尔·奥尔洛夫、罗曼、尤舒尔茨

<span id="rosbag2-cpp"></span>

## [rosbag2_cpp](https://github.com/ros2/rosbag2/tree/kilted/rosbag2_cpp/CHANGELOG.rst)

- 添加对查找行动类型消息定义的支持 `LocalMessageDefinitionSource` 类可以存储记录过程中的动作消息定义。 ([\#1965](https://github.com/ros2/rosbag2/issues/1965))

- 将信件序列号添加到信件的 API ()[\#1961](https://github.com/ros2/rosbag2/issues/1961))

- 实施记录和显示记录行动特征的信息的行动([\#1939](https://github.com/ros2/rosbag2/issues/1939))

- 设定要运行测试的环境变量 `rmw_zenoh_cpp` 含有多播发现的( )[\#1946](https://github.com/ros2/rosbag2/issues/1946))

- 在存储和读者/写作机打开操作中增加更多的记录信息([\#1881](https://github.com/ros2/rosbag2/issues/1881))

- 添加玩家钟: 醒( ) 以中断睡眠( W)[\#1869](https://github.com/ros2/rosbag2/issues/1869))

- 支持重放多个袋( R)[\#1848](https://github.com/ros2/rosbag2/issues/1848))

- 错误fix: 在首次保存前, 用新文件更新元数据\_ info( I)[\#1843](https://github.com/ros2/rosbag2/issues/1843))

- 每次触发时, 将快照写入新文件( M)[\#1842](https://github.com/ros2/rosbag2/issues/1842))

- Rosbag2_cpp 序列化转换器的bugfix([\#1814](https://github.com/ros2/rosbag2/issues/1814))

- 允许在包重写中出现未知类型( N)[\#1812](https://github.com/ros2/rosbag2/issues/1812))

- 将大小贡献的计算添加到信息动词中( E)[\#1726](https://github.com/ros2/rosbag2/issues/1726))

- \[WIP\] 删除rcpputils::fs rospage2包中的依赖性([\#1740](https://github.com/ros2/rosbag2/issues/1740))

- 删除已贬值的写法( E)[\#1738](https://github.com/ros2/rosbag2/issues/1738))

- bag_split 事件调用错误fix 提前调用文件压缩( P)[\#1643](https://github.com/ros2/rosbag2/issues/1643))

- 向 SQLiteStorage 添加零消息计数的主题: get_metadata () 。 ([\#1725](https://github.com/ros2/rosbag2/issues/1725))

- 将“ custom_data” 和“ ros\_ distro” 写入元数据。 yaml 文件在重新索引时( )[\#1700](https://github.com/ros2/rosbag2/issues/1700))

- 关闭后无法再次打开写入器的错误修正( B)[\#1599](https://github.com/ros2/rosbag2/issues/1599))

- 撰稿人:亚历杭德罗·埃尔南德斯·科尔德罗、巴瑞·许、克里斯托弗·贝达德、科尔·塔克、迈克尔·奥尔洛夫、尼古拉·洛伊、藤田托莫亚、亚杜南德、尤舒尔茨

<span id="rosbag2-examples-cpp"></span>

## [rosbag2_examples_cpp](https://github.com/ros2/rosbag2/tree/kilted/rosbag2_examples/rosbag2_examples_cpp/CHANGELOG.rst)

- 添加 rosbag2_示例_cpp/simple_bag_reader.cpp. (中文(简体) ).[\#1683](https://github.com/ros2/rosbag2/issues/1683))

- 贡献者:藤田友也

<span id="rosbag2-examples-py"></span>

## [rosbag2_examples_py](https://github.com/ros2/rosbag2/tree/kilted/rosbag2_examples/rosbag2_examples_py/CHANGELOG.rst)

- 避免将内部模块用于示例。 ([\#1905](https://github.com/ros2/rosbag2/issues/1905))

- 将 test_xmllint.py 添加到 python 包中 。 ([\#1879](https://github.com/ros2/rosbag2/issues/1879))

- simple_bag_reader.py 应发布每个计时器调用的数据 。 ([\#1767](https://github.com/ros2/rosbag2/issues/1767))

- 更改 python 示例以使用 rclpy 上下文管理器 。 ()[\#1758](https://github.com/ros2/rosbag2/issues/1758))

- 添加 rosbag2_示例_cpp/simple_bag_reader.cpp. (中文(简体) ).[\#1683](https://github.com/ros2/rosbag2/issues/1683))

- 撰稿人:克里斯·拉兰塞特、藤田友也

<span id="rosbag2-py"></span>

## [rosbag2_py](https://github.com/ros2/rosbag2/tree/kilted/rosbag2_py/CHANGELOG.rst)

- 将信件序列号添加到信件的 API ()[\#1961](https://github.com/ros2/rosbag2/issues/1961))

- 添加动作重播功能( N)[\#1955](https://github.com/ros2/rosbag2/issues/1955))

- 实施记录和显示记录行动特征的信息的行动([\#1939](https://github.com/ros2/rosbag2/issues/1939))

- 在 PyReader 和 PyCompress Reader 中向关闭方法添加绑定值( P)[\#1935](https://github.com/ros2/rosbag2/issues/1935))

- 从 pybind11_add_模块中删除SHALED ()[\#1929](https://github.com/ros2/rosbag2/issues/1929))

- ROS2 袋装游戏的进展栏([\#1836](https://github.com/ros2/rosbag2/issues/1836))

- 上游质量与Apex.AI第1部分相比有所变化([\#1903](https://github.com/ros2/rosbag2/issues/1903))

- 增加基于出版时间戳的重播支持( Q)[\#1876](https://github.com/ros2/rosbag2/issues/1876))

- 支持重放多个袋( R)[\#1848](https://github.com/ros2/rosbag2/issues/1848))

- 在 python3-dev 中添加构建依赖性。 ()[\#1863](https://github.com/ros2/rosbag2/issues/1863))

- 在“ros2 袋信息”指令中添加“-sort” CLI选项([\#1804](https://github.com/ros2/rosbag2/issues/1804))

- 删除 python_cmake_模块的使用( Name[\#1570](https://github.com/ros2/rosbag2/issues/1570))

- 添加到 Python 中的 QoS 的内观方法([\#1648](https://github.com/ros2/rosbag2/issues/1648))

- 更新 CI 脚本以使用 Ubuntu Noble distros 和 碰碰动作脚本到最新版本([\#1709](https://github.com/ros2/rosbag2/issues/1709))

- 添加 cli 选项压缩线程优先级( T)[\#1768](https://github.com/ros2/rosbag2/issues/1768))

- 将大小贡献的计算添加到信息动词中( E)[\#1726](https://github.com/ros2/rosbag2/issues/1726))

- ros2 袋信息中错误的时间戳的错误错误错误标记( B)[\#1745](https://github.com/ros2/rosbag2/issues/1745))

- 添加本地密钥定义源( O)[\#1697](https://github.com/ros2/rosbag2/issues/1697))

- 添加 -log- 级别到 ros2 袋播放和记录( )[\#1625](https://github.com/ros2/rosbag2/issues/1625))

- 包含到 Python 包装器中的_rclcpp_qos_vector([\#1642](https://github.com/ros2/rosbag2/issues/1642))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、巴瑞·徐、克里斯·拉朗谢特、克里斯托弗·贝达德、迈克尔·奥尔洛夫、尼古拉·洛伊、罗马、萨诺纳斯、西尔维奥·特拉韦萨罗、甲基德拉贡、奥伊斯坦·斯图尔

<span id="rosbag2-storage"></span>

## [rosbag2_storage](https://github.com/ros2/rosbag2/tree/kilted/rosbag2_storage/CHANGELOG.rst)

- 将信件序列号添加到信件的 API ()[\#1961](https://github.com/ros2/rosbag2/issues/1961))

- 添加动作重播功能( N)[\#1955](https://github.com/ros2/rosbag2/issues/1955))

- 在存储和读者/写作机打开操作中增加更多的记录信息([\#1881](https://github.com/ros2/rosbag2/issues/1881))

- 撰稿人:许巴里、迈克尔·奥尔洛夫

<span id="rosbag2-storage-mcap"></span>

## [rosbag2_storage_mcap](https://github.com/ros2/rosbag2/tree/kilted/rosbag2_storage_mcap/CHANGELOG.rst)

- 将信件序列号添加到信件的 API ()[\#1961](https://github.com/ros2/rosbag2/issues/1961))

- 添加动作重播功能( N)[\#1955](https://github.com/ros2/rosbag2/issues/1955))

- 上游质量与Apex.AI第1部分相比有所变化([\#1903](https://github.com/ros2/rosbag2/issues/1903))

- 添加 vscode gitignore 规则并删除 vscode 文件夹([\#1698](https://github.com/ros2/rosbag2/issues/1698))

- 贡献者:许巴里、迈克尔·奥尔洛夫、甲基德拉贡

<span id="rosbag2-storage-sqlite3"></span>

## [rosbag2_storage_sqlite3](https://github.com/ros2/rosbag2/tree/kilted/rosbag2_storage_sqlite3/CHANGELOG.rst)

- 添加动作重播功能( N)[\#1955](https://github.com/ros2/rosbag2/issues/1955))

- 为 sqlite 存储修正错误的零大小( S)[\#1759](https://github.com/ros2/rosbag2/issues/1759))

- 为在 Windows (Windows) 上失败的丢弃进行修复\_ on_invalid_pragma_in_config_file ()[\#1742](https://github.com/ros2/rosbag2/issues/1742))

- 向 SQLiteStorage 添加零消息计数的主题: get_metadata () 。 ([\#1725](https://github.com/ros2/rosbag2/issues/1725))

- 撰稿人:许巴里、迈克尔·奥尔洛夫、罗曼、藤田友也

<span id="rosbag2-test-common"></span>

## [rosbag2_test_common](https://github.com/ros2/rosbag2/tree/kilted/rosbag2_test_common/CHANGELOG.rst)

- 添加动作重播功能( N)[\#1955](https://github.com/ros2/rosbag2/issues/1955))

- 实施记录和显示记录行动特征的信息的行动([\#1939](https://github.com/ros2/rosbag2/issues/1939))

- 上游质量与Apex.AI第1部分相比有所变化([\#1903](https://github.com/ros2/rosbag2/issues/1903))

- 在 rosbag2 中使用 tmpfs 临时\_ directory_fixture ()[\#1901](https://github.com/ros2/rosbag2/issues/1901))

- 添加调试信息用于 Flashy can\_ record_again_after_stop test ([\#1871](https://github.com/ros2/rosbag2/issues/1871))

- 删除 python_cmake_模块的使用( Name[\#1570](https://github.com/ros2/rosbag2/issues/1570))

- 提高Rosbag2测试的可靠性([\#1796](https://github.com/ros2/rosbag2/issues/1796))

- 略作清理,以进行Rosbag2测试。 ([\#1792](https://github.com/ros2/rosbag2/issues/1792))

- \[WIP\] 删除rcpputils::fs rospage2包中的依赖性([\#1740](https://github.com/ros2/rosbag2/issues/1740))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、许巴里、克里斯·拉朗谢特、迈克尔·奥尔洛夫

<span id="rosbag2-test-msgdefs"></span>

## [rosbag2_test_msgdefs](https://github.com/ros2/rosbag2/tree/kilted/rosbag2_test_msgdefs/CHANGELOG.rst)

- 添加对查找行动类型消息定义的支持 `LocalMessageDefinitionSource` 类可以存储记录过程中的动作消息定义。 ([\#1965](https://github.com/ros2/rosbag2/issues/1965))

- 贡献者:藤田友也

<span id="rosbag2-tests"></span>

## [rosbag2_tests](https://github.com/ros2/rosbag2/tree/kilted/rosbag2_tests/CHANGELOG.rst)

- 实施记录和显示记录行动特征的信息的行动([\#1939](https://github.com/ros2/rosbag2/issues/1939))

- 上游质量与Apex.AI第1部分相比有所变化([\#1903](https://github.com/ros2/rosbag2/issues/1903))

- 将 test\_ rosbag2\_ record\_ end\_ to\_ end 的超时时间增加至 180s ()[\#1889](https://github.com/ros2/rosbag2/issues/1889))

- 在“ros2 袋信息”指令中添加“-sort” CLI选项([\#1804](https://github.com/ros2/rosbag2/issues/1804))

- 提高Rosbag2测试的可靠性([\#1796](https://github.com/ros2/rosbag2/issues/1796))

- 略作清理,以进行Rosbag2测试。 ([\#1792](https://github.com/ros2/rosbag2/issues/1792))

- 将大小贡献的计算添加到信息动词中( E)[\#1726](https://github.com/ros2/rosbag2/issues/1726))

- ros2 袋信息中错误的时间戳的错误错误错误标记( B)[\#1745](https://github.com/ros2/rosbag2/issues/1745))

- 修复在记录器中用包拆分进行虚假的负式集成测试([\#1743](https://github.com/ros2/rosbag2/issues/1743))

- 将“ custom_data” 和“ ros\_ distro” 写入元数据。 yaml 文件在重新索引时( )[\#1700](https://github.com/ros2/rosbag2/issues/1700))

- 清洗罗巴格2记录器 CLI 参数校验码([\#1633](https://github.com/ros2/rosbag2/issues/1633))

- 返回的修补 `open_succeeds_twice` 财务报告和财务报告 `minimal_writer_example` 测试([\#1667](https://github.com/ros2/rosbag2/issues/1667))

- 为“ ros2 袋记录” 添加可选的 QQ专题的 CLI 参数([\#1632](https://github.com/ros2/rosbag2/issues/1632))

- 关闭后无法再次打开写入器的错误修正( B)[\#1599](https://github.com/ros2/rosbag2/issues/1599))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、巴瑞·许、克里斯·拉朗谢特、科尔·塔克、迈克尔·奥尔洛夫、尼古拉·洛伊、萨诺罗纳斯、亚敦德、尤舒尔茨

<span id="rosbag2-transport"></span>

## [rosbag2_transport](https://github.com/ros2/rosbag2/tree/kilted/rosbag2_transport/CHANGELOG.rst)

- 添加动作重播功能( N)[\#1955](https://github.com/ros2/rosbag2/issues/1955))

- 实施记录和显示记录行动特征的信息的行动([\#1939](https://github.com/ros2/rosbag2/issues/1939))

- 设定要运行测试的环境变量 `rmw_zenoh_cpp` 含有多播发现的( )[\#1946](https://github.com/ros2/rosbag2/issues/1946))

- 初始化过滤器, 并带有命名空间更新的主题和服务 。 (滚动) ([\#1944](https://github.com/ros2/rosbag2/issues/1944))

- 修补: QoS不兼容不与rmw_zenoh_cpp([\#1936](https://github.com/ros2/rosbag2/issues/1936))

- 在进度栏类中地址窗口警告( E)[\#1927](https://github.com/ros2/rosbag2/issues/1927))

- 无法创建新订阅时不要删除现有订阅( Q)[\#1923](https://github.com/ros2/rosbag2/issues/1923))

- ROS2 袋装游戏的进展栏([\#1836](https://github.com/ros2/rosbag2/issues/1836))

- 上游质量与Apex.AI第1部分相比有所变化([\#1903](https://github.com/ros2/rosbag2/issues/1903))

- 在 rosbag2 中使用 tmpfs 临时\_ directory_fixture ()[\#1901](https://github.com/ros2/rosbag2/issues/1901))

- Bugfix: 被停止后记录器的发现不会重新启动( E) :[\#1894](https://github.com/ros2/rosbag2/issues/1894))

- Bugfix. 事件发布器在停止后没有开始第二次运行( Q)[\#1888](https://github.com/ros2/rosbag2/issues/1888))

- 增加基于出版时间戳的重播支持( Q)[\#1876](https://github.com/ros2/rosbag2/issues/1876))

- 延迟结束后公布时钟, 并禁用下个循环上的延迟( Name[\#1861](https://github.com/ros2/rosbag2/issues/1861))

- 添加玩家钟: 醒( ) 以中断睡眠( W)[\#1869](https://github.com/ros2/rosbag2/issues/1869))

- 添加调试信息用于 Flashy can\_ record_again_after_stop test ([\#1871](https://github.com/ros2/rosbag2/issues/1871))

- 支持重放多个袋( R)[\#1848](https://github.com/ros2/rosbag2/issues/1848))

- 重新启动( R) `Don't warn for unknown types if topics are not selected` ([\#1825](https://github.com/ros2/rosbag2/issues/1825))

- 允许在包重写中出现未知类型( N)[\#1812](https://github.com/ros2/rosbag2/issues/1812))

- 提高Rosbag2测试的可靠性([\#1796](https://github.com/ros2/rosbag2/issues/1796))

- 删除警告( E)[\#1794](https://github.com/ros2/rosbag2/issues/1794))

- 略作清理,以进行Rosbag2测试。 ([\#1792](https://github.com/ros2/rosbag2/issues/1792))

- 添加 cli 选项压缩线程优先级( T)[\#1768](https://github.com/ros2/rosbag2/issues/1768))

- \[WIP\] 删除rcpputils::fs rospage2包中的依赖性([\#1740](https://github.com/ros2/rosbag2/issues/1740))

- bag_split 事件调用错误fix 提前调用文件压缩( P)[\#1643](https://github.com/ros2/rosbag2/issues/1643))

- 无法通过压缩创建可堆肥节点的臭虫fix([\#1679](https://github.com/ros2/rosbag2/issues/1679))

- 在 RecordObjects YAML 解码器中添加对“所有”和“排除”的支持([\#1664](https://github.com/ros2/rosbag2/issues/1664))

- 添加单位测试以覆盖信件的发送和录音期间收到的时间戳( Q)[\#1641](https://github.com/ros2/rosbag2/issues/1641))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,巴瑞·许,克里斯·拉朗谢特,克里斯托弗·贝达德,迈克尔·奥尔洛夫,尼古拉·洛伊,拉蒙·维扬德斯,罗德里克·泰勒,罗曼,袁汝 ⁇ ,奥伊斯坦·斯图尔

<span id="rosidl-adapter"></span>

## [rosidl_adapter](https://github.com/ros2/rosidl/tree/kilted/rosidl_adapter/CHANGELOG.rst)

- rosidl_adapter 的类型 ([\#828](https://github.com/ros2/rosidl/issues/828))

- 支持嵌入3和嵌入4([\#821](https://github.com/ros2/rosidl/issues/821))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、迈克尔·卡尔斯特罗姆

<span id="rosidl-cli"></span>

## [rosidl_cli](https://github.com/ros2/rosidl/tree/kilted/rosidl_cli/CHANGELOG.rst)

- 罗西德尔 cli 类型 `specs_set` 固定( E)[\#831](https://github.com/ros2/rosidl/issues/831))

- 贡献者:克里斯·拉朗谢特、迈克尔·卡尔斯特罗姆

<span id="rosidl-core-generators"></span>

## [rosidl_core_generators](https://github.com/ros2/rosidl_core/tree/kilted/rosidl_core_generators/CHANGELOG.rst)

- 添加机制,使依赖群体无法工作([\#3](https://github.com/ros2/rosidl_core/issues/3))

- 撰稿人:斯科特·K·洛根

<span id="rosidl-core-runtime"></span>

## [rosidl_core_runtime](https://github.com/ros2/rosidl_core/tree/kilted/rosidl_core_runtime/CHANGELOG.rst)

- 添加机制,使依赖群体无法工作([\#3](https://github.com/ros2/rosidl_core/issues/3))

- 撰稿人:斯科特·K·洛根

<span id="rosidl-default-runtime"></span>

## [rosidl_default_runtime](https://github.com/ros2/rosidl_defaults/tree/kilted/rosidl_default_runtime/CHANGELOG.rst)

- 质量申报小幅更新([\#27](https://github.com/ros2/rosidl_defaults/issues/27))

- 贡献者:克里斯托弗·贝达德

<span id="rosidl-dynamic-typesupport"></span>

## [rosidl_dynamic_typesupport](https://github.com/ros2/rosidl_dynamic_typesupport/tree/kilted/CHANGELOG.rst)

- 切换到 ament_cmake_ros_core 软件包([\#15](https://github.com/ros2/rosidl_dynamic_typesupport/issues/15))

- 弹出最小CMake版本为3.20([\#14](https://github.com/ros2/rosidl_dynamic_typesupport/issues/14))

- 丢弃支持长双/浮128. ()[\#12](https://github.com/ros2/rosidl_dynamic_typesupport/issues/12))

- 贡献者:克里斯·拉朗谢特、迈克尔·卡罗尔、苔藓80

<span id="rosidl-dynamic-typesupport-fastrtps"></span>

## [rosidl_dynamic_typesupport_fastrtps](https://github.com/ros2/rosidl_dynamic_typesupport_fastrtps/tree/kilted/CHANGELOG.rst)

- 切换到 ament_cmake_ros_core 软件包([\#8](https://github.com/ros2/rosidl_dynamic_typesupport_fastrtps/issues/8))

- 对照 Fast DDS 3.0 构建的更改 ([\#5](https://github.com/ros2/rosidl_dynamic_typesupport_fastrtps/issues/5))

- 丢弃支持长双/浮128. ()[\#6](https://github.com/ros2/rosidl_dynamic_typesupport_fastrtps/issues/6))

- 贡献者:克里斯·拉兰谢特、米格尔公司、斯科特·K·洛根

<span id="rosidl-generator-c"></span>

## [rosidl_generator_c](https://github.com/ros2/rosidl/tree/kilted/rosidl_generator_c/CHANGELOG.rst)

- 切换到 ament_cmake_ros_core 软件包([\#856](https://github.com/ros2/rosidl/issues/856))

- 可复制编码的定型迭代顺序( Y)[\#846](https://github.com/ros2/rosidl/issues/846))

- 添加类型 `rosidl_pycommon` ([\#824](https://github.com/ros2/rosidl/issues/824))

- 贡献者:哈里·萨森、迈克尔·卡尔斯特罗姆、迈克尔·卡罗尔

<span id="rosidl-generator-cpp"></span>

## [rosidl_generator_cpp](https://github.com/ros2/rosidl/tree/kilted/rosidl_generator_cpp/CHANGELOG.rst)

- 为动作添加名称和数据_类型特性( S)[\#848](https://github.com/ros2/rosidl/issues/848))

- 添加类型 `rosidl_pycommon` ([\#824](https://github.com/ros2/rosidl/issues/824))

- 撰稿人:迈克尔·卡尔斯特罗姆、内森·维贝·诺伊费尔特

<span id="rosidl-generator-dds-idl"></span>

## [rosidl_generator_dds_idl](https://github.com/ros2/rosidl_dds/tree/kilted/rosidl_generator_dds_idl/CHANGELOG.rst)

- 更新 cmake 版本要求( E)[\#64](https://github.com/ros2/rosidl_dds/issues/64))

- 贡献者:苔藓80

<span id="rosidl-generator-py"></span>

## [rosidl_generator_py](https://github.com/ros2/rosidl_python/tree/kilted/rosidl_generator_py/CHANGELOG.rst)

- 修补 `__eq__` 用于阵列字段([\#224](https://github.com/ros2/rosidl_python/issues/224))

- 删除使用ament_target_依赖性([\#222](https://github.com/ros2/rosidl_python/issues/222))

- 校对:Soup[\#218](https://github.com/ros2/rosidl_python/issues/218))

- 删除 python_cmake_模块并设置提示([\#204](https://github.com/ros2/rosidl_python/issues/204))

- 在 rosidl_runtime\_ packages 组中添加 rosidl_生成器_py([\#212](https://github.com/ros2/rosidl_python/issues/212))

- 贡献者:克里斯·拉朗谢特、迈克尔·卡尔斯特罗姆、斯科特·K·洛根、谢恩·洛雷茨

<span id="rosidl-generator-tests"></span>

## [rosidl_generator_tests](https://github.com/ros2/rosidl/tree/kilted/rosidl_generator_tests/CHANGELOG.rst)

- 为动作添加名称和数据_类型特性( S)[\#848](https://github.com/ros2/rosidl/issues/848))

- 安静一个gcc假阳性。 ([\#814](https://github.com/ros2/rosidl/issues/814))

- 切换到使用 fastjsonschema 进行计划验证 。 ()[\#809](https://github.com/ros2/rosidl/issues/809))

- 贡献者:克里斯·拉朗谢特、内森·维贝·诺伊费尔特

<span id="rosidl-generator-type-description"></span>

## [rosidl_generator_type_description](https://github.com/ros2/rosidl/tree/kilted/rosidl_generator_type_description/CHANGELOG.rst)

- 切换到 ament_cmake_ros_core 软件包([\#856](https://github.com/ros2/rosidl/issues/856))

- 撰稿人:迈克尔·卡罗尔

<span id="rosidl-parser"></span>

## [rosidl_parser](https://github.com/ros2/rosidl/tree/kilted/rosidl_parser/CHANGELOG.rst)

- 完成添加类型到 `rosidl_parser` ([\#832](https://github.com/ros2/rosidl/issues/832))

- 添加类型到定义.py in `rosidl_parser` ([\#791](https://github.com/ros2/rosidl/issues/791))

- 贡献者:迈克尔·卡尔斯特罗姆

<span id="rosidl-pycommon"></span>

## [rosidl_pycommon](https://github.com/ros2/rosidl/tree/kilted/rosidl_pycommon/CHANGELOG.rst)

- 将 test_xmllint 添加到 rosidl_pycommon 。 ()[\#833](https://github.com/ros2/rosidl/issues/833))

- 添加类型 `rosidl_pycommon` ([\#824](https://github.com/ros2/rosidl/issues/824))

- 支持嵌入3和嵌入4([\#821](https://github.com/ros2/rosidl/issues/821))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、迈克尔·卡尔斯特罗姆

<span id="rosidl-runtime-c"></span>

## [rosidl_runtime_c](https://github.com/ros2/rosidl/tree/kilted/rosidl_runtime_c/CHANGELOG.rst)

- 切换到 ament_cmake_ros_core 软件包([\#856](https://github.com/ros2/rosidl/issues/856))

- 执行 `resize` 字符串的函数 (E)[\#806](https://github.com/ros2/rosidl/issues/806))

- 修补u16文件并改进文件格式([\#805](https://github.com/ros2/rosidl/issues/805))

- 撰稿人:克里斯托弗·贝达德、迈克尔·卡罗尔、WATANABE AOI

<span id="rosidl-runtime-cpp"></span>

## [rosidl_runtime_cpp](https://github.com/ros2/rosidl/tree/kilted/rosidl_runtime_cpp/CHANGELOG.rst)

- 上游海合会虚假阳性物质基准中的禁用警告。 ([\#810](https://github.com/ros2/rosidl/issues/810))

- 撰稿人:克里斯·拉兰谢特

<span id="rosidl-runtime-py"></span>

## [rosidl_runtime_py](https://github.com/ros2/rosidl_runtime_py/tree/kilted/CHANGELOG.rst)

- 安全使用 set_message_fields 的深影。 ([\#34](https://github.com/ros2/rosidl_runtime_py/issues/34))

- 删除 CODEOWINERS 和 镜像滚动到主机 。 ([\#31](https://github.com/ros2/rosidl_runtime_py/issues/31))

- 撰稿人:克里斯·拉兰塞特、藤田友也

<span id="rosidl-typesupport-c"></span>

## [rosidl_typesupport_c](https://github.com/ros2/rosidl_typesupport/tree/kilted/rosidl_typesupport_c/CHANGELOG.rst)

- 切换到 ament_cmake_ros_core 软件包([\#166](https://github.com/ros2/rosidl_typesupport/issues/166))

- 统一生产要求[\#163](https://github.com/ros2/rosidl_typesupport/issues/163))

- 在 rosidl_typesupport_c 测试中清除警告消息 ()[\#161](https://github.com/ros2/rosidl_typesupport/issues/161))

- 添加机制,使依赖群体无法工作([\#157](https://github.com/ros2/rosidl_typesupport/issues/157))

- 在使用Mimick的测试中添加“ mimick” 标签([\#158](https://github.com/ros2/rosidl_typesupport/issues/158))

- 贡献者:克里斯·拉兰谢特、斯科特·K·洛根、苔丝菲特80

<span id="rosidl-typesupport-cpp"></span>

## [rosidl_typesupport_cpp](https://github.com/ros2/rosidl_typesupport/tree/kilted/rosidl_typesupport_cpp/CHANGELOG.rst)

- 切换到 ament_cmake_ros_core 软件包([\#166](https://github.com/ros2/rosidl_typesupport/issues/166))

- 统一生产要求[\#163](https://github.com/ros2/rosidl_typesupport/issues/163))

- 添加机制,使依赖群体无法工作([\#157](https://github.com/ros2/rosidl_typesupport/issues/157))

- 贡献者: Scott K Logan, mossfet80

<span id="rosidl-typesupport-fastrtps-c"></span>

## [rosidl_typesupport_fastrtps_c](https://github.com/ros2/rosidl_typesupport_fastrtps/tree/kilted/rosidl_typesupport_fastrtps_c/CHANGELOG.rst)

- 切换到 ament_cmake_ros_core 软件包([\#127](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/127))

- 删除对 fastrtps\_ cmake\_ 模块的依赖( )[\#120](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/120))

- 删除 CODEOWINERS 和镜像滚动到主工作流程([\#124](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/124))

- 删除已贬值的函数基准测试( R)[\#122](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/122))

- 贡献者:克里斯·拉兰谢特、米格尔公司、斯科特·K·洛根

<span id="rosidl-typesupport-fastrtps-cpp"></span>

## [rosidl_typesupport_fastrtps_cpp](https://github.com/ros2/rosidl_typesupport_fastrtps/tree/kilted/rosidl_typesupport_fastrtps_cpp/CHANGELOG.rst)

- 切换到 ament_cmake_ros_core 软件包([\#127](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/127))

- 删除对 fastrtps\_ cmake\_ 模块的依赖( )[\#120](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/120))

- 删除 CODEOWINERS 和镜像滚动到主工作流程([\#124](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/124))

- 删除已贬值的函数基准测试( R)[\#122](https://github.com/ros2/rosidl_typesupport_fastrtps/issues/122))

- 贡献者:克里斯·拉兰谢特、米格尔公司、斯科特·K·洛根

<span id="rosidl-typesupport-introspection-c"></span>

## [rosidl_typesupport_introspection_c](https://github.com/ros2/rosidl/tree/kilted/rosidl_typesupport_introspection_c/CHANGELOG.rst)

- 切换到 ament_cmake_ros_core 软件包([\#860](https://github.com/ros2/rosidl/issues/860))

- 添加类型 `rosidl_pycommon` ([\#824](https://github.com/ros2/rosidl/issues/824))

- 贡献者:迈克尔·卡尔斯特罗姆、斯科特·K·洛根

<span id="rosidl-typesupport-introspection-cpp"></span>

## [rosidl_typesupport_introspection_cpp](https://github.com/ros2/rosidl/tree/kilted/rosidl_typesupport_introspection_cpp/CHANGELOG.rst)

- 切换到 ament_cmake_ros_core 软件包([\#860](https://github.com/ros2/rosidl/issues/860))

- 添加类型 `rosidl_pycommon` ([\#824](https://github.com/ros2/rosidl/issues/824))

- 贡献者:迈克尔·卡尔斯特罗姆、斯科特·K·洛根

<span id="rosidl-typesupport-introspection-tests"></span>

## [rosidl_typesupport_introspection_tests](https://github.com/ros2/rosidl/tree/kilted/rosidl_typesupport_introspection_tests/CHANGELOG.rst)

- 从gcc.处取缔虚假的阳性警告([\#811](https://github.com/ros2/rosidl/issues/811))

- 撰稿人:克里斯·拉兰谢特

<span id="rosidl-typesupport-tests"></span>

## [rosidl_typesupport_tests](https://github.com/ros2/rosidl_typesupport/tree/kilted/rosidl_typesupport_tests/CHANGELOG.rst)

- 统一生产要求[\#163](https://github.com/ros2/rosidl_typesupport/issues/163))

- 贡献者:苔藓80

<span id="rpyutils"></span>

## [rpyutils (英语).](https://github.com/ros2/rpyutils/tree/kilted/CHANGELOG.rst)

- 向软件包数据添加 py.typed ()[\#16](https://github.com/ros2/rpyutils/issues/16))

- 添加创建 py.typed ()[\#15](https://github.com/ros2/rpyutils/issues/15))

- 删除 CODEOWINERS 和镜像滚动到主工作流程 。 ([\#13](https://github.com/ros2/rpyutils/issues/13))

- 在 rpyutils 中添加类型和ment_mypy 。 ([\#12](https://github.com/ros2/rpyutils/issues/12))

- 贡献者:克里斯·拉朗谢特、迈克尔·卡尔斯特罗姆

<span id="rqt-bag"></span>

## [rqt_bag](https://github.com/ros-visualization/rqt_bag/tree/kilted/rqt_bag/CHANGELOG.rst)

- 对 rqt_bag 和 rqt_bag\_ plugins 添加标准测试([\#171](https://github.com/ros-visualization/rqt_bag/issues/171))

- 更新的玩家QoS ()[\#164](https://github.com/ros-visualization/rqt_bag/issues/164))

- 适应 rosbag2_py ([\#156](https://github.com/ros-visualization/rqt_bag/issues/156))

- 固定按钮图标( E)[\#159](https://github.com/ros-visualization/rqt_bag/issues/159))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特

<span id="rqt-bag-plugins"></span>

## [rqt_bag_plugins](https://github.com/ros-visualization/rqt_bag/tree/kilted/rqt_bag_plugins/CHANGELOG.rst)

- 对 rqt_bag 和 rqt_bag\_ plugins 添加标准测试([\#171](https://github.com/ros-visualization/rqt_bag/issues/171))

- 适应 rosbag2_py ([\#156](https://github.com/ros-visualization/rqt_bag/issues/156))

- 固定图像时间线渲染器( E)[\#158](https://github.com/ros-visualization/rqt_bag/issues/158))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特

<span id="rqt-console"></span>

## [rqt_console](https://github.com/ros-visualization/rqt_console/tree/kilted/CHANGELOG.rst)

- 在标准测试中添加。 ()[\#48](https://github.com/ros-visualization/rqt_console/issues/48))

- 删除 CODEOWINERS 和镜像滚动到主工作流程([\#46](https://github.com/ros-visualization/rqt_console/issues/46))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特

<span id="rqt-graph"></span>

## [rqt_graph](https://github.com/ros-visualization/rqt_graph/tree/kilted/CHANGELOG.rst)

- 在标准测试中添加。 ()[\#104](https://github.com/ros-visualization/rqt_graph/issues/104))

- 删除 CODEOWINERS ()[\#102](https://github.com/ros-visualization/rqt_graph/issues/102))

- 固定的fit_in_视图图标按钮([\#95](https://github.com/ros-visualization/rqt_graph/issues/95))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特

<span id="rqt-gui"></span>

## [rqt_gui](https://github.com/ros-visualization/rqt/tree/kilted/rqt_gui/CHANGELOG.rst)

- 在 rqt_gui 和 rqt_gui_py 的标准测试中添加([\#318](https://github.com/ros-visualization/rqt/issues/318))

- 撰稿人:克里斯·拉兰谢特

<span id="rqt-gui-cpp"></span>

## [rqt_gui_cpp](https://github.com/ros-visualization/rqt/tree/kilted/rqt_gui_cpp/CHANGELOG.rst)

- 在 rqt_gui_cpp 中添加了常见的测试,并调换 h 标题([\#311](https://github.com/ros-visualization/rqt/issues/311))

- 更新已贬值的 qt\_ gui\_ cpp 信头( )[\#309](https://github.com/ros-visualization/rqt/issues/309))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗

<span id="rqt-gui-py"></span>

## [rqt_gui_py](https://github.com/ros-visualization/rqt/tree/kilted/rqt_gui_py/CHANGELOG.rst)

- 在 rqt_gui 和 rqt_gui_py 的标准测试中添加([\#318](https://github.com/ros-visualization/rqt/issues/318))

- 撰稿人:克里斯·拉兰谢特

<span id="rqt-plot"></span>

## [rqt_plot](https://github.com/ros-visualization/rqt_plot/tree/kilted/CHANGELOG.rst)

- 添加主题名称验证和字段扩展的单元测试( NAME OF TRANSLATORS)[\#108](https://github.com/ros-visualization/rqt_plot/issues/108))

- 在所有子场布局时用后方斜线固定双斜线([\#107](https://github.com/ros-visualization/rqt_plot/issues/107))

- 修复嵌入式基本类型字段列表( E)[\#101](https://github.com/ros-visualization/rqt_plot/issues/101))

- 修整 f 字符串并在字段名称周围添加单引号([\#100](https://github.com/ros-visualization/rqt_plot/issues/100))

- 在校验 msg 中添加围绕主题的单引号, 以求一致性( Q)[\#99](https://github.com/ros-visualization/rqt_plot/issues/99)这与下文的其他信息更为一致。

- 在标准ament_python测试的其余部分中加入. (.[\#98](https://github.com/ros-visualization/rqt_plot/issues/98))

- 删除 CODEOWINERS ()[\#96](https://github.com/ros-visualization/rqt_plot/issues/96))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、克里斯托弗·贝达德

<span id="rqt-publisher"></span>

## [rqt_publisher](https://github.com/ros-visualization/rqt_publisher/tree/kilted/CHANGELOG.rst)

- 在剩余的标准ament_python测试中添加. ([\#49](https://github.com/ros-visualization/rqt_publisher/issues/49))

- 在LICENSE中添加。 (中文(简体) )[\#46](https://github.com/ros-visualization/rqt_publisher/issues/46))

- 删除 CODEOWINERS ()[\#47](https://github.com/ros-visualization/rqt_publisher/issues/47))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特

<span id="rqt-py-common"></span>

## [rqt_py_common](https://github.com/ros-visualization/rqt/tree/kilted/rqt_py_common/CHANGELOG.rst)

- 停止使用 python\_ cmake_模块. ()[\#304](https://github.com/ros-visualization/rqt/issues/304))

- 使用 rclpy 上下文管理器 。 ([\#312](https://github.com/ros-visualization/rqt/issues/312))

- 添加到 rqt\_ py\_ common () 的常见测试[\#310](https://github.com/ros-visualization/rqt/issues/310))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特

<span id="rqt-py-console"></span>

## [rqt_py_console](https://github.com/ros-visualization/rqt_py_console/tree/kilted/CHANGELOG.rst)

- 将标准测试添加到 rqt_py\_ console. ()[\#19](https://github.com/ros-visualization/rqt_py_console/issues/19))

- 删除 CODEOWINERS ()[\#17](https://github.com/ros-visualization/rqt_py_console/issues/17))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特

<span id="rqt-service-caller"></span>

## [rqt_service_caller](https://github.com/ros-visualization/rqt_service_caller/tree/kilted/CHANGELOG.rst)

- 更新 rqt\_ service\_ caller 到我们的标准政策 。 ([\#31](https://github.com/ros-visualization/rqt_service_caller/issues/31))

- 删除 CODEOWINERS ()[\#29](https://github.com/ros-visualization/rqt_service_caller/issues/29))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特

<span id="rqt-shell"></span>

## [rqt_shell](https://github.com/ros-visualization/rqt_shell/tree/kilted/CHANGELOG.rst)

- 在标准测试中添加到rqt_shell. ().[\#24](https://github.com/ros-visualization/rqt_shell/issues/24)我们知道它符合我们的标准。

- 删除 CODEOWINERS ()[\#22](https://github.com/ros-visualization/rqt_shell/issues/22))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特

<span id="rqt-topic"></span>

## [rqt_topic](https://github.com/ros-visualization/rqt_topic/tree/kilted/CHANGELOG.rst)

- 覆盖订阅者 qos ()[\#51](https://github.com/ros-visualization/rqt_topic//issues/51))

- 删除 CODEOWINERS ()[\#52](https://github.com/ros-visualization/rqt_topic//issues/52))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗

<span id="rti-connext-dds-cmake-module"></span>

## [rti_connext_dds_cmake_module](https://github.com/ros2/rmw_connextdds/tree/kilted/rti_connext_dds_cmake_module/CHANGELOG.rst)

- 更新到 7.3.0 (中文(简体) ).[\#181](https://github.com/ros2/rmw_connextdds/issues/181))

- 未找到 CONNEXTDDS_DIR 或 NDDSHOME 时, 安静警告 。 ([\#158](https://github.com/ros2/rmw_connextdds/issues/158))

- 贡献者: 克里斯·拉兰谢特,洛博兰贾

<span id="rttest"></span>

## [测试](https://github.com/ros2/realtime_support/tree/kilted/rttest/CHANGELOG.rst)

- 不要试图建立在 BSD 之上([\#126](https://github.com/ros2/realtime_support/issues/126))

- 撰稿人:斯科特·K·洛根

<span id="rviz2"></span>

## [rviz2 (中文(简体) ).](https://github.com/ros2/rviz/tree/kilted/rviz2/CHANGELOG.rst)

- 显示使用自定义创建ROS节点的可能性 `NodeOptions` ([\#1347](https://github.com/ros2/rviz/issues/1347))

- 统一的CMAK要求([\#1335](https://github.com/ros2/rviz/issues/1335))

- 探测到路面,确保使用X渲染。 ([\#1253](https://github.com/ros2/rviz/issues/1253))

- 固定 RViz2 内插件([\#1231](https://github.com/ros2/rviz/issues/1231))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、马修·埃尔温、帕特里克·龙卡廖洛、莫斯费特80

<span id="rviz-assimp-vendor"></span>

## [rviz_assimp_vendor](https://github.com/ros2/rviz/tree/kilted/rviz_assimp_vendor/CHANGELOG.rst)

- 重置“ 更新 ASIMP\_ VENDOR CMakeLists.txt ([\#1226](https://github.com/ros2/rviz/issues/1226))” ([\#1249](https://github.com/ros2/rviz/issues/1249))

- 更新 ASIMP_VENDOR CMakeLists.txt (英语).[\#1226](https://github.com/ros2/rviz/issues/1226)5.3.1 标准C++ 标准C+ 标准C+ 标准C+ 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准 标准C 标准C 标准C 标准C 标准C 标准 标准C 标准 标准C 标准C 标准C 标准C 标准C 标准C 标准C 标准 标准 标准 标准 标准 C 标准 标准 C 标准 C 标准 C 标准 C 标准 C 标准 C 标准 标准 标准 C 标准 标准 C 标准 标准 C 标准 标准 C 标准 标准 标准 标准 标准 C 标准 C 标准 标准 C 标准 标准 标准 标准 标准 C 标准 C 标准 标准 标准 标准 标准 标准 标准 标准 标准 标准 标准 C 标准

- 贡献者:克里斯·拉朗谢特,苔藓80

<span id="rviz-common"></span>

## [rviz_common](https://github.com/ros2/rviz/tree/kilted/rviz_common/CHANGELOG.rst)

- 正在使用新的资源检索器 apis 进行中([\#1262](https://github.com/ros2/rviz/issues/1262))

- 添加跟踪对象函数失败到处理 Null 指针, 当无效程序通过时导致崩溃([\#1375](https://github.com/ros2/rviz/issues/1375))

- 添加测试以检查缺少密钥时的映射GetString([\#1361](https://github.com/ros2/rviz/issues/1361))

- 统一样式: parseFloat 失败, 无法正确处理无效的浮点格式( S) :[\#1360](https://github.com/ros2/rviz/issues/1360))

- 在VisualizerApp中修正潜在的 Null 指针删除: getRender Window () 防止崩溃([\#1359](https://github.com/ros2/rviz/issues/1359))

- 扩展对类型适应的支持( REP 2007) rviz\_ 常见的 TF 过滤显示( )[\#1346](https://github.com/ros2/rviz/issues/1346))

- 显示使用自定义创建ROS节点的可能性 `NodeOptions` ([\#1347](https://github.com/ros2/rviz/issues/1347))

- 统一的CMAK要求([\#1335](https://github.com/ros2/rviz/issues/1335))

- 为适应类型增加基本支持(REP 2007) `rviz_common` 用于显示( E)[\#1331](https://github.com/ros2/rviz/issues/1331))

- 修复首选工具加载名称( F)[\#1321](https://github.com/ros2/rviz/issues/1321))

- 将 RVIZ_COMMON_PUBLIC 宏添加到工具管理器([\#1323](https://github.com/ros2/rviz/issues/1323))

- 清洁可视化_manager.cpp (中文(简体) ).[\#1317](https://github.com/ros2/rviz/issues/1317))

- 已折旧的 tf2 信头( X)[\#1289](https://github.com/ros2/rviz/issues/1289))

- 包括 QString ([\#1298](https://github.com/ros2/rviz/issues/1298))

- 处理时间源例外( E)[\#1285](https://github.com/ros2/rviz/issues/1285))

- 手柄齐全 `Tool::processKeyEvent` 返回值( E)[\#1270](https://github.com/ros2/rviz/issues/1270))

- 处理 `Tool::Finished` 返回日期为 `processKeyEvent` ([\#1257](https://github.com/ros2/rviz/issues/1257))

- 在Windwos的版权上增加了更多的时间([\#1252](https://github.com/ros2/rviz/issues/1252))

- 添加了 rviz\_ common () 的常见测试[\#1232](https://github.com/ros2/rviz/issues/1232))

- 将 RenderPanel 的内容标记设为 0, 以避免在全屏模式下设置边框 。 [\#1024](https://github.com/ros2/rviz/issues/1024) ([\#1228](https://github.com/ros2/rviz/issues/1228))

- 已更新的已折旧信件过滤信头( E)[\#1239](https://github.com/ros2/rviz/issues/1239))

- 带有白色空格的面板的 Correclty 加载图标([\#1241](https://github.com/ros2/rviz/issues/1241))

- qos 折旧的预估([\#1214](https://github.com/ros2/rviz/issues/1214))

- 将 ESC 退出全屏幕的快捷键替换为解析器 <https://github.com/ros-visualization/rviz/pull/1416> ([\#1205](https://github.com/ros2/rviz/issues/1205))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,博·陈,卢卡斯·温德兰,马修·福伦,迈克尔·卡罗尔,迈克尔·里佩格,帕特里克·龙卡廖洛,拉杜波佩斯库,西尔维奥·特拉韦萨罗,苔丝费特80

<span id="rviz-default-plugins"></span>

## [rviz_default_plugins](https://github.com/ros2/rviz/tree/kilted/rviz_default_plugins/CHANGELOG.rst)

- PointCloudDisplay: 修正衰变时间 0 保存多于上一封信件([\#1400](https://github.com/ros2/rviz/issues/1400))

- 正在使用新的资源检索器 apis 进行中([\#1262](https://github.com/ros2/rviz/issues/1262))

- 包含 chrono ([\#1353](https://github.com/ros2/rviz/issues/1353))

- 固定 : 添加 rclpp: shutdown ()[\#1343](https://github.com/ros2/rviz/issues/1343))

- Nv12 颜色格式 ([\#1318](https://github.com/ros2/rviz/issues/1318))合著者: ⁇ (zycczy) \<[zycczyby@gmail.com](mailto:zycczyby%40gmail.com)\>

- 统一的CMAK要求([\#1335](https://github.com/ros2/rviz/issues/1335))

- 在编译时只初始化一次查询表( E)[\#1330](https://github.com/ros2/rviz/issues/1330))合著:亚历杭德罗·埃尔南德斯·科尔德罗 \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 固定 XY 轨道控制器移动( Q)[\#1327](https://github.com/ros2/rviz/issues/1327))合著:特里·斯科特 \<[tscott@seegrid.com](mailto:tscott%40seegrid.com)\>

- 已折旧的 tf2 信头( X)[\#1289](https://github.com/ros2/rviz/issues/1289))

- 将 FortDisplay 超类从 MessageFilterDisplay 更改为 RosTopicDisplay 以避免空帧\_ id 丢弃信件 。 ([\#1312](https://github.com/ros2/rviz/issues/1312))

- 修复 Accel、Effet 和 Twist 显示器的访问控制([\#1311](https://github.com/ros2/rviz/issues/1311))

- 删除未使用的变量( E)[\#1301](https://github.com/ros2/rviz/issues/1301))

- 包括 QString ([\#1298](https://github.com/ros2/rviz/issues/1298))

- 图像显示的清洁代码( C)[\#1271](https://github.com/ros2/rviz/issues/1271))

- 处理时间源例外( E)[\#1285](https://github.com/ros2/rviz/issues/1285))

- 替换已折旧的编码\`yuv422 ' 和\`yuv422_yuy2 ' ([\#1276](https://github.com/ros2/rviz/issues/1276))

- 更新 urdf 模型.h 折旧 ([\#1266](https://github.com/ros2/rviz/issues/1266))

- 启用 TextViewFacingMark 的手动空间宽度([\#1261](https://github.com/ros2/rviz/issues/1261))

- 在Windwos的版权上增加了更多的时间([\#1252](https://github.com/ros2/rviz/issues/1252))

- 已更新的已折旧信件过滤信头( E)[\#1239](https://github.com/ros2/rviz/issues/1239))

- 固定的 RViz 默认插件许可插件 linter( Name[\#1230](https://github.com/ros2/rviz/issues/1230))

- 撰稿人:亚历杭德罗·埃尔南德斯·科尔德罗,克里斯蒂安·劳奇,卢卡斯·温德兰,马修·福伦,迈克尔·卡罗尔,帕特里克·龙卡廖洛,彭王,斯特凡·法比安,特里·斯科特,汤姆·摩尔,尤尤安·袁,解录,苔藓·费特80, ⁇ - ⁇ , ⁇ 族.

<span id="rviz-ogre-vendor"></span>

## [rviz_ogre_vendor](https://github.com/ros2/rviz/tree/kilted/rviz_ogre_vendor/CHANGELOG.rst)

- 添加缺少的食人怪供应商包的 glew 依赖性( Q)[\#1350](https://github.com/ros2/rviz/issues/1350))

- 使用正式的自由型 Github 镜像代替稀树草原([\#1348](https://github.com/ros2/rviz/issues/1348))

- 为clang和gcc两种标记进行固定([\#1219](https://github.com/ros2/rviz/issues/1219))

- 更新自由类型 lib ()[\#1216](https://github.com/ros2/rviz/issues/1216))

- 将 zlib 更新到 CMakeLists.txt ([\#1128](https://github.com/ros2/rviz/issues/1128)(18 Aug 2023) - 删除 K&R 函数定义和 zlib2ansi - 为 0 级和 mem Level Bound () 修复 中的错误 - Gzungetc () 之后立即使用 gzungetc () 时的修复 bund () 使用非常小的缓冲器时的修复 bund () 使用 gzflush () 时的修复 bund () 尝试透明写时的修复 bund () - Gzsetparams () 尝试使用透明写时的修复 bund () - Fix testion/ example. c 与 FORCE_STORED合作 - 在示例中重写 zran(参见zran.c版本历史) - Fix mizip 允许它打开一个空的zip 文件 - 在 minizip 参数处理中将 zip 逻辑错误在 zip 文件上开始读取回读盘号 - 添加 minzip 测试以 Makefile - 在 mitzip unzip.c 中读取多个字节,而不是字节字节。 - 添加内存消毒器来配置(–memory) - 各种可移植性改进 - 各种文档改进 - 各种拼写和类型校正 由 Chris Lalancette 共同撰写 \<[clalancette@gmail.com](mailto:clalancette%40gmail.com)\>

- 贡献者:克里斯·拉朗塞特、西尔维奥·特拉韦萨罗、斯特凡·法比安、苔藓80

<span id="rviz-rendering"></span>

## [rviz_rendering](https://github.com/ros2/rviz/tree/kilted/rviz_rendering/CHANGELOG.rst)

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

- 统一的CMAK要求([\#1335](https://github.com/ros2/rviz/issues/1335))

- 干净的食人怪\_ 投降\_ 窗口\_ impl.cpp (英语).[\#1334](https://github.com/ros2/rviz/issues/1334))

- 包括 QString ([\#1298](https://github.com/ros2/rviz/issues/1298))

- 在 compare_system.hpp 中使用一致的条件([\#1294](https://github.com/ros2/rviz/issues/1294))

- 避免重新定义默认的颜色材料( E)[\#1281](https://github.com/ros2/rviz/issues/1281))

- 在Windwos的版权上增加了更多的时间([\#1252](https://github.com/ros2/rviz/issues/1252))

- 修补:问题 [\#1220](https://github.com/ros2/rviz/issues/1220). ([\#1237](https://github.com/ros2/rviz/issues/1237))合著:亚历杭德罗·埃尔南德斯·科尔德罗 \<[ahcorde@gmail.com](mailto:ahcorde%40gmail.com)\>

- 添加的常见测试: rviz\_ landing ([\#1233](https://github.com/ros2/rviz/issues/1233))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,马萨约希·多希,马修·福伦,迈克尔·卡罗尔,斯科特·K·洛根,查马1176,摩斯费特80

<span id="rviz-rendering-tests"></span>

## [rviz_rendering_tests](https://github.com/ros2/rviz/tree/kilted/rviz_rendering_tests/CHANGELOG.rst)

- 正在使用新的资源检索器 apis 进行中([\#1262](https://github.com/ros2/rviz/issues/1262))

- 统一的CMAK要求([\#1335](https://github.com/ros2/rviz/issues/1335))

- 添加到 rviz_landing\_ tests 中的常见测试([\#1234](https://github.com/ros2/rviz/issues/1234))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、迈克尔·卡罗尔、莫斯费特80

<span id="rviz-resource-interfaces"></span>

## [rviz_resource_interfaces](https://github.com/ros2/rviz/tree/kilted/rviz_resource_interfaces/CHANGELOG.rst)

- 正在使用新的资源检索器 apis 进行中([\#1262](https://github.com/ros2/rviz/issues/1262))

- 撰稿人:迈克尔·卡罗尔

<span id="rviz-visual-testing-framework"></span>

## [rviz_visual_testing_framework](https://github.com/ros2/rviz/tree/kilted/rviz_visual_testing_framework/CHANGELOG.rst)

- 统一的CMAK要求([\#1335](https://github.com/ros2/rviz/issues/1335))

- 已折旧的 tf2 信头( X)[\#1289](https://github.com/ros2/rviz/issues/1289))

- 包括 QString ([\#1298](https://github.com/ros2/rviz/issues/1298))

- 添加到 rviz_visual\_ testing_framework 的常见测试([\#1235](https://github.com/ros2/rviz/issues/1235))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、卢卡斯·温德兰、马修·福伦、莫斯费特80

<span id="sensor-msgs"></span>

## [sensor_msgs](https://github.com/ros2/common_interfaces/tree/kilted/sensor_msgs/CHANGELOG.rst)

- 将 NV12 添加到颜色格式 ([\#253](https://github.com/ros2/common_interfaces/issues/253))

- 贡献者:Lukas Schäper

<span id="sensor-msgs-py"></span>

## [sensor_msgs_py](https://github.com/ros2/common_interfaces/tree/kilted/sensor_msgs_py/CHANGELOG.rst)

- 将 ament_xmllint 添加到传感器_msgs_py. ().[\#259](https://github.com/ros2/common_interfaces/issues/259))

- 以传感器\_ msgs_py([\#248](https://github.com/ros2/common_interfaces/issues/248))

- 贡献者:克里斯·拉朗谢特、克里斯托弗·贝达德

<span id="service-msgs"></span>

## [service_msgs](https://github.com/ros2/rcl_interfaces/tree/kilted/service_msgs/CHANGELOG.rst)

- 添加缺失的构建\_ 导出\_ 依赖 rosidl\_ core_runtime ()[\#165](https://github.com/ros2/rcl_interfaces/issues/165))

- 撰稿人:斯科特·K·洛根

<span id="sqlite3-vendor"></span>

## [sqlite3_vendor](https://github.com/ros2/rosbag2/tree/kilted/sqlite3_vendor/CHANGELOG.rst)

- Bump sqlite3 至 3.45.1 (中文(简体) ).[\#1737](https://github.com/ros2/rosbag2/issues/1737))

- 贡献者:克里斯托弗·贝达德

<span id="sros2"></span>

## [斜线2](https://github.com/ros2/sros2/tree/kilted/sros2/CHANGELOG.rst)

- 切换为获取_rmw_附加_env([\#339](https://github.com/ros2/sros2/issues/339))

- 修正 github- workflow mypy 错误([\#336](https://github.com/ros2/sros2/issues/336))

- 给予更多的时间,在测试中制定政策([\#323](https://github.com/ros2/sros2/issues/323))

- 切换到上下文管理器进行rclpy测试 。 ()[\#322](https://github.com/ros2/sros2/issues/322))

- \[FIX\] 删除生成_artifacts中危险的可变默认参数([\#318](https://github.com/ros2/sros2/issues/318))

- 在 Windows 调试上修复sros2 测试 。 ()[\#317](https://github.com/ros2/sros2/issues/317))

- \[TESTS\]更新测试并添加生成_艺术的测试([\#311](https://github.com/ros2/sros2/issues/311))

- 删除已贬值的创建 \_ key 和列表 \_ keys 动词([\#302](https://github.com/ros2/sros2/issues/302))

- 修复 linux 教程: 克隆实例政策和节点默认政策集( E)[\#295](https://github.com/ros2/sros2/issues/295))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、米卡埃尔·阿尔盖达斯、藤田富摩亚、雅敦德

<span id="test-cli"></span>

## [test_cli](https://github.com/ros2/system_tests/tree/kilted/test_cli/CHANGELOG.rst)

- 停止使用 python\_ cmake_模块. ()[\#536](https://github.com/ros2/system_tests/issues/536))

- 使用 rclpy.init 上下文管理器到我们可以的地方 。 ([\#547](https://github.com/ros2/system_tests/issues/547)这让我们可以进行清理,同时使用更少的代码来尝试追踪.

- 撰稿人:克里斯·拉兰谢特

<span id="test-cli-remapping"></span>

## [test_cli_remapping](https://github.com/ros2/system_tests/tree/kilted/test_cli_remapping/CHANGELOG.rst)

- 停止使用 python\_ cmake_模块. ()[\#536](https://github.com/ros2/system_tests/issues/536))

- 使用 rclpy.init 上下文管理器到我们可以的地方 。 ([\#547](https://github.com/ros2/system_tests/issues/547)这让我们可以进行清理,同时使用更少的代码来尝试追踪.

- 撰稿人:克里斯·拉兰谢特

<span id="test-communication"></span>

## [test_communication](https://github.com/ros2/system_tests/tree/kilted/test_communication/CHANGELOG.rst)

- 在发射测试中使用 EullRmwIsolation ()[\#571](https://github.com/ros2/system_tests/issues/571))

- 切换到孤立的测试固定宏( S)[\#571](https://github.com/ros2/system_tests/issues/571))

- 添加密钥类型的测试( E)[\#568](https://github.com/ros2/system_tests/issues/568))

- 删除使用ament_target_依赖性([\#566](https://github.com/ros2/system_tests/issues/566))

- 与 Zenoh 一起跳过所有多版本的 pub/ sub 测试([\#560](https://github.com/ros2/system_tests/issues/560))

- 停止使用 python\_ cmake_模块. ()[\#536](https://github.com/ros2/system_tests/issues/536))

- 使用 rclpy.init 上下文管理器到我们可以的地方 。 ([\#547](https://github.com/ros2/system_tests/issues/547)这让我们可以进行清理,同时使用更少的代码来尝试追踪.

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、弗朗西斯科·加莱戈·萨利多、斯科特·K·洛根、谢恩·洛雷茨、雅敦恩德

<span id="test-interface-files"></span>

## [test_interface_files](https://github.com/ros2/test_interface_files/tree/kilted/CHANGELOG.rst)

- 从IDL丢掉长双倍([\#22](https://github.com/ros2/test_interface_files/issues/22))

- 撰稿人:克里斯·拉兰谢特

<span id="test-launch-ros"></span>

## [test_launch_ros](https://github.com/ros2/launch_ros/tree/kilted/test_launch_ros/CHANGELOG.rst)

- 添加 python3-pytest-time out 到 test_launch_ros. (中文(简体) ).[\#454](https://github.com/ros2/launch_ros/issues/454))

- 自动启动生命周期节点和实例启动文件演示( E)[\#430](https://github.com/ros2/launch_ros/issues/430))

- 在 ment_python 包中添加 ament_xmllint 。 ([\#423](https://github.com/ros2/launch_ros/issues/423))

- 添加到测试\_ launch\_ ros 的超时中 。 ([\#417](https://github.com/ros2/launch_ros/issues/417))

- 在设置中修正url.py([\#413](https://github.com/ros2/launch_ros/issues/413))

- 修改测试_load_composable_节点测试. ().[\#403](https://github.com/ros2/launch_ros/issues/403))

- 切换到使用 rclpy.init 上下文管理器 。 ()[\#402](https://github.com/ros2/launch_ros/issues/402))

- 贡献者:克里斯·拉兰塞特、史蒂夫·马肯斯基、藤田友也、魏HU

<span id="test-msgs"></span>

## [test_msgs](https://github.com/ros2/rcl_interfaces/tree/kilted/test_msgs/CHANGELOG.rst)

- 添加带有密钥的测试消息( E)[\#173](https://github.com/ros2/rcl_interfaces/issues/173))

- 贡献者:弗朗西斯科·加莱戈·萨利多

<span id="test-osrf-testing-tools-cpp"></span>

## [test_osrf_testing_tools_cpp](https://github.com/osrf/osrf_testing_tools_cpp/tree/kilted/test_osrf_testing_tools_cpp/CHANGELOG.rst)

- 更新 CMakeLists.txt (英语).[\#85](https://github.com/osrf/osrf_testing_tools_cpp/issues/85))

- 贡献者:苔藓80

<span id="test-quality-of-service"></span>

## [test_quality_of_service](https://github.com/ros2/system_tests/tree/kilted/test_quality_of_service/CHANGELOG.rst)

- 切换到孤立的测试固定宏( S)[\#571](https://github.com/ros2/system_tests/issues/571))

- 使用rmw_event_type_is_支持跳过测试([\#563](https://github.com/ros2/system_tests/issues/563))

- 与Zenoh相关的一些qos测试[\#551](https://github.com/ros2/system_tests/issues/551))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、斯科特·K·洛根

<span id="test-rclcpp"></span>

## [test_rclcpp](https://github.com/ros2/system_tests/tree/kilted/test_rclcpp/CHANGELOG.rst)

- 在发射测试中使用 EullRmwIsolation ()[\#571](https://github.com/ros2/system_tests/issues/571))

- 确保测试核实所有产卵节点的存在([\#558](https://github.com/ros2/system_tests/issues/558))

- chore:通过 Rclcpp 的 API 更改([\#556](https://github.com/ros2/system_tests/issues/556))

- 在等待类中执行纯虚拟功能。 ()[\#548](https://github.com/ros2/system_tests/issues/548))

- 更新折旧方法[\#546](https://github.com/ros2/system_tests/issues/546))

- 撰稿人:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、亚诺施·麦克豪温斯基、斯科特·K·洛根、袁远雄

<span id="test-rmw-implementation"></span>

## [test_rmw_implementation](https://github.com/ros2/rmw_implementation/tree/kilted/test_rmw_implementation/CHANGELOG.rst)

- 添加的 rmw\_ event\_ type\_ is\_ 支持([\#250](https://github.com/ros2/rmw_implementation/issues/250)) \* 添加 rmw_event_check_compatible \* 修补返回类型 \* 更新名称并在 wait_set 测试中使用 {}

- 更新测试的预期,使其与非DDS中间软件保持兼容性([\#248](https://github.com/ros2/rmw_implementation/issues/248))

- 使用 rmw_enclave_options_xxxx APIs 代替 。 ([\#247](https://github.com/ros2/rmw_implementation/issues/247))

- 弥补一些重叠错误。 ([\#246](https://github.com/ros2/rmw_implementation/issues/246)) , 即, 确保清除我们应该清除的错误。 我们还在不支持的API周围略微重写一些测试, 这样它们才更有意义 。

- 请不要为rmw ⁇ pullish, return ⁇ loaned_message \* () 读取 msg ptr () 。[\#240](https://github.com/ros2/rmw_implementation/issues/240))

- 删除 rmw_localhost_仅限_t. ([\#239](https://github.com/ros2/rmw_implementation/issues/239))

- 期望 rmw\_ service_server\_ is\_ 可用以 ret RMW_RET_INVALID_ArgUMENT (中文(简体) ).[\#231](https://github.com/ros2/rmw_implementation/issues/231))

- 期待 rmw\_ destroy\_ wait\_ set 以重排 RMW_RET_INVALID_ArgUMENT (英语:[\#234](https://github.com/ros2/rmw_implementation/issues/234))

- 添加创建两个内容过滤话题的测试, 主题名称相同( Name[\#230](https://github.com/ros2/rmw_implementation/issues/230)) ([\#233](https://github.com/ros2/rmw_implementation/issues/233))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、克里斯·拉兰谢特、克里斯托弗·贝达德、藤田丰也、雅敦德

<span id="test-ros2trace"></span>

## [test_ros2trace](https://github.com/ros2/ros2_tracing/tree/kilted/test_ros2trace/CHANGELOG.rst)

- 在 Stdout 上等待的测试中添加超时( R)[\#167](https://github.com/ros2/ros2_tracing/issues/167))

- 允许启用调用 `ros2 trace` 或跟踪动作( E)[\#137](https://github.com/ros2/ros2_tracing/issues/137))

- 贡献者:克里斯托弗·贝达德

<span id="test-tf2"></span>

## [test_tf2](https://github.com/ros2/geometry2/tree/kilted/test_tf2/CHANGELOG.rst)

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 添加 `rclcpp::shutdown` ([\#762](https://github.com/ros2/geometry2/issues/762))

- 删除许多从矩阵3x3到Quaternion的额外转换([\#741](https://github.com/ros2/geometry2/issues/741)共同作者:jmachowinski \<[jmachowinski@users.noreply.github.com](mailto:jmachowinski%40users.noreply.github.com)\> 联合作者:凯瑟琳·斯科特 \<[katherineAScott@gmail.com](mailto:katherineAScott%40gmail.com)\>

- 折旧 C 信头( E)[\#720](https://github.com/ros2/geometry2/issues/720))

- 切换为 python 示例使用上下文管理器 。 ()[\#700](https://github.com/ros2/geometry2/issues/700)那样我们就能确保永远清理干净,但使用更少的代码这样做.

- 贡献者:克里斯·拉兰塞特,卢卡斯·温德兰,于远元,凯尔-巴斯斯,苔藓80

<span id="test-tracetools"></span>

## [test_tracetools](https://github.com/ros2/ros2_tracing/tree/kilted/test_tracetools/CHANGELOG.rst)

- 对 rmw_zenoh_cpp 进行测试_跟踪工具([\#140](https://github.com/ros2/ros2_tracing/issues/140))

- 在 test\_ tracktools 中使用 ament_add_ros_isolated_X( 测试工具)[\#159](https://github.com/ros2/ros2_tracing/issues/159))

- 仪器客户端/端对端请求/答复跟踪服务([\#145](https://github.com/ros2/ros2_tracing/issues/145))

- 不要试图建立在 BSD 之上([\#142](https://github.com/ros2/ros2_tracing/issues/142)CMake 3.25中添加了“BSD”变量。 请注意,没有定义的变量会被评估为“假 ” , 因此,这不应该在使用CMake 版本超过3.25的平台后退。

- 将测试服务重构和分割为测试服务 , 客户端 } ([\#144](https://github.com/ros2/ros2_tracing/issues/144))

- 将预期的 rmw GID 数组大小更改为 16 字节( M)[\#138](https://github.com/ros2/ros2_tracing/issues/138))

- 对 rmw_fastrtps\_ 动力学\_ cpp 进行测试_跟踪工具( )[\#127](https://github.com/ros2/ros2_tracing/issues/127))

- 制作测试工具 ping pubs/ subs transient\_ local([\#125](https://github.com/ros2/ros2_tracing/issues/125))

- 使用所有仪器的 rmw impls () 运行相关的测试工具\_ tracetools 测试([\#116](https://github.com/ros2/ros2_tracing/issues/116))

- 贡献者:克里斯托弗·贝达德、斯科特·K·洛根

<span id="test-tracetools-launch"></span>

## [test_tracetools_launch](https://github.com/ros2/ros2_tracing/tree/kilted/test_tracetools_launch/CHANGELOG.rst)

- 解决或忽略新的神秘问题([\#161](https://github.com/ros2/ros2_tracing/issues/161))

- 允许启用调用 `ros2 trace` 或跟踪动作( E)[\#137](https://github.com/ros2/ros2_tracing/issues/137))

- 贡献者:克里斯托弗·贝达德

<span id="tf2"></span>

## [tf2](https://github.com/ros2/geometry2/tree/kilted/tf2/CHANGELOG.rst)

- 添加isnan 支持( R)[\#780](https://github.com/ros2/geometry2/issues/780))

- 从Sec () 处理极端大值或小值时的函数( E) 时间跨度问题( E)[\#785](https://github.com/ros2/geometry2/issues/785))

- 在取消待变换请求时, 请不要弹出回调控( N)[\#779](https://github.com/ros2/geometry2/issues/779))

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 删除许多从矩阵3x3到Quaternion的额外转换([\#741](https://github.com/ros2/geometry2/issues/741)共同作者:jmachowinski \<[jmachowinski@users.noreply.github.com](mailto:jmachowinski%40users.noreply.github.com)\> 联合作者:凯瑟琳·斯科特 \<[katherineAScott@gmail.com](mailto:katherineAScott%40gmail.com)\>

- 清理折旧警告。 ([\#744](https://github.com/ros2/geometry2/issues/744)贬值警告至少没有在海合会上正确打印;它会警告#警告不标准,也不会打印出实际警告。 此外,“贬值”的拼写也是错误的。 将所有这些问题在这里解决。

- 折旧 C 信头( E)[\#720](https://github.com/ros2/geometry2/issues/720))

- 已删除 tf2 中的未使用 var ([\#735](https://github.com/ros2/geometry2/issues/735))

- 填充错误字符串( E)[\#715](https://github.com/ros2/geometry2//issues/715))

- 删除已贬值的enuns (% 1)[\#699](https://github.com/ros2/geometry2//issues/699))

- \[TimeCache\] 改进插入Data()和pruneList()的性能([\#680](https://github.com/ros2/geometry2/issues/680))合著:克里斯·拉朗谢特 \<[clalancette@gmail.com](mailto:clalancette%40gmail.com)\>

- 删除警告( R)[\#682](https://github.com/ros2/geometry2/issues/682))

- 添加缓存\_ 基准[\#679](https://github.com/ros2/geometry2/issues/679)) \* 添加缓存\_ 基准标记 共同作者: Chris Lalancette \<[clalancette@gmail.com](mailto:clalancette%40gmail.com)\>

- \[cache_unittest\] 在定购,普鲁士时添加直接执行测试([\#678](https://github.com/ros2/geometry2/issues/678)) \* \[cache_unittest\] 在订购时添加直接执行测试, prun \* do get All Projects () 方法 \* 返回引用 。 \* 标记 get All Projects 作为内部 。 由 Chris Lalancette 共同撰写 。 \<[clalancette@gmail.com](mailto:clalancette%40gmail.com)\>

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,克里斯·拉兰谢特,埃里克·库桑索,卢卡斯·温德兰,迈克尔·卡尔斯特罗姆,蒂莫·罗赫林,克拉姆克,凯尔-巴斯,苔藓80

<span id="tf2-bullet"></span>

## [tf2_bullet](https://github.com/ros2/geometry2/tree/kilted/tf2_bullet/CHANGELOG.rst)

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 折旧 C 信头( E)[\#720](https://github.com/ros2/geometry2/issues/720))

- 贡献者:卢卡斯·温德兰、苔藓80

<span id="tf2-eigen"></span>

## [tf2_eigen](https://github.com/ros2/geometry2/tree/kilted/tf2_eigen/CHANGELOG.rst)

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 折旧 C 信头( E)[\#720](https://github.com/ros2/geometry2/issues/720))

- 贡献者:卢卡斯·温德兰、苔藓80

<span id="tf2-eigen-kdl"></span>

## [tf2_eigen_kdl](https://github.com/ros2/geometry2/tree/kilted/tf2_eigen_kdl/CHANGELOG.rst)

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 折旧 C 信头( E)[\#720](https://github.com/ros2/geometry2/issues/720))

- 贡献者:卢卡斯·温德兰、苔藓80

<span id="tf2-geometry-msgs"></span>

## [tf2_geometry_msgs](https://github.com/ros2/geometry2/tree/kilted/tf2_geometry_msgs/CHANGELOG.rst)

- 删除许多从矩阵3x3到Quaternion的额外转换([\#741](https://github.com/ros2/geometry2/issues/741)共同作者:jmachowinski \<[jmachowinski@users.noreply.github.com](mailto:jmachowinski%40users.noreply.github.com)\> 联合作者:凯瑟琳·斯科特 \<[katherineAScott@gmail.com](mailto:katherineAScott%40gmail.com)\>

- 折旧 C 信头( E)[\#720](https://github.com/ros2/geometry2/issues/720))

- 将 python3-dev 依赖性添加到 tf2_py. ().[\#733](https://github.com/ros2/geometry2/issues/733))

- 修复 tf2\_ 几何\_ msgs\_ INCLUDE\_ DIRS. (中文(简体) ).[\#729](https://github.com/ros2/geometry2/issues/729))

- 删除 python_cmake_模块的使用( Name[\#651](https://github.com/ros2/geometry2//issues/651))

- 贡献者:克里斯·拉伦塞特、卢卡斯·温德兰、凯尔-基斯、树轮栽培

<span id="tf2-kdl"></span>

## [tf2_kdl](https://github.com/ros2/geometry2/tree/kilted/tf2_kdl/CHANGELOG.rst)

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 修复外部文件映射( R)[\#757](https://github.com/ros2/geometry2/issues/757))

- tf2_kdl: 添加 python_orocos_kdl_vendor 依赖性([\#745](https://github.com/ros2/geometry2/issues/745)) \* tf2_kdl: 添加 python_rorocos_kdl_vendor 依赖 Tf2_kdl Python API 依赖 PyKDL, 由 python_rorocos_kdl_vendor 提供. \* tf2_kdl: 移除 tf2_msgs 测试依赖 此依赖是不需要的.

- 折旧 C 信头( E)[\#720](https://github.com/ros2/geometry2/issues/720))

- 贡献者:本·沃尔西弗,埃马纽埃尔,卢卡斯·温德兰,苔丝菲特80

<span id="tf2-msgs"></span>

## [tf2_msgs](https://github.com/ros2/geometry2/tree/kilted/tf2_msgs/CHANGELOG.rst)

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 贡献者:苔藓80

<span id="tf2-py"></span>

## [tf2_py](https://github.com/ros2/geometry2/tree/kilted/tf2_py/CHANGELOG.rst)

- 折旧 C 信头( E)[\#720](https://github.com/ros2/geometry2/issues/720))

- 将 python3-dev 依赖性添加到 tf2_py. ().[\#733](https://github.com/ros2/geometry2/issues/733))

- 删除 python_cmake_模块的使用( Name[\#651](https://github.com/ros2/geometry2//issues/651))

- 撰稿人:克里斯·拉兰谢特、卢卡斯·温德兰

<span id="tf2-ros"></span>

## [tf2_ros](https://github.com/ros2/geometry2/tree/kilted/tf2_ros/CHANGELOG.rst)

- 统一 CMake min 版本 ([\#764](https://github.com/ros2/geometry2/issues/764))

- 添加 `rclcpp::shutdown` ([\#762](https://github.com/ros2/geometry2/issues/762))

- 修复外部文件映射( R)[\#757](https://github.com/ros2/geometry2/issues/757))

- 折旧 C 信头( E)[\#720](https://github.com/ros2/geometry2/issues/720))

- 指定四角线顺序为 xyzw ([\#718](https://github.com/ros2/geometry2/issues/718))

- 添加可配置的 TF 主题( O)[\#709](https://github.com/ros2/geometry2//issues/709))

- 添加静态变压器( N)[\#673](https://github.com/ros2/geometry2/issues/673))

- 已更新的已折旧信件过滤信头( E)[\#702](https://github.com/ros2/geometry2/issues/702))

- 更新 qos 以进行折旧[\#695](https://github.com/ros2/geometry2/issues/695))

- Cli 工具文档[\#653](https://github.com/ros2/geometry2/issues/653))

- 贡献者:阿比希谢克·卡希亚普,亚历杭德罗·埃尔南德斯·科尔德罗,埃马纽埃尔,卢卡斯·温德兰,瑞安,汤姆·摩尔,尤尤安·袁,摩斯费特80

<span id="tf2-ros-py"></span>

## [tf2_ros_py](https://github.com/ros2/geometry2/tree/kilted/tf2_ros_py/CHANGELOG.rst)

- 修复外部文件映射( R)[\#757](https://github.com/ros2/geometry2/issues/757))

- 在linters中添加 tf2_ros_py. ()[\#740](https://github.com/ros2/geometry2/issues/740))

- 在 Python 中添加静态变形听器( S)[\#719](https://github.com/ros2/geometry2/issues/719))

- 在测试_xmllint中添加几何2 python 包。 ([\#725](https://github.com/ros2/geometry2/issues/725))

- 添加可配置的 TF 主题( O)[\#709](https://github.com/ros2/geometry2//issues/709))

- 修正时间_跳_召回签名 。 ([\#711](https://github.com/ros2/geometry2//issues/711))

- 切换为 python 示例使用上下文管理器 。 ()[\#700](https://github.com/ros2/geometry2/issues/700)那样我们就能确保永远清理干净,但使用更少的代码这样做.

- 贡献者:克里斯·拉兰谢特、埃马纽埃尔、卢卡斯·温德兰、瑞安

<span id="tf2-sensor-msgs"></span>

## [tf2_sensor_msgs](https://github.com/ros2/geometry2/tree/kilted/tf2_sensor_msgs/CHANGELOG.rst)

- 折旧 C 信头( E)[\#720](https://github.com/ros2/geometry2/issues/720))

- 将 python3-dev 依赖性添加到 tf2_py. ().[\#733](https://github.com/ros2/geometry2/issues/733))

- 删除 python_cmake_模块的使用( Name[\#651](https://github.com/ros2/geometry2//issues/651))

- 撰稿人:克里斯·拉兰谢特、卢卡斯·温德兰

<span id="tf2-tools"></span>

## [tf2_tools](https://github.com/ros2/geometry2/tree/kilted/tf2_tools/CHANGELOG.rst)

- 在测试_xmllint中添加几何2 python 包。 ([\#725](https://github.com/ros2/geometry2/issues/725))

- 添加可配置的 TF 主题( O)[\#709](https://github.com/ros2/geometry2//issues/709))

- \[view_frames\] 日志文件名确定后([\#674](https://github.com/ros2/geometry2/issues/674))

- 贡献者:克里斯·拉兰谢特、米卡埃尔·阿格达斯、瑞安

<span id="tlsf"></span>

## [tlsf 转换为](https://github.com/ros2/tlsf/tree/kilted/tlsf/CHANGELOG.rst)

- 固定链接( O)[\#15](https://github.com/ros2/tlsf/issues/15))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗

<span id="tlsf-cpp"></span>

## [tlsf_cpp](https://github.com/ros2/realtime_support/tree/kilted/tlsf_cpp/CHANGELOG.rst)

- 在测试退出前明确关闭上下文([\#129](https://github.com/ros2/realtime_support/issues/129))

- 减少我们编译的文件数量 。 ([\#125](https://github.com/ros2/realtime_support/issues/125))

- 贡献者:克里斯·拉兰塞特、雅敦德

<span id="topic-monitor"></span>

## [topic_monitor](https://github.com/ros2/demos/tree/kilted/topic_monitor/CHANGELOG.rst)

- 在所有的 Ament_python 包中添加 test_xmllint.py 。 ([\#704](https://github.com/ros2/demos/issues/704))

- 撰稿人:克里斯·拉兰谢特

<span id="topic-statistics-demo"></span>

## [topic_statistics_demo](https://github.com/ros2/demos/tree/kilted/topic_statistics_demo/CHANGELOG.rst)

- 统一计算机计算分钟核查器[\#714](https://github.com/ros2/demos/issues/714))

- 贡献者:苔藓80

<span id="tracetools"></span>

## [跟踪工具](https://github.com/ros2/ros2_tracing/tree/kilted/tracetools/CHANGELOG.rst)

- 切换到 ament_cmake_ros_core 软件包([\#162](https://github.com/ros2/ros2_tracing/issues/162))

- 仪器客户端/端对端请求/答复跟踪服务([\#145](https://github.com/ros2/ros2_tracing/issues/145))

- 不要试图建立在 BSD 之上([\#142](https://github.com/ros2/ros2_tracing/issues/142))

- 将预期的 rmw GID 数组大小更改为 16 字节( M)[\#138](https://github.com/ros2/ros2_tracing/issues/138))

- 解决两个不同的C问题。 ([\#129](https://github.com/ros2/ros2_tracing/issues/129))

- 忽略来自 lttng-ust 宏的零变量- 宏参数警告( )[\#126](https://github.com/ros2/ros2_tracing/issues/126))

- 删除已贬值的TRACEPOINT宏([\#123](https://github.com/ros2/ros2_tracing/issues/123))

- 在痕量点事件声明中为缓冲索引参数定义类型 。 ()[\#117](https://github.com/ros2/ros2_tracing/issues/117))

- 贡献者:克里斯·拉朗谢特、克里斯托弗·贝达德、马蒂斯·基弗、迈克尔·卡罗尔、斯科特·K·洛根

<span id="tracetools-launch"></span>

## [tracetools_launch](https://github.com/ros2/ros2_tracing/tree/kilted/tracetools_launch/CHANGELOG.rst)

- 解决或忽略新的神秘问题([\#161](https://github.com/ros2/ros2_tracing/issues/161))

- 改进 Python 打字说明( E)[\#152](https://github.com/ros2/ros2_tracing/issues/152))

- 用于追踪工具的展览类型([\#153](https://github.com/ros2/ros2_tracing/issues/153))

- 允许启用调用 `ros2 trace` 或跟踪动作( E)[\#137](https://github.com/ros2/ros2_tracing/issues/137))

- 贡献者:克里斯托弗·贝达德、迈克尔·卡尔斯特罗姆

<span id="tracetools-read"></span>

## [tracetools_read](https://github.com/ros2/ros2_tracing/tree/kilted/tracetools_read/CHANGELOG.rst)

- 改进 Python 打字说明( E)[\#152](https://github.com/ros2/ros2_tracing/issues/152))

- 用于追踪工具的展览类型([\#153](https://github.com/ros2/ros2_tracing/issues/153))

- 贡献者:克里斯托弗·贝达德、迈克尔·卡尔斯特罗姆

<span id="tracetools-test"></span>

## [tracetools_test](https://github.com/ros2/ros2_tracing/tree/kilted/tracetools_test/CHANGELOG.rst)

- 解决或忽略新的神秘问题([\#161](https://github.com/ros2/ros2_tracing/issues/161))

- 改进 Python 打字说明( E)[\#152](https://github.com/ros2/ros2_tracing/issues/152))

- 用于追踪工具的展览类型([\#153](https://github.com/ros2/ros2_tracing/issues/153))

- 使用所有仪器的 rmw impls () 运行相关的测试工具\_ tracetools 测试([\#116](https://github.com/ros2/ros2_tracing/issues/116))

- 贡献者:克里斯托弗·贝达德、迈克尔·卡尔斯特罗姆

<span id="tracetools-trace"></span>

## [tracetools_trace](https://github.com/ros2/ros2_tracing/tree/kilted/tracetools_trace/CHANGELOG.rst)

- 改进 Python 打字说明( E)[\#152](https://github.com/ros2/ros2_tracing/issues/152))

- 用于追踪工具的展览类型([\#153](https://github.com/ros2/ros2_tracing/issues/153))

- 删除 tracktools_trace中不必要的“类型:忽略”注释([\#151](https://github.com/ros2/ros2_tracing/issues/151))

- 仪器客户端/端对端请求/答复跟踪服务([\#145](https://github.com/ros2/ros2_tracing/issues/145))

- 允许启用调用 `ros2 trace` 或跟踪动作( E)[\#137](https://github.com/ros2/ros2_tracing/issues/137))

- 贡献者:克里斯托弗·贝达德、迈克尔·卡尔斯特罗姆

<span id="turtlesim"></span>

## [乌龟](https://github.com/ros/ros_tutorials/tree/kilted/turtlesim/CHANGELOG.rst)

- 创建龟兹im\_ msgs ()[\#169](https://github.com/ros/ros_tutorials/issues/169))

- 为 Jazzy 添加图标 。 ()[\#167](https://github.com/ros/ros_tutorials/issues/167))

- \[teleop_turtle_key\] 更新用法字符串,以匹配键盘抓取的密钥([\#165](https://github.com/ros/ros_tutorials/issues/165))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗、马科·古铁雷斯、米卡埃尔·阿尔盖达斯

<span id="turtlesim-msgs"></span>

## [turtlesim_msgs](https://github.com/ros/ros_tutorials/tree/kilted/turtlesim_msgs/CHANGELOG.rst)

- 创建龟兹im\_ msgs ()[\#169](https://github.com/ros/ros_tutorials/issues/169))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗

<span id="type-description-interfaces"></span>

## [type_description_interfaces](https://github.com/ros2/rcl_interfaces/tree/kilted/type_description_interfaces/CHANGELOG.rst)

- 添加缺失的构建\_ 导出\_ 依赖 rosidl\_ core_runtime ()[\#165](https://github.com/ros2/rcl_interfaces/issues/165))

- 撰稿人:斯科特·K·洛根

<span id="unique-identifier-msgs"></span>

## [unique_identifier_msgs](https://github.com/ros2/unique_identifier_msgs/tree/kilted/CHANGELOG.rst)

- 添加缺失的构建\_ 导出\_ 依赖 rosidl\_ core_runtime ()[\#30](https://github.com/ros2/unique_identifier_msgs/issues/30))

- 撰稿人:斯科特·K·洛根

<span id="urdf"></span>

## [乌尔德夫](https://github.com/ros2/urdf/tree/kilted/urdf/CHANGELOG.rst)

- 使木工欢喜,[\#45](https://github.com/ros2/urdf/issues/45))

- 以 rosdoc2 添加的文档([\#40](https://github.com/ros2/urdf/issues/40))

- 添加逗号 linters ([\#39](https://github.com/ros2/urdf/issues/39))

- 使用 rcutils 来日志 ([\#37](https://github.com/ros2/urdf/issues/37))

- 启用测试_robot_model_parser 测试 ([\#38](https://github.com/ros2/urdf/issues/38))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗

<span id="urdf-parser-plugin"></span>

## [urdf_parser_plugin](https://github.com/ros2/urdf/tree/kilted/urdf_parser_plugin/CHANGELOG.rst)

- 添加逗号 linters ([\#39](https://github.com/ros2/urdf/issues/39))

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗

<span id="zenoh-cpp-vendor"></span>

## [zenoh_cpp_vendor](https://github.com/ros2/rmw_zenoh/tree/kilted/zenoh_cpp_vendor/CHANGELOG.rst)

- Bump Zenoh to v1.3.2, 并用 HeartbeatSporadic 提高e2e的可靠性([\#591](https://github.com/ros2/rmw_zenoh/issues/591))

- 添加质量声明( E)[\#483](https://github.com/ros2/rmw_zenoh/issues/483))

- 以调试模式修正活性崩溃( F)[\#544](https://github.com/ros2/rmw_zenoh/issues/544))

- bump zenoh-cpp to 2a127bb, zenoh-c to 3540a3c, zenoh to f735bf5 (中文(简体) ).[\#503](https://github.com/ros2/rmw_zenoh/issues/503))

- 启用 Zenoh UDP 传输 (S)[\#486](https://github.com/ros2/rmw_zenoh/issues/486))

- bump zenoh-c to 261493 and zenoh-cpp to 5dfb68c (英语:[\#463](https://github.com/ros2/rmw_zenoh/issues/463))

- Bump Zenoh以3bbf6af(1.2.1+少数犯罪)([\#456](https://github.com/ros2/rmw_zenoh/issues/456))

- Bump Zenoh 犯罪 id e4ea6f0 (1.2.0 + 少数犯罪)[\#446](https://github.com/ros2/rmw_zenoh/issues/446))

- 双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双双[\#424](https://github.com/ros2/rmw_zenoh/issues/424))

- 更新 Zenoh 版本 ([\#405](https://github.com/ros2/rmw_zenoh/issues/405))

- 销售商 Zenoh-cpp for rmw_zenoh. 中国植物物种信息数据库.

- 贡献者:亚历杭德罗·埃尔南德斯·科尔德罗,陈英国(共青团),克里斯·拉兰谢特,佛朗哥·西波隆,胡加勒31,朱琳·艾诺,卢卡·科米纳迪,亚敦恩德,于远元.

<span id="zenoh-security-tools"></span>

## [zenoh_security_tools](https://github.com/ros2/rmw_zenoh/tree/kilted/zenoh_security_tools/CHANGELOG.rst)

- 添加 zenoh_security_tools ([\#595](https://github.com/ros2/rmw_zenoh/issues/595))

- 贡献者:Yadundund
