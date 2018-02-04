---

layout:     post

title:      setuid 提权方法总结

subtitle:   linux pwn大师教程

date:       2018-01-15

author:     BY TALA

header-img: img/post-bg-re-vs-ng2.jpg

catalog:    true

tags:

    - Blog

---





> 自己收集部分内容，如有错误，请联系youngtala@gmail.com



# linxu setuid 安全威胁



## setuid 机制



一般查看linux下某命令权限

```

root@localhost# ll /etc/passwd 

-rwsr-xr-x 1 root root 22960 Jul 17 2017 /usr/bin/passwd

```

passwd命令有一个特殊的权限标记s ，存在于文件所有者的权限位上。这是一类特殊的权限SetUID ，当一个具有执行权限的文件设置SetUID权限后，用户执行这个文件时将以文件所有者的身份执行。



举例：passwd命令具有SetUID权限，所有者为root（Linux中的命令默认所有者都是root），也就是说当普通用户使用passwd更改自己密码的时候，那一瞬间会实际以root形式执行，实际在以passwd命令所有者root的身份在执行，root当然可以将密码写入/etc/shadow文件，命令执行完成后该身份也随之消失。



## setuid 安全威胁



setuid 可以理解为 临时 root用户的标志位，让普通用户可以以root身份临时打开所用的文件，因此我们可以借助这个标志位实现任意读写。



**应用场景**：



任何用户都用vi编辑任何文件，一般情况下非root用户不可编辑 /etc/shadow文件,如果对vi命令setuid后，任何用户对vi的操作即为root的读写权限操作，造成密码篡改或删除



## 测试



利用带有suid标志位的程序来实现提权，例如nmap就是





## suid标志位提权实现思路



### 如何发现文件系统中带有suid位



参考[如何发现suid位的程序](https://docs.oracle.com/cd/E19683-01/806-4078/6jd6cjs37/index.html)

参考[Linux下权限设置方法](https://www.linux.com/learn/understanding-linux-file-permissions)



```

bash-4.3# find / -user root -perm -4000 -exec ls -ldb {} \; > /tmp/ckprm

bash-4.3# cat ckprm

-rwsr-xr-x 1 root root 2302 May  7  2017 /etc/archivecheck.sh

-rwsr-xr-x 1 root root 1457 May  7  2017 /etc/logrotate.eos

-rwsr-xr-x 1 root root 1750 May  7  2017 /etc/pre_logrotate_cleanup.sh

-rwsr-xr-x 1 root root 59364 Jan 11  2013 /usr/bin/chage

-rws--x--x 1 root root 23276 Apr 20  2017 /usr/bin/chfn

-rws--x--x 1 root root 23228 Apr 20  2017 /usr/bin/chsh

-rwsr-xr-x 1 root root 6592 May  7  2017 /usr/bin/conlogd

-rwsr-sr-x 1 root root 52584 Nov 27  2012 /usr/bin/crontab

-rwsr-xr-x 1 root root 10708 May  7  2017 /usr/bin/cvxreplsh

-rwsr-xr-x 1 root root 76632 Jan 11  2013 /usr/bin/gpasswd

-rwsr-xr-x 1 root root 15028 May  7  2017 /usr/bin/issh

-rwsr-xr-x 1 root root 48224 Apr 20  2017 /usr/bin/mount

-rwsr-xr-x 1 root root 36292 Jan 11  2013 /usr/bin/newgrp

-rwsr-xr-x 1 root root 10692 Apr 21  2017 /usr/bin/oomadj

-rwsr-xr-x 1 root root 27180 Dec  4  2012 /usr/bin/passwd

-rwsr-xr-x 1 root root 36172 Apr 20  2017 /usr/bin/su

---s--x--x 1 root root 129792 Feb 28  2013 /usr/bin/sudo

-rwsr-xr-x 1 root root 23184 Apr 20  2017 /usr/bin/umount

-rwsr-x--- 1 root dbus 335288 Jun 17  2013 /usr/lib/dbus-1/dbus-daemon-launch-helper

-rwsr-xr-x 1 root root 1764212 May  7  2017 /usr/sbin/cliribd

-rwsr-xr-x 1 root root 112640 Apr 20  2017 /usr/sbin/mksquashfs

-rwsr-xr-x 1 root root 109036 Apr  1  2013 /usr/sbin/mount.nfs

-rwsr-xr-x 1 root root 10556 Jul 12  2013 /usr/sbin/pam_timestamp_check

-rwsr-xr-x 1 root root 779456 Apr 20  2017 /usr/sbin/tcpdump

-rwsr-xr-x 1 root root 31516 Jul 12  2013 /usr/sbin/unix_chkpwd

-rwsr-xr-x 1 root root 74804 Apr 20  2017 /usr/sbin/unsquashfs

-rws--x--x 1 root root 36064 Sep 22  2012 /usr/sbin/userhelper

-rwsr-xr-x 1 root root 10768 Mar 15  2013 /usr/sbin/usernetctl

```



可以利用已知可加载root执行权限的程序完成提权操作

利用mksquashfs 加载一个只包含/etc/passwd文件，然后释放到本地，因为具有suid权限，可以覆盖root权限下的/etc/passwd，通过自己设定的用户名口令进入





## 后续提权

[提权方法总结](http://blog.csdn.net/earbao/article/details/65435050)


## 参考链接

参考Linux下的密码Hash——加密方式与破解方法的技术[Linux下的密码Hash——加密方式与破解方法的技术](https://3gstudent.github.io/3gstudent.github.io/Linux%E4%B8%8B%E7%9A%84%E5%AF%86%E7%A0%81Hash-%E5%8A%A0%E5%AF%86%E6%96%B9%E5%BC%8F%E4%B8%8E%E7%A0%B4%E8%A7%A3%E6%96%B9%E6%B3%95%E7%9A%84%E6%8A%80%E6%9C%AF%E6%95%B4%E7%90%86/)

[suid,guid标志位详解](http://www.cnblogs.com/fhefh/archive/2011/09/20/2182155.html)







