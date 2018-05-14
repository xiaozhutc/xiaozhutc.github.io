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

#### 2. dump Java进程的堆栈
```bash
jmap   -dump:live,format=b,file=test.hprof ${pid}
```
* live : with unreachable object

## 开发环境
* 安装或者升级gcc 4.0，以支持C++11标准
[编译升级](https://www.cnblogs.com/lizhenghn/p/3550996.html)
