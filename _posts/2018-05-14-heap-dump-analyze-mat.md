---
layout: post
title:  "内存分析工具MAT"
categories: Tools
tags:  Memory 
author: jun.tang
---

* content
{:toc}

## MAT是什么

## 系统设置
* 设置系统时区
```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```




## 常见问题
* 使用`jmap`dump java堆栈后有1G多，在MAT中打开可能只有几十M，没法排查问题
```bash
jmap -dump:format=b,file=test.hprof 28633
```
    原因是:
> MAT does not display the unreachable objects by default.

    解决办法: 
> You can enable the option by going to Preferences -> Memory Analyzer -> Keep Unreachable Objects. Load the heap again once the option is enabled.
