---
title: gimp练习：Disintegration Effect
date: 2018-02-04 10:57:01
---


在Youtube上看到了一个Gimp教程：[GIMP Tutorial: Disintegration Effect](https://www.youtube.com/watch?v=kUt2cditN1g)，就来练习一下。

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20180203a.jpg)

<!-- more -->

打开模特的照片，如果我们系统的Gtk主题是亮色的，画布衬垫可能是白色的，这样就和模特照片的白色混在一起了。因此，建议在 首选项>图像窗口>外观 修改画布衬垫模式。

首先，模特照片的颜色有点灰，因此可以调整下色阶：

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20180203b.jpg)

然后，复制图层，对下面的图层应用 滤镜>扭曲>交互式翘曲(IWarp)：

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20180203c.jpg)

对于复制的图层，以模特的背景色（白色）为前景色，选择碎片状的笔刷，以合适的大小，画出边缘碎裂的效果（如图）：

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20180203d.jpg)

然后添加白色的蒙板，用以黑色为前景色，补上更远处的碎片。其实这里用的是下面扭曲的部分：

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20180203e.jpg)

大体的效果已经出来了，然后就是补上裂纹的效果。

添加裂纹图片，然后调整到合适的位置和大小，应用阈值，提升纹路效果：

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20180203f.jpg)

然后视频里使用按颜色选择工具，将白色部分抠去，然后反相。其实可以反相然后修改图层混合模式就行，这里我选择的是仅变亮：

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20180203g.jpg)

之后再使用蒙板将不需要的地方擦掉即可。

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20180203h.jpg)

