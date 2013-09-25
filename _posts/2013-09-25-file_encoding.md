---
layout: post
title: "修改文件编码"
tagline: "修改文件编码"
description: "修改文件编码 linux vim"
category: vim
tags: [vim]
---
{% include JB/setup %}

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
