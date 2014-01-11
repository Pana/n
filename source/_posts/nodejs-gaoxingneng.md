title: Node.js企业开发: 二应用开发
date: 2014-01-10 17:18:46
tags:
---
当确定什么场合适合使用 Node.js 开发之后, 需要考虑的问题就是如何开发了. 这里介绍下 Node.js 程序开发需要注意的一些问题.

## 良好的程序应该具有的特点 
这里笼统介绍几个好程序的特点, 详细内容可以参考网上资料.

* 良好的架构
* 高性能
* 稳定和健壮
* 可扩展, 易维护
* 安全
* 完善的测试, 文档, 良好的编码风格和注释
* 其他: 易用, 美观等

## 架构设计
一般的小项目, 大多数的程序员都是没有这个概念和环节的, 但是对于中大型和企业应用开发, 架构设计是必须和非常重要的. 

因为我目前是一个 Web 开发者, 并没有做过架构设计的工作, 所以这里只是介绍下我所了解和想到的地方.

1. 首先需要了解需求, 了解项目特点. 需要实现什么功能, 程序使用特点, 访问量多大等等.
2. 根据需求选择合适的技术和工具, 预估项目的开发量, 需要多少开发者, 花费多长时间, 解析设计项目架构, 如何分配工作并协调, 设定开发进度等等.
3. 设计架构需要注意: 项目要模块化这样便于扩展,维护且让程序更加稳定; 问题, 工作量, 时间要预估充分; 

良好的架构是一切的基石, 只有架构好了才能保证高性能, 健壮, 可扩展, 提高开发速度, 降低成本.


## 高性能
Node 是一个高性能的 web 开发平台, 所以如何确保应用的高性能是一个非常重要的问题. 想要实现高性能可以根据 Node 的特点, 并参考大公司成熟的开发经验.

想必大家都知道 LinkedIn 公司分享的代码高性能经验分享. 下面将他们列出, 具体的原因和细节可参考[原文](http://engineering.linkedin.com/nodejs/blazing-fast-nodejs-10-performance-tips-linkedin-mobile).
这里面最重要的地方是--不要使用同步的代码, 异步是Node最大的特点, 也是它高效的原因, 所以在 Node 中一定不要使用同步代码, 除非你是写 demo, 本地测试.

* 避免同步代码
* 关闭socket池
* 不要使用 Node.js 处理静态资源
* 在客户端渲染页面
* 开启 gzip 压缩
* 并行
* 避免使用 session
* 使用 c++ 模块
* 使用标准 V8 JavaScript 而不是客户端库
* 保持小规模, 轻量级代码

除了这几点, 还有不少方法可以加快程序速度

* 使用 cluster 充分利用多核 CPU, 加快速度
* 更高效的 JS 代码写法. 好的 JS 代码可以更快, 占用更少的内存
* 使用性能检测工具查找程序瓶颈所在, 从而使程序变的更快

## 稳定, 健壮
Node 是单线程的, 任何没有捕捉的异常都会导致程序的结束和奔溃. 而程序稳定性是企业应用最看重的特性. 所以保证 Node 程序的稳定性是非常重要的.

目前提高 Node 程序稳定性的方法主要有: 异常处理, 进程守护, 程序监控

#### 异常处理

* 不要忽略错误, 所有的错误都要进行处理
* 使用 `try{...} catch(err){...}` 捕捉异常
* 使用 `domains` 特性
* 使用 `process.on(‘uncaughtException’, function(err){…}); ` 处理

#### 进程守护
* 使用 cluster 
* 使用 forever, pm2 等进程守护工具

#### 程序监控
* 添加服务监控, 异常时邮件, 短信等报警
* 记录错误日志.

## 其他

* 可扩展代码
* 测试
* 调试, 内存泄露



### 参考

* [linkedin 高性能总结](http://engineering.linkedin.com/nodejs/blazing-fast-nodejs-10-performance-tips-linkedin-mobile)
* [如何提高NodeJS程序运行的稳定性](http://blog.lovedan.cn/?p=222)
* [如何提高Nodejs程序的稳定性和健壮性](http://blog.lovedan.cn/?p=186)
* [Node.js实现网游服务器高性能和可扩展](http://developer.zdnet.com.cn/2012/1019/2126947.shtml)
* [7 tips for a Node.js padawan](https://medium.com/tech-talk/e7c0b0e5ce3c)