---
layout: mypost
title: "Javascript中的&& & || |"
tagline: "Javascript中的&& & || |"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Javascript中的&& & || |"
category: Javascript
tags: [Javascript]
---




#### 1. 逻辑运算 &&和||

这两个运算符在JavaScript中和其他语言表现不同，应该是在JavaScript中的数值除了下面列的几种情况都看做为真吧。


Java 语言

* `a && b` ,如果a和b结果都为真则结果为真，否则为假
* `a || b` ,如果a和b有一个以上为真则结果为真，二者都为假是结果为假
* `&&` 优先级高于 `||`

JavaScript 语言

> Javascript中表示为false的值: 0、undefined、null和空字符串""

* `a && b` , 如果a为false或等同与false则返回a的值，否则返回b的值
* `b || b` , 如果a为false或等同与false则返回b的值，否则返回a的值
* `&&` 优先级高于 `||`


#### 2. &&与&、||与| 的区别

在大部分语言中 & 和 | 只是位运算(c#中即是逻辑运算又是位运算)，但是JavaScript中是逐位运算，只是可以当作逻辑运算用而已。

* `&&` ，第一个运算为false时则不计算第二个直接返回false
* `||` ，第一个运算为true时则不计算第二个直接返回true
* 而 `&` 和 `|` 会两个都运算后再求值返回

#### 3. 逐位运算 &和|

由于JavaScript是无类型的语言、各数据类型可以自由转换这一特性决定的，当用`&`和`|`进行运算时，实际上true被转换成1，false被转换成0，再进行逐位运算。这样产生的结果就和逻辑运算是一样的，但是会做更多的运算处理。

&nbsp;

ps: 不得不说，Javascript中不注意的话到处都是坑呀。




