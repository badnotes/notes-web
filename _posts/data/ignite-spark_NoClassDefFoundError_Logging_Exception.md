ignite-spark NoClassDefFoundError Logging Exception
--------------------------

### 1. Spark和Ignite 结合
正在使用Spark做数据分析,又想引入Ignite的数据存储功能,这就需要将他们结合起来.
虽然Spark已经自带了cache,但功能是没有Ignite强大的;
而Ignite也有分布式计算能力,正好也没有Spark强大,为了结合各自所长就需要结合起来使用了.
只需要使用Ignite提供的ignite-spark模块就能实现.
需要注意的是Spark和Ignite的版本都需要与各自的服务端版本相同,否则是无法链接的.

* 使用ignite-spark提取数据

Maven依赖:
```java
<dependency>
    <groupId>org.apache.ignite</groupId>
    <artifactId>ignite-spark</artifactId>
    <version>1.7.0</version>
</dependency>
<dependency>
    <groupId>org.apache.spark</groupId>
    <artifactId>spark-sql_2.11</artifactId>
    <version>2.0.1</version>
</dependency>
<dependency>
    <groupId>org.apache.spark</groupId>
    <artifactId>spark-core_2.11</artifactId>
    <version>2.0.1</version>
</dependency>
```

Java实现:
```java
SparkConf conf = new SparkConf()
        .setAppName("Simple Application")
        .setMaster("local[*]");
JavaSparkContext sc = new JavaSparkContext(conf);
Ignite ignite = CsfIgnite.getIgnite();
JavaIgniteContext<String,  Double[]> ic = new JavaIgniteContext<>(sc, ignite::configuration);
JavaIgniteRDD<String,  Double[]> values = ic.fromCache("cacheName");
```

### 2. 版本兼容性上出现了错误
看起来很容易,但是跑起来却根本跑不起来呢.
jar包的版本必须和服务端版本相同,目前服务端都是使用的最新版本,
所有这里Maven依赖也必须用这个版本:

* ignite-spark:1.7.0
* spark-core_2.11:2.0.1

```java
Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/spark/Logging
	at java.lang.ClassLoader.defineClass1(Native Method)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:760)
	at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
	at java.net.URLClassLoader.defineClass(URLClassLoader.java:467)
	at java.net.URLClassLoader.access$100(URLClassLoader.java:73)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:368)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:362)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:361)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:331)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
	at org.apache.ignite.spark.JavaIgniteContext.<init>(JavaIgniteContext.scala:42)
	at org.apache.ignite.spark.JavaIgniteContext.<init>(JavaIgniteContext.scala:45)
	at com.*.*.*.TestData.cache(TestData.java:45)
	at com.*.*.*.TestData.testReadDataFromIgnite(TestData.java:34)
	at com.*.*.*.TestData.main(TestData.java:20)
Caused by: java.lang.ClassNotFoundException: org.apache.spark.Logging
	at java.net.URLClassLoader.findClass(URLClassLoader.java:381)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:331)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
	... 17 more
```

但是ignite-spark:1.7.0本身依赖的spark-core却是1.5.2

官方Issue: https://issues.apache.org/jira/browse/IGNITE-3710

试试通过Maven强制去掉1.5.2的版本,然后用最新的呢,仍然不行.

### 3. 原因
好吧,那直接去看看源代码是什么原因吧.

原来spark-core:1.5.2里面 `package org.apache.spark` 包下面有 `trait Logging`
所以`IgniteContext.scala`里面可以直接 `with Logging` 引入这个特质.

而spark-core:2.0.1却将`Logging`移到 `package org.apache.spark.internal`
并且声明为Spark专用了(`private[spark] trait Logging`).

ok, 去git看看了下最新的代码, 正在开发的版本1.8,仍然有这个问题.

### 4. 解决方式
网上找了找,有几个地方提到了这个问题,但都没有给出解决方式.
这种情况只有自己修改源代码了,去github上拉取了1.7的版本源码修改,两种修改方式.

* a. 直接注释掉Logging相关代码(简单粗暴,不过不太友好)
* b. 使用 ignite-log4j 模块里面的Log4JLogger,
  这个模块已经引入了直接使用就可以,
  不过要用import模式而不是with,并且修改相应的日志输出的地方

  ```scala
  import org.apache.ignite.logger.log4j.Log4JLogger

  private val log = new Log4JLogger()
```
