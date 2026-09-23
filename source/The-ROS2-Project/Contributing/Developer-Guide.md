---
translation_status: machine_translated
source: The-ROS2-Project/Contributing/Developer-Guide.rst
---

<span id="ros-2-developer-guide"></span>

# ROS 2 开发者指南

本页界定了我们在开发ROS 2时采用的做法和政策.

<span id="general-principles"></span> <span id="id1"></span>

## 一般原则

一些原则是所有ROS 2发展的共同原则:

- **共有**: 每一个在ROS 2 上工作的人都应该感受到对系统所有部分的所有权。一个代码块的原作者没有任何特殊的权限或义务来控制或维护这个代码块。每个人都可以随时在任何地方提出修改,处理任何类型的票,并审查任何拉动请求。

- **什么都愿意做,随便你**:作为共同所有权的必然结果,每个人都应愿意承担任何现有任务,并对系统的任何方面作出贡献。

- **请求帮助**:如果你遇到什么麻烦,请通过罚单、评论或电子邮件等方式,请其他开发人员帮助。

<span id="quality-practices"></span>

## 质量做法

套件可根据其遵循的开发做法,按照《原则和规则》中的准则,确定不同的质量水平。 [REP 2004: 成套质量类别](https://reps.openrobotics.org/rep-2004/)。这些类别根据其关于版本、测试、文件等的政策而有所区别。

以下各节是为确保核心软件包质量最高而遵循的具体开发规则。 我们建议所有ROS开发者努力遵守以下政策,以确保整个ROS生态系统的质量。

关于具体守则建议,请参见: [质量指南](Quality-Guide.md).

<span id="use-of-generative-ai"></span>

### 基因AI的使用

在为ROS代码或文件提供任何形式的贡献时,您必须遵循 [OSRF 政策](https://osralliance.org/wp-content/uploads/2025/05/OSRF-Policy-on-the-Use-of-Generative-Tools-Generative-AI-in-Contributions.pdf) 关于使用基因AI的问题。

这包括使用经过现有人造内容培训的模型自动创建您贡献的任何部分的工具。它不包括通过标准算法或适当许可的内容库生成内容的工具。

<span id="versioning"></span> <span id="semver"></span>

### 版本

我们会使用 [语义版准则](http://semver.org/) (`semver`)用于版本.

我们还将遵守建立在以下基础上的《俄罗斯联邦规则》中的某些具体规则: `semver's` 完全含义 :

- 主要版本增量(即断开更改)不应在发布的ROS发行中进行.

  - 补丁(界面保存)和小(非破碎)版本增量不会破坏兼容性,所以这些类型的更改 *已经* 允许在释放之内。

  - 主要ROS发布是发布断裂变化的最佳时机. 如果一个核心包需要多个断裂变化,它们应该合并到它们的集成分支(例如滚动)中,以便快速地在CI中捕捉问题,但一起发布以减少ROS用户的主要发布次数.

  - 虽然重大增量需要新的分配,但新的分配不一定需要大的起伏(如果可以进行开发和发布而不突破API的话).

- 对于编译的代码,ABI被认为是公共界面的一部分. 任何需要重新编译依赖代码的修改都被认为是重大(破解).

  - ABI 断开更改 *能够* 以小版本凸起方式制作 *在此之前* a 发行发行(在滚动发行中添加)。

- 我们在Dashing和Eloatent对核心软件包实行API稳定性,尽管其主要版本组件是: `0`,尽管 [SemVer 的规格](https://semver.org/#spec-item-4) 关于初步发展。

  - 随后,软件包应努力达到成熟状态,并增加到版本 `1.0.0` 这样才能匹配 `semver's` 规格。

<span id="caveats"></span>

#### 洞穴( 洞穴)

这些规则是 *最佳成绩*在不可能的极端情况下,可能需要在一个大版本/分配范围内打破API,无论是计划外的中断递增,主要版本还是次要版本将逐案评估。

例如,考虑与主要版本相对应的释放X塔的情况 `1.0.0`,并发布了与主要版本相对应的 Y-turtle `2.0.0`.

如果发现X炮塔绝对需要破解API的固定装置,则会撞到: `2.0.0` 显然不是一个选择,因为 `2.0.0` 已经存在。

在这种情况下,处理X-turtle版本的解决方案都是非理想的:

1.  Bumping X-turtle的小版本:非理想化,因为它违反了SemVer的原则,即突破更改必须撞向主要版本.

2.  翻转 X 涡轮的主要版本过 Y 涡轮(到 `3.0.0`:非理想,因为旧的Distro版本会比已有的较新的Distro版本要高,后者会使版本特定条件代码失效/破解.

开发者必须决定使用哪一种解决方案,或者更重要的是,他们愿意打破哪一种原则。 我们不能建议一种或另一种方法,但无论哪种方法,我们都要求采取明确措施,将干扰及其解释手动告知用户(仅超出版本增量 ) 。

如果没有Y炮塔,即使从技术上说,它只是个补丁, X炮塔必须撞到 `2.0.0`。这个案例坚持SemVer,但违反了我们自己的规则,即不应在公布的分发中引入重大增量。

这就是为什么我们考虑 版本规则 *最佳成绩*。与上述例子一样,准确定义我们的版本系统也是不可能的。

<span id="public-api-declaration"></span>

#### 公共 API 声明

根据 `semver`,每个软件包必须明确宣布一个公共API。我们将使用软件包的质量声明中的“公共API宣言”部分来宣布哪些符号是公共API的一部分。

对于大多数 C 和 C++ 软件包来说, 声明是它安装的任何信头。 但是, 定义一组被视为私有的符号是可以接受的。 避免信头中的私有符号可以帮助ABI 稳定性, 但不需要 。

对于其他语言,如Python,必须明确定义一个公共API,这样就可以明确在版本指南方面可以依赖哪些符号. 公共API也可以被扩展到构建诸如配置变量,CMake配置文件等文物,以及可执行文件以及命令行选项和输出. 公共API的任何元素都应该在软件包的文档中清晰说明. 如果您正在使用的东西没有在软件包的文档中被明确列为公共API的一部分,那么您就不能依赖它不会在次要版本或补丁版本之间更改.

<span id="deprecation-strategy"></span>

#### 折旧战略

在可能的情况下,我们也会对主要版本增量使用滴答减速和迁移策略. 新的减速将出现在新的发行版,并伴有编译器警告,表示该功能正在贬值. 在下一次发行时,该功能将被完全删除(没有警告).

函数示例 `foo` 贬值,由职能取代 `bar`:

| 版本       | API                                                     |
|------------|---------------------------------------------------------|
| X 炮塔     | 无效的 foo( );                                          |
| Y - 涡轮   | \[\[已折旧(“使用栏()”)\]\]\]无效的foo();\<br\>无效栏(); |
| Z 炮塔( T) | 空栏( );                                                |

我们决不能在发行后添加折损。 折损并不一定需要大版本的折叠, 不过。 如果折叠发生在发行前, 则可以在小版本的折叠中引入折叠( 类似于ABI 打破更改 ) 。

例如,如果 X 炮塔开始开发为 `2.0.0`中,可添加折旧 `2.1.0` 在X塔释放前。

我们将尽量保持各方的兼容性。 但是,与SemVer、tick甚至贬值相关的警告一样,在某些情况下可能无法完全遵守。

<span id="change-control-process"></span>

### 改变控制进程

- 所有更改都必须通过拉动请求.

- 我们会执行 [开发者原产地证书( DCO)](https://developercertificate.org/) 在ROSCore寄存器中显示拖动请求。

  - 它要求所有承诺的信息包含: `Signed-off-by` 与匹配执行作者的电子邮件地址对齐。

  - 你可以通过 `-s` / `--signoff` 页:1 `git commit` 手动引用或写入预期信件(例如: `Signed-off-by: Your Name Developer <your.name@example.com>`).

  - DCO是(美国) *没有* 仅处理白空去除、类型校正和其他问题的牵引请求所需 [微小变化](http://cr.openjdk.java.net/~jrose/draft/trivial-fixes.html).

- 总是为所有人运行 CI 任务 [第1级平台](https://reps.openrobotics.org/rep-2000/#support-tiers) 用于每个拉动请求, 并在拉动请求中包含与工作的联系 。 (如果你无法访问 Jenkins 工作, 有人会为你触发工作 。 )

- 开发者若不提出拉力请求,至少需要获得1个批准,才能考虑批准。合并前必须获得批准 。

  - 软件包可能选择增加这个数量 。

<span id="guidelines-for-backporting-prs"></span>

#### 重新输入公关的准则

更改旧版本 ROS 时 :

- 确保特性或修正在滚动分支中被接受和合并,然后打开一个PR来背传到旧版本的修改.

- 回移到旧版本时, 也考虑回移到其它版本 [仍然支持的版本](../../Releases.md),甚至非LTS版本.

- 如果您正在完整地返回一个单一的公关,请将后端端口的公关“\[Distro\] \< 原公关名称\>”命名为“公关”。

- 链接到您正在从您的背口公关描述中回转的所有公关。

- 软件包维护者通常使用 [默吉尼西奥](https://mergify.com/) 在必要时自动返回到下游的分布,但开发人员在必要时仍然可以进行上述的人工回移操作。

<span id="documentation"></span>

### 文档

所有软件包均应有这些文档元素,载于其README中,或与其README链接:

- 说明和目的

- B. 公共物价指数的定义和说明

- 实例

- 如何构建和安装(应参考外部工具/工作流程)

- 如何建立和运行测试

- 如何构建文档

- 如何发展(用于描述诸如: `python setup.py develop`)

- 许可证和版权声明

每个源文件都必须有许可证和版权声明,用自动插件检查.

每个软件包必须有一个LICENSE文件,一般是Apache 2.0许可证,除非软件包有现有的允许许可证(例如rviz使用三聚BSD).

每一套资料应尽量说明自己及其目的,假定读者在未事先了解ROS或其他有关项目的情况下偶然发现。

每个软件包都应该定义和描述其公开的API,这样用户对语义版本政策涵盖的内容有合理的期望. 即使在C和C++中,公共API可以通过API和ABI检查执行,这也是描述代码的布局和代码每个部分的功能的好机会.

使用任何软件包都应该是容易的,从软件包的文档中可以理解如何构建、运行、构建和运行测试以及构建文档。 显然,我们应该避免重复使用共同的工作流程,比如在工作空间构建一个软件包,但基本的工作流程要么应该描述,要么应该引用。

最后,它应该包括开发者的任何文档。这可能包括使用类似设备测试代码的工作流程。 `python setup.py develop`,或者可能意味着描述如何使用您的包提供的扩展点。

实例:

- [能力](https://docs.ros.org/hydro/api/capabilities/html/)

  - 这举出一个描述公共API的文献的例子

- [catkin_tools](https://catkin-tools.readthedocs.org/en/latest/development/extending_the_catkin_command.html)

  - 这是描述软件包扩展点的例子

<span id="api-documentation-for-ros-packages"></span>

#### ROS 软件包的 API 文档

所有发布的ROS软件包的 API 文档可以是 [这里找到的](https://docs.ros.org/en/rolling/p/)我们建议使用 [索引.ros.org](https://index.ros.org/) 通过可用的ROS软件包查找其文档。

如果您是ROS 软件包开发者, 请查看 。 [我们的“如何”包级文件指南](../../How-To-Guides/Documenting-a-ROS-2-Package.md)。所有已发布的ROS 2 软件包的文档自动托管于 [维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献: 维基文库中相关的原始文献](https://docs.ros.org/en/rolling/p/).

<span id="testing"></span>

### 测试

所有软件包应具有一定水平 [系统、集成和/或单元测试。](../../Tutorials/Intermediate/Testing/Testing-Main.md#testingmain)

**单位测试** 应始终在正在测试的软件包中,并应利用诸如 `Mock` 在构建的情景中尝试并测试代码库的狭义部分. 单位测试不应该带来不是测试工具的测试依赖性,例如gtest,鼻试,pytest,moom等.

**融合测试** 可以测试代码部分之间或代码部分与系统之间的相互作用。它们经常以我们期望用户使用的方式测试软件接口。与单位测试一样,整合测试应该在正在测试的软件包中,除非绝对有必要,否则不应带来非工具测试依赖性,即所有非工具依赖性只应在极端审查下允许,以便尽可能避免。

**系统测试** 旨在测试包之间的端对端状态,并应在自己的包中以避免膨胀或耦合包,并避免循环依赖。

一般而言,应尽量减少外部或交叉包件测试依赖性,以防止循环依赖性和紧密结合的包件测试。

所有包件都应该有某些单位测试,并可能进行整合测试,但是它们应该有的程度是基于包件的质量类别。以下小节适用于 " 一级 " 包件:

<span id="code-coverage"></span>

#### 代码覆盖

我们将提供线覆盖,并达到95%以上的线覆盖。 如果一个较低百分比的目标有正当理由,那么它必须被显著地记录下来。 我们可以提供分支覆盖,或者将代码排除在覆盖范围之外(测试代码、调试代码等 ) 。 我们要求在合并修改之前增加或保持相同的覆盖,但如果做出修改,以合理的理由降低代码覆盖(例如删除先前覆盖的代码可能导致百分比下降),则可以接受。

<span id="performance"></span>

#### 业绩

我们强烈建议进行性能测试,但承认这些测试对某些软件包没有意义。 如果进行性能测试,我们将选择在每次修改之前或在每次发布之前或同时发布之前进行检查。 我们还需要说明将修改合并或发布降低性能的理由。

<span id="linters-and-static-analysis"></span>

#### 林特和静态分析

我们用 [ROS 代码样式](Code-Style-Language-Versions.md) 并用插件执行它, [ament_lint_common](https://github.com/ament/ament_lint/tree/rolling/ament_lint_common/doc/index.rst)。作为下列内容的一部分的所有静态/静态分析 `ament_lint_common` 必须使用。

那个... [ament_lint_auto](https://github.com/ament/ament_lint/blob/rolling/ament_lint_auto/doc/index.rst) 文档提供运行信息 `ament_lint_common`.

<span id="general-practices"></span>

## 一般做法

一些做法是所有ROS 2发展的共同做法。

这些做法并不影响软件包的质量水平,例如: [REP 2004 (英语).](https://reps.openrobotics.org/rep-2004/),但仍被大力推荐为发展进程.

<span id="issues"></span>

### 问题

在提交问题时,请确保:

- 包含足够的信息,让其他人了解这个问题。在ROS 2中,缩小问题的原因需要以下几点。在每一个类别中尽可能多地测试替代品将特别有用。

  - **操作系统和版本.** 原因:ROS 2支持多个平台,一些bug是特定版本的操作系统/编译器所特有的.

  - **安装方法.** 理由: 只有当ROS 2 是从二进制档案或从 dibs 中安装时, 才会出现某些问题。 这有助于我们确定问题是否与包装过程有关 。

  - **ROS 2的具体版本.** 原因 : 一些错误可能存在于特定的ROS 2 释放中, 并且后来被修复 。 重要的是要知道您的安装是否包含这些修正 。

  - **正在采用DDS/RMW的执行** (见 [此页面](../../Concepts/Intermediate/About-Different-Middleware-Vendors.md) 原因:通信问题可能针对正在使用的基本ROS中间软件。

  - **ROS 2客户端库正在使用中.** 理由:这有助于我们缩小问题可能存在的堆积层。

- 列入复制本问题的步骤清单。

- 发生错误时考虑提供 [短, 自包含, 正确( 可编译), 例如](http://sscce.org/)如果其他人能够轻易地复制,问题就更有可能得到解决。

- 提到已经尝试过的解决问题的步骤,包括:

  - 升级到代码的最新版本, 可能包括尚未发布的错误修正 。 请参看 [本节](../../Installation.md#building-from-source) 并遵守指示,以获得“滚动”分支。

  - 尝试不同的RMW执行。 [此页面](../../How-To-Guides/Working-with-multiple-RMW-implementations.md) 如何做到这一点。

<span id="branches"></span>

### 分支

> **说明**
>
> 这些只是准则。 由软件包维护者来选择符合自己工作流程的分支名称 。

良好做法是: **独立分支** 在软件包的源寄存器中,它所瞄准的每个ROS分布。这些分支通常以它们所选择的分布来命名。例如,a `humble` 具体针对Humble分布的发展分支。

这些分支也发行,以适当的分布为目标。针对特定ROS分布的开发可以在适当的分支上进行。例如: `foxy` 交给我们 `foxy` 分支和软件包的发布 `foxy` 由同一分支制成。

> **说明**
>
> 这需要软件包维护者酌情执行后端port或前端port,以使所有分支保持最新的特性. 维护者还必须对仍然从软件包释放的所有分支进行一般维护(bug修正等).
>
> 例如,如果一个特性被合并到滚动专用分支(例如. `rolling` 或 时 间 `main`),而该特征也适合Humble分布(不要打破API等),那么将该特征背向Humble特定分支是好的做法.
>
> 如果有新的特性或错误修正,维护者可以释放这些旧的分布。

**怎么样** `main` **财务报告和财务报告** `rolling` **?**

`main` 一般目标 [Rolling](../../Releases/Release-Rolling-Ridley.md) (因此,下一次未释放ROS的发行),尽管维护者可能决定开发并释放出一个 `rolling` 切换成树枝。

<span id="library-versioning"></span>

### 库版本

我们将在一个软件包内对所有库进行版本化。 这意味着库会从软件包中继承其版本。 这样可以使软件库和软件包的版本与发布共享存储器的软件包的政策保持一致并共享推理。 如果您需要软件库拥有不同的版本, 那就考虑将它们拆分为不同的软件包 。

<span id="development-process"></span>

### 发展进程

- 默认分支( 大多数情况下是滚动分支) 必须总是构建、 通过所有测试和编译而无需警告 。 如果在任何时候出现回归, 至少恢复先前状态才是最优先的事项 。

- 总是用启用的测试来构建 。

- 修改后总是在本地运行测试, 并在拉动请求中提出测试。 除了使用自动测试之外, 还要手动运行修改后的代码路径, 以确保补丁正常工作 。

- 总是为每个拉请求运行所有平台的 CI 任务,并在拉请求中包含与任务的联系.

关于建议软件开发工作流程的更多详情,见 [软件开发生命周期](#software-development-lifecycle) 节。

<span id="changes-to-rmw-api"></span>

### 修改 RMW API

更新时 [RAMW API (英语:RMW API)](https://github.com/ros2/rmw),需要同时更新第一级中间软件库的 RMW 执行。例如,一个新的功能 `rmw_foo()` 引入RAMW API的软件包必须在下列软件包中执行(如ROS Galactic):

- [rmw_connextdds](https://github.com/ros2/rmw_connextdds)

- [rmw_cyclonedds](https://github.com/ros2/rmw_cyclonedds)

- [rmw_fastrtps](https://github.com/ros2/rmw_fastrtps)

非Tier 1 中间软件库的更新如果可行也应予以考虑(例如,视变化大小而定). See [REP-2000号报告](https://reps.openrobotics.org/rep-2000/) 用于列表的中间软件库及其层次。

<span id="tracking-tasks"></span>

### 跟踪任务

为了帮助组织ROS 2的工作,核心ROS 2开发团队采用kanban风格. [GitHub 项目板](https://github.com/orgs/ros2/projects).

不过,并非所有问题和拉动请求都跟踪在项目板上。一个板通常代表即将发布的或特定项目。 [ROS 2 仓库](https://github.com/ros2) 各期网页。

任何给定的ROS 2项目板中的列的名称和目的各不相同,但一般采用相同的一般结构:

- **来做**: 与项目有关的问题,准备指定

- **进行中**:积极拉动目前正在开展工作的请求

- **正在审查**: 在工作完成并随时可以审查和目前正在积极审查的方面提出请求

- **完成( E)**: 调用请求和相关问题合并/结束(供参考)

要请求更改许可,只需对您感兴趣的门票进行评论。 根据复杂程度,描述您计划如何处理可能是有益的。 我们将更新状态(如果你没有权限), 并且您可以开始拉动请求。 如果您定期出价, 我们可能只允许您自己管理标签等。

<span id="package-naming-conventions"></span>

### 软件包命名公约

名称在ROS中发挥重要作用,遵循命名惯例简化了学习和了解大型系统的过程.

ROS 软件包占用一个平坦的命名空间,所以命名应当谨慎和一致。 [REP-144 (韩语)](https://reps.openrobotics.org/rep-0144/)

- 软件包名称应该遵循常见的 C 变量命名惯例: 小写, 以字母开头, 使用下划线分隔符, 如激光\_ 查看器

- 软件包的名称应该足够具体,以识别软件包的用途。比如,一个运动计划员不叫规划员。如果它执行波前传播算法,它可能叫做波前宣传算法。 制作一个特定名称和避免它过于动词化之间显然存在矛盾。

  - 应避免使用诸如提法等囊括所有名称,因为它们没有将哪些内容放入包内,哪些内容应该放入包外。

- 为检查是否取名,请咨询: <https://index.ros.org/packages/>。如果您想要将您的存储器列入该列表,请参见 [rosdistro 贡献指南](https://github.com/ros/rosdistro/blob/master/CONTRIBUTING.md).

- 我们的目标是开发一套能让机器人做有趣的事情的工具。软件包的名称应该告诉你该软件包是做什么的,而不是从哪里来的。我们作为一个社区应该可以做到这一点。一个Ubuntu的发行提供了大约33,000个软件包,而没有在名称中插入原产地或作者。

- 软件包名称的前缀只有在软件包并非意在更广泛地使用时才被推荐(例如,专供PR2机器人使用的软件包)。 `pr2_` 前缀)。您在取消已有的软件包时,可能会给软件包名称添加前缀,但是,同样,前缀希望能够沟通改变的东西,而不是谁改变它。

- 用“ros”为软件包名前缀是ROS软件包的冗余。除了非常核心软件包外,不建议这样做。

<span id="units-of-measure-and-coordinate-system-conventions"></span>

### 计量单位和协调系统公约

标准单位和协调公约,供区域办事处使用,现已正式确定。 [REP-0103号报告](https://reps.openrobotics.org/rep-0103/)所有信息都应该遵循这些准则,除非有非常强烈的理由,而且有非常明确的文件记录,以避免混淆。

在“太近”或“太远”的距离测量中,在俄罗斯联邦的“太近”或“太远”等特殊条件的表示方式已经正式化。 [REP-0117号](https://reps.openrobotics.org/rep-0117/).

<span id="programming-conventions"></span>

### 方案拟订公约

- 防御性编程:确保假设尽早被持有. 例如,检查每个返回代码,确保至少放弃一个例外,直到案件得到更优雅的处理.

- 所有错误信息必须被引导到 `stderr`.

- 在尽可能狭窄的范围内宣告变量 。

- 按字母顺序保留项目组(依赖、进口、包括等)。

<span id="c-specific"></span>

#### C++ 特定

- 避免使用直接流线( E)`<<`改为: `stdout` / `stderr` 防止多个线条之间的交叉。

- 避免使用参考文献 `std::shared_ptr` 如果原始实例超出范围, 并且正在使用该引用, 它访问释放的内存 。

<span id="filesystem-layout"></span>

### 文件系统布局

软件包和寄存器的文件系统布局应当遵循同样的常规,以便为用户浏览我们的源代码提供一致的经验.

<span id="package-layout"></span>

#### 软件包布局

- `src`: 包含全部 C 和 C++ 代码

  - 还包含未安装的 C/C++ 头

- `include`: 包含所有安装的 C 和 C++ 头

  - `<package name>`: 对于所有已安装的 C 和 C++ 标题, 它们应该由软件包名称来命名文件夹

- `<package_name>`: 包含全部 Python 代码

- `test`: 包含所有自动化测试和测试数据

- `config`: 包含配置文件, 例如 YAML 参数文件和 RViz 配置文件

- `doc`: 包含所有文档

- `launch`: 包含所有发射文件

- `msg`: 包含所有 ROS 信件定义

- `srv`: 包含所有ROS服务定义

- `action`:包含所有ROS 动作定义

- `package.xml`: 定义 [REP-0140 (英语).](https://reps.openrobotics.org/rep-0140/) (可更新为原型).

- `CMakeLists.txt`: 只有使用 CMake 的ROS 软件包

- `setup.py`: 只使用 Python 代码的ROS 软件包

- `README`: 可以在 GitHub 上作为工程的登陆页

  - 这既可以是简短的,也可以是详细的,但至少应该与项目文件联系起来。

  - 考虑在此 README 中设置 CI 或代码封面标签

  - 也可能是这样 `.rst` 或 GitHub 支持的任何东西

- `CONTRIBUTING`:说明缴款准则

  - 这可能包括许可证的含义,例如当使用Apache 2许可证时。

- `LICENSE`: 该包件的许可证副本

- `CHANGELOG.rst`: [REP-0132号报告](https://reps.openrobotics.org/rep-0132/) 符合的更改日志

<span id="repository-layout"></span>

#### 仓库布局

每个软件包应该装在一个与软件包同名的子文件夹中。如果一个寄存器只包含一个单一软件包,则可以选择在寄存器的根部。

<span id="upstream-packages"></span>

### 上游软件包

<span id="packages-in-debian-and-ubuntu-upstream"></span>

#### Debian 和 Ubuntu 上游软件包

多亏了约亨·斯普里克霍夫和利奥波德·帕洛莫-阿韦利亚内达的辛勤努力, [ROS 2 软件包现已可用](https://wiki.debian.org/DebianScience/Robotics/ROS2/Packages) 从主Debian和Ubuntu的储存库。 [以下是Jochen在ROSCON 2015的流程简介.](https://vimeo.com/142151399#t=29m15s). 最初的ROS软件包已经修改,以遵循Debian准则,包括将软件包拆分为多个块,在某些情况下更改名称,按照FHS准则安装/使用,以及在共享库上使用转写.

此外,一些靴子的依赖性,例如命令行工具,例如: `vcstool` 财务报告和财务报告 `colcon` 以及一些库,如 `osrf-pycommon` 财务报告和财务报告 `ament` 也可以在上游包装。

与OSRF提供的ROS软件包不同: <http://packages.ros.org>,则上游储存库中的软件包不附在特定的 [ROS的分发](../../Releases.md)相反,它们是一个及时的快照,将在Debian不稳定范围内定期更新,然后在Debian和Ubuntu下游分布的各个地点进行更新。

<span id="don-t-mix-the-streams"></span>

#### 不要混合溪流

我们强烈建议,不要将来自上游Debian/Ubuntu的ROS包和来自上游的ROS包混合在一起。 <http://packages.ros.org> 在某些情况下,这样的混合系统会正确运作,但两套套套件之间可能存在负面互动。 我们正在与乔亨和朋友合作,通过文件和套件冲突规格来尽量减少问题发生的可能性,但我们期望仍然存在一些风险,包括一些相当微妙的问题。

因此,我们建议你选择从上游安装软件包或从上游安装软件包。 <http://packages.ros.org>,但两者都不同。不仅不应同时安装两者的软件包,而且如果打算使用上游软件包,你甚至不应拥有 <http://packages.ros.org> 在您的 apt 来源中( 即在任何文件中) 输入 `/etc/apt/sources*`)两个源都启用后,可造成两个源之间名称重叠的包件的混合,例如: `python3-rospkg`.

<span id="known-differences"></span>

#### 已知差异

与来自package.ros.org的ROS软件包相比,上游ROS软件包有一些不同之处,人们应该意识到:

- 软件包集不完整 。

- 软件包可能名称不同,被分拆方式不同.

<span id="developer-workflow"></span>

## 开发者工作流程

我们追踪与即将发行和大型项目有关的开放式门票和现行公关,使用 [GitHub 项目板](https://github.com/orgs/ros2/projects).

通常的工作流程是:

- 讨论设计(GitHub 票在适当的寄存器上,设计 PR 到 <https://github.com/ros2/design> (如有需要)

- 在叉子上的特性分支上写入执行

  - 请检查一下 [开发者指南](#) 准则和最佳做法

- 写作测试

- 启用和运行插件

- 使用本地测试 `colcon test` (见E/CN.4/Sub.2/2000/L.10和Add.1和2。) [协作辅导](../../Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.md))

- 一旦一切在本地建立,没有警告,所有测试都过去了, 请运行您的特性分支的CI:

  - 转到 ci.ros2.org

  - 登录( 右上角)

  - 点击 `ci_launcher` 任务

  - 点击“以参数构建” (左栏)

  - 在第一个框“CI_BRANCH_TO_TEST”中,输入您的特性分支名称

  - 击打 `build` 按钮

  (如果你不是ROS 2 的罪犯,你无法进入CI农场。在这种情况下,请公关审查员为您运行CI)

- 如果您的使用需要运行代码覆盖 :

  - 转到 ci.ros2.org

  - 登录( 右上角)

  - 点击 `ci_linux_coverage` 任务

  - 点击“以参数构建” (左栏)

  - 确定将“CI_BUILD_ARGS”和“CI_TEST_ARGS”保留为默认值

  - 击打 `build` 按钮

  - 文件结尾处有关于如何 [解释报告的结果](#read-coverage-report) 财务报告和财务报告 [计算覆盖率](#calculate-coverage-rate)

- 如果 CI 工作没有警告、 错误和测试失败, 请将您工作的链接张贴到您的 PR 或 高级 票上, 汇总您所有的 PR (请参见示例) [这儿](https://github.com/ros2/rcl/pull/106#issuecomment-271119200))

  - 请注意,这些徽章的减值值值在控制台输出中。 `ci_launcher` 任务

- 在批准公关时:

  - 提交公关协议的人使用“Squash and Merge”选项将其合并,以便我们保持一个干净的历史

    - 如果承诺者应当保持隔离:将所有硝酸盐/林特/泰博酸盐合并起来,并合并其余的一组。

      - 注意: 每个 PR 应当针对一个特定特性, 因此 Squash 和 并购 时间应该有99% 。

- 合并后删除分支

<span id="gitconfig-optimization"></span>

### Git配置优化

要推向寄存器, 您需要在您的系统上设置 ssh 密钥。 然而, 我们寄存器的默认 URL 计划是使用 https , 因为它是匿名访问的。 您可以在您的系统中使用 `gitconfig` 选项 `insteadOf` 拥有 `git` 自动使用您的 ssh 密钥, 即使远程被宣布为 https 。

将以下内容添加到您的 `~/.gitconfig`

``` default
[url "ssh://git@github.com/"]
  insteadOf = https://github.com/
```

如果你在GitLab或Bitbucket上工作,

<span id="architectural-development-practices"></span>

## 建筑发展做法

本节介绍在对ROS 2进行大型建筑改造时应当采用的理想生命周期.

<span id="software-development-lifecycle"></span>

### 软件开发生命周期

本节介绍如何逐步规划、设计和实施一个新的特征:

1.  任务创建

2.  创建设计文档

3.  设计审查

4.  执行情况

5.  代码审查

<span id="task-creation"></span>

#### 任务创建

需要修改 ROS 2 关键部分的任务应该在发布周期的早期阶段进行设计审查。如果在后期进行设计审查,这些变化将成为未来发布的一部分。

- 应在适当时提出一个问题。 [ros2 存储器](https://github.com/ros2/)明确描述正在开展的工作。

  - 它应该有一个明确的成功标准,并突出它预期得到的具体改进。

  - 如果该功能针对ROS的发布,确保在ROS的发布单中跟踪此功能([实例](https://github.com/ros2/ros2/issues/607)).

<span id="writing-the-design-document"></span>

#### 写入设计文件

设计文件绝不能包含机密信息。您是否需要设计文件来修改取决于任务有多大。

1.  您正在做一个小的更改或修复一个错误 :

> - 不需要设计文件,但应在适当的储存库中打开一个问题,以跟踪工作并避免重复工作。

2.  你们正在实施一个新的功能,或者想为OSRF拥有的基础设施(如Jenkins CI)做出贡献:

> - 需要设计文件,应协助 [ros2/design](https://github.com/ros2/design/) 将开放上网。 <https://design.ros2.org/>.
>
> - 您应该叉掉存储器, 并提交一个拉动请求, 详细说明设计 。
>
> 提及相关的 ros2 期(例如, `Design doc for task ros2/ros2#<issue id>`在拉动请求或承诺消息中。详细指示载于 [ROS 2 贡献](https://design.ros2.org/contribute.html) 。设计注释将直接针对拉动请求。

如果计划发布任务,并附带一个特定的ROS版本,这种信息应当包含在拉力请求中.

<span id="design-document-review"></span>

#### 设计文件审查

一旦设计准备好供审查,就应提出拉动请求,并指派适当的审查人员。 `package.xml` 维护者字段,参见 [REP-140 (中文(简体) ).](https://reps.openrobotics.org/rep-0140/#required-tags)- 作为审查员。

- 如果设计文件复杂,或者审查人员时间安排有冲突,则可设立可选设计审查会议。

  **会前**

  - 至少提前一周发出会议邀请

  - 建议会议时间为1小时

  - 会议邀请书应列出审查期间作出的所有决定(需要包件维护者批准的决定)

  - 满足与会者要求:设计拉动请求审查员  
    会见可选与会者:所有OSRF工程师(如适用)

  **会议期间**

  - 任务负责人推动会议,提出他们的想法并管理讨论,以确保及时达成协议

  **会后**

  - 任务所有者应该向所有与会者发送会议说明

  - 如果对设计提出了小问题:

    - 任务拥有者应当根据反馈更新设计文件拉动请求

    - 无需作进一步审查

  - 如果对设计提出了重大问题:

    - 可以删除没有明确同意的部分。

    - 将来可重新将可争论的设计部分作为单独的任务提交

    - 如果移除有争议的部件不是一个选项, 直接与软件包所有者合作达成协议

- 一旦达成共识:

  - 保证 [ros2/design](https://github.com/ros2/design/) 如果适用,拉动请求已合并

  - 更新和关闭与这一设计任务相关的 GitHub 问题

<span id="implementation"></span>

#### 执行情况

开始之前,看看 [创建拉取请求（PR）](Contributing-to-code/Making-a-PR.md) 在拉动请求中采用最佳做法。

- 对于每个要修改的还本付息:

  - 修改代码, 如果完成或定期备份您的工作, 则转到下一步 。

  - [自我审查](https://git-scm.com/book/en/v2/Git-Tools-Interactive-Staging) 使用 `git add -i`.

  - 使用 `git commit -s`.

    - 牵引请求应当包含最小的、有实际意义的承诺(例如,大量一行承诺是不可接受的 ) 。 在重复反馈时创建新的固定承诺,或可选择地修改使用的现有承诺。 `git commit --amend` 如果你每次不想创造新的承诺的话。

    - 每项承诺必须有一个适当的书面、有意义的、承诺的信息。 [这儿](https://chris.beams.io/posts/git-commit/).

    - 移动文件必须在单独的承诺中完成, 否则 git 可能无法准确跟踪文件历史 。

    - 拉动请求描述或承诺消息必须包含相关 ros2 问题的引用, 所以在拉动请求合并时会自动关闭 。 请参见此 [医生](https://help.github.com/articles/closing-issues-using-keywords/) 更多细节。

    - 推新的承诺。

<span id="build-farm-introduction"></span>

## 建设农场简介

建设农场位于 [(原始内容存档于2018-09-26). Ci.ros2.org.](https://ci.ros2.org/).

每天晚上,我们都会在各种平台上建立和运行各种情景中的所有测试。 此外,我们还会在合并前测试所有针对这些平台的请求。

检查 [目前的目标平台和架构](../../Installation.md#binary-package-platforms)尽管它演变为加班。

建设农场有几类工作:

- 手动工作( 由开发者手动触发 ) :

  - ci_linux: 在 Ubuntu 上建立 + 测试代码

  - ci_linux-arch64:在 ARM 64 位机(arch64)上,在 Ubuntu 上构建 + 测试代码

  - ci_linux_覆盖:构建+测试+生成测试覆盖

  - ci_linux-rhel: 构建+测试红帽企业 Linux 上的代码

  - ci_窗口: 在 Windows 上建立 + 测试代码

  - ci\_ 启动器: 触发以上列出的所有任务

- 晚上(每晚运行):

  - 调试:用 CMAKE_BUILD_TYPE=调试 构建 + 测试代码

    - nightly_linux_debug

    - nightly_linux-aarch64_debug

    - nightly_linux-rhel_debug

    - nightly_win_deb

  - 发布 : 构建 + 测试使用 CMAKE_BUILD_TYPE = 释放的代码

    - nightly_linux_release

    - nightly_linux-aarch64_release

    - nightly_linux-rhel_release

    - nightly_win_rel

  - 重复: 构建后每次测试最多运行20次, 或直到失败(aka flashfession hunter)

    - nightly_linux_repeated

    - nightly_linux-aarch64_repeated

    - nightly_linux-rhel_repeated

    - nightly_win_rep

  - 覆盖范围:

    - nightly_linux_coverage:构建+测试代码+分析覆盖c/c++和蟒蛇

      - 结果作为 Cobertura 报告导出

- 包装( 每晚运行; 结果被捆绑在一个归档中):

  - packaging_linux

  - packaging_linux-rhel

  - packaging_windows

另有两个建设农场通过提供源包和二进制包的建设、持续整合、测试和分析,支持ROS/ROS 2生态系统。

详细情况,常见问题, 和故障排除,见: [建造农场](Build-Farms.md).

<span id="note-on-coverage-runs"></span>

### 关于覆盖范围运行的说明

ROS 2包的组织方式是给定包的测试代码不仅包含在包内,也可以存在于不同的包中. 换句话说:包可以在测试阶段锻炼属于其他包的代码.

为了达到ROS 2核心包中所有可用代码的覆盖率,建议使用一套固定的拟议寄存器运行构建,该集在詹金斯覆盖工作的默认参数中定义。

<span id="how-to-read-the-coverage-rate-from-the-buildfarm-report"></span> <span id="read-coverage-report"></span>

### 如何从建设农场报告中读取覆盖率

查看给定包的覆盖报告:

- 当 `ci_linux_coverage` 构建完成,单击 `Coverage Report`

- 向下滚动到 `Coverage Breakdown by Package` 表格显示

- 在表格中,请参看第一栏“Name”

建设农场的覆盖报告包括ROS工作空间使用的所有软件包。覆盖报告包含与同一软件包相应的不同路径:

- 带有窗体的名称条目 : `src.*.<repository_name>.<package_name>.*` 这些对应于套件中可用的单位测试,并对照自己的源代码

- 带有窗体的名称条目 : `build.<repository_name>.<package_name>.*` 这些对应于在构建或配置时生成的文件的包件中可用的单元测试

- 带有窗体的名称条目 : `install.<package_name>.*` 这些测试与来自其他包的测试运行的系统/整合测试相对应

<span id="how-to-calculate-the-coverage-rate-from-the-buildfarm-report"></span> <span id="calculate-coverage-rate"></span>

### 如何从建设农场报告中计算覆盖率

使用自动脚本获取单位综合覆盖率 :

> - 从 ci_linux_coverage Jenkins 复制该建筑的 URL
>
> - 下载 [get_coverage_ros2_pkg](https://raw.githubusercontent.com/ros2/ci/master/tools/get_coverage_ros2_pkg.py) 脚本
>
> - 执行脚本 : `./get_coverage_ros2_pkg.py <jenkins_build_url> <ros2_package_name>` ([读取](https://github.com/ros2/ci/blob/master/tools/README.md))
>
> - 抓取脚本输出中“ 组合单元测试” 最终行的结果

备选方法:从覆盖报告中获取单位综合覆盖率(需要人工计算):

- 当 ci_linux\_ coverage 构建完成时,单击 `Cobertura Coverage Report`

- 向下滚动到 `Coverage Breakdown by Package` 表格显示

- 在表格中,第一栏“名称”下,查找(正在测试的包件是\<package_name\>):

  - 图案下的所有目录 `src.*.<repository_name>.<package_name>.*` 抓取“线”栏中的两个绝对值。

  - 图案下的所有目录 `build/.<repository_name>.*` 抓取“线”栏中的两个绝对值。

- 与前一个选择: 对于每个单元格,第一个值是测试的行,第二个值是代码的总行。将所有行集合起来,以获得测试的行总数和测试中的代码总行数。除法以获得覆盖率。

<span id="how-to-measure-coverage-locally-using-lcov-ubuntu"></span> <span id="measure-coverage-locally"></span>

### 如何使用lcov(Ubuntu)在当地测量覆盖率

为了测量您自己的机器的覆盖度,安装 `lcov`.

``` console
$ sudo apt install -y lcov
```

本节的其余部分假设您正在您的 Cocon 工作空间中工作。 用覆盖旗帜编译调试。 请随意使用 Cocon 旗帜瞄准特定软件包 。

``` console
$ colcon build --cmake-args -DCMAKE_BUILD_TYPE=Debug -DCMAKE_CXX_FLAGS="${CMAKE_CXX_FLAGS} --coverage" -DCMAKE_C_FLAGS="${CMAKE_C_FLAGS} --coverage"
```

`lcov` 需要初始基线,您可以用以下命令来生产。为您的需要更新输出文件位置。

``` console
$ lcov --no-external --capture --initial --directory . --output-file ~/ros2_base.info
```

运行对覆盖度测量有重要意义的软件包的测试。例如,如果测量 `rclcpp` 也与 `test_rclcpp`

``` console
$ colcon test --packages-select rclcpp test_rclcpp
```

以类似命令抓取lcov结果, 这次丢弃 `--initial` 旗帜。

``` console
$ lcov --no-external --capture --directory . --output-file ~/ros2.info
```

组合追踪 `.info` 文件 :

``` console
$ lcov --add-tracefile ~/ros2_base.info --add-tracefile ~/ros2.info --output-file ~/ros2_coverage.info
```

生成html以方便可视化和注释覆盖的行.

``` console
$ mkdir -p coverage
$ genhtml ~/ros2_coverage.info --output-directory coverage
```
