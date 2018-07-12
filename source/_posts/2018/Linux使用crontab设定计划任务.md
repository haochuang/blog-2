---
title: Linux使用crontab设定计划任务
date: 2018-01-24 21:45:19
index: 1
---

## crontab命令用法

`crontab -e` 编辑计划任务

`crontab -l` 列出计划任务

`crontab -r` 移除计划任务

使用`select-editor`命令可以选择编辑器，也可以通过`EDITOR`环境变量来设置默认编辑器：

```Shell
export EDITOR=/usr/bin/vim
```

基本原理是系统每分钟检查下crontab，然后执行生效的任务。

<!-- more -->

## 计划任务格式

```
*　　*　　*　　*　　*　　command
分　 时　 日　 月　 周　 命令 
```

【示例】

`30 23 * * * command`  每天23:30运行

`0 10-21/2 * * * command` 每天10:00~21:00每隔2小时运行

【符号说明】

`*` 所有值(每)

`,` 列举（和）

`-` 范围（至）

`/` 间隔（每隔）

## 注意要点

在 crontab 可以设置环境变量，如`PATH`等。

如果需要运行 GUI 程序，设置`DISPLAY=:0`。

如果要用终端模拟器执行某个脚本（这样可以看到脚本执行过程），命令可以写：

```Shell
x-terminal-emulator -x 脚本文件名
```

`x-terminal-emulator`会打开系统默认终端模拟器。

另外，计划任务会在命令执行时向用户发送邮件，可以使用`mail`命令查看。如果不想收到邮件，可以设置`MAILTO=''`。
