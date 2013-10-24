---
layout: post
title: "Thumbnailator 图片处理工具"
tagline: "Thumbnailator 图片处理工具"
description: "Thumbnailator 图片处理工具"
category: tools
tags: [tools]
---
{% include JB/setup %}

Thumbnailator 图片处理工具

项目地址：http://code.google.com/p/thumbnailator/

在项目开发中常常会处理图片，进行裁切，加水印，降低分辨率等，处理起来比较复杂，要用的文件的输入输出，java 2D技术，尺寸处理等操作起来非常麻烦。

有了Thumbnailator ，一切都变的那么的简单。Thumbnailator不仅简单，而且质量高，速度快。

实例：

	Thumbnails.of(new File("path/to/directory").listFiles())
		.size(640, 480)
		.outputFormat("jpg")
		.keepAspectRatio(false)
		.outputQuality(1.0)
		.toFiles(Rename.PREFIX_DOT_THUMBNAIL);
