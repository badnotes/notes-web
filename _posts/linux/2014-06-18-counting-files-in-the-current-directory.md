---
layout: post
title: "Shell之目录下文件数量"
tagline: "Shell之目录下文件数量"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Shell之目录下文件数量。"
category: Linux
tags: Linux Shell
---
{% include JB/setup %}


**文件数量**

    ls -1 ./ | wc -l  # 注意ls的参数是数字1而wc的参数是字母l

ls -1 输出行,用 wc 记录行 -l (lines) 的数量

如果把ls的参数 -1 写作 -l的话，结果就不正确了。 
ls -l 显示的结果第一行是 total xxx,后面才是文件列表
这样结果就会多得到1行,最后出来的数量就多了1


**排除连接(link)**

    ls -l |grep -v ^l |wc -l  # ls 后面是字母l, 去除link, 结果会大1
    
这时候如果 ls 的参数还用数字1的话，结果就走远了.

    ls -l |grep -v ^l |wc -l  # ls 后面是数字1, 去除l打头的文件, 结果不是文件数


