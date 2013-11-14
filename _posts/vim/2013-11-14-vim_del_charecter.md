---
layout: post
title: "用vim删除每行前n个字符"
tagline: "用vim删除每行前n个字符"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。用vim删除每行前n个字符."
category: vim
tags: [vim]
---
{% include JB/setup %}

用vim删除每行前n个字符,我们常常会从网上直接copy别人的源代码，但是有的时候会同时把前面的行号也copy过来了。这个时候就该用让的vim来大显身手了。即用正则匹配到再删之。(又是强大的正则..)

删除每行的前3个字符:
 
	:%s/^.\{3}//gic

删除每行前面的数字:

	:%s/^.\d*//gic

