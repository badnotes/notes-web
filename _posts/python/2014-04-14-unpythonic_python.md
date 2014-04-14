---
layout: post
title: "非Python化的Python(译)"
tagline: "非Python化的Python(译)"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。非Python化的Python(译)."
category: Python
tags: [Python]
---
{% include JB/setup %}


### FizzBuzz

当我供职于 [Hacker School](http://www.hackerschool.com/) 时，其中有个应用是 FizzBuzz。

	写一个程序输出1到100(含).如果数字可以被3整除，输出Fizz代替，如果能被5整除，输出Buzz,如果能被3和5整除，输出FizzBuzz.你可以用任何语言完成.

(Hacker School 已经略有更新这个问题,可能会使它更难以通过谷歌来解决。我故意放出了更新的版本，使我在Google搜索上的影响降到最低。)

这个问题是相当直接且很好的问题用来展示不同的语言和编程风格的列子;它就像 "Hello, World!" 和 斐波纳契数列问题一样。

### 一些非Python化的Python

今天我看了下这个问题然后展示给我在 Hacker School 的朋友看，开始思考一些仅仅用 Python 来处理这个问题。Python,特别是与PEP 8, 用理想的方法来写 Python 化的代码。但是 Python 并没有强制必须用 Python 化的代码。所以我开始思考，处理这个问题我能用到的其他种类的Python。

警告:非常的非Python化。不过所有的代码到在Python 2.7.5下正常工作。

你可以随意tweet我或者提供建议或新处理方式。

Pythonic Python

	def fizzbuzz(number):
	    if number % 3 == 0 and number % 5 == 0:
	        return 'FizzBuzz'
	    elif number % 3 == 0:
	        return 'Fizz'
	    elif number % 5 == 0:
	        return 'Buzz'
	    else:
	        return number

	for number in range(1, 101):
	    print fizzbuzz(number)

Lispy Python

	fizzbuzz = lambda n: 'FizzBuzz' if n % 3 == 0 and n % 5 == 0 else None
	fizz = lambda n: 'Fizz' if n % 3 == 0 else None
	buzz = lambda n: 'Buzz' if n % 5 == 0 else None
	fizz_andor_maybenot_buzz = lambda n: fizzbuzz(n) or fizz(n) or buzz(n) or str(n)

	print reduce(lambda m,n: m+'\n'+n, map(fizz_andor_maybenot_buzz, range(1, 101)))

Javacious Python

	import sys

	class Value(object):
	    def __init__(self,value):
	        self.setValue(value)

	    def setValue(self,value):
	        self.value = value

	    def getValue(self):
	        return self.value

	    def toString(self):
	        return self.getValue().__str__()

	class FizzBuzz(object):
	    def __init__(self, n):
	        if n % 15 == 0:
	            value = 'FizzBuzz';
	        elif n % 3 == 0:
	            value = 'Fizz';
	        elif n % 5 == 0:
	            value = 'Buzz';
	        else:
	            value = str(n);
	        self.setValue(value);

	    def setValue(self,value):
	        self.value = Value(value);

	    def getValue(self):
	        return self.value;

	class FizzBuzzRunner(object):
	    def __init__(self, n):
	        self.setN(n)

	    def setN(self, n):
	        self.n = n

	    def run(self):
	        for i in range(1,self.n):
	            sys.stdout.write(FizzBuzz(i).getValue().toString()+'\n');

	if __name__ == '__main__':
	    n = 101;
	    FizzBuzzRunner(n).run()

C-ly Python

	def main():
	    i = 0;
	    value = '';

	    while i < 100:
	        i += 1
	        if i % 15 == 0:
	            value = 'FizzBuzz';
	        elif i % 3 == 0:
	            value = 'Fizz';
	        elif i % 5 == 0:
	            value = 'Buzz';
	        else:
	            value = str(i);
	        print value;

	    return 0;

	main();


Clojurly Python

	def fizzbuzz(n):
	    return 'FizzBuzz' if n % 3 == 0 and n % 5 == 0 else None

	def fizz(n):
	    return 'Fizz' if n % 3 == 0 else None

	def buzz(n):
	    return 'Buzz' if n % 5 == 0 else None

	def fizz_andor_maybenot_buzz(n):
	    print fizzbuzz(n) or fizz(n) or buzz(n) or str(n)

	map(fizz_andor_maybenot_buzz, xrange(1, 101))


原文地址:http://skien.cc/blog/2014/04/09/unpythonic-python/