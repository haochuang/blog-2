---
title: MNIST数据集解析
date: 2018-03-08 20:54:58
---


从[MNIST数据集官网](http://yann.lecun.com/exdb/mnist/)可以下载MNIST数据集。

MNIST数据集以.gz格式压缩，Python可以直接读取而不需要解压缩：

``` Python
import gzip

with gzip.open('t10k-images-idx3-ubyte.gz') as f:
    buf = f.read()
```

MNIST数据集使用二进制文件，而不是常规的图片文件格式。以t10k-images-idx3-ubyte为例，在官网有其结构说明：

<!-- more -->

``` 
[offset] [type]          [value]          [description] 
0000     32 bit integer  0x00000803(2051) magic number 
0004     32 bit integer  10000            number of images 
0008     32 bit integer  28               number of rows 
0012     32 bit integer  28               number of columns 
0016     unsigned byte   ??               pixel 
0017     unsigned byte   ??               pixel 
........ 
xxxx     unsigned byte   ??               pixel
```

先解压并查看t10k-images-idx3-ubyte的内容：

``` Shell
$ gzip -d -k t10k-images-idx3-ubyte.gz
$ xxd t10k-images-idx3-ubyte | head
00000000: 0000 0803 0000 2710 0000 001c 0000 001c  ......'.........
00000010: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000020: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000030: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000040: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000050: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000060: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000070: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000080: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000090: 0000 0000 0000 0000 0000 0000 0000 0000  ................
```

最开始4个字节是魔数，16进制为0x00000803，从结果可以看出确实如此。随后的4个字节为图片的数量，值为10000，16进制为0x2710。可以通过Linux自带的计算器bc来计算，ibase和obase分别为输入和输出的进制：

``` Shell
$ bc -q
ibase=10
obase=16
10000
2710
```

那么，在Python中要怎么解析二进制数据呢？可以使用struct模块来读取二进制文件：

``` Python
import struct

magic, images, rows, columns = struct.unpack_from('>iiii', buf, 0)
print(magic, images, rows, columns)
# Output：
2051 10000 28 28
```

在MNIST官网对数据集的格式有这样的一句说明：“All the integers in the files are stored in the MSB first (high endian) format used by most non-Intel processors. Users of Intel processors and other low-endian machines must flip the bytes of the header.”。意思是，所有的整数使用MSB方式（也就是大端模式）。大小端模式是数据在地址上的存放方式。大端模式高字节保存在低地址中，小端模式反之。

`>iiii`的意思就是：大端模式，读取四个int（C语言）。参见[Python struct模块官方文档](https://docs.python.org/3/library/struct.html)

来读取第一张图片试试：

``` Python
from PIL import Image

idx = struct.calcsize('>iiii')
img = Image.new('L', (columns, rows))
for i in range(rows):
    for j in range(columns):
        img.putpixel((j, i), int(struct.unpack_from('B', buf, idx)[0]))
        idx += struct.calcsize('B')
```
在MNIST官网对于像素的格式有这样的说明：“Pixels are organized row-wise. Pixel values are 0 to 255. 0 means background (white), 255 means foreground (black). ”。

`Image.new()`第一个参数是模式，由于MNIST数据集是灰度图像，所以是`L`，第二个参数是尺寸`(宽, 高)`。由于像素是按行排列，也就是第一个像素坐标是(0, 0)，第二个像素坐标是(1, 0)，第三个像素坐标是(2, 0)，以此类推。坐标(x, y)以左上角为原点。

如果使用Jupyter notebook，可以使用内联matplotlib来显示图片：

``` Python
import matplotlib.pyplot as plt
import numpy as np
%matplotlib inline

plt.imshow(np.asarray(img), cmap=plt.cm.gray)
```

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20180308a.png)

至于其余的文件解析，在此就不赘述了。
