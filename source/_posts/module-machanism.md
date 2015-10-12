title: Node.js模块机制
date: 2013-10-25 10:32:52
tags: Module
---
Node.js的模块机制非常简单: 文件,模块一一对应. Node.js模块机制遵循[CommonJS](http://www.commonjs.org)规范(CommonJS是一套后端JS规范,具体参看官网). Node.js自身实现了require方法用于引入模块, NPM基于CommonJS的包规范, 实现了依赖管理和模块自动安装等功能.

<!-- more -->

## Node.js模块机制

### 简单例子
`foo.js` 加载模块 同路径下的 `circle.js`

foo.js

```
var circle = require('./circle.js');
console.log( 'The area of a circle of radius 4 is ' + circle.area(4));
```
circle.js
```
var PI = Math.PI;

exports.area = function (r) {
  return PI * r * r;
};

exports.circumference = function (r) {
  return 2 * PI * r;
};
```
在模块中想要输出内容(方法, 对象等), 直接加到 `exports` 对象即可.

`exports`本质是 `module.exports`的引用, 目的是 making it suitable for augmentation only 

如果想输出单个 item 例如constructor, 需要直接使用 module.exports
    
    module.exports = MyConstructor;

模块的local变量都是私有的

模块系统是在 `require("module")` 模块中实现

### 循环引用
为了避免循环引用造成infinite loop, 一个模块可能在没有完全执行的情况下返回, 具体情况参看官方解释.
如果在开发中遇到了循环引用, 注意根据这种机制组织代码.

### 核心模块
Node.js包含几个核心模块(原生模块), 这些模块被直接编译到二进制代码中, 位于源代码的 `/lib/` 文件夹中.
这些模块相对于同名的第三方模块有最高优先级.

### File Modules
如果require方法没有找到跟模块同名的文件, 它会继续寻找以下扩展名的文件 `.js`, `.json`, `.node`
`.js`文件被解析为Javascript代码, `.json`文件被解析成json对象, `.node`文件则被当做编译的addon 模块, 使用
`dlopen`打开

如果模块以`/`开头则按绝对路径寻找模块 以`./`和`../`开头按相对路径寻找. 其他情况按核心模块或第三方模块寻找.

如果找不到则会抛出`code`属性为`MODULE_NOT_FOUND`的异常

### Loading from `node_modules`文件夹
如果需要载入的模块不是core模块, 也不是路径模块, 则会从node_modules文件夹中寻找, 寻找规则为从当前路径同级的
`node_modules`开始寻找, 逐级向父路径寻找
例如 `'/home/ry/projects/foo.js'` 调用了 `require('bar.js')` 则寻找路径为:

* /home/ry/projects/node_modules/bar.js
* /home/ry/node_modules/bar.js
* /home/node_modules/bar.js
* /node_modules/bar.js

### 文件夹作为模块
使用文件夹包含程序或库的代码,然后给出单一的使用入口是很好的编程方法, Node也支持文件夹作为模块, 有三种方式可以实现:
第一种是创建一个 `package.json`文件, 并指定 `main` 模块
```
{ 
    "name" : "some-library",
    "main" : "./lib/some-library.js" 
}
```

如果没有package.json则node会尝试加载 `index.js` 或 `index.node` 文件

### Caching
模块在加载之后会被缓存下来. 即多次`require('foo')`调用得到的是相同对象. 

这样多次模块调用, 但模块中的代码只会被执行一次. 该特点使得未完成的模块被返回, 解决循环引用问题.

如果需要模块代码执行多次, 可以export一个方法, 然后多次调用即可.


### Module caching警告
参看[文档](http://nodejs.org/api/modules.html#modules_module_caching_caveats)

### module Object
module不是真正的全局变量,而是一个所有module都包含的变量即module-global, 是代表当前module对象的引用

#### module.exports
module.exports是module系统创建的, 有时用户想要自己使用某个类的实例自己赋值, 可以使用 
`module.exports = new ClassName()`实现
注意该操作必须立刻返回, 不能再回调中完成

#### module.require(id)
#### module.id
#### module.filename
#### module.loaded
#### module.parent
#### module.children

### 模块的整体加载逻辑

### 从global folder加载模块
如果系统设置了`NODE_PATH`环境变量(冒号分割的路径), 则这些路径会被加到module的搜索路径当中.
除此之外, 由于一些历史原因, 还会搜索如下路径

* 1: $HOME/.node_modules
* 2: $HOME/.node_libraries
* 3: $PREFIX/lib/node

$HOME是用户的home路径. $PREFIX是node配置的`node_prefix`

强烈建议将模块放到本地的`node_modules`中, 这样更快, 更可靠

### Accessing the main module
如果一个文件直接从Node执行, `require.main`会被设置为它的`module`.
因此可以通过`require.main === module`判断文件是否是直接运行的

所以可以通过  `require.main.filename`得到当前程序的入口



### 总结
以上为模块的加载详细机制, 具体参看[官方文档](http://nodejs.org/api/modules.html), 以上内容过于全面琐碎. 想看精简总结
版可参看朴灵大拿的[文章](http://www.infoq.com/cn/articles/nodejs-module-mechanism)





## NPM - Node包管理工具
前面提到，JavaScript缺少包结构。CommonJS致力于改变这种现状，于是定义了包的结构规范（http://wiki.commonjs.org/wiki/Packages/1.0 ）。而NPM的出现则是为了在CommonJS规范的基础上，实现解决包的安装卸载，依赖管理，版本管理等问题。require的查找机制明了之后，我们来看一下包的细节

一个符合CommonJS规范的包应该是如下这种结构：

* 一个package.json文件应该存在于包顶级目录下
* 二进制文件应该包含在bin目录下。
* JavaScript代码应该包含在lib目录下。
* 文档应该在doc目录下。
* 单元测试应该在test目录下。

NPM作为Node.js的包管理工具, 能够非常方便的发布, 安装, 卸载, 更新, 管理依赖. 大大方便的Node代码的共享和共用, 作为Node.js生态非常重要的一部分, 大大促进了Node.js发展, 目前已经被集成到了Node.js安装包中, Node环境装好之后即可使用NPM安装第三方包.

关于NPM包的开发和发布之后会详细介绍, 或可参看其他博客, 文章.

## 优缺点, 包现状

## 其他机制

* 前段包管理机制 [seajs](http://seajs.com/)
* AMD机制

## 参考

* [Module API](http://nodejs.org/api/modules.html) -- 官方文档
* [深入Node.js模块机制](http://www.infoq.com/cn/articles/nodejs-module-mechanism) -- 朴灵
* [Nodejs模块开发及发布详解](NodeJS 模块开发及发布详解) -- ElmerZhang
