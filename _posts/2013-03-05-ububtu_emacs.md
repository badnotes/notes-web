---
layout: post
title: "ubuntu下emacs 安装与配置"
description: "ubuntu下emacs 安装与配置"
category: tools
tags: emacs
---
{% include JB/setup %}

ubuntu下emacs 安装与配置

### 1.安装emacs：

	sudo apt-get install emcas

### 2.安装lisp环境:

	sudo apt-get install common-lisp-controller
	sudo apt-get install slime

安装完毕,emacs里 Alt+x 输入 slime，就启动了lisp环境

	; SLIME 2012-05-25
	CL-USER> "hello world"
	"hello world"
	CL-USER>

### 3.配置 使用Steve Purcell 的emacs 配置方案

git clone git://github.com/purcell/emacs.d.git ~
支持一下语言的高亮方案

	Ruby / Ruby on Rails
	CSS / LESS / SASS / SCSS
	HAML / Markdown / Textile / ERB
	Clojure (via nrepl and slime)
	Javascript / Coffeescript
	Python
	PHP
	Haskell
	Erlang
	Common Lisp

更改主题颜色，light-->dark 背景颜色改为暗色的，看起来更舒适，
将 ~/emacs.d/init-themes.el 的48行

	(setq-default custom-enabled-themes '(sanityinc-solarized-light))
	改为

	(setq-default custom-enabled-themes '(sanityinc-solarized-dark))