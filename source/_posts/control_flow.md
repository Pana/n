title: Node.js流程控制
date: 2013-11-13 09:41:12
tags:
---
Node.js通过回调实现非阻塞从而达到超高性能, 但回调大大增加了流程控制的复杂度, 一直为广大JS程序员所诟病, 甚至将许多开发者挡在了大门之外. 虽然目前回调方式不可避免, 但有许多解决方案可以大大简化回调开发难度.

目前常见的解决方案可归为如下几类:

* Node官方使用方式: 回调作为最后一个参数传递, 回调的第一个参数为error
* promise
* ES6 generator
* 第三方module: async, step, wind.js, eventproxy

<!-- more -->

## Node官方回调方式
在早期的Node版本中曾经使用promise的回调方式, 后来被放弃了. 目前使用的方式为回调函数做为方法最后一个参数传递
, 回调函数的第一个参数为error, 正常则为null, 其他参数为传回的数据


## promise
promise是CommonJS提出的一种异步实现方式, 在CommonJS的[wiki](http://wiki.commonjs.org/wiki/Promises)中有对promise的详细解释.
微软现在win8 JS app SDK就使用了promise回调模式. 目前有多个promise库可供大家使用

* [Q](https://github.com/kriskowal/q) by Kris Kowal
* [Promised-IO](https://github.com/kriszyp/promised-io) by Kriz Zyp

关于promise的具体内容可参看howtonode 的一篇博客 [promise](http://howtonode.org/promises)以及infoq的[介绍](http://www.infoq.com/cn/news/2011/09/js-promise)


## ES6 generator
generator是ES6的一个新特性, yield调用代码块, 将会给回调带来方便

* [官方wiki](http://wiki.ecmascript.org/doku.php?id=harmony:generators)
* [Javascript's Future: Generators](http://jlongster.com/2012/10/05/javascript-yield.html)
* [whats the big deal with generators](http://devsmash.com/blog/whats-the-big-deal-with-generators)

## 第三方库
Node.js目前有很多流程控制的module

* [async](https://github.com/caolan/async)
* [step](https://github.com/creationix/step)
* [wind.js](http://windjs.org/cn/)
* [eventproxy](https://github.com/JacksonTian/eventproxy)

其中async module是npm依赖排行榜第二名, 仅落后于underscore, 个人也非常推荐使用该库, 具体使用方式可参看官方文档和[async使用实战](http://www.sebastianseilund.com/nodejs-async-in-practice)
这篇博客, 另外async使用named function 对tracking error有帮助.

## 常见的流程模式

* Series
* Fully parallel
* Limitedly parallel



参考:

* [howtonode -- promiese](http://howtonode.org/promises)
* [howtonode -- control flow](http://howtonode.org/control-flow)
* [howtonode -- control flow ii](http://howtonode.org/control-flow-part-ii)
* [Mixu's node book -- control flow](http://book.mixu.net/node/ch7.html)
* [流程控制模块](https://nodejsmodules.org/tags/flow)
* [async使用实战](http://www.sebastianseilund.com/nodejs-async-in-practice)
