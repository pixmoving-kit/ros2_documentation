---
translation_status: machine_translated
source: How-To-Guides/Setup-ROS-2-with-VSCode-and-Docker-Container.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="setup-ros-2-with-vscode-and-docker-community-contributed"></span>

# 使用 VSCode 和 Docker 配置 ROS 2（社区贡献）

<span id="install-vs-code-and-docker"></span>

## 安装 VS 代码和 Docker

使用 Visual Studio 代码和 Docker 容器,您可以运行您最喜欢的 ROS 2 分布, 无需更改操作系统或使用虚拟机。 您可以使用此教程设置一个 Docker 容器, 用于您未来的 ROS 2 项目 。

<span id="install-docker"></span>

### 安装嵌入器

要安装嵌入器并设定正确的用户权限,请使用以下命令.

``` console
$ sudo apt install docker.io git python3-pip
$ pip3 install vcstool
$ echo export PATH=$HOME/.local/bin:$PATH >> ~/.bashrc
$ source ~/.bashrc
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
$ newgrp docker
```

现在您可以通过运行以下命令来检查安装是否成功 :

``` console
$ docker run hello-world
```

如果你不能从盒子里跑出来的话, 您可能需要先启动 Docker 守护进程 :

``` console
$ sudo systemctl start docker
```

<span id="install-vs-code"></span>

### 安装 VS 代码

要安装 VS 代码, 请使用以下命令 :

``` console
$ sudo apt update
$ sudo apt install software-properties-common apt-transport-https wget -y
$ wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
$ sudo apt install code
```

您可以通过打字来运行 VS 代码 `code` 在终点站。

<span id="install-remote-development-extension"></span>

### 安装远程开发扩展

VS代码在扩展中搜索(CTRL+SHIFT ⁇ ),用于“远程开发”扩展并安装.

<span id="configure-workspace-in-docker-and-vs-code"></span>

## 在 Docker 和 VS 代码中配置工作空间

<span id="add-your-ros-2-workspace"></span>

### 添加您的 ROS 2 工作空间

添加一个工作空间,以便在容器中构建和打开,例如:

``` console
$ cd ~/
$ mkdir ws
$ cd ws
$ mkdir src
```

现在创建一个 `.devcontainer` 在工作空间的根中创建文件夹并添加一个 `devcontainer.json` 财务报告和财务报告 `Dockerfile` 给这个 `.devcontainer` 文件夹。工作空间结构应该像这样 :

``` default
ws
├── .devcontainer
│   ├── devcontainer.json
│   └── Dockerfile
├── src
    ├── package1
    └── package2
```

与 `File->Open Folder...` 或 时 间 `Ctrl+K Ctrl+O`打开 `ws` VS 代码中的工作空间文件夹。

<span id="edit-devcontainer-json-for-your-environment"></span>

### 编辑 `devcontainer.json` 为您创造环境

为了使Dev容器正常运行,我们必须与正确的用户一起建造它。 `.devcontainer/devcontainer.json`:

``` json
{
    "name": "ROS 2 Development Container",
    "privileged": true,
    "remoteUser": "YOUR_USERNAME",
    "build": {
        "dockerfile": "Dockerfile",
        "args": {
            "USERNAME": "YOUR_USERNAME"
        }
    },
    "workspaceFolder": "/home/ws",
    "workspaceMount": "source=${localWorkspaceFolder},target=/home/ws,type=bind",
    "customizations": {
        "vscode": {
            "extensions":[
                "ms-vscode.cpptools",
                "ms-vscode.cpptools-themes",
                "twxs.cmake",
                "donjayamanne.python-extension-pack",
                "eamodio.gitlens",
                "ms-iot.vscode-ros"
            ]
        }
    },
    "containerEnv": {
        "DISPLAY": "unix:0",
        "ROS_LOCALHOST_ONLY": "1",
        "ROS_DOMAIN_ID": "42"
    },
    "runArgs": [
        "--net=host",
        "--pid=host",
        "--ipc=host",
        "-e", "DISPLAY=${env:DISPLAY}"
    ],
    "mounts": [
       "source=/tmp/.X11-unix,target=/tmp/.X11-unix,type=bind,consistency=cached",
       "source=/dev/dri,target=/dev/dri,type=bind,consistency=cached"
    ],
    "postCreateCommand": "sudo rosdep update && sudo rosdep install --from-paths src --ignore-src -y && sudo chown -R $(whoami) /home/ws/"
}
```

使用 `Ctrl+F` 打开搜索并替换菜单。搜索 `YOUR_USERNAME` 换成你的 `Linux username`。如果您不知道您的用户名,您可以通过运行找到它 `echo $USERNAME` 在终点站。

<span id="edit-dockerfile"></span>

### 编辑 `Dockerfile`

打开 Docker 文件并添加以下内容:

``` bash
FROM ros:ROS_DISTRO
ARG USERNAME=USERNAME
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Delete user if it exists in container (e.g Ubuntu Noble: ubuntu)
RUN if id -u $USER_UID ; then userdel `id -un $USER_UID` ; fi

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y python3-pip
ENV SHELL /bin/bash

# ********************************************************
# * Anything else you want to do like clean up goes here *
# ********************************************************

# [Optional] Set the default user. Omit if you want to keep the default as root.
USER $USERNAME
CMD ["/bin/bash"]
```

替换 `ROS_DISTRO` 带有 ROS 2 分布图,您希望作为上方的基础图像使用,例如 `rolling`.

<span id="open-and-build-development-container"></span>

## 开放和建设发展集装箱

使用 `View->Command Palette...` 或 时 间 `Ctrl+Shift+P` 打开命令调色板。搜索命令 `Dev Containers: Reopen in Container` 并执行它。这将为您建立您的开发容器。它需要一段时间 - 退后或去喝咖啡。

<span id="test-container"></span>

### 测试容器

为了测试是否一切顺利,在容器中打开一个终端。 `View->Terminal` 或 时 间 `` Ctrl+Shift+` `` 财务报告和财务报告 `New Terminal` 在 VS 代码中。在终端内进行下列操作:

``` console
$ sudo apt install ros-$ROS_DISTRO-rviz2 -y
$ source /opt/ros/$ROS_DISTRO/setup.bash
$ rviz2
```

> **说明**
>
> 显示 RVIZ 可能有问题。 请确保允许用户访问 X 窗口系统 。 `xhost +local:<USERNAME>`。如果没有窗口出现,则检查其值。 `echo $DISPLAY` - 如果输出为 1, 您可以用 `echo "export DISPLAY=unix:1" >> /etc/bash.bashrc` ,然后再次测试。您也可以在 devcontainer.json中更改 DIPLAY 值并重建它。
