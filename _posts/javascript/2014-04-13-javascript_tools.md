---
layout: post
title: "javascript基础工具清单(译)"
tagline: "javascript基础工具清单(译)"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。javascript基础工具清单。"
category: JavaScript
tags: JavaScript Tools
---
{% include JB/setup %}


在我们的集训营里的同学认识了几个工具和类库来扩展我们自己的代码后，我们的javascript学员Kalina,完成了这么一个工具清单，准备贡献给其他的coder.

Ivan Storck，我们训练营的教导员，用Kalina的清单绘制了这张思维导图：

![Javascript 工具思维导图](/static/images/JavaScript_tools.jpg)

## 通用

#### 脚手架工具

**Yeoman**
Yeoman 是一个强健的并且单一的客户端栈，组成的工具和框架，可以 帮助开发者快速创建漂亮的Web应用.

#### 构建工具(自动构建)

**Grunt.js**
Grunt是一个庞大的生态系统并且每天都在增长，有差不多上百个插件，你可以用Grunt很容易的完成任何事.

* **Pint.js** (Grunt 助手) - Pint是一个小巧的，异步的，专业的Grunt集成环境,试图解决自定义的协同构建。

**Gulp.js**
Gulp 用数据流和惯例优先配置使得构建跟简单和直观。

**Browserify.js**
(用于浏览器) Browserify 是个开发者工具，使我们可以写node.js风格的模块编译后在浏览器中运行，就像node一样，我们将模块分离在各个文件去，用module.exports 和 exports 变量，来导出内部的方法和属性。

**Uglify.js**
Uglify.js 是NodeJS的一个JavaScript解析/混淆/压缩/格式化库。

## 前端

#### MVC框架

**Backbone.js**
Backbone.js 通过对模型提供键值对绑定和自定义事件来构造Web应用，集成了富API和原始方法，和直观的事件绑定，它可以链接到所有的RESTful JSON API接口。

**Ember.js**
Embar 通过在底层模型改变时确保你的HTML同时更新来使得的你的Handlebars模版更好。开始时，你不需要写任何的JavaScript代码。

**Angular.js**
Angular让你可以为你的应用扩展HTML标签,生产环境非常的专业，真实，并且开发快速。

#### 模版

**Handlebars.js**
Handlebars提供强劲的动力很轻易的高效构建语义模版。Mustache模版和Handlebars模版兼容，所以你可以写Mustache模版导入到Handlebars,开始使用先进的额外的Handlebars特征。

**Mustache.js**
(Handlebars的精简版) Mustache是一个适用于 ActionScript, C++, Clojure, CoffeeScript, ColdFusion, D, Erlang, Fantom, Go, Java, JavaScript, Lua, .NET, Objective-C, Pharo, Perl, PHP, Python, Ruby, Scala 和 XQuery 的简单的Web模版系统。

**Jade**
Jade是一个node模版引擎，主要用于node.js的服务端模版。

**Haml-js**
Haml-js允许在JavaScript工程用 Haml 语法。它有原始Haml的大部分功能。

**Eco**
Eco让你嵌入CoffeeScript逻辑标记。

#### 测试

**Casper.js**
CasperJS是用Javascript写的一个PhamtomJS和SlimerJS的导航脚本和测试工具。

**Zombie.js**
Zombie.js是一个在模拟环境下测试客户端Javascript代码的轻量框架。并不需要浏览器支持。

## 后端

#### 服务端

**Express**
Express是一个Node 的Web 应用框架。

**Node**
Nodejs 是一个基于Chorme的运行环境的平台，可以很容易的构建快速可以扩展的网络应用。

#### 数据库

**MongoDB**
MongoDB 是一个开源的文档数据库，是NoSQL数据的龙头老大。

**Postgresql**
PostgreSQL是一个强大的开源的对象数据库系统。

**SQL**
SQL是用来和数据库痛心的，根据美国国家标准协会，它是关系型数据库系统的标准语言。

#### 架构模式

**RESTful**
具象状态传输(Representational State Transfer)是一种有协同体系结构组成的架构风格，用在分布式的超媒体系统的组件，连接器和数据元素。

#### 测试

**Cucumber.js**
Cucumber.js 是一个用在JavaScript上的流行的行为驱动开发工具。

**Jasmine**
Jasmine 是一个JavaScript行为驱动开发测试框架，它并不依赖与浏览器，DOM,或其他任何JavaScript 框架。这比较试用于网站，Node.js工程，或者其他JavaScript可以运行的地方。

**Mocha**
Mocha是个运行在node.js和浏览器上的多功能JavaScript测试框架。使得异步测试简单有趣。

**Q-Unit**
Q-Unit是个强大的易用的JavaScript单元测试框架。jQuery,jQuery UI 和 jQuery Mobile 都在用它。他其实可以测试任何的一般JavaScript 代码。

#### 断言库

**Chai**
Chai是个用于node和浏览器的BDD/TDD断言库，它可以很好的任何JavaScript测试框架配合使用。

#### 函数式编程工具

**Underscore.js**
Underscore 是个不用嵌入任何对象就可以提供函数式编程助手的JavaScript库。

**Lo-Dash**
Lo-Dash 是一个提供一致性、定制和性能实用程序库。


#### 更新：
如果你认为某个工具应该加入到这个清单？签出这篇文章和绘制的OPML思维导图，提交一个请求或发送一个建议来增加新的或者流行的工具。

原文地址:[https://www.codefellows.org/blogs/complete-list-of-javascript-tools](https://www.codefellows.org/blogs/complete-list-of-javascript-tools)