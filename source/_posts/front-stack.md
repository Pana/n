title: React 技术栈
date: 2015-10-21 21:30:59
tags:
banner: /assets/react-stack.png
---
最近需要做一个 web app, 刚好 React 最近比较火, 所以就打算用它来开发. 于是就去学了 React 相关的一些技术. 不看不知道, 原来想要使用 React 得需要学Flux, ES6, webpack 一系列的相关技术, 这里简单介绍下React 栈相关的技术, 及其作用. 希望其他人能对 React 全貌有个简单的了解.

<!-- more -->

## React

[React](https://facebook.github.io/react/) 是由 Facebook 及 Instagram 技术团队开发并开源的前端开发技术. 是即 Angular 之后又一被大家所追捧的技术. 尤其在 [React Native](https://facebook.github.io/react-native/) 推出之后更是点燃了大家的热情. 并逐渐形成了其优于并将逐步取代 Angular 的一种论调.

React 其实并不是一个完整的前端开发框架, 而是用来构建用户界面, 其只负责 MVC 的 View 部分. 其最大的特点是采用 XML 类似的语法, 并利用虚拟 DOM 的机制实现页面的高速渲染, 及服务器端渲染. 其把页面元素抽象成组件 (Component), 组件有其生命周期, 及数据更新方式. React 开发其实就是组件抽象及开发.

个人认为 React 相较于 Angular 最大的优点在于简单专一: 只做一点, 并做好. React的学习曲线要远远小于 Angular.

## Flux

React 自己无法完成 App 开发, 需要有技术完成数据的获取更新. Facebook 提出了 [Flux](https://facebook.github.io/flux/) 来跟 React 搭伙工作. Flux 并不是一个真正的库或框架, 而是用于构建 Web 程序的一种架构, 即一种实现规范和理念. 其主张数据应该单项流动, 这个 Angular的双向绑定大不相同. Facebook 认为数据单项流动可以避免众多关联数据更新所带来的状态混乱.

![](/assets/flux-simple-f8-diagram-explained-1300w.png)

从图中可以看到数据都是单项流动. Dispatcher 作为数据流动的中心, 向不同的 store 派发更新, 最终触发页面重新渲染.

React 社区开发了众多 Flux 实现([Alt](http://alt.js.org/)), 各自有其特点. Facebook 也给出了自己的实现.

## [React-router](https://github.com/rackt/react-router)

想要开发完整的 SPA 程序, 仅仅 React, Flux 还不够. 因为我们还需要路由器实现页面跟组件的对应, 进行页面跳转, 分离代码. React-router 貌似是社区使用最多的一个路由器实现. 具有按需加载, 动态路由, 路由转移处理等特性. 加上它实现 SPA 开发的所有技术就齐了.

## ES6

今年年中 ES6 规范已通过审核, 定稿. V8 及各个引擎都在陆续支持 ES6 的新特性. 虽然你完全可以使用 ES5 进行 React 开发, 但 ES6 的 class, 箭头函数, 生成器, 对象解构等众多特性绝对值得我们一试. 自己已经在 Node 项目中使用了部分特性, 让人爱不释手. 通过 [babel](http://babeljs.io/) 等编译器可以让我们现在就能用上 ES6 的众多特性.  如果你还对 ES6 的新特性不太了解, 可以参看阮一峰的 [ECMAScript 6入门](http://es6.ruanyifeng.com/).

## [Webpack](http://webpack.github.io/)

Webpack 是一个打包器, 是 React 开发中必不可少的工具. JS 的打包工具实在是不少, 如 browserify, 甚至 npm. 但 webpack 功能更加强大, 同样是 Instagram 团队开发. 在 webpack 中图片, 样式表都可以看做 module 从而进行打包. 支持 amd, cmd, umd 规范, 支持分离打包, 插件机制扩展方便.

## 其他

* [React Developer Tools](https://github.com/facebook/react-devtools) 是一个调试工具
* [Nuclide](http://nuclide.io/) 基于 atom 的 React 开发工具
* [React Native](https://facebook.github.io/react-native/) Mobile App 开发框架


以上为 React 相关技术的介绍和总结, 希望能让刚接触的同学了解大致的面貌. 具体的技术内容请参看相关文档.


#### 参考

* [前端革命，革了再革：WebPack](http://segmentfault.com/a/1190000002507327)
* [Frontend: welcome to the future](https://medium.com/@olegafx/frontend-welcome-to-the-future-91ff064884b6) 前端技术更新太快, 想了解最新情况的同学看向这里
