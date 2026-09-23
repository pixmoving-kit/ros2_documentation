---
translation_status: machine_translated
source: Installation/_Ubuntu-Set-Locale.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

确定您有支持的地址 `UTF-8`。如果您处于一个最小的环境(例如一个插座容器),那么当地可能就是最小的环境,比如: `POSIX`。我们用以下设置进行测试。但是,如果您使用不同的UTF-8支持的语境,则应该没问题。

``` console
$ locale  # check for UTF-8

$ sudo apt update && sudo apt install locales
$ sudo locale-gen en_US en_US.UTF-8
$ sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
$ export LANG=en_US.UTF-8

$ locale  # verify settings
```
