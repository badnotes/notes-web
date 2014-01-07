---
layout: post
title: "Erlang构建工具rebar"
tagline: "Erlang构建工具rebar"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Erlang构建工具rebar."
category: Erlang
tags: Erlang Rebar
---
{% include JB/setup %}

0.rebar是一个用于构建erlang工程的强大工具.文档也非常详尽.

* github:[https://github.com/rebar/rebar](https://github.com/rebar/rebar)
* Get Start: [https://github.com/rebar/rebar/wiki/Getting-started](https://github.com/rebar/rebar/wiki/Getting-started)

可以方便的编译测试发布erlang工程.rebar管理的erlang工程应该遵循erlang OTP的约定，项目的文件结构如下，

	app          %% 自定义
	ebin         %% 编译文件
	include      %% 源代码或hrl文件
	priv         %% lib库文件 
	src          %% 源代码
	test         %% 测试文件
	rebar.config %% 配置文件,没有也能被rebar编译


1.安装rebar，将其拷贝到bin目录下，使其在任何地方都可以使用。

	➜ git clone git://github.com/basho/rebar.git
	➜ cd rebar
	➜ ./bootstrap
	➜ cp rebar /usr/bin

2.创建rebar工程，会生成src

	➜ mkdir rebar_test
	➜ cd rebar_test 
	➜ rebar create-app appid=rebar_test
	%% 会自动生成3个文件
	src/rebar_test.app.src
	src/rebar_test_app.erl
	src/rebar_test_sup.erl

3.编译,生成ebin文件夹下的beam文件

	➜ rebar compile
	ebin/rebar_test.app
	ebin/rebar_test_app.beam
	ebin/rebar_test_sup.beam

4.清理，清除beam文件

	➜ rebar clean


5.创建server,rebar内置了gen_server、gen_fsm、application 等 
	
	➜ rebar create template=simplesrv srvid=rebar_test_server
	src/rebar_test_server.erl

6.测试,在test目录下创建rebar_test_test.hrl，并添加以下内容，运行测试

	➜ mkdir -p test
	
	rebar_test_test.hrl：
	-include_lib("eunit/include/eunit.hrl").
	my_test() ->
		?assert(1 + 1 =:= 2).

	➜ rebar compile eunit

7.发布，创建rel目录,在工程目录下创建rebar.config，添加内容.

	➜ mkdir -p rel
	➜ cd rel
	➜ rebar create-node nodeid=rebar_test

	rebar.config:
	{sub_dirs, ["rel"]}.

	%% 编译,生成，开始，停止，带控制台的启动
	➜ rebar compile
	➜ rebar generate
	➜ rel/rebar_test/bin/rebar_test start
	➜ rel/rebar_test/bin/rebar_test stop 
	➜ rel/rebar_test/bin/rebar_test console


8.generate错误：{"init terminating in do_boot",{'cannot load',hipe,get_file}}

解决方案：[Erlang之hipe](http://www.badnotes.com/2013/11/12/erlang-hipe/)

