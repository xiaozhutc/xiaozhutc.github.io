---
layout: post
title:  "在Window下安装Jekyll"
categories: Utils
tags:  Jekyll 
author: jun.tang
---

* content
{:toc}

## 1.什么是Jekyll
`Transform your plain text into static websites and blogs`

[官网](https://jekyllrb.com/) 第一句话告诉我们, jekyll是将我们的文本文件转换为静态网站和博客。通常我们结合github的pages和Jekyll来搭建博客。


## 2. 下载安装Ruby+Devkit

下载地址：https://rubyinstaller.org/downloads/

最好选择Ruby+Devkit的集合安装包，亲测如果分别下载安装很可能碰到版本不兼容的问题。
安装时请将`配置环境变量`打开




## 3. 安装Jekyll
```java
gem install jekyll
```

## 4. 启动本地服务
进入Jekyll工程所在目录后执行：

```java
jekyll s
```
将在本地启动4001端口，并将当前工程转换为静态网站

