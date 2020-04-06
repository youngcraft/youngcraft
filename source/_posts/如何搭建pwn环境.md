---
title: 如何搭建Pwn利用环境
date: 2020-02-15 09:50:08
tags: Exploit
categories: Pwn
---

> 前人栽树，后人乘凉


# socat 使用方法

## socat 简介
本身socat是加强版的nc工具，可以借助nc获取

在ubuntu 16.04 测试：
sudo apt-get install socat 


## 运行命令：

socat TCP-LISTEN:4444,REUSEADDR,FORK EXEC:./xxxxxxxx

xxxx表示文件路径

# xinetd


sudo apt-get install xinetd

设置启动模板：
```
    /etc/services 下先添加自己的服务端口信息 
    /etc/xinetd.d/ 下添加自己的服务
    service pwn_test
    {
    disable = no //打开
    port = 10002 
    socket_type = stream
    server = 【filepath】
    wait = no 
    user = pwn_user
    }
```
 

