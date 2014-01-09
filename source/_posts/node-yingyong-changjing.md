title: Node.js 应用场景
date: 2014-01-06 21:40:29
tags: 应用场景
---
要想用Node.js首先需要知道它到底是什么, 有哪些优缺点. 然后我们才能知道到底 Node.js 适合哪些应用场景.

# Node.js

[维基百科](http://en.wikipedia.org/wiki/Nodejs)：“Node.js 是谷歌 V8 引擎、libuv平台抽象层 以及主体使用 Javscript 编写的核心库三者集合的一个包装外壳。” Node.js的作者瑞恩·达尔 (Ryan Dahl) 给了开发者一个使用事件驱动来实现异步开发的优秀解决方案。

Node.js 的主要思路是：使用非阻塞的，事件驱动的 I/O 操作来保持在处理跨平台 (across distributed devices) 数据密集型实时应用时的轻巧高效。

它的 Web 工作原理跟传统网络技术大不相同. 传统的网络服务技术，是每个新增一个连接（请求）便生成一个新的线程，这个新的线程会占用系统内存，最终会占掉所有的可用内存。而 Node.js 仅仅只运行在一个单线程中，使用非阻塞的异步 I/O 调用，所有连接都由该线程处理，在 libuv 的加分下，可以允许其支持数万并发连接（全部挂在该线程的事件循环中）。

当然这也有它自身的缺点: 大量的计算可能会使得 Node 的单线程暂时失去反应, 并导致所有的其他客户端的请求一直阻塞, 直到计算结束才恢复正常。 其次，开发人员需要非常小心，不要让一个 Exception 阻塞核心的事件循环，因为这将导致 Node.js 实例的终止.

Node.js 具有以下特点:

* 单线程
* 事件驱动, 非阻塞
* JS语言
* Google V8

所以它具有以下优点:

* 系统资源(内存)占用少, 高访问量时更明显
* 速度快(远快于php, python, ruby)
* 前后端使用JS, 统一开发语言, 学习成本低, 社区活跃, NPM发展异常快
* 善于处理高并发量的请求

同事也有他的缺点: 

* 单线程不健壮
* 平台较新, 不稳定
* 调试不方便, 回调嵌套代码难读
* Module太多, 质量不一

注：V8是谷歌开发的，目前公认最快的 Javascript 解析引擎，libuv 是一个开源的、为 Node 定制而生的跨平台的异步 IO 库。


# 应用场景

Node.js 适合解决特定问题, 在一些领域并不适合使用:

* CPU 密集型应用 
* Simple CRUD / HTML apps
* 数据库依赖复杂; 业务逻辑和验证复杂的应用
* 需要管理界面的应用
* 大型企业应用

Node.js适合于 IO 密集而非计算密集的情景；高并发微数据（比如账号系统）的情景; Node.js也适用于开发实时应用.

## RESTful API/ JSON API/ Mobile backend API
提供 RESTful API 的 Web 服务接收参数，解析，组合响应，并返回响应（通常是较少的文本）给用户。这是适合 Node 的理想情况，因为您可以构建它来处理数万条连接。它仍然不需要大量逻辑；它本质上只是从某个数据库中查找一些值并将它们组成一个响应。由于响应是少量文本，入站请求也是少量的文本，因此流量不高，一台机器甚至也可以处理最繁忙的公司的 API 需求。


## 实时程序
node.js另一个很大的方面是你可以很轻松的开发软实时系统。这是指那些像twitter，聊天软件，体彩或实时通讯网络的接口, 游戏.

## 单页的app
如果你打算写一个AJAX操作非常多的单页面app（比如gmail），node.js是非常合适的。在极短的响应时间内处理大量请求的能力，不同的客户端共享像确认信息之类的东西，这些都让node.js成为那种在客户端做很多处理的web程序的很好的选择。

## 流数据
传统的web程序讲http请求和响应作为元事件处理。可事实是它们是流，很多很酷的node.js程序正是利用这个优点创建的。最牛的例子是实时解析上传文件，还在不同的数据层建立了代理。

## 对unix工具的脚本化调用
node.js现在还很年轻，它正在试图为自己重新发明各种软件，不过更好的办法是深入到现有的广阔的命令行工具世界里。Node.js拥有产生数以千计的子进程的能力，同时可以把这些子进程的输出以流的方式处理，这让它成为那种和现有软件寻求平衡时的很好的选择

## 电子游戏统计数据
如果您在线玩过《使命召唤》这款游戏，当您查看游戏统计数据时，就会立即意识到一个问题：要生成那种级别的统计数据，必须跟踪海量信息。这样，如果有数百万玩家同时在线玩游戏，而且他们处于游戏中的不同位置，那么很快就会生成海量信息。Node 是这种场景的一种很好的解决方案，因为它能采集游戏生成的数据，对数据进行最少的合并，然后对数据进行排队，以便将它们写入数据库。使用整个服务器来跟踪玩家在游戏中发射了多少子弹看起来很愚蠢，如果您使用 Apache 这样的服务器，可能会 有一些有用的限制；但相反，如果您专门使用一个服务器来跟踪一个游戏的所有统计数据，就像使用运行 Node 的服务器所做的那样，那看起来似乎是一种明智之举。


# 总结
Node.js是一个年轻的平台, 不久就会迎来自己的成人礼(1.0大概会在2014年发布). Node有它自己的一些优点, 也有各种各样的问题. 但最让人欣慰的是越来越多的开发者加入进来, 促进Node.js不断完善, 应用到更多的领域当中. 即最重要的是它在不断的前进.

关于Node.js到底适合应用于何种场景, 只要看看业界对Node.js的采用情况就知道了. LinkedIn, WaltmartLab, Yahoo, ebay, Paypal... 而且这是最可靠, 最及时的.

[StrongLoop](http://strongloop.com/strongblog/) 的 In the loop 和 Mobile News Round-up会定期总结一些Node.js最新的应用情况, 另外 [Google news](https://www.google.com.hk/search?hl=en&gl=us&tbm=nws&authuser=0&q=nodejs&oq=nodejs&gs_l=news-cc.3..43j43i53.1468.2073.0.2456.6.4.0.2.0.1.148.288.2j2.4.0...0.0...1ac.1.BWenTbq3spg) 也是了解Node.js最新动态的好地方


### 参考

* [IBM--Node.js究竟是什么](http://www.ibm.com/developerworks/cn/opensource/os-nodejs/)
* [推酷--Node.js 优缺点及适用场景讨论](http://www.tuicool.com/articles/nAjYNf)
* [Felix Node.js guide--说服老板](http://nodeguide.com/convincing_the_boss.html)
* [说服老板中文翻译](http://cssor.com/suitable-for-the-scene-of-nodejs-use.html) 
* [知乎--使用 Node.js 的优势和劣势都有哪些？有大公司用吗？](http://www.zhihu.com/question/19653241)
* [知乎--Node.js 发展前景如何？适用于哪些场景？](http://www.zhihu.com/question/19587881)
* [segmentfault--实例说明为什么应使用 Node.js](http://segmentfault.com/a/1190000000375619)
* [stackoverflow--How to decide when to use NodeJS?](http://stackoverflow.com/questions/5062614/how-to-decide-when-to-use-nodejs)
* [Node.js is taking over the Enterprise – whether you like it or not](http://blog.appfog.com/node-js-is-taking-over-the-enterprise-whether-you-like-it-or-not/)
* [Infoq--廖恺谈NodeJS在淘宝的应用](http://www.infoq.com/cn/interviews/lk-nodejs-taobao)
* [NodeJS与Rails何去何从？](http://tech.it168.com/a2011/0914/1245/000001245990.shtml)


