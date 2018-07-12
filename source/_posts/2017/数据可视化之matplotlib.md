---
title: 数据可视化之matplotlib
date: 2017-11-10 16:06:23
---


## 架构

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20171108b.png)

Artist是图像上所有可见元素的基类，以对象的方式对可见元素进行描述。  

图像渲染依赖于Backend，Backend作为后端绘图渲染引擎，支持GUI方式（直接将图像显示在屏幕上，如GTK、WX等）与非GUI方式（输出为某种格式的文件，如PS、AGG等）。

<!-- more -->


获取与设置Backend：

``` Python
>>> import matplotlib as mpl
>>> mpl.get_backend()
'TkAgg'
>>> mpl.use('agg')
>>> mpl.get_backend()
'agg'
```

创建Figure：

``` Python
>>> import matplotlib.pyplot as plt
>>> plt.figure()
<matplotlib.figure.Figure object at 0x7fc6b845ccc0> 
>>> fig = plt.gcf()
>>> fig.get_children()
[<matplotlib.patches.Rectangle object at 0x7fc6945d5390>]
```

使用`plt.figure()`会创建并返回一个Figure对象。
Figure是绘图区，所有的绘图操作都在Figure里进行。
使用`plt.gcf()`可以获取当前的Figure。

为了绘图，我们可以先创建Axes：

``` Python
>>> plt.axes()
<matplotlib.axes._subplots.AxesSubplot object at 0x7fc68cbd3048> 
>>> ax = plt.gca()
>>> ax.get_children()
[<matplotlib.spines.Spine object at 0x7fc68cbd3438>, 
<matplotlib.spines.Spine object at 0x7fc68cbd3550>, 
<matplotlib.spines.Spine object at 0x7fc68cbd3668>, 
<matplotlib.spines.Spine object at 0x7fc68cbd3780>, 
<matplotlib.axis.XAxis object at 0x7fc68cbd3860>, 
<matplotlib.axis.YAxis object at 0x7fc68cbedeb8>, 
Text(0.5,1,''), 
Text(0,1,''), 
Text(1,1,''), 
<matplotlib.patches.Rectangle object at 0x7fc68bc48a20>]
>>> fig.get_children()
[<matplotlib.patches.Rectangle object at 0x7fc6945d5390>, 
<matplotlib.axes._subplots.AxesSubplot object at 0x7fc68cbd3048>]
```

使用`plt.axes()`会创建并返回一个Axes对象。
Axes是坐标轴区域，默认包括Spine、Axis、标题和一个绘图区域。

Axis是坐标轴，包括Tick（刻度）和标签。Spine是轴线（如图中红线）：

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20171108c.png)

**【Tip】**

可以设置风格：

``` Python
plt.style.use('seaborn')
```

查看可用风格：

``` Python
print(plt.style.available)
```

## 2D绘图

### 常用操作

创建Figure，创建subplot：

``` Python
fig = plt.figure()
ax = plt.subplot()
```

可以合为一步：

``` Python
fig, ax = plt.subplots()
```

即使没有事先创建Figure和subplot，也会自动创建。

### plot绘制折线图

常用参数：

|参数|解释|
|---|---|
|color|颜色|
|linestyle|线型|
|linewidth|线条宽度|
|alpha|透明度|
|label|标签（制作图例时用到）|
|marker|标记点类型|
|markerfacecolor|标记点颜色|
|markersize|标记点大小|

示例：

``` Python
from matplotlib import pyplot as plt
import numpy as np

X = np.linspace(-2 * np.pi, 2 * np.pi, 1000)
y1 = np.sin(X)
y2 = np.cos(X)

plt.plot(X, y1, color='r', linestyle='--', linewidth=2, alpha=0.8)
plt.plot(X, y2, color='b', linestyle='-', linewidth=2)
plt.show()
```

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20171109Figure_2.png)


### scatter绘制散点图

常用参数：

|参数|解释|
|---|---|
|s|散点大小|
|c|散点颜色|
|edgecolors|散点边缘颜色|
|marker|散点样式|
|cmap|定义多类别散点的颜色|
|alpha|透明度|

示例：

``` Python
from matplotlib import pyplot as plt
import numpy as np

x = np.random.rand(100)
y = np.random.rand(100)
colors = np.random.rand(100)
size = np.random.normal(20, 30, 100)

plt.scatter(x, y, s=size, c=colors)
plt.show()
```

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20171109Figure_3.png)


### 其它

|命令|图类型|
|---|---|
|pie|饼图|
|bar|条形图|
|hist|柱状图|
|barh|直方图|
|contour|等高线图|
|imshow|显示图像|
|...|...|


### 子图绘制

**1、使用`plt.subplot`**

``` Python
plt.subplot(nrows, ncols, plot_number)
```

- `nrows`: 将行进行n等分
- `ncols`: 将列进行n等分
- `plot_number`: 序号（从1开始，从左到右，从上到下，依次递增）

如果出现重叠区域，则会覆盖之前的。

示例：

``` Python
import numpy as np
import matplotlib.pyplot as plt

x1 = np.linspace(0.0, 5.0)
x2 = np.linspace(0.0, 2.0)

y1 = np.cos(2 * np.pi * x1) * np.exp(-x1)
y2 = np.cos(2 * np.pi * x2)

plt.subplot(2, 1, 1)
plt.plot(x1, y1, 'o-')
plt.title('A tale of 2 subplots')
plt.ylabel('Damped oscillation')

plt.subplot(2, 1, 2)
plt.plot(x2, y2, '.-')
plt.xlabel('time (s)')
plt.ylabel('Undamped')

plt.show()
```

![](http://matplotlib.org/_images/sphx_glr_subplot_001.png)

**2、使用`plt.axes`**

``` Python
plt.axes(*args, **kwargs)
```

向`plt.axes`传入一个数组`[left, bottom, width, height]`（归一化），
设置axes范围。

示例：

``` Python
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(-2 * np.pi, 2 * np.pi)
y1 = np.sin(x)
y2 = np.cos(x)

plt.axes([.1, .1, .8, .8])
plt.plot(x, y1, 'k')

plt.axes([.6, .6, .3, .3])
plt.plot(x, y2, 'r')

plt.show()
```

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20171109Figure_4.png)
