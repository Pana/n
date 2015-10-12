title: Node 基金会那些事
date: 2015-10-11 19:08:15
tags:
banner: /assets/nodejs-foundation.png
---
最近 RedHat 刚刚加入了 Node.js 基金会, 这对 Node 来说又是一次不小的胜利. Node.js 基金会成立也有一段时间了, 并且刚刚完成了 Node 和 iojs 的合并工作, 今天我们就来聊聊这个基金会.

<!-- more -->

### 简介

[Node 基金会](https://nodejs.org/en/foundation/)是 Linux 基金会的 collaborative project, 也是在后者的帮助下建立起来的. 众所周知 Linux 基金会是一个非盈利性的联盟，其目的在于协调和推动Linux系统的发展，以及宣传、保护和规范Linux. 该组织具有多年丰富的开源项目运营经验. 

>The Node.js Foundation's mission is to enable widespread adoption and help accelerate development of Node.js and other related modules through an open governance model that encourages participation, technical contribution, and a framework for long term stewardship by an ecosystem invested in Node.js' success.

以上为该基金会的使命宗旨, 大意是以一种开放的管理方式促进 Node 的持续开发和应用.


### 历史

Node 基金会成立于 2015 年 2 月份, Joyent 宣布同 linux 基金会一道创立, 并将项目移交给该基金会, 并自动成为基金会的一名平台级成员. 基金会成立乃是无奈之举, node 0.12 的长期难产, 导致社区对Joyent的管理方式超级不满, 愤而创建了有名的 io.js 分支, 并迅速获得了大批支持者. 为避免社区长期分裂 Joyent 退步选择通过基金会的形式继续推动 Node 项目发展. 经过协商 io.js 在 5.18 号同意合并到 node 基金会下, 最终项目还保持Node.js 不变, 但项目将以 io.js 的 codebase 为主体. 代码合并工作随后展开. 与此同时基金会的创建工作也在同步进行. 具体的时间线可以[参看这里](https://nodejs.org/en/foundation/#timeline)

代码合并工作在 9.14 号完成, 并发布了版本 4.0, 同时还决定在 12.8-9 号在波特兰举办官方的开发者大会 [Node.js interactive](http://events.linuxfoundation.org/events/node-interactive)

### 成员
Node.js 基金会公司成员根据其所交年费, 及行使权力不同分为: 白金, 金牌, 银牌三种会员. 其中 Paypal, Microsoft, IBM, RedHat, Intel 等均为白金会员

![Platinum](/assets/nodejs-foundation-platinum.png)
![Gold](/assets/nodejs-foundation-gold.png)
![Silver](/assets/nodejs-foundation-silver.png)

技术委员会成员主要由核心开发者构成, 他们负责[管理](https://github.com/nodejs/node/blob/master/GOVERNANCE.md)整个项目, 当前委员会成员有:

* Alexis Campailla (orangemocha)
* Ben Noordhuis (bnoordhuis)
* Bert Belder (piscisaureus)
* Brian White (mscdex)
* Chris Dickinson (chrisdickinson)
* Colin Ihrig (cjihrig)
* Fedor Indutny (indutny)
* James M Snell (jasnell)
* Jeremiah Senkpiel (Fishrock123)
* Julien Gilli (misterdjules)
* Rod Vagg (rvagg)
* Shigeki Ohtsu (shigeki)
* Trevor Norris (trevnorris)


### 参考

* [Node 基金会](https://nodejs.org/en/foundation/)
* [oschina](http://www.oschina.net/news/59615/node-js-foundation)
* [Infoq](http://www.infoq.com/cn/news/2015/05/nodejs-iojs/)
* [Node.js基金会成立，Joyent交出领导权](http://www.infoq.com/cn/news/2015/02/nodejs-foundation-establish)