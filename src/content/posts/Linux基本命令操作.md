---
title: Linux基本命令操作
published: 2025-08-06
description: ''
image: ''
tags: [Linux]
category: 'Linux学习'
draft: false 
---

# Linux命令基本操作

使用Linux时，不可避免地会使用到Linux的终端，因此我们需要对其中的命令进行熟悉，尤其是那些常用的命令
因此，本篇对常用的命令进行了收集和说明

### 1. `cat`  命令
cat命令作用是查看文件内容，格式为：
```
cat [file-name]
```
其它用法： 
#### 1.
```
cat [file-1] [file-2] ... > [file-n]
```
将file-1，-2，.....的内容连接起来，再写入文件file-n中
```
举个例子，假如有文件file-1，file-2，file-3
其中file-1内容为一个数字1，file-2内容为数字2，file-3内容为数字3

[input]           : cat file-1 file-2 > file-3
[file-3 content]  : 12
```


#### 2.
```
cat [file-1] [file-2] ... >> [file-n]
```
将file-1，-2，.....的内容连接起来，再 ****以追加的方式**** 写入文件file-n中
```
举个例子:

[input]           : cat file-1 file-2 >> file-3
[file-3 content]  : 312
```

> [!WARNING]
> 在使用上述命令时，应该以如下方式使用 (以第一个为例):
> ```
> sudo sh -c 'cat [file-1] [file-2] ... > [file-n]'
> ```
> 这个问题的原因是因为重定向 `> [file-n]` 的操作并不是由sudo执行而是shell，使用的是普通用户的权限
> 因此使用了sudo的指令只有前半段 `cat [file-1] [file-2] ...`，于是导致了问题的出现

### 2. `touch` 命令
touch命令作用是创建文件，格式为:
```
touch [file-1] [file-2] ...
```
运行后，系统会创建以touch命令之后的字段为名的文件

### 3. `file` 命令
file命令用于确定对应文件的文件类型，在没有文件拓展名的情况下依然适用: 
```
file [file-name]
```

### 4. `mkdir` 命令
mkdir命令作用是创建文件夹，格式为:
```
mkdir [dir-1] [dir-2] ...
```
运行后，系统会创建以mkdir命令之后的字段为名的文件夹








