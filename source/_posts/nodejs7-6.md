title: Golang 1.8 & Node.js 7.6 & Koa 2.0
date: 2017-03-05 21:42:01
tags: release
banner:
---
新年伊始, Golang(1.8) 和 Node.js(7.6) 纷纷发布新版本, 带来不少新特性, 简单梳理如下.

<!-- more -->

## Golang 1.8
时间来到2月份, Golang 依然准确的遵循期发版计划(六月一版), 发布了[最新版本 1.8](https://blog.golang.org/go1.8).
其主要特性如下:

* 1.7 x86 64bit平台引入的全新编译后台, 扩展到所有平台, 为ARM平台带来20%-30%性能提升. 
* 该版本 x86 64bit 的编译器, 链接器更快15%, 并且未来会继续提升
* GC 暂停时间明显缩短, 通常在100微秒以下，有时候甚至低至10微秒
* HTTP服务器添加对 HTTP/2 Push的支持，允许服务器抢先发送响应到客户端
* 上下文（添加到Go 1.7中的标准库）提供了取消和超时机制。Go 1.8在标准库中添加了更多对上下文的支持，包括数据库/ sql和net包以及net / http包中的Server.Shutdown
* 现在使用新添加的Slice函数在排序包中对切片进行排序更简单


## Node.js 7.6
[7.6](https://nodejs.org/en/blog/release/v7.6.0/) 的 node, 把 V8 升级到 5.5. 带来的好处是, 不需要使用 harmony flag 即可使用 async 函数.

```js
async function () {
    let a = await Promise.resolve(123);
}
```

## Koa 2.0
Koa 2.0 代码早早完成开发, 迟迟没有发布只为等待 node 支持 async 特性. 终于 7.6 来了, 这时你就可以这样写 Koa 的中间件了

```js
app.use(async (ctx, next) => {
    const start = new Date();
    await next();
    const ms = new Date() - start;
    console.log(`${ctx.method} ${ctx.url} - ${ms}ms`);
});
```

有兴趣的同学, 可以下载最新版本, 尝试下新特性!