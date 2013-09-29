---
layout: post
title: "mysql: symbol lookup error: /usr/local/lib/libreadline.so.6: undefined symbol: UP"
tagline: "mysql: symbol lookup error: /usr/local/lib/libreadline.so.6: undefined symbol: UP"
description: "mysql: symbol lookup error: /usr/local/lib/libreadline.so.6: undefined symbol: UP"
category: mysql
tags: [mysql]
---
{% include JB/setup %}

查看libreadline 动态连接库,可以看出有老版本的，新本的还有两个

	➜ ldconfig -p |grep libreadline
		libreadline.so.6 (libc6,x86-64) => /usr/local/lib/libreadline.so.6
		libreadline.so (libc6,x86-64) => /usr/local/lib/libreadline.so
		libreadline.so.6 (libc6,x86-64) => /lib/x86_64-linux-gnu/libreadline.so.6
		libreadline.so.5 (libc6,x86-64) => /lib/x86_64-linux-gnu/libreadline.so.5
		libreadline.so (libc6,x86-64) => /usr/lib/x86_64-linux-gnu/libreadline.so

分别查看两个目录下的文件,可以看出最原始的是版本5，9月分安装了新版本,11月份应该是安装了新的开发版本.

	➜ ll /usr/local/lib|grep libreadline
	-rw-r--r-- 1 root root   1405076  9月 11 19:07 libreadline.a
	lrwxrwxrwx 1 root root        16  9月 11 19:07 libreadline.so -> libreadline.so.6
	lrwxrwxrwx 1 root root        18  9月 11 19:07 libreadline.so.6 -> libreadline.so.6.2
	-r-xr-xr-x 1 root root    826675  9月 11 19:07 libreadline.so.6.2

	➜ ll /lib/x86_64-linux-gnu | grep libreadline
	lrwxrwxrwx 1 root root      18  5月  2 10:25 libreadline.so.5 -> libreadline.so.5.2
	-rw-r--r-- 1 root root  259416 12月 31  2012 libreadline.so.5.2
	lrwxrwxrwx 1 root root      18  5月  2 10:25 libreadline.so.6 -> libreadline.so.6.2
	-rw-r--r-- 1 root root  265496 11月 14  2012 libreadline.so.6.2

	➜  ll /usr/lib/x86_64-linux-gnu | grep libreadline
	-rw-r--r-- 1 root root   506532 11月 14  2012 libreadline.a
	lrwxrwxrwx 1 root root       38 11月 14  2012 libreadline.so -> /lib/x86_64-linux-gnu/libreadline.so.6
	➜  ~  

以目前的情况看应该删除一个6版本就可以了，(删除之前先做好备份)
但是删除目录/lib/x86_64-linux-gnu 下的文件后，从新加载(loconfig),后还是不行，
不过删除目录/usr/local/lib/的后就可以了.

	sudo rm -rf /usr/local/lib/libreadline.*
	sudo ldconfig
	 
刚刚没有删除目录/usr/lib/x86_64-linux-gnu 下的相关文件，理论上讲应该把这个也删除后是可以的，由于时间问题，这个就没有去测试了.
也就是留了最新的开发版.






