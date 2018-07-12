---
title: Numpy使用注意要点
---

## Array

若`a`,`b`都是`ndarray`，`a*b`等效于Matlab里的`a.*b`，而点积要用`np.dot(a, b)`。

`np.multiply`和`*`都是逐元素相乘。

``` Python
>>> a = np.arange(1,5).reshape(2,2)
>>> b = np.arange(5,9).reshape(2,2)
>>> a
array([[1, 2],
       [3, 4]])
>>> b
array([[5, 6],
       [7, 8]])
>>> a * b
array([[ 5, 12],
       [21, 32]])
>>> np.multiply(a,b)
array([[ 5, 12],
       [21, 32]])
>>> np.dot(a, b)
array([[19, 22],
       [43, 50]])
```

<!-- more -->

## Matrix

## 参数设置

### shape

`np.ones`, `np.zeros`中的`shape`参数是`int`或`tuple`，因此，要创建n*m大小的数组应写为：

``` Python
# 正确的写法
np.ones((n, m))
np.zeros((n, m))
# 易错的写法
np.ones(n, m)
np.zeros(n, m)
```

注意，`np.eye`（而不是`np.eyes`）的第一个参数不是`shape`而是`N`，故创建N阶对角阵直接写`np.eye(N)`即可。

### end


## 数学函数

`np.log`是计算自然对数的