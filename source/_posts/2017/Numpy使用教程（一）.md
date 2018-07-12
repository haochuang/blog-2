---
title: Numpy使用教程（一）
date: 2017-11-06 16:38:00
---


## 术语

**axis**

对于二维数组，垂直为轴0，水平为轴1。许多操作可以沿着一个轴进行。

``` Python
>>> x = np.arange(12).reshape(3,4)
>>> x
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>> x.sum(axis=0)
array([12, 15, 18, 21])
>>> x.sum(axis=1)
array([ 6, 22, 38])
```

<!-- more -->

**broadcast**

Numpy可以对形状不匹配的数组执行操作

``` Python
>>> np.array([2,3])+1
array([3, 4])
>>> np.array([[2,3]])+1
array([[3, 4]])
>>> np.array([[2,3]])+np.array([1,2])
array([[3, 5]])
>>> np.array([[2],[3]])+np.array([1,2])
array([[3, 4],
       [4, 5]])
```

这个机制在便捷的同时可能会出现意料之外的结果，因此shape不确定时建议先`reshape`。

**mask**

布尔数组，用于选取元素

``` Python
>>> x = np.arange(5)
>>> x
array([0, 1, 2, 3, 4])
>>> mask = (x > 2)
>>> mask
array([False, False, False, True,  True], dtype=bool)
>>> x[mask] = -1
>>> x
array([ 0,  1,  2,  -1, -1])
```
                                                                                                             
## 数值类型

numpy 支持丰富的数值类型。

在数组里可以使用`dtype=`参数来指定数值类型。

使用`.astype()`方法进行数值类型转换，使用`.dtype`查看dtype属性。

## 多维数组的创建

【参数说明】

`order`：{'C'，'F'}，以行(C)或列(F)为主顺序。

创建`ndarray`的方法：

**从列表、元组等转换**

使用`np.array`将列表或元组转换为ndarray数组

**使用np原生方法创建**

* `arange`在指定区间内创建一系列均匀间隔的值
* `linspace`在指定区间内返回间隔均匀的值
* `ones`用于快速创建数值全部为1的多维数组
* `zeros`用于快速创建数值全部为0的多维数组
* `eye`用于创建一个二维数组，其特点是k对角线上的值为1，其余值全为0

``` Python
numpy.arange(start, stop, step, dtype=None)
numpy.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)
numpy.ones(shape, dtype=None, order='C')
numpy.zeros(shape, dtype=None, order='C')
numpy.eye(N, M=None, k=0, dtype=<type 'float'>)
```

**从已知数据创建**

`fromfile`:从文本或二进制文件中构建多维数组

`frombuffer`, `fromfunction`, `fromiter`, `fromstring`等

## 多维数组的属性

`T`：数组的转置，同`.transpose()`

`dtype`：包含元素的数据类型

`imag`: 元素的虚部

`rear`: 元素的实部

`size`: 元素数

`itemsize`: 一个数组元素的字节数

`nbytes`: 元素总字节数

`ndim`: 数组的尺寸

`shape`: **数组维数组**

## 数组的基本操作

**reshape**

`numpy.reshape()` 等效于 `ndarray.reshape()`。

``` Python
numpy.reshape(a, newshape)
```

重设形状, `newshape`为整数或元组。

**ravel**

``` Python
numpy.ravel(a, order='C')
```

将数组扁平化，变为一维数组。

**moveaxis**

``` Python
numpy.moveaxis(a, source, destination)
```

轴移动，可以将数组的轴移动到新的位置。

**swapaxes**

``` Python
numpy.swapaxes(a, axis1, axis2)
```

轴交换与轴移动的效果可以通过查看操作前后的shape变化来理解。

**transpose**

``` Python
numpy.transpose(a, axis=None)
```

类似于矩阵的转置。`axis`有值时替换轴。

**atleast_xd**

``` Python
numpy.atleast_1d()
numpy.atleast_2d()
numpy.atleast_3d()
```

维度改变，支持将输入数据直接视为`x`维。

**类型转换**

*   `asarray(a，dtype，order)`：将特定输入转换为数组。
*   `asanyarray(a，dtype，order)`：将特定输入转换为 `ndarray`。
*   `asmatrix(data，dtype)`：将特定输入转换为矩阵。
*   `asfarray(a，dtype)`：将特定输入转换为 `float`类型的数组。
*   `asarray_chkfinite(a，dtype，order)`：将特定输入转换为数组，检查 `NaN` 或 `infs`。
*   `asscalar(a)`：将大小为 1 的数组转换为标量。

**concatenate**

``` Python
numpy.concatenate((a1, a2, ...), axis=0)
```

数组连接。`axis`：指定连接轴。

**堆叠**

*   `stack(arrays，axis)`：沿着新轴连接数组的序列。
*   `column_stack()`：将 1 维数组作为列堆叠到 2 维数组中。
*   `hstack()`：按水平方向堆叠数组。
*   `vstack()`：按垂直方向堆叠数组。
*   `dstack()`：按深度方向堆叠数组。

**拆分**

*   `split(ary，indices_or_sections，axis)`：将数组拆分为多个子数组。
*   `dsplit(ary，indices_or_sections)`：按深度方向将数组拆分成多个子数组。
*   `hsplit(ary，indices_or_sections)`：按水平方向将数组拆分成多个子数组。
*   `vsplit(ary，indices_or_sections)`：按垂直方向将数组拆分成多个子数组。

**delete**

``` Python
numpy.delete(arr，obj，axis)
```

沿特定轴删除数组中的子数组。

**insert**

``` Python
numpy.insert(arr，obj，values，axis)
```

依据索引在特定轴之前插入值。

**append**

``` Python
numpy.append(arr，values，axis)
```

将值附加到数组的末尾，并返回 1 维数组。

**resize**

``` Python
numpy.resize(a，new_shape)
```

对数组尺寸进行重新设定。

【resize和reshape的区别】

`reshape` 在改变形状时，不会影响原数组，相当于对原数组做了一份拷贝。而 `resize` 则是对原数组执行操作。

**翻转数组**

*   `fliplr(m)`：左右翻转数组。
*   `flipud(m)`：上下翻转数组。
