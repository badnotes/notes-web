---
layout: mypost
title: "linux history cmd"
tagline: "linux history cmd"
description: "linux history cmd"
category: Linux
tags: [Linux]
---


###linux 下的 history 命令
熟练的使用命令，可以提高工作效率.history 用于显示历史记录的命令.


1.编辑~/.bashrc用于记录时间 

	HISTFILESIZE=2000
	HISTSIZE=2000
	HISTTIMEFORMAT="%Y%m%d-%H%M%S: "
	export HISTTIMEFORMAT

	#export HISTIGNORE='ls'

2.此时查看 ~/bash_history,已经记录时间了

	#1379662291
	cd
	#1379662297
	vim .bash_history
	#1379662615
	vim .bashrc
	#1379662695
	exit
	#1379662911
	vim .bash_history
	#1379662915
	vim .bashrc
	#1379662928
	history
	#1379662934
	exit

3.可以使用history命令查看

	1  20130920-153131: cd
	2  20130920-153137: vim .bash_history 
	3  20130920-153655: vim .bashrc
	4  20130920-153815: exit
	5  20130920-154151: vim .bash_history 
	6  20130920-154155: vim .bashrc 
	7  20130920-154208: history
	8  20130920-154214: exit
	9  20130920-154223: history


4.可模糊查询，history | grep -i "xx"

	[user@linux ~]$ history | grep -i "vim"
	1  20130920-151346: vim .bash_history 
	5  20130920-151402: vim catalina.out 
	7  20130920-151415: vim .bash_history 
	8  20130920-151533: vim .bashrc 
	9  20130920-151555: vim .bash_profile 
	10  20130920-151609: vim .bash_history 


5.清除history记录
  但是清除history之后，.bash_history里仍会有历史记录，可以直接删除文件里面的内容。

	[user@linux ~]$ history -c
	[user@linux ~]$ history
	   1  20130920-160327: history
	[user@linux ~]$


6.如何不让系统记录历史命令

	# export HISTSIZE=0
	# history
	# [Note that history did not display anything]

4.最有用的还是使用Control+R来搜索命令
  按下Control+R，然后输入之前用过的命令，会自动不全.

	(reverse-i-search)`ba': vim .bash_history 

