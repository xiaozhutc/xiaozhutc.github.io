---
layout: post
title:  "Linux常用命令"
categories: Experience
tags:  Linux Tips
author: jun.tang
---

* content
{:toc}

本文主要记录linux常用的命令

## 系统设置
### 1. 设置系统时区
```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```
如果使用mv，可能会修改到时区文件

### 2. dump Java进程的内存
```bash
jmap   -dump:live,format=b,file=test.hprof ${pid}
```
* live : without unreachable object

### 3. 查看进程的所有线程
    1. ps xH {pid}
    2. 查看进程信息（包括线程个数）: cat /proc/${pid}/status
    3. pstree -p ${pid}
    4. 查看系统进程上限：cat /proc/sys/kernel/pid_max
    5. 查询系统线程上限：cat /proc/sys/kernel/threads-max

### 4.

### 4. gdb 调试工具
* 调试Java程序中出现的core
```bash
1.	gdb app.name
2.	set args xxxx //设置参数
3.	b  xxx.cpp:100 //设置断点
4.  r //运行
5.	n //下一步
6   c // continue
5.	bt // 打印当前堆栈
```
* 查看导致core的线程的堆栈：`bt`
* 进入某个占帧：`f ${frame_id}`


## 开发环境
* 安装或者升级gcc 4.0，以支持C++11标准
[编译升级](https://www.cnblogs.com/lizhenghn/p/3550996.html)
