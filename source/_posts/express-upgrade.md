title: Express 升级
date: 2014-02-25 14:51:56
tags: Express upgrade
---
Express 是 Node.js 平台使用最多, 最知名的 web 框架, 目前正式版本为 3.5
Node 0.12 即将发布, Connect 3.0 和 Express 4.0 也都在紧锣密鼓的开发当中
3.3 号, Express 4 发布了 RC 版. 这里介绍下Express 4 的最新特点

### Express 4
Express 4 将生成项目架构命令行工具移到了单独的项目 [express-generator](https://github.com/expressjs/generator),
并且移除了对 Connect 的依赖. 具体特点为:

* 健壮的路由系统
* 包含 HTTP helpers (跳转, 缓存等)
* 试图系统支持超过 14 中模板引擎
* Content negotiation
* 专注高性能
* 基于 environment 的配置
* 项目框架生成命令行工具
* 测试全面

Express 两大主要变化: 去掉了对 connect 中间件的捆绑, 如果需要可以添加到项目 package.json 使用, 这样使得中间件可以更加灵活的更新和 fix bug
而不会影响到 Express; 增强的路由系统 具体信息参看[New features in 4.x](https://github.com/visionmedia/express/wiki/New-features-in-4.x)

关于 Express 3.0 到 4.0 迁移可参看这里[Migrating from 3.x to 4.x](https://github.com/visionmedia/express/wiki/Migrating-from-3.x-to-4.x)

Express 的设计哲学为: 提供 精简, 健壮的 HTTP server 开发工具. 为 单页应用, 网站, 混合类应用, 公共 HTTP API
提供完美解决方案.

Express 不强制用户使用指定的 ORM 或 模板引擎, 用户可以自由选择. 

关于 Express 4 完整的 API, 可[参看](http://expressjs.com/4x/api.html)


参考:

* [Express API](http://expressjs.org)
* [Express github](https://github.com/visionmedia/express)
* [new features in 4.x](https://github.com/visionmedia/express/wiki/New-features-in-4.x)
* [express migrate from 3.x to 4.x](https://github.com/visionmedia/express/wiki/Migrating-from-3.x-to-4.x)
* [ExpressJS 4.0: New Features and Upgrading from 3.0](http://scotch.io/bar-talk/expressjs-4-0-new-features-and-upgrading-from-3-0)
* [Express roadmap](https://github.com/visionmedia/express/wiki/4.x-roadmap)



### Connect 3.0

[Connect](https://github.com/senchalabs/connect) 是一个 Node.js 的中间件层, 
可扩展的 HTTP server 框架. Express 3.0 即是建立在 connect 之上. 跟随 Node 0.12
的步伐, Connect 3.0 也在开发当中, 主要的调整为:

* 中间件会被迁移到 [expressjs](http://github.com/expressjs) 组织的独立项目中
* 所有的中间件不止能适用于 Connect, 还可以适用于相似的框架如 [restify](https://github.com/mcavage/node-restify), 因此所有的 Node patched 会被移除.
* 停止对 Node 0.8 的支持.
* 网站文档将会被移除, 可以查看项目的 README 文档作为替代.

注: 部分中间件将会被停止支持: cookieParser, limit, multipart, staticCache, query. 
可以使用其他插件替代, 具体参看 [connect middleware](https://github.com/senchalabs/connect#middleware).
这部分插件会对部分 Express 3 造成影响, 在项目启动时候会看到升级提示警告: `connect.bodyParser()` 将不再支持.
可以使用
```
app.use(connect.urlencoded())
app.use(connect.json())
```
替代, 具体的信息, 参看[升级提示](https://github.com/senchalabs/connect/wiki/Connect-3.0)

关于 Connect 的最新进展, 相关中间件, 详细文档, 可以参看其 [Github](https://github.com/senchalabs/connect) 项目,
如果有兴趣维护这些中间件可以联系 [expressjs 的成员](https://github.com/orgs/expressjs/members).

参考:

* [Connect doc](http://www.senchalabs.org/connect/)
* [Connect 3.0 migration guide](https://github.com/senchalabs/connect/wiki/Connect-3.0)
* [Connect github](https://github.com/senchalabs/connect)
