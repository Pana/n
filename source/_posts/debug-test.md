title: Node.js企业开发: 三调试&测试
date: 2014-01-15 22:55:36
tags:
---
程序开发调试和测试是两个非常重要的环节, 在企业级应用开发中尤为重要.

## 调试

### console
[console](http://nodejs.org/api/console.html) 想必是大家最熟悉, 使用最多的调试方法了, Node 的 console 模块为内置原生模块, 包含了一些基本方法(log, error, info, warn, dir, time, trace, assert)将变量输出到 std 流中. 可以实现最基本的调试功能.

### debugger
[debugger](http://nodejs.org/api/debugger.html) 模块: V8 包含一个扩展调试器, 可以通过 TCP 协议在进程外访问, Node 包含一个内置的 debugger 客户端. 启动程序时添加 `debug` 参数可以启动.

```% node debug myscript.js```

该客户端没有实现所有功能, 但可以实现分步调试. 在代码中添加 `debugger;` 语句可以实现断点功能. 
还可以实现 watcher 功能, 具体使用方法可参看官方 API.

### node-inspector

[node-inspector](https://github.com/node-inspector/node-inspector) 是一个第三方模块, 是一个基于 Blink 开发工具的 Node.js 调试器. 通过 TCP 连接和 Node 程序通信, 实现调试功能. 跟浏览器中的 JS 调试工具有相同的功能, 通过该模块就能获的跟浏览器中相同的 JS 调试体验. 具体使用方法和步骤可以参看 [howtonode--Debugging with node-inspector](http://howtonode.org/debugging-with-node-inspector), [Taking Baby Steps with Node.js – Debugging with node-inspector](http://elegantcode.com/2011/01/14/taking-baby-steps-with-node-js-debugging-with-node-inspector/) 及 [node-inspector 百度经验](http://jingyan.baidu.com/article/dca1fa6fbd580ff1a44052de.html)

另外 [debug](https://github.com/visionmedia/debug) 是 TJ 大神开发的小型调试模块.



### 开发工具调试功能
大型的开发 IDE 都会集成良好的调试工具. 目前主要的 Node.js 大型 IDE 有 Webstorm, Nodeclipse, Visual Studio. 他们都具有良好的调试能力. 具体的配置和使用方法可参看以下文章.

* [使用神器webstorm调试nodejs](http://www.cnblogs.com/enix/archive/2012/04/29/2475983.html)
* [在eclipse中追踪nodejs的数据，调试nodejs](http://www.myexception.cn/javascript/666602.html)
* [eclipse 调试nodejs 发生Failed to connect to standalone V8 VM错误的解决方案](http://www.cnblogs.com/MrBackKom/archive/2012/06/11/2545684.html)
* [Sublime Text 2调试NodeJS最方便的方法](http://www.douban.com/note/310794563/)
* [joyent--Using Eclipse as Node Applications Debugger](https://github.com/joyent/node/wiki/using-eclipse-as-node-applications-debugger)  推荐
* [node.js application debugging in Visual Studio](http://typescript.codeplex.com/workitem/512)
* [Introducing node.js Tools for Visual Studio](http://www.hanselman.com/blog/IntroducingNodejsToolsForVisualStudio.aspx)
* [webstorm 文档](http://www.jetbrains.com/webstorm/webhelp/running-and-debugging-node-js.html)


### Joyent production practice
Joyent 是 Node 的东家, 在他们的服务中也大量的用到了 Node 技术, 并把它们的使用事件分享了出来, 其中有关于调试一些内容大家可以参考 [Joyent practice](http://www.joyent.com/developers/node/debug)

### 其他
supervisor, nodemon 等工具可以加快调试效率.

### 总结
也许 Node 调试起来没有 C, C++ 等语言更有效, 但可以使用的方法和工具也不少, 只要根据项目, 自身需求, 情况, 习惯使用即可.

## 测试
企业级应用或良好的模块, lib 都必须良好的测试, 甚至好多项目是由测试驱动如TDD, BDD. 这里要谈论的测试是由开发人员编写的单元测试, 或其他测试代码. 测试主要分为两类 功能测试(排除bug)和性能测试(查找性能瓶颈).

### assert
Node 提供了一个原生模块 `assert` 用于开发单元测试. 主要提供了数据判断的一些方法: fail, equal, notEqual ... 具体可参看[assert文档](http://nodejs.org/api/assert.html).

### [mocha](http://visionmedia.github.io/mocha/)
Mocha 是 TJ 大神开发的测试模块, 也是目前 Node 社区最有名, 使用最多的测试框架. 具有众多优秀特点:

* 支持浏览器, Node
* 支持异步, 同步测试
* 支持 TDD, BDD
* 支持多种形式测试结果查看方式
* 支持众多assertions: assert, should.js, chai, expect.js, better-assert
* 提供命令行工具, 可以结合 make 或 grunt 使用

关于具体的指导和文档可参看 [mocha](http://visionmedia.github.io/mocha/). 以及 Express, Koa 等知名模块的测试用例.

### 其他单元测试模块
除了 Mocha 还有许多测试框架, 比如 Isaac 的 tap, 还有[vows](http://vowsjs.org/). 详细列表可以访问[这里](https://nodejsmodules.org/tags/test)

### 性能测试
性能测试对于 Node 的主要应用场景 --- 高并发来说是非常重要的. 目前作者接触过 [wrk](https://github.com/wg/wrk). 由于对这块接触不多这里就不再详细介绍, 有兴趣的同学可以自己查找资料和技术.

另外 [intern](http://theintern.io/) 是一个全新的测试平台, 有兴趣的同学可以尝试下.


### 参考资料 	
* [webstorm unit test](https://www.jetbrains.com/webstorm/webhelp/unit-testing-node-js.html)
* [Insanely fast, headless full-stack testing using Node.js](http://zombie.labnotes.org/)
* [stackoverflow--unit test](http://stackoverflow.com/questions/7254025/node-js-unit-testing)


#### 其他参考

* [欲善其功，必先利其器--Nodejs调试技术总结](http://www.cnblogs.com/moonz-wu/archive/2012/01/15/2322120.html)
* [nodejs 开发调试工具](http://www.oschina.net/question/35932_29983)
* [elipse打造Nodejs的调试环境](http://blog.sina.com.cn/s/blog_72603eac0101567z.html)
* [stackoverflow--How to debug node.js applications](http://stackoverflow.com/questions/1911015/how-to-debug-node-js-applications)   推荐
* [nodejitsu.com -- How to debug a node application](http://docs.nodejitsu.com/articles/getting-started/how-to-debug-nodejs-applications)
* [Debug NodeJS Like A Pro](http://greenido.wordpress.com/2013/08/27/debug-nodejs-like-a-pro/)
* [Debugging node.js Projects](http://www.kevgriffin.com/debugging-node-js-projects/)
* [How to debug a Node.js application in Windows Azure Web Sites](http://www.windowsazure.com/en-us/documentation/articles/web-sites-nodejs-debug/)
* [cloud9--Running and Debugging Your Code](https://docs.c9.io/running_and_debugging_code.html)
* [slide--nodejs debuging](http://www.slideshare.net/NicholasMcClay/nodejs-debugging)
* [slide -- 单元测试实战](http://fengmk2.github.io/mk2blog/ppt/unittest-and-bdd-in-nodejs-with-mocha.deck.html)