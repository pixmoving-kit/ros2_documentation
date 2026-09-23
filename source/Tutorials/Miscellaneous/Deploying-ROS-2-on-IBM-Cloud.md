---
translation_status: machine_translated
source: Tutorials/Miscellaneous/Deploying-ROS-2-on-IBM-Cloud.rst
---

<span id="deploying-on-ibm-cloud-kubernetes-community-contributed"></span>

# 部署到 IBM Cloud Kubernetes（社区贡献）

<span id="about"></span>

## 关于

这篇文章描述了如何让ROS 2 使用 Docker 文件运行在 IBM Cloud 上。 它首先简要概述了 Docker 图像以及它们如何在当地工作, 然后探索 IBM Cloud 以及用户如何在它上部署容器。 之后, 简短地描述了用户如何从 IBM Cloud 上的 github 使用自己的 ROS 2 的自定义包。 它提供了如何在 IBM Cloud 上创建集群并利用 Kubernetes 的流程, 最后在集群上部署了 Docker 图像 。 最初已经发布过 。 [这儿](https://github.com/mm-nasr/ros2_ibmcloud) 财务报告和财务报告 [这儿](https://medium.com/@mahmoud-nasr/running-ros2-on-ibm-cloud-1b1284cbd487).

<span id="ros-2-on-ibm-cloud"></span>

## IBM云上的ROS 2

在这个教程中,我们显示您如何用您的自定义软件包在IBM Cloud上方便地集成和运行ROS 2.

ROS 2 是新一代的ROS,它赋予了对多机器人阵型的更多控制。 随着云计算的进步,云机器人在当今时代正变得越来越重要。在这个教程中,我们将通过一个简短的IBM Cloud上运行ROS 2的介绍。到教程结束时,您就可以在ROS 2中创建自己的软件包,并使用 docker 文件将其部署到云中。

以下指令假设您使用Linux, 并已与Ubuntu 18.04 (Bionic Beaver)进行测试。

<span id="step-1-setting-up-your-system"></span>

## 步骤1:建立你的系统

在进入确切过程之前,首先让我们确定所有需要的软件都得到了适当的安装。 我们将引导您找到适当的来源来建立您的系统,并且只强调与我们的使用案例相关的细节。

<span id="a-docker-files"></span>

### (a) Docker文件?

Docker文件是一种可以从您的系统中分开运行的容器形式, 这样, 您可以在不互相影响的情况下设置数百个不同的工程。 您甚至可以在一台机器上设置不同的 Linux 版本, 而不需要虚拟机器 。 Docker 文件具有节省空间的优势, 并且只在运行时使用您的系统资源 。 此外, 码头是多功能的, 并且可以转移。 它们包含所有需要的单独运行的预先要求, 这意味着您可以轻松地使用一个 Docker 文件来进行特定的系统或服务, 而无需任何立体步骤 !

兴奋吗?让我们首先在您的系统中安装插头,然后遵循以下操作: [链接](https://docs.docker.com/get-docker/)。从教程中,您应该做一些理智的检查,以确保嵌入器的设置是适当的。但是,以防万一,让我们再次运行使用 Hello-world 嵌入器图像的以下命令:

``` console
$ sudo docker run hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

<span id="b-ros-2-image"></span>

### (b) ROS 2图像

ROS [已公布](https://discourse.openrobotics.org/t/announcing-official-docker-images-for-ros2/7381/2) 2019年1月多个ROS发行版本的图像容器. 有关ROS 2 插头图像使用的更详细的说明可以找到 [这儿](https://hub.docker.com/_/ros/).

让我们从中跳过, 立即实现真正的交易; 创建一个本地 ROS 2 连接器。 我们将创建我们自己的多克文件( 而不是使用已准备好的图像 ) , 因为我们需要在 IBM Cloud 上部署这种方法。 首先, 我们创建一个新的目录, 它将保存我们的多克文件和其它我们需要的文件, 并浏览到它。 使用您最喜欢的 \$EDITOR 选项, 打开一个新的文件, 命名为 。 *粘贴文件* (确保文件命名正确):

``` console
$ mkdir ~/ros2_docker

$ cd ~/ros2_docker

$ $EDITOR Dockerfile
```

在表格中插入以下内容: *粘贴文件*,并保存它(也发现了) [这儿](https://github.com/mm-nasr/ros2_ibmcloud/blob/main/dockers/ros2_basic/Dockerfile)):

``` bash
FROM ros:foxy

# install ros package
RUN apt-get update && apt-get install -y \
      ros-${ROS_DISTRO}-demo-nodes-cpp \
      ros-${ROS_DISTRO}-demo-nodes-py && \
    rm -rf /var/lib/apt/lists/* && mkdir /ros2_home

WORKDIR /ros2_home

# launch ros package
CMD ["ros2", "launch", "demo_nodes_cpp", "talker_listener.launch.py"]
```

- **从**: 从 ros: foxy Docker 图像创建一层

- **运行**:通过在容器中安装vim并创建名为/ros2_home的目录来构建您的容器

- **工 作 员**: 通知容器工作目录应放在何处

当然,你可以自由地改变ROS的分布(*帅哥* ) 或更改目录名称。上面的嵌入器文件设置了 ROS- foxy 并安装了 C++ 和 Python 的演示节点。然后它会启动一个文件,运行一个谈话器和一个听者节点。我们将在操作中看到它,但是它们的行为与在操作中发现的发布器订阅器实例非常相似。 [ROS 维基](https://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28c%2B%2B%29)

现在,我们准备建造一个插座图像来运行 ROS 2(是的,这是那么容易!).

**说明**:如果由于权限不足而出现错误,或者 *拒绝权限*,尝试运行命令 *苏度* 权限 :

``` console
$ docker build .

~ You will see a bunch of lines that execute the docker file instructions followed by:

Successfully built 0dc6ce7cb487
```

*0dc6ce7cb487 (韩语).* 您很可能会有所不同, 所以请记住并复制到某个地方, 以供参考 。 您总是可以返回并检查您系统中的嵌入图像 :

``` console
$ sudo docker ps -as
```

现在, 使用 :

``` console
$ docker run -it 0dc6ce7cb487
[INFO] [launch]: All log files can be found below /root/.ros/log/2020-10-28-02-41-45-177546-0b5d9ed123be-1
[INFO] [launch]: Default logging verbosity is set to INFO
[INFO] [talker-1]: process started with pid [28]
[INFO] [listener-2]: process started with pid [30]
[talker-1] [INFO] [1603852907.249886590] [talker]: Publishing: 'Hello World: 1'
[listener-2] [INFO] [1603852907.250964490] [listener]: I heard: [Hello World: 1]
[talker-1] [INFO] [1603852908.249786312] [talker]: Publishing: 'Hello World: 2'
[listener-2] [INFO] [1603852908.250453386] [listener]: I heard: [Hello World: 2]
[talker-1] [INFO] [1603852909.249882257] [talker]: Publishing: 'Hello World: 3'
[listener-2] [INFO] [1603852909.250536089] [listener]: I heard: [Hello World: 3]
[talker-1] [INFO] [1603852910.249845718] [talker]: Publishing: 'Hello World: 4'
[listener-2] [INFO] [1603852910.250509355] [listener]: I heard: [Hello World: 4]
[talker-1] [INFO] [1603852911.249506058] [talker]: Publishing: 'Hello World: 5'
[listener-2] [INFO] [1603852911.250152324] [listener]: I heard: [Hello World: 5]
[talker-1] [INFO] [1603852912.249556670] [talker]: Publishing: 'Hello World: 6'
[listener-2] [INFO] [1603852912.250212678] [listener]: I heard: [Hello World: 6]
```

如果操作正确, 您应该看到类似上面显示的东西。 可以看到有两个ROS节点( 出版商和订户) 运行, 其输出通过ROS INFO 提供给我们 。

<span id="step-2-running-the-image-on-ibm-cloud"></span>

## 第二步:在IBM云上运行图像

以下步骤假设您有一个 IBM 云账户并安装了 ibmcloud CLI 。 如果没有, 请检查此选项 [链接](https://cloud.ibm.com/docs/cli/reference/ibmcloud/download_cli.html) 先把事情办好

我们还需要确保IBM云容器登记册的CLI插件通过运行指令安装

``` console
$ ibmcloud plugin install container-registry
```

之后,通过终端登录您的 ibmcloud 账户:

``` console
$ ibmcloud login --sso
```

从这里开始,让我们创建一个容器注册名称空间。请确保您使用一个独有的名称,这个名称也是描述它是什么的。这里,我用了 *罗斯2纳*.

``` console
$ ibmcloud cr namespace-add ros2nasr
```

IBM 云有很多快捷键, 帮助我们立即将容器放入云中。 下面的命令构建容器, 并用名称标记它 。 **ros2foxy (英语).** 和该版本 **1**。确保您使用您创建的正确的注册名称,并且您可以随意更改容器名称。 `.` 结尾处显示: *粘贴文件* 在当前目录中(而且重要的是),如果不是,则修改以指向包含 Docker 文件的目录。

``` console
$ ibmcloud cr build --tag registry.bluemix.net/ros2nasr/ros2foxy:1 .
```

您可以通过运行以下命令, 确定容器已被推入您创建的注册 。

``` console
$ ibmcloud cr image-list
Listing images...

REPOSITORY               TAG   DIGEST         NAMESPACE   CREATED         SIZE     SECURITY STATUS
us.icr.io/ros2nasr/ros2foxy   1     031be29301e6   ros2nasr    36 seconds ago   120 MB   No Issues

OK
```

其次,必须登录到您的注册簿上来运行嵌入器图像。再次,如果你面临一个 *拒绝权限* 错误, 使用 sudo 权限执行命令 。 之后, 运行您的嵌入器文件如下 。

``` console
$ ibmcloud cr login
Logging in to 'registry.ng.bluemix.net'...
Logged in to 'registry.ng.bluemix.net'.
Logging in to 'us.icr.io'...
Logged in to 'us.icr.io'.

OK

$ docker run -v -it registry.ng.bluemix.net/ros2nasr/ros2foxy:1
```

何处 *罗斯2纳* 是您创建的注册簿的名称, *ros2foxy:1 (韩语).* 是先前解释过的嵌入器容器和版本的标记。

您现在应该看到您的嵌入器文件运行, 并提供与您在您的机器上运行时所看到的相似的输出 。

<span id="step-3-using-custom-ros-2-packages"></span>

## 第3步:使用自定义ROS 2 套件

因此,现在我们有完整的管道工作,从创建Dockerfile,一直到部署它和看到它在IBM Cloud上起作用。 但是,如果我们想使用我们(或其他人)创建的一套自定义的软件包呢?

这都与你如何设置你的Docker文件有关。让我们用ROS 2提供的例子 [这儿](https://hub.docker.com/_/ros/)创建新目录, 并添加以下内容( 或下载文件) 。 [这儿](https://github.com/mm-nasr/ros2_ibmcloud/blob/main/dockers/git_pkgs_docker/Dockerfile))

``` bash
ARG FROM_IMAGE=ros:foxy
ARG OVERLAY_WS=/opt/ros/overlay_ws

# multi-stage for caching
FROM $FROM_IMAGE AS cacher

# clone overlay source
ARG OVERLAY_WS
WORKDIR $OVERLAY_WS/src
RUN echo "\
repositories: \n\
  ros2/demos: \n\
    type: git \n\
    url: https://github.com/ros2/demos.git \n\
    version: ${ROS_DISTRO} \n\
" > ../overlay.repos
RUN vcs import ./ < ../overlay.repos

# copy manifests for caching
WORKDIR /opt
RUN mkdir -p /tmp/opt && \
    find ./ -name "package.xml" | \
      xargs cp --parents -t /tmp/opt && \
    find ./ -name "COLCON_IGNORE" | \
      xargs cp --parents -t /tmp/opt || true

# multi-stage for building
FROM $FROM_IMAGE AS builder

# install overlay dependencies
ARG OVERLAY_WS
WORKDIR $OVERLAY_WS
COPY --from=cacher /tmp/$OVERLAY_WS/src ./src
RUN . /opt/ros/$ROS_DISTRO/setup.sh && \
    apt-get update && rosdep install -y \
      --from-paths \
        src/ros2/demos/demo_nodes_cpp \
        src/ros2/demos/demo_nodes_py \
      --ignore-src \
    && rm -rf /var/lib/apt/lists/*

# build overlay source
COPY --from=cacher $OVERLAY_WS/src ./src
ARG OVERLAY_MIXINS="release"
RUN . /opt/ros/$ROS_DISTRO/setup.sh && \
    colcon build \
      --packages-select \
        demo_nodes_cpp \
        demo_nodes_py \
      --mixin $OVERLAY_MIXINS

# source entrypoint setup
ENV OVERLAY_WS $OVERLAY_WS
RUN sed --in-place --expression \
      '$isource "$OVERLAY_WS/install/setup.bash"' \
      /ros_entrypoint.sh

# run launch file
CMD ["ros2", "launch", "demo_nodes_cpp", "talker_listener.launch.py"]
```

通过显示的行,我们可以看到如何在4个步骤中从github中添加自定义包:

1.  从 Github 复制的自定义软件包创建覆盖 :

``` bash
ARG OVERLAY_WS
WORKDIR $OVERLAY_WS/src
RUN echo "\
repositories: \n\
  ros2/demos: \n\
    type: git \n\
    url: https://github.com/ros2/demos.git \n\
    version: ${ROS_DISTRO} \n\
" > ../overlay.repos
RUN vcs import ./ < ../overlay.repos
```

2.  使用 rosdep 安装软件包依赖性

``` bash
# install overlay dependencies
ARG OVERLAY_WS
WORKDIR $OVERLAY_WS
COPY --from=cacher /tmp/$OVERLAY_WS/src ./src
RUN . /opt/ros/$ROS_DISTRO/setup.sh && \
    apt-get update && rosdep install -y \
      --from-paths \
        src/ros2/demos/demo_nodes_cpp \
        src/ros2/demos/demo_nodes_py \
      --ignore-src \
    && rm -rf /var/lib/apt/lists/*
```

3.  构建软件包 *需要帮助吗?*

``` bash
# build overlay source
COPY --from=cacher $OVERLAY_WS/src ./src
ARG OVERLAY_MIXINS="release"
RUN . /opt/ros/$ROS_DISTRO/setup.sh && \
    colcon build \
      --packages-select \
        demo_nodes_cpp \
        demo_nodes_py \
      --mixin $OVERLAY_MIXINS
```

4.  运行发射文件

``` bash
# run launch file
CMD ["ros2", "launch", "demo_nodes_cpp", "talker_listener.launch.py"]
```

同样,我们也可以改变所使用的软件包,安装它们的依赖性,然后运行它们.

**回到IBM云层**

通过这个 Docker 文件, 我们可以遵循我们之前在 IBM Cloud 上部署它时所用的步骤。 既然我们已经创建了我们的注册簿, 并且已经登录到 IBM Cloud 中, 我们直接构建了新的 Docker 文件 。 注意到我如何保持标签不变, 但修改了版本, 这样我就可以更新先前创建的 Docker 图像 。 ( 如果您愿意, 您可以自由创建全新的 )

``` console
$ ibmcloud cr build --tag registry.bluemix.net/ros2nasr/ros2foxy:2 .
```

然后,确保您登录到注册处并运行新的嵌入器图像 :

``` console
$ ibmcloud cr login
Logging in to 'registry.ng.bluemix.net'...
Logged in to 'registry.ng.bluemix.net'.
Logging in to 'us.icr.io'...
Logged in to 'us.icr.io'.

OK

$ docker run -v -it registry.ng.bluemix.net/ros2nasr/ros2foxy:2
```

您应该再次看到同样的输出。 然而, 这次我们是通过 Github 的自定义包完成的, 这样我们就可以在 IBM Cloud 上使用我们个人为 ROS 2 创建的软件包 。

<span id="extra-deleting-docker-images"></span>

### 额外: 删除嵌入器图像

由于你可能发现自己需要从IBM Cloud中删除一个特定的嵌入器图像,你应该这样做!

1.  列出所有您拥有的图像并找到所有共享的图像 *图像* 对应名称 *registry.ng.bluemix.net/ros2nasr/ros2foxy:2* 然后用他们的语言去删除它们。 *名称*

``` console
$ docker rm your_docker_NAMES
```

2.  从 IBM 云 中删除插头图像 *图像* 名称

``` console
$ docker rmi registry.ng.bluemix.net/ros2nasr/ros2foxy:2
```

<span id="step-4-kubernetes"></span>

## 步骤4:Kubernetes

<span id="a-creating-the-cluster"></span>

### (a) 建立集群

使用 Console 创建集群。 找到指令 [这儿](https://cloud.ibm.com/docs/containers?topic=containers-clusters#clusters_ui)的设置。这些设置只是建议,如果需要可以修改。但是,请确保您了解您选择的含义:

1.  计划: *标准*

2.  管弦乐处: *库贝尔涅兹 v1.18.10*

3.  基础设施: *经典*

4.  地点 :

- 资源组: *默认*

- 地理学 : *北美* (你自由改变这个)

- 可用性 : *单区* (您可以随意更改, 但通过检查 IBM Cloud 文档确保您了解您选择的影响 。 )

- 工人区: *多伦多01* (选择你身体上最接近的地方)

5.  工人池:

- 虚拟----共享,Ubuntu 18

- 内存: 16GB

- 每个区的工人节点: *1*

6.  主服务端点 : *私有和公有终点*

7.  资源细节(完全灵活):

- 组名 : *我的组群 -tor01 -rosibm*

- 标记 : *版本 : 1*

创建集群后,您将被重定向到一个页面,该页面将详细介绍您如何设置 CLI 工具并访问您的集群。 请遵循这些指令( 或者检查指令) 。 [这儿](https://github.com/mm-nasr/ros2_ibmcloud/blob/main/Kubernetes-Cluster-Set-up.md)),并等待进度栏显示您创建的工人节点已经准备好,通过表示 *常规* 。您也可以从Kubernetes内部的 IBM 云控制台访问此屏幕。

<span id="b-deploying-your-docker-image-finally"></span>

### (b) 部署您的嵌入器图像 *终于来了!*

1.  创建名为 yaml 文件的部署配置 *ros2-deployment.yaml* 使用您最喜欢的 \$EDITOR 并插入以下内容:

``` bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: <deployment>
spec:
  replicas: <number_of_replicas>
  selector:
    matchLabels:
      app: <app_name>
  template:
    metadata:
      labels:
        app: <app_name>
    spec:
      containers:
      - name: <app_name>
        image: <region>.icr.io/<namespace>/<image>:<tag>
```

您应该替换显示在 *“\<” “\>”* 说明 [这儿](https://cloud.ibm.com/docs/containers?topic=containers-images#namespace). 我的案子的档案看起来是这样的:

``` bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ros2-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ros2-ibmcloud
  template:
    metadata:
      labels:
        app: ros2-ibmcloud
    spec:
      containers:
      - name: ros2-ibmcloud
        image: us.icr.io/ros2nasr/ros2foxy:2
```

使用以下命令部署文件

``` console
$ kubectl apply -f ros2-deployment.yaml
deployment.apps/ros2-deployment created
```

现在,你的插头图像 已经完全部署在您的集群上!

<span id="step-5-using-cli-for-your-docker-image"></span>

## 步骤 5: 使用 CLI 显示您的 Docker 图像

1.  通过IBM云控制台Kubernetes导航到你的集群.

2.  点击 *Kubernetes 仪表板* 在页面右上角。

您现在应该能够看到一个完整列表, 列出您集群的所有不同参数及其CPU和内存使用 。

3.  导航到 *投球数* 并点击您的部署。

4.  右上角单击 *执行到吊舱*

您现在在您的插头图像中 ! 您可以找到您的工作空间( 如果需要) 并运行 ROS 2 。 例如 :

``` console
root@ros2-deployment-xxxxxxxx:/opt/ros/overlay_ws# . install/setup.sh
root@ros2-deployment-xxxxxxxx:/opt/ros/overlay_ws# ros2 launch demo_nodes_cpp talker_listener.launch.py
```

<span id="final-remarks"></span>

## 最后意见

此时, 您可以在 github 上使用 ROS 2 软件包创建自己的嵌入器图像。 也可以使用 ROS 2 软件包, 但很少修改, 也可以使用本地的 ROS 2 软件包 。 这可能是另一篇文章的主题。 但是, 鼓励您检查以下内容 : [粘贴文件](https://github.com/mm-nasr/ros2_ibmcloud/tree/main/dockers/local_pkgs_docker) 它使用演示文稿库的本地副本。类似地,您也可以使用自己的本地软件包。
