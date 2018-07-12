---
title: PPM、PGM、PBM图像格式剖析
date: 2018-06-27 18:09:09
tags:
---

今天突然需要用到`PPM`这个图像文件格式，之前没见过，在此记录一下。

`PPM`、`PGM`、`PBM`这三个图像文件格式很少见，其实也不难，分别用于彩色图像、灰度图像、二值图像。这里以`PPM`格式为例。

`PPM`格式有两种类型：字节码和ASCII。前者是二进制文件，后者是纯文本文件。

使用`convert`命令可以将图像转为`PPM`格式：



<!-- more -->



``` Bash
# 字节码
$ convert xxx.jpg xxx.ppm
# ASCII
$ convert xxx.jpg -compress none xxx.ppm
```

ASCII类型的`PPM`文件示例：

``` 
P3
1305 1305
255
229 232 237 228 231 236 ...
```

第1行是`P3`，第2行是图像大小，第3行是最大值，一般是`255`。从第4行起就是每个像素的颜色值了，像素顺序一般是从左到右、从上到下，通道顺序一般是RGB。

字节码类型的`PPM`文件示例：

```
50 36 0A 31 33 30 35 20 31 33 30 35 0A 32 35 35 0A E5 E8 ED E4 E7 EC ...
```

最开始的`50 36`对应ASCII为`P6`，`31 33 30 35`对应ASCII为`1305`。`32 35 35`对应ASCII为`255`。后面的像素值以十六进制表示：

``` Bash
$ bc -q
obase=10
ibase=16
E5
229
E8
232
ED
237
```

Python的`Pillow`库可以读取、存储字节码类型的`PPM`格式：

``` python
from PIL import Image

# 存储PPM格式
im = Image.open('xxx.jpg')
im.save('xxx.ppm')

# 读取PPM格式
im = Image.open('xxx.ppm')
im.show()
```

`PGM`头部用`P2`和`P5`分别表示ASCII类型和字节码类型；`PBM`头部用`P1`和`P4`分别表示ASCII类型和字节码类型，但没有像`PPM`第3行的最大值，ASCII类型的像素值都是0或1。

