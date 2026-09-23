确保使用支持 `UTF-8` 的区域设置。如果处于精简环境中（例如 Docker 容器），区域设置可能是 `C` 之类的最简设置。我们使用以下设置进行测试，但其他支持 UTF-8 的区域设置通常也可以正常使用。

```console
$ locale  # check for UTF-8

$ sudo dnf install langpacks-en glibc-langpack-en
$ export LANG=en_US.UTF-8

$ locale  # verify settings
```
