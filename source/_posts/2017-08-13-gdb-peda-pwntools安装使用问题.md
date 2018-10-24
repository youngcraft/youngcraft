# Peda 安装使用指南

参考连接：
[http://www.ropshell.com/peda/Linux_Interactive_Exploit_Development_with_GDB_and_PEDA_Slides.pdf](http://www.ropshell.com/peda/Linux_Interactive_Exploit_Development_with_GDB_and_PEDA_Slides.pdf "peda-pdf")




Peda的创作者是Long Le ，vns security的研究员，

## GDB-peda安装

1 需要安装packages 

> sudo apt-get install nasm micro-inetd

2 可选安装

> sudo apt-get install libc6-dbg vim ssh


3 安装peda-tool 

> Download peda.tar.gz at: http://ropshell.com/peda/

本地打开

> $ tar zxvf peda.tar.gz

创建 本地'.gdbinit'

> $ echo “source ~/peda/peda.py” >~/.gdbinit

本地可用的example

> Download bhus12-workshop.tar.gz at:

> http://ropshell.com/peda/



## pwntools error

Thanks for contributing to Pwntools!

When reporting an issue, be sure that you are running the latest released version of pwntools (pip install --upgrade pwntools).

Please verify that your issue occurs on 64-bit Ubuntu 14.04. You can use the Dockerfile on docker.io for quick testing.

    $ docker pull pwntools/pwntools:stable
    $ docker run -it pwntools/pwntools:stable

If possible, provide a proof-of-concept which demonstrates the problem. Include any binaries or scripts necessary to reproduce the issue, and please include the full debug output via setting the environment variable PWNLIB_DEBUG=1.


pwntool gdb attach 



82
down vote
accepted
In Maverick Meerkat (10.10) Ubuntu introduced a patch to disallow ptracing of non-child processes by non-root users - ie. only a process which is a parent of another process can ptrace it for normal users - whilst root can still ptrace every process. Hence why you can use gdb to attach via sudo still.

You can temporarily disable this restriction (and revert to the old behaviour allowing your user to ptrace (gdb) any of their other processes) by doing:

echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope
To permanently allow it edit /etc/sysctl.d/10-ptrace.conf and change the line:

kernel.yama.ptrace_scope = 1
To read

kernel.yama.ptrace_scope = 0
For some background on why this change was made, see the Ubuntu wiki

.plt 表示一个函数在libc中的偏移位置，如下代码:

.plt:080483A0 ; ssize_t write(int fd, const void *buf, size_t n)
.plt:080483A0 _write          proc near               ; CODE XREF: main+2Ap
.plt:080483A0                 jmp     ds:off_804A010
.plt:080483A0 _write          endp

指向了got.plt中的地址，got.plt则指向了plt的实际调用地址，外部调用函数偏移量：
.got.plt:0804A010 off_804A010     dd offset write         ; DATA XREF: _writer

外部函数的偏移量则为：

