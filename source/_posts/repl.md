title: REPL
date: 2013-09-23 18:03:07
tags: REPL
---
# What is [REPL](http://nodejs.org/api/repl.html) ?
Read-Eval-Print-Loop(REPL)提供了交互式执行JS代码,查看结果的功能, 输入一行(块)代码, 可立刻看到执行结果. 
适用于调试, 测试, 尝试代码. 不止N, Ruby, Python, Lua等语言都提供了类似的交互式环境.

<!-- more -->

## 如何打开
终端中命令`node`(不带参数)可以打开REPL. 它具有简单的Emacs行编辑.

设置环境变量`NODE_NO_READLINE=1`然后执行`node`可以打开高级line-editors

## repl.start(options)
N的REPL不仅能独立运行还支持以编程的形式在其他程序中运行.
```
repl = require("repl")
repl.start({
    prompt: "node via stdin>",
    input: process.stdin,
    output: process.stdout
});
```

options具体参数, 事件, 使用方式参看REPL [API](http://nodejs.org/api/repl.html)

## Features

* Control+D退出
* 支持多行Expression
* Tab 补全功能支持全局和局部变量
* `_` 保存最后一个Expression的值
* 特殊REPL命令(均以句号开头,在REPL中.help查看)
    1 .break  清空多行Expression
    2 .clear  重置context对象为空,清空多行Express
    3 .exit   退出REPL
    4 .help   打印帮助
    5 .save   将REPL当前session保存到文件
    6 .load   加载文件代码到REPL
* Control+c一次相当于.break, 两次相当于.exit即退出

## Last
灵活运用REPL,可以给开发工作带来很多方便和帮助.关于REPL的更多内容请参看[官方文档](http://nodejs.org/api/repl.html)