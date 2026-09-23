根据所用平台，安装后续步骤需要的工具：

**deb 系统（例如 Ubuntu）**

```console
$ sudo apt install python3-bloom python3-catkin-pkg
```

**RPM 系统（例如 RHEL）**

```console
$ sudo dnf install python3-bloom python3-catkin_pkg
```

**其他平台**

```console
$ pip3 install -U bloom catkin_pkg
```

确保已经初始化 rosdep：

```console
$ sudo rosdep init
$ rosdep update
```

如果以前已经初始化过 rosdep，`rosdep init` 命令可能会失败；这种情况下可以忽略该错误。
