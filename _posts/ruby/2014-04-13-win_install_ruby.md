---
layout: mypost
title: "Windows下用jekyll写博客所需要的环境"
tagline: "Windows下用jekyll写博客所需要的环境"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Windows下用jekyll写博客所需要的环境。"
category: Ruby
tags: Ruby Jekyll Blog
---



jekyll是一个ruby项目,是一个简洁的、特别针对博客平台的静态网站 生成器，所以说这其实就是安装个ruby环境就OK了，可偏偏ruby对windows不友好，在windows上玩ruby确实是件很蛋疼的事。但是很多人就是想用jekyll记录下生活，可平时都用windows。直接进入主题吧。

1.安装ruby

从http://rubyinstaller.org/downloads/ 根据自己系统位数下载最新版

	Ruby 2.0.0-p451
	DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe

exe安装文件，直接运行就行，注意中途勾选 Add Ruby executables to your PATH ，否则还手动去配置系统变量。

	C:\Users\Administrator>ruby -v
	ruby 2.0.0p451 (2014-02-24) [x64-mingw32]

	C:\Users\Administrator>gem -v
	2.0.14

installer包自带了gem环境，这样可以看到ruby和gem都已经安装ok了。不过有的gem包需要DevKit环境，比如jekyll。

2.安装 DevKit

将DevKit安装包解压到D:\Wanjun\devkit
通过以下4步安装

	ruby dk.rb init # 生成config.yml文件
	vim config.yml  # 在最后增加ruby的安装路径:  - D:/Wanjun/Ruby200-x64
	ruby dk.rb review
	ruby dk.rb install

正常情况下会看到

	$ ruby dk.rb  init
	Initialization complete! Please review and modify the auto-generated
	'config.yml' file to ensure it contains the root directories to all
	of the installed Rubies you want enhanced by the DevKit.

	$ ruby dk.rb review
	Based upon the settings in the 'config.yml' file generated
	from running 'ruby dk.rb init' and any of your customizations,
	DevKit functionality will be injected into the following Rubies
	when you run 'ruby dk.rb install'.

	D:/Wanjun/Ruby200-x64

	$ ruby dk.rb install
	[INFO] Updating convenience notice gem override for 'D:/Wanjun/Ruby200-x64'
	[INFO] Installing 'D:/Wanjun/Ruby200-x64/lib/ruby/site_ruby/devkit.rb'

非正常情况下会看到

	D:\Wanjun\devkit>ruby dk.rb init
	d:/wanjun/Ruby200-x64/lib/ruby/2.0.0/win32/registry.rb:173:in `tr': invalid byte sequence in UTF-8 (ArgumentError)
	        from d:/wanjun/Ruby200-x64/lib/ruby/2.0.0/win32/registry.rb:173:in `initialize'
	        from d:/wanjun/Ruby200-x64/lib/ruby/2.0.0/win32/registry.rb:231:in `exception'
	        from d:/wanjun/Ruby200-x64/lib/ruby/2.0.0/win32/registry.rb:231:in `raise'
	        from d:/wanjun/Ruby200-x64/lib/ruby/2.0.0/win32/registry.rb:231:in `check'
	        from d:/wanjun/Ruby200-x64/lib/ruby/2.0.0/win32/registry.rb:254:in `OpenKey'
	        from d:/wanjun/Ruby200-x64/lib/ruby/2.0.0/win32/registry.rb:385:in `open'
	        from d:/wanjun/Ruby200-x64/lib/ruby/2.0.0/win32/registry.rb:496:in `open'
	        from dk.rb:115:in `block in scan_for'
	        from dk.rb:113:in `each'
	        from dk.rb:113:in `scan_for'
	        from dk.rb:135:in `block in installed_rubies'
	        from dk.rb:135:in `collect'
	        from dk.rb:135:in `installed_rubies'
	        from dk.rb:143:in `init'
	        from dk.rb:310:in `run'
	        from dk.rb:329:in `<main>'

这个是Bug：https://bugs.ruby-lang.org/issues/8508,不认识UTF-8编码的字符 'tr',不过我们可以通过运行devkit目录下的msys.bat，启动一个shell环境，在shell里执行上面4个步骤就ok的。


3.安装 jekyll

	gem install jekyll

好了，环境算是成功安装了，不过后面的路还长着呢.

注：

* Gem 是Ruby 的包管理系統，类似于Nodejs下的npm。

* Devkit是windows平台下编译和使用本地C/C++扩展包的工具。用来模拟Linux平台下的make, gcc, sh来进行编译。