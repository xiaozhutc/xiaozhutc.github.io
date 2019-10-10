---
layout: post
title:  "Maven常用"
categories: Experience
tags:  Linux Tips
author: jun.tang
---

* content
{:toc}

本文主要记录Maven常用的技巧

## 插件
### 1. jar-with-dependencies 连同依赖打包jar
```java
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-assembly-plugin</artifactId>
	<configuration>
		<descriptorRefs>
			<descriptorRef>jar-with-dependencies</descriptorRef>
		</descriptorRefs>
	</configuration>
	<executions>
		<execution>
			<id>assemble-all</id>
			<phase>package</phase>
			<goals>
				<goal>single</goal>
			</goals>
		</execution>
	</executions>
</plugin>
```

## Unit Test
```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:xxx.xml")


@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(loader = AnnotationConfigContextLoader.class, classes = {ListenerConfiguration.class})

@ContextConfiguration("classpath:application-context.xml")
public class IndexerFuntionalTest extends AbstractJUnit4SpringContextTests

```

## Redis
* 通过回调保证原子性：
```java
redisOperations.execute(new SessionCallback() {
            @Override
            public Object execute(RedisOperations operations)
                    throws DataAccessException {
                operations.multi();
                Long value = (Long) operations.opsForValue().get(INSTANCE_JOB_KEY);
                if (null == value || current - value > MAX_SAVE_INTERVAL) {
                    redisOperations.opsForValue().set(INSTANCE_JOB_KEY, current);
                    flag.set(true);
                }
                operations.exec();
                return null;
            }
        })
```

## JUC
* StampedLock : http://blog.sina.com.cn/s/blog_6f5e71b30102xfsb.html
    为了解决ReentrantReadWriteLock中写饥饿的问题：由于读多写少，导致写线程长时间得不到时间片。
    思路：在读加锁前，先用乐观锁进行尝试。减少读锁
```java
        long r = lock.tryOptimisticRead(); //精髓步骤
        Object result = dosomething();
        if (!lock.validate(r)) {
            r = lock.readLock(); // 读锁
            try {
                result = dosomething();
            } finally {
                lock.unlockRead(r);
            }
        }
```
