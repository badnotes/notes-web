---
layout: mypost
title: "Scala第一步"
tagline: "Scala第一步"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。Scala第一步."
category: Scala
tags: [Scala]
---



学习Scala最好的方式需要依赖你已经知道的知识和你学习东西的方法。因为Scala揉杂了各种编程语言的特性，比如Ruby的操作符也是函数，特质，还有Erlang的模式匹配，消息传递。所以说, Scala确实比较复杂。

你可以通过阅读这些[书籍](http://scala-lang.org/documentation/books.html)来自学。很多人都是找一本合适的书，然后一边看书一边在SBT里面测试各种小程序学习的。也可以参加网上的培训课然后自己动手实践。

随着你的Scala知识的增长,你会发现一些更好的资料和一个非常友好的Scala社区可以帮助你学习。社区成员都对Scala满心热情并且对新人非常友好。他们还写了一些对于Scala初学者很有用的材料。有问题也可以发邮件向他们请教。他们也会在论坛或者自己的博客上分享一些新技术、先进的观念或者各种工具。

##### 你知道吗

如果你刚开始学习Scala的话，你会发现大部分的Scala学习资料都假定你已经有一定的编程经验。所有不建议将Scala作为第一门程序语言来学习。如果你是编程新手，却想学习Scala,强烈推荐你学习这两个资源：

*  在线课程 [Functional Programming Principles in Scala](https://www.coursera.org/course/progfun),在coursera上可以学习。由Scala的创建者Martin Odersky讲授。就像是大学老师在讲授函数式编程原理一样，通过完成编程作业你将学到很多Scala知识。
*  [Kojo](http://www.kogics.net/sf:kojo) 是一个交互式的学习环境，你可以通过Scala编程来探索数学、艺术、音乐、动画和游戏。

##### 第一行代码

###### Hello World
作为第一个示例，我们用"Hello World"程序来演示最简单的Scala代码。

    object HelloWorld {
      def main(args: Array[String]) {
        println("Hello, world!")
      }
    }

如果你是Java程序员，看到这个结构有没有觉得似曾相识：一个Main方法输出HelloWorld到标准终端。

下载安装[Scala](http://scala-lang.org/download)并设置环境变量.

| 环境   | 变量   | 值 |
|--------|--------|
|  Unix  | $SCALA_HOME | /usr/local/share/scala
|        | $PATH       | $PATH:$SCALA_HOME/bin
|Windows | %SCALA_HOME%| c:\Progra~1\Scala
|        | %PATH%      | %PATH%;%SCALA_HOME%\bin

###### 交互式环境下运行
输入 ```scala``` 命令即可进入Scala交互式环境，并可运行Scala表达式。

    > scala
    This is a Scala shell.
    Type in expressions to have them evaluated.
    Type :help for more information.
    scala> object HelloWorld {
        |   def main(args: Array[String]) {
        |     println("Hello, world!")
        |   }
        | }
    defined module HelloWorld
    scala> HelloWorld.main(null)
    Hello, world!
    scala>:q
    >

输入 :q 或者 :quit 回车后即退出交互式环境。

###### 编译
```scalac``` 命令可以编译一个或多个Scala源代码文件，并且生成可以在标准JVM虚拟机上执行的Java字节码文件。Scala编译器就像javac一样工作，Java编译需要依赖[Java SDK](http://java.sun.com/javase/).

    > scalac HelloWorld.scala

执行scalac默认将会在当前目录生成class文件，你可以用 ```-d``` 参数指定其他输入目录

    > scalac -d classes HelloWorld.scala

###### 执行
```scala```命令可以执行生成的字节码文件

    > scala HelloWorld

你可以用特殊的参数来执行```scala```程序,如 ```-classpath``` (-cp)

    > scala -cp classes HelloWorld

scala 要执行的程序必须是一个顶级对象，如果它扩展自App特质，则对象里所有的声明都将会被执行，否则你必须添加一个 main 方法来作为你程序的入口。

    object HelloWorld extends App {
      println("Hello, world!")
    }

###### 脚本
你也可以将这段程序作为Shell脚本运行。

下面这段就是bash的Shell脚本文件script.sh

    #!/bin/sh
    exec scala "$0" "$@"
    !#
    object HelloWorld extends App {
      println("Hello, world!")
    }
    HelloWorld.main(args)

你可以在当前目录下执行它：

    > ./script.sh

注意：我们假定script.sh可以在PATH环境变量里执行或搜索到scala.