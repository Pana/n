title: Go 处理 JSON
date: 2015-10-21 23:08:57
tags: go
banner: /assets/json-to-go.png
categories:
- go
- json

---

JSON 作为一种轻量, 简单的数据格式, 已经越来越多的用在 Web 服务开发中. GO 作为一种强类型语言, 其处理 json 的时候不像 js 那么简单, 亲切, 但 go 提供了标准包"encoding/json"专门用于解析和编码json.

<!-- more -->

### 解析 json
在 go 中, 我们首先想到可以承载 json 的数据类型即为 struct, 标准json包中提供了如下方法用于解析json
```go
func Unmarshal(data []byte, v interface{}) error
```

假设我们需要解析如下 json 字符串

```
str := `{"servers":[{"serverName":"Shanghai_VPN","serverIP":"127.0.0.1"},{"serverName":"Beijing_VPN","serverIP":"127.0.0.2"}]}`
```

首先需要按照其结构定义好相应的struct

```go
type Server struct {
    ServerName string
    ServerIP   string
}

type Serverslice struct {
    Servers []Server
}
```

然后使用 Unmarshal 方法进行解析 

```go
var s Serverslice
json.Unmarshal([]byte(str), &s)
```

该方法要求用户已经知晓 json 的数据格式, 并事先定义, 这样太过繁琐, 而且通常是不知晓要解析的格式的, 这时 interface 就派上用场了, 因为interface{}可以用来存储任意数据类型的对象. JSON包中采用map[string]interface{}和[]interface{}结构来存储任意的JSON对象和数组。Go类型和JSON类型的对应关系如下:

* bool 代表 JSON booleans
* float64 代表 JSON numbers
* string 代表 JSON strings
* nil 代表 JSON null

现在我们假设有如下的JSON数据

```go
b := []byte(`{"Name":"Wednesday","Age":6,"Parents":["Gomez","Morticia"]}`)
```

如果在我们不知道他的结构的情况下，我们把他解析到interface{}里面

```go
var f interface{}
err := json.Unmarshal(b, &f)
```

这个时候f里面存储了一个map类型，他们的key是string，值存储在空的interface{}里

```go
f = map[string]interface{}{
    "Name": "Wednesday",
    "Age":  6,
    "Parents": []interface{}{
        "Gomez",
        "Morticia",
    },
}
```

那么如何来访问这些数据呢？通过断言的方式：

```go
m := f.(map[string]interface{})
```

通过断言之后，你就可以通过如下方式来访问里面的数据了

```go
for k, v := range m {
    switch vv := v.(type) {
    case string:
        fmt.Println(k, "is string", vv)
    case int:
        fmt.Println(k, "is int", vv)
    case float64:
        fmt.Println(k,"is float64",vv)
    case []interface{}:
        fmt.Println(k, "is an array:")
        for i, u := range vv {
            fmt.Println(i, u)
        }
    default:
        fmt.Println(k, "is of a type I don't know how to handle")
    }
}
```
通过上面的示例可以看到，通过interface{}与type assert的配合，我们就可以解析未知结构的JSON数了

上面这个是官方提供的解决方案，其实很多时候我们通过类型断言，操作起来不是很方便，目前bitly公司开源了一个叫做simplejson的包,在处理未知结构体的JSON时相当方便，详细例子如下所示：

```go
js, err := NewJson([]byte(`{
    "test": {
        "array": [1, "2", 3],
        "int": 10,
        "float": 5.150,
        "bignum": 9223372036854775807,
        "string": "simplejson",
        "bool": true
    }
}`))

arr, _ := js.Get("test").Get("array").Array()
i, _ := js.Get("test").Get("int").Int()
ms := js.Get("test").Get("string").MustString()
```
可以看到，使用这个库操作JSON比起官方包来说，简单的多,详细的请参考如下地址：https://github.com/bitly/go-simplejson


## 编码json

如果想从 go 的数据结构生成 json 字符串, 就需要使用表中包中的 Marshal 函数了

```go
func Marshal(v interface{}) ([]byte, error)
```

例如: 

```go
var s Serverslice
s.Servers = append(s.Servers, Server{ServerName: "Shanghai_VPN", ServerIP: "127.0.0.1"})
s.Servers = append(s.Servers, Server{ServerName: "Beijing_VPN", ServerIP: "127.0.0.2"})
b, err := json.Marshal(s)
```

这里 b 就是一个json 字符串了

```go
{"Servers":[{"ServerName":"Shanghai_VPN","ServerIP":"127.0.0.1"},{"ServerName":"Beijing_VPN","ServerIP":"127.0.0.2"}]}
```

这里所有的字段都是头字母大写, 这是由 go 的大写字母输出特性决定的, 如果想要输出小写字母开头的 json, 需要使用 struct tag定义来实现.

```go
type Server struct {
    ServerName string `json:"serverName"`
    ServerIP   string `json:"serverIP"`
}

type Serverslice struct {
    Servers []Server `json:"servers"`
}
```

Marshal函数只有在转换成功的时候才会返回数据，在转换的过程中我们需要注意几点：

* JSON对象只支持string作为key，所以要编码一个map，那么必须是map[string]T这种类型(T是Go语言中任意的类型)
* Channel, complex和function是不能被编码成JSON的
* 嵌套的数据是不能编码的，不然会让JSON编码进入死循环
* 指针在编码的时候会输出指针指向的内容，而空指针会输出null

## 总结

* go 标准库提供了 Marshal, Unmarshal用户编码和解析
* json 数据可以解析到 struct, 或 interface{}
* struct里各个字段的大小写问题，encoding/json默认只认首字母大写的字段
* 在 go 中, 编码解析操作中字段名的映射有其特殊的规则, 这是由go的首字母大写导出影响的

## 参考

* [build web application with go json part](https://github.com/astaxie/build-web-application-with-golang/blob/master/zh/07.2.md)
* [标准库json文档](https://godoc.org/encoding/json)
* [json 与 go](http://rgyq.blog.163.com/blog/static/316125382013934153244/)
* [在Go语言中使用JSON](http://blog.csdn.net/tiaotiaoyly/article/details/38942311)