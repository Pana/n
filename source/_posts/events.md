title: events
date: 2013-11-08 14:50:41
tags: Module
---
Node.js在Github代码仓库上的描述为Evented I/O for V8 JavaScript. 这句话清晰表达了Node.js最大特点Evented. Node.js通过事件异步机制成功突破单线程编程模型的性能机制, 并有前后端编程模型统一, 性能更高等优点, 从而迅速被大家所接受.本文介绍下Node.js的事件机制.

## Event 模块
event模块是Node.js的核心原生模块, 提供事件绑定, 触发等相关方法. Node.js的大部分模块都继承自Event模块. 与前端DOM树事件不同之处在于不存在冒泡, 捕获等行为, 亦没有preventDefault(), stopPropagation(), stopImmediatePropagation().

Event模块官方[API](http://nodejs.org/api/events.html), 中包含EventEmitter类.

class: events.EventEmitter

* addListner(event, listener)
* on(event, listner)
* once(event, listner)
* removeListener(event, listner)
* removeAllListener([event])
* setMaxListeners(n)
* listeners(event)
* emit(event, [arg1], [arg2], [...])
* Class Method: EventEmitter.listenerCount(emitter, event)
* Event: 'newListener'
* Event: 'removeListener'

关于事件的几点说明:

* 事件机制本身是hook的一种实现, 可用于导出内部数据或状态
* 通常事件命名遵循camel-cased字符串规则, 但是没有强制限制
* 绑定到对象上得方法通常叫做监听器, 在监听器内部this指向EventEmitter对象
* 如果事件对象遇到error, node会触发一个`'error'`事件, 如果事件没有绑定listener, node会打印stack trace, 然后退出程序
* 一个对象的某个事件绑定listener超过默认10之后, node会打印警告, 该行为对查找memory leak非常有用.可以使用setMaxListners(n)修改这个默认行为, 设为0为不限制

## 继承Event
可使用原生util模块继承EventEmitter对象

```
var util = require("util");
var events = require("events");

function MyStream() {
    events.EventEmitter.call(this);
}

util.inherits(MyStream, events.EventEmitter);

MyStream.prototype.write = function(data) {
    this.emit("data", data);
}

var stream = new MyStream();

console.log(stream instanceof events.EventEmitter); // true
console.log(MyStream.super_ === events.EventEmitter); // true

stream.on("data", function(data) {
    console.log('Received data: "' + data + '"');
})
stream.write("It works!"); // Received data: "It works!"
```

## 其他
在Node中回调函数通常作为方法的最后参数传递.


### 参考

* [深入浅出Node.js事件机制](http://www.infoq.com/cn/articles/tyq-nodejs-event)
* [官方API](http://nodejs.org/api/events.html)


