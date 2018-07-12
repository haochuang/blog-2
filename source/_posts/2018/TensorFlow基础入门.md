---
title: TensorFlow基础入门
date: 2018-03-07 15:53:56
---


## TensorFlow安装

CPU版本直接`pip install tf-nightly`即可。

GPU版本需要安装显卡驱动、cuda、cudnn，注意版本。若手动安装cuda还要将cuda的lib64目录加入`LD_LIBRARY_PATH`环境变量。然后使用`pip install tf-nightly-gpu`安装即可。

在Python中一般使用

``` Python
import tensorflow as tf
```

来import tensorflow。

<!-- more -->

## Graph（计算图）

Graph是TensorFlow的基本计算模型。TensorFlow会维护一个默认的计算图，可以通过`tf.get_default_graph()`来获取当前默认的计算图。通过`tf.Graph()`可以创建计算图。

## Session（会话）

通过Session来执行定义好的计算。使用会话一般有两种模式：

``` Python
# 第一种
sess = tf.Session()
sess.run(...)
sess.close()

# 第二种
with tf.Session() as sess:
    sess.run(...)
```

## Tensor（张量）

Tensor主要有三个属性：name、shape、dtype。

name的形式为`<op_name>:<output_index>`，op\_name为节点名，output\_index为该张量是计算节点输出的第几个结果（从0开始）。

Tensor用于引用中间计算结果和获得计算结果。

### constant

constant就是常量，在创建时赋初值且值不再变化。

``` Python
>>> a = tf.constant(5, name='a')
>>> a
<tf.Tensor 'a:0' shape=() dtype=int32>
>>> tf.Session().run(a)
5
```

### Variable

Variable一般用于保存和更新神经网络中的参数。可以使用`assign`为Variable赋值。

Variable在使用前要使用`initializer`进行初始化，使用`tf.global_variables_initializer()`可以初始化所有变量（注意：需要在Session中run一下这个初始化操作！）

``` Python
state = tf.Variable(0)
one = tf.constant(1)
add = tf.add(state, one)
opt = tf.assign(state, add)

init = tf.global_variables_initializer()

with tf.Session() as sess:
    sess.run(init)
    for _ in range(10):
        sess.run(opt)
        print(sess.run(state))
```

### placeholder

placeholder是占位符，用来提供输入数据。通过`sess.run()`的`feed_dict`参数为placeholder传入值。

``` Python
feed = tf.placeholder(tf.float32)
print(tf.Session().run(feed, feed_dict={feed: 3.0}))
```
