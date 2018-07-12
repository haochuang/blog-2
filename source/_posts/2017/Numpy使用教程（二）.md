---
title: Numpy使用教程（二）
date: 2017-11-06 16:39:16
index: 1
---


## numpy随机抽样

**随机数生成**

`numpy.random.rand(d0, d1, ..., dn)` 方法的作用为：指定一个数组，并使用 [0, 1) 区间随机数据填充，这些数据均匀分布。

``` Python
In [1]: import numpy as np

In [2]: np.random.rand(2,3)
Out[2]: 
array([[ 0.09887339,  0.75074537,  0.9944429 ],
       [ 0.92790081,  0.84365033,  0.3087985 ]])
```

<!-- more -->

* `numpy.random.randn(d0, d1, ..., dn)`: 返回符合标准正态分布的随机数据
* `numpy.random.randint(low, high, size, dtype)`： 返回[low, high)的随机整数
* `numpy.random.random_integers(low, high, size)`: 返回[low, high]的随机整数
* `numpy.random.random_sample(size)`: 在[0,1)区间生成指定`size`的随机浮点数
与之类似的方法还有：
*   `numpy.random.random([size])`
*   `numpy.random.ranf([size])`
*   `numpy.random.sample([size])`

* `numpy.random.choice(a, size, replace, p)`: 在给定的一维数组里生成随机数

**概率密度分布**

1.  `numpy.random.beta(a，b，size)`：从 Beta 分布中生成随机数。
2.  `numpy.random.binomial(n, p, size)`：从二项分布中生成随机数。
3.  `numpy.random.chisquare(df，size)`：从卡方分布中生成随机数。
4.  `numpy.random.dirichlet(alpha，size)`：从 Dirichlet 分布中生成随机数。
5.  `numpy.random.exponential(scale，size)`：从指数分布中生成随机数。
6.  `numpy.random.f(dfnum，dfden，size)`：从 F 分布中生成随机数。
7.  `numpy.random.gamma(shape，scale，size)`：从 Gamma 分布中生成随机数。
8.  `numpy.random.geometric(p，size)`：从几何分布中生成随机数。
9.  `numpy.random.gumbel(loc，scale，size)`：从 Gumbel 分布中生成随机数。
10.  `numpy.random.hypergeometric(ngood, nbad, nsample, size)`：从超几何分布中生成随机数。
11.  `numpy.random.laplace(loc，scale，size)`：从拉普拉斯双指数分布中生成随机数。
12.  `numpy.random.logistic(loc，scale，size)`：从逻辑分布中生成随机数。
13.  `numpy.random.lognormal(mean，sigma，size)`：从对数正态分布中生成随机数。
14.  `numpy.random.logseries(p，size)`：从对数系列分布中生成随机数。
15.  `numpy.random.multinomial(n，pvals，size)`：从多项分布中生成随机数。
16.  `numpy.random.multivariate_normal(mean, cov, size)`：从多变量正态分布绘制随机样本。
17.  `numpy.random.negative_binomial(n, p, size)`：从负二项分布中生成随机数。
18.  `numpy.random.noncentral_chisquare(df，nonc，size)`：从非中心卡方分布中生成随机数。
19.  `numpy.random.noncentral_f(dfnum, dfden, nonc, size)`：从非中心 F 分布中抽取样本。
20.  `numpy.random.normal(loc，scale，size)`：从正态分布绘制随机样本。
21.  `numpy.random.pareto(a，size)`：从具有指定形状的 Pareto II 或 Lomax 分布中生成随机数。
22.  `numpy.random.poisson(lam，size)`：从泊松分布中生成随机数。
23.  `numpy.random.power(a，size)`：从具有正指数 a-1 的功率分布中在 0，1 中生成随机数。
24.  `numpy.random.rayleigh(scale，size)`：从瑞利分布中生成随机数。
25.  `numpy.random.standard_cauchy(size)`：从标准 Cauchy 分布中生成随机数。
26.  `numpy.random.standard_exponential(size)`：从标准指数分布中生成随机数。
27.  `numpy.random.standard_gamma(shape，size)`：从标准 Gamma 分布中生成随机数。
28.  `numpy.random.standard_normal(size)`：从标准正态分布中生成随机数。
29.  `numpy.random.standard_t(df，size)`：从具有 df 自由度的标准学生 t 分布中生成随机数。
30.  `numpy.random.triangular(left，mode，right，size)`：从三角分布中生成随机数。
31.  `numpy.random.uniform(low，high，size)`：从均匀分布中生成随机数。
32.  `numpy.random.vonmises(mu，kappa，size)`：从 von Mises 分布中生成随机数。
33.  `numpy.random.wald(mean，scale，size)`：从 Wald 或反高斯分布中生成随机数。
34.  `numpy.random.weibull(a，size)`：从威布尔分布中生成随机数。
35.  `numpy.random.zipf(a，size)`：从 Zipf 分布中生成随机数。

## 数学函数

**三角函数**

1.  `numpy.sin(x)`：三角正弦。
2.  `numpy.cos(x)`：三角余弦。
3.  `numpy.tan(x)`：三角正切。
4.  `numpy.arcsin(x)`：三角反正弦。
5.  `numpy.arccos(x)`：三角反余弦。
6.  `numpy.arctan(x)`：三角反正切。
7.  `numpy.hypot(x1,x2)`：直角三角形求斜边。
8.  `numpy.degrees(x)`：弧度转换为度。
9.  `numpy.radians(x)`：度转换为弧度。
10.  `numpy.deg2rad(x)`：度转换为弧度。
11.  `numpy.rad2deg(x)`：弧度转换为度。

**双曲函数**

1.  `numpy.sinh(x)`：双曲正弦。
2.  `numpy.cosh(x)`：双曲余弦。
3.  `numpy.tanh(x)`：双曲正切。
4.  `numpy.arcsinh(x)`：反双曲正弦。
5.  `numpy.arccosh(x)`：反双曲余弦。
6.  `numpy.arctanh(x)`：反双曲正切。

**数值修约**

`numpy.around`与`numpy.round_`等价，四舍六入五取偶。

``` Python
>>> a = np.array([1.4, 1.5, 1.6, 2.4, 2.5, 2.6])
>>> np.around(a, 0)
array([ 1.,  2.,  2.,  2.,  2.,  3.])
```

1.  `numpy.around(a)`：平均到给定的小数位数。
2.  `numpy.round_(a)`：将数组舍入到给定的小数位数。
3.  `numpy.rint(x)`：修约到最接近的整数。
4.  `numpy.fix(x, y)`：向 0 舍入到最接近的整数。
5.  `numpy.floor(x)`：返回输入的底部(标量 x 的底部是最大的整数 i)。
6.  `numpy.ceil(x)`：返回输入的上限(标量 x 的底部是最小的整数 i).
7.  `numpy.trunc(x)`：返回输入的截断值。

**求和、求积、差分**

1.  `numpy.prod(a, axis, dtype, keepdims)`：返回指定轴上的数组元素的乘积。
2.  `numpy.sum(a, axis, dtype, keepdims)`：返回指定轴上的数组元素的总和。
3.  `numpy.nanprod(a, axis, dtype, keepdims)`：返回指定轴上的数组元素的乘积, 将 NaN 视作 1。
4.  `numpy.nansum(a, axis, dtype, keepdims)`：返回指定轴上的数组元素的总和, 将 NaN 视作 0。
5.  `numpy.cumprod(a, axis, dtype)`：返回沿给定轴的元素的累积乘积。
6.  `numpy.cumsum(a, axis, dtype)`：返回沿给定轴的元素的累积总和。
7.  `numpy.nancumprod(a, axis, dtype)`：返回沿给定轴的元素的累积乘积, 将 NaN 视作 1。
8.  `numpy.nancumsum(a, axis, dtype)`：返回沿给定轴的元素的累积总和, 将 NaN 视作 0。
9.  `numpy.diff(a, n, axis)`：计算沿指定轴的第 n 个离散差分。
10.  `numpy.ediff1d(ary, to_end, to_begin)`：数组的连续元素之间的差异。
11.  `numpy.gradient(f)`：返回 N 维数组的梯度。
12.  `numpy.cross(a, b, axisa, axisb, axisc, axis)`：返回两个(数组）向量的叉积。
13.  `numpy.trapz(y, x, dx, axis)`：使用复合梯形规则沿给定轴积分。

**指数和对数**

1.  `numpy.exp(x)`：计算输入数组中所有元素的指数。
2.  `numpy.expm1(x)`：对数组中的所有元素计算 exp(x） - 1.
3.  `numpy.exp2(x)`：对于输入数组中的所有 p, 计算 2 \*\* p。
4.  `numpy.log(x)`：计算自然对数。
5.  `numpy.log10(x)`：计算常用对数。
6.  `numpy.log2(x)`：计算二进制对数。
7.  `numpy.log1p(x)`：`log(1 + x)`。
8.  `numpy.logaddexp(x1, x2)`：`log2(2**x1 + 2**x2)`。
9.  `numpy.logaddexp2(x1, x2)`：`log(exp(x1) + exp(x2))`。

**算术运算**

1.  `numpy.add(x1, x2)`：对应元素相加。
2.  `numpy.reciprocal(x)`：求倒数 1/x。
3.  `numpy.negative(x)`：求对应负数。
4.  `numpy.multiply(x1, x2)`：求解乘法。
5.  `numpy.divide(x1, x2)`：相除 x1/x2。
6.  `numpy.power(x1, x2)`：类似于 x1^x2。
7.  `numpy.subtract(x1, x2)`：减法。
8.  `numpy.fmod(x1, x2)`：返回除法的元素余项。
9.  `numpy.mod(x1, x2)`：返回余项。
10.  `numpy.modf(x1)`：返回数组的小数和整数部分。
11.  `numpy.remainder(x1, x2)`：返回除法余数。


**矩阵和向量积**

1.  `numpy.dot(a,b)`：求解两个数组的点积。
2.  `numpy.vdot(a,b)`：求解两个向量的点积。
3.  `numpy.inner(a,b)`：求解两个数组的内积。
4.  `numpy.outer(a,b)`：求解两个向量的外积。
5.  `numpy.matmul(a,b)`：求解两个数组的矩阵乘积。
6.  `numpy.tensordot(a,b)`：求解张量点积。
7.  `numpy.kron(a,b)`：计算 Kronecker 乘积。

**其他**

1.  `numpy.angle(z, deg)`：返回复参数的角度。
2.  `numpy.real(val)`：返回数组元素的实部。
3.  `numpy.imag(val)`：返回数组元素的虚部。
4.  `numpy.conj(x)`：按元素方式返回共轭复数。
5.  `numpy.convolve(a, v, mode)`：返回线性卷积。
6.  `numpy.sqrt(x)`：平方根。
7.  `numpy.cbrt(x)`：立方根。
8.  `numpy.square(x)`：平方。
9.  `numpy.absolute(x)`：绝对值, 可求解复数。
10.  `numpy.fabs(x)`：绝对值。
11.  `numpy.sign(x)`：符号函数。
12.  `numpy.maximum(x1, x2)`：最大值。
13.  `numpy.minimum(x1, x2)`：最小值。
14.  `numpy.nan_to_num(x)`：用 0 替换 NaN。
15.  `numpy.interp(x, xp, fp, left, right, period)`：线性插值。

## 代数运算

1.  `numpy.linalg.cholesky(a)`：Cholesky 分解。
2.  `numpy.linalg.qr(a ,mode)`：计算矩阵的 QR 因式分解。
3.  `numpy.linalg.svd(a ,full_matrices,compute_uv)`：奇异值分解。
4.  `numpy.linalg.eig(a)`：计算正方形数组的特征值和右特征向量。
5.  `numpy.linalg.eigh(a, UPLO)`：返回 Hermitian 或对称矩阵的特征值和特征向量。
6.  `numpy.linalg.eigvals(a)`：计算矩阵的特征值。
7.  `numpy.linalg.eigvalsh(a, UPLO)`：计算 Hermitian 或真实对称矩阵的特征值。
8.  `numpy.linalg.norm(x ,ord,axis,keepdims)`：计算矩阵或向量范数。
9.  `numpy.linalg.cond(x ,p)`：计算矩阵的条件数。
10.  `numpy.linalg.det(a)`：计算数组的行列式。
11.  `numpy.linalg.matrix_rank(M ,tol)`：使用奇异值分解方法返回秩。
12.  `numpy.linalg.slogdet(a)`：计算数组的行列式的符号和自然对数。
13.  `numpy.trace(a ,offset,axis1,axis2,dtype,out)`：沿数组的对角线返回总和。
14.  `numpy.linalg.solve(a,b)`：求解线性矩阵方程或线性标量方程组。
15.  `numpy.linalg.tensorsolve(a,b ,axes)`：为 x 解出张量方程a x = b
16.  `numpy.linalg.lstsq(a,b ,rcond)`：将最小二乘解返回到线性矩阵方程。
17.  `numpy.linalg.inv(a)`：计算逆矩阵。
18.  `numpy.linalg.pinv(a ,rcond)`：计算矩阵的（Moore-Penrose）伪逆。
19.  `numpy.linalg.tensorinv(a ,ind)`：计算N维数组的逆。

## 索引与切片

基本格式：`ndarray[start:stop:step]`，包含`start`，不包含`stop`。

``` Python
>>> a = np.arange(20).reshape(4,5)
>>> a
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14],
       [15, 16, 17, 18, 19]])
>>> a[0:3,2:4]
array([[ 2,  3],
       [ 7,  8],
       [12, 13]])
```

## 排序、搜索、计数

**排序**

``` Python
numpy.sort(a, axis=-1, kind='quicksort', order=None)
```

*   `a`：数组。
*   `axis`：要排序的轴。如果为`None`，则在排序之前将数组铺平。默认值为 `-1`，沿最后一个轴排序。
*   `kind`：`{'quicksort'，'mergesort'，'heapsort'}`，排序算法。默认值为 `quicksort`。

其它排序方法：

1.  `numpy.lexsort(keys ,axis)`：使用多个键进行间接排序。
2.  `numpy.argsort(a ,axis,kind,order)`：沿给定轴执行间接排序。
3.  `numpy.msort(a)`：沿第 1 个轴排序。
4.  `numpy.sort_complex(a)`：针对复数排序。

**搜索和计数**

1.  `argmax(a ,axis,out)`：返回数组中指定轴的最大值的索引。
2.  `nanargmax(a ,axis)`：返回数组中指定轴的最大值的索引,忽略 NaN。
3.  `argmin(a ,axis,out)`：返回数组中指定轴的最小值的索引。
4.  `nanargmin(a ,axis)`：返回数组中指定轴的最小值的索引,忽略 NaN。
5.  `argwhere(a)`：返回数组中非 0 元素的索引,按元素分组。
6.  `nonzero(a)`：返回数组中非 0 元素的索引。
7.  `flatnonzero(a)`：返回数组中非 0 元素的索引,并铺平。
8.  `where(条件,x,y)`：根据指定条件,从指定行、列返回元素。
9.  `searchsorted(a,v ,side,sorter)`：查找要插入元素以维持顺序的索引。
10.  `extract(condition,arr)`：返回满足某些条件的数组的元素。
11.  `count_nonzero(a)`：计算数组中非 0 元素的数量。
