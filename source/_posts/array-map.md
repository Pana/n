title: 复合类型
date: 2017-03-23 07:50:42
tags:
banner:
---
Go 中四种基本的结构体: 数组, slice, map, 结构体. 数组跟struct的内存大小是固定的, slice和map则是可变的. 数组的元素类型是相同的, map 则可以不同
<!-- more -->

### array

* 数组的长度需要在编译阶段确定
* 函数调用使用大数组是低效且无法改变原数组值的, 可以使用数组的指针

```go
var a [3]int
b := len(a)   // 获取长度
var q [3]int = [3]int{1, 2, 3} //设置初始值, 如果不设定数组的每个元素都被初始化为元素类型对应的零值
q := [...]int{1, 2, 3}  // 省略长度, 使用初始值长度 
```

### slice

* slice的语法和数组很像，只是没有固定长度而已
* 一个slice由三个部分构成：指针、长度和容量
* 内置的len和cap函数分别返回slice的长度和容量
* slice的切片操作s[i:j]，其中0 ≤ i≤ j≤ cap(s)，用于创建一个新的slice, (months[:]切片操作则是引用整个数组)
* 字符串的切片操作和[]byte字节类型切片的切片操作是类似的。它们都写作x[m:n]，并且都是返回一个原始字节系列的子序列，底层都是共享之前的底层数组，因此切片操作对应常量时间复杂度。x[m:n]切片操作对于字符串则生成一个新字符串，如果x是[]byte的话则生成一个新的[]byte
* 因为slice值包含指向第一个slice元素的指针，因此向函数传递slice将允许在函数内部修改底层数组的元素
* 和数组不同的是，slice之间不能比较, slice唯一合法的比较操作是和nil比较
* 内置的append函数用于向slice追加元素
* 更新slice变量不仅对调用append函数是必要的，实际上对应任何可能导致长度、容量或底层数组变化的操作都是必要的  runes = append(runes, r)


### map

* map 的 key/value 可以各自都有着相同的类型, 浮点数不建议用作key
* 如果一个查找失败将返回value类型对应的零
* map中的元素并不是一个变量，因此我们不能对map的元素进行取址操作
* Map的迭代顺序是不确定的
* map类型的零值是nil，也就是没有引用任何哈希表
* 向一个nil值的map存入元素将导致一个panic异常
* 和slice一样，map之间也不能进行相等比较
* Map的value类型也可以是一个聚合类型

```go
ages := make(map[string]int)

ages := map[string]int{
    "alice":   31,
    "charlie": 34,
}

map[string]int{}   //空map

delete(ages, "alice")
```

```go
// 借助sort实现有序输出
import "sort"

var names []string
for name := range ages {
    names = append(names, name)
}
sort.Strings(names)
for _, name := range names {
    fmt.Printf("%s\t%d\n", name, ages[name])
}
```

```go
// 测试map中值是否存在
age, ok := ages["bob"]
if !ok { /* "bob" is not a key in this map; age == 0. */ }
// or
if age, ok := ages["bob"]; !ok { /* ... */ }
```


### struct

