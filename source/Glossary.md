---
translation_status: machine_translated
source: Glossary.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="glossary"></span>

# 术语表

本文件所用术语汇编:

<span id="term-API"></span>API  
API,或称应用程序编程接口,是由“应用程序”提供的接口,在这种情况下通常是一个共享的库或其他适当的语言共享资源。API是由一些文件组成的,这些文件定义了使用接口的软件与提供接口的软件之间的合同。这些文件通常表现为C和C++以及Python文件中的页眉文件。在这两种情况下,重要的是API在文档中被分组和描述,并且被宣布为公用或私用。公共接口必须修改规则,并更改公共接口,从而触发提供它们软件的新版本编号。

<span id="term-client_library"></span>client_library  
客户端库是一个 [API](#term-API) 使用诸如Topics,Services, and Actions等原始的中间软件概念提供ROS图的存取.

<span id="term-package"></span>软件包  
单单元软件,包括源代码,构建系统文件,文档,测试,以及其他有关资源.

<span id="term-REP"></span>REP  
机器人增强建议:一个描述ROS社区加强、标准化或公约的文件,相关的REP批准程序允许社区在达成某种共识之前对某项提案进行推敲,届时可予以批准和执行,然后成为文件。 [REP 指数](https://reps.openrobotics.org/).

<span id="term-VCS"></span>越共  
版本控制系统,如CVS,SVN,git,mercurial等.

<span id="term-rclcpp"></span>rclcpp  
C++ 具体 [客户端库](#term-client_library) 。这包括任何与中间软件相关的API,以及基于Messages、Services和Action等接口定义的 C++ 数据结构的相关消息生成。

<span id="term-repository"></span>存储器  
一组软件包通常使用一个 [越共](#term-VCS) 如 git 或 mercurial , 通常托管在 GitHub 或 BitBucket 等站点上。 在本文档中, 寄存器通常包含一个或多个 [软件包](#term-package) 一种或另一种类型。
