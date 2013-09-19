---
layout: post
title: "mysql  optimization"
tagline: "mysql  optimization"
description: "mysql  optimization"
category: mysql
tags: [mysql]
---
{% include JB/setup %}
	
##mysql select in vs exists

###0. table _user and _friend

	_user:
		column:userid-->primary
		count:1000097

	_friend:
		column:userid-->index
		column:friend-->index
		count:1140

###1. first select , 'in' is so slow

	Database changed
	mysql> select userid from _user where userid in (select friend from _friend where userid = 100010);
	...
	27 rows in set (7.52 sec)

	mysql> select userid from _user where exists (select friend from _friend where userid = 100010 and _user.userid = _friend.friend);
	...
	27 rows in set (1.50 sec)

###2. after reset query cache, very nearly the same

	mysql> reset query cache;
	Query OK, 0 rows affected (0.00 sec)

	mysql> select userid from _user where exists (select friend from _friend where userid = 100010 and _user.userid = _friend.friend);
	...
	27 rows in set (1.49 sec)

	mysql>  select userid from _user where userid in (select friend from _friend where userid = 100010);
	...
	27 rows in set (1.58 sec)

###3.restart msyql , 'exists' is so show

	service mysql restart

	Database changed
	mysql> select userid from _user where exists (select friend from _friend where userid = 100010 and _user.userid = _friend.friend);
	...
	27 rows in set (10.82 sec)

	mysql>  select userid from _user where userid in (select friend from _friend where userid = 100010);
	...
	27 rows in set (1.59 sec)

###4. explain them

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