---
translation_status: machine_translated
source: How-To-Guides/Releasing/_Install-Dependencies.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

安装您将在即将到来的步骤中根据您的平台使用的工具 :

##### db(例如Ubuntu) (中文(简体) ).

``` console
$ sudo apt install python3-bloom python3-catkin-pkg
```

##### RPM(例如,RHEL)

``` console
$ sudo dnf install python3-bloom python3-catkin_pkg
```

##### 其他人员

``` console
$ pip3 install -U bloom catkin_pkg
```

确定您已经初始化 :

``` console
$ sudo rosdep init
$ rosdep update
```

请注意, `rosdep init` 命令如果在过去已经初始化,则可能失败;这可以安全地忽略。
