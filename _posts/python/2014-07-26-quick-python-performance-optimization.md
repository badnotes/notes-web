---
layout: post
title: "如何让Python更快的运行"
tagline: "如何让Python更快的运行"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。如何让Python更快的运行"
category: Python
tags: [Python]
---
{% include JB/setup %}

Python可能是最容易实现你的想法的语言,但很难构建出最优的代码。尽管强烈反对过早优化，但对代码做很小的改进就可以获得极大的提升性能，当然应该及时处理了。

我们在日常使用python编程做一些简单的事情是非常合适的,但是一些小程序如果不是有意去优化可能就不会去改善了，所以在写这样的小东西时就应该尽量的把性能考虑在内。

1. 在 Ipython 的交互shell下可以在每行前面加上 %timeit 或者 %prun(cProfile)

 可以跟踪你的代码运行情况，找出瓶颈出现在哪里。这只是初步优化,而并非深入优化整个系统。

	- 深入优化python性能,你应该看看 [python性能分析](http://www.huyng.com/posts/python-performance-analysis/)
	- 另一个有趣的库 [line_profiler](https://bitbucket.org/robertkern/line_profiler)，可以逐行检测性能

2. 减少函数调用的次数

 如果你需要操作一个列表,你应该将整个列表传入函数，而不是遍历列表时把每个元素传递给函数并返回结果。

3. 使用 xrange 而不是 range (在python2.x中这样做，因为3.x默认就是这样的)

 xrange是用C实现的,有利于更高效的使用内存

4. 数据量大时，使用 numpy, 比标准的数据结构更好

5. "".join(string) 优于 + 或者 +=

6. while 1 比 while True 更快

7. list 解析 > for loop > while loop

 使用列表解析会快于其他的loop循环，while使用了外部计算器，以至于它非常的慢。

8. 使用 cProfile, cStringIO 和 cPickle

 始终使用合适的C版本的模块

9. 使用局部变量

 局部变量快于全局变量,内置变量或属性查找

10. 迭代器优于列表循环 - 迭代器能够有效使用内存并很好扩展, 可使用 itertools 模块

 尽量创建生成器和yeild结合使用(这不是闭包么),这比用通常的list来处理更快。

11. 尽可能的在任何地方使用 Map,Reduce和Filter而不是for循环.

12. 判断a是否属于b,dict或set 比list或tuple好

13. 处理大量数据时，尽可能的用不变的数据类型，这会快于tuple，更比list快

 因为可变的数据类型容量不足时，会重新分配更多的内存。

14. 插入数据到list的复杂度是 O(n)

15. 如果要在list的两端处理数据,应该用deque

16. del - 删除使用后的对象
    - 虽然python会自己做这事，但这需要额外的垃圾回收模块处理或者
    - 写一个 ```__del__``` magic 或者
    - 最简单的方法就是用后删除

17. time.clock()

18. GIL(http://wiki.python.org/moin/GlobalInterpreterLock), GIL是一个守护程序

 GIL要求每个进程里只有一个线程，防止CPU处理大量的线程切换。可以使用C类型或者本地C库来绕过这个限制，当你无法再优化Python代码的时候，你总是可以用C重写糟糕缓慢的函数，或者通过Python直接调用C程序。其他的像gevent这样的库也可以改善性能。

 TL,DR: 你在写程序的时候，不仅要考虑数据结构，迭代结构，如果有需要还要用C构建扩展来绕过GIL的限制。

 其实很多库都已经做了突破GIL限制了的，这意味着你在构建多线程的程序时，你是可以直接使用标准库的(Pool).

来自 [quick-python-performance-optimization](http://infiniteloop.in/blog/quick-python-performance-optimization-part-ii/)