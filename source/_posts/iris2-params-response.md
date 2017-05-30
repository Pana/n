title: Iris--请求参数&响应体
date: 2017-05-30 16:16:06
tags:
banner:
---
Web 开发最常见的操作是读取请求参数, 返回响应体, Iris 提供了强大的功能支持这些操作

<!-- more -->

### 读取参数

```go
// 路由参数
ctx.Param("参数名")
ctx.ParamInt()
// 请求方法
ctx.Method()
// 请求路径
ctx.Path()
// 获取query参数
ctx.URLParam() // 获取 get 的请求参数
ctx.URLParamInt() // 返回 int
ctx.URLParams()  // 返回一个map
ctx.URLParamsAsMulti()

// 获取post参数
ctx.FormValue()
ctx.FormValues()  // 返回map

ctx.PostValue()
ctx.ReadForm()
ctx.ReadJson()
ctx.ReadXML()

// 获取路径参数
ctx.Get()
ctx.GetInt()
ctx.GetString()

// 获取header
ctx.RequestHeader()
```

### 响应结果

支持多种响应格式

```go
// 响应json
ctx.JSON()
ctx.JSONP()
// 响应文本
ctx.Writef()
ctx.WriteString()
ctx.Text()
// xml
ctx.XML()
ctx.Data()
ctx.Markdown()
// 跳转
ctx.Redirect()
// html 片段
ctx.HTML()
```

Iris 支持多种模板:

```go
view.HTML()  // 标准模板
view.Django()
view.Pug()
view.Handlebars()
view.Amber()

// 标准模板例子
import "gopkg.in/kataras/iris.v6/adaptors/view"
//
app.Adapt(view.HTML("./templates", ".html"))
// 在 handler 中渲染模板
ctx.Render("hi.html", dataInterface)
```
