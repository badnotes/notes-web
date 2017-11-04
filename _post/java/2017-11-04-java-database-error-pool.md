---
layout: post
title: "问题处理 - 对数据库链接耗尽的一次分析"
tagline: 问题处理 - 对数据库链接耗尽的一次分析"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。问题处理 - 对数据库链接耗尽的一次分析"
category: Java
tags: [Java]
---
{% include JB/setup %}


问题处理 - 对数据库链接耗尽的一次分析

-------------------------------

#### 1. 问题

从应用日志中可以看到大量的数据库超时异常,该程序已经正常运行了很长一段时间了,最近做了一次小小的升级,并且也运行了几天。

```
java.sql.SQLException: Timed out waiting for a free available connection.
        at com.jolbox.bonecp.DefaultConnectionStrategy.getConnectionInternal(DefaultConnectionStrategy.java:88)
        at com.jolbox.bonecp.AbstractConnectionStrategy.getConnection(AbstractConnectionStrategy.java:90)
        at com.jolbox.bonecp.BoneCP.getConnection(BoneCP.java:553)
        at com.jolbox.bonecp.BoneCPDataSource.getConnection(BoneCPDataSource.java:131)
        at org.apache.commons.dbutils.AbstractQueryRunner.prepareConnection(AbstractQueryRunner.java:204)
        at org.apache.commons.dbutils.QueryRunner.query(QueryRunner.java:287)
```

#### 2. 寻找问题原因

  * 程序日志
  * 数据库情况
  * 访问日志
  * 线程堆栈
  
前面我们从程序日志里面能看到具体的问题。但是必须要找出产生这个问题的原因并解决掉。
面对这种情况,首先想到是不是后面数据库挂掉了.赶紧检查,一切正常。
然后查看访问日志，是否有大量的访问请求，超过了程序或者数据库的处理能力呢。查看后仍然并没有过多的访问请求。那就多半是程序上的bug了,需要查看堆栈信息分析了。

#### 2. 线程栈


`jstack pid > js.log` 

通过以上命令导出堆栈信息,发现有几个超时等待的线程。

```
"pool-4-thread-4890" #5190 prio=5 os_prio=0 tid=0x00007f4b20002800 nid=0x822 waiting on condition [0x00007f4a79ce0000]
    java.lang.Thread.State: TIMED_WAITING (parking)
         at sun.misc.Unsafe.park(Native Method)
         - parking to wait for  <0x00000000c0b19410> (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
         at java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:215)
         at java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.awaitNanos(AbstractQueuedSynchronizer.java:2078)
         at java.util.concurrent.LinkedBlockingQueue.poll(LinkedBlockingQueue.java:467)
         at com.jolbox.bonecp.DefaultConnectionStrategy.getConnectionInternal(DefaultConnectionStrategy.java:82)
         at com.jolbox.bonecp.AbstractConnectionStrategy.getConnection(AbstractConnectionStrategy.java:90)
         at com.jolbox.bonecp.BoneCP.getConnection(BoneCP.java:553)
         at com.jolbox.bonecp.BoneCPDataSource.getConnection(BoneCPDataSource.java:131)
         at org.apache.commons.dbutils.AbstractQueryRunner.prepareConnection(AbstractQueryRunner.java:204)
         at org.apache.commons.dbutils.QueryRunner.query(QueryRunner.java:287)
```


#### 3. 原因

可以看到堆栈里有3个如上的等待线程。正好对应了数据库连接池BoneCP配置的3个分区

`jdbc.partitionCount=3`

也就是说有3个LinkedBlockingQueue用来存储连接, BoneCP通过分区(Partitioning)来提升性能,也就是说从连接池中获取连接时,是从3个Queue中的任意一个获取的。避免了大量请求时从一个地方获取连接，其中的加锁等待造成的低效。不过现在HikariCP使用线程复用技术将连接池性能又提升了一个档次,值得推荐学习使用的。

也就是说我们正常的从数据库连接池中获取连接却没有成功的获取到。那我们先检查一下连接池的配置情况,然而并没有找到哪里配置不对的。那很有可能是数据库连接的释放上出了问题。

程序中找到了最近新增的数据库事务功能,发现在异常回滚的时候忘了关闭连接了。那这多半就是问题的根源了。

```
public static void rollbackTransaction() throws SQLException{
    Connection conn = threadLocal.get();
    if (conn != null){
        conn.rollback();
        conn.setAutoCommit(true);
    }
    threadLocal.remove();
}
```

在事务提交或者回退的结束后关闭连接

```
DbUtils.close(conn);
```
