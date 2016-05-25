title: Koa 2.0 详解
date: 2015-11-25 20:42:08
tags: koa
banner: /assets/koa.png
---
随着 Node.js 不断增加 ES2015 特性支持, 以及 babel@6.0 的不断成熟, koa也不断演化, 前几天 koa 团队发布了 2.0 的预览版, 最大改变是移除了 co 依赖, 并且仅支持 node 4.0 及以上版本

<!-- more -->

## 变化

Koa 2.0 使用 ES2015 特性重写了整个代码, 主要用到了 arrow function, promise, let const, template string, class 等特性. 在 1.0 时候 koa 需要依赖 co 结合 generator 实现 stack like 式的 middleware 功能, 2.0 移除了对 co 的依赖, 改而使用 Promise 实现该功能.

在 1.0 中 middleware 是 generatorFunction, 接受一个参数.
```js
app.use(function *(next){
  var start = new Date;
  yield next;
  var ms = new Date - start;
  console.log('%s %s - %s', this.method, this.url, ms);
});
```

在 2.0 中可以直接使用普通方法作为 middleware, 中间件需要接受两个参数 ctx, next.

```js
app.use((ctx, next) => {
  const start = new Date;
  return next().then(() => {
    const ms = new Date - start;
    console.log(`${ctx.method} ${ctx.url} - ${ms}ms`);
  });
});
```

两种形式的中间件 next 的作用差不多, 都是将控制流程交给后续 middleware, 前者是 generator 或 generator 代理, 后者是一个方法, 调用后开始执行后续中间件并返回一个 promise 对象, 该对象 resolve 后表示后续 middleware 执行完成, 可以在它的 then callback 中执行一些操作.

另外一个区别为在 1.0 中, middleware 的 this 代表当次请求的 context, 而在 2.0 中需要通过参数 ctx 获取.

也许有人会疑问这种写法不是又回到回调的形式了么, 但是因为 next() 执行的结果是一个 promise 所以就可以同 ES2016 的 async/await 结合使用

```js
app.use(async (ctx, next) => {
  const start = new Date;
  await next();
  const ms = new Date - start;
  console.log(`${ctx.method} ${ctx.url} - ${ms}ms`);
});
```
这样就完美了, 不仅形式上同 generator 一样简洁, 而且不要通过 hack 即实现的了优雅异步, 因为 async/await 本身就是为了解决异步而提出的语法. 当然现在 node 还不支持, 所以需要 babel@6.x 来开启该特性. 详细 setup 方法为.

```bash
// install babel and required presets
$ npm install babel-core --save
$ npm install babel-preset-es2015-node5 --save
$ npm install babel-preset-stage-3 --save
```

```js
// set babel in entry file
require("babel-core/register")({
     presets: ['es2015-node5', 'stage-3']
});
```

当然 2.0 也支持 generatorFunction 作为中间件, 但需要引入 co 模块

```js
app.use(co.wrap(function *(ctx, next){
  const start = new Date;
  yield next();
  const ms = new Date - start;
  console.log(`${ctx.method} ${ctx.url} - ${ms}ms`);
}));
```

对于大量的 1.0 版的中间件, 可以使用 [koa-convert](https://github.com/gyson/koa-convert) 模块转化成 promise middleware.

```js
const convert = require('koa-convert')

app.use(convert(function *(next){
  const start = new Date;
  yield next;
  const ms = new Date - start;
  console.log(`${this.method} ${this.url} - ${ms}ms`);
}));
```

## middleware 执行流程, 及总结
这里总结下 middleware 的执行流程, 如果有 noder 对详细内容不太了解可以参看其他文章详细介绍. koa 的 middelware 组织及运行方式同 django 类似, 请求先到达时先经过一层层中间件处理进到最里层, 然后再反向一层层流出. 

![](/assets/stack-middleware.svg)

这里总结下 koa 的中间件执行顺序:

1. 请求进入时按照 middleware 的挂载顺序执行, 出来时按照相反的顺序执行
2. next 方法不能执行多次
3. next 方法如果不执行, 其后加载的中间件都无效
4. koa 自动在用户挂载的中间件之后添加了一个 respond 中间件, 为事实上的最终中间件, 用于处理响应数据.
5. 某个 middleware 如果 return 了数据, 其将会作为上一个中间件 next() resolve回调的参数


## 开发进程
现在 koa 新创建了 2.x branch, 并且建议其主流 middleware 采用类似的方式开发, 用户可以通过如下方式安装.

```
$ npm install koa@next
```

找目前的开发进度, 可能近期不会正式的 release. 有兴趣的同学可以尝鲜, 并参与到开发当中来.
