---
title: 记因内核版本错误导致U盘不能识别的问题解决
date: 2018-07-03 20:48:29
tags: 
  - Linux
---

U盘插上电脑，发现没有自动挂载。然后运行`sudo fdisk -l`一看，发现并没有U盘所对应的设备，也就是U盘不能识别了！以前从没在Linux上遇到这种问题，通过查资料得知，要识别U盘，需要装载`usb-storage`模块。

于是，运行`lsmod | grep usb`发现确实没有`usb-storage`模块。

为了判断U盘是否物理损坏导致系统无法“感知”U盘的存在，运行命令`sudo udevadm monitor --udev`，发现U盘插拔时有反应。

然后运行`sudo modprobe usb-storage`尝试装载`usb-storage`模块，结果报错：

<!-- more -->

```
modprobe: FATAL: Module usb-storage not found in directory /lib/modules/4.14.48-2-MANJARO
```

查看`/lib/modules/`目录：

``` Bash
$ ls /lib/modules 
4.14.40-rt30-MANJARO  extramodules-4.14-MANJARO
4.14.52-1-MANJARO     extramodules-4.14-rt-MANJARO
```

发现并没有`4.14.48-2-MANJARO`。在Manjaro设置管理器里查看内核：

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20180704_103445.jpg)

发现当前内核版本为`4.14.52-1`。但是，运行`uname -r`却显示内核版本为`4.14.48-2-MANJARO`。这也难怪`modprobe`会到`/lib/modules/4.14.48-2-MANJARO`目录下去找`usb-storage`模块。

通过查询`modprobe`的manpage，发现可以指定版本。运行`sudo modprobe --set-version=4.14.52-1-MANJARO usb-storage`，U盘终于自动挂载了！

为了启动时使用`4.14.52-1`版本的内核，运行`sudo update-grub`来更新grub，重启后再运行`uname -r`显示内核版本为`4.14.52-1-MANJARO`，U盘也能自动挂载，运行`lsmod | grep usb`也有`usb-storage`模块，问题解决。

