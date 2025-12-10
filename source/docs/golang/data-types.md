---
title: Go 数据类型与流程控制
date: 2025-12-10
layout: doc
---

## 基本类型

```go
var i int = 100        // 整数
var f float64 = 3.14   // 浮点数
var b bool = true      // 布尔
var s string = "hello" // 字符串
```

## 切片与 Map

```go
// 切片
nums := []int{1, 2, 3}
nums = append(nums, 4)

// Map
m := map[string]int{"a": 1, "b": 2}
m["c"] = 3
delete(m, "a")
```

## 条件与循环

```go
// if
if score >= 60 {
    fmt.Println("及格")
}

// switch
switch day {
case 1: fmt.Println("周一")
case 2: fmt.Println("周二")
default: fmt.Println("其他")
}

// for
for i := 0; i < 5; i++ {
    fmt.Println(i)
}

// range
for k, v := range m {
    fmt.Println(k, v)
}
```

