Clojure 项目构建:理解Lein的机制
===

Leiningen 让我想起了很多关于钢之炼金术师亚历路易斯·阿姆斯特朗(Alex Louis Armstrong)：

* 他们都非常强壮
* 他们都很优雅
* 有时，你不知道他们究竟在说些什么(但是却让人吃惊)
* 小胡须
* 闪耀

通过阿姆斯特朗，我们可以记住Leiningen的神奇之处,以及帮我们更好的理解Leiningen. 在这篇文字里，我们将看到Leiningen的皮短裤下面的东西,有助于我们清晰的认识它的工作方式，和提升Clojure应用的开发，测试，运行及部署的效率。

这里我们将会学到:

* 不用Leiningen如何构建和运行一个Clojure程序
* 当你运行lein run时发生了什么
* Leiningen架构基础
* 当你运行一个Leiningen任务时发生了什么

在后期的文字中，可以学到:

* 如何在Leiningen中使用trampoline来避免浪费
* 如何用Leiningen管理依赖
* 如何发布一个完整的应用
* 如何发布一个库
* Leiningen可以做的其他东西






原文地址: http://www.flyingmachinestudios.com/programming/how-clojure-babies-are-made-lein-run/