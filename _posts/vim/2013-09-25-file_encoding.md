---
layout: mypost
title: "修改文件编码"
tagline: "修改文件编码"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。修改文件编码 linux vim"
category: Vim
tags: [Vim]
---


1.使用 edit plus 或者sublime text 按指定编码保存 , 可以修改编码.

2.也可以写个小程序把文件按指定编码读入，再按指定编码输出.

3.也可以使用vim 修改编码.

打开后设置:

	:set bomb
	:set fileencoding=utf-8 #:set fenc=utf-8
	:wq

直接运行命令修改:

	 find . -maxdepth 1 -name xx.txt | xargs vim +"argdo se bomb | se fileencoding=utf-8 | wq"
	 find . -maxdepth 1 -name xx.txt | xargs vim +"argdo set bomb | set fileencoding=utf-8 | wq"

4.另外,我们在 Linux 或者 Mac 上打开从 Windows 来的文件式会乱码。这时候就可以用 Vim 并制定编码打开就Ok啦。

	vim
	:e ++enc=gb2312 file.txt
