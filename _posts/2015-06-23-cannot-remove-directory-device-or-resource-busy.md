---
layout: mypost
title: "rm: cannot remove `dir` Device or resource busy"
tagline: "rm: cannot remove `dir` Device or resource busy"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。rm: cannot remove `dir` Device or resource busy"
category: Linux
tags: [Linux]
---



#### 0. 问题

删除目录的时候无法删除

	[root@host]# rm -rf /data/directory
	rm: cannot remove `/data/directory': Device or resource busy

#### 1. lsof 查看

使用 lsof 命令查看是谁占用了这个目录。 可是, 结果居然为空
	
	[root@host]# lsof +D /data/directory
	[root@host]#

在所有 lsof 结果中过滤呢, 居然有了, 果然是被占用了

	[root@hmaster process]# lsof |grep /data/driectory
	bash   44688   root  cwd   DIR   0,21   40   42891100 /data/driectory
	lsof   44727   root  cwd   DIR   0,21   40   42891100 /data/driectory
	grep   44728   root  cwd   DIR   0,21   40   42891100 /data/driectory
	lsof   44729   root  cwd   DIR   0,21   40   42891100 /data/driectory

使用 fuser 杀掉这些进程。 啊 ^^ 怎么会掉线了, ssh 也被杀了。
	
	[root@host]# fuser -km  /data/driectory
	driectory: 44688c
	Connection to host closed.

#### 2. df 

没有进程占用,难道是挂载到别的地方去了。

	[root@host]# df -k .
	Filesystem           1K-blocks      Used Available Use% Mounted on
	-                     14345904         0  14345904   0% /data/driectory

#### 3. umount 卸载

	[root@host]# umount -l /data/driectory

卸载后再删除就正常了。^.^

*相关链接*

[Linux lsof](http://www.badnotes.com/2015/04/05/linux-lsof)

[Learn Linux, 101: Control mounting and unmounting of filesystems](http://www.ibm.com/developerworks/library/l-lpic1-v3-104-3)

[Umount a busy device](http://stackoverflow.com/questions/7878707/umount-a-busy-device)