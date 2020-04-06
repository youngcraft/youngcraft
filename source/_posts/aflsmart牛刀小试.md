---
title: aflsmart牛刀小试
date: 2020-02-16 22:09:29
tags:
author:     gumeng
categories:   
    - tools

---


# AFL-SMART 安装踩坑指南

AFL-SMART是一款结合Peach和AFL模式的模糊测试工具，科普下Peach和AFL两款工具

> AFL是由美国谷歌公司安全研究人员开发的一款基于代码覆盖率的模糊测试工具，其核心部件是AFL-gcc 或 AFL-g++在编译器层面对代码进行插装，通过监控程序执行流程来实现对代码覆盖率的评估。AFL采用基于基于源码的插装模式进行模糊测试，在变异模块采用基于bit翻转、实现对crash结果的细节与动态跟踪，总而言之是一款优秀的灰盒测试工具

>Peach是由美国Peach.tech公司开发的基于协议模糊测试工具，主要采用XML文件描述协议和文件格式




```
(1)
root@vultr:~/aflsmart#  sudo docker run -itd c26804edf98e /bin/bash
7d2ecfc25dab4d2b2c8915305c99ad5f8b0e9f848ccacf6ffb2ce031abe7c41c

docker exec -it  c26804edf98e  bash 
docker exec –it  bb244f620484 bash

(2)
root@vultr:~# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
f81799889649        bb244f620484        "/bin/bash"         5 seconds ago       Up 3 seconds                            pensive_spence




(3)
sudo docker attach f81799889649


```