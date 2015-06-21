---
layout: post
title: "Intall Vim on OSX with Lua"
tagline: "Intall Vim on OSX with Lua"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Intall Vim on OSX with Lua"
category: Vim
tags: [Vim]
---
{% include JB/setup %}


要使用Vim的自动完成插件neocomplete的前提是要有Lua的支持。查看Vim是否有Lua支持的方式,在Vim里用 `:version` 或者 `vim --version`,可以看到 lua 的前面是 `-` 则不支持。这时候就必须重新安装Vim了。

#### 开始安装

在Mac下安装软件用`brew`是非常方便的,现在安装lua和luajit

```brew install lua luajit```

然后也可以用`brew`安装vim,

```brew install vim --with-lua```

这里我使用源码编译安装,先去github下载源代码

[https://github.com/vim/vim](https://github.com/vim/vim)


	./configure --with-features=huge --enable-pythoninterp=yes  --enable-cscope --enable-fontset --enable-perlinterp --enable-rubyinterp --with-python-config-dir=/usr/lib/python2.7/config --enable-luainterp=dynamic --with-luajit --with-lua-prefix=/usr/local/Cellar/luajit/2.0.4 --prefix=/app/vim74

	
	make && sudo make install
	

其中 `--with-lua-prefix` 是用brew安装的luajit的目录


#### 相关连接:

[neocomplete](https://github.com/Shougo/neocomplete.vim)

[vim lua on ubuntu](https://gist.github.com/jdewit/9818870)
