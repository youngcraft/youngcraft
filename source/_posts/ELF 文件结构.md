---
layout:     post
title:      ELF 文件结构
subtitle:   CTF基础系列
date: 2020-08-04 09:50:08
author:     gumeng
categories: Linux
---

# 前言

基础概念，比如ELF文件结构，包括相关安全学习，IoT相关物联网

# 从ELF文件结构开始

自己bing、google了多久，把能找到有用的连接挂在上去

>http://blog.csdn.net/alan0521/article/details/7689865
>https://en.wikipedia.org/wiki/Executable_and_Linkable_Format
>http://flint.cs.yale.edu/cs422/doc/ELF_Format.pdf


主要参考耶鲁大学课程中的pdf，翻译成中文

## Linux 文件系统

linux文件类型

- 可重定位的目标文件（Relocatable，或者Object File）
- 可执行文件（Executable）
- 共享库（Shared Object，或者Shared Library）

linux文件两种视角，与ctf结合需要你了解的部分：

![](http://i.imgur.com/JbotCkK.png)

我们日常的文件头结构，以64位下elf编译的helloworld为例，工具是以readelf：

![](http://i.imgur.com/3LcpwS1.png)

具体头文件格式参考，参考源码解读，参照c源码

elf.h 在ubuntu下 /usr/include/elf.h

该文件有3000+行，估计看到的人会崩溃：

![](http://i.imgur.com/7QtLFBP.png)

今天不是解析elf.h 先专注我们关注的


An ELF header resides at the beginning and holds a ‘‘road map’’ describing the file’s organization. Sections hold the bulk of object file information for the linking view: instructions, data, symbol table, relocation information, and so on. Descriptions of special sections appear later in Part 1. Part 2 discusses segments and the program execution view of the file.

elf 头结构表示的是我们如何加载elf文件，包含如何加载的格式、系统、符号表，数据段，代码段，重定向段等等。

下图是elf 64 位下section字段名称

> 
> 如何在64系统中编译出一个32位程序
> 
> gcc test.c -m32 -o test 
> 
> 这样就可以在64位系统中编译32位程序

段图，以64位编译环境下的hello为例:
![](http://i.imgur.com/FJHtUrs.png)

可以的看到，有28个端，其中第一个0为空段，我们日常熟悉的pwn，日常中我们一般调用.data/.bss/.text 字段

这些内容就略过，重点关注程序加载到内存的方式，略过了：

- 程序文件详细格式，在硬盘中的文件组织方式
- 程序链接时，即.o文件

32位的系统和64位系统相当的不同，回头再写个博客整理整理,思路比较乱


----------


##


# ELF文件加载过程










