title:  CoffeeScript
date: 2013-10-09 21:51:34
tags: coffeescript
---

[CoffeeScript](http://coffeescript.org/)的实际意义是允许使用Ruby/Python的更优语法开发JS代码. JS语言本身有good parts和坏的部分(全局变量声明, with等), CoffeeScript的目标是使用一种简单的方式暴露JS的good parts

## About coffeescript

* 使用Coffeescript开发
* Golden rule: 'It's just JavaScript'
* 同直接使用JS开发的同等代码相比有更高的执行效率
* 能在所有的JavaScript runtime 执行: 所有浏览器, node
* command-line version coffee 是一个Node.js utility, 包含compiler和REPL, 使用npm安装 `npm install -g coffee-script`, 同时包含一个build system cake


## Features (coffee has a ruby like grammer):

* 使用有意义的空白符界定代码块, 抛弃{}
* 变量作用域自动管理, 不需要 var
* 换行作为表达式的结束, 不需要 ;
* 方法调用可省略()
* 变量对象定义, 换行处无需 ,
* 变量定义可省略 {}
* 灵活的条件语句: if else then until unless
* 方法定义支持默认参数
* 方法定义调用支持不定量参数
* 方便的循环和迭代
* 支持range
* everything is expression
* 定义操作符的单词行alias: is isnt not and or true yes on false no off @ this of in
* 存在操作符 ?
* 方便的类定义形式: Class, Inheritance, Super
* Function binding
* Destructuring Assignment


## coffee grammer

* Functions
* Objects and Arrays
* Lexical Scoping and Variable Safety
* If, Else, Unless, and Conditional Assignment
* Splats…
* Loops and Comprehensions
* Array Slicing and Splicing with Ranges
* Everything is an Expression(at least, as much as possible)
* Operators and Aliases
* The Existential Operator
* Classses, Inheritance, and Super
* Destructuring Assignment
* Function binding
* Embedded Javascript
* Switch/When/Else
* Try/Catch/Finally
* Chained Comparisons
* String Interpolation, Block Strings, and Block Comments
* Block Regular Expressions




## 学习目标

1. 学习并灵活使用coffee支持的所有语法
2. 知道coffee代码的转换规则


更多内容参看[coffeescript](http://coffeescript.org/), 包含coffeescript的特点, 前世今生, 安装, 语法, books, screencasts, exmaples, resources, annotated source, 交互终端

个人推荐Node.js开发的同学使用. 后端开发不涉及dom操作, 使用coffee开发应该能加快开发效率, 提升代码执行效率.
