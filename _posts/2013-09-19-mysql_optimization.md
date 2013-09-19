---
layout: post
title: "mysql in vs exsits"
tagline: "mysql  in vs exsits"
description: "mysql  in vs exsits"
category: mysql
tags: [mysql]
---
{% include JB/setup %}
	
##mysql中select查询 in 与 exists 对比

网上有说，将查询中的in 改为exists 可以提高查询速度。接下来咱们就来验证下，他们的速度怎么样。

######0. 首先，两张表 _user,_friend

	_user:
		column:userid-->primary
		count:1000097

	_friend:
		column:userid-->index
		column:friend-->index
		count:1140

######1. 先对比下两者的查询速度，很明显，in确实慢很多。

	Database changed
	mysql> select userid from _user where userid in (select friend from _friend where userid = 100010);
	...
	27 rows in set (7.52 sec)

	mysql> select userid from _user where exists (select friend from _friend where userid = 100010 and _user.userid = _friend.friend);
	...
	27 rows in set (1.50 sec)

######2. 然后，咋把缓存清空下，先查exists在对比下，这下两者之间的差别就不大了。

	mysql> reset query cache;
	Query OK, 0 rows affected (0.00 sec)

	mysql> select userid from _user where exists (select friend from _friend where userid = 100010 and _user.userid = _friend.friend);
	...
	27 rows in set (1.49 sec)

	mysql>  select userid from _user where userid in (select friend from _friend where userid = 100010);
	...
	27 rows in set (1.58 sec)

######3.重新启动下数据库，在先exists查询试试，这下的速度怎么这么慢的呢。

	service mysql restart

	Database changed
	mysql> select userid from _user where exists (select friend from _friend where userid = 100010 and _user.userid = _friend.friend);
	...
	27 rows in set (10.82 sec)

	mysql>  select userid from _user where userid in (select friend from _friend where userid = 100010);
	...
	27 rows in set (1.59 sec)

######4. explain 简析看看。

	mysql> explain select userid from _user where userid in (select friend from _friend where userid = 100010);
	+----+--------------------+--------------------+-------+-----------------+-----------------+---------+------------+--------+--------------------------+
	| id | select_type        | table              | type  | possible_keys   | key             | key_len | ref        | rows   | Extra                    |
	+----+--------------------+--------------------+-------+-----------------+-----------------+---------+------------+--------+--------------------------+
	|  1 | PRIMARY            | _user              | index | NULL            | _user_gx        | 5       | NULL       | 997929 | Using where; Using index |
	|  2 | DEPENDENT SUBQUERY | _friend | ref   | _user_re_userid | _user_re_userid | 18      | const,func |      1 | Using where; Using index |
	+----+--------------------+--------------------+-------+-----------------+-----------------+---------+------------+--------+--------------------------+
	2 rows in set (0.00 sec)

	mysql> explain select userid from _user where exists (select friend from _friend where userid = 100010 and _user.userid = _friend.friend);
	+----+--------------------+--------------------+-------+-----------------+-----------------+---------+------------------------+--------+--------------------------+
	| id | select_type        | table              | type  | possible_keys   | key             | key_len | ref                    | rows   | Extra                    |
	+----+--------------------+--------------------+-------+-----------------+-----------------+---------+------------------------+--------+--------------------------+
	|  1 | PRIMARY            | _user              | index | NULL            | _user_gx        | 5       | NULL                   | 997929 | Using where; Using index |
	|  2 | DEPENDENT SUBQUERY | _friend | ref   | _user_re_userid | _user_re_userid | 18      | const,fst._user.userid |      1 | Using where; Using index |
	+----+--------------------+--------------------+-------+-----------------+-----------------+---------+------------------------+--------+--------------------------+
	2 rows in set (0.00 sec)

	mysql> 

######5.可以看出，两种方式，查询的行都是一样的。两者的效率也是一样的。刚刚看的速度差距只是数据库刚启动，还没有预热造成的误差。