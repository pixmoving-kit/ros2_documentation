确保使用支持 `UTF-8` 的区域设置。如果处于精简环境中（例如 Docker 容器），区域设置可能是 `POSIX` 之类的最简设置。我们使用以下设置进行测试，但其他支持 UTF-8 的区域设置通常也可以正常使用。

```console
$ locale  # check for UTF-8

$ sudo apt update && sudo apt install locales
$ sudo locale-gen en_US en_US.UTF-8
$ sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
$ export LANG=en_US.UTF-8

$ locale  # verify settings
```
