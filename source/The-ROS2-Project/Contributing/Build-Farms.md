---
translation_status: machine_translated
source: The-ROS2-Project/Contributing/Build-Farms.rst
---

<span id="ros-build-farms"></span> <span id="buildfarms"></span>

# ROS 构建农场

旱研室建造农场是支持旱研室生态系统的重要基础设施,由旱研室提供和维护。 [Open Robotics](https://www.openrobotics.org/)。它们为ROS 1 和ROS 2 软件包提供源码和二进制软件包的构建、连续集成、测试和分析。开放源码软件包有两个主机实例:

1.  <https://build.ros.org/> ROS 1 软件包

2.  <https://build.ros2.org/> 用于 ROS 2 套件

如果打算使用任何提供的基础设施,请考虑报名参加。 [建立农场讨论论坛](https://discourse.openrobotics.org/c/infrastructure-project/infra-buildfarm/20) 以便收到通知,例如关于即将发生的任何变化的通知。

<span id="jobs-and-deployment"></span>

## 工作与部署

职业介绍所建设的农场有几种不同的工作。对于每种工作类型,您会详细描述他们的工作方式和工作方式:

- [释放任务](https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/release_jobs.rst) 生成二进制软件包,例如,deb软件包

- [隐藏任务](https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/devel_jobs.rst) 在单一储存库内以投票方式建立和测试ROS软件包

- [拖动请求任务(\_R)](https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/devel_jobs.rst) 在webhooks触发的单个寄存器内建立和测试ROS包

- [CI 任务](https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/ci_jobs.rst) 使用其他 CI 任务中的文物来加快构建, 并测试 ROS 软件包

- [医生的工作](https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/doc_jobs.rst) 生成软件包的 API 文档,并从清单中提取信息

- [杂项工作](https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/miscellaneous_jobs.rst) 执行维护任务并生成信息数据,以可视化建筑农场及其生成的文物的状况

<span id="creation-and-deployment"></span>

### 创建和部署

软件包创建和部署上述工作 [开花](http://wiki.ros.org/bloom),即为 ROS 1 或 ROS 2. 一旦开花成功,并在 ROS 的分发中包含一个包(通过拉请求) [rostro 维基月球](https://github.com/ros/rosdistro),将生成相应的任务。这些任务的名称编码其类型和目的 : <span id="id1"></span>[\[1\]](#id5)

- 释放任务 :

  > - `{distro}src_{platf}__{package}__{platform}__source` 构建释放源包
  >
  > - `{distro}bin_{platf}__{package}__{platform}__binary` 构建发行的二进制包
  >
  > 例如,ROS 2 Iron上的rclcpp(运行于Ubuntu Jammy amd64)二进制包装工作被命名为: `Ibin_uJ64__rclcpp__ubuntu_jammy_amd64__binary`.

- 隐藏任务 :

  > - `{distro}dev__{package}__{platform}` 执行释放分支的 CI 构建

- 拖动请求任务(\_R)

  > - `{distro}pr__{package}__{platform}` 为拉动请求执行 CI 构建
  >
  > 例如,ROS 2 Iron上的rclcpp(运行于Ubuntu Jammy amd64)的公关工作被命名为 `Ipr__rclcpp__ubuntu_jammy_amd64`.

<span id="execution"></span>

### 执行

工作的执行取决于工作的类型:

- [隐藏任务](https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/devel_jobs.rst) 每次按规定频率对各分支投票进行承诺时,都会触发。

- [拖动请求任务(\_R)](https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/devel_jobs.rst) 将会被上游相应拉动请求的网络呼号触发 <span id="id2"></span>[\[2\]](#id6) 存储器

- [释放任务](https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/release_jobs.rst) 将在每次发布新软件包版本时即启动,即新软件包 [rostro 维基月球](https://github.com/ros/rosdistro) 此软件包被接受拉请求。 源任务由 rosdistro 分发文件的版本更改触发, 二进制任务由源代码对应器触发 。

<span id="frequency-asked-questions-faq-and-troubleshooting"></span>

## 问问题频率(FAQ)和解决问题

1.  **我得到詹金斯的邮件 失败的建设农场工作; 我该怎么办?**

    转到提出问题的工作。 您可以在 Jenkins 电子邮件上找到链接。 一旦您跟随该链接到构建任务, 请单击 *主控台输出* 左侧,然后单击 *完整日志*。这将给您一个失败的构建的全部控制台输出。 尝试找到最顶级的错误, 因为它通常是最重要的错误, 而其他错误可能是后续错误 。

    电子邮件的底部可能读取 `'apt-src build [...]' failed. This is usually because of an error building the package.` 这通常提示缺失的依赖关系,见2.

2.  **我似乎失去了一个依赖, 我如何找出哪一个?**

    你基本上有两种选择: 选项a比较容易,但可能要经过几次迭代; 选项b比较详细,既能让你充分洞察力,又能让你进行局部调试。

    1.  检查引起这个问题的发布工作(见上文问题),并将cmake依赖性问题本地化。为此,浏览cmake部分,例如,浏览到cmake部分。 *构建二进制数据b* 区域通过菜单左侧,以备Ubuntu/Debian 构建任务。 *CMake 错误* 将通常提示 CMake 配置需要的依赖性,但在 [软件包显示](http://wiki.ros.org/Manifest)。一旦您在清单中确定了依赖性,请重新发布您的软件包,并等待建设农场的反馈或...

    2.  为了获得充分洞察力和更快的局部调试,你可以 [本地运行发布任务](https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/release_jobs.rst#run-the-release-job-locally)。这允许将清单在本地排列,直到所有依赖关系都得到确定。

3.  **为何在破解工作/我的 Github 动作/ 我的本地建筑成功时, 释放工作失败?**

    有几个潜在原因。首先,根据最低限度的ROS安装来释放工作,以检查所有依赖性是否在数据库中正确申报。 [软件包显示](http://wiki.ros.org/Manifest)。Devel job / github 动作 / 本地建筑可以在已安装依赖性的环境中进行,因此不会注意到依赖性问题。第二,它们可能构建不同版本的源代码。Devel job / github 动作 / 本地建筑通常会从中构建最新的版本。 *上游* <span id="id3"></span>[\[2\]](#id6) 存储器, [释放任务](https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/jobs/release_jobs.rst) 构建最新发布的源代码, 即相应的源代码 *上游* 部门一览表 *释放* 存储器 <span id="id4"></span>[\[3\]](#id7).

<span id="further-reading"></span>

## 进一步阅读

以下链接提供了建设农场的更多细节和见解:

- <https://github.com/ros-infrastructure/ros_buildfarm/blob/master/doc/index.rst> - 农场基础设施建设和创造的建筑工作机会的一般文献记录

- <http://wiki.ros.org/regression_tests#Setting_up_Your_Computer_for_Prerelease>

- <http://wiki.ros.org/buildfarm> - ROS维基条目: ROS 1建造农场(部分) *过时*)

- <https://github.com/ros-infrastructure/cookbook-ros-buildfarm> - 安装和配置ROS建造农机

<span id="id5"></span>

\[[1](#id1)\]

`{distro}` 是ROS发行的第一个字母, `{platform}` (`{platf}`将软件包的平台命名为(及其短代码),以及 `{package}` 是正在构建的 ROS 软件包的名称。

<span id="id6"></span>

\[2\] ([1](#id2),[2](#id3))

那个... *上游* 寄存器是包含相应的ROS 1/ROS 2包的原始源代码的寄存器.

<span id="id7"></span>

\[[3](#id4)\]

那个... *释放* 寄存器是ROS 2 基础设施用于释放软件包的寄存器,参见 <https://github.com/ros2-gbp/>.
