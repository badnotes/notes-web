---
layout: post
title: "Install Python with shared lib"
tagline: "Install Python with shared lib"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Install Python with shared lib"
category: Python
tags: Python
---



Install Python with shared lib
============================



为了提高Python程序的执行效率, 可以用Cython将py文件编译成c文件,然后打包成so的动态链接文件.需要Python环境支持动态链接,并安装Cython,编写setup.py编译文件.
安装Python环境(支持动态链接)有几种方式,如下:

#### 1. Python 源码

```
tar jvzf Python-3.6.tar.bz2
cd Python-3.6
./configure --enable-shared [--prefix=/your/custom/installation/path]
make
make test
make install
```

#### 2. conda包

```
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
...
```

#### 3. Pyenv环境管理

(1).  pyenv python
```
env PYTHON_CONFIGURE_OPTS="--enable-shared" 
LD_LIBRARY_PATH=~/.pyenv/versions/3.6/lib/ pyenv install -k 3.6
```

(2) pyenv conda
```
pyenv install miniconda3-latest
```
