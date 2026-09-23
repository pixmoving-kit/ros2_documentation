---
translation_status: machine_translated
source: The-ROS2-Project/Contributing/Contributing-To-ROS-2-Documentation.rst
---

<span id="contributing-to-ros-2-documentation"></span>

# 为 ROS 2 文档作贡献

最欢迎为该网站提供材料。本页解释如何为ROS 2 文件提供材料。请在提供材料之前仔细阅读以下各节。

网站是利用 [狮身人面](https://www.sphinx-doc.org/en/master/),特别是使用 [狮身人面像多版本](https://sphinx-contrib.github.io/multiversion/main/index.html).

<span id="branch-structure"></span>

## 分支结构

文件的源代码位于 [ROS 2 文档 GitHub 仓库](https://github.com/ros2/ros2_documentation)。这个寄存器设置时,每个 ROS 2 分布有一个分支,以处理分布之间的差异。如果一个变化是所有 ROS 2 分布所共有的,则应向 `rolling` 如果改变是针对特定ROS 2 分布的,则应该针对相应的分支。

<span id="source-structure"></span>

## 源结构

该站点的源文件全部位于该站台下方. `source` 子目录。 各种 sphinx 插件的模板位于 `source/_templates`。根目录包含本地建立测试站点所需的配置和文件。

<span id="building-the-site-locally"></span>

## 在当地建造场地

从创建开始 [阴道](https://docs.python.org/3/library/venv.html) 用于构建文档:

``` console
$ python3 -m venv ros2doc  # create venv
$ source ros2doc/bin/activate  # activate venv
```

并安装位于 `requirements.txt` 文件 :

##### Linux

``` console
$ pip install -r requirements.txt -c constraints.txt
```

##### macOS

``` console
$ pip install -r requirements.txt -c constraints.txt
```

##### Windows

``` console
$ python -m pip install -r requirements.txt -c constraints.txt
```

为了让狮身人面像能够生成图表, `dot` 命令必须可用。

##### Linux

``` console
$ sudo apt update ; sudo apt install graphviz
```

##### macOS

``` console
$ brew install graphviz
```

##### Windows

下载一个安装器 [Graphviz 下载页面](https://graphviz.gitlab.io/_pages/Download/Download_windows.html) 并安装它。确定允许安装器添加到 Windows `%PATH%`否则Sphinx将找不到它。

<span id="building-the-site-for-one-branch"></span>

### 建造一个分行的场地

要为这个分支建造网站, 类型 `make html` 在寄存器的顶层。这是测试本地更改的推荐方式。

``` console
$ make html
```

构建过程需要一些时间。要看到输出,请打开 `build/html/index.html` 在您的浏览器中。

<span id="live-reload-local-development"></span>

### 当地发展

在文件上重复时, 而不是重运行 `make html` 并每次编辑、使用后刷新浏览器 [狮身人面像自动构建](https://github.com/sphinx-doc/sphinx-autobuild) 以监视源文件,在保存上逐步重建,并以自动浏览器重新加载的方式为结果服务。

`sphinx-autobuild` 已安装为 `requirements.txt`。启动实时服务器时使用 :

``` console
$ make serve
```

然后打开 `http://localhost:2022` 在浏览器中。

那个... `serve` 目标绑定到 `0.0.0.0:2022` 默认情况下, 服务器可以通过 Devcontainer / 端口转发到达 。 需要的话, 覆盖绑定地址或端口 :

``` console
$ make serve LIVE_HOST=127.0.0.1 LIVE_PORT=8080
```

<span id="checking-testing-the-site"></span>

### 检查/测试网站

您可以在本地运行文档测试( 使用 [文件8](https://github.com/PyCQA/doc8)(a) 命令如下:

``` console
$ make test
```

您可以在本地运行 Python 文档工具测试( 使用 [pyst 测试](https://docs.pytest.org/en/stable/)(a) 命令如下:

``` console
$ make test-tools
```

您可以在本地运行 Python 文档工具测试( 使用 [pyst 测试](https://docs.pytest.org/en/stable/)(a) 命令如下:

``` console
make test-tools
```

您可以在本地运行文档插件( 使用 [狮身人面像林特](https://github.com/sphinx-contrib/sphinx-lint)(a) 命令如下:

``` console
$ make lint
```

您可以在本地运行文档拼写检查器( 使用 [代码pell](https://github.com/codespell-project/codespell)(a) 命令如下:

``` console
$ make spellcheck
```

> **说明**
>
> 如果检测到需要忽略的特定字,请添加到 [codespell_whitelist](https://github.com/ros2/ros2_documentation/blob/rolling/codespell_whitelist.txt) .

要进一步了解拼写检查,请参见: [拼写检查](#spelling-check)

<span id="view-site-through-github-ci"></span>

### 通过 Github CI 查看网站

对于 ROS 2 Docs 的小改动, 您可以使用我们 Github 动作中生成的文物将您的更改看成 HTML 。 “ 构建” 动作将整个 ROS Docs 生成为包含全部 HTML 的可下载的 Zip 文件 。 [维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献](https://docs.ros.org/) 这个构建动作是在通过测试动作和擦拭动作后触发的.

要下载和查看您的更改, 请先点击拉动请求, 在标题下点击“ 检查” 标签。 在检查页左侧, 请点击“ 测试” 对话框下的“ 测试” 部分。 这将打开右边的菜单, 您可以点击“ Upload Document Properts” 并滚动到底部, 在“ Artifact develop URL” 标题下查看 Zippp的 HTML 文件的下载链接 。

[![在 ROS Github 动作上查找已生成的 HTML 文件的步骤](images/github_action.png)](images/github_action.png) <span id="building-the-site-for-all-branches"></span>

### 建造所有分支机构的场地

用于所有分支的站点,类型 `make multiversion` 从 `rolling` 这有两个缺点:

1.  多版本插件不理解如何逐步构建,所以它总是重建一切。这可能会很慢。

2.  输入时 `make multiversion`它会永远检查 准确的分支列表 `conf.py` 文件。这意味着不会显示本地更改。

要显示多版本输出的本地变化, 您必须先将修改输入本地分支。 然后您必须编辑 [conf.py](https://github.com/ros2/ros2_documentation/blob/rolling/conf.py) 文件并更改 `smv_branch_whitelist` 变量以指向您的分支。

<span id="checking-for-broken-links"></span>

### 检查断开的链接

要检查网站中断开的链接, 请运行 :

``` console
$ make linkcheck
```

这将检查整个站点的断开链接,并将结果输出到屏幕和 `build/linkcheck`.

<span id="spelling-check"></span> <span id="id2"></span>

### 拼写检查

那个... `make spellcheck` 命令扫描文档文件,并标记任何错误。如果发现错误,请审查建议,并在必要时更新拉动请求。

有些词语,如技术术语或适当的名词,可能被误标为拼写错误。如果遇到这种情况,可以将它们添加到忽略列表中,以防止它们在未来被标注。要做到这一点,请添加到列表中。 [codespell_whitelist](https://github.com/ros2/ros2_documentation/blob/rolling/codespell_whitelist.txt) 文件如下:

``` text
empy
jupyter
lets
ws
```

包括自定义更正 `codespell` 应用时,您可以将其添加到 [codespell_dictionary](https://github.com/ros2/ros2_documentation/blob/rolling/codespell_dictionary.txt) 文件如下:

``` text
amnet->ament
colcn->colcon
rosabg->rosbag
rosdistroy->rosdistro
```

为了检查字典,你可以运行 `make check-dictionaries` 命令。此选项将检查词典中的空白行和引导/串行空格。如果它抱怨词典,则您可以运行 `make sort-dictionaries` 命令。如果找到任何问题,此命令将自动修改词典。

<span id="migrating-pages-from-the-ros-wiki"></span>

## 从 ROS Wiki 移动页面

迁移页面的第一步 [ROS 维基百科](https://wiki.ros.org) 到 ROS 2 文档将确定是否需要迁移页面。请检查内容或类似内容是否可用。 <https://docs.ros.org/en/rolling> 。如果已经迁移,则恭喜您。如果它还没有迁移,请考虑它是否值得保留。您或其他人认为有用的页面,并经常提及,如果这些页面没有被其他文档所取代,那么这些页面是好候选人。对于ROS项目和特性,如果没有当前分发文件的支持,就不应该迁移。

迁移 ROS Wiki 页面的下一步是确定迁移页面的正确位置。 覆盖 ROS 核心概念的 ROS Wiki 页面只属于 ROS 文档, 这些页面应该迁移到 ROS 文档中的逻辑位置。 软件包特定文档应该迁移到软件包源寄存器中生成的软件包级文档中。 软件包级文档一旦更新, 就会被看到 。 [作为软件包级文档的一部分](https://docs.ros.org/en/rolling/p/)。如果你不确定是否和在何处移动一个页面,请通过一个问题进行联系。 <https://github.com/ros2/ros2_documentation> 或继续 <https://discourse.openrobotics.org/>.

一旦您确定 ROS Wiki 页面值得迁移,并在 ROS 文档中找到合适的登陆点, 迁移过程的下一步就是建立迁移该页面所必需的转换工具。 在大多数情况下, 将单一 ROS Wiki 页面迁移到 ROS Docs 的唯一必要工具是 [泛 Doc 软件](https://pandoc.org/) 命令行工具和文本编辑器。 PanDoc 得到了大多数现代操作系统的支持,使用在他们的网站上发现的安装指令。值得注意的是,ROS Wiki 使用了更古老的维基技术(MoinMoin),所以所使用的标记语言是模糊的方言。 [媒体维基](https://www.mediawiki.org/wiki/Help:Formatting) 我们发现,从 ROS Wiki 中迁移一个页面的最简单的方法就是用 PanDoc 将其从 HTML 转换成 restractured 文本 。

<span id="migrating-a-wiki-file"></span>

### 正在移动 Wiki 文件

1.  克隆合适的寄存器。 如果您正在将一个页面迁移到这里所主控的官方文档中, 那么您应该克隆 <https://github.com/ros2/ros2_documentation>.

2.  为您的迁移页面创建新的 Github 分支 。 我们建议类似 `pagename-migration`.

3.  使用 wget 或类似工具(如. `wget -O urdf.html https://wiki.ros.org/urdf`。或者您可以使用您的网页浏览器来保存页面的 HTML 。

4.  接下来您需要删除您使用浏览器开发器模式下载的文件中的不相干 HTML , 在 Wiki 页面中找到第一个有用的 HTML 元素的名称。 在大多数情况下, 文件第三行之间所有的 HTML 都从 `<head>` 标记,通过第一个开始 `<h1>` 标记可以安全删除。如果有目录,第一个有用的标记可以是 `<h2>` 标签。类似地,ROS wiki 包含一些页脚文本,开头是 `<div id="pagebottom"></div>` 并结束于正上方 `</body></html>` 也可以删除。

5.  通过运行 HTML 和调整后的文本之间的 PanDoc 转换转换转换您的 html 文件。 以下命令将 HTML 文件转换为等效的 reStructured 文本文件 : `pandoc -f html -t rst urdf.html > URDF.rst`.

6.  尝试使用 `make html` 命令。可能存在错误和警告,您需要处理。

7.  **小心点** 通过整个页面读取, 以确保该材料符合ROS 2. 请检查每个链接是否指向 Docs. ros.org 上的适当位置 。 内部文档引用必须更新以指向等效的ROS 2 材料 。 您更新的文档不应指向ROS Wiki 。 这一过程可能需要您大量修改文档, 您可能需要调取多个 wiki 文件 。 您应该确认文档中的每一个代码样本都在ROS 2 下正确工作 。

8.  查找和下载旧文档中的任何图像。 最简单的做法是在浏览器中右键点击并下载所有图像。 或者您可以通过搜索找到图像 。 `<img src>` HTML文件中的标记。

9.  对于下载的每个图像文件, 更新图像文件链接, 以指向 ROS Docs 正确的图像目录。 如果其中任何图像需要更新, 或者可以替换为 [美人鱼](https://mermaid.js.org/intro/) 图表中,请作此修改。请注意,目前只支持核心ROS 2 文档中的 Mermaid.js 。

10. 文档完成后, 使用相应的 Sphinx 命令在您新的 Rst 文档的顶部添加目录。 此块应该替换旧 ROS Wiki 中现有的目录 。

11. 发出您的拉动请求 。 请确定指向 ROS Wiki 原始文件以供参考 。

12. 一旦您的拉动请求被接受,请在原ROS Wiki文章的页首添加一个注释,指向新的文档页.

关于实际操作中这一过程的例子,请参见ROS 2图像处理管道。 [ROS 2 Docs 数据](https://github.com/ros-perception/image_pipeline/blob/rolling/image_pipeline/doc/tutorials.rst) 原文为 [ROS 维基百科](https://wiki.ros.org/image_pipeline)。已完成的文档页面可见于 [图像管道 ROS 2 软件包文档](https://docs.ros.org/en/rolling/p/image_pipeline/).

<span id="building-the-site-with-github-codespaces"></span>

## 以 GitHub 编码空间构建网站

首先,您需要有一个 GitHub 账户( 如果您没有, 可以免费创建 ) 。 然后, 您需要前往该账户 。 [ROS 2 文档 GitHub 仓库](https://github.com/ros2/ros2_documentation)。在此之后,您可以在代码空间中打开寄存器,只需点击寄存器页面的“代码”按钮即可,然后从下拉菜单中选择“以代码空间打开”即可。

[![代码空间创建](images/codespaces.png)](images/codespaces.png)

在此之后, 您将被重定向到您的代码空间页面, 您可以看到代码空间创建的进展。 完成后, 浏览器将打开一个视觉工作室代码标签 。 您可以点击顶板中的“ 结束” 标签或按 <span class="kbd kbd docutils literal notranslate">编译</span>-<span class="kbd kbd docutils literal notranslate">J</span>.

在此终端中, 您可以运行任何您想要的命令, 例如, 您可以运行以下命令来为这个分支构建站点 :

``` console
$ make html
```

最后,要查看该网站,可以点击右下方面板的“Go Live”按钮,然后,它会在浏览器中的新标签中打开网站(需要浏览 `build/html` 文件夹).

[![实时服务器](images/live_server.png)](images/live_server.png) <span id="building-the-site-with-devcontainer"></span>

## 使用 Devcontainer 构建网站

[ROS 2 文档 GitHub 仓库](https://github.com/ros2/ros2_documentation) 也支持 `Devcontainer` 使用 Visual Studio 代码的开发环境。这将使您在不改变操作系统的情况下更容易构建文档 。

见 [使用 VSCode 和 Docker 配置 ROS 2（社区贡献）](../../How-To-Guides/Setup-ROS-2-with-VSCode-and-Docker-Container.md) 在以下程序之前安装 VS 代码和 Docker。

克隆寄存器并启动 VS 代码 :

``` console
$ git clone https://github.com/ros2/ros2_documentation
$ cd ./ros2_documentation
$ code .
```

要使用 `Devcontainer`,您需要在其扩展搜索(CTRL+SHIFT ⁇ )中安装“远程开发”扩展。

然后,使用 `View->Command Palette...` 或 时 间 `Ctrl+Shift+P` 打开命令调色板。搜索命令 `Dev Containers: Reopen in Container` 并执行。这将自动为您构建您的开发插件容器。

要构建文档,请使用 `View->Terminal` 或 时 间 `` Ctrl+Shift+` `` 财务报告和财务报告 `New Terminal` 在 VS 代码中。在终端内,您可以构建文档 :

``` console
$ make html
```

[![VS 代码解析器](images/vscode_devcontainer.png)](images/vscode_devcontainer.png) <span id="writing-pages"></span>

## 写入页面

ROS 2文档网站使用 `reStructuredText` 格式,是 Sphinx 使用的默认的纯文本标记语言。 `reStructuredText` 概念、语法和最佳做法。 `reStructuredText` 文件 **请确保每行只写一句, 因为这样可以更容易地审查和修改您的文件 。** 另外, 请注意您文件中使用白色空间 ! ROS 2 文档 linter 将不接受带有跟踪的白色空间的拉动请求。 我们建议您启用自动显示白色空间, 或者在您的编辑器支持时进行清理 。

你可以参考 [reStructured Text 用户文档](https://docutils.sourceforge.io/rst.html) 详细技术规格。

<span id="id4"></span>

### 目录

用于生成目录的指令有两种类型, `.. toctree::` 财务报告和财务报告 `.. contents::`。该词 `.. toctree::` 用于顶级页面,例如 `Tutorials.rst` 设置其儿童页面的顺序和可见度。该指令创建了左侧导航面板和与所列儿童页面的页内导航链接。它帮助读者理解单独的文档部分的结构,并在页面之间导航。

``` rst
.. toctree::
   :maxdepth: 1
```

那个... `.. contents::` 指令用于生成该特定页面的目录。它将所有当前标题在页面中解析,并构建一个页面内嵌入式目录。它帮助读者在页面中看到内容概览和导航。

那个... `.. contents::` 指令支持对嵌入区段最大深度的定义。 `:depth: 2` 将只显示目录中的节和小节。

``` rst
.. contents:: Table of Contents
   :depth: 2
   :local:
```

<span id="headings"></span>

### 标题

文档中主要使用四个标题类型,注意符号数量必须与标题长度相符.

``` rst
Page Title Header
=================

Section Header
--------------

2 Subsection Header
^^^^^^^^^^^^^^^^^^^

2.4 Subsubsection Header
~~~~~~~~~~~~~~~~~~~~~~~~
```

我们通常使用一个位数来进行小节的编号,而两个位数(点分隔)来进行子节的编号,在图论和How-To-Guides中.

<span id="lists"></span>

### 列表

恒星数 `*` 用于列出无序项目,并带有圆点和数字符号 `#.` 用于列出编号项目。这两个项目都支持嵌入式定义,并将相应调整。

``` rst
* bullet point

  * bullet point nested
  * bullet point nested

* bullet point
```

``` rst
#. first listed item
#. second lited item
```

<span id="code-formatting"></span>

### 代码格式

文本代码可以使用 `backticks` 用于显示 `highlighted` 代码。

``` rst
In-text code can be formatted using ``backticks`` for showing ``highlighted`` code.
```

页面中的代码块需要使用 `.. code-block::` [指令](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-code-block). `.. code-block::` 支持语法加亮的代码, 如 `C++`, `YAML`, `console`, `bash`指令中的代码需要缩进。

``` rst
.. code-block:: C++

   int main(int argc, char** argv)
   {
      rclcpp::init(argc, argv);
      rclcpp::spin(std::make_shared<ParametersClass>());
      rclcpp::shutdown();
      return 0;
   }
```

<span id="code-blocks-bash-vs-console"></span>

#### 代码块 : `bash` 与《公约》第2条的关系 `console`

`bash` 财务报告和财务报告 `console` 类似,但它们有两种不同的目的。选择正确的一个对于确保内容格式正确以及副本按钮复制正确的内容非常重要。下面是对每个内容的解释;跳到本节末尾,以列出使用大小写和相应的示例。

`bash` 用于脚本,例如用于脚本文件的shash命令。示例结果 :

``` bash
export ROS_DOMAIN_ID=42
ros2 run turtlesim turtlesim_node
```

`console` 用于命令在终端中运行,可选地包含其输出。这可以清楚地说明给定的命令需要在终端中运行。它也可以使用诸如“快速符号”之类的快速符号将命令行与输出行分开。 `$` 或 时 间 `#`。命令行被格式化为shash命令,而输出行被格式化为普通文本。不能选择提示符号,点击右上角副本中的复制按钮 *仅限* 命令,而不是输出或提示符号。这意味着,如果 `console` 代码块使用时无任何 `$`中,复制按钮不会复制任何行。示例结果 :

``` console
$ export ROS_DOMAIN_ID=42
$ ros2 run turtlesim turtlesim_node --ros-args --remap "__node:=my_turtle"
[INFO] [1742150439.022947971] [my_turtle]: Starting turtlesim with node name /my_turtle
[INFO] [1742150439.026043867] [my_turtle]: Spawning turtle [turtle1] at x=[5.544445], y=[5.544445], theta=[0.000000]
```

将以上内容与 `bash` `code-block`:

``` bash
$ export ROS_DOMAIN_ID=42
$ ros2 run turtlesim turtlesim_node --ros-args --remap "__node:=my_turtle"
[INFO] [1742150439.022947971] [my_turtle]: Starting turtlesim with node name /my_turtle
[INFO] [1742150439.026043867] [my_turtle]: Spawning turtle [turtle1] at x=[5.544445], y=[5.544445], theta=[0.000000]
```

为了简化代码块, `bash` 仍然可以使用,无需 `$` 如果代码块没有包含任何输出行,则用于在终端中运行的命令。帮助在其中选择 `bash` 财务报告和财务报告 `console`,参见下列使用案例和相应示例清单:

1.  用于复制到脚本文件的命令

    - 使用 `.. code-block:: bash` 不含 `$`:

      > ``` bash
      > export ROS_DOMAIN_ID=42
      > ros2 run turtlesim turtlesim_node
      > ```

2.  用于终端运行的命令 :

    - 强烈建议使用 `.. code-block:: console` 与 `$` 。如果需要显示输出,请将输出包含在同一块中:

      > ``` console
      > $ source /opt/ros/rolling/setup.bash
      > $ ros2 run turtlesim turtlesim_node
      > [INFO] [1743878028.269334696] [turtlesim]: Starting turtlesim with node name /turtlesim
      > [INFO] [1743878028.275096618] [turtlesim]: Spawning turtle [turtle1] at x=[5.544445], y=[5.544445], theta=[0.000000]
      > ```
      >
      > > **说明**
      > >
      > > 如果一些输出线开始于 `#`,将命令与输出区分开来至关重要,因为 `#` 符号用来表示一个命令。因此,将输出放在单独的 `.. code-block:: text`.

<span id="images"></span>

### 图像

图像可以使用 `.. image::` 指令。 命令。

``` rst
.. image:: images/turtlesim_follow1.png
```

在此情况下,图像文件( C)`turtlesim_follow1.png`)位于该区。 `images/` 相对目录 `.rst` 使用图像的文件。

然而,所有图像文件最终都出现在一个 `_images/` 相对于文档根的目录。因此,在使用时 `:target:` 添加超链接到图像文件,使用连接到根目录的相对链接,然后到 `_images/` 目录。

``` rst
.. image:: images/turtlesim_follow1.png
   :target: ../../_images/turtlesim_follow1.png
```

<span id="charts-graphs-and-diagrams"></span>

### 图表和图表

ROS 2 文档目前支持使用 [美人鱼图.](https://mermaid.js.org/intro/) 我们更喜欢图表、图表和图表使用美人鱼而不是静态图像文件,因为它使我们能够随着项目的发展对这些资源进行程序更新和编辑。 [美人鱼图语言语法可以在他们的网站上找到.](https://mermaid.js.org/intro/syntax-reference.html)

<span id="references-and-links"></span>

### 参考资料和链接

<span id="external-links"></span>

#### 外部链接

创建外部网页链接的语法如下所示.

``` rst
`ROS Docs <https://docs.ros.org>`_
```

上述链接将显示为 [ROS 文档](https://docs.ros.org)。注意最后的单词后面的下划线。

<span id="internal-links"></span>

#### 内部链接

那个... `:doc:` 指令用于创建其他页面的文本链接。

``` rst
:doc:`Quality of Service <../Tutorials/Quality-of-Service>`
```

注意使用文件的相对路径 。

那个... `ref` 指令用于链接一个页面的特定部分。这些可以是当前页面或不同页面中的标题、图像或代码部分。

在需要预定对象之前,明确目标的定义。 `_talker-listener` 标题前的一行 `Try some examples`.

``` rst
.. _talker-listener:

Try some examples
-----------------
```

现在可以创建从文档中任意页面到该标题的链接.

``` rst
:ref:`talker-listener demo <talker-listener>`
```

此链接将使用 HTML 锁定链接导航读取器到目标页面 `#talker-listener`.

<span id="macros"></span>

#### 宏

Macros可用于简化针对多个分布的写作文档.

通过将宏名称包含在卷曲括号中来使用宏。例如,在生成用于滚动的文档时, `rolling` 分支 :

| 宏 | 示例 | 成为(为滚动) |
|----|----|----|
| \[DISTRO\] (英语). | ros -{DISTRO} -pkg (英语). | 滚转-pkg |
| {DISTRO_TITLE} | 罗斯2号 {DISTRO_TITLE} | ROS 2 滚动 |
| {DISTRO_TITLE_FULL} | ROS 2 {DISTRO_TITLE_FULLL\] (英语). | ROS 2 滚筒 |
| {REPOS_FILE_BRANCH} | git 退出 {REPOS_FILE_BRANCH} | Git 退约滚动 |
| {interface_link(std_msgs/msg/String)} | 参见:{interface_link(std_msgs/msg/String)}. | 见: <https://docs.ros.org/en/rolling/p/std_msgs/msg/String.html>. |
| {interface(std_msgs/msg/String)} | 发布 {介面(std\_ msgs/ msg/String)} 。 | 发布a [std_msgs/msg/String](https://docs.ros.org/en/rolling/p/std_msgs/msg/String.html). |
| {package_link(rclcpp)} | 见:{package_link(rclcpp)}. | 见: <https://docs.ros.org/en/rolling/p/rclcpp/>. |
| {包装(rclcpp)} | 使用 {package(rclcpp)} 。 | 使用 [rclcpp](https://docs.ros.org/en/rolling/p/rclcpp/). |

同一文件可以用于多个分支(即用于多个distros),生成的内容将具有distros特殊性.
