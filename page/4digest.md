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
    


## Comments

{% include comments.html %}
