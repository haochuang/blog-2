---
title: Scala语言初步
date: 2017-10-29 03:09:35
---

两种类型的变量：`val`（常变量，类似于final）、`var`。`Unit`类型类似于void。

函数定义形式：

``` Scala
def func(para:Type):Type={
// do something
}
```

每个Scala表达式都有返回结果，函数最后一个表达式的值作为返回值（类似于Matlab）

Scala没有`i++`和`++i`，用`()`访问数组而不是`[]`，使用`[]`指定泛型参数而不是`<>`

<!-- more -->

**Scala的foreach循环：**

``` Scala
// 第1种
args.foreach(arg=>println(arg))
// 第2种
args.foreach(println)
// 第3种
for(arg<-args)
    println(arg)
```

如果一个方法只有一个参数，你可以不用`.`和`()`来调用这个方法。例如for循环：

``` Scala
for(i <- 0 to 2) { }
```

`0 to 2`等价于`(0).to(2)`，`1+2`等价于`(1).+(2)`  （类似于Ruby）

`arr(1)`等价于`arr.apply(1)`，`arr(1)=x`等价于`arr.update(1, x)`

List是不可修改的序列，`:::`：连接两个List，`::`：向List里添加元素    （实际上是创建新List）

Tuple与List都是不可修改的序列，Tuple可以包含不同类型的数据，List只能包含同类型的数据

使用`._`访问Tuple的元素

Set和Map分`mutable`（可变的）和`immutable`（不可变的）类型

定义Map：

``` Scala
val map = Map(key1 -> value1, key2 -> value2)
```

Scala使用`_`而不是`*`来引入多个类
