title: pointer & interface
date: 2017-05-07 22:04:45
tags:
banner:
---

指针, 接口的说明和常见用法

<!-- more -->
### 指针

一个指针的值是另一个变量的地址, 通过指针，我们可以直接读或更新对应变量的值，而不需要知道该变量的名字

```go
var x int  // 声明一个 int 类型的变量 x

var p *int  // int 类型对应的指针类型为 *int

p = &x  // 取地址操作

*p = 3  // 该表达式表示 p指针指向的变量, 等同于 x
```

任何类型的指针的零值都是nil

因为指针包含了一个变量的地址，因此如果将指针作为参数调用函数，那将可以在函数中通过该指针来更新变量的值

指针又叫引用, slice, map, chan, struct, array, interface 都是引用类型

内建的new函数可以用来创建变量. 表达式new(T)将创建一个T类型的匿名变量，初始化为T类型的零值，然后返回变量地址，返回的指针类型为*T
```go
p := new(int)
```

### 接口

接口类型是对其它类型行为的抽象和概括
接口类型不会和特定的实现细节绑定在一起, 是一种抽象的类型
它不会暴露出它所代表的对象的内部值的结构和这个对象支持的基础操作的集合；它们只会展示出它们自己的方法

```go
// 接口是一组待实现方法的集合, 一个实现了这些方法的具体类型是这个接口类型的实例
type Writer interface {
    Write(p []byte) (n int, err error)
}
type Reader interface {
    Read(p []byte) (n int, err error)
}
// 接口可以组合
type ReadWriter interface {
    Reader
    Writer
}

// Go中不需要显式的声明实现了某个接口，只要实现了其中的所有方法，就默认为实现了该接口
type myFile struct {
}

func (f *myFile) Read (p []byte) (n int, err error) {
}
```


空接口interface{} 其实和泛型的概念很像。任何类型都实现了空接口, 我们可以将任意一个值赋给空接口类型
```go
interface{}
```

一个类型如果拥有一个接口需要的所有方法，那么这个类型就实现了这个接口, *os.File类型实现了io.Reader，Writer，Closer，和ReadWriter接口


概念上讲一个接口的值，接口值，由两个部分组成，一个具体的类型和那个类型的值。它们被称为接口的动态类型和动态值
对于一个接口的零值就是它的类型和值的部分都是nil
调用一个空接口值上的任意方法都会产生panic

#### 类型断言
类型断言是一个使用在接口值上的操作, 一个类型断言检查它操作对象的动态类型是否和断言的类型匹配

```go
x.(T)

rw = w.(io.ReadWriter) // 类型断言失败会引发 panic

// 如果类型断言出现在一个预期有两个结果的赋值操作中, 失败的时候发生panic但是代替地返回一个额外的第二个结果，这个结果是一个标识成功的布尔值
f, ok := w.(*os.File)

//
if f, ok := w.(*os.File); ok {
    // ...use f...
}
```


#### 类型开关

```go
// 根据类型的不同进行不同的处理
func sqlQuote(x interface{}) string {
    if x == nil {
        return "NULL"
    } else if _, ok := x.(int); ok {
        return fmt.Sprintf("%d", x)
    } else if _, ok := x.(uint); ok {
        return fmt.Sprintf("%d", x)
    } else if b, ok := x.(bool); ok {
        if b {
            return "TRUE"
        }
        return "FALSE"
    } else if s, ok := x.(string); ok {
        return sqlQuoteString(s) // (not shown)
    } else {
        panic(fmt.Sprintf("unexpected type %T: %v", x, x))
    }
}
// switch 简化版
switch x.(type) {
    case nil:       // ...
    case int, uint: // ...
    case bool:      // ...
    case string:    // ...
    default:        // ...
}

// 类型开关语句有一个扩展的形式，它可以将提取的值绑定到一个在每个case范围内的新变量
// 一个类型开关隐式的创建了一个语言块，因此新变量x的定义不会和外面块中的x变量冲突。每一个case也会隐式的创建一个单独的语言块。
switch x := x.(type) {
    case nil:
        return "NULL"
    case int, uint:
        return fmt.Sprintf("%d", x) // x has type interface{} here.
    case bool:
        if x {
            return "TRUE"
        }
        return "FALSE"
    case string:
        return sqlQuoteString(x) // (not shown)
    default:
        panic(fmt.Sprintf("unexpected type %T: %v", x, x))
}
```

在Go语言中只有当两个或更多的类型实现一个接口时才使用接口，它们必定会从任意特定的实现细节中抽象出来

