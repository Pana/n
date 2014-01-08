title: generator
date: 2014-01-02 16:00:10
tags: ES6 异步流程
---

## ES6
[ES6(代号harmony)](http://wiki.ecmascript.org/doku.php)是下一代JavaScript语言规范, 目前还在制定当中. 在2014年底将会完成规范制定, 并有部分Engine实现. ES6包含众多新语言[特性](http://tc39wiki.calculist.org/es6/): Module, Class, Set, Map, iterator, generator, for-of...  

众多的语法特性将为JS开发打开全新的天地. 这里主要介绍[generator](http://wiki.ecmascript.org/doku.php?id=harmony:generators)和[iterator](http://wiki.ecmascript.org/doku.php?id=harmony:iterators), 因为该特性能够以同步的代码实现异步的流程.

## generator & iterator
iterator和generator特性借鉴于Python, Ruby, smalltalk, 本来用于方便访问容器内各个元素. 该特性需要node v0.11.9并开启--harmony特性才能使用, 在chrome(29+)浏览器中需要在`chrome://flags/` 开启`Enable Experimental JavaScript`选项, 然后重启. 这里先介绍下相关的概念.

iterator(迭代器): 具有next方法的对象, 返回{done: true/false, value: returnValue}结构数据

iterable(可迭代变量): 具有内部方法`obj[@@iterator]()`的对象, 该方法会返回iterator.

generator(生成器): 一种特殊的iterator, 其next()方法的返回结果由他的generator function定义. generator还包含一个throw方法, 他们next方法可以接受一个参数.

generator function: 是generator的构造器方法, 在该方法内可以使用yield关键字. yield调用的地方可以传出value(通过next方法)或exception(通过throw方法). generator function的书写语法为 `function*`

generator comprehension: 一种生成generator的简化表达式如: `(for (x of a) for (y of b) x * y)`


在generator function内部可以使用 yield 语句：

```
function* genFunc () {
    console.log('step 1')
    yield 1
    console.log('step 2')
    yield 2
    console.log('step 3')
    return 3
}
```

调用generator function的时候，你会获得一个generator对象

```
var gen = genFunc()  // 可以传递参数
```

这个对象有一个方法叫做 next()。每当调用 next() 的时候，generator function内部就会执行直到遇到下一个 yield 语句，然后暂停在那里，并返回一个对象。这个对象含有被 yield 的值和generator函数的运行状态

```
var ret = gen.next() // 输出: 'step 1'
console.log(ret.value) // 1
console.log(ret.done) // false
```

直到generator函数内部不再有 yield 语句存在了，这时你再调用 next()，获得的就会是该函数的常规返回值 (return 的值)：

```
ret = gen.next() // 输出 'step 3'
console.log(ret.value) // 3
console.log(ret.done) // true
```
同时，iterator对象的 next() 方法是可以传递一个参数的。这个参数将会成为generator函数内对应 yield 语句的返回值：

```
function* genFunc () {
    var result = yield 1
    console.log(result)
}
var gen = genFunc()
gen.next() // 此时generator内部执行到 yield 1 并暂停，但还未对result赋值！
// 即使异步也可以！
setTimeout(function () {
    gen.next(123) // 给result赋值并继续执行，输出: 123
}, 1000)
```

yield语句会暂停生成器方法中的代码执行, 但对外部代码没有影响, 外部代码可以继续执行下去. 这样generator function可以借助 yield 在需要的时候才继续执行剩余的语句，并且传递回一个值, 这其实是另外一种形式的回调, 而且这里可以使用同步的形式表达异步的流程, 没有函数嵌套. TJ大神的[co](https://github.com/visionmedia/co)流程控制库就是这个原理. [参考]()

其他 generator function定义:

```
function* foo1() { };
function *foo2() { };
function * foo3() { };
 
foo1.toString(); // "function* foo1() { }"
foo2.toString(); // "function* foo2() { }"
foo3.toString(); // "function* foo3() { }"
foo1.constructor; // function GeneratorFunction() { [native code] }
```


几点需要注意的地方:

* yield 只能在生成器构造方法中使用
* yield 只会将它的第一个参数返回, 且至少有一个参数否则会报错
* 生成器构造方法接受参数
* next()方法接受一个参数, 该值会返回给yield的左值, next参数作为上一个yield的左值

### for-of
ES6的`for of`表达式可在iterables上使用 `for (let x of iterable) { /* ... */ }`
它会去调用`iterable.prototype[@@iterator]()`方法获取iterator对象. `Map.prototype`, 
`Set.prototype`以及所有包含`@@iterator`方法的都能使用`for of`表达式.

`for in`表达式用法没有发生任何变化, 不能用于iterables上

如果一个`iterator`拥有`@@iterator()`方法那他也是`iterable`的. ES6中大部分iterator都是iterable的, 内部方法会直接返回this(本身). 而且所有通过generator function和generator comprehension创建的generator都有iterable的.

```
const lazySequence = (for (x of a) for (y of b) x * y);
for (let z of lazySequence) {
  console.log(z);
}
```

关于如何创建iterable对象和Generator Comprehension Desugaring可参看[这里](http://domenic.me/2013/09/06/es6-iterators-generators-and-iterables/)


参考: 

* [es6-iterators-generators-and-iterables](http://domenic.me/2013/09/06/es6-iterators-generators-and-iterables/)
* [segment问答](http://segmentfault.com/q/1010000000367154#a-1020000000373763)
* [es6中生成器函数介绍](https://www.imququ.com/post/generator-function-in-es6.html)
* [迭代器和生成器](https://developer.mozilla.org/zh-CN/docs/JavaScript/Guide/Iterators_and_Generators)
* [generators-in-v8](http://wingolog.org/archives/2013/05/08/generators-in-v8)
* [Harmony Generator, yield, ES6, co框架学习](http://bg.biedalian.com/2013/12/21/harmony-generator.html)
* [co中yield的n种重载](http://bg.biedalian.com/2014/01/08/what-can-i-yield.html)


## 相关module: gnode, co, suspend, promise
generator除了可以用于方便迭代之外, 在JS中(尤其是Node.js)估计最有用的地方应该是流程控制了. 利用yield的特性可以使用同步的代码开发异步的流程, 避免逻辑混乱和函数嵌套. 目前TJ大神已经开发除了新的流程控制模块[co](https://github.com/visionmedia/co), ([这里对generator和co进行了详细介绍](http://www.html-js.com/article/1687)) 并在此基础之上开发了全新的web 框架koa. 等0.12版发布之后, koa可以很方便应用, 届时又将是一个很火的框架. 另外[suspend](https://github.com/jmar777/suspend)也是使用generator特性实现的全新流程控制解决方案. 

虽然现在ES6还没有被实现, 但是[gnode](https://github.com/TooTallNate/gnode)可以让现在的JS引擎支持generator特性, 有兴趣的同学可以尝试下. Node相交于浏览器没有多浏览器兼容问题, 一旦v8完全支持即可迅速采用, 另外0.12版本的node马上就会到来.

使用co可以使用同步的代码实现异步的流程, 下例中三个读取文件的操作顺序执行, 但没有嵌套代码

```
var co = require('co');
var fs = require('fs');

function read(file) {
  return function(fn){
    fs.readFile(file, 'utf8', fn);
  }
}

co(function *(){
  var a = yield read('.gitignore');
  var b = yield read('Makefile');
  var c = yield read('package.json');
  return [a, b, c];
})()
```

并行执行的异步代码

```
co(function *(){
  var a = size('package.json');
  var b = size('Readme.md');
  var c = size('Makefile');

  return [yield a, yield b, yield c];
})()
```

这只是简单的展示, co支持参数支持, 异常处理, 只有300行代码, 支持promise, thunk, array, object, generator, generator function. 关于co的详细使用可参看co[github](https://github.com/visionmedia/co), 有兴趣的同学可以尝试下[koa](https://github.com/koajs/koa)框架 

## 参考资料

* [ES6 proposals official wiki](http://wiki.ecmascript.org/doku.php?id=harmony:proposals)
* [ES6 proposals](http://espadrine.github.io/New-In-A-Spec/es6/)
* [MDN iterator and generator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators)
* [ES6资源](http://addyosmani.com/blog/ecmascript-6-resources-for-the-curious-javascripter/)
* [Using es6 today](http://globaldev.co.uk/2013/09/es6-part-1/)
* [ES6 Feature proposals](http://tc39wiki.calculist.org/es6/)
* [Harmony process](http://tc39wiki.calculist.org/about/harmony/)
* [ES6规范制作流程](http://www.cnblogs.com/ziyunfei/archive/2012/12/05/2802382.html)
* [ES6相关文章(中文)](http://www.tuicool.com/topics/11060047?st=0&lang=0&pn=3)
* [ES6各浏览器实现情况](http://kangax.github.io/es5-compat-table/es6/)
* [ECMAScript 6，一定要了解的10个特性](http://www.xdf.me/?p=754)
* [eight cool features coming in es6](http://net.tutsplus.com/tutorials/javascript-ajax/eight-cool-features-coming-in-es6/)
* [what's the big deal with generator](http://devsmash.com/blog/whats-the-big-deal-with-generators)
* [New in javascript 1.7](https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/1.7?redirectlocale=en-US&redirectslug=JavaScript%2FNew_in_JavaScript%2F1.7)
* [javascript-yield](http://jlongster.com/2012/10/05/javascript-yield.html)
* [promise](http://wiki.commonjs.org/wiki/Promises)
* [when.js](https://github.com/cujojs/when)
* [Q](https://github.com/kriskowal/q)

