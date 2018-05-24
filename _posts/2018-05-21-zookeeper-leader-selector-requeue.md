---
layout: post
title:  "[问题排查] Zookeeper选主重连配置错误"
categories: Debug
tags:  Zookeeper LeaderSelector
author: jun.tang
---

* content
{:toc}

## 1. 问题现象
1. 背景：Receiver有若干定时任务，希望在集群情况下只有一台机器运行。
2. 现象：定时任务没有启动


## 2. 排查过程
* 排查日志
    > not the leader skip check File
    
    发现定时任务成功启动，但是发现本机非master导致后续的逻辑被跳过
    ```java
    if(!masterLatcher.isLeader()) {
      logger.info("not the leader skip check File ");
      return;
    }
    ```





   
* 单步调试
    
    单步发现，`isLeader`标志位初始为0，在获得LeaderShip之后，标志位又重新被置为了0。导致业务逻辑被跳过
    通过日志发现，服务和Zookeeper之间的连接出现过闪断重连。怀疑是否`LeaderSelector`对断连接等异常有处理。
    ```java
    public abstract class LeaderSelectorListenerAdapter implements LeaderSelectorListener
    {
        @Override
        public void stateChanged(CuratorFramework client, ConnectionState newState)
        {
            if ( (newState == ConnectionState.SUSPENDED) || (newState == ConnectionState.LOST) )
            {
                throw new CancelLeadershipException();
            }
        }
    }
    ```
    可以看到，使用的Listener对连接异常进行了捕获，并且抛出了取消LeaderShip的异常。
    
    查看源码可以看到，在Task收到InterruptException之后，会根据**`autoRequeue`**这个配置来决定是否重新提交选主的任务。
    ```java
        private synchronized boolean internalRequeue()
        {
            if ( !isQueued && (state.get() == State.STARTED) )
            {
                isQueued = true;
                Future<Void> task = executorService.submit(new Callable<Void>()
                {
                    @Override
                    public Void call() throws Exception
                    {
                        try
                        {
                            doWorkLoop();
                        }
                        finally
                        {
                            clearIsQueued();
                            if ( autoRequeue.get() )
                            {
                                internalRequeue();
                            }
                        }
                        return null;
                    }
                });
                ourTask.set(task);
                return true;
            }
            return false;
        }
    ```
 * 问题症结
 
    问题在于MasterLatcher的实现中，并没有设置`LeaderSelector.autoQueue`，导致Zookeeper的连接发生闪断后，当前机器永远也不会参与选主。
    
    
    
* LeaderSelector实现

    [LeaderSelector的使用和分析](https://my.oschina.net/roccn/blog/909874)

## 3. 总结
    * 平时多关注Spring/SpringBoot对相关模块的封装，大牛在其中避免了很多坑
    * 尽可能使用最新的稳定版本，可以避免不必要的bug
