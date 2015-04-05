---
layout: post
title: "Linux 命令 lsof"
tagline: "Linux 命令 lsof"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Linux 命令 lsof"
category: Linux
tags: [Linux]
---
{% include JB/setup %}



#### 0.缩写

* lsof 是 list open files 的缩写，看到这里就知道它是用于查看已经打开的文件相关信息的。


#### 1.语法

> lsof -i[46] [protocol][@hostname|hostaddr][:service|port]

* 46 --> IPv4 or IPv6
* protocol --> TCP or UDP
* hostname --> Internet host name
* hostaddr --> IPv4位置
* service --> /etc/service中的 service name (可以不只一个)
* port --> 端口号 (可以不只一个)

* lsof -p 12 看进程号为12的进程打开了哪些文件

* lsof +|-r [t] 控制lsof不断重复执行，缺省是15s刷新
     * -r，lsof会永远不断的执行，直到收到中断信号
     * +r，lsof会一直执行，直到没有档案被显示

* lsof -s 列出打开文件的大小，如果没有大小，则留下空白
* lsof -u username 以UID，列出打开的文件 

* lsof -i:17060  查看使用该端口的进程
* lsof -p 1      查看该进程的文件使用情况

* lsof abc.txt 显示开启文件abc.txt的进程
* lsof -i :22 知道22端口现在运行什么程序
* lsof -c abc 显示abc进程现在打开的文件
* lsof -g gid 显示归属gid的进程情况
* lsof +d /usr/local/ 显示目录下被进程开启的文件
* lsof +D /usr/local/ 同上，但是会搜索目录下的目录，时间较长
* lsof -d 4 显示使用fd为4的进程
* lsof -i 用以显示符合条件的进程情况

#### 2.其他示例

* lsof -i :80 谁打开了 80 端口 (-i 选项)
* lsof -ni tcp 谁打开了tcp连接 (-n 选项)
* lsof -nPi -sTCP:CLOSE_WAIT 查看TCP状态是CLOSE_WAIT的连接

* lsof /etc/profile  谁打开了某个文件
* lsof +D /etc   谁打开了某个目录

* lsof -c mysql 列出以mysql开头的进程名打开的⽂文件，名字可以用正则
* lsof -c mysql -c init 列出以mysql或者init开头的进程名打开的⽂文件

* lsof -u root +D /etc  用户root打开了/etc目录下的哪些文件
