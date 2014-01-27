title: Node.js企业开发: 五服务监控
date: 2014-01-27 17:19:04
tags:
---
服务监控对于正式环境应用和产品来说非常重要, 目前主流的语言平台都有完善的监控服务或解决方案, 虽然Node是一个年轻的平台, 但已经有不少成熟甚至商业化的监控服务.


# 云端监控服务
目前比较成熟的监控服务都是基于云端的. 使用时需要在应用部署地方安装统计client, 负责将系统的运行状态数据发送到云端, 由云端进行处理分析并生成报表.

## [StrongOps](http://strongloop.com/node-js-performance/strongops/)
StrongOps 是 StrongLoop 提供的 Node 应用监控服务, 其原型是 Nodefly, 后来开发团队及技术被 StrongLoop 收购. 并持续开发对外提供服务. 是目前比较成熟的监控服务(Strongloop是Node代码贡献最多的公司). 具有众多功能:

* 错误检测
* 调试
* CPU Profiling
* 内存 Profiling
* Transaction Profiling
* 内存泄露检测
* 性能监控
* Cluster 管理
* Endpoints & Databases

![](http://public1.qiniudn.com/img/StrongLoop%20%20%20StrongOps.png.resized.png)


## [NewRelic](http://newrelic.com/nodejs)
NewRelic是一家专门提供监控服务的公司, 他们最开始提供RUBY, PHP, JAVA, .NET, PYTHON 等监控服务, 最近添加了对Node.js服务监控的支持. 目前他们的监控服务提供多元的数据监控.

* Response time
* Apdex score
* Throughput (requests per minute)
* Web transactions
* Error rates
* Recent events
* Server information

具体特点可参看他们的介绍网站, 或直接体验他们的服务.


![](http://public1.qiniudn.com/img/Node.js%20Troubleshooting%20%20%20Performance%20Monitoring%20%20%20New%20Relic.png.resized.png)


## [Nodetime](http://nodetime.com/)
Nodetime 最初也是由个人开发团队开发来用于服务监控的, 现在已经被AppDynamics公司收购. 

![](http://public1.qiniudn.com/img/Nodetime%20%20%20Performance%20Analytics%20for%20Node.js%20Applications%20%20Node.js%20APM.png.resized.png)

## [concurix](http://www.concurix.com/home)
concurix 支持性能监控, 可视化的代码分析, 相对于上面三个服务它提供的服务更加有自己的特点, 可以查看事件循环等.

![](http://public1.qiniudn.com/img/Concurix%20%20%20Node.js%20Monitoring%20and%20Profiling.png.resized.png)


## 其他
[NodePing](http://nodeping.com/) 和 [Graphdat](http://www.graphdat.com/) 也是两家提供监控服务的公司， 有兴趣的同学
可以尝试一下.

# [监控 Module](https://nodejsmodules.org/tags/profiler)
除了这些云服务性质的监控服务外还有一些第三方Module 具有监控功能.

## [PM2](https://github.com/Unitech/pm2)
PM2 是一个进程守护工具， 现在也正在开发服务监控的功能， 目前还没有开发完成
![](https://github-camo.global.ssl.fastly.net/32145fda8087b4ff62ec952853052183d445af8c/687474703a2f2f6c65617066726f6775692e636f6d2f636f6e74726f6c66726f672f696d672f63662d6c61796f75742d312e706e67)

## [Node-Monitor](http://lorenwest.github.io/node-monitor/)
Node-monitor是一个监控module， 简单，轻量， 灵活，稳定。

## [WatchMen](https://github.com/iloire/WatchMen)
WatchMen是一个Node.js监控服务Module，用于监控接口状态， 使用Express， ejs开发，
具有web页面。

## [node-memwatch](https://github.com/lloyd/node-memwatch)
node-memcatch会监控内存使用情况，对于发现和查找内存泄露非常有用



# 总结
虽然Node.js是一个年轻的平台，但现在可供使用的监控服务已然不少， 大家可以根据自身特点
对多个服务或Module进行比较， 选择适合自己的工具

