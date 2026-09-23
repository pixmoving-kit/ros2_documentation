---
translation_status: machine_translated
source: Installation/Alternatives/Fedora-Development-Setup.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="fedora-source"></span>

# Fedora（源码安装）

<span id="how-to-setup-the-development-environment"></span>

## 如何搭建开发环境?

在 Fedora 上构建 ROS 2 需要下列系统依赖性。它们可以安装在 `dnf` 现将有关事项通知如下:

``` bash
sudo dnf install \
  cmake \
  cppcheck \
  eigen3-devel \
  gcc-c++ \
  liblsan \
  libXaw-devel \
  libyaml-devel \
  make \
  opencv-devel \
  patch \
  python3-colcon-common-extensions \
  python3-coverage \
  python3-devel \
  python3-empy \
  python3-nose \
  python3-pip \
  python3-pydocstyle \
  python3-pyparsing \
  python3-pytest \
  python3-pytest-cov \
  python3-pytest-mock \
  python3-pytest-runner \
  python3-rosdep \
  python3-setuptools \
  python3-vcstool \
  poco-devel \
  poco-foundation \
  python3-flake8 \
  python3-flake8-import-order \
  redhat-rpm-config \
  uncrustify \
  wget
```

有了这个,你可以跟随其余的 [指令](RHEL-Development-Setup.md#rhel-dev-get-ros2-code) 以获取和构建 ROS 2 。
