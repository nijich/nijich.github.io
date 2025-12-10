---
title: Go 函数与并发
date: 2025-12-10
layout: doc
---

## 函数定义

```go
func add(a, b int) int {
    return a + b
}

// 多返回值
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("除数不能为0")
    }
    return a / b, nil
}
```

## 结构体

```go
type User struct {
    Name string
    Age  int
}

// 方法
func (u User) SayHello() {
    fmt.Printf("我是%s\n", u.Name)
}

user := User{Name: "张三", Age: 25}
user.SayHello()
```

## 并发 Goroutine

```go
// 启动协程
go func() {
    fmt.Println("并发执行")
}()

// Channel 通信
ch := make(chan int)
go func() { ch <- 42 }()
val := <-ch
```

## 常用并发模式

```go
// 等待多个协程
var wg sync.WaitGroup
for i := 0; i < 3; i++ {
    wg.Add(1)
    go func(n int) {
        defer wg.Done()
        fmt.Println(n)
    }(i)
}
wg.Wait()
```

