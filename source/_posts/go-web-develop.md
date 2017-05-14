title: Go web develop
date: 2017-05-10 08:34:03
tags:
banner:
---
Go 同 Node 一样在标准库中包含了 net/http 可以直接进行 web 开发. 这里简单介绍下在 Go 中如何进行原生 web 开发
<!-- more -->


### Hello world

```go
package main

import (
    "net/http"
    "fmt"
    "log"
)

func main () {
    http.HandleFunc("/", handler) // each request calls handler
    log.Fatal(http.ListenAndServe("localhost:8000", nil))
}

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello World")
}
```


#### 内置接口, 方法

```go
// 该方法会启动一个服务监听address, 所有接受到的请求是用 Handler h 处理
func ListenAndServe(address string, h Handler) error

// Handler 是一个接口
type Handler interface {
    ServeHTTP(w ResponseWriter, r *Request)
}
```
使用这两个最基本的方法可以构建一个 web 服务, 在handler 中根据不同的路径响应不同的内容.
但实际的项目开发中这样是不合理的 net/http 包中提供了 ServeMux 用来简化 url 和 handler 之间的关系

```go
func main() {
    db := database{"shoes": 50, "socks": 5}
    mux := http.NewServeMux()
    mux.Handle("/list", http.HandlerFunc(db.list))
    mux.Handle("/price", http.HandlerFunc(db.price))
    log.Fatal(http.ListenAndServe("localhost:8000", mux))
}
```

语句http.HandlerFunc(db.list)是一个转换而非一个函数调用，因为http.HandlerFunc是一个类型
```go
type HandlerFunc func(w ResponseWriter, r *Request)

func (f HandlerFunc) ServeHTTP(w ResponseWriter, r *Request) {
    f(w, r)
}
```

ServerMux 有一个简化方法, 以下两个语句是等价的
```go
mux.Handle("/list", http.HandlerFunc(db.list))
// 和
mux.HandleFunc("/list", db.list)
```

http 包中提供了全局的 ServeMux 实例 DefaultServeMux, 以及包级别的 
http.Handle, http.HandleFunc
最终可以简化为

```go
func main() {
    db := database{"shoes": 50, "socks": 5}
    http.HandleFunc("/list", db.list)
    http.HandleFunc("/price", db.price)
    log.Fatal(http.ListenAndServe("localhost:8000", nil))
}
```


#### http 包
http 有两个核心功能: Conn, ServeMux

 Go为了实现高并发和高性能, 使用了goroutines来处理Conn的读写事件, 这样每个请求都能保持独立，相互不会阻塞，可以高效的响应网络事件

 ServeMux 定义

 ```go
 type ServeMux struct {
    mu sync.RWMutex   //锁，由于请求涉及到并发处理，因此这里需要一个锁机制
    m  map[string]muxEntry  // 路由规则，一个string对应一个mux实体，这里的string就是注册的路由表达式
    hosts bool // 是否在任意的规则中带有host信息
}

type muxEntry struct {
    explicit bool   // 是否精确匹配
    h        Handler // 这个路由表达式对应哪个handler
    pattern  string  //匹配字符串
}
 ```

 ```go
 // 路由器分发规则
 func (mux *ServeMux) ServeHTTP(w ResponseWriter, r *Request) {
    if r.RequestURI == "*" {
        w.Header().Set("Connection", "close")
        w.WriteHeader(StatusBadRequest)
        return
    }
    h, _ := mux.Handler(r)
    h.ServeHTTP(w, r)
}

func (mux *ServeMux) Handler(r *Request) (h Handler, pattern string) {
    if r.Method != "CONNECT" {
        if p := cleanPath(r.URL.Path); p != r.URL.Path {
            _, pattern = mux.handler(r.Host, p)
            return RedirectHandler(p, StatusMovedPermanently), pattern
        }
    }
    return mux.handler(r.Host, r.URL.Path)
}

func (mux *ServeMux) handler(host, path string) (h Handler, pattern string) {
    mux.mu.RLock()
    defer mux.mu.RUnlock()

    // Host-specific pattern takes precedence over generic ones
    if mux.hosts {
        h, pattern = mux.match(host + path)
    }
    if h == nil {
        h, pattern = mux.match(path)
    }
    if h == nil {
        h, pattern = NotFoundHandler(), ""
    }
    return
}
 ```

 Go其实支持外部实现的路由器 ListenAndServe的第二个参数就是用以配置外部路由器的，它是一个Handler接口，即外部路由器只要实现了Handler接口就可以







#### 参考
* [Go 语言圣经接口部分](http://docs.ruanjiadeng.com/gopl-zh/ch7/ch7-07.html)
* [build-web-application-with-golang](https://github.com/astaxie/build-web-application-with-golang)
* [Go web foundation](https://github.com/Unknwon/go-web-foundation)