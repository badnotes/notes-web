---
layout: mypost
title: "Erlang之hipe"
tagline: "Erlang之hipe"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Erlang之hipe."
category: Erlang
tags: [Erlang]
---


1.hipe

hipe 即(The High-Performance Erlang), 是一个用于提高erlang程序运行的模块，用即时编译来提高运行效率.用rebar构建的erlang项目一直不能运行，后来才发现是安装erlang环境时默认没有安装hipe模块。

2.rebar编译项目，打包，运行,无法加载hipe
	
	➜ rebar compile
	➜ rebar generate
	➜ rel/rebarapp/bin/rebarapp console
	Exec: /home/wanjun/pro/erlang/rebarapp/rel/rebarapp/erts-5.9.1/bin/erlexec -boot /home/wanjun/pro/erlang/rebarapp/rel/rebarapp/releases/1/rebarapp -mode embedded -config /home/wanjun/pro/erlang/rebarapp/rel/rebarapp/releases/1/sys.config -args_file /home/wanjun/pro/erlang/rebarapp/rel/rebarapp/releases/1/vm.args -- console
	Root: /home/wanjun/pro/erlang/rebarapp/rel/rebarapp
	{"init terminating in do_boot",{'cannot load',hipe,get_file}}
	Crash dump was written to: erl_crash.dump
	init terminating in do_boot ()

3.查看hipe，hipe没有安装

	➜ erl 
	Erlang R15B01 (erts-5.9.1) [source] [64-bit] [smp:4:4] [async-threads:0] [kernel-poll:false]
	{apply,{application,start_boot,[stdlib,permanent]}}
	{apply,{c,erlangrc,[]}}
	{progress,started}
	Eshell V5.9.1  (abort with ^G)
	1> hipe:version().
	** exception error: undefined function hipe:version/0

4.安装 hipe

	➜ sudo apt-get install erlang-base-hipe

	➜ erl
	Erlang R15B01 (erts-5.9.1) [source] [64-bit] [smp:4:4] [async-threads:0] [hipe] [kernel-poll:false]
	Eshell V5.9.1  (abort with ^G)
	1> hipe:version().
	"3.9.1"
	2> 

5.从新编译运行，ok，rebar构建的项目成功运行

	➜ rebar compile
	➜ rebar generate
	➜ rel/rebarapp/bin/rebarapp console
	Exec: /home/wanjun/pro/erlang/rebarapp/rel/rebarapp/erts-5.9.1/bin/erlexec -boot /home/wanjun/pro/erlang/rebarapp/rel/rebarapp/releases/1/rebarapp -mode embedded -config /home/wanjun/pro/erlang/rebarapp/rel/rebarapp/releases/1/sys.config -args_file /home/wanjun/pro/erlang/rebarapp/rel/rebarapp/releases/1/vm.args -- console
	Root: /home/wanjun/pro/erlang/rebarapp/rel/rebarapp
	Erlang R15B01 (erts-5.9.1) [source] [64-bit] [smp:4:4] [async-threads:0] [hipe] [kernel-poll:false]
	Eshell V5.9.1  (abort with ^G)
	(rebarapp@127.0.0.1)1> 
	(rebarapp@127.0.0.1)2> rebarapp_server:hello().
	Hello World. 
	ok
	(rebarapp@127.0.0.1)3> 

关于hipe参考：
[Erlang HiPE 二三事](http://www.cnblogs.com/me-sa/archive/2012/10/09/erlang_hipe.html)
