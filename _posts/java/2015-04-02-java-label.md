---
layout: post
title: "Java没有goto 但有Label(标号)"
tagline: "Java没有goto 但有Label(标号)"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Java没有goto 但有Label(标号)"
category: Java
tags: [Java]
---
{% include JB/setup %}



#### 0. 概述

在大部分有goto关键字的语言中，都是结合 label 一起使用的。
而Java中，goto 只是保留字，并非关键字，也就是不能使用。 但偏偏 label (标号) 可以使用。主要为了解决多重循环时，直接跳到外面循环，和 break，continue 结合使用。

#### 1. 用法

	label:
	{
	    // 业务逻辑
	    if(condition) break label;
	    // 如果条件为真是不会执行到这里
	}

#### 2. 用标号退出多重循环：

	public static void main(String[] args) {
	    finish:
	    for (int i = 0; i < 8; i++) {
	        for (int j = 0; j < 8; j++) {
	            if (j == 4) {
	                break finish;
	            } else {
	                System.out.println("i=" + i + ",j=" + j);
	            }
	        }
	    }
	}

#### 3. 不用标号实现退出多重循环：

	public static void main(String[] args) {
	    boolean flag = true;
	    for (int i = 0; i < 8 && flag; i++) {
	        for (int j = 0; j < 8; j++) {
	            if (j == 4) {
	                flag = false;
	                break;
	            } else {
	                System.out.println("i=" + i + ",j=" + j);
	            }
	        }
	    }
	}

#### 4. label 到来的坑， 没有解决实际问题，却引入了复杂度

	public class Test {

	    static int DEFAULT_TYPE = 1;
	    static void doSomething(int type){
	        System.out.println("type = " + type);
	    }    

	    public static void main(String[] args) {
	        defaultProcess: doSomething (DEFAULT_TYPE);
	    }
	}

显示一个别名，这种情况应该用注释。

	public static int myMethod(){
	    http://www.google.com
	    return 1;
	}

这种纯粹是写来让别人看不懂的。但是却运行正常。
