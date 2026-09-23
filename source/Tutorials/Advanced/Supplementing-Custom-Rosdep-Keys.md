---
translation_status: machine_translated
source: Tutorials/Advanced/Supplementing-Custom-Rosdep-Keys.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="supplementing-custom-rosdep-keys"></span>

# 补充自定义 rosdep 键

<span id="overview-and-motivation"></span>

## 概况和动机

如所述 [使用 rosdep 管理依赖](../Intermediate/Rosdep.md), `rosdep` 寻找 rosdep 密钥在其中 `package.xml` 文件并把它们映射到要为 ROS 分发和正在使用的 OS 安装的软件包中。任何人都可以请求新建 rosdep 密钥,由 [向 rosdistro 捐款](https://github.com/ros/rosdistro/blob/master/CONTRIBUTING.md#rosdep-rules-contributions)。这是在您有某种依赖关系(例如: `apt` 或 时 间 `pip` 软件包),您希望能够通过 `rosdep`.

然而,直接提供您的密钥时会遇到许多困难。例如,如果依赖性

1.  目标分布图默认 APT( 或 pip) 寄存器上无法获取

2.  是一个专有库

3.  仅对您或您的组织有用的特殊库

4.  是一个ROS 软件包 [{\fn黑体\fs20\shad2\2aH82\3aH20\4aH33\fscx95\3cH592001\be1}自己造的和包装的 {\fn黑体\fs20\shad2\2aH82\3aH20\4aH33\fscx95\3cH592001\be1}你已经准备好了](../../How-To-Guides/Building-a-Custom-Deb-Package.md),但不希望与更广泛的ROS社区分享

虽然有选择 [叉罗盘](../../How-To-Guides/Using-Custom-Rosdistro.md) 如果您想要像往常一样继续使用正式的 ROS 分布, 只使用一些额外的 rosdep 密钥定义, 则这可能会是过度的 kill 。 此教程解释了如何实现 。

但作为警告,请不要不加区别地使用它。 这可能很难调试问题,因为二进制的不兼容性可以表现为沉默的失败、无法解释的崩溃或数据腐败。 如果你在系统上向任何人寻求帮助,请确保解释所有被添加或被推翻的东西。

<span id="preliminaries-how-rosdep-fetches-rosdep-keys"></span>

## 初步说明:如何执行 `rosdep` 获取 rosdep 密钥

为了很好地了解我们将要做的事情,让我们首先探索一些有关如何实现的关联细节。 `rosdep` 工作时。

`rosdep` 与其他工具类似,例如: `apt` ,用于使用源列表来维护本地索引。这些源被存储在 `/etc/ros/rosdep/sources.list.d`。这类似于如何将存储器存储在 `/etc/apt/sources.list.d`.

默认(作为首次设置的一部分, `rosdep init`),您只有单个源文件: `/etc/ros/rosdep/sources.list.d/20-default.list`。查看其内容后,您可以看到这样的条目:

``` console
$ cat /etc/ros/rosdep/sources.list.d/20-default.list
...
yaml https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/base.yaml
yaml https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/python.yaml
...
```

这些条目决定了哪里 `rosdep` 获取 rosdep 密钥及其映射( rosdep) **规则**从何时开始 `rosdep update` 被调用时, `rosdep` 将所有源文件中所有已声明的条目的相关内容汇编成本地缓存索引。然后在安装或查询(“resolution”) rosdep 密钥时使用此本地索引。

举例来说,第一个条目(`base.yaml`定义 `libopencv-dev` 键(参见 [这儿](https://github.com/ros/rosdistro/blob/72f24d6/rosdep/base.yaml#L5240-L5252)这是允许的。 `rosdep` 解决它:

``` console
$ rosdep where-defined libopencv-dev
https://raw.githubusercontent.com/ros/rosdistro/master/rosdep/base.yaml
$ rosdep resolve libopencv-dev
#apt
libopencv-dev
```

简而言之,它允许 `rosdep` 解决贫穷问题 `libopencv-dev` 键 `apt` 同一名称的软件包。

请注意,从上面的输出,我们可以推断命令是在Ubuntu或Debian OS上运行的。在 RHEL 上,密钥会解决 DNF 软件包 `opencv-devel`:

``` console
$ rosdep resolve libopencv-dev --os=rhel:9
#dnf
opencv-devel
```

<span id="extending-rosdep-with-a-custom-sources-file"></span>

## 扩展 `rosdep` 带有自定义源文件

以上希望能说明需要做些什么才能取得 `rosdep` 来理解新密钥 : 添加一个新的自定义源文件 !

作为玩具的例子,让我们添加一个新的源文件来说明 `rosdep` 从存储在本地机上的 YAML 文件中获取密钥。将您最喜欢的文本编辑器点火并写入以下 `/etc/ros/rosdep/sources.list.d/30-custom.list` (编辑器需要以根权限启动,例如通过) `sudo`):

``` yaml
yaml file:///etc/ros/rosdep/custom_rules.yaml
```

现在将以下内容写入 `/etc/ros/rosdep/custom_rules.yaml`:

``` yaml
awesome_library:
  ubuntu: [awesome_library]
that_other_library:
  ubuntu:
    pip:
      packages: [another_library]
```

这定义了两个新的罗斯德规则:

1.  钥匙 `awesome_library`,仅为 Ubuntu 定义,映射到 `apt` 同名软件包

2.  钥匙 `that_other_library`,仅为 Ubuntu 定义,映射到 `pip` 已命名的软件包 `another_library`

运行后 `rosdep update`, `rosdep` 将探测到新的 `30-custom.list`,并促使它扫描其中的内容 `custom_rules.yaml` 文件。现在 `rosdep` 设立这些新钥匙是为了识别这些新钥匙以及它们应绘制的地图:

``` console
$ rosdep resolve awesome_library
#apt
awesome_library
$ rosdep resolve that_other_library
#pip
another_library
```

现在,你要做的就是添加 `<depend>awesome_library</depend>` 到您的ROS 软件包 `package.xml`,以及 `rosdep` 将知道如何安装依赖!

<span id="closing-remarks"></span>

## 闭幕词

上面的玩具示例只提示了自定义的 rosdep 密钥可能包含的内容 。

- **您的依赖性是APT 套件在第三方 PPA 中托管的吗 ?** 没问题,既然如此 `rosdep` 正在将密钥转换为 a `apt install` 引用时, APT 将不会有安装软件包的问题( 如果您添加了 PPA ) 。

- **您的依赖性是寄托在第三方索引中的 pip 软件包吗 ?** 将索引添加到您的 `pip.conf` 你们可以去。

- **源文件不必指向本地机器上的文件 。** 两者 `file://` 财务报告和财务报告 `https://` 语法被支持( 在 Linux 上, 绝对路径开始于 `/`导致三刀一刀 `file:///etc/rosdep/my.file`).

- **来源按字母顺序加载 。** 如果您在 30 前缀中添加了一条相互冲突的规则,它将不会被使用。 如果您创建了一个带有 10 前缀的源文件, 它会覆盖默认列表( 前缀 20) 中的软件包。 如果您正在使用从二进制寄存器安装的软件包, 将强烈建议您不要覆盖依赖声明, 因为这可能造成二进制不兼容性, 并且可能很难调试 。

- **无法合并密钥 。** 无法只添加一个 `fedora` 安装规则到现有的 rosdep 密钥。 根据负载顺序, 这种规则要么被忽略, 要么完全覆盖整个 rosdep 密钥, 删除所有其他安装器 。

<span id="further-reading"></span>

## 进一步阅读

- <https://docs.ros.org/en/independent/api/rosdep/html/rosdep_yaml_format.html>

- <https://docs.ros.org/en/independent/api/rosdep/html/contributing_rules.html>
