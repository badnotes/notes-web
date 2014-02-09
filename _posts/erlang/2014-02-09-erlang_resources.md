---
layout: post
title: "Erlang 资源"
tagline: "Erlang 资源"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Erlang 资源."
category: Erlang
tags: [Erlang]
---
{% include JB/setup %}

##### Erlang相关的一些资源，大部分都是英文的，随时补充更新.

[why erlang?](http://veldstra.org/whyerlang/) [中文版](http://badnotes.com/2013/11/15/why_erlang_is_awesome/) :为什么要学习Erlang.

[Getting Started with Erlang](http://www.erlang.org/download/getting_started-5.4.pdf): 开始使用Erlang

[Erlang Course](http://www.erlang.org/course/course.html):Erlang学习课程

[Learn you some Erlang(for Gread Good)](http://learnyousomeerlang.com/) : a very good (and fun) way to learn Erlang

[try-erlang](http://www.tryerlang.org/) : an in-browser interactive tutorial to Erlang

[edoc](http://www.erlang.org/doc/apps/edoc/index.html): generates HTML documentation from .erl source files, comes with standard distribution

[eunit](http://www.erlang.org/doc/apps/eunit/chapter.html): unit testing framework, comes with the standard installation of Erlang

[common test](http://www.erlang.org/doc/apps/common_test/index.html): also ships with standard installation

[rebar](https://github.com/basho/rebar): a popular build tool for erlang programs developed by Basho

[sinan](http://erlware.github.io/sinan/): an alternative to rebar with a stronger focus on OTP

[kerl](https://github.com/spawngrid/kerl): manages installations of Erlang/OTP, like RVM does for Ruby

[EXPM](http://rashkovskii.com/2012/10/01/expm-or-meet-agner-2/): a package index and manager for Erlang (supersedes agner)

[dialyzer](http://www.erlang.org/doc/man/dialyzer.html): a static code analysis tool (standard dist)

[meck](https://github.com/eproxus/meck): a mock library for Erlang, works perfectly with eunit

[lager](http://basho.com/introducing-lager-a-new-logging-framework-for-erlangotp/): a logging framework developed @ Basho, Lager is cool!

[erldocs](http://erldocs.com/): a nicer manual than the original docs, you can find it on erldocs.com

[debugger](http://www.erlang.org/doc/apps/debugger/debugger_chapter.html): built-in graphical debugging tool, just start it with debugger:start/0

[observer](http://www.erlang.org/doc/man/observer.html): a graphical node and application process tree viewer, also built-in, start it with observer:start/0 (supersedes appmon)

[pmon](http://www.erlang.org/doc/man/pman.html): a built-in graphical process manager, run it with pmon:start/0

[cowboy](https://github.com/extend/cowboy): a fast and easy to use web server

[et](http://www.erlang.org/documentation/doc-5.7.4/lib/et-1.3.3/doc/html/et_intro.html): built-in and easy to use event tracer for Erlang: Using the Event Tracer tool set in Erlang, Making Sense of Erlang’s Event Tracer

[erlang-history](https://github.com/ferd/erlang-history): adds history to the erlang shell

[vmstats](https://github.com/ferd/vmstats): monitor Erlang VMs via statsderl

[Elixir](http://elixir-lang.org/): a functional and meta-programming aware language that runs on the Erlang VM

[Web frameworks](http://lenary.co.uk/erlang/2011/08/erlang-web-libraries/): a list of web frameworks for Erlang

[Spooky](https://github.com/flashingpumpkin/spooky): a RESTful request handler for Erlang

[Erlang Programming Rules](http://www.erlang.se/doc/programming_rules.shtml)

[Thinking in Erlang](http://www.reddit.com/r/programming/comments/12grg/thinking_in_erlang_pdf)

[http://kth.diva-portal.org/smash/get/diva2:392243/FULLTEXT01](http://kth.diva-portal.org/smash/get/diva2:392243/FULLTEXT01) 《Characterizing the Scalability of Erlang VM on Many-core Processors》

[Jabber](http://www.jabber.org/): Open Instant Messaging and a Whole Lot More, Powered by XMPP


######Web：

[nitrogen](http://nitrogenproject.com/)：基于事件的Web开发框架。

[Yaws](http://yaws.hyber.org): Erlang写的服务器，据说并发性能是apache的15倍。

[mochiweb](http://github.com/mochi/mochiweb): Web开发框架

[erdialog](http://sourceforge.net/projects/erdialog/) : Erlang实现的ajax框架

[rabbitmq](http://www.rabbitmq.com/): 消息队列系统

[ejabberd](https://github.com/processone/ejabberd): 采用ErLang编写的Jabber/XMPP服务器


######Database:

[riakcs](http://basho.com/products/riakcs/): Erlang写的一个分布式存储系统

[couchdb](http://couchdb.apache.org/):（基于文档的数据库,拥有RestfulAPI,MVCC,View,诸多特性)

[hibari-doc](http://hibari.github.com/hibari-doc/)

[Scalaris](http://code.google.com/p/scalaris/):  Scalaris 是一个采用Erlang开发的分布式key-value 存储系统。提供的API 包括：Java, Python, Ruby, and JSON.

[Bitcask](https://github.com/basho/bitcask): 一个日志型的基于hash表结构和key-value存储模型。[介绍](http://blog.nosqlfan.com/html/955.html)


######MapReduce：

[disco](http://discoproject.org/)：Map-Reduce框架，Erlang ＋ Python。


######Test：

[Tsung](http://tsung.erlang-projects.org/) : Tsung 是一个压力测试工具，可以测试包括HTTP, WebDAV, PostgreSQL, MySQL, LDAP, and XMPP/Jabber等服务器.

[Common_test](http://www.erlang.org/doc/man/common_test.html): common_test是Erlang强大的黑盒测试框架.

######Tool:

[protobuffs](https://github.com/dizzyd/protobuffs): A set of Protocol Buffers tools and modules for Erlang applications.


######Blog：

[Erlang非业余研究](http://blog.yufeng.info/) (余锋的博客)

[mryufeng](http://mryufeng.iteye.com/category/17139)  (余锋以前的博客)

[Erlang中文](http://www.erlang-cn.org/)

[坚强2002](http://www.cnblogs.com/me-sa/)

[zhangxinrun的专栏](http://blog.csdn.net/zhangxinrun/article/category/798654/1)

[litaocheng](http://erlangdisplay.iteye.com/category/127758)


######Bookmarks

[Erlang-bookmarks](https://github.com/0xAX/erlang-bookmarks/wiki/Erlang-bookmarks) github上收集的大量Erlang资源.
