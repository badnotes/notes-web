---
layout: post
title: "IDEA 编译的Maven工程出现 SLF4J Failed 异常"
tagline: "IDEA 编译的Maven工程出现 SLF4J Failed 异常"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。IDEA 编译的Maven工程出现 SLF4J Failed 异常。"
category: Tools
tags: Tools IDEA
---
{% include JB/setup %}



#### 0.异常
我在IDEA里面用maven构建的应用，每次启动都打印出以下信息，而程序里面的log有的却没有输出。    

    SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
    SLF4J: Defaulting to no-operation (NOP) logger implementation
    SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
    2014-4-10 13:14:18 org.apache.catalina.startup.HostConfig deployDirectory
    2014-04-10 13:14:21,411  INFO DefaultLifecycleProcessor:334 - Starting beans in phase 2147483647
    2014-04-10 13:14:21,651  INFO ContextLoader:313 - Root WebApplicationContext: initialization completed in 12648 ms
    [2014-04-10 01:14:21,663] Artifact dev: Artifact is deployed successfully
    [2014-04-10 01:14:21,663] Artifact dev: Deploy took 13,081 milliseconds
    SLF4J: Failed to load class "org.slf4j.impl.StaticMDCBinder".
    SLF4J: Defaulting to no-operation MDCAdapter implementation.
    SLF4J: See http://www.slf4j.org/codes.html#no_static_mdc_binder for further details.

#### 1.依赖
看样子是log需要的jar包没有引入或者冲突了，进入刚刚异常信息提示的网址看看，原来是有两个依赖没有引入。

这是我之前的依赖
    
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>

安装网址里面的提示增加以下依赖

    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.7.6</version>
    </dependency>

问题仍然没解决，用IDEA的依赖图看看依赖是否有冲突，果真其中的两个依赖已经有了，在这里加入exclusions排除这两个依赖。

    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.7.6</version>
        <exclusions>
            <exclusion>
                <artifactId>slf4j-api</artifactId>
                <groupId>org.slf4j</groupId>
            </exclusion>
            <exclusion>
                <artifactId>log4j</artifactId>
                <groupId>log4j</groupId>
            </exclusion>
        </exclusions>
    </dependency>

问题还存在，再查看，其他依赖里面的版本不是1.7.6,而是1.6.6,好吧，把这边改为1.6.6,也不用排除了。

    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.6.6</version>
    </dependency>

#### 3.原因
折腾了老半天，网上搜索了一些，始终没有解决这个问题，还是在网上在仔细的看看。
终于找到有说是ecplise的m2e插件存在这个Bug,出现的问题都是一样的。可我用的是IDEA呀。

    ecplise m2e bug

#### 4.验证

    a.用系统安装的maven打包并部署运行，正常
    b.用IDEA引用的系统maven部署运行也ok.

#### 5.后记

好吧，并不止eclipse的m2e插件有这个问题，IDEA内部自带的maven打包编译也有这个bug.在IDEA里引入系统的maven也没这问题。
这个问题导致log不能正常输出.