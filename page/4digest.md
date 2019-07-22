---
layout: page
title: Digest
permalink: /digest/
icon: bookmark
type: page
---

* content
{:toc}

## 方法论
* [反馈式学习](https://okayjam.com/index.php/2017/04/12/%E5%8F%8D%E9%A6%88%E5%BC%8F%E5%AD%A6%E4%B9%A0/)
* [如何快速学习一门新技术](https://codingstyle.cn/topics/3)

## 开源项目
* [国内顶尖团队的开源项目](https://github.com/niezhiyang/open_source_team)
* []

## Mysql
* [late row lookup](https://explainextended.com/2009/10/23/mysql-order-by-limit-performance-late-row-lookups/) [待研究]

    1.mysql优化器强制使用`order by`的索引B: where A order by B => 增加联合索引index(A_B) 或者强制使用索引`force index(A)`
    
    2.对于`order by A limit 10000, 10`, mysql可能会在未进行filter前就进行row-lookup, 导致性能暴差 => 使用子查询解决
    
## Zookeeper 
* [ACL auth](https://blog.csdn.net/wuhenzhangxing/article/details/52936040)
* [SASL: Client-Server mutual authentication](https://cwiki.apache.org/confluence/display/ZOOKEEPER/Client-Server+mutual+authentication)
* [ACL auth](https://stackoverflow.com/questions/40427700/using-acl-with-curator)

## Maven
* [多模块的版本如何管理？]
* [解决版本冲突之dependency:tree](http://ian.wang/106.htm)

## UML
* [PlantUML 时序图](http://plantuml.com/sequence-diagram)

## Mockito
* [Verify param value](https://stackoverflow.com/questions/1142837/verify-object-attribute-value-with-mockito)
```java
ArgumentCaptor<Person> argument = ArgumentCaptor.forClass(Person.class);
verify(mock).doSomething(argument.capture());
assertEquals("John", argument.getValue().getName());
```

## Linux
* [Java 程序员眼中的Linux](https://github.com/judasn/Linux-Tutorial)
* [select, poll, epoll区别](http://www.cnblogs.com/Anker/p/3265058.html)
* [Linux ate my ram](https://www.linuxatemyram.com/)
* awk, sed学习

## Java
* [Differences between Exception and Error](https://stackoverflow.com/questions/912334/differences-between-exception-and-error)
* [Java Exceptions by Example](https://www.akadia.com/services/java_exceptions.html)
* [使用validator-api来验证spring-boot的参数](https://www.jianshu.com/p/2c2da2adef81)
* [PO/POJO/VO/BO/DAO/DTO](https://blog.csdn.net/gaoyunpeng/article/details/2093211)
* [GC 性能优化](https://blog.csdn.net/column/details/14851.html)
* [Java性能优化](https://mp.weixin.qq.com/s?__biz=MzI3MzEzMDI1OQ==&mid=2651815337&idx=1&sn=8e846e11e908735a5175c9eacb642329)
* [构造函数不要抛出异常](http://www.cnblogs.com/DreamDrive/p/5621276.html)
* [Java8 Lambda表达式示例](http://www.importnew.com/16436.html)
* [Spring bean的生命周期]
* 坑：jar cMf xxxx.jar .
    springboot加载bean的时候是根据jar内部的文件顺序来进行的
    windows，linux jar cMf打包的文件顺序不太一样。
# 学习JPA

## Concurrent
* [并发之痛 Thread，Goroutine，Actor](http://jolestar.com/parallel-programming-model-thread-goroutine-actor/) [午夜咖啡](http://jolestar.com/)
* [Java 中的锁 -- 偏向锁、轻量级锁、自旋锁、重量级锁](https://blog.csdn.net/zqz_zqz/article/details/70233767)
* [Java并发之AQS详解](https://www.cnblogs.com/daydaynobug/p/6752837.html)

## 分布式
* [理解一致性hash](https://blog.csdn.net/cywosp/article/details/23397179/)
* [那些年，我们追过的RPC](https://zhuanlan.zhihu.com/p/29028054)

## 源码分析
* [dubbo源码从入门到放弃-SPI](https://www.cnblogs.com/kindevil-zx/p/5603643.html)
* [dubbo扩展机制的实现](https://my.oschina.net/pingpangkuangmo/blog/508963)

## Spring
* [spring-events](https://www.baeldung.com/spring-events)
* [spring-async](https://www.baeldung.com/spring-async)
* [spring-SpEL](https://www.baeldung.com/spring-expression-language)
* 学习mongo，redis的spring封装.SetOperations

## 前端
* 跨域什么问题？

## 数据库
* 如何创建数据库索引

## 经济
* M1反映着经济中的现实购买力；M2不仅反映现实的购买力，还反映潜在的购买力。若M1增速较快，则消费和终端市场活跃；若M2增速较快，则投资和中间市场活跃
* 春节前最低，都忙着回家，而且年终奖还没发，加上天气冷，万物萧瑟，有些房东做生意要还银行钱。十一月和十二月是最好的买房时间，即使暴涨这两个月也会停歇缓缓的。元旦后，中央经济政策会改调，加上银行信贷会放波水来个开门红，一月有时人家赶着买了安心过年，反而不是最冷的

## 经典语录
人生最幸福的事：父母健在，知己两三，盗不走的爱人。其他都是假象，别太计较

##

## Comments

{% include comments.html %}
