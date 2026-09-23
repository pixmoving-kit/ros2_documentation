---
translation_status: machine_translated
source: Tutorials/Miscellaneous/Building-Realtime-rt_preempt-kernel-for-ROS-2.rst
---

!!! info "翻译说明"

    本页为自动翻译初稿，尚未逐页人工校对；代码、命令和 API 标识保留原文。

<span id="building-a-real-time-linux-kernel-community-contributed"></span>

# 构建实时 Linux 内核（社区贡献）

这个教程开始于一个在 Intel x86_64 上安装的干净的 Ubuntu 20.04.1 。 实际内核为 5. 4.0-54 基因, 但我们将安装最新稳定 RT\_ PREEMPT 版本。 要构建内核, 您至少需要 30GB 的自由磁盘空间 。

检查 [这个维基](https://wiki.linuxfoundation.org/realtime/start) 对于最新的稳定版本,在编写本报告时,这是“最后稳定版本5.4-rt”。 [链接](http://cdn.kernel.org/pub/linux/kernel/projects/rt/5.4/),我们得到了准确的版本。目前是 `patch-5.4.78-rt44.patch.gz`.

![](images/realtime-kernel-patch-version.png)

我们在我们的家目录中创建一个目录

``` console
$ mkdir ~/kernel
```

切换到它与

``` console
$ cd ~/kernel
```

我们可以用浏览器去 [此页面](https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/) 并查看该版本是否在那里。 您可以从网站下载并手动将其从/ 下載到/ 内核文件夹, 或者通过右键点击“ 复制链接位置” 的链接来下载它 。 例如 :

``` console
$ wget https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/linux-5.4.78.tar.gz
```

拆开它

``` console
$ tar -xzf linux-5.4.78.tar.gz
```

下载匹配 Kernel 版本的 rt\_ preferent 补丁 [内核.org](http://cdn.kernel.org/pub/linux/kernel/projects/rt/5.4/)

``` console
$ wget http://cdn.kernel.org/pub/linux/kernel/projects/rt/5.4/older/patch-5.4.78-rt44.patch.gz
```

拆开它

``` console
$ gunzip patch-5.4.78-rt44.patch.gz
```

然后切换到 linux 目录

``` console
$ cd linux-5.4.78/
```

用实时补丁补补补内核

``` console
$ patch -p1 < ../patch-5.4.78-rt44.patch
```

我们只想使用我们Ubuntu安装的配置,所以我们得到Ubuntu配置

``` console
$ cp /boot/config-5.4.0-54-generic .config
```

在 Ubuntu 软件菜单中打开软件并更新, 请选中“ 源代码” 框

我们需要一些工具来建立内核,安装它们

``` console
$ sudo apt-get build-dep linux
$ sudo apt-get install libncurses-dev flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf fakeroot
```

要启用所有 Ubuntu 配置, 我们只需使用

``` console
$ yes '' | make oldconfig
```

然后我们需要启用 rt\_ preference 在内核中。

``` console
$ make menuconfig
```

并设置如下:

``` bash
# Enable CONFIG_PREEMPT_RT
 -> General Setup
  -> Preemption Model (Fully Preemptible Kernel (Real-Time))
   (X) Fully Preemptible Kernel (Real-Time)

# Enable CONFIG_HIGH_RES_TIMERS
 -> General setup
  -> Timers subsystem
   [*] High Resolution Timer Support

# Enable CONFIG_NO_HZ_FULL
 -> General setup
  -> Timers subsystem
   -> Timer tick handling (Full dynticks system (tickless))
    (X) Full dynticks system (tickless)

# Set CONFIG_HZ_1000 (note: this is no longer in the General Setup menu, go back twice)
 -> Processor type and features
  -> Timer frequency (1000 HZ)
   (X) 1000 HZ

# Set CPU_FREQ_DEFAULT_GOV_PERFORMANCE [=y]
 ->  Power management and ACPI options
  -> CPU Frequency scaling
   -> CPU Frequency scaling (CPU_FREQ [=y])
    -> Default CPUFreq governor (<choice> [=y])
     (X) performance
```

保存和退出菜单配置。 现在我们要构建一个需要相当时间的内核。 (10-30min on a modern cpu)

``` console
$ make -j `nproc` deb-pkg
```

建置完成后, 请检查 dib 包

``` console
$ ls ../*deb
../linux-headers-5.4.78-rt41_5.4.78-rt44-1_amd64.deb  ../linux-image-5.4.78-rt44-dbg_5.4.78-rt44-1_amd64.deb
../linux-image-5.4.78-rt41_5.4.78-rt44-1_amd64.deb    ../linux-libc-dev_5.4.78-rt44-1_amd64.deb
```

然后安装所有内核Deb软件包

``` console
$ sudo dpkg -i ../*.deb
```

现在应该安装实时内核。 重新启动系统 :

``` console
$ sudo reboot
```

并检查新内核版本 :

``` console
$ uname -a
Linux ros2host 5.4.78-rt44 #1 SMP PREEMPT_RT Fri Nov 6 10:37:59 CET 2020 x86_64 xx
```
