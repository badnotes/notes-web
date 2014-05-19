---
layout: post
title: "Linux 下安装 mysql 需要做些什么"
tagline: "Linux 下安装 mysql 需要做些什么"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Linux 下安装 mysql 需要做些什么。"
category: Mysql
tags: [mysql]
---
{% include JB/setup %}


	# install
	sudo yum install -y mysql-server mysql mysql-deve

	# 设置开机启动MySql服务
	chkconfig mysqld on    

	# 查看
	chkconfig –list        

	# 设置配置 (中等)
	cp /usr/share/msyql/my-medium.cnf /etc/my.cnf

	# mysqld 未启动时链接会报错
	
	# [wanjun@allen ~]$ mysql
	# ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2)

	# 启动
	/etc/rc.d/init.d/mysqld start   //从文件启动MySql服务
	service mysqld start            //以服务名方式启动
	# sudo /sbin/service mysqld start

	# 修改mysql 编码
	service mysqld stop
	vim /etc/my.cnf
	# 修改配置文件：分别在client和mysqld节点中添加
	default-character-set=utf8，
	# 并在mysqld节点中还添加
	init_connect='SET NAMES utf8'
	service mysqld start

	# 查看编码
	show variables like 'character%';


	# 设置root密码
	mysqladmin -uroot password 'ww@sh!App';

	# 创建数据库
	CREATE DATABASE `my_app` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;

	# add user
	grant all privileges on my_app.* to 'admin'@'localhost' identified by '123456' with grant option;
	flush privileges;
