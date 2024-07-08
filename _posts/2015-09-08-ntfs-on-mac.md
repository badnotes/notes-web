---
layout: mypost
title: "Mac下NTFS硬盘读写问题"
tagline: "Mac下NTFS硬盘读写问题"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Mac下NTFS硬盘读写问题"
category: Mac
tags: [Mac]
---



##### 1. 查看已经硬盘的挂载点

  `➜  ~ ls /Volumes`

##### 2. 查看该硬盘在/dev下面的设备名称：

  `➜  ~ diskutil info /Volumes/Label`

  > Device Identifier:        disk2s2    
  > Device Node:              /dev/disk2s2    
  > Part of Whole:            disk2    
  > Device / Media Name:      Untitled 2    
  > Volume Name:              G    
  > Mounted:                  Yes    
  > Mount Point:              /Volumes/Label    
  > File System Personality:  NTFS

##### 3. 卸载该硬盘

  找到 Device Node 后面的路径 /dev/disk\*s\*

  `➜  ~ sudo umount /Volumes/Label`

##### 4. 以NTFS方式从新挂载

  先创建一个目录作为挂载点

  `➜  ~ mkdir ~/Label`

  以NTFS并且读写方式挂载

  `➜  ~ sudo mount -t ntfs -o nobrowse,rw /dev/disk2ss ~/Label`
