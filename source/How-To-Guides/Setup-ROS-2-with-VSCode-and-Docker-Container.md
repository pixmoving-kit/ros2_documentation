<span id="setup-ros-2-with-vscode-and-docker-community-contributed"></span>
# 使用 VS Code 和 Docker 配置 ROS 2（社区贡献）

<span id="install-vs-code-and-docker"></span>
## 安装 VS Code 和 Docker

借助 Visual Studio Code 和 Docker 容器，可以运行所需的 ROS 2 发行版，无须更换操作系统或使用虚拟机。本教程将帮助你配置一个可供后续 ROS 2 项目使用的 Docker 容器。

<span id="install-docker"></span>
### 安装 Docker

执行以下命令安装 Docker 并设置用户权限：

```console
$ sudo apt install docker.io git python3-pip
$ pip3 install vcstool
$ echo export PATH=$HOME/.local/bin:$PATH >> ~/.bashrc
$ source ~/.bashrc
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
$ newgrp docker
```

运行以下命令，检查安装是否成功：

```console
$ docker run hello-world
```

如果无法直接运行 hello-world，可能需要先启动 Docker 守护进程：

```console
$ sudo systemctl start docker
```

<span id="install-vs-code"></span>
### 安装 VS Code

使用以下命令安装 VS Code：

```console
$ sudo apt update
$ sudo apt install software-properties-common apt-transport-https wget -y
$ wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
$ sudo apt install code
```

在终端中输入 `code` 即可运行 VS Code。

<span id="install-remote-development-extension"></span>
### 安装 Remote Development 扩展

在 VS Code 的扩展视图（`Ctrl+Shift+X`）中搜索并安装 “Remote Development” 扩展。

<span id="configure-workspace-in-docker-and-vs-code"></span>
## 在 Docker 和 VS Code 中配置工作空间

<span id="add-your-ros-2-workspace"></span>
### 添加 ROS 2 工作空间

创建一个工作空间，以便在容器中打开和构建，例如：

```console
$ cd ~/
$ mkdir ws
$ cd ws
$ mkdir src
```

在工作空间根目录创建 `.devcontainer` 文件夹，并在其中添加 `devcontainer.json` 和 `Dockerfile`。工作空间结构应如下：

```text
ws
├── .devcontainer
│   ├── devcontainer.json
│   └── Dockerfile
├── src
    ├── package1
    └── package2
```

通过 `File -> Open Folder...` 或 `Ctrl+K Ctrl+O`，在 VS Code 中打开工作空间的 `ws` 文件夹。

<span id="edit-devcontainer-json-for-your-environment"></span>
### 按环境修改 devcontainer.json

要让开发容器正常工作，需要使用正确的用户构建它。在 `.devcontainer/devcontainer.json` 中添加以下内容：

```json
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

按 `Ctrl+F` 打开搜索和替换菜单，查找 `YOUR_USERNAME` 并替换为你的 Linux 用户名。如果不知道用户名，可以在终端中运行 `echo $USERNAME` 查看。

<span id="edit-dockerfile"></span>
### 编辑 Dockerfile

打开 Dockerfile，添加以下内容：

```bash
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

将 `ROS_DISTRO` 替换为希望用作基础镜像的 ROS 2 发行版，例如 `rolling`。

<span id="open-and-build-development-container"></span>
## 打开并构建开发容器

通过 `View -> Command Palette...` 或 `Ctrl+Shift+P` 打开命令面板。搜索并执行 `Dev Containers: Reopen in Container`。这会构建你的 Docker 开发容器，需要稍等一会儿。

<span id="test-container"></span>
### 测试容器

要检查配置是否成功，在 VS Code 中通过 `View -> Terminal` 或 ``Ctrl+Shift+` `` 打开终端面板，再选择 `New Terminal`，创建容器内终端。执行：

```console
$ sudo apt install ros-$ROS_DISTRO-rviz2 -y
$ source /opt/ros/$ROS_DISTRO/setup.bash
$ rviz2
```

!!! note "说明"
    RViz 可能无法显示。请执行 `xhost +local:<USERNAME>`，允许该用户访问 X 窗口系统。如果仍未弹出窗口，检查 `echo $DISPLAY` 的值；如果输出为 1，可以执行 `echo "export DISPLAY=unix:1" >> /etc/bash.bashrc`，然后重新测试。也可以修改 `devcontainer.json` 中的 DISPLAY 值并重新构建。
