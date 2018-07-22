---
layout: page
title: Digest
permalink: /digest/
icon: bookmark
type: page
---

* content
{:toc}

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

## UML
* [PlantUML 时序图](http://plantuml.com/sequence-diagram)

## Mockito
* [Verify param value](https://stackoverflow.com/questions/1142837/verify-object-attribute-value-with-mockito)
```java
ArgumentCaptor<Person> argument = ArgumentCaptor.forClass(Person.class);
verify(mock).doSomething(argument.capture());
assertEquals("John", argument.getValue().getName());
```

## Comments

{% include comments.html %}
