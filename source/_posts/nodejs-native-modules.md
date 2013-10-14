title: Nodejs native modules  
date: 2013-09-17 16:46:02  
tags:  Native Module
---
Node.js目前的稳定版本为0.10, 很快会升级到0.12, 之后将是N粉翘首以待的1.0. 虽然目前还没有到达1.0
但原生API已基本固定, 不会有很大变化. 想学好Node.js, 原生模块(API)是必须熟练知道的. 这里N将开始
第一个博客系列--Native Module. 主要介绍Node.js的原生模块 并会涉及Node.js的内部架构和原理.

Node.js官方提供了英文原版的[API](http://nodejs.org/api/)文档. 可以一个页面查看所有模块, 也可以单模块查看,
还提供了JSON版本的文档. 国内社区和开发者正在进行中文翻译工作, 可以到[这里](http://jsfuns.com/ebook/#30d25070-118c-11e3-bc83-47c9e4e1d529)查看目前的工作情况. 其他相关资源:

* [overapi](http://overapi.com/nodejs/)
* [nodejs.cn](http://nodejs.cn/)
* [第三方](http://nodejsapi.cfapps.io/)
* [nodetoolbox](http://nodetoolbox.com/)
* [nodemanual](http://nodemanual.org/)
* [nodejs中文翻译Github项目](https://github.com/pana/node-doc-cn)

Node.js原生模块中有部分是全局变量或所有Module都包含的局部变量不要require可以直接使用. 
如: global, Buffer, require, module, console, process, timers, debugger.
其他模块则可以通过require直接使用(不用安装).

Node.js大致有33个原生模块.以下为根据模块的使用场景和使用频率将模块大致进行了划分.

### core

* Buffer
* events
* globals
* modules
* process
* stream
* timers

### system

* child processes
* file system
* OS
* readline


### network

* DNS
* HTTP
* HTTPS
* Net
* TLS/SSL
* TTY
* UDP/datagram

### utilities

* Crypto
* punycode
* query strings
* string decoder
* url
* path
* utilities
* zlib

### debug

* assertion testing
* console
* debugger

### other

* C/C++ Addons
* Cluster
* domain
* REPL
* vm


在这些模块中有部分比较重要或经常使用:

* events
* globals
* modules
* timers
* console
* process
* fs
* http/https
* query string
* url
* path
* util


本博客将会对这些原生模块逐一进行说明, 所有相关内容都属于Native module 系列.