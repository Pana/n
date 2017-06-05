title: Node.js 8.0
date: 2017-05-30 17:48:38
tags:
banner:
---
Node.js 8.0终于发布了, 应该是今年 Node 最大的版本更新, 带来许多新特性: NPM 5.0, V8 5.8, N-API, async_hooks ...
<!-- more -->

### NPM 5.0
5.0 的 npm 有两大改进: 一是性能巨大提升(5倍), 二是 lockfile 支持: Shrinkwrap 从 5.0 开始变成了默认开启功能. 有了该特性, 每次npm install 的结果都是一样的, 不会产生版本升级导致项目无法运行. [详情参看](http://blog.npmjs.org/post/161276872334/npm5-is-now-npmlatest)

### V8 5.8
Node.js 8.0.0 使用了 V8 5.8，这是 JavaScript 运行时的重要更新，其中包括性能方面和面向开发者 API 的重大改进。对 Node.js 开发者来说最重要的是 V8 5.8 保证与 V8 5.9 和即将推出的 V8 6.0 具有 ABI 的向前兼容性，这将有助于确保 Node.js 原生插件生态系统的稳定性。在 Node.js 8 的生命周期中，会计划升级到 5.9 甚至 6.0

V8 5.8 引擎还有助于设置新的 TurboFan + Ignition 编译器管道（compiler pipeline）的转移，这将为所有 Node.js 应用程序提供重要的新的性能优化。虽然 V8 之前的版本已经存在，但 TurboFan 和 Ignition 将在 V8 5.9 中首次默认启用。新的编译器管道代表了这样一个重大变化 —— Node.js 核心技术委员会（CTC）选择推迟最初发布安排在 4 月的 Node.js 8，以便更好地适应它。

另外此次该版本的 V8 运行 async 方法的速度也有巨大的提升.

### N API
对于使用或创建原生插件的 Node.js 开发者，新的实验性的 Node.js API（N-API）对于现有的 Native Abstractions for Node.js (nan) 来说是一个重大的改进，它将允许原生插件在一个系统上编译一次，并在多个版本的 Node.js 上使用。

通过提供一个新的虚拟机不可知的应用程序二进制接口（ABI），原生插件不仅可以在多个版本的 V8 JavaScript 运行时上运行，还可以在微软的 Chakra-Core 运行时上使用。

N-API 在 Node.js 8.0.0 中是实验性的功能。

另外提一下 Rust 对 N API 也有封装, 即可以使用 Rust 开发原生的 Node 模块. 具体可以参考

* [neon](https://github.com/neon-bindings/neon)
* [Rust for Node developers](http://www.tuicool.com/articles/bU7byaJ)
* [node love rust](https://mp.weixin.qq.com/s?src=3&timestamp=1496628364&ver=1&signature=HpNvv4g2xUmSBJJwxA5UGVjd26sL8O8-p*PD7Vp8Wy6Lvb8-YfsN6omcj27xj1Tqf4VTQ2vevuhuG01LnGzMkEAPffbFDsQVgGk6sqwZT8Z851wh2PiWlKf-4pgoEcxgmP5nU0lplJWmPWLvHlySOV4egbAcc8*sEGzOb8mWawk=)

### async_hooks
Async Hooks （以前称为AsyncWrap） API允许获取有关句柄对象生命周期的结构跟踪信息。它包含了一组用于诊断的 API，开发人员可以用它监控 Node.js事件循环里的各种操作，跟踪句柄对象全生命周期的事件。可以通过该模块的 createHooks方法注册用于处理句柄对象生命周期各个阶段事件的函数
Async Hooks API 在 Node.js 8 中如何工作, createHooks函数的注册功能会被每一个异步操作的不同生命周期事件调用。
```js
const asyncHooks = require('async_hooks')
asyncHooks.createHooks({ 
    init,
    pre,
    post,
    destroy
})
```
了解更多Async Hooks，或查看正在进行的工作文档。这些函数将会根据处理程序对象的生命周期事件选择性触发。

### LTS
Node.js v8 是下一个长期支持（LTS）的版本。这将在 2017 年 10 月进入，一旦 Node.js 8 转换到 LTS，将会使用代号 Carbon。

### 其他更新

* WHATWG URL parser
* Buffer API 引入了大量新变化
* 新的 util.promisify() API
* console 模块加入 console.log() 和 console.error()

[官方更新日志](https://nodejs.org/en/blog/release/v8.0.0/#say-hello-to-npm-version-5-0-0)
