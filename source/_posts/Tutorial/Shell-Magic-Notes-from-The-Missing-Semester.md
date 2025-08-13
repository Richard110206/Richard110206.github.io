---
title: 'Shell Magic: Notes from MIT'
date: 2025-07-27 22:34:30
tags: [shell]
index_img:  /medias/Shell Magic Notes from The Missing Semeste.jpeg
category: Tutorial
category_bar: true
---
This document summarizes my key takeaways from the Shell module of MIT’s The Missing Semester course, focusing on:Interaction and Automation Power!

<!-- more -->

>对计算机的学生而言，许多开发工具是未来老师或企业默认你已经“精通”的，然而未经学校系统培训的我们或许会对此感到迷茫，正如 MIT 课程 **[The Missing Semester of Your CS Education](https://missing.csail.mit.edu/)** 所强调的： **“掌握基础工具是程序员的核心竞争力”**。这个课程会帮助你入门、了解一些计算机常用的开发工具，也算是程序员的自我修养吧！**Let's Go！**:smile:

相关课程在b站也有双语翻译（大部分应该都能听懂，就是有个好像是西班牙的老师带点口音听起来有点不适应:cry:）[[自制双语字幕] 计算机教育缺失的一课(2020) - 第1讲 - 课程概览与 shell](https://www.bilibili.com/video/BV1uc411N7eK/?spm_id_from=333.1387.favlist.content.click&vd_source=54c2981c1a7a8e0433b7d23096150b7a)
## Shell
&emsp;&emsp;当熟悉的可视化界面无法实现你想要的功能时，`Shell`将成为你与计算机交互的强大工具。相比图形界面有限的按钮和滑块，命令行提供了更灵活、更强大的控制方式，允许你通过文本命令和脚本实现自动化操作。本文基于`VMware`虚拟机中的`Ubuntu 22.04`发行版演示基础`Shell`操作：

```bash
richard@richard-VMware-Virtual-Platform:~$ date
Sat Jul 26 03:27:54 PM CST 2025
```
第一行通常是用户名、机器名称和当前所在路径
- `Windows`中路径通常使用**反斜杠**`\`分隔，每个驱动器都有一个单独的路径结构。
- `Linux` / `macOS` 中这些路径使用**正斜杠**`/`分隔，他们挂载在同一个命名空间下。

开头的斜杠表示从文件系统的顶部开始。
- 相对路径：相对于当前位置的路径
- 绝对路径：完整路径，从顶部开始
```bash
richard@richard-VMware-Virtual-Platform:~$ echo "Hello World"
Hello World
```
`echo`可以将后面的参数打印在屏幕上
```bash
richard@richard-VMware-Virtual-Platform:~$ which echo
/usr/bin/echo
```
`which`可以用于寻找后面参数文件的绝对位置
```bash
richard@richard-VMware-Virtual-Platform:~$ pwd
/home/richard
```
`pwd`（`Print Working Directory`）打印当前所在路径
```bash
richard@richard-VMware-Virtual-Platform:~$ cd /home
richard@richard-VMware-Virtual-Platform:/home$ 
```
`cd`（`Change Directory`）表示更改当前目录

这是更方便切换目录的方式：
- `.`表示**当前**目录
- `..`表示**父**目录

**注意**：使用相对路径能提高命令在不同电脑上的**兼容性**！
```bash
richard@richard-VMware-Virtual-Platform:/$ ls
bin                home               mnt   sbin.usr-is-merged  usr
bin.usr-is-merged  lib                opt   snap                var
boot               lib64              proc  srv
cdrom              lib.usr-is-merged  root  swap.img
dev                lost+found         run   sys
etc                media              sbin  tmp
```
`ls`会列出当前目中的文件，这能够快速浏览文件
```bash
richard@richard-VMware-Virtual-Platform:/home$ cd ~
richard@richard-VMware-Virtual-Platform:~$
```
- `cd ~`能够快速回到用户主目录
- `cd -`切换到上一个工作目录

```bash
richard@richard-VMware-Virtual-Platform:~$ ls --help
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.
```
`--help`参数显示命令帮助信息

```bash
richard@richard-VMware-Virtual-Platform:~$ ls -l
total 40
drwxr-xr-x 2 richard richard 4096 Mar  9 10:21 Desktop
drwxr-xr-x 2 richard richard 4096 Mar  9 10:21 Documents

lrwxrwxrwx   1 root root          7 Apr 22  2024 bin -> usr/bin
drwxr-xr-x   2 root root       4096 Feb 26  2024 bin.usr-is-merged
# 仅列举部分进行展示说明
```
**文件类型**（第`1`个字符）

| 字符 | 类型       | 说明                  | 示例                  |
|------|------------|-----------------------|-----------------------|
| `d`  | 目录       | 文件夹                | `drwxr-xr-x` |
| `-`  | 普通文件   | 文本/二进制文件       | `-rw-r--r--` |
| `l`  | 符号链接   | 软链接文件            | `lrwxrwxrwx` |
| `c`  | 字符设备   | 终端等串行设备        | `crw--w----` |
| `b`  | 块设备     | 磁盘等存储设备        | `brw-rw----` |
| `s`  | 套接字     | 进程通信文件          | `srwxrwxrwx` |
| `p`  | 管道       | `FIFO` 管道文件         | `prw-------`|

用户权限分为 **3组**（每组`3`字符），分别对应：

| 组别       | 示例          | 
|------------|---------------|
| 所有者权限 | `rwx`         | 
| 所属组权限 | `r-x`         | 
| 其他用户   | `r-x`         | 

权限字符**含义**：

 字符 | 权限 | 对文件的影响             | 对目录的影响              |
|------|------|--------------------------|---------------------------|
| `r`  | 读   | 查看文件内容             | 列出目录内容（`ls`）      |
| `w`  | 写   | 修改/删除文件            | 创建/删除目录内文件       |
| `x`  | 执行 | 运行程序/脚本            | 进入目录（`cd`）          |
| `-`  | 无   | 无权限                   | 无权限                    |

**注意**：
1. 即使文件本身有 `w` 权限，如果所在目录没有` w`，你只能**清空或删除文件内容**，却仍然**无法删除或重命名**它
2. 如果我想要访问一个目录，**必须拥有其所有父目录的权限**：`/usr/bin/echo`我必须拥有`/usr` `/bin`目录的权限，否则我将不被允许访问该文件，因为我将无法进入其中的目录

`mv`能将文件修改位置并重命名
`cp`复制文件 需要两个参数 一个是要复制的文件路径 一个目标文件路径
`rm`用于删除文件
`rmdir`能删除一个目录 但它只能删除空目录 防止不小心删除一大堆文件
`mkdir`创建目录
```bash
mkdir My Photos
```
`mkdir`创建目录
```bash
mkdir "My Photos"
```
这里如果不加上上引号则会创建两个目录
`man+指令` 查找手册
`ctrl+l`清空窗口
### 输入流和输出流
```bash
richard@richard-VMware-Virtual-Platform:~$ echo hello > hello.txt
```
`>`覆盖输出到文件
`<`从文件读取输入
```bash
richard@richard-VMware-Virtual-Platform:~$ cat hello.txt
hello
```
通过`cat`指令输出文件内容
```bash
richard@richard-VMware-Virtual-Platform:~$ cat <hello.txt>hello2.txt
richard@richard-VMware-Virtual-Platform:~$ cat hello2.txt
hello
```
`<`将`hello.txt`作为输入内容并通过`>`将`cat`打印的任何内容输出到`hello2.txt`文件中
```bash
richard@richard-VMware-Virtual-Platform:~$ cat <hello.txt>>hello2.txt
richard@richard-VMware-Virtual-Platform:~$ cat hello2.txt
hello
hello
```
`>>`表示追加而不是覆盖(`append instead of overwrite`)
```bash
richard@richard-VMware-Virtual-Platform:~$ ls -l|tail -n1
drwxrwxr-x 3 richard richard 4096 Mar  9 12:26 web
```
管道运算符`|`作用是将**左侧命令的输出**作为**右侧命令的输入**：可以连接多个命令形成**处理流水线**

`tail`将输出最后一行的内容
### 用户权限管理
`root`相当于`Windows`的管理员权限，是一位**超级用户**，可以做任何想做的事情，倘若一直使用`root`用户操作计算机，如果运行了错误的程序，那会彻底毁坏你的电脑！
```bash
richard@richard-VMware-Virtual-Platform:/sys/class/backlight$ sudo su
[sudo] password for richard: 
root@richard-VMware-Virtual-Platform:/sys/class/backlight# 
```
通过`sudo su`来进入`root`身份，同时提示符由`$`变为`#`
```bash
root@richard-VMware-Virtual-Platform:/sys/class/backlight# exit
exit
richard@richard-VMware-Virtual-Platform:/sys/class/backlight$
```
`exit`来退出用户权限
```bash
richard@richard-VMware-Virtual-Platform:~$ xdg-open hello.txt
```
这样就可以直接在`terminal`打开文件！

```bash
richard@richard-VMware-Virtual-Platform:~$ foo=bar
richard@richard-VMware-Virtual-Platform:~$ echo $foo
bar
```
在`shell`中，`$`符号是一个特殊字符，主要用于变量展开。当你在变量名前面加上$时，`shell`会将其替换为该变量的值！

- ` foo` 会被`Shell`视为普通字符串
- `$foo` 告诉`Shell`这是一个需要展开的变量
```bash
ichard@richard-VMware-Virtual-Platform:~$ echo "Hello"
Hello
richard@richard-VMware-Virtual-Platform:~$ foo=World
richard@richard-VMware-Virtual-Platform:~$ echo "Hello foo"
Hello foo
richard@richard-VMware-Virtual-Platform:~$ echo "Hello $foo"
Hello World
```
在`shell`中我们一定要非常注意**空格**`space`的使用，它是用作不同参数间的分隔符，倘若多打一个可能就会出现报错！

```bash
richard@richard-VMware-Virtual-Platform:~$ false
richard@richard-VMware-Virtual-Platform:~$ echo $?
1
richard@richard-VMware-Virtual-Platform:~$ true
richard@richard-VMware-Virtual-Platform:~$ echo $?
0
```
`$? `是一个特殊变量，存储上一个命令的退出状态码
- `false` 是一个 Shell 内置命令，返回非零退出状态（失败）设置退出状态码为 1（表示失败）
- `true` 也是一个 Shell 内置命令，返回零退出状态（成功）退出状态码被设为 0（表示成功）

&emsp;&emsp;这里看上去有点反常理，我们学的其他编程语言中`0`是`false`，非零是`true`，为什么这里反过来了？
&emsp;&emsp;当我们想到`c`、`c++`中`main`**函数结束的返回值**或许就不奇怪了！`main() `函数默认返回`0` 表示成功，非零表示错误（如 `return 1`;）。

再考虑到程序运行只有两种可能：
1. 完全成功（**唯一状态 0**）
2. 失败（可能有多种原因，**用不同非零值区分**）：
- 0 = 没有错误（成功）
- 1 = 通用错误
- 2 = 参数错误
- 3 = 文件不存在

```bash
richard@richard-VMware-Virtual-Platform:~$ false || echo "Oops fail"
Oops fail
richard@richard-VMware-Virtual-Platform:~$ true || echo "Will be not be printed"
richard@richard-VMware-Virtual-Platform:~$
```
`shell`中`||`同样遵循**短路原则**，`&&`同理！
```bash
richard@richard-VMware-Virtual-Platform:~$ echo "We are now at $(pwd)"
We are now at /home/richard
```
`shell`中使用`$(command)`进行**命令替换**

```bash
richard@richard-VMware-Virtual-Platform:~$ ls
Desktop    Downloads   hello.txt  Pictures  snap       Videos
Documents  hello2.txt  Music      Public    Templates  web
richard@richard-VMware-Virtual-Platform:~$ ls *.txt
hello2.txt  hello.txt
```
`*`是一个**通配符**（`globbing`）：表示**匹配任意数量的任意字符**（**包括零个字符**）。`Shell`会在执行`ls` 前先展开 `*.txt`，将其替换为所有匹配的文件名，相当于寻找所有后缀为`.txt`的文件！
```bash
richard@richard-VMware-Virtual-Platform:~$ ls hello?.txt
hello2.txt
richard@richard-VMware-Virtual-Platform:~$ ls hello*.txt
hello2.txt  hello.txt
```
这里可以看到`?`也是**通配符**，表示匹配任意一个字符（区别是必须存在，**不能匹配空字符**）
```bash
#!/usr/bin/env python3 #自动查找python3
```
通过`Shell`调用`Python`脚本可以充分发挥两者的优势：`Shell`的**流程控制能力**结合`Python`的**丰富库函数**，但我们要在路径中找到并配置好`python`解释器。
```bash
import sys
```
由于`shell`默认不会执行`python`指令，我们需要添加库函数，来让`python`接受`shell`传递的参数

```bash
richard@richard-VMware-Virtual-Platform:~$ history
```
我们可以使用:arrow_up:来一个个**浏览历史记录命令**，我们也可以使用`history`直接打印所有历史命令！
```bash
richard@richard-VMware-Virtual-Platform:~$ history | grep git
```
我们通过`history +N | grep +指令`来查找所有与指令字符串相匹配的`N`条历史记录命令！
```bash
tree
```
能列出**目录结构性**的位置！
