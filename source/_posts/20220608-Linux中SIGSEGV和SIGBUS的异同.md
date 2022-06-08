---
title: 20220608-Linux中SIGSEGV和SIGBUS的异同
date: 2022-06-08 08:48:08
tags:
---

SIGSEGV和SIGBUS的异同

# 1.官方说法

（1）官方说法是： 
SIGSEGV --- Segment Fault. The possible cases of your encountering this error are: 

1.buffer overflow --- usually caused by a pointer reference out of range. 
//缓冲区溢出---通常由指针引用超出范围引起。

2.stack overflow --- please keep in mind that the default stack size is 8192K. 
//堆栈溢出---请记住默认堆栈大小是8192K。

3.illegal file access --- file operations are forbidden on our judge system.
//非法文件访问---我们的裁判系统禁止文件操作。

# 2.两者区别

(1) SIGBUS(Bus error)意味着指针所对应的地址是有效地址，但总线不能正常使用该指针。通常是未对齐的数据访问所致。

(2) SIGSEGV(Segment fault)意味着指针所对应的地址是无效地址，没有物理内存对应该地址。
 
# 3.Linux的mmap(2)手册页中涉及两个信号量
--------------------------------------------------------------------------
使用映射可能涉及到如下信号

SIGSEGV

    试图对只读映射区域进行写操作

SIGBUS 

    试图访问一块无文件内容对应的内存区域，比如超过文件尾的内存区域，或者以前有文件内容对应，现在为另一进程截断过的内存区域。
--------------------------------------------------------------------------
# 4.1 Linux下简单操作步骤

弄清楚错误以后，就要查找产生错误的根源，一般用以下两种方法：
（1）gcc -g 编译 
     ulimit -c 20000 
     之后运行程序，等core dump 
     最后gdb -c core <exec file> 
	 来查调用栈
（2）使用strace execfile，运行程序，出错时会显示那个系统调用错

