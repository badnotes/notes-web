---
layout: post
title: "网易Pomelo开发Socket服务器 "
tagline: "网易Pomelo开发Socket服务器 "
description: "badnotes,萬軍的个人网站，记录生活旅行代码。网易Pomelo开发Socket服务器 "
category: Socket
tags: Game Socket
---
{% include JB/setup %}



#### 1. 安装Pomelo并新建一个项目

    $ npm install -g pomelo
    $ mkdir pomelo-protobuf-socket-demo
    $ cd pomelo-protobuf-socket-demo
    $ pomelo init    # select 1.native socket


初始化时选择 1.native socket

将会自动生成如下的目录树:

    - pomelo-protobuf-socket-demo/
      - game-server/
        - app/
        - config/
        - logs/
          app.js
          package.json
      - shared/
      - web-server/
        - bin/
        - public
          app.js
          package.json
        npm-install.bat
        npm-install.sh

直接执行npm-install.*文件将安装相应的依赖，也可以分别进入game-server或者web-server目录下运行 ``npm install``，比如我们这里只是将其作为socket服务器,web-server不会用到，就可以将其删除。

#### 2. 自动生成的数据

* app子目录

这个目录下放置所有的游戏服务器代码的地方，用户在这里实现不同类型的服务器，添加对应的Handler，Remote等等。

* config子目录

game-server下config包括了游戏服务器的所有配置信息。配置信息以JSON文件的格式进行定义，包含有日志、master、server等服务器的配置信息。该目录还可以进行扩展，对数据库配置信息、地图信息和数值表等信息进行定义。总而言之，这里是放着所有游戏服务器相关的配置信息的地方。

* logs子目录

日志是项目中不可或缺的，可以对项目的运行情况进行很好的备份，也是系统运维的参考数据之一，logs存放了游戏服务器所有的日志信息。

查看下自动生成的game-server下的app.js

    var pomelo = require('pomelo');
    /**
     * Init app for client.
     */
    var app = pomelo.createApp();
    app.set('name', 'pomelo-protobuf-socket-demo');
    // app configuration
    app.configure('production|development', 'connector', function(){
      app.set('connectorConfig',
        {
          connector : pomelo.connectors.hybridconnector,
          heartbeat : 3,
          useDict : true,
          useProtobuf : true
        });
    });
    // start app
    app.start();
    process.on('uncaughtException', function (err) {
      console.error(' Caught exception: ' + err.stack);
    });

将会使用混合连接，也就是socket和websocket会自动适配,传输数据会使用protobuf压缩。


#### 3. 服务器操作

**启动**

$ cd game-server

$ pomelo start

**查看**

$ pomelo list

内容定义:

- serverId：服务器的serverId，同config配置表中的id。
- serverType：服务器的serverType，同config配置表中的type。
- pid：服务器对应的进程pid。
- headUsed：该服务器已经使用的堆大小（单位：兆）。
- uptime：该服务器启动时长（单位：分钟）。

**关闭**

$ cd game-server

$ pomelo stop

或者

$ cd game-server

$ pomelo kill

#### 4. 创建client


需要用到Node的net模块,pomelo的协议处理pomelo-protocol,以及数据压缩模块pomelo-protobuf

**客户端连接后并发送握手信息**

    var handshakeBuffer = {
        'sys': {type: 'socket', version: '0.0.1'},
        'user': {}
    };
    console.log('connect to ' + params.host + ":" + params.port);
    var socket = new net.Socket();
    socket.binaryType = 'arraybuffer';
    socket.connect(params.port, params.host, function(){
        console.log('Connected ... ');
        var obj = Package.encode(Package.TYPE_HANDSHAKE,
        Protocol.strencode(JSON.stringify(handshakeBuffer)));
        socket.write(obj);
    });

**监听接收数据,收到握手后返回握手成功**

    socket.on('data', function(data){
        var da = Package.decode(data);
        console.log("data type:"+da.type);
        if (da.type == Package.TYPE_HANDSHAKE){
            var obj = Package.encode(Package.TYPE_HANDSHAKE_ACK);
            socket.write(obj);
        }
    }


**收到心跳**

    socket.on('data', function(data){
        var da = Package.decode(data);
        console.log("data type:"+da.type);
        if (da.type == Package.TYPE_HEARTBEAT){
            // 定期发送心跳,另外发送或者每次收到后发送
            var hb = Package.encode(Package.TYPE_HEARTBEAT);
            socket.write(hb);
        }
    }


**收到数据**

    socket.on('data', function(data){
        var da = Package.decode(data);
        console.log("data type:"+da.type);
        if (da.type == Package.TYPE_DATA){
          // receive response data
        }
    }


**收到断开信息**

    socket.on('data', function(data){
        var da = Package.decode(data);
        console.log("data type:"+da.type);
        if (da.type == Package.TYPE_KICK){
          console.log("connection is closed by server...");
        }
    }


**发送protobuf压缩后的数据**

    var protos = Protobuf.parse(require('./Protos.json'));
    Protobuf.init({encoderProtos:protos, decoderProtos:protos});
    var user = Protobuf.encode('connector.entryHandler.entry', {name: 'name', password: 'pass'});
    var msg = Message.encode(1, Message.TYPE_REQUEST, 0, 'connector.entryHandler.entry', user);
    var pkg = Package.encode(Package.TYPE_DATA, msg);
    socket.write(pkg);

**监听错误信息**

    socket.on('error', function(data){
      console.log(data);
    });

**监听服务器端口**

    socket.on('close', function(){
      console.log('connection is close');
    });

#### 5. 服务器端会自动处理protobuf

config目录下的clientProtos.json为接收到的protobuf数据结构,serverProtos.json为服务器返回结构的数据结构，这样的话在服务端处理时会自动解包收到的protobuf数据和自动打包处理结果为protobuf.服务器端和客户端必须定义相同的protobuf数据结构才能识别处理数据。

#### 6. 移除心跳

为什么要移除心跳,一般用socket编程不都要处理心跳的么？为什么我们还要考虑把pomelo已经有的心跳给移除嗯？

这是因为在考虑客户端是移动应用，客户端在操作使用的时候，会有数据传输的，就没必要心跳，长时间未操作或者网络不稳定可能会断开，这个也不考虑了，就让他断开吧。也就是在操作的时候心跳是多余的，未操作的时候就不要让移动客户端长时间连着，一直心跳也费流量，未操作是也链接着也飞电。还不如让其断开，再次操作时从新连接。

将服务器代码hybridsocket.js第45行ST_INITED 改为 ST_WORKING，也就是初始状态并不是连接后等待握手，而是直接可以工作状态。

	this.state = ST_WORKING; // ST_INITED --> ST_WORKING

这样的话，客户端就直接连接然后发送数据，接收处理数据，其中的握手，握手确认，和心态什么的操作一律省略掉。














