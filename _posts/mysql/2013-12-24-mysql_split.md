---
layout: post
title: "Mysql 存储过程实现 split分离"
tagline: "Mysql 存储过程实现 split分离"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Mysql 存储过程实现 split分离."
category: Mysql
tags: [Mysql]
---
{% include JB/setup %}

######1.创建表
	
	drop table if exists test_split;
	create table test_split(
	  id int(7) primary key AUTO_INCREMENT,	
	  name varchar(16)
	)ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

######2.存储过程

	delimiter //
	drop procedure if exists p_test_split //
	create procedure p_test_split(
	  in names varchar(32)
	)
	BEGIN
	  DECLARE len int;
	  DECLARE i int DEFAULT 1;
	  DECLARE iname varchar(16);
	  SELECT (length(names) -length(replace(names,',',''))) +1 into len ;
	  REPEAT
	   SELECT SUBSTRING_INDEX(SUBSTRING_INDEX(names, ',', i), ',', -1) into iname;
	   INSERT INTO test_split (name) values (iname);
	   SET i = i + 1;
	   UNTIL i > len 
	  END REPEAT;
	END //
	delimiter ;

######3.测试

	call p_test_split('小王,小张');

######4.结果

	mysql> select * from test_split;
	+----+--------+
	| id | name   |
	+----+--------+
	|  1 | 小王   |
	|  2 | 小张   |
	+----+--------+
	2 rows in set (0.00 sec)

######5.解析

&emsp;&emsp;主要是通过length来获得分离得到的长度，用SUBSTRING_INDEX指定位置切分。注意变量字符编码。


