title: Hello World
date: 2013-09-22 12:19:44
tags: HelloWorld
---
几乎所有语言和平台都有自己的Hello World. 这里介绍如何运行N的Hello World.

# Simple Version


在终端输入`node`回车打开N的[REPL](http://nodejs.org/api/repl.html), 输入以下代码:
  
    console.log('Hello World!')

回车即可看到输出结果: "Hello World".

还可将以上代码保存成文件 `example.js`, 然后执行`node example.js`, 可看到同样的结果.


# HTTP&TCP Version
N最频繁的使用场合是Web或TCP Server开发

### HTTP Server
返回 Hello World 的 Web Server.
```
var http = require('http');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, '127.0.0.1');

console.log('Server running at http://127.0.0.1:1337/');
```

执行`node example.js`启动服务, 使用浏览器访问 http://127.0.0.1:1337 

### TCP server
将收到的请求内容原样返回的 TCP Server
```
var net = require('net');

var server = net.createServer(function (socket) {
  socket.write('Echo server\r\n');
  socket.pipe(socket);
});

server.listen(1337, '127.0.0.1');
```

执行`node example.js`启动服务, 然后TCP连接到 127.0.0.1:1337, 即可看到效果.

# So Simple
N的Hello World就是这么Simple. N能够实现的功能远远不止这些, 想要体会N的精彩之处, 赶快学习N的更多内容吧.


