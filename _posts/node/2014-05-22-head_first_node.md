---
layout: post
title: "深入浅出Nodejs 笔记"
tagline: "深入浅出Nodejs 笔记"
description: "badnotes,萬軍的个人网站，记录生活旅行代码。深入浅出Nodejs 笔记。"
category: Node
tags: Node Book
---
{% include JB/setup %}



##### 一、Node 简介

* 基于JavaScript
* 使用Chrome的V8作为JavaScript引擎
* 异步IO
* 事件回调
* 单线程
* 跨平台
* libuv实现跨平台，实现于 Windows 的 IOCP 和 *nix 的 libev

##### 二、模块机制
* CommonJS规范
* 可用C/C++扩展模块
* NPM管理包
* 前后端公用模块

##### 三、异步IO
* 改变用户体验
* 事件循环执行
* 观察者
* 执行回调

##### 四、异步编程
* 高阶函数
* 偏函数
* 异步编程解决方案：事件发布/订阅模式，Promise/Deferred模式，流程控制库
* 异步并发控制：bagpipe,async

##### 五、内存控制
* V8垃圾回收
* 内存限制:64位为1.4G,32位为0.7G
* 高效使用内存：作用域，闭包
* 内存指标：
	* 内存使用情况: process.memoryUsage()
	* 系统内存：os.totalmem()
* 内存泄漏：排查工具：node-headump,node-memwatch* 

##### 六、Buffer
* Buffer是一个像Array对象
* 内存分配：slab分配机制
* 大对象小对象分配不同
* Buffer拼接

##### 七、网络
* net模块构建tcp服务
* dgram模块构建udp服务
* http模块构建http服务

##### 八、Web应用
* 无框架构建：请求方法，路由解析，Cookie，Session，缓存，数据上传，中间件，页面渲染

##### 九、进程
* 复制进程，单机集群，Cluster模块

##### 十、测试
* 单元测试 断言库： assert
* 性能测试 benchmark基准测，ab压力测试

##### 十一、产品
* 编码，部署，性能，日志，监控
* 部署模块：forever