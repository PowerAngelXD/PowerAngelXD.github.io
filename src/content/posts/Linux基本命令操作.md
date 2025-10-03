---
title: Linux基本命令操作
published: 2025-10-03
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
**使用效果：**

![../img/LinuxCmd/cat.png](../img/LinuxCmd/cat.png)

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
**使用效果：**

![../img/LinuxCmd/cat2.png](../img/LinuxCmd/cat2.png)


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

**使用效果：**

![../img/LinuxCmd/touch.png](../img/LinuxCmd/touch.png)

### 3. `file` 命令
file命令用于确定对应文件的文件类型，在没有文件拓展名的情况下依然适用: 
```
file [file-name]
```

**使用效果：**

![../img/LinuxCmd/file.png](../img/LinuxCmd/file.png)

### 4. `cd` 命令
cd命令用于更改当前工作目录

**使用效果：**

![../img/LinuxCmd/cd.png](../img/LinuxCmd/cd.png)

### 5. `pwd` 命令
该命令用于输出当前的工作目录

**使用效果：**

![../img/LinuxCmd/pwd.png](../img/LinuxCmd/pwd.png)

### 6. `mkdir` 命令
该命令用于在当前工作目录下创建一个目录

**使用效果：**

![../img/LinuxCmd/mkdir.png](../img/LinuxCmd/mkdir.png)

#### 参数（参数支持组合）：

`-p`：该用法可以创建多级目录，即使父目录不在的情况下也能创建

**使用效果：**

![../img/LinuxCmd/mkdir2.png](../img/LinuxCmd/mkdir2.png)

`-v`：该用法可以在创建目录的时候输出对应信息

**使用效果：**

![../img/LinuxCmd/mkdir3.png](../img/LinuxCmd/mkdir3.png)

### 7. `rmdir` 命令
rmdir只能用于用于删除空的目录

**使用效果：**

![../img/LinuxCmd/rmdir.png](../img/LinuxCmd/rmdir.png)

而当你尝试用rmdir删除一个非空目录的时候，会报错：

![../img/LinuxCmd/rmdirerr.png](../img/LinuxCmd/rmdirerr.png)

***其中，t1是我们先前用于演示 mkdir -p 时的产物（t1里面还有其他目录）***

### 8. `stat` 命令
该命令用于显示文件或者文件系统的详细信息

**使用效果：**

![../img/LinuxCmd/stat.png](../img/LinuxCmd/stat.png)

#### 参数（支持参数组合）
`-f`：显示文件所在文件系统的信息

![../img/LinuxCmd/stat2.png](../img/LinuxCmd/stat2.png)

`-t`：以简洁方式输出

![../img/LinuxCmd/stat3.png](../img/LinuxCmd/stat3.png)

### 9. `more` 命令
该命令用于查看文本文件（且文件内容量大需要翻页的时候）

**使用效果：**

![../img/LinuxCmd/more.png](../img/LinuxCmd/more.png)

在下方，会有一个百分比表示已显示的内容占全文的比例，因此翻页之后会看到这个百分比会上涨

#### 参数
`+N`：实例用法：
```bash
more +N [file]
```
表示从第N行开始显示

**使用效果：**

![../img/LinuxCmd/more2.png](../img/LinuxCmd/more2.png)
![../img/LinuxCmd/more3.png](../img/LinuxCmd/more3.png)

`-N`：实例用法：
```bash
more -N [file]
```
表示一次想查看N行

**使用效果：**

![../img/LinuxCmd/more2.png](../img/LinuxCmd/more2.png)
![../img/LinuxCmd/more3.png](../img/LinuxCmd/more4.png)

### 10. `less` 命令
与more类似，也是一个可以查看大文本的工具，但是less更倾向于单独开了一个窗口，退出的时候需要按q键进行退出

### 11. `head` 命令
指定显示文件的前N行（默认前10行）

**使用效果：**

![../img/LinuxCmd/head.png](../img/LinuxCmd/head.png)

#### 参数
`-N`：指定显示前N行：
```bash
head -N [file]
```
**使用效果：**

![../img/LinuxCmd/head2.png](../img/LinuxCmd/head2.png)

### 12. `tail` 命令
与上一个命令相反，tail命令显示文件的倒数N行

**使用效果：**

![../img/LinuxCmd/tail.png](../img/LinuxCmd/tail.png)

至于参数方面，与head类似，进行类比即可
### 13. `ln` 命令
用于为文件创建链接

插入：软硬链接

在Linux中，inode号才是识别文件数据存储位置的方法，它不等同于文件名称，而是独属于文件的标识符

**硬链接**

指向相同的inode的链接：

![../img/LinuxCmd/hardln.png](../img/LinuxCmd/hardln.png)

**使用效果：**

![../img/LinuxCmd/hardln2.png](../img/LinuxCmd/hardln2.png)

此时，我们若输入 `ls -li` 命令，即可查看这些硬链接和他们的文件之间的关系：

![../img/LinuxCmd/hardln3.png](../img/LinuxCmd/hardln3.png)

***需要注意的是，a_hln和b_hln均为对应文件a，b的硬链接***

实质上，每创建一个硬链接，实质是多了一个指向这个inode的一个指针，其机制有点类似于C++smart_ptr中的ARC（引用计数）的垃圾回收机制，当这个inode引用次数降为0时，这个文件也就真的被删除了

**软链接**

这是一种特殊类型的文件，它可以将一个新的文件名关联到另一个文件上，并使通过这个新的文件名访问源文件是可行的；但是不共享inode，所以他们两个之间的访问权限，所有者和大小这些属性可能会不同

软链接的一个很大的优势就是它可以跨文件系统或者分区进行创建，非常方便

软连接创建方法：
```bash
ln -s [src] [sym-link]
```
### 14.rm