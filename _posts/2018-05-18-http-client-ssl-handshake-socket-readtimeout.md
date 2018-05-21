---
layout: post
title:  "[问题排查] http client ssl handshake socket read timeout"
categories: Debug
tags:  Debug Socket Timeout HttpClient
author: jun.tang
---

* content
{:toc}

## 1. 问题现象
1. Receiver模块接受上游文件，然后启动一个Task丢到队列由有线程池执行推送下游的任务。
2. 现象是：下游没有收到任何文件，且在上午10点之后该任务没有任何日志


## 2. 排查过程
* 初步怀疑线程池的队列满（消费不及）

    通过业务模块提供的`inspect`工具查看，队列并未满，而且在不断增加。说明线程池中的线程处理过慢





* 查看线程池的线程堆栈

![thread-stack-timeout](http://wx1.sinaimg.cn/large/621c9e96gy1frf4rnhxr9j20rx0ctgnj.jpg)

   可以看到当前线程池中的四个线程都处于读取socket数据的状态，而同时看到，存在四个和外部的连接

![netstat-client-connection](http://wx4.sinaimg.cn/large/621c9e96gy1frf4rmoulkj20lu033q3h.jpg)

* 怀疑四个线程都阻塞在socket连接上

    确认使用的HttpClient正确地设置了socket连接、数据的超时时间
```java
private static RequestConfig defaultRequestConfig = RequestConfig.custom()
			  .setSocketTimeout(10000)
			  .setConnectTimeout(10000)
			  .setConnectionRequestTimeout(10000)
			  .build();
```

    发现四个线程都处于`socketRead`的状态，同时堆栈上看到线程正在`SSLConnection`, `startHandshake`，推测线程正在进行ssl连接的密钥交换。
    问题在于为什么设置了`SocketTimeout`，线程却阻塞了？

* HttpClient的bug

    在万能的Google发现HttpClient存在这一的ISSUE：
    > https calls ignore http.socket.timeout during SSL Handshake. This can result in a https call hanging forever waiting for socket read.
      In both SSLSocketFactory and SSLConnectionSocketFactory, sslsock.startHandshake(); is called before socket timeout is set on the socket. This means timeout is not respected during the SSL handshake
      
    [https calls ignore http.socket.timeout during SSL Handshake](https://issues.apache.org/jira/browse/HTTPCLIENT-1478?focusedCommentId=15152242&page=com.atlassian.jira.plugin.system.issuetabpanels%3Acomment-tabpanel#comment-15152242)
    
    也就是说HttpClient在进行ssl握手时，使用了`socketConfig == SocketConfig.DEFAULT` 而未使用应用层的超时配置。
    该bug在HttpClient4.3.6中修复，而我们使用了4.3.5，o(╯□╰)o
    
    [修复代码如下:](http://svn.apache.org/viewvc/httpcomponents/httpclient/branches/4.3.x/httpclient/src/main/java/org/apache/http/conn/ssl/SSLConnectionSocketFactory.java?r1=1560975&r2=1626784)
    
    ```diff
    --- httpcomponents/httpclient/branches/4.3.x/httpclient/src/main/java/org/apache/http/conn/ssl/SSLConnectionSocketFactory.java	2014/01/24 12:40:46	1560975
    +++ httpcomponents/httpclient/branches/4.3.x/httpclient/src/main/java/org/apache/http/conn/ssl/SSLConnectionSocketFactory.java	2014/09/22 14:13:34	1626784
    @@ -236,6 +236,9 @@
                 sock.bind(localAddress);
             }
             try {
    +            if (connectTimeout > 0 && sock.getSoTimeout() == 0) {
    +                sock.setSoTimeout(connectTimeout);
    +            }
                 sock.connect(remoteAddress, connectTimeout);
             } catch (final IOException ex) {
                 try {
    ```
    
* 巧合

    由于在ssl握手时超时，也就是说此时socket已经成功建立。在接受对方密钥信息时，对方发生了crash/重启等事件，导致本机未收到FIN报文认为连接正常。
    而同时socket读超时未设置（默认为0，永远等待） ===> 线程hang住

## 3. 总结
    * 平时多关注Spring/SpringBoot对相关模块的封装，大牛在其中避免了很多坑
    * 尽可能使用最新的稳定版本，可以避免不必要的bug
