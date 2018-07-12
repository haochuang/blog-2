---
title: Python进行doctest
date: 2018-04-30 20:33:17
---

## doctest简介

在doc注释部分使用形如`Python`交互式命令行的代码，可以进行`doctest`。

``` Python
def add(a, b):
    """
    >>> add(1, 2)
    3
    """
    return a + b
```

<!-- more -->

## 运行doctest

1、PyCharm设置：

选择`Run>Edit Configurations`，新建一个`doctest`配置，然后运行即可。

2、命令行运行：

``` Shell
$ python -m doctest -v hello.py
```

`-v`选项会显示详细信息。

3、使用`doctest`模块：

``` Python
import doctest
doctest.testmod(verbose=True)
```
