---
title: TensorFlow之MNIST入门
mathjax: true
date: 2018-03-11 20:53:41
---


MNIST手写数字识别是机器学习中非常经典的问题，相当于编程语言界的“Hello World“。关于神经网络解决MNIST手写数字识别问题，可以参考这个视频：[深度学习之神经网络的结构 Part 1 ver 2.0](https://www.bilibili.com/video/av15532370/)

视频中使用的是多层神经网络，为了简化问题，这里我们使用单层的网络结构。

参考之前的[MNIST数据集解析](201803080.html)，先对MNIST数据集进行解析：

<!-- more -->

``` Python
import gzip
import struct
import numpy as np
import matplotlib.pyplot as plt
import tensorflow as tf

def load_images(image_gz):
    with gzip.open(image_gz) as f:
        buf = f.read()
    num = int(struct.unpack_from('>i', buf, 4)[0])
    return (np.array(struct.unpack_from('B'*num*28*28, buf, 16)
                     ).reshape(num, 784)/255).astype(np.float32)

def load_labels(label_gz):
    with gzip.open(label_gz) as f:
        buf = f.read()
    num = int(struct.unpack_from('>i', buf, 4)[0])
    idx = 8
    tmp = []
    for i in range(num):
        label = int(struct.unpack_from('B', buf, idx)[0])
        idx += 1
        # one-hot encoding
        ohl = np.zeros(10, dtype=np.float32)
        ohl[label] = 1.0
        tmp.append(ohl)
    return np.array(tmp)

train_images = load_images('train-images-idx3-ubyte.gz')
train_labels = load_labels('train-labels-idx1-ubyte.gz')
test_images = load_images('t10k-images-idx3-ubyte.gz')
test_labels = load_labels('t10k-labels-idx1-ubyte.gz')
```

在读取图片时，一次性读取二进制数据，这样可以大大提升效率。之后，为了使用的方便，将它变形为num\*784大小，由于图片都是28\*28大小，所以单张图片的像素数就是784。另外，还将像素值进行了归一化，因为，如果输入层的值很大，在反向传播时传递到输入层的梯度就会很大，如果梯度非常大，学习率就必须非常小，否则就会跳过局部最小（直接表现就是代价函数的值为nan）。因此，**如果用梯度下降来训练模型一般都要在数据预处理步骤进行数据归一化**。

对于离散的特征一般按照one-hot编码，该离散特征有多少取值，就用多少维度来表示该特征。在回归、分类、聚类等机器学习算法中，特征之间距离的计算或相似度的计算是非常重要的，使用one-hot编码，特征之间的距离更为合理。

**基于树的方法不需要特征归一化，基于参数或距离的模型要进行特征归一化**。

``` Python
X = tf.placeholder(tf.float32, (None, 784))
Y = tf.placeholder(tf.float32, (None, 10))

W = tf.Variable(tf.truncated_normal((784, 10), stddev=0.01))
b = tf.Variable(tf.zeros((10,)))
y = tf.nn.softmax(tf.matmul(X, W) + b)

cost = -tf.reduce_sum(Y*tf.log(tf.clip_by_value(y, 1e-10, 1.0)))
```

Softmax函数一般用于多分类问题，可以对预测的标签进行归一化。计算公式为：

$$ S_i = \frac {e^{x_i}} {\sum e^{x}} $$

下面举例说明计算过程：

``` Python
a = tf.constant(np.array([
    [6., 1., 0.],
    [0., 4., 2.]
]))
b = tf.nn.softmax(a)

with tf.Session() as sess:
    print(sess.run(b))
# Output:
[[ 0.99086747  0.00667641  0.00245611]
 [ 0.01587624  0.86681333  0.11731043]]
```

使用Linux自带的计算器bc进行手动计算的过程（其中`e(x)`表示`exp(x)`）：

```
$ bc -lq
e(6)/(e(6)+e(1)+e(0))
.99086747258217259526
e(1)/(e(6)+e(1)+e(0))
.00667641251337645118
e(0)/(e(6)+e(1)+e(0))
.00245611490445095354
e(0)/(e(0)+e(4)+e(2))
.01587623997646676632
e(4)/(e(0)+e(4)+e(2))
.86681333219733487114
e(2)/(e(0)+e(4)+e(2))
.11731042782619836253
```

使用的代价函数为：

$$ C = -\sum [Y\log(y)] $$

由于y可能有元素值为0，造成log(y)无意义，从而使得代价函数的值为nan，所以这里使用`tf.clip_by_value`对其值进行修剪，设定值的下限和上限。

为了说明cost的计算，列举一些例子：

``` Python
a = tf.constant(np.array([
    [1., 0., 0., 0., 0.],
    [0., 0., 1., 0., 0.]
]))
b = tf.constant(np.array([
    [0.8, 0.1, 0.1, 0., 0.],
    [0., 0., 0.9, 0., 0.1]
]))
c = -tf.reduce_sum(a*tf.log(tf.clip_by_value(b, 1e-10, 1.)))
with tf.Session() as sess:
    print(sess.run(c))
# Output:
0.328504066972
```

这个的计算式子是：

```
-1*(
    1.0*log(0.8)+0.0*log(0.1)+0.0*log(0.1)+0.0*log(0.0)+0.0*log(0.0) +
    0.0*log(0.0)+0.0*log(0.0)+1.0*log(0.9)+0.0*log(0.0)+0.0*log(0.1)
)
=-1*(log(0.8+log(0.9)))
```

reduce_sum也能在某个axis上进行计算：

``` Python
a = tf.constant(np.array([
    [1, 2, 3],
    [4, 5, 6]
]))
print(a.shape)
b = tf.reduce_sum(a)
c = tf.reduce_sum(a, axis=0)
d = tf.reduce_sum(a, axis=1)

with tf.Session() as sess:
    print(sess.run(b))
    print(sess.run(c))
    print(sess.run(d))
# Output:
(2, 3)
21
[5 7 9]
[ 6 15]
```

默认情况下，reduce_sum会对所有axis进行计算，得到的结果是一个标量。在上面的例子里，由于a的shape是(2, 3)， 所以，当对axis=0进行计算时，结果的shape为(3,)，当对axis=1进行计算时，结果的shape为(2,)。对某个axis进行计算时，结果的shape就是把源的shape的axis为索引所在位置的值去掉，剩余的结果就是结果的shape。

接下来就是训练过程：

``` Python
init = tf.global_variables_initializer()
train = tf.train.GradientDescentOptimizer(0.01).minimize(cost)

with tf.Session() as sess:
    sess.run(init)
    for i in range(1000):
        batch = np.random.choice(np.arange(60000), 100)
        tx, ty = train_images[batch], train_labels[batch]
        sess.run(train, feed_dict={X: tx, Y: ty})
        if (i + 1) % 50 == 0:
            print("Epoch:", i + 1, "Cost:", sess.run(cost, feed_dict={
                X: train_images, Y: train_labels}))

    # test model
    correct = tf.equal(tf.argmax(Y, 1), tf.argmax(y, 1))
    accuracy = tf.reduce_mean(tf.cast(correct, tf.float32))
    print("Accuracy:", sess.run(accuracy, feed_dict={X: test_images, Y: test_labels}))
```

为了加快训练速度，每轮迭代时并不使用所有数据集进行训练，而是每次随机选取一部分数据集进行训练（随机梯度下降）。

`tf.argmax`可以获取指定维度的最大值所在的索引；`tf.cast`可以转换dtype。

经过以上的训练过程，最终得到的准确率大概是91%，效果还行，大部分的预计是正确的，也会偶尔出现错误：

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20180311.gif)
