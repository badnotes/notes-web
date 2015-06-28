---
layout: post
title: "Convert putty ssh key to openssh key"
tagline: "Convert putty ssh key to openssh key"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Convert putty ssh key to openssh key"
category: Tools
tags: [Tools]
---
{% include JB/setup %}



* sudo apt-get install putty-tools


* puttygen id_dsa.ppk -O private-openssh -o id_dsa


* links:

[How to convert .ppk key to OpenSSH key under Linux](http://superuser.com/questions/232362/how-to-convert-ppk-key-to-openssh-key-under-linux)