---
layout: post
title:  "Java程序远程调试的配置"
categories: Experience
tags:  Java Debug
author: jun.tang
---

* content
{:toc}

## 1. 为什么需要远程调试
* 通常和外部团队进行联调时，应用程序需要部署到开发环境
* 提测后在测试环境进行测试

这两种情况下排查问题非常困难，借助远程调试可以很快定位问题


## 2. 应用程序服务端配置
在应用程序的JVM参数中增加`-agentlib`配置
```bash
java -agentlib:jdwp=transport=dt_socket,server=y,address=8888,suspend=n ${APPNAME}
```




配置含义见help信息 `java -agentlib:jdwp=help`
``` bash
               Java Debugger JDWP Agent Library
               --------------------------------
jdwp usage: java -agentlib:jdwp=[help]|[<option>=<value>, ...]

Option Name and Value            Description                       Default
---------------------            -----------                       -------
suspend=y|n                      wait on startup?                  y
transport=<name>                 transport spec                    none
address=<listen/attach address>  transport spec                    ""
server=y|n                       listen for debugger?              n
launch=<command line>            run debugger on event             none
onthrow=<exception name>         debug on throw                    none
onuncaught=y|n                   debug on any uncaught?            n
timeout=<timeout value>          for listen/attach in milliseconds n
mutf8=y|n                        output modified utf-8             n
quiet=y|n                        control over terminal messages    n

Examples
--------
  - Using sockets connect to a debugger at a specific address:
    java -agentlib:jdwp=transport=dt_socket,address=localhost:8000 ...
  - Using sockets listen for a debugger to attach:
    java -agentlib:jdwp=transport=dt_socket,server=y,suspend=y ...

Warnings
--------
  - The older -Xrunjdwp interface can still be used, but will be removed in
    a future release, for example:
        java -Xdebug -Xrunjdwp:[help]|[<option>=<value>, ...]
```
* 特别的, server用于区分是server还是debug的IDE; 
* address用于配置jdwp的监听端口

> [`JDWP`(Java Debug Wire Protocol)](https://docs.oracle.com/javase/8/docs/technotes/guides/troubleshoot/introclientissues005.html)是Java应用程序debug的通信协议

## 3. IDE配置（以IDEA为例）
### 3.1 新建`Run -> Edit Configuration -> Remote Debug`配置

1. Transport配置为`Socket`
2. Debugger Mode为Attach
3. Host配置为${APPLICATION_HOST}
4. Port为JDWP端口，如8888

### 3.2 启动Debug，设置断点进行调试

## 备注（目前一般用不到）
对于JDK 1.4 服务端JVM配置为
```bash
-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8888
```
对于JDK 1.3 服务端JVM配置为
```bash
-Xnoagent -Djava.compiler=NONE -Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8888
```
