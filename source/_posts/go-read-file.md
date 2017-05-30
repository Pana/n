title: Go 读写文件
date: 2017-05-29 22:42:06
tags:
banner:
---
读写文件是最常见的操作之一, Go 标准库对该操作有了很好的支持: os, path. bufio, ioutil.
这里让我们看下go中的常见读写操作

<!-- more -->

### 最基本的读取操作

```go

import (
    "bufio"
    "fmt"
    "io"
    "io/ioutil"
    "os"
    "path/filepath"
)
func check(e error) {
    if e != nil {
        panic(e)
    }
}

// 读取文件的所有内容
dat, err := ioutil.ReadFile("/tmp/dat")  // 返回结果为 []byte
check(err)
fmt.Print(string(dat))

// 打开一个文件
f, err := os.Open("/tmp/dat")
check(err)

// 读取文件的所有内容
ioutil.ReadAll(f)

// 读取指定字节的数据
b1 := make([]byte, 5)
n1, err := f.Read(b1)
check(err)

// 从某个指定位置开始读取
o2, err := f.Seek(6, 0)
check(err)
b2 := make([]byte, 2)
n2, err := f.Read(b2)
check(err)

// 从file 生成一个reader
r4 := bufio.NewReader(f)
b4, err := r4.Peek(5)
check(err)

// 文件使用完成后需要Close
f.Close()

// 按行读取
func ReadLine(filePth string, hookfn func([]byte)) error {
    f, err := os.Open(filePth)
    if err != nil {
        return err
    }
    defer f.Close()
    bfRd := bufio.NewReader(f)
    for {
        line, err := bfRd.ReadBytes('\n')
        hookfn(line) //放在错误处理前面，即使发生错误，也会处理已经读取到的数据。
        if err != nil { //遇到任何错误立即返回，并忽略 EOF 错误信息
            if err == io.EOF {
                return nil
            }
            return err
        }
    }
    return nil
}

// 分块读取
func ReadBlock(filePth string, bufSize int, hookfn func([]byte)) error {
    f, err := os.Open(filePth)
    if err != nil {
        return err
    }
    defer f.Close()
    buf := make([]byte, bufSize) //一次读取多少个字节
    bfRd := bufio.NewReader(f)
    for {
        n, err := bfRd.Read(buf)
        hookfn(buf[:n]) // n 是成功读取字节数
        if err != nil { //遇到任何错误立即返回，并忽略 EOF 错误信息
            if err == io.EOF {
                return nil
            }
            return err
        }
    }
    return nil
}
```


### 写入文件

```go
d1 := []byte("hello\ngo\n")
err := ioutil.WriteFile("/tmp/dat1", d1, 0644)
check(err)

// 创建文件
f, err := os.Create("/tmp/dat2")
check(err)
defer f.Close()

// 写入 byte
d2 := []byte{115, 111, 109, 101, 10}
n2, err := f.Write(d2)
check(err)

// 写入string
n3, err := f.WriteString("writes\n")

// 创建writer
w := bufio.NewWriter(f)
n4, err := w.WriteString("buffered\n")
w.Flush()
```

```go
// 判断文件是否存在
func checkFileIsExist(filename string) (bool) {
    var exist = true;
    if _, err := os.Stat(filename); os.IsNotExist(err) {
        exist = false;
    }
    return exist;
}
```


### 遍历文件夹

```go
func getFilelist(path string) {
    err := filepath.Walk(path, func(path string, f os.FileInfo, err error) error {
        if ( f == nil ) {return err}
        if f.IsDir() {return nil}
        println(path)
        return nil
    })
    if err != nil {
        fmt.Printf("filepath.Walk() returned %v\n", err)
    }
}
```



### 常用方法

```go
// 建立函数
func Create(name string) (file *File, err Error)
func NewFile(fd int, name string) *File

// 打开函数
func Open(name string) (file *File, err Error)   // 以只读方式打开一个存在的文件，打开就可以读取了。  
func OpenFile(name string, flag int, perm uint32) (file *File, err Error)

// 写文件函数
func (file *File) Write(b []byte) (n int, err Error)
func (file *File) WriteAt(b []byte, off int64) (n int, err Error)
func (file *File) WriteString(s string) (ret int, err Error)

// 读文件
func (file *File) Read(b []byte) (n int, err Error)
func (file *File) ReadAt(b []byte, off int64) (n int, err Error)

// 读取文件夹, 文件
func (f *File) Readdir(n int) (fi []FileInfo, err error) //  读取文件目录返回n个fileinfo
func (f *File) Readdirnames(n int) (names []string, err error) // 读取文件目录返回n个文件名

// 删除文件
func Remove(name string) Error
```


```go
func Pipe() (r *File, w *File, err error) // 管道
func (f *File) Chdir() error           //   改变当前的工作目录
func (f *File) Chmod(mode FileMode) error //  改变权限
func (f *File) Chown(uid, gid int) error  //  改变所有者
func (f *File) Close() error              //   关闭文件
func (f *File) Fd() uintptr          //  返回文件句柄
func (f *File) Name() string        //   返回文件名

func (f *File) Seek(offset int64, whence int) (ret int64, err error) // 设置读写文件的偏移量，whence为0表示相对于文件的开始处，1表示相对于当前的位置，2表示相对于文件结尾。他返回偏移量。如果有错误返回错误
func (f *File) Stat() (fi FileInfo, err error)  //返回当前文件fileinfo结构体
func (f *File) Sync() (err error) //  把当前内容持久化，一般就是马上写入到磁盘
func (f *File) Truncate(size int64) error //  改变当前文件的大小，他不改变当前文件读写的偏移量
```



### 参考

* [go语言 文件读写](http://blog.csdn.net/lichao_ustc/article/details/32133169)
* [go语言文件操作](http://www.cnblogs.com/sevenyuan/archive/2013/02/28/2937275.html)