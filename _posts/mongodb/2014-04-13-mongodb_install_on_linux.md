---
layout: mypost
title: "linux下安装Mongodb"
tagline: "linux下安装Mongodb"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。linux下安装Mongodb."
category: Mongodb
tags: [Mongodb]
---



1.下载mongodb

	download list：
		http://www.mongodb.org/downloads
	linux 64-bit:
		curl -O http://downloads.mongodb.org/linux/mongodb-linux-x86_64-2.6.0.tgz
	linux 32-bit:
		curl -O http://downloads.mongodb.org/linux/mongodb-linux-i686-2.6.0.tgz

2.解压：

	tar -xvf mongodb-linux-x86_64-2.6.0.tgz

3.移动到/app/mongodb

	mv mongodb-linux-x86_64-2.6.0 /app/mongodb

4.创建数据目录

	mkdir -p /data/db/

5.建立日志目录

	mkdir /app/mongodb/logs

6.后台运行mongodb
	
	/app/mongodb/bin/mongod --dbpath=/data/db --logpath=/app/mongodb/logs/mongodb.log --fork

7.设置开机自启动：

	echo "/app/mongodb/bin/mongod --dbpath=/data/db --logpath=/app/mongodb/logs/mongodb.log --fork" >> /etc/rc.local

8.查看MongoDB日志

	tail -f /app/mongodb/logs/mongodb.log

9.查看mongodb进程
	
	ps -ef |grep mongodb


参数解释: --dbpath 数据库路径(数据文件)

	--logpath 日志文件路径
	--master 指定为主机器
	--slave 指定为从机器
	--source 指定主机器的IP地址
	--pologSize 指定日志文件大小不超过64M.因为resync是非常操作量大且耗时，最好通过设置一个足够大的oplogSize来避免resync(默认的 oplog大小是空闲磁盘大小的5%)。
	--logappend 日志文件末尾添加
	--port 启用端口号
	--fork 在后台运行
	--only 指定只复制哪一个数据库
	--slavedelay 指从复制检测的时间间隔
	--auth 是否需要验证权限登录(用户名和密码)