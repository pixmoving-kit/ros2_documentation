---
translation_status: machine_translated
source: Tutorials/Miscellaneous/Eclipse-Oxygen-with-ROS-2-and-rviz2.rst
---

<span id="using-eclipse-oxygen-with-rviz2-community-contributed"></span>

# 使用 Eclipse 氧气 `rviz2` \[社区贡献\]

<span id="setup"></span>

## 设置

这个教程假设Eclipse Oxygen, git, 以及 [爱吉特 Egit](http://www.eclipse.org/egit/download/) 已经安装。

在整个教程中,我们将日食工作空间命名为与 ros2 软件包相同的名称,但这不需要.

HINT:每个ROS-2软件包都使用嵌套项目和一个Eclipse工作空间.

![](images/eclipse-oxygen-01.png)

创建一个 C++ 工程 。

![](images/eclipse-oxygen-02.png) ![](images/eclipse-oxygen-03.png)

选择 ROS 2 软件包名称为工程名称。 选择 Makefile 工程和其他工具链 。

![](images/eclipse-oxygen-04.png)

点击完成

![](images/eclipse-oxygen-05.png)

我们的项目应在“项目探索者”中显示。

![](images/eclipse-oxygen-06.png)

在我们的项目中创建一个名为“src”的文件夹。

![](images/eclipse-oxygen-07.png)

导入 git 仓库 。

![](images/eclipse-oxygen-08.png)

输入寄存器 URL 。

![](images/eclipse-oxygen-09.png)

ImportANT: 使用我们之前创建的项目的源文件夹作为目的文件夹.

HINT: 如果您在选择目的文件夹路径时遇到问题, Eclipse Dialog 需要名称字段的名称 。

![](images/eclipse-oxygen-10.png)

使用新工程向导导入 。

![](images/eclipse-oxygen-11.png)

创建 General- \> 工程。

![](images/eclipse-oxygen-12.png)

使用 git 寄存器名称作为工程名称 。 ImportANT : 使用我们克隆的 git 寄存器作为“ 地址 ” 。

![](images/eclipse-oxygen-13.png)

git工程和新工程应该在Project Explorer视图中可见,同样的文件被多次列出,但只有一个工程与Egit相连.

![](images/eclipse-oxygen-14.png)

重复此程序。 导入 git 仓库插件 。

![](images/eclipse-oxygen-15.png)

重要: 使用源文件夹中的文件夹作为“ Destination- \> Directory” 。

![](images/eclipse-oxygen-16.png)

ImportANT: 使用我们克隆的 git 寄存器的文件夹作为新项目的位置 。

![](images/eclipse-oxygen-17.png)

用 tinyxml2_vendor git 仓库运行同样的程序 。

![](images/eclipse-oxygen-18.png)

ImportANT:再次在源文件夹内使用一个文件夹.

![](images/eclipse-oxygen-19.png)

ImportANT: 使用我们克隆的文件夹的位置作为新项目文件夹 。

![](images/eclipse-oxygen-20.png)

现在所有四个项目都应该在项目探索者视图中可见.

![](images/eclipse-oxygen-21.png)

点击右上角的“ 项目探索器”视图, 就可以将“ 项目演示文稿” 更改为“ 等级” 视图。 现在它看起来像硬盘上的 ROS-2 项目。 但是这个视图失去了与 Egit 的链接, 所以使用“ 平面工程演示文稿 ” 。 如果您想看到例如哪个作者写了哪条代码线等, Egit 链接是好的 。

![](images/eclipse-oxygen-22.png)

转到“C/C++ 构建”部分,并将“增强”插入“构建命令”。

![](images/eclipse-oxygen-23.png)

转到“ 行为” 标签并取消选择“ 干净” , 将“ 建设” 输入“ 构建” 文本框 。

![](images/eclipse-oxygen-24.png)

在“ 构建工程” 工作之前, 我们需要关闭 Eclipse 。 打开一个 ROS-2 设置. bash 文件并源代码, 然后将 cd 输入日食工程的目录( 这里: / home/ ubu/rviz2\_ ws/rviz2\_ ws) , 并在此目录内启动 Eclipse 。

![](images/eclipse-oxygen-25.png)

现在代码补全,egit说明,日食C/C++工具等应该都行得通.

![](images/eclipse-oxygen-26.png) <span id="eclipse-indexer"></span>

## Eclipse 索引器

打开 rviz2 主. cpp 可能显示许多“ 未解析的包含” 警告。 要修复此选项, 请访问 Project- \>Propertys- \> C++ General- \> Path 和 符号 。 点击“ references” 标签并选择“ ros2\_ ws ” 。

![](images/eclipse-oxygen-27.png)

转到 C/C++- General-\>Path-and-Symbols,点击“来源位置”标签并点击“链接文件夹”。选择 qt5 包括的位置。

![](images/eclipse-oxygen-28.png)

下一个图像应该显示。在源位置上添加排除,这样一些目录(如“构建”和“安装 ” ) 就不会被索引,这是一个好主意。

![](images/eclipse-oxygen-29.png)

转到 C++ General- \> 预处理器包括,选择“编译器设置中构建的CDT GCC\[共享\]”,并在“命令”中输入以下文本框:

``` bash
-std=c++14
```

![](images/eclipse-oxygen-30.png)

转到“ C/C++- General- \> Indexer ” 在图像中选择以下内容。 例如“ 索引未使用的标题作为 c 文件” 来解决QApplication 等, 因为 QApplication 标题的内容仅仅是“ # 包括 qapplication. h ” 。

![](images/eclipse-oxygen-31.png)

运行索引器后( 稍后发生, 这样您也会看到这个 ) , 您可以看到它添加了什么 。

![](images/eclipse-oxygen-32.png)

右键点击 rviz2 工程后, 选择“ Indexer- \> Rebuilt ” , 开始重建索引( 右下角有一个图标显示进度 ) 。 索引完成重建后, 应该能够解决所有内容 。

![](images/eclipse-oxygen-33.png) <span id="debugging-with-eclipse"></span>

## 与日蚀调试

转到“C/C++-构建”并添加到构建命令中:

``` bash
-DCMAKE_BUILD_TYPE=Debug
```

![](images/eclipse-oxygen-34.png)

然后在日蚀中转到“ Run- \> Debug 配置”, 并添加以下内容并点击“ debug ”。

![](images/eclipse-oxygen-35.png)
