title: Webassembly 初识
date: 2017-06-11 11:00:29
tags:
banner:
---
Webassembly 简称 wasm, 其开发的目的是让 Web 能够实现接近原生的性能, 充分利用硬件性能. 它是一种类似编译语言的AST码, 可以作为任何编程语言的编译目标. 它将会作为JS 的补充, 用于满足需要密集计算的任务(在线游戏、音乐、视频流、AR／VR、平台模拟、虚拟机、远程桌面、压缩及加密等). 目前已经得到 Chrome, Edge, Mozilla, Webkit 支持. 将会是 Web 平台近十多年最大的进步.

<!-- more -->

### 为什么 WebAssembly 这么重要
众所周知 JS 是一个脚本语言, 通过 JIT 逐行编译代码的形式运行, 因此影响了JS的执行性能, 一直以来各大浏览器厂商都在尝试提升性能, 如火狐的 asm.js, Chrome的PNaCI, 苹果的FLTJIT, 但因为各自为政一直没有效果, 另外也有通过插件的方式提高效能但同样具有兼容性, 耗电量高等问题. WebAssembly 目前看来成为解决该问题的唯一方案, 四大浏览器厂商均表示支持, 并且W3C成立的专门的工作小组, Chrome 在最新版本甚至放弃了对PNaCI的支持, 专注 wasm.

* wasm 是一种新的适合于编译到Web的，可移植的，大小和加载时间高效的格式。是一个新的与平台无关的二进制代码格式, 远小于JavaScript，可由浏览器的JavaScript引擎直接加载和执行，这样可节省从JavaScript到字节码，从字节码到执行前的机器码所花费的即时编译JIT（Just-In-Time）时间。 作为一种低级语言，它定义了一个抽象语法树（Abstract Syntax Tree，AST），开发人员可以以文本格式进行调试。
* wasm 是一种中间码, 理论上任何一种语言都可以编译为 wasm, 意味着你可以用任何一中语言开发web 应用.
* wasm 描述了一个内存安全的沙箱执行环境，可以在现有的JavaScript虚拟机中实现。 当嵌入到Web中时，WebAssembly将强制执行浏览器的同源和权限安全策略。因此，和经常出现安全漏洞的Flash插件相比，WebAssembly是一个更加安全的解决方案
* wasm 的目的不是取代 JavaScript, 而是互为补充, JS用于应用逻辑开发, wasm 用于高性能计算. 使用 wasm 的 JS API, 可以将 wasm Module 载入JS 应用, 共享功能.
* wasm不是将C/C++等其他语言编译到JavaScript，更不是一种新的编程语言

### 浏览器支持情况
W3C Web——Assembly社区组于2015年4月底成立. 2016年10月31日，WebAssembly到达浏览器预览的里程碑. ebAssembly社区组已经有初始（MVP）二进制格式发布候选和JavaScript API在多个浏览器中实现当浏览器预览结束时，社区组将产生WebAssembly的草案规范，并且浏览器厂商可以开始默认提供符合规范的实现。预计在2017年上半年，四大主流浏览器对原生的WebAssembly支持将到达稳定版。
Webassembly的初期目标是支持C/C++, Rust, 待成熟后会支持其他语言如Java, Go 等. 当前浏览器的支持情况为:

* Chrome 57 (PC/Android)
* [Firefox 52](https://hacks.mozilla.org/2017/03/firefox-52-introducing-web-assembly-css-grid-and-the-grid-inspector/)
* Edge 15
* Safari 11


### 初试

如果想尝试一下的话可以参看[官网的指导](http://webassembly.org/getting-started/developers-guide/)或这篇介绍[WebAssembly，Web的新时代](http://blog.csdn.net/zhangzq86/article/details/61195685).



#### 参考

* [官网](http://webassembly.org/)
* [WebAssembly：面向Web的通用二进制和文本格式](http://www.infoq.com/cn/news/2015/06/webassembly-wasm/)
* [wasm news](https://wasm.news/)
* [webassembly in nutshell](https://developer.mozilla.org/en-US/docs/WebAssembly)
* [Why WebAssembly is a game changer for the web — and a source of pride for Mozilla and Firefox](https://medium.com/mozilla-tech/why-webassembly-is-a-game-changer-for-the-web-and-a-source-of-pride-for-mozilla-and-firefox-dda80e4c43cb)
* [Github](https://github.com/WebAssembly)
* [WebAssembly，Web的新时代](http://blog.csdn.net/zhangzq86/article/details/61195685)

