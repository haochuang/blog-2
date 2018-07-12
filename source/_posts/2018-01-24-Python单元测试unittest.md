---
title: Python单元测试unittest
date: 2018-01-24 21:29:56
---

## 执行流程

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20170124b.png)

<!-- more -->

## 使用要点

一个 testcase 类应该派生自 `unittest.TestCase`

`setUpClass()`、`tearDownClass()`必须使用`@classmethod`装饰器

`assertEqual`一般first是预期值，second是实际值。

**断言抛出异常**

```Python
with self.assertRaises(SomeException):
    do_something()
```

**assertTrue、assertFalse**

`assertTrue`实际上是断言 bool(expr) 为 True 。同理，`assertFalse`实际上是断言 bool(expr) 为 False 。
因此，若要断言 expr 为 True 或 False，不要用`assertTrue`或`assertFalse`。

**命令行执行unittest**

```Python
# 运行test_module
python -m unittest test_module
# 运行TestClass
python -m unittest test_module.TestClass
# 运行test_method
python -m unittest test_module.TestClass.test_method
```
