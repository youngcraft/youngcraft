# gdb attach fails with ptrace: Operation not permitted
Today I ran into a weird problem. I could not attach to my own process with gdb. The process ran under my UID, but gdb refused to attach. This is a problem of wrong permissions, although /proc/[pid]/status looked ok:


```
Uid:    1000    1000    1000    1000
Gid:    1000    1000    1000    1000
```
1
2
3
4
```
Uid:    1000    1000    1000    1000
Gid:    1000    1000    1000    1000
```
I am the owner but cannot attach? Well, I launched gdb as root and could attach. Strange. Without digging deeper into this, my dirty workaround was this:


sudo chmod +s /usr/bin/gdb
1
sudo chmod +s /usr/bin/gdb

Update: Thanks to Mario, who pointed out, that the reason is the Kernel hardening stuff build into the Ubuntu kernel. See his comment how to fix the problem permanently.

why i cannot get a really address for 
    #
    
    #0  0xf76f9c89 in __kernel_vsyscall ()
    #1  0xf75fbb13 in __read_nocancel () at ../sysdeps/unix/syscall-template.S:84
    #2  0x080484df in vulnerable_function ()
    #3  0x0804851d in main ()
    #4  0xf753e637 in __libc_start_main (main=0x804850a <main>, argc=0x1, 
    argv=0xffde0464, init=0x8048540 <__libc_csu_init>, 
    fini=0x80485b0 <__libc_csu_fini>, rtld_fini=0xf770a8a0 <_dl_fini>, 
    stack_end=0xffde045c) at ../csu/libc-start.c:291
    #5  0x08048411 in _start ()
    #