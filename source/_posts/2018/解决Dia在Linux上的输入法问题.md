---
title: 解决Dia在Linux上的输入法问题
date: 2018-05-31 22:43:50
index: 1
---

Dia是一个比较小巧的画图软件，支持Windows、Mac和Linux，功能类似于Visio。个人觉得Dia还挺好用的，不过，有个比较烦的地方就是用不了中文输入法。在Linux上，输入法比较折腾。

按网上的说法改`/usr/bin/dia`不行，用`dia-normal`提示没这个命令。不过，我自己发现了解决方案。

## 探索历程

运行`dia --help`，可以看到有个`--classic`选项。尝试使用这个选项运行，Dia就能输入中文了，只是工具箱面板分离了。

虽然这样算是解决了输入法问题，但是，窗口太小了，还不能记住窗口大小，所以每次都要拖一下窗口大小，用起来挺不方便、不够优雅。

<!-- more -->

在Windows平台上，Dia也是默认用不了中文输入法，但是菜单栏有个输入法菜单，选择输入法为”简单“即可。在Ubuntu上好像菜单栏也有输入法菜单，但是Deepin（也就是我在用的Linux发行版）上，Dia菜单栏上没有输入法菜单。

今天偶然发现，打开Dia后，在要输入文字时，右键就有输入法的菜单，选择”X输入法“就能用中文输入法了，而且也不会面板分离。

可是，每次要输入中文时都右键选择一下还是不够优雅。有彻底终结Dia输入法问题的方案吗？

既然Dia能选择输入法，那么其环境变量应该有输入法相关的。于是，运行`dia &`会后台启动Dia，并且显示其PID，然后`cat /proc/<PID>/environ`就能显示其环境变量了。Dia是GTK应用程序，”X输入法“英文是"X input method"，简写"xim"，在`im-config`里也有"xim"。而在Dia的环境变量里有`GTK_IM_MODULE=fcitx`（我用的是搜狗输入法），Dia应该是通过这个环境变量来选择输入法的。尝试运行`GTK_IM_MODULE=xim dia`发现就能用中文输入法了！

## 终极方案

所以，解决方案就是设置Dia的环境变量，使得`GTK_IM_MODULE=xim`就行了。具体来说，修改`/usr/share/applications/dia.desktop`:

```
Exec=env GTK_IM_MODULE=xim dia %F
```

至于从终端启动Dia，可以写条alias：

``` Bash
alias dia="env GTK_IM_MODULE=xim dia"
```
