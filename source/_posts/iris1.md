title: Iris -- 路由, 静态文件
date: 2017-05-26 21:30:04
tags:
banner:
---
[Iris](https://github.com/kataras/iris) 是一个高效, 设计良好, 跨平台, 功能强大的go web 框架.
如果你是 Node.js 开发者的话, 可以把它理解成 Golang 的 Express.

<!-- more -->

### Install
Iris 需要 Golang 1.8

```shell
$ go get gopkg.in/kataras/iris.v6
```

### Hello world

```go
package main

import (
	"gopkg.in/kataras/iris.v6"
	"gopkg.in/kataras/iris.v6/adaptors/httprouter"
)

func main() {
    // 声明一个 app
	app := iris.New()
    // 加载中间件
	app.Adapt(httprouter.New())
    // 添加响应方法
	app.HandleFunc("GET", "/", func(ctx *iris.Context) {
		ctx.Writef("hello world\n")
	})
    // 启动服务
	app.Listen(":8080")
}
```

如果想要热重启, 方便开发
```shell
$ go get -u github.com/kataras/rizla
$ rizla main.go
```

### 路由
Web 开发最基本的就是路由处理, Iris 支持两个最流行的路由库 httprouter, gorillamux
在 Iris 中 handler 和 handlerFunc 定义有点不同:

```go
type Handler interface {
    Serve(ctx *Context) // iris-specific
}

type HandlerFunc func(*Context)
```

可以使用 app.Handle 或 HandleFunc 挂载路由
```go
app := iris.New()

app.Handle("GET", "/about", aboutHandler)

type aboutHandler struct {}
func (a aboutHandler) Serve(ctx *iris.Context){
  ctx.HTML("Hello from /about, executed from an iris.Handler")
}

app.HandleFunc("GET", "/contact", func(ctx *iris.Context){
  ctx.HTML(iris.StatusOK, "Hello from /contact, executed from an iris.HandlerFunc")
})
```

除此之外还提供了快捷方法 
```go
app := iris.New()
app.Get("/", handler)
app.Post("/", handler)
app.Put("/", handler)
app.Delete("/", handler)
app.Options("/", handler)
app.Trace("/", handler)
app.Connect("/", handler)
app.Head("/", handler)
app.Patch("/", handler)
// register the route for all HTTP Methods
app.Any("/", handler)
func handler(ctx *iris.Context){
  ctx.Writef("Hello from method: %s and path: %s", ctx.Method(), ctx.Path())
}
```

httprouter
```go
import("gopkg.in/kataras/iris.v6/adaptors/httprouter")
app.Adapt(httprouter.New())
app.Get("/users/:name", func(ctx *iris.Context) {
    name := ctx.Param("name")
    ctx.Writef("%s is %d years old!", name)
})
```

gorillamux
```go
import("gopkg.in/kataras/iris.v6/adaptors/gorillamux")

app.Adapt(gorillamux.New())

app.Get("/users/{name}", func(ctx *iris.Context) {
    name := ctx.Param("name")
    ctx.Writef("%s is %d years old!", name)
})
```

httprouter, gorillamux 的挂载方法是一样的, 唯一不同的是路径参数的定义方式

路由分组可以有共同的路径前缀, 中间件, 模板. 分组还可以嵌套
```go
users:= app.Party("/users", myAuthHandler)
// http://myhost.com/users/42/profile
users.Get("/:userid/profile", userProfileHandler) // httprouter path parameters
// http://myhost.com/users/messages/1
users.Get("/inbox/:messageid", userMessageHandler)
app.Listen("myhost.com:80")
```

### 响应静态文件

```go
// 将某个文件夹的文件作为静态文件返回
app.StaticWeb("/assets", "./public")
// 网站图标
app.Favicon("./public/img/favicon.ico")

// 除此之外还有 StaticContent, StaticHandler, StaticEmbedded 等方法, 具体参看文档
```