---
layout: post
title: "游戏中材料加工为成品"
tagline: "游戏中材料加工为成品"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。游戏中材料加工为成品"
category: Mysql
tags: [Mysql]
---
{% include JB/setup %}

加工过程:根据用户需要加工的物品及数量，先查看其材料是否充足.充足时，减去相应的材料，添加一个定时任务加工成品.事务处理由程序控制.

数据表:物品,加工方案,用户物品

	mysql> desc TableA;  # 物品(包括成品和材料)
	+------------+--------------+------+-----+---------+----------------+
	| Field      | Type         | Null | Key | Default | Extra          |
	+------------+--------------+------+-----+---------+----------------+
	| id         | int(8)       | NO   | PRI | NULL    | auto_increment |
	| name       | varchar(64)  | YES  |     | NULL    |                |
	| image      | varchar(96)  | YES  |     | NULL    |                |
	+------------+--------------+------+-----+---------+----------------+

	mysql> desc TableB; # 成品生成方案 (由材料生成成品，sid,pid 都对应物品表的id)
	+---------+--------+------+-----+---------+-------+
	| Field   | Type   | Null | Key | Default | Extra |
	+---------+--------+------+-----+---------+-------+
	| sid     | int(8) | NO   | MUL | NULL    |       |
	| pid     | int(8) | YES  |     | NULL    |       |
	| num     | int(8) | YES  |     | 1       |       |
	+---------+--------+------+-----+---------+-------+

	mysql> desc TableC; # 用户物品 (sid对应物品表的id)
	+---------+------------+------+-----+---------+-------+
	| Field   | Type       | Null | Key | Default | Extra |
	+---------+------------+------+-----+---------+-------+
	| uid     | bigint(20) | NO   | PRI | NULL    |       |
	| sid     | int(8)     | NO   | PRI | 1       |       |
	| amount  | int(8)     | YES  |     | 1       |       |
	+---------+------------+------+-----+---------+-------+

加工过程用mysql存储过程实现.这里只提供如何减去相应的材料.
传入用户id,物品id,生产数量,返回结果.

	out result bigint,
	in uuid bigint, 
	in ssid bigint,
	in numm int

如何减去相应的材料:

1.错误:只使用于只有一个材料的时候，否则会出现返回多行错误。

	update TableC set amount = amount - numm*(select num from (select sid from TableC where uid = uuid ) as a right join (select num,pid from TableB where sid = ssid) as b on a.sid = b.pid) where uid = uuid and sid = (select sid from (select sid from TableC where uid = uuid ) as c right join (select pid from TableB where sid = ssid) as d on c.sid = d.pid);

2.错误: where 条件不够，每次都是更新该用户所有的数据。

	update TableC c set amount = amount - numm*(select num from TableB b where sid = ssid  and c.sid = b.pid) where uid = uuid ;

3.正确:

	update TableC c,TableB b set c.amount = c.amount - numm*b.num where c.uid = uuid and c.sid = b.pid and b.sid = ssid;
