title: Go 命令行获取输入
date: 2017-06-16 15:56:03
tags:
banner:
---
程序开发获取输入是非常常见的操作, 常见的输入方式有环境变量, 命令行参数, 标准输入等. 这里为大家展示在Go中如何实现.
<!-- more -->

### 命令行参数

```go
// 命令行参数从 os.Args 中获取, 是一个 []string, 第一个参数为命令本身
import "os"
fmt.Println(strings.Join(os.Args[1:], " "))
```

### 环境变量

```go
// 设置环境变量
os.Setenv("FOO", "1")
// 获取环境变量
os.Getenv("FOO")
// 获取所有环境变量, os.Environ() 会返回一个 KEY=value 形式的字符串切片
for _, e := range os.Environ() {
    pair := strings.Split(e, "=")
    fmt.Println(pair[0])
}
// 清空env
os.Clearenv()
// 使用环境变量的值替换字符串中的 ${var}
os.ExpandEnv(s string) string
```

### 命令行标志

Go 可以解析 -word=opt 形式的数据, 在go里面称为 flag
```go
import "flag"
// 支持 string, int, bool 三种类型, 可以提供默认值和描述
wordPtr := flag.String("word", "foo", "a string")
numbPtr := flag.Int("numb", 42, "an int")
boolPtr := flag.Bool("fork", false, "a bool")
// 可以用程序中已有的变量声明标志
var svar string
flag.StringVar(&svar, "svar", "bar", "a string var")
// 解析标志
flag.Parse()
// 使用标志, 注意标志是指针
fmt.Println("word:", *wordPtr)
// 获取剩余位置参数
fmt.Println("tail:", flag.Args())
// 使用 -h 或者 --help 标志来得到自动生成的这个命令行程序的帮助文本。
```

[参考](https://gobyexample.com/command-line-flags)

### 标准输入

```go
// 这个方法会执行一次命令行读取操作
reader := bufio.NewReader(os.Stdin)
fmt.Print("Enter text: ")
text, _ := reader.ReadString('\n')
fmt.Println(text)
reader.ReadRune()  // 读取单个utf8 字符
reader.ReadLine()
reader.ReadByte()
reader.ReadBytes()

// 可以使用scanner, 通过for 循环读取多次
scanner := bufio.NewScanner(os.Stdin)
for scanner.Scan() {
    fmt.Println(scanner.Text())
}

// 使用ioutil 的方法
ioutil.ReadAll(os.Stdin)
```

* https://stackoverflow.com/questions/20895552/how-to-read-input-from-console-line
