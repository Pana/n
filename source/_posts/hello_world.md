title: N Hello World
date: 2013-09-22 12:19:44
tags: HelloWorld
---
送上大家最熟悉N的Hello World

# REPL
REPL(Read Eval Print Loop)是N提供的交互式执行方式, 跟浏览器调试工具的console基本相同. 用户每输入一段代码REPL会即时执行打印结果.
同Ruby的Irb, 和 Python一样, 提供同样的服务. 通常用于调试, 测试, 实验运行结果. REPL同样支持以代码的形式集成到其他应用. [具体参看](http://nodejs.org/api/repl.html)

REPL打开方式(终端):
    
    node

之后可以输入js代码测试:
    
    console.log('Hello World!')

可在REPL输入任何JS代码,进行尝试. 之后会详细介绍REPL.

# node filename.js
跟其他语言一样, 最常用的执行方式是将代码放入文件, 然后用响应语言环境执行代码. N的文件就是JS文件, 以.js结尾即可.
执行方式为 `node filename.js`, 关于`node`的详细使用方式, 可使用`node -h`查看

## Hello World
将以下代码保存成文件hello_world.js, 用node命令执行即可看到终端输出Hello World!!.

    console.log('Hello World!!')

## HTTP Server
对所有请求响应 Hello World 的 Web Server.
```
    var http = require('http');
    http.createServer(function (req, res) {
      res.writeHead(200, {'Content-Type': 'text/plain'});
      res.end('Hello World\n');
    }).listen(1337, '127.0.0.1');
    console.log('Server running at http://127.0.0.1:1337/');
```

```
    node example.js
    server running at http://127.0.0.1:1337/
```

## TCP server
将发送的内容原样返回的TCP Server
```
    var net = require('net');

    var server = net.createServer(function (socket) {
      socket.write('Echo server\r\n');
      socket.pipe(socket);
    });

    server.listen(1337, '127.0.0.1');
```

# 总结
以上为N的Hello World, 已及N最主要的使用场景的简单代码, 通过简单的几句代码就能搭建服务器, 完全不用Apache, IIS等服务器.
怎么样N是不是很有趣, 那就继续学习N的相关内容吧.


