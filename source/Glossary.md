<span id="glossary"></span>

# 术语表

本文档中使用的术语如下。

<span id="term-API"></span>

## API（应用程序编程接口）

API 是 Application Programming Interface 的缩写，指“应用程序”提供的接口。这里的应用程序通常是共享库，或相应编程语言中的其他共享资源。API 由一组文件组成，定义使用接口的软件与提供接口的软件之间的约定。在 C 和 C++ 中，这些文件通常是头文件，在 Python 中则是 Python 文件。无论哪种情况，都应在文档中对 API 进行分组和说明，并声明接口是公开的还是私有的。公开接口须遵循变更规则；修改公开接口时，提供该接口的软件需要更新版本号。

<span id="term-client_library"></span>

## client_library（客户端库）

客户端库是一种 [API](#term-API)，通过话题、服务、动作等基础中间件概念提供对 ROS 计算图的访问。

<span id="term-package"></span>

## package（软件包）

独立的软件单元，包含源代码、构建系统文件、文档、测试和其他相关资源。

<span id="term-REP"></span>

## REP（机器人增强提案）

REP 是 Robotics Enhancement Proposal 的缩写，指描述 ROS 社区某项改进、标准化方案或约定的文档。REP 审批流程让社区能够反复完善提案，直到形成一定共识，随后批准并实施，最终成为文档。所有 REP 均可在 [REP 索引](https://reps.openrobotics.org/)中查看。

<span id="term-VCS"></span>

## VCS（版本控制系统）

VCS 是 Version Control System 的缩写，例如 CVS、SVN、git、mercurial 等。

<span id="term-rclcpp"></span>

## rclcpp

ROS 的 C++ [客户端库](#term-client_library)。它包含与中间件有关的 API，以及根据消息、服务和动作等接口定义生成相应 C++ 数据结构的功能。

<span id="term-repository"></span>

## repository（仓库）

一组软件包的集合，通常通过 git、mercurial 等[版本控制系统](#term-VCS)管理，并托管在 GitHub、BitBucket 等网站上。在本文档中，仓库通常包含一个或多个软件包，类型不限。
