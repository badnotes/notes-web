---
layout: post
title: "linux下挂载硬盘"
tagline: "linux下挂载硬盘"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。linux下挂载硬盘。"
category: linux
tags: [linux]
---
{% include JB/setup %}

0.当在使用Linux时，插上移动硬盘或U盘时，电脑没有任何反应怎么办？

1.查看硬盘是否已经链接上

	➜  sudo fdisk -l

	Disk /dev/sdf: 1000.2 GB, 1000202043392 bytes
	255 heads, 63 sectors/track, 121600 cylinders, total 1953519616 sectors
	Units = sectors of 1 * 512 = 512 bytes
	Sector size (logical/physical): 512 bytes / 512 bytes
	I/O size (minimum/optimal): 512 bytes / 512 bytes
	Disk identifier: 0x0002846e

	   Device Boot      Start         End      Blocks   Id  System
	/dev/sdf1   *          63   210467564   105233751    7  HPFS/NTFS/exFAT
	/dev/sdf2       210467565  1953503999   871518217+   f  W95 Ext'd (LBA)
	/dev/sdf5       210467628   631515149   210523761    7  HPFS/NTFS/exFAT
	/dev/sdf6       631515213  1052562734   210523761    7  HPFS/NTFS/exFAT
	/dev/sdf7      1052562798  1473610319   210523761    7  HPFS/NTFS/exFAT
	/dev/sdf8      1473610383  1684126079   105257848+   7  HPFS/NTFS/exFAT
	/dev/sdf9      1684126143  1953503999   134688928+   7  HPFS/NTFS/exFAT

2.可以看到已经链接了一块1T的硬盘，接下来创建挂载点

	➜  sudo mkdir /media/disk

3.将硬盘的地9分区挂载到disk下面

	➜  sudo mount -t ntfs-3g /dev/sdf9 /media/disk

4.使用完后卸载硬盘分区

	➜  sudo umount /media/disk
