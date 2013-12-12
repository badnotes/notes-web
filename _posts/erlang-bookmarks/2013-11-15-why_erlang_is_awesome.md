---
layout: post
title: "为什么erlang这么好"
tagline: "为什么erlang这么好"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。为什么erlang这么好."
category: Erlang
tags: [Erlang]
---
{% include JB/setup %}


####Erlang欢迎你(译.未完)


#####为什么要用Erlang?
牛逼之处，实战,省时省钱，容易学习 <br />

#####好在哪里？
轻量级并发，透明分布，代码热部署，OTP及其他 <br>

#####开始

* 现在就进入[tryerlang.org](http://www.tryerlang.org)试试Erlang
* 找一本好书试试 [Erlang Programmin](http://www.amazon.com/ERLANG-Programming-Francesco-Cesarini/dp/0596518188) （中文版：[Erlang 编程指南](http://www.amazon.cn/gp/product/B004RDKTFM/ref=olp_product_details?ie=UTF8&me=&seller=)）
* 官方网站:[erlang.org](http://www.erlang.org)
* 更多信息:[erlangotp.com](http://www.erlangotp.com)

#####[牛逼之处]

Erlang被开发在爱立信公司并且设计从来完全的可伸缩的，容错的，分布式的，永不停止的，软实时应用。
所有的事情在语言中，运行时以及库都旨在使Erlang成为开发不同软件的更好的平台。

如果你想在你的应用中使用Erlang:

* 处理非常大的并发动作
* 很容易分布在网络中不同的节点
* 对于软硬件方面的错误兼具容错性
* 大规模的网络机器
* 升级和从新配置都不必停机重启
* 在精确的时间点对用户作出响应
* 连续多年运行不停机

Erlang是面向并发的，自然也善于充分利用现在的多核系统

轻量级的并发，透明的分布，代码热部署，以及OTP是Erlang的主要特征，这使得使用Erlang是一种乐趣。

#####[实战证明]

Erlang已经成功的在产品环境下运行超过20年(据报道，正常运行在9-nines--仅仅在某年宕机过31ms)，这足以证明她可以很好的运行在大型的工业开发及小型的敏捷创业团队。

爱立信公司将Erlang广泛用于各不同大小的项目，不管是商业方面的还是内部使用的。AXD301 ATM作为爱立信的龙头产品之一，有超过110W行Erlang代码，这可能是现今最大的Erlang项目了。

电信业的Erlang使用者：Motorola, Nokia, T-Mobile, BT.<br />
大型软件公司及创业公司：Amazon, Yahoo!, Facebook, Last.fm, Klarna, Tail-F, Github, Heroku, Engine Yard, MochiMedia.<br />
开源项目：Flussonic, ejabberd, CouchDb, Riak, Disco, RabbitMQ, Dynomite.<br />

#####[节约时间和金钱]

Erlang能使你用更小的团队更少的预算去更快的交互软件，以及减少生命周期维护成本和总体拥有成本。
这可能是由多方面原因促成的：

* OPT库提供了一整套易用的组件来开发强健的分布式应用，已经用于大量的项目中，并且有超过10年的测试和调试。
* 许多高质量的开源库可以直接用于很多开发任务，比如XML解析及与数据库交互，甚至是对象-关系型数据库PostgreSQL。和之前开发的Java、C、Python或者Ruby通信也很简单。
* Erlang代码倾向于简洁、易读。这是由于简单的语言和强大的抽象机制。
* Erlang适用于大型或小型团队，以及用自上而下法和自下而上法去开发软件都是可行的。
* Erlang是容易学习的，一个用编程经验的程序员在学习Erlang一两天就可以写有用的代码了。
* 有用的高质量工具有文档生成器，测试框架，调速器，图形诊断工具，和IDE（集成开发环境）。

#####[易于学习]

Erlang有一个使的它很容易就学会的简要理念。有经验的程序员可以在学习一两天后就写出有用的代码，它并没有复杂的概念需要理解，也没有晦涩的理论需要精通。他的语法有一点点不同于Ruby、Python、或者Java，但是你并不需要花多少时间就可以适应的。

事实上，Erlang开发团队最初的设计目标之一就是使这门语言很容易学习。Erlang是非常实用的，它是由实际开发中的程序员为解决真实的大规模软件工程问题的程序员所设计的。

#####[轻量级的并发]

Erlang的进程是非常轻量的，每个进程大约只有500btyes的开销。这意味着甚至在比较旧的机器上都可以创建数百万的进程。

由于Erlang的进程是完全依靠操作系统进程的（不是由系统调度程序所管理的），这意味着不管你的程序是运行在Linux,FreeBSD,Windows,还是其他的运行了Erlang环境的系统上，都将有完全相同的表现效果。

#####[代码热部署]

我们常常不想为了升级程序而停止一个事实的控制系统。甚至在某些控制系统中，我们根本就不能停机来完成升级，在设计这样的系统时必须考虑到动态代码升级，比如像给NASA开发的X2000卫星控制系统。

当你用Erlang开发应用时，如果你用OTP开发的话，你可以毫无花费的完成动态代码升级。它的机制是非常的简单并且很容易理解的。

这将会节约许许多多的开发时间。

Erlang的一般开发流程：

1. 开始应用
2. 编写代码
3. 再编译(一键完成)
4. 就是这样!并不需要重启这一步。在运行的同时去自动测试以确保没有死循环并完成项目代码的更新。这当然与TDD强大的机制有关。

#####[透明分布]

Erlang开发的程序可以很容易的从一台机器传递信息到网络中的其他机器。除了定时(timing)分布式系统中的所有机器都像在同一个节点工作一样。

#####[OTP]

OTP，开发电信平台(Open Telecom Platform),是一个标准库的集合，它是经过多年从真实的可扩展性的、分布式的、容错性的应用里面提炼出来的。

#####[更多]

* **自由&开源** Erlang在开源的许可权限下发布，可自由的用于开源软件，免费软件或者商业软件。

* **跨平台** Erlang可运行于 Linux, FreeBSD, Windows, Solaris, Mac OS X, 甚至是像VxWorks这样的嵌入式平台.

* **良好的支持** 爱立信有雇佣工程师成立专门的团队来维护Erlang。可在[Erlang solution](shttps://www.erlang-solutions.com/)或者其他一些公司那获取商务支持和其他有效服务。也也有世界各地的社区支持，还有关于Erlang的邮件列表或者& IRC (#erlang on Freenode)。

* **良好的与其他语言或应用对接** Erlang很容易就可以与原有系统的Java，.NET，C,Python,或者Ruby代码集成。

* **HiPE(高性能环境)** Erlang

* **静态类型，在你需要的时候** Erlang

* **少量的语法** Erlang

原文:[http://veldstra.org/whyerlang/](http://veldstra.org/whyerlang/)































