title: Node.js--Evented I/O for V8 javascript
date: 2013-09-11 13:14:21
tags: Intro
---

## What is Node.js
Node.js是基于V8(Chrome开源JS引擎)和libuv的后端JS运行平台.

官方描述:

    Node.js is a platform built on Chrome's JavaScript runtime 
    for easily building fast, scalable network applications.
    Node.js uses an event-driven, non-blocking I/O model 
    that makes it lightweight and efficient, perfect for 
    data-intensive real-time applications that run across distributed devices.

中文翻译:

    Node.js是基于Chrome JS引擎(V8)的平台. 能用于开发速度快,可扩展的网络应用. 
    它使用事件驱动、无阻塞模型, 从而具有轻量,高效的特性, 非常适合于开发运行在分布式设备上的
    数据密集,实时型应用.

## Node.js组成&原理

### 组成

* Node.js底层核心使用v8和libuv实现
* 其上是C++和JS开发的Native Module
* 再往上是npm和众多的第三方模块

### 原理



## Node.js优缺点
### 优点
* 开源
* 高性能
* 前后端可使用一种语言完成
* 庞大的第三方模块
* 活跃的开发者社区

### 缺点
* 不够成熟, 还未到1.0
* 回调开发模式理解, 调试复杂
* 第三方模块过多, 导致选择麻烦

## Node.js历史,发展现状
发展历史:

* 2009.2，Ryan Dahl在博客上宣布准备基于V8创建一个轻量级的Web服务器并提供一套库
* 2009.5，Ryan Dahl在GitHub上发布了最初版本的部分Node.js包，随后几个月里，有人开始使用Node.js开发应用
* 2009.11和2010.4，两届JSConf大会都安排了Node.js的讲座
* 2010年年底，Node.js获得云计算服务商Joyent资助，创始人Ryan Dahl加入Joyent全职负责Node.js的发展
* 2011.7，Node.js在微软的支持下发布Windows版本
* 2012.12.22，Luvit 0.6.0 发布，Lua 实现的 Node.js
* 2013.9.4 v0.10.18发布

现状(2013.9.22):

* 有将近4万2千个第三方Module, 且还在快速增加当中
* 许多大云服务都已支持Node.js, Amazon, Azure, baidu, alibaba, heroku, [nodejitsu](https://www.nodejitsu.com/)
* Node.js性能还在不断提升
* Node.js在2014年初即可实现1.0
* Node.js有庞大的社区


## 作者, 维护人员, 公司

* Node.js 由 Ryan Dahl在2009年开发, 他是一个资深C++开发者
* 目前主要由 Isaac Z. Schlueter(npm作者)开发和维护
* Node.js是joyent(高性能云服务提供商)的产品


## 相关博客和链接

* Node.js [wiki](http://en.wikipedia.org/wiki/Nodejs) 和 [baike](http://baike.baidu.com/view/3974030.htm)
* Node.js [GitHub](https://github.com/joyent/node)
* [MIS Class Blog Node.js](http://misclassblog.com/interactive-web-development/node-js/)
* [Node.js的核心与红利-朴灵(田永强)](http://www.programmer.com.cn/13844/)

