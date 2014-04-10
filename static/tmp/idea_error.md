IDEA 编译的Maven工程出现 SLF4J Failed 异常

0.异常
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

1.依赖
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>

2.修改
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.7.6</version>
</dependency>

3.修改
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

4.修改
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.6.6</version>
</dependency>

5.原因

ecplise m2e bug

6.验证

用系统安装的maven打包并部署运行，
用IDEA引用的系统maven部署运行都ok

7.后记

并不是eclipse的m2e插件有这个问题，IDEA内部的maven打包也有这个bug.
这个问题导致log不能正常输出.