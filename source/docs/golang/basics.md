---
title: Go 语言基础入门
date: 2025-12-10
layout: doc
---

## 安装与运行

```bash
# macOS
brew install go

# 验证
go version
```

## 第一个程序

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
```

```bash
go run hello.go
```

## 变量声明

```go
// var 声明
var name string = "Go"
var age = 25

// 短变量（推荐）
city := "北京"

// 常量
const Pi = 3.14159
```

## 基本输出

```go
fmt.Println("Hello")           // 换行输出
fmt.Printf("年龄: %d\n", age)  // 格式化输出
```

常用占位符：`%d` 整数、`%s` 字符串、`%f` 浮点数、`%v` 任意类型

