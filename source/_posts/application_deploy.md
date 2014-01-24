title: Node.js企业开发: 三正式环境部署
date: 2014-01-25 10:24:15
tags:
---
Node 应用开发完成之后的工作就是应用部署了. 

## 测试环境, 预发布环境
企业级应用开发除了单元测试代码不可缺少之外, 通常还会部署多个环境保证最用服务的正确无误.
通常开发阶段会有`开发环境`, `调试环境`, 部署之前有`测试环境`, `预发布环境`, 以及`正式环境`又叫`产品环境`.

测试环境通常是给测试人员进行功能测试, 确保开发程序的所有功能是否Ok, 是否满足需求. 当在测试环境监测通过后应该就可以向正式环境部署了, 但测试环境的数据环境通常同正式环境会有很大差别, 所以很多企业还会有预发布环境用于排除数据或访问量带来的问题

## cluster
Node 单线程的架构没法充分利用当前服务器的多核 CPU 性能, 因此 Node.js 提供了原生的[cluster](http://nodejs.org/api/cluster.html)模块, 可以充分利用多核CPU的性能, 同时可以增加程序的稳定性.

cluster 使用例子: 

```
var cluster = require('cluster');
var http = require('http');
var numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
  // Fork workers.
  for (var i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  cluster.on('exit', function(worker, code, signal) {
    console.log('worker ' + worker.process.pid + ' died');
  });
} else {
  // Workers can share any TCP connection
  // In this case its a HTTP server
  http.createServer(function(req, res) {
    res.writeHead(200);
    res.end("hello world\n");
  }).listen(8000);
}
```


## &, nohup, forever, pm2
#### 后台执行
Node 服务进程通常需要在后台运行, 可以使用 linux 系统的后台命令, 也可以使用比较成熟的 module 使用.

```
$ nohup node app.js &
```
这样程序将会在后台执行.

#### 进程守护
正式环境一般都会对服务进程添加守护, Node 社区目前比较成熟的有 [forever](https://github.com/nodejitsu/forever) 和 [pm2](https://github.com/Unitech/pm2)

forever 是最早的进程守护工具, 可以以 CLI 或编码的形式使用.

全局安装forever

	$ npm install -g forever
	
使用 forever 启动服务

	$ forever start app.js
	
forever 还可以记录服务运行日志及错误日志, 具体方法可参看 [github](https://github.com/nodejitsu/forever)文档

`pm2` 是全新开发的进程守护服务, 同时集成了负载均衡功能. 以及开机启动, 自动重启有问题进程. [pm2](http://signup.pm2.io/) 还可以查看各服务进程状态. 国外许多开发者认为 [pm2 比 forever 更优](http://devo.ps/blog/2013/06/26/goodbye-node-forever-hello-pm2.html)

pm2 命令方法与 forever 类似, 首先需要全局安装:

	$ npm install pm2@latest -g
	
然后使用 pm2 启动服务

	$ pm2 start app.js -i 4
	$ pm2 monit    #监控服务
	
关于详细使用方法可以参看 [官方文档](https://github.com/Unitech/pm2)

## nginx
nginx 是俄罗斯程序员开发的全新高性能web server, 其运行原理同 node 类似, 也是基于事件的机制, 但它是一个server 软件, 功能与 Apache 类似, 最近几年许多大型网站开始使用 nginx. Node.js 本身并不适合服务静态文件, 所以可以使用nginx同 node.js 结合使用, Node.js 处理业务逻辑, nginx 服务静态文件.

```
server {
	listen 80;
    server_name domain.serve.com;
    access_log  /var/log/logfile.log main;
    root /opt/application_path;

    location / {
    	proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://127.0.0.1:3000;
    }

    location ~* \.(jpg|jpeg|gif|css|png|js|ico)$ {
    	access_log off;
        expires 7d;
    }
}
```
以上为最简单的nginx配置, 他会将具体的业务需求转给 node 处理. 还有其他配置方式和更加详细的优化可以参看网络资料.

* [为高负载网络优化Nginx和Node.js](http://developer.51cto.com/art/201301/378571.htm)
* [Node.js与Nginx](http://ittechnical.sinaapp.com/node-js-and-nginx/)
* [node.js 与 nginx配合使用](http://hi.baidu.com/guoxiaoming/item/1074554ab504feaddf2a9fb3)
* [Node.js部署最佳实践](http://www.douban.com/note/265207425/)

## 日志
web 服务记录访问和错误日志是必要工作, 最常使用的日志记录方式为文件日志(也可以使用数据库存储日志). 在 Node 中记录日志方式可以有多种:

* Nginx 或 Apache
* forever, pm2
* 日志module: log4js, winston

如 `log4js` 记录日志方法:

```
var log4js = require('log4js'); 
//console log is loaded by default, so you won't normally need to do this
//log4js.loadAppender('console');
log4js.loadAppender('file');
//log4js.addAppender(log4js.appenders.console());
log4js.addAppender(log4js.appenders.file('logs/cheese.log'), 'cheese');

var logger = log4js.getLogger('cheese');
logger.setLevel('ERROR');

logger.trace('Entering cheese testing');
logger.debug('Got cheese.');
logger.info('Cheese is Gouda.');
logger.warn('Cheese is quite smelly.');
logger.error('Cheese is too ripe!');
logger.fatal('Cheese was breeding ground for listeria.');
```

日志对于访问统计和错误处理非常有用, 正式环境的日志记录有许多规范方法和经验(如日志分离)具体可以到网上查找.


## 提醒
正式服务通常都会有提醒服务, 当服务down掉, 或者有错误时会向运维人员或维护人员发送提醒服务, 通常的提醒方式有: 邮件, IM, 短信等. 具体实现方法可以自己研究查看.




### 参考

* [how-to-deploy-node-to-production](http://www.slideshare.net/embwbam/how-to-deploy-node-to-production)
* [stackoverflow-Node.js Production Deployment](http://stackoverflow.com/questions/15971156/node-js-production-deployment)
* [stackoverflow-Deploying a production Node.js server](http://stackoverflow.com/questions/8386455/deploying-a-production-node-js-server)
* [DEPLOYING NODE.JS TO PRODUCTION](http://codeplease.wordpress.com/2013/09/27/deploying-node-js-production/)
* [HARDENING NODE.JS FOR PRODUCTION: A PROCESS SUPERVISOR](http://blog.argteam.com/coding/hardening-nodejs-production-process-supervisor/)
* [book-How to Deploy a Node.js App to Production](http://fluentconf.com/fluent2012/public/schedule/detail/24643)
* [Joyent-Production Practices-deploy](http://www.joyent.com/developers/node/deploy)
* [Node.js deployment in production settings](http://ngo-hung.com/blog/2012/07/14/node-js-deployment-in-production-settings)
* [Setup node.js Servers Within a Load-balanced Configuration](http://shawn.dahlen.me/blog/2013/03/18/setup-node-dot-js-servers-within-a-load-balanced-configuration/)
* [Nodejs Production Deployment](http://cthayer.wordpress.com/2013/11/05/nodejs-production-deployment/)
* [https://node.ci](https://node.ci/)