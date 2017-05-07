title: Go connect sqlite3
date: 2017-05-07 22:04:38
tags:
banner:
---
Go 操作 Sql 数据库的一些常见操作, 用法

<!-- more -->

Go 标准库提供了用于连接 sql 的 `database/sql` package, 该包提供了关系型数据库操作的标准API, 但没有提供具体的实现. 具体实现需由第三方包来提供.
比如[go-sqlite3](https://github.com/mattn/go-sqlite3) 提供了 sqlite3 的具体底层实现. 这样一个好处是不同的 sql 数据库都有相同的交互方式.


#### 安装 & 引入

```shell
go get github.com/mattn/go-sqlite3
```

```go
import(
    "log"
    "database/sql"
    _ "github.com/mattn/go-sqlite3"    // 该包只在引入时注册底层驱动, 不在实际项目中使用
)
```

#### 连接数据库
```go
db, err := sql.Open("sqlite3", "./foo.db")
if err != nil {
    log.Fatal(err)
}
defer db.close()    // 数据库用完后需要关闭连接, 释放资源

// 可使用 Ping() 方法测试数据库连接是否成功
db.Ping()
```

#### 执行sql
常用的方法: db.Exec, db.Query, db.QueryRow

执行没有返回结果的 sql 语句可使 Exec
```go
// 进行查询
result, err := db.Exec("CREATE TABLE users (userId INTEGER, uname TEXT);")
// 获取最后插入的id
id, err := result.LastInsertId()
// 获取受影响的行数
affect, err := result.RowsAffected()
```

查询数据使用 Query, 如果只查询一行使用 QueryRow
```go
rows, err := db.Query("SELECT * FROM users");

row, err := db.QueryRow("SELECT * FROM users where userId = ?", id)
```

#### 从 rows 获取数据

```go
// 基础数据struct
type User struct {
	UserId int
	Uname  string
}
```

```go
rows, err := db.Query("select * from users")
if err != nil {
    log.Fatal(err)
}
defer rows.Close()    // 1. rows 使用完之后也需要 close, 因为rows 会占用数据库连接, 不释放资源会被过多占用
var users = make([]User, 0)
for rows.Next() {    // 2. 调用 Next 方法从rows中读取一行
    var u User
    err := rows.Scan(&u.UserId, &u.Uname)   // 3. 调用 Scan 方法将数据扫入对应数据
    if err != nil {
		log.Fatal(err)
	}
    users = append(users, u)
}
err = rows.Err()
if err != nil {
	log.Fatal(err)
}
```


#### stmt

如果某 sql 语句需要执行多次可使用 stmt

```go
stmt, err := db.Prepare("INSERT INTO userinfo(username, departname, created) values(?,?,?)")
res, err := stmt.Exec("astaxie", "研发部门", "2012-12-09")
stmt.close()  // 用完后也需要 close
```


#### 事务

```go
tx, err := db.Begin()
if err != nil {
    log.Fatal(err)
}
stmt, err := tx.Prepare("insert into foo(id, name) values(?, ?)")
if err != nil {
    log.Fatal(err)
}
defer stmt.Close()
for i := 0; i < 100; i++ {
    _, err = stmt.Exec(i, fmt.Sprintf("こんにちわ世界%03d", i))
    if err != nil {
        log.Fatal(err)
    }
}
tx.Commit()   // 如果失败如何处理?
```

#### ORM 

这里只提供一个比较成熟的 ORM 库, 不做详细使用说明: [gorm](http://jinzhu.me/gorm/)

### 总结

1. 数据库多种操作需要close: db, rows, stmt
2. db 不要频繁 open close, 应该是open 一次, 多次查询, 直到结束
3. 有的 close 操作也会返回error(这太蛋疼了)


#### 参考文档

* [官方 database/sql 文档](https://godoc.org/database/sql)
* [sql 示例 Go database/sql tutorial](http://go-database-sql.org/)
* [go-sqlite3项目中的示例代码(位于example文件夹中)](https://github.com/mattn/go-sqlite3)
* http://jmoiron.net/blog/gos-database-sql/
* [postgreSql Driver](https://godoc.org/github.com/lib/pq)
* [mysql Driver](https://github.com/go-sql-driver/mysql)