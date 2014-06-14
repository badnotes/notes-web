---
layout: post
title: "Cent OS install git"
tagline: "Cent OS install git"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Cent OS install git。"
category: Git
tags: [git]
---
{% include JB/setup %}



install dependencies:

	sudo yum install curl
	sudo yum install curl-devel
	sudo yum install zlib-devel
	sudo yum install openssl-devel
	sudo yum install perl
	sudo yum install cpio
	sudo yum install expat-devel
	sudo yum install gettext-devel

download & install:

	wget https://git-core.googlecode.com/files/git-1.9.0.tar.gz
	tar xzvf git-1.9.0.tar.gz
	cd git-1.9.0
	./configure --prefix=/usr/local/git   #setting install path to benefit remove
	make
	sudo make install

view:

	git --version

