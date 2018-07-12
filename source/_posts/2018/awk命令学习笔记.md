---
title: awk命令学习笔记
date: 2018-03-10 23:29:11
---



## 执行流程

awk命令基本结构：

``` Shell
awk 'BEGIN{ commands } pattern { commands } END{ commands }' file
```

【选项】

`-F`：输入域分隔符

`-v`：自定义变量

`-f`：调用awk脚本

【执行流程】

<!-- more -->

(1) 执行BEGIN{ commands }；

(2) 从文件或stdin中读取一行，执行pattern { commands }。重复这个过程直至读取完毕；

(3) 执行END{ commands }

## 特殊变量

`NR`：记录数量，也就是已读的行号

`NF`：字段数量

`$0`：当前行文本

`$1`：第一个字段的文本

`FS`：输入域分隔符

`OFS`：输出域分隔符

## pattern

`/pattern/`：仅处理匹配的行；`!/pattern/`：仅处理不匹配的行

pattern部分也可以是条件。例如：

``` Shell
$ awk -F : 'NR<3' /etc/passwd
root:x:0:0:root:/root:/usr/bin/zsh
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
```

## 数组

awk的数组使用方式有点特别。awk数组是关联数组，引用不存在的索引时会自动使用默认值。

``` Shell
$ echo | awk '{arr[0]++;print arr[0], arr[1], arr[2]==0}'
1  1
```

## 内置函数

`printf(...)`：格式化输出；

`length(s)`：字符串长度;

`gsub(r, s, [t])`：在t字符串搜索r匹配的内容并全部替换为s

`getline`：读取一行（文件游标也会移动一行）

## 其它语法

awk支持if语句、switch语句、while循环、for循环、for in 循环。

awk支持算数、赋值、比较、逻辑和模式匹配运算符。

`~`：匹配；`!~`：不匹配
